---
title: ？CTF 2025(QCTF) WP
date: 2026-03-31T15:23:10+08:00
categories: [WP, 2025]
tags: [WP]
---

# ？CTF 2025(Question CTF, or QCTF) WP

```
``````

# osint

## week1

### Task 1. 见面地点

题干：
你的高中同学 Moe 已经有一个多月没联系你了...但今天又突然收到了他的消息。
“不用我多说你一定懂了，坐地铁在这里碰面ww”以及一张他随手拍的照片。
哦——原来是暑假旅行啊。
Moe 这人很喜欢给朋友出些丈二和尚摸不着头脑的谜题。出乎意料的是，这次他给出的线索似乎很直截了当。
你需要找到距目的地直线距离最近的地铁站点，以及经过该站点的线路。
flag以flag{线路号_站点名}的形式给出，站点名为首字母大写的拼音形式。
例如：目标站点为“国际机场“，有1号线与空港S3号线经过，则答案为flag{1_S3_GuoJiJiChang}。
很简单，一张图片![](assets/img/media/pictures/2026_4_2/location.jpg)里面有位置信息，轻轻松松找到flag：`flag{1_4_HuiZhanZhongXin}`

## week2

### Task 2. 方块世界？！

题干：“欸，你知道打字机吗？我最近在玩一款很有意思的游戏~”
打字机？游戏？这个概念似乎很新颖...你如是地说不懂。
“简单来说，就是...”Moe 打开了一个不知名的小网站和一个小工具，兴致勃勃地讲着什么。
你拼尽全力，但依然很难跟上他的语速与思路，唯一能做的是趁 Moe 不注意，对着屏幕随手拍一张照片。
“有这张照片，问题就解决了。”![](assets/img/media/pictures/2026_4_2/screenshot.jpg)
过程：图片很明显有个网址，和游戏（文字游戏，不知道也没关系）。我们搜一下codeberge，发现是类似github的网站，找一下这个what_is_the_flag用户，就能找到它的[仓库](https://codeberg.org/what_is_the_flag/what-ctf-world)
里面有个介绍：
嘛，我们都知道世界是一种抽象的对象，一般是所有元素继承的父类；而世界在不同的维度可以呈现为迥然不同的形式：在官方比赛平台呈现为一个有题目、队伍、排行榜等等的项目，在这里则呈现为一个 2D 游戏中的关卡，而这是作者的安排...
它的载体被称为**[文字游戏世界](https://store.steampowered.com/app/3114370)**，一个基于文字构成图形的开放性游戏。在 ?CTF 世界中，游戏的主角“你”身为玩家，踏上一段简短而又令人回味无穷的寻找 Flag 的旅程。
所以，我们去steam上下个游戏（~~光明正大的摸鱼~~），然后图片里很明显是关卡编辑界面，我们过了新手教程后进关卡编辑器弄一下，
![](assets/img/media/pictures/2026_4_2/Pasted%20image%2020251008144435.png)
找到第三关
![](assets/img/media/pictures/2026_4_2/屏幕截图%202025-10-04%20214752.png)
很明显一个base64字符串
![](assets/img/media/pictures/2026_4_2/屏幕截图%202025-10-04%20211908.png)
但是解码后发现有问题，提示我们去玩一下游戏，按照提示弄一下就能看到 ~~（别告诉我你没过新手教程而不会操作）~~ 
![](assets/img/media/pictures/2026_4_2/屏幕截图%202025-10-04%20213003.png)
实际的密文应该是这样的`LzkuL4gSqI6hrH0JYTUkLR9jLy0PnQNlKIY1Y4VmJ2DlqFZso4VsDzQkpmYusC==`
替换的字符在上面
![](assets/img/media/pictures/2026_4_2/Pasted%20image%2020251008144515.png)
（关卡编辑器多看几眼就能复制粘贴了，不用手打，虽然手打也很快，小心打错字）
直接用标准映射表解不出来，再看看其他关，发现第一个关卡下面有个自定义映射表`M-ZA-Ln-za-m1-90+/=`
解密后得到flag：`flag{Funny_W0r1d_0f_Ch@rac43rs_Tru3_or_Fa1s3!}`

# forensics

## week1

### 取证第一次

题干：
Arch Linux 又安装失败惹qwq
出错的原因似乎很奇怪，不如在日志里找找，说不定有什么潜藏的秘密...
OneDrive（推荐使用）：
[https://1drv.ms/u/c/81c139afae09cca2/EQ5S3OwWZTdOkIslNXz1n50BR-Ce-IVCFmDjhqH59fmzCg?e=H8Z4M1](https://1drv.ms/u/c/81c139afae09cca2/EQ5S3OwWZTdOkIslNXz1n50BR-Ce-IVCFmDjhqH59fmzCg?e=H8Z4M1)
百度网盘(365d)：
[https://pan.baidu.com/s/1xK6HQvJtXxDbP_KUjhSVIA?pwd=ux6x](https://pan.baidu.com/s/1xK6HQvJtXxDbP_KUjhSVIA?pwd=ux6x)
提取码: ux6x
压缩文件密码：82dc915fa5133d52270e3b93a4da40b1
过程：我们下下来，拖入010 editor打开，Ctrl + F搜索`flag{`，秒出flag：`flag{F0r3ns1cs_i$_Fun_Rea11y_???}`

# Misc

## week1

### 《关于我穿越到CTF的异世界这档事:序》

题干：
metavi正在深夜刷题，屏幕突然弹出计算器窗口，紧接着浮现一句话：
【"即使引导早已破碎，也请您当上MISC之王。"】
一阵强光过后，他已然站在一片不可思议的大陆上，NPC告诉他，这里是“异世界·Misc大陆”，而他是被召唤来的挑战者。他被选中代表“Misc分域”挑战其他技术领域，否则 Misc 将被系统删除。挑战形式则是一道道Misc题的具象化场景。为了Misc也为了自己，metavi不得不接受挑战。
【第一层·base：请解出隐藏在base里的flag。】
附件：Do_you_know_Base.zip
里面的内容：
base8.txt

```
Tsmssic?FT?ii?sFFi?iTimCTC?mcCmsTiTmmCCCFs?sCCiiTFTcmCmFTCscFicTTs?ciC?TFFTim?s?TTmsmCmFCmmiFCmsTFTimCCsFCmiTicTT?msFCTTTs?c??ssFCmi?mciCcT=====
```

alphabet.txt

```
VGhlIGtleSBoYXMgbmV2ZXIgYmVlbiBmYXIgYXdheTsgaXTgbGllcyBwZWFjZWZ1bGx5IHdpdGhpbiB0aGUgdGV4dCBpdHNlbGYuDT==
VGhlIGtleSBoYXMgbmV2ZXIgYmVlbiBmYXIgYXdheTsgaXfgbGllcyBwZWFjZWZ1bGx5IHdpdGhpbiB0aGUgdGV4dCBpdHNlbGYuDf==
VGhlIGtleSBoYXMgbmV2ZXIgYmVlbiBmYXIgYXdheTsgaXUgbGllcyBwZWFjZWZ1bGx5IHdpdGhpbiB0aGUgdGV4dCBpdHNlbGYuDU==
VGhlIGtleSBoYXMgbmV2ZXIgYmVlbiBmYXIgYXdheTsgaXTgbGllcyBwZWFjZWZ1bGx5IHdpdGhpbiB0aGUgdGV4dCBpdHNlbGYuDT==
VGhlIGtleSBoYXMgbmV2ZXIgYmVlbiBmYXIgYXdheTsgaXVgbGllcyBwZWFjZWZ1bGx5IHdpdGhpbiB0aGUgdGV4dCBpdHNlbGYuDV==
VGhlIGtleSBoYXMgbmV2ZXIgYmVlbiBmYXIgYXdheTsgaXUgbGllcyBwZWFjZWZ1bGx5IHdpdGhpbiB0aGUgdGV4dCBpdHNlbGYuDU==
VGhlIGtleSBoYXMgbmV2ZXIgYmVlbiBmYXIgYXdheTsgaXUgbGllcyBwZWFjZWZ1bGx5IHdpdGhpbiB0aGUgdGV4dCBpdHNlbGYuDU==
VGhlIGtleSBoYXMgbmV2ZXIgYmVlbiBmYXIgYXdheTsgaXWgbGllcyBwZWFjZWZ1bGx5IHdpdGhpbiB0aGUgdGV4dCBpdHNlbGYuDW==
VGhlIGtleSBoYXMgbmV2ZXIgYmVlbiBmYXIgYXdheTsgaXWgbGllcyBwZWFjZWZ1bGx5IHdpdGhpbiB0aGUgdGV4dCBpdHNlbGYuDW==
VGhlIGtleSBoYXMgbmV2ZXIgYmVlbiBmYXIgYXdheTsgaXdgbGllcyBwZWFjZWZ1bGx5IHdpdGhpbiB0aGUgdGV4dCBpdHNlbGYuDd==
VGhlIGtleSBoYXMgbmV2ZXIgYmVlbiBmYXIgYXdheTsgaXWgbGllcyBwZWFjZWZ1bGx5IHdpdGhpbiB0aGUgdGV4dCBpdHNlbGYuDW==
VGhlIGtleSBoYXMgbmV2ZXIgYmVlbiBmYXIgYXdheTsgaXZgbGllcyBwZWFjZWZ1bGx5IHdpdGhpbiB0aGUgdGV4dCBpdHNlbGYuDZ==
VGhlIGtleSBoYXMgbmV2ZXIgYmVlbiBmYXIgYXdheTsgaXXgbGllcyBwZWFjZWZ1bGx5IHdpdGhpbiB0aGUgdGV4dCBpdHNlbGYuDX==
VGhlIGtleSBoYXMgbmV2ZXIgYmVlbiBmYXIgYXdheTsgaXTgbGllcyBwZWFjZWZ1bGx5IHdpdGhpbiB0aGUgdGV4dCBpdHNlbGYuDT==
VGhlIGtleSBoYXMgbmV2ZXIgYmVlbiBmYXIgYXdheTsgaXWgbGllcyBwZWFjZWZ1bGx5IHdpdGhpbiB0aGUgdGV4dCBpdHNlbGYuDW==
VGhlIGtleSBoYXMgbmV2ZXIgYmVlbiBmYXIgYXdheTsgaXTgbGllcyBwZWFjZWZ1bGx5IHdpdGhpbiB0aGUgdGV4dCBpdHNlbGYuDT==
VGhlIGtleSBoYXMgbmV2ZXIgYmVlbiBmYXIgYXdheTsgaXQgbGllcyBwZWFjZWZ1bGx5IHdpdGhpbiB0aGUgdGV4dCBpdHNlbGYu
```

过程：alphabet里面的东西很明显是base64字符串，解密后看到

```
The key has never been far away; itàlies peacefully within the text itself.
The key has never been far away; iwàlies peacefully within the text itself.
The key has never been far away; iu lies peacefully within the text itself.
The key has never been far away; itàlies peacefully within the text itself.
The key has never been far away; iu`lies peacefully within the text itself.
The key has never been far away; iu lies peacefully within the text itself.
The key has never been far away; iu lies peacefully within the text itself.
The key has never been far away; iu lies peacefully within the text itself.
The key has never been far away; iu lies peacefully within the text itself.
The key has never been far away; iw`lies peacefully within the text itself.
The key has never been far away; iu lies peacefully within the text itself.
The key has never been far away; iv`lies peacefully within the text itself.
The key has never been far away; iuàlies peacefully within the text itself.
The key has never been far away; itàlies peacefully within the text itself.
The key has never been far away; iu lies peacefully within the text itself.
The key has never been far away; itàlies peacefully within the text itself.
The key has never been far away; it lies peacefully within the text itself.
```

提示我们关键信息就在`base8.txt`中。我们去搜base8，

- 其定义是
  1.1、Base8编码算法
  Base8编码是一种基于8个可打印字符来表示二进制数据的方法，Base8编码包含数字0-7等共计8个字符。
  1.2、Base8编码标准
  Base8编码位数和映射字符默认值为跨平台兼容的通用标准：
  **编码位数**：3
  **映射字符**：01234567
  1.3、Base8编码示例
  例如：i like 试试吧，Base8编码后为：
  3222015432265545101642574536425745362620516=====
  我们观察txt里面的字符串，所有重复出现的能拼出来`?CTFmisc`，这个大概率是映射表。
  但是这个网站需要积分使用，积分不好获取，我们让AI帮忙写个加密解密脚本，

```Python
def base8_custom_encode(data: bytes, alphabet: str = '01234567') -> str:  
    """\  
    使用自定义字符集进行Base8编码\  
    :param data: 输入字节数据\  
    :param alphabet: 自定义字符集(必须包含8个唯一字符)\  
    :return: 自定义Base8编码字符串\  
    """    # 验证字符集有效性  
    if len(alphabet) != 8 or len(set(alphabet)) != 8:  
        raise ValueError("字符集必须包含8个唯一字符")  
  
    # 将字节转换为二进制字符串  
    bin_str = ''.join(format(byte, '08b') for byte in data)  
  
    # 计算并添加填充位(使总位数成为3的倍数)  
    padding = (3 - len(bin_str) % 3) % 3  
    bin_str += '0' * padding  
  
    # 每3位分组并转换为自定义字符  
    encoded = []  
    for i in range(0, len(bin_str), 3):  
        chunk = bin_str[i:i + 3]  
        index = int(chunk, 2)  
        encoded.append(alphabet[index])  
  
    return ''.join(encoded)  
  
  
def base8_custom_decode(data: str, alphabet: str = '01234567') -> bytes:  
    # 验证字符集  
    if len(alphabet) != 8 or len(set(alphabet)) != 8:  
        raise ValueError("字符集必须包含8个唯一字符")  
  
    # 关键修复：强制确保输入为字符串  
    if not isinstance(data, str):  
        try:  
            data = ''.join(chr(x) if isinstance(x, int) else str(x) for x in data)  
        except TypeError:  
            raise TypeError("输入数据必须是字符串或整数可迭代对象")  
  
    char_to_index = {char: idx for idx, char in enumerate(alphabet)}  
    invalid_chars = set(data) - set(alphabet)  
  
    # 修复错误信息生成逻辑  
    if invalid_chars:  
        # 显式提取原始字符（非ASCII码值）  
        invalid_str = ''.join([  
            f"[{ord(c)}]" if c not in alphabet else c  # 标注非法字符的ASCII值  
            for c in data if c in invalid_chars  
        ])  
        raise ValueError(f"非法字符（ASCII值）: {invalid_str}")  
  
    # 后续解码逻辑保持不变...  
    bin_str = ''.join(format(char_to_index[char], '03b') for char in data)  
    num_bytes = len(bin_str) // 8  
    bin_str = bin_str[:num_bytes * 8]  
    return bytes(int(bin_str[i:i + 8], 2) for i in range(0, len(bin_str), 8))  
  
  
bdata = b"Tsmssic?FT?ii?sFFi?iTimCTC?mcCmsTiTmmCCCFs?sCCiiTFTcmCmFTCscFicTTs?ciC?TFFTim?s?TTmsmCmFCmmiFCmsTFTimCCsFCmiTicTT?msFCTTTs?c??ssFCmi?mciCcT"  
print(base8_custom_decode(bdata, "?CTFmisc"))
```

解密后得到base64字符串`ZmxhZ3tUaDNfUHIxbmMxcGwzXzBmX0Jhc2VfMXNfUzBfRXp6fQ==`，再解密得到flag：`flag{Th3_Pr1nc1pl3_0f_Base_1s_S0_Ezz}`

### 俱乐部之旅(1) - 邀请函

题干：一天Tomoki在冲浪的时候，突然收到了一封来自“c5im俱乐部”的邮件，里面是一封俱乐部邀请函，但想要打开邀请函必须先解出附件中的flag……
附件：Try_t0_f1nd_My_s3cret.zip
过程：我们下载下来，解压发现需要密码。属性一栏能看到`c5im???`，我们打开ARCHPR进行掩码爆破，![](assets/img/media/pictures/2026_4_2/Pasted%20image%2020250928195015.png)![](assets/img/media/pictures/2026_4_2/Pasted%20image%2020250928195017.png)
得到压缩包密码`c5im8467`，解压后得到里面的`steg.docx`。我们把后缀名修改为`.zip`，解压，在里面找找，文件都看看，
我们在core.xml里找到了

```xml
<cp:coreProperties xmlns:cp="http://schemas.openxmlformats.org/package/2006/metadata/core-properties" xmlns:dc="http://purl.org/dc/elements/1.1/" xmlns:dcterms="http://purl.org/dc/terms/" xmlns:dcmitype="http://purl.org/dc/dcmitype/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">

<dcterms:created xsi:type="dcterms:W3CDTF">2025-08-04T11:47:00Z</dcterms:created>

<dc:creator>2hi5hu</dc:creator>

<dc:description>11001101101100110000111001111111011101011101100001110010110010010111110110101111010001100111100111101111111010011110011101111101100011111010</dc:description>

<cp:keywords>do u know cyberchef?</cp:keywords>

<cp:lastModifiedBy>炙恕qwq</cp:lastModifiedBy>

<dcterms:modified xsi:type="dcterms:W3CDTF">2025-08-13T09:59:15Z</dcterms:modified>

<dc:title>标准ASCII码使用‌7位二进制数‌表示字符</dc:title>

</cp:coreProperties>
```

我们按照提示解密后得到flag的一半`flag{W0rd_5t3g_is_1z`。
另一个在word文件夹里，
![](assets/img/media/pictures/2026_4_2/Pasted%20image%2020250928195345.png)
我们用记事本打开，里面存的16进制字符`2657656c63306d655f74305f7468335f6335696d5f433175627d`，`from hex`一下得到右半部分`&Welc0me_t0_th3_c5im_C1ub}`
拼起来得到
flag：`flag{W0rd_5t3g_is_1z&Welc0me_t0_th3_c5im_C1ub}`

### 布豪有黑客(一)

题干：c3把心爱的flag藏在了服务器上，但粗心的他好像忘记关闭HTTP服务了，于是黑客成功偷走了c3的flag和解压密码…
附件：`布豪有黑客(一).7z`，打开是`布豪有黑客(一).pcapng`
过程：分析流量数据，用**Wire Shark**，打开来看，![](assets/img/media/pictures/2026_4_2/Pasted%20image%2020250928195734.png)
![](assets/img/media/pictures/2026_4_2/Pasted%20image%2020250928195813.png)
一眼看到`flag.zip`和`password.txt`，我们导出保存一下![](assets/img/media/pictures/2026_4_2/Pasted%20image%2020250928195917.png)
就完事了，用密码解压压缩包得到flag：`flag{Wireshark_1s_4wes0m3}`

### 文化木的侦探委托(一)

题干：文化木是一家仅有一人的灵异侦探所的员工，有一天，她收到了一张奇怪的照片……？
附件：`奇怪的图片.zip`，解压得到图片`奇怪的图片.png`
过程：压缩包的属性里有提示`注释: 文化木正冥思苦想打量着眼前的图片，毫无思路时一张照片掉到了她的桌上。一张被折了一部分导致长宽比例怪怪的照片……？她忽然有了想法。这个图片，会不会也是被“折”起来了呢。要是有什么办法可以改变它的长宽，或许就能看见被“折”起来的部分。`
常见的隐藏手法是修改图片的长宽，这里我们打开010修改下宽度![](assets/img/media/pictures/2026_4_2/Pasted%20image%2020250928200253.png)
宽度是第二行的5-8字节存储的数据，我们稍微改大一点![](assets/img/media/pictures/2026_4_2/Pasted%20image%2020250928200410.png)
得到关键信息。我们打开stegsolve，重新弄个没修改过的图片打开（修改过不行）
![](assets/img/media/pictures/2026_4_2/Pasted%20image%2020250928200644.png)
![](assets/img/media/pictures/2026_4_2/Pasted%20image%2020250928200712.png)
这样操作preview一下，保存一下txt，该删的空格删了，就能得到flag：`flag{Please_FindME}`

### 维吉尼亚朋友的来信

题干：你的维吉尼亚朋友向你来信，奇怪的是你却收到了一个音频，你用眼睛仔细去听，发现信件从深处的声音中来…
附件：`维吉尼亚朋友的来信.zip`，打开了是`summer.wav`
过程：题目提示这里用到了维吉尼亚加密。我们来看这个wav的频谱图，能看到![](assets/img/media/pictures/2026_4_2/Pasted%20image%2020250928200942.png)
`KEY{deepsound}`。去搜一下，能找到软件deepsound，下下来把wav拖进去，看到
![](assets/img/media/pictures/2026_4_2/Pasted%20image%2020250928201133.png)
提取一下，得到`XX.txt`，里面写的是

```
Gieg fsq sulirs,
  Osfprpi xd lvy gkumpaaba jruph dx QNS!Wkmw xkb’n wxvx e vsay—vw’v e tasmaerxrh lzslr fxvmdkwnl phixh uvuyohrkt, ovyeh hzigq zcah rj gdvs, yihuc lxvrya foyi, pfr yihuc tjrnfr krphh s gypuhx apahcaj ws ft mbwbyhvis. Zslr, bry’pa khlrwfl cdmf gvqg, pipjb nb vhi tplhyeqv mr rzoif, dqh xjjb "C qrq’x ocgk" cawr "M jxyilrg lx sjl."
  Ria’w zsvgq wz gklrkh xsyy ryivlzsfzlqk ei xwlfw. Zi’zt szf ohhr xwwfy—fwdvmcy on n susfawa, mpudxgwaba bxu lipvg, qbqgivxfu quhui xd khuew. Eyx izon’f wki qpyww bi lx: ikwfs zlvxezw wm n ohwwdf, sprub wqpdz qvq d vyhz. Ohq bry’vt fcn norri. Izwm prpqycahs gkumztk ch propeqgfuglrr, sc kvuelqk mswom, nqg pmulwht hdgl dlvye xs.
  Ws sajy vq. Hbtagfy. Rasivxeshg. Dvo ujwgnvrqw. Gtdsvedwi xww hcab ymgigfcrv, drh sgb’n shdv xww gnhpepih. Lvy PWI asgdr cf eumkwlsl jlwl cdm wh vw, drh lw qua’w zemi lc mrh zligw mihu msygfss gdniw ngi.
Zydj mw "umbhl ohxxtj hi lrx". Vibwavru zvee lvy sodk gdfhyaw lr jasu{} uag xwi jfryeolri‘_' ig fycodgi hhowr fkevpuhye' '.

Ehwx lagbrv!

```

根据题目说的维吉尼亚加密，和密钥`deepsound`，解密后看到

```
Dear new friend,
  Welcome to the thrilling world of CTF!This isn’t just a game—it’s a playground where curiosity meets challenge, where every line of code, every hidden clue, and every puzzle holds a secret waiting to be uncovered. Here, you’ll stretch your mind, learn to see patterns in chaos, and turn "I don’t know" into "I figured it out."
  Don’t worry if things feel overwhelming at first. We’ve all been there—staring at a problem, scratching our heads, wondering where to start. But that’s the magic of it: every mistake is a lesson, every small win a rush. And you’re not alone. This community thrives on collaboration, on sharing ideas, and lifting each other up.
  So dive in. Explore. Experiment. Ask questions. Celebrate the tiny victories, and don’t fear the stumbles. The CTF world is brighter with you in it, and we can’t wait to see where your journey takes you.
Flag is "funny letter to you". Remember wrap the flag content in flag{} and use underline‘_' to replace space character' '.

Best wishes!

```

根据内容提示，得到flag：`flag{funny_letter_to_you}`

## week2

### 破碎的拼图

题干：Maybe是一个三国杀爱好者，最近她在整理电脑文件时遇到了麻烦。她记得自己有一个重要的文档，但是在整理过程中操作有误，现在找不到完整的文件了。
她只记得当时为了安全起见，对文件进行了一些处理，还涉及到了她最喜欢的三国杀图片。现在文件散落在各处，Maybe完全搞不清楚该如何恢复了。
你能帮Maybe找回她的重要文档吗？
过程：压缩包里的txt内容是

```
系统日志片段
===============

[2024-03-15 14:22:31] 文件处理任务开始
[2024-03-15 14:22:45] 执行分卷压缩: flag.doc -> 3个部分
[2024-03-15 14:23:12] 文件重命名操作完成
[2024-03-15 14:23:28] 调用隐写工具: st*gh*d*
[2024-03-15 14:23:35] 密码验证: 使用赛事标识符
[2024-03-15 14:23:41] 载体文件: image.jpg
[2024-03-15 14:23:55] 任务完成，文件已安全存储

---
配置信息:

密码构成: [赛事标识] = ?CTF

---
注意事项:
- 分卷文件顺序可能被打乱
- 隐写内容需要正确密码提取
- 所有文件类型以实际为准

最后访问时间: 2024-03-15 14:24:00
```

很显然，是分卷压缩包，第三个包藏在图片里，前两个已给出，重命名一下就行。用到的工具是steghide，我们下好，输个命令`steghide extract -sd ...`再输个密码`?CTF`
就得到了第三个压缩包
重命名一下，放一块分卷解压，就可以解压出一个doc，里面就是flag：`flag{br0k3n_p1eces_r3a553mb13d_7h3_puzz13}`

### 文化木的侦探委托(二)

题干：文化木在解出了图片里的求助信号后，她的电脑突然开始自己动了起来，并又下载得到了一张图片，同时电脑上出现了两行字。
【表面无异，隐形涟漪里藏着求助的回声。】
【捕捉波纹，对比参差，真相会回应你。】
（注：flag内为可识别的明文，仅首个单词的首字母大写，以flag{}包裹）
过程：

## week3

### 《关于我穿越到CTF的异世界这档事:Q》

题干：穿过第二层的“终端祭坛”，metavi 来到一座漂浮于半空的大陆。大陆中央伫立着一台巨大的游戏机，散发着淡蓝色的光芒。
当他触碰的刹那，光影骤然交错，他被拉入了一款横版 2D 动作游戏的世界——这就是第三层的舞台。
这一次，没有隐晦的提示，也没有复杂的命令。挑战的方式，就是单纯的游玩游戏。
在这片虚拟大陆中，flag 已被打碎，零散地隐藏在游戏的各个角落，只有凭借操作与探索，才能逐一收集并最终拼接完整。
【第三层 · 游戏题：通关游戏，找到深埋其中的各个flag碎片】
过程：玩了一下，获取到前4个后，游戏有点难了，就逆向去了。很显然是godot写的游戏，网上搜一下
反编译得到源文件，获取到flag5，6（1,3,5,6有文本）
1                2              3               4             5             6
`ZmxhZ3tZMHV fQHJlX1 JlQDExeV9 BX0cwMGR fR0FNRV IhISF9`
base64解密一下得到flag：`flag{Y0u_@re_Re@11y_A_G00d_GAMER!!!}`

### 俱乐部之旅(3) - 与时间对话

https://www.ohyewang.com/ZeroWidth.html
https://tool.bfw.wiki/tool/1695021695027599.html
15:00  1760079600

### 布豪有黑客(三)

过程：学习于[参考文献](https://perfsky.github.io/matches/qiangwang2024-misc/)（虽然写的很不详细，前半部分）
打开流量，看6，7，8，SMB2中找到以下数据并提取（最好不要直接Ctrl+C，这样很可能复制不完全，右键复制去）
![](assets/img/media/pictures/2026_4_2/Pasted%20image%2020251015172844.png)
NTLM Server Challenge: 91e15fed933eff0c
NTProofStr: e20402e8a924e2de7c1e3fd3f949bc38
Domain name: C3NGH--DESKTOP
User name: rockyou
Session Key: 55d34fb498993a0aceaa960eea8ab7e9
NTLM Response: e20402e8a924e2de7c1e3fd3f949bc3801010000000000001df551630839dc011473b30c1fc1c20f000000000200160046004c00410047002d005300450052005600450052000100160046004c00410047002d005300450052005600450052000400160046004c00410047002d005300650072007600650072000300160046004c00410047002d00530065007200760065007200070008001df551630839dc0106000400020000000800300030000000000000000100000000200000a6cd8042becda35cc7967ee26857127fac305123020cefe31fcefbfd7ece32d50a001000000000000000000000000000000000000900200063006900660073002f0046004c00410047002d00530045005200560045005200000000000000000000000000
session id（小端序）：19 00 00 0c 00 1c 00 00
ntlmv2的hash组合方式
*username==::==domain==:==ServerChallenge:NTproofstring==:==modifiedntlmv2response*
modifiedntlmv2response(去除 NTProofStr 后的一整个 NTLM Response 流):
或者去apackets.com提交一下pcapng，它会提供分析后的数据（但是文件会被公开）
组好的hash:`rockyou::C3NGH--DESKTOP:91e15fed933eff0c:e20402e8a924e2de7c1e3fd3f949bc38:01010000000000001df551630839dc011473b30c1fc1c20f000000000200160046004c00410047002d005300450052005600450052000100160046004c00410047002d005300450052005600450052000400160046004c00410047002d005300650072007600650072000300160046004c00410047002d00530065007200760065007200070008001df551630839dc0106000400020000000800300030000000000000000100000000200000a6cd8042becda35cc7967ee26857127fac305123020cefe31fcefbfd7ece32d50a001000000000000000000000000000000000000900200063006900660073002f0046004c00410047002d00530045005200560045005200000000000000000000000000`
然后去github下个hashcat，找个rockyou的字典，然后把组合好的hash存到一个txt里，cmd输入类似的命令`hashcat -m 5600 cm.txt rockyou.txt`
然后就爆破成功，获得了密码，在`hashcat.potfile`里面，是`poohkitty13`。
从网上找到解密脚本

```Python
from Crypto.Cipher import ARC4  
from Crypto.Hash import MD4, MD5, HMAC  
  
password = 'poohkitty13'  
passwordHash = MD4.new(password.encode('utf-16-le')).hexdigest()  
username = 'rockyou'  
domain = 'C3NGH--DESKTOP'  
ntProofStr = 'e20402e8a924e2de7c1e3fd3f949bc38'  
serverChallenge = '91e15fed933eff0c'  
sessionKey = '55d34fb498993a0aceaa960eea8ab7e9'  
  
responseKey = HMAC.new(bytes.fromhex(passwordHash), (username.upper()+domain.upper()).encode('utf-16-le'), MD5).digest()  
keyExchangeKey = HMAC.new(responseKey, bytes.fromhex(ntProofStr), MD5).digest()  
decryptedSessionKey = ARC4.new(keyExchangeKey).decrypt(bytes.fromhex(sessionKey))  
print('Decrypted SMB Session Key is: {}'.format(decryptedSessionKey.hex()))  
# c97ac1002c85cbd04f69f5406c430fbd
```

获得解密后的SMB session key：c97ac1002c85cbd04f69f5406c430fbd
最后选择 编辑-首选项-protocols-SMB2，添加session id和session key，确定后就可以看到并提取smb流量数据。（看不明白问AI）
提取出flag：`flag{D0_U_L1k3_1z_NTLMv2_4nd_r0ckYouuuuu~}`

# Web

## week1

### 前端小游戏

题干：这是一个前端小游戏，开始寻找flag吧！
过程：我们打开容器，游戏不玩，F12看一眼![](assets/img/media/pictures/2026_4_2/Pasted%20image%2020250928201655.png)
翻几下就找到密文`ZmxhZ3tiYTA5OWJlMS0zMmYxLTQwNmUtYWMxZC0yMzZkOWU5ZDRlNjF9`，base64解一下得到flag：`flag{ba099be1-32f1-406e-ac1d-236d9e9d4e61}`

# RE

## week1

### 8086ASM

题干：你的汇编入门了吗？
文件名：8086.asm
~~回答：没有~~很显然是汇编题

```asm
.MODEL SMALL
.STACK 100H
.DATA
    WELCOME_MSG db 'Welcome to 8086ASM.', 0DH, 0AH, '$'
    INPUT_MSG db 'Input your flag:', '$'

    WRONG_MSG db 0DH, 0AH, 'Wrong.', 0DH, 0AH, '$'
    CORRECT_MSG db 0DH, 0AH, 'Correct.', 0DH, 0AH, '$' 

    DATA1 DB 0BBH, 01BH, 083H, 08CH, 036H, 019H, 0CCH, 097H
            DB 08DH, 0E4H, 097H, 0CCH, 00CH, 048H, 0E4H, 01BH
            DB 00EH, 0D7H, 05BH, 065H, 01BH, 050H, 096H, 006H
            DB 03FH, 019H, 00CH, 04FH, 04EH, 0F9H, 01BH, 0D7H
            DB 0CH, 01DH, 0A0H, 0C6H

    DATA2 DW 01122H, 03344H, 01717H, 09090H, 0BBCCH 

    INPUT_BUFFER db 37 dup(0)
    BUFFER db 37 dup(0)
.CODE

START:
    MOV AX, @DATA
    MOV DS, AX
    MOV AH, 09H
    MOV DX, OFFSET WELCOME_MSG
    INT 21H
    MOV DX, OFFSET INPUT_MSG
    INT 21H
    MOV AH,0AH
    MOV DX, OFFSET INPUT_BUFFER
    MOV BYTE PTR[INPUT_BUFFER], 37
    INT 21H
    CALL ENCRYPT
    MOV DI, OFFSET DATA1
    MOV SI, OFFSET INPUT_BUFFER + 2
    MOV CX, 35
LOOP1:
    MOV AX, [DI]
    CMP AX, [SI]
    JNE WRONG_EXIT
    INC DI
    INC SI
    LOOP LOOP1
    JMP CORRECT_EXIT
WRONG_EXI:
    MOV AH,09H
    LEA DX,WRONG_MSG
    INT 21H
    JMP EXIT
CORRECT_EXIT:
    MOV AH,09H
    LEA DX,CORRECT_MSG
    INT 21H
    JMP EXIT
EXIT:
    MOV AX, 4C00H
    INT 21H
ENCRYPT PROC
    PUSH AX
    PUSH BX
    PUSH CX
    MOV SI, OFFSET INPUT_BUFFER + 2
    MOV BX, OFFSET DATA2
    MOV CX, 35
LOOP2:
    PUSH CX
    MOV CL, 2
    MOV AL, [SI]
    ROR AL, CL
    POP CX
    MOV [SI], AL
    MOV AX, WORD PTR[SI]
    XOR AX, WORD PTR[BX]
    MOV WORD PTR[SI], AX
    INC SI
    ADD BX, 2
    CMP BX, OFFSET DATA2 + 10
    JNE CASE1
    MOV BX, OFFSET DATA2 
CASE1:
    LOOP LOOP2
    POP CX
    POP BX
    POP AX
    RET
ENCRYPT ENDP 

END START
```

~~AI能直接跑出解密脚本~~
AI的回答
以下是汇编程序的逆向分析和正确 Flag 的解密过程。该程序通过循环右移和异或操作对用户输入进行加密，并与预设的 `DATA1` 数据比较。解密需逆向执行加密操作，从后向前处理，并处理字节间依赖关系。

#### 加密流程分析

1. **初始化**：

   - 读取用户输入 35 字节（存储于 `INPUT_BUFFER+2`）。
   - 调用 `ENCRYPT` 过程加密输入。
2. **加密操作 (**​**​`ENCRYPT`​**​ ** 过程)** ：

   - 遍历 35 字节（`i=0` 到 `34`）：
     - 当前字节循环右移 2 位（`ROR AL, 2`）。
     - 以当前字节和下一字节组成字（小端序）与 `DATA2` 密钥字异或。
     - 密钥循环使用 5 个值：`0x1122`, `0x3344`, `0x1717`, `0x9090`, `0xBBCC`）。
   - 示例（位置 `i=0`）：
     - 输入 `'f' (0x66)` → 右移 2 位 → `0x99`
     - 与密钥 `0x1122` 异或 → 字 `0x6C99` ⊕ `0x1122` = `0x7DBB`
     - 结果：位置 `0` 存 `0xBB`，位置 `1` 存 `0x7D`
3. **校验**：

   - 比较加密后的输入与 `DATA1` 的 35 字节。
   - 匹配则输出 `"Correct"`。

#### 解密算法

需从后向前（`i=34` 到 `0`）逆操作：

1. **输入**：

   - `DATA1` 的 35 字节（加密结果）。
   - 第 36 字节（位置 35）在加密后为 `0xBB`（初始为 `0` 并经加密得出）。
2. **步骤**：

   - 对每个 `i`：
     - 取位置 `i` 和 `i+1` 的字节组成字（小端序）。
     - 与密钥字 `KEY = DATA2[i % 5]` 异或。
     - 分离字：低字节 `A` 为位置 `i` 循环右移后的值，高字节 `B` 为位置 `i+1` 的原始值。
     - 对 `A` 循环左移 2 位（`ROL`）恢复位置 `i` 的原始值。
     - 将 `B` 存入位置 `i+1`。
   - 公式：
     ```
     word = (S[i+1] << 8) | S[i]
     word = word ⊕ KEY[i % 5]
     S[i] = ROL(word & 0xFF, 2)   // 低字节循环左移
     S[i+1] = (word >> 8) & 0xFF  // 高字节直接还原
     ```

#### 解密代码（Python 实现）

```python
# 加密数据 (DATA1)
data1 = [
    0xBB, 0x1B, 0x83, 0x8C, 0x36, 0x19, 0xCC, 0x97,
    0x8D, 0xE4, 0x97, 0xCC, 0x0C, 0x48, 0xE4, 0x1B,
    0x0E, 0xD7, 0x5B, 0x65, 0x1B, 0x50, 0x96, 0x06,
    0x3F, 0x19, 0x0C, 0x4F, 0x4E, 0xF9, 0x1B, 0xD7,
    0x0C, 0x1D, 0xA0
]

# 密钥字 (DATA2)
key_words = [0x1122, 0x3344, 0x1717, 0x9090, 0xBBCC]
S = data1 + [0xBB]  # 添加第 36 字节

for i in range(34, -1, -1):  # 从 34 到 0
    low = S[i]
    high = S[i + 1]
    word_val = (high << 8) | low
    key_val = key_words[i % 5]
    word_val ^= key_val
    
    A = word_val & 0xFF        # 循环右移后的字节
    B = (word_val >> 8) & 0xFF  # 下一字节的原始值
    
    # 循环左移 2 位 (ROL)
    S[i] = ((A << 2) | (A >> 6)) & 0xFF
    S[i + 1] = B

# 提取前 35 字节为原始 Flag
flag_bytes = bytes(S[:35])
print("Flag:", flag_bytes.decode())
```

#### 解密结果

运行上述代码，得到 **Flag：**​**​`flag{We1c0m3_t0_8086_Ass3mb1y_w0r1d!}`​** 。

**验证**

- 首字节 `'f' (0x66)` 的加密过程：
  - 循环右移 2 位 → `0x99`
  - 与密钥 `0x1122` 异或 → 位置 `0` 变为 `0xBB`（匹配 `DATA1`）。
- 其他字节同理，加密后与 `DATA1` 完全匹配。

此 Flag 符合程序逻辑，输入后输出  **"Correct"** 。

由小艺AI生成<xiaoyi.huawei.com>

### PlzDebugMe

题干：超级简单呢~快去尝试debug出flag吧！
过程：先从字符表找到核心函数

```C
int sub_401697()
{
  int i; // [esp+1Ch] [ebp-4h]

  sub_401860();
  printf("WeLcome to Reverse World!\n");
  printf("Length of the flag is 32.\n");
  printf("Please enter your flag:\n");
  sub_4015D0("%s", input);
  if ( input[0] == 'f'
    && byte_415061 == 'l'
    && byte_415062 == 'a'
    && byte_415063 == 'g'
    && byte_415064 == '{'
    && byte_41507F == '}' )
  {
    sub_401648(123456);
    for ( i = 0; i <= 31; ++i )
    {
      input[i] = sub_40167D(input[i]);
      if ( input[i] != byte_410020[i] )
      {
        printf("Pity! Wrong flag!\n");
        return 0;
      }
    }
    printf("Congratulations! You get the flag!\n");
    return 0;
  }
  else
  {
    printf("WRONG\n");
    return 0;
  }
}
```

一眼，`sub_40167D`是加密函数，`byte_410020`是密文。加密函数我们看看，是

```C
int __cdecl sub_40167D(unsigned __int8 a1)
{
  return (unsigned __int8)sub_401656() ^ a1;
}
```

再跟进去，看到

```C
unsigned int sub_401656()
{
  dword_415080 = 1103515245 * dword_415080 + 12345;
  return HIWORD(dword_415080) & 0x7FFF;
}
```

这里的dword_415080很明显是全局变量，每次异或都会改变。动态调试得到它的初始值是`123456`，HIWORD保留32位数（dword)的高16位。还有个LOWORD，效果是反过来的
我们我们提取下密文数据，写出脚本（过程中我们发现`& 0x7FFF`不需要，我们只要低8位即一个字节的数据）

```Python
dword = 123456  
# fl = bytearray.fromhex("5B50A125848E61C46BBBAE050BC63D425AFBC1C94EE98D5091878724ADAFD536")  
key = []  
flf = [0x5B, 0x50, 0xA1, 0x25, 0x84, 0x8E, 0x61, 0xC4, 0x6B, 0xBB,  
0xAE, 0x05, 0x0B, 0xC6, 0x3D, 0x42, 0x5A, 0xFB, 0xC1, 0xC9,  
0x4E, 0xE9, 0x8D, 0x50, 0x91, 0x87, 0x87, 0x24, 0xAD, 0xAF,  
0xD5, 0x36]  
for i in range(32):  
    dword = ((dword * 0x41C64E6D + 12345) & 0xFFFFFFFF)  
    pl = dword >> 16  
    key.append(pl)  
for i in range(32):  
    flf[i] = (flf[i] ^ key[i]) & 0xFF  
  
for i in range(32):  
    print(chr(flf[i]), end='')
```

得到flag：`flag{Y0u_Kn0w_H0w_t0_D3bug!!!!!}`

### ezCSharp

题干：用ida看不懂？那就对了，建议换个工具（你知道dnspy吗？）
过程：我们根据提示，用dnspy打开来看![](assets/img/media/pictures/2026_4_2/Pasted%20image%2020250928203755.png)
我们能找到密文`D1ucj0u!tqjwf!fohjoffsjoh!xj!epspqz!ju!gvo!2025`。下面的program里是加密函数
![](assets/img/media/pictures/2026_4_2/Pasted%20image%2020250928204733.png)
我们照着写一写，得到脚本

```Python
cip = "D1ucj0u!tqjwf!fohjoffsjoh!xj!epspqz!ju!gvo!2025"  
lcip = list(cip)  
lenc = len(cip)  
for i in range(lenc):  
    if lcip[i] == '!':  
        lcip[i] = '_'  
    elif lcip[i] == 'a':  
        lcip[i] = 'z'  
    elif ord(lcip[i]) > 96:  
        c = ord(lcip[i])  
        c -= 1  
        lcip[i] = chr(c)  
    # elif lcip[i] == 'b':  
    #     c = ord(lcip[i])    #     c -= 1    #     lcip[i] = chr(c)for i in range(lenc):  
    print(lcip[i], end = '')
```

套下格式，得到flag：`flag{D1tbi0t_spive_engineering_wi_doropy_it_fun_2025}`

### ezCalculate

题干：考察简单的数值计算
过程：主函数

```C
int __fastcall main(int argc, const char **argv, const char **envp)
{
  size_t v3; // r12
  unsigned __int64 v4; // rdi
  size_t v5; // rbx
  size_t v6; // r8
  unsigned __int64 v7; // rcx
  size_t v8; // r8
  unsigned __int64 v9; // rcx
  __int64 i; // rax
  char Str[248]; // [rsp+20h] [rbp-F8h] BYREF

  _main(argc, argv, envp);
  scanf_constprop_0(&unk_14000C000, Str);
  v3 = strlen(Str);
  if ( !v3 )                                    // 空的
    goto LABEL_17;
  v4 = 0;
  do
  {
    v5 = strlen(key);
    Str[v4] += key[v4 % v5];
    ++v4;
  }
  while ( v3 != v4 );
  v6 = strlen(Str);
  if ( !v6 )
    goto LABEL_17;
  v7 = 0;
  do
  {
    Str[v7] ^= key[v7 % v5];
    ++v7;
  }
  while ( v6 != v7 );
  v8 = strlen(Str);
  if ( !v8 )
    goto LABEL_17;
  v9 = 0;
  do
  {
    Str[v9] -= key[v9 % v5];
    ++v9;
  }
  while ( v8 != v9 );
  if ( strlen(Str) == 21 )
  {
    for ( i = 0; i != 21; ++i )
    {
      if ( Str[i] != answer[i] )
      {
        printf("wrong");
        return 0;
      }
    }
    printf("right");
  }
  else
  {
LABEL_17:
    printf("incorrect length");
  }
  return 0;
}
```

我们看一看，双击answer跳转看一下是啥，
![](assets/img/media/pictures/2026_4_2/Pasted%20image%2020250928205138.png)
就能写出脚本（逆向操作一下就完事了）

```Python
ad = "wwqessgxsddkaao123wms"  
key = list(ad)  
v4 = 0  
v5 = len(ad)  
cip = [  0x33, 0x1D, 0x32, 0x44, 0x2A, 0x54, 0x45, 0x2C, 0x2E, 0x74,  
  0x8C, 0x4B, 0x40, 0x42, 0x43, 0x73, 0x71, 0x82, 0x24, 0x35,  
  0x10]  
v9 = 0  
v8 = 21  
for i in range(21):  
    cip[i] += ord(key[i % v5])  
for i in range(21):  
    cip[i] ^= ord(key[i % v5])  
for i in range(21):  
    cip[i] -= ord(key[i % v5])  
for i in range(21):  
    print(chr(cip[i]), end='')
```

得到flag：`flag{Add_X0r_and_Sub}`

### jvav

题干：觉得Main很史可以直接去看加密
过程：安卓逆向，jadx看一下
![](assets/img/media/pictures/2026_4_2/Pasted%20image%2020250928205409.png)
找到一个加密工具，

```Java
package com.utilis.enc;
import java.util.Base64;
import kotlin.Metadata;
import kotlin.jvm.internal.Intrinsics;
import kotlin.text.Charsets;
/* compiled from: enc.kt */
@Metadata(d1 = {"\u0000\u0016\n\u0000\n\u0002\u0010\u0012\n\u0000\n\u0002\u0010\u000e\n\u0002\b\u0003\n\u0002\u0010\u000b\n\u0000\u001a\u000e\u0010\u0000\u001a\u00020\u00012\u0006\u0010\u0002\u001a\u00020\u0003\u001a\u000e\u0010\u0004\u001a\u00020\u00012\u0006\u0010\u0002\u001a\u00020\u0001\u001a\u000e\u0010\u0005\u001a\u00020\u00012\u0006\u0010\u0002\u001a\u00020\u0001\u001a\u000e\u0010\u0006\u001a\u00020\u00072\u0006\u0010\u0002\u001a\u00020\u0003¨\u0006\b"}, d2 = {"encoder", "", "input", "", "confuser", "rounder", "checker", "", "app_release"}, k = 2, mv = {2, 0, 0}, xi = 48)
  
/* loaded from: classes2.dex */
  
public final class EncKt {
    public static final byte[] encoder(String input) {
        Intrinsics.checkNotNullParameter(input, "input");
        byte[] bytes = input.getBytes(Charsets.UTF_8);
        Intrinsics.checkNotNullExpressionValue(bytes, "getBytes(...)");
        byte[] encode = Base64.getEncoder().encode(bytes);
        Intrinsics.checkNotNullExpressionValue(encode, "encode(...)");
        return encode;
    }
    public static final byte[] confuser(byte[] input) {
        Intrinsics.checkNotNullParameter(input, "input");
        int length = input.length;
        for (int i = 0; i < length; i++) {
            input[i] = (byte) (~((input[i] + 32) ^ 11));
        }
        return input;
    }
  
    public static final byte[] rounder(byte[] input) {
        Intrinsics.checkNotNullParameter(input, "input");
        byte[] bArr = new byte[input.length];
        int length = input.length;
        for (int i = 0; i < length; i++) {
            bArr[i] = input[(i + 5) % input.length];
        }
        return bArr;
    }
    public static final boolean checker(String input) {
        Intrinsics.checkNotNullParameter(input, "input");
        byte[] rounder = rounder(confuser(encoder(input)));
        byte[] bArr = {-89, 96, 102, 118, -89, -122, 103, -103, -125, -95, 114, 117, -116, -102, 114, -115, -125, 108, 110, 118, -91, -83, 101, -115, -116, -114, 124, 114, -123, -87, -87, -114, 121, 108, 124, -114};
        if (rounder.length != 36) {
            return false;
        }
        int length = rounder.length;
        for (int i = 0; i < length; i++) {
            if (rounder[i] != bArr[i]) {
                return false;
            }
        }
        return true;
    }
}
```

里面各个函数作用都说清楚了，rounder作用是使字符串向左滚动字符串，confuser异或加密，encoder是base64。我们按照checker的数据，去编写对应脚本

```Python
cip = [-89, 96, 102, 118, -89, -122, 103, -103, -125, -95, 114, 117, -116, -102, 114,  
 -115, -125, 108, 110, 118, -91, -83, 101, -115, -116, -114, 124, 114, -123, -87, -87, -114, 121, 108, 124, -114]  
lcip = len(cip)  
bArr = [0]*lcip  
for i in range(lcip):  
    bArr[i] = cip[(i + lcip - 5) % lcip]  
# print(bArr)  
for i in range(lcip):  
    # tmp = (bArr[i] ^ 11) & 0xFF  
    # tmp = ((abs(bArr[i]) & 0xFF) ^ 11)    tmp = (((bArr[i] & 0xFF) ^ 0xFF) ^ 11) & 0xFF  
    tmp -= 32  
    bArr[i] = tmp & 0xFF  
print(bArr)  
for i in range(lcip):  
    print(chr(bArr[i]), end='')
```

得到base64字符串`ZmxhZ3trb3RsMW5faXNfYWxzb19qYXZhfQ==`，解密得到flag：`flag{kotl1n_is_also_java}`

### rand

题干：随机的加密总是好的
过程：~~最难的一个？~~ 
先看主函数

```C
__int64 __fastcall main(int a1, char **a2, char **a3)
{
  int v3; // ebp
  size_t v4; // r12
  int v5; // r13d
  int v6; // eax
  char v7; // si
  __int64 v8; // rax
  char *v9; // rbx

  v3 = 256;
  __isoc99_scanf(&unk_555555556004, input, a3);
  v4 = strlen(input);
  do
  {
    v5 = rand() % (int)v4;                      
    v6 = rand();
    v7 = input[v5];
    v8 = v6 % (int)v4;
    input[v5] = input[v8];                      
    input[v8] = v7;
    --v3;
  }
  while ( v3 );
  v9 = input;
  do
    *v9++ ^= rand();
  while ( v9 != &input[(unsigned int)(v4 - 1) + 1] );
  if ( v4 == 27 && unk_555555558010 == *(_OWORD *)input && xmmword_55555555801B == xmmword_55555555806B )
    puts("right !");
  else
    puts("wrong !");
  return 0;
}
```

是一个自定义的加密算法。我们知道，rand()是伪随机的，有一个用于初始化的srand，我们在函数表里找一下
![](assets/img/media/pictures/2026_4_2/Pasted%20image%2020250928210435.png)
找到了srand的种子`0x3039`。由于这是一个linux的可执行文件，rand在相同种子情况下生成的随机数不同。
看主函数，flag长度位27，最后比较部分看着有点奇怪，双击看过去
![](assets/img/media/pictures/2026_4_2/Pasted%20image%2020250928210830.png)
把这些值都提取一下，发现刚好27个字节，于是**大胆猜测这些数据拼起来就是密文**。
对了，rand()在同种子情况下，每次生成的随机数序列都是一样的
分析下主函数（这个就不用说了吧），我们这次编写C++的解密脚本

```CPP
#include <iostream>
#include <cstdlib>
using namespace std;
int main(){
	srand(0x3039);
	for(int i = 0; i < 512; i++)
		rand();
	int cip[27] = {0x5A, 0x66, 0x86, 0xCE, 0x46, 0x23, 0x75, 0x30, 0x18, 0x6F, 0x5B, 0x7D, 0x4D, 0x4F, 0xF7, 0xC4, 0x4A, 0x0D, 0x45, 0xAE, 0x36, 0xEF,0x6B, 0x81, 0xC1, 0x82, 0x03};
	for(int i = 0; i < 27; i++){
		cip[i] = cip[i] ^ rand();
	}
	srand(0x3039);
	int rndL[514] = {0};
	for(int i = 0; i < 514; i++){
		rndL[i] = rand();
	}
	for(int i = 256; i > -1; i--){
		int i1 = rndL[2*i] % 27;
		int i2 = rndL[2*i + 1] % 27;
		swap(cip[i1], cip[i2]);
	}
	for(int i = 0; i < 27; i++){
		char tmp = static_cast<char>(cip[i]);
		cout << tmp;
	}

	return 0;
}
```

linux环境下编译运行一下，
![](assets/img/media/pictures/2026_4_2/9b14506e616064af20a4ae0124c06dc4.png)
发现得到的flag似乎有点奇怪（实际上已经能自己拼出正确的flag了）~~不是为啥啊，为啥会这样~~
多跑几遍
![](assets/img/media/pictures/2026_4_2/0f6113de340c25ffdf9066dc20e3c7af.png)
很明显，flag：`flag{there_1s_s0_many_rand}`
PS：问AI找到原因了，数组越界了，但好像避免不了，不管怎么修改总是有问题

## week2

### base

~~w2最简单的一道，没有之一~~
题干：base家族小合集
过程：看一眼主函数

```C
__int64 __fastcall main(int argc, char **argv)
{
  const char *encoded_message; // [rsp+20h] [rbp-20h]
  const char *encoded_user_alphabet; // [rsp+28h] [rbp-18h]
  char *user_plaintext; // [rsp+30h] [rbp-10h]
  char *user_alphabet; // [rsp+38h] [rbp-8h]

  _main();
  if ( argc != 3 )
  {
    printf("用法: %s <你的Base64字符表> <你的明文>\n", *argv);
    printf("示例: %s ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/ \"Hello, World!\"\n", *argv);
    return 1;
  }
  user_alphabet = argv[1];
  user_plaintext = argv[2];
  printf("接收到你的字符表: %s\n", user_alphabet);
  printf("接收到你的明文:   %s\n\n", user_plaintext);
  if ( strlen(user_alphabet) != 64 )
  {
    puts("错误: 你提供的Base64字符表长度不正确。它必须是64个字符。");
    return 1;
  }
  encoded_user_alphabet = base58_encode_str(user_alphabet);
  if ( !encoded_user_alphabet )
  {
    puts("错误: Base58编码失败，内存分配失败。");
    return 1;
  }
  puts("正在验证你的Base64字符表...");
  if ( !strcmp(encoded_user_alphabet, CORRECT_ENCODED_BASE64_ALPHABET) )
  {
    puts("✓ 字符表验证成功！\n");
    puts("正在使用你的字符表对明文进行Base64加密...");
    encoded_message = base64_encode(user_plaintext, user_alphabet);
    if ( !encoded_message )
    {
      puts("错误: Base64编码失败，内存分配失败。");
      return 1;
    }
    printf("加密后的明文: %s\n", encoded_message);
    puts("正在验证明文...");
    if ( !strcmp(encoded_message, CORRECT_ENCODED_MESSAGE) )
    {
      puts("✓ 明文验证成功！");
      puts("   你提供的字符表和明文都是正确的。");
    }
    else
    {
      puts("✕ 明文验证失败。");
      puts("   你提供的明文不是正确的明文。");
      printf("   预期的密文是: %s\n", CORRECT_ENCODED_MESSAGE);
    }
  }
  else
  {
    puts("✕ 字符表验证失败。");
    puts("   你提供的字符表不是正确的Base64字符表。");
    printf("   预期的Base58编码结果是: %s\n", CORRECT_ENCODED_BASE64_ALPHABET);
    printf("   你得到的Base58编码结果是: %s\n", encoded_user_alphabet);
  }
  return 0;
}
```

我们就知道用了base64+base58两个加密（看英文，直接告诉你这加密啥了），base58把base64用的字符表加密了。看一眼字符串表
![](assets/img/media/pictures/2026_4_2/Pasted%20image%2020251006005516.png)
头几个就是我们要的东西

```asm
.rdata:0000000140005000 a2wvnsjresxyfyt db '2wvnsjrESxyfytuhEwqChbLLZRtA4VLhf5HgrKNRR3jYZGgyd1XHEhypTQ8b546tx'
.rdata:0000000140005000                                         ; DATA XREF: .data:CORRECT_ENCODED_BASE64_ALPHABET↑o
.rdata:0000000140005041                 db 'jJx7wHgJaJw2mBxbDtS8dCS',0
.rdata:0000000140005059 aZmxhz3tubdrpc1 db 'zMXHz3TuBdrPC18XB0bZzx0=',0
.rdata:0000000140005059                                         ; DATA XREF: .data:CORRECT_ENCODED_MESSAGE↑o
```

上面的一整串`2wvnsjrESxyfytuhEwqChbLLZRtA4VLhf5HgrKNRR3jYZGgyd1XHEhypTQ8b546txjJx7wHgJaJw2mBxbDtS8dCS`都是我们需要的，一眼看出是base58加密后的字符表
解密一下得到base64用的字符表`abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789+/`，下面的内容再用这个解密一下，
就得到flag：`flag{Tl4is_1o@se}`​~~确实ez，直接秒了~~ (~~其实还是做了一小会~~)

### Pyc

题干：这个exe怎么看起来有点不一样...
过程：这是个python逆向题，我们有一个`pyc.exe`，![](assets/img/media/pictures/2026_4_2/Pasted%20image%2020251006003830.png)
去github上找个pyinstxtractor，cmd里面敲一下`python 地址\pyinstxtractor.py 地址\pyc.exe`（其实是敲一下，拖两下）例如`python D:\pyinstxtractor-2025.02\pyinstxtractor.py D:\pyinstxtractor-2025.02\pyc.exe`
如果python不是3.8版本的![](assets/img/media/pictures/2026_4_2/Pasted%20image%2020251006004139.png)
会提示版本不对，并且跳过解压过程。我们下个3.8的python，并直接到对应目录下重新执行以上命令![](assets/img/media/pictures/2026_4_2/Pasted%20image%2020251006004240.png)
就能得到解压后的东西找一下里面的`pyc.pyc`，这里是主程序
![](assets/img/media/pictures/2026_4_2/Pasted%20image%2020251006004336.png)
随便找个网站，或者pip装一下uncompyle6（随便网上搜搜就行，很多方法的）
就能反编译字节码，得到~~柚子厨蒸鹅心~~

```Python
print("Ciallo~")  
print("Plz input your flag~")  
flag = input()  
flag_list = list(flag)  
for i in range(len(flag_list)):  
    a = flag_list[i]  
    if "a" <= a <= "z":  
        a = ord(a)  
        a = (a - 12) * 2 + 6  
    else:  
        if "A" <= a <= "Z":  
            a = ord(a)  
            a = (a + 6) * 3 + 9  
        else:  
            a = ord(a)  
            a = a + 11  
    flag_list[i] = chr(a)  
else:  
    flag = "".join(flag_list)  
    hex_flag = ",".join([hex(ord(c)) for c in flag])  
    data = "0xba,0xc6,0xb0,0xbc,0x86,0x10b,0x126,0xe4,0x6a,0xc0,0x40,0x6a,0xda,0x3f,0xd2,0xe0,0x6a,0xb8,0x3f,0xd4,0xe0,0x89,0x88"  
    if data != hex_flag:  
        print("wrong~")  
    else:  
        print("great~")
```

我们分析一下，写出对应的解密脚本（这里我们知道加密前是ASCII表里面的可打印字符，加密后又会变大特别多，故每个都尝试一下就行）

```Python
def decrypt_flag():  
    hex_data = "0xba,0xc6,0xb0,0xbc,0x86,0x10b,0x126,0xe4,0x6a,0xc0,0x40,0x6a,0xda,0x3f,0xd2,0xe0,0x6a,0xb8,0x3f,0xd4,0xe0,0x89,0x88"  
    hex_list = hex_data.split(',')  
    encrypted_values = [int(x, 16) for x in hex_list]  
    flag = ''  
    for val in encrypted_values:  
        if (val - 6) % 2 == 0: 
            char_val = (val - 6) // 2 + 12  
            if 97 <= char_val <= 122: 
                flag += chr(char_val)  
                continue  
        if (val - 9) % 3 == 0:  
            char_val = (val - 9) // 3 - 6  
            if 65 <= char_val <= 90: 
                flag += chr(char_val)  
                continue  
        char_val = val - 11  
        flag += chr(char_val)  
    return flag  
decrypted_flag = decrypt_flag()  
print(decrypted_flag)
```

得到flag：`flag{PYC_i5_v4ry_e4sy~}`

### UPX

#UPX# #手动脱壳# #魔改# #RE# #脱壳#
题干：这应该是道手脱题。。。
过程：根据题目信息，我们知道这是一道手动脱壳题
DIE能识别出UPX![](assets/img/media/pictures/2026_4_2/Pasted%20image%2020251006001144.png)
我们用x64dbg来手动脱壳，使用ESP(64位系统对应RSP)定律法来手动脱壳。[参考文献](https://www.cnblogs.com/Meteor-Kai/articles/18473549)
先按F9跳到程序提供的入口点
![](assets/img/media/pictures/2026_4_2/Pasted%20image%2020251006001402.png)
这里4个push相当于pushad，我们在这之后下个断点，运行到这里，能看到RSP被修改了
![](assets/img/media/pictures/2026_4_2/Pasted%20image%2020251006001519.png)
这之后，我们对栈顶的内存地址(RSP存储的地址)下个断点（在转储中跟随）（大小随意）
![](assets/img/media/pictures/2026_4_2/Pasted%20image%2020251006002933.png)
![](assets/img/media/pictures/2026_4_2/Pasted%20image%2020251006002935.png)
下完后，按F9运行，肯定能看到pop或者popad
![](assets/img/media/pictures/2026_4_2/Pasted%20image%2020251006003111.png)
这下面有一个大跳，我们双击跳转过去，跳到的地方就是OEP（原始入口点）了
然后用插件dump（转储）一下（断点下不下无所谓，可以下一个防止手贱）
![](assets/img/media/pictures/2026_4_2/Pasted%20image%2020251006003314.png)
dump得到的程序（会比源程序大不少）就可以静态分析了
我们打开得到的程序，通过字符串表迅速定位下主函数

```C
__int64 sub_7FF67F7A1450()
{
  char v1[48]; // [rsp+20h] [rbp-60h] BYREF
  _DWORD v2[8]; // [rsp+50h] [rbp-30h] BYREF
  char v3; // [rsp+70h] [rbp-10h]
  int v4; // [rsp+74h] [rbp-Ch]
  int v5; // [rsp+78h] [rbp-8h]
  int i; // [rsp+7Ch] [rbp-4h]

  sub_7FF67F7A16D0();
  strcpy((char *)v2, "fkyd{YNek_SD_AB@ars_OKT}");
  BYTE1(v2[6]) = 0;
  HIWORD(v2[6]) = 0;
  v2[7] = 0;
  v3 = 0;
  v5 = sub_7FF67F7A2A28(v2);
  *((_BYTE *)v2 + v5) = 0;
  sub_7FF67F7A2A78(aInputTheFlag);
  sub_7FF67F7A2740("%s", v1);
  v4 = sub_7FF67F7A2A28(v1);
  v1[v4] = 0;
  for ( i = 0; v1[i] != 125; ++i )
  {
    if ( v1[i] > 64 && v1[i] <= 90 )
      v1[i] = (v1[i] - 65 + i + 26) % 26 + 65;
    if ( v1[i] > 96 && v1[i] <= 122 )
      v1[i] = (v1[i] - 97 - i + 26) % 26 + 97;
  }
  if ( (unsigned int)sub_7FF67F7A2A20(v1, v2) )
    sub_7FF67F7A2A78(aWrong);
  else
    sub_7FF67F7A2A78(aCongratulation);
  return 0;
}
```

能明显看到密文和加密部分，就能编写出对应脚本

```Python
cip = list("fkyd{YNek_SD_AB@ars_OKT}")  
l = len(cip)  
for i in range(l):  
    if ord(cip[i]) > ord('@') and ord(cip[i]) <= ord('Z'):  
        cip[i] = chr((ord(cip[i]) - ord('A') - i + 26) % 26 + ord('A'))  
    elif ord(cip[i]) > ord('`') and ord(cip[i]) <= ord('z'):  
        cip[i] = chr((ord(cip[i]) - ord('a') + i + 26) % 26 + ord('a'))  
for i in range(l):  
    print(cip[i], end='')
```

运行后得到flag：`flag{THls_IS_NN@qik_UPX}`

### Do You Like To Drink Tea?

#TEA# #变种# #RE#
题干：艾丽丝邀请你来魔女会喝茶，但是这杯TEA似乎被施加了魔法...
过程：题目明示这是~~魔改~~TEA。按照常例，先把dword值换成16进制显示，主函数就是

```C
int __fastcall main(int argc, const char **argv, const char **envp)
{
  _DWORD v4[10]; // [rsp+20h] [rbp-80h]
  unsigned __int64 v5; // [rsp+48h] [rbp-58h] BYREF
  _QWORD v6[3]; // [rsp+50h] [rbp-50h] BYREF
  int v7; // [rsp+68h] [rbp-38h]
  __int16 v8; // [rsp+6Ch] [rbp-34h]
  int v9; // [rsp+70h] [rbp-30h]
  int v10; // [rsp+74h] [rbp-2Ch]
  int v11; // [rsp+78h] [rbp-28h]
  int v12; // [rsp+7Ch] [rbp-24h]
  void *Block; // [rsp+80h] [rbp-20h]
  int k; // [rsp+88h] [rbp-18h]
  int j; // [rsp+8Ch] [rbp-14h]
  int v16; // [rsp+90h] [rbp-10h]
  int i; // [rsp+94h] [rbp-Ch]
  unsigned int v18; // [rsp+98h] [rbp-8h]
  unsigned int v19; // [rsp+9Ch] [rbp-4h]

  sub_4023F0(argc, argv, envp);
  v9 = 0x12345678;
  v10 = 0xABCDEF01;
  v11 = 0x11451419;
  v12 = 0x19198101;
  puts("Ciallo~");
  puts("Plz input your flag:");
  memset(v6, 0, sizeof(v6));
  v7 = 0;
  v8 = 0;
  scanf("%s", v6);
  Block = (void *)sub_401530(v6, &v5);
  for ( i = 0; i < v5 - 1; ++i )
  {
    v19 = *((_DWORD *)Block + i);
    v18 = *((_DWORD *)Block + i + 1);
    v16 = 0;
    for ( j = 0; j <= 31; ++j )
    {
      v19 -= ((v18 << 6) + v9) ^ (v16 + v18 + 11) ^ ((v18 >> 9) + v10);
      v18 -= ((v19 << 6) + v11) ^ (v16 + v19 + 20) ^ ((v19 >> 9) + v12);
      v16 -= 1640531527;
    }
    *((_DWORD *)Block + i) = v19;
    *((_DWORD *)Block + i + 1) = v18;
  }
  v4[0] = 0xF05D46E8;
  v4[1] = 0x4785FFEF;
  v4[2] = 0xF401BF82;
  v4[3] = 0xE5FCC60A;
  v4[4] = 0xBE70045D;
  v4[5] = 0x20788733;
  v4[6] = 0x933BA369;
  for ( k = 0; k < v5; ++k )
  {
    if ( v4[k] != *((_DWORD *)Block + k) )
    {
      printf("OH ~ NO!");
      exit(0);
    }
  }
  printf("I love drink tea too ~");
  free(Block);
  return 0;
}
```

==其实不管加密算法究竟是什么，我们只要做逆运算就行了==。为了方便核验内容，我们可以改一下主函数（或者脚本模仿主函数写，我这里用了模板，故改主函数方便核验脚本）

```C
int __fastcall main(int argc, const char **argv, const char **envp)
{
  _DWORD v4[10]; // [rsp+20h] [rbp-80h]
  size_t v5; // [rsp+48h] [rbp-58h] BYREF
  _QWORD v6[3]; // [rsp+50h] [rbp-50h] BYREF
  int v7; // [rsp+68h] [rbp-38h]
  __int16 v8; // [rsp+6Ch] [rbp-34h]
  int v9; // [rsp+70h] [rbp-30h]
  unsigned int v10; // [rsp+74h] [rbp-2Ch]
  int v11; // [rsp+78h] [rbp-28h]
  int v12; // [rsp+7Ch] [rbp-24h]
  void *Block; // [rsp+80h] [rbp-20h]
  int k; // [rsp+88h] [rbp-18h]
  int j; // [rsp+8Ch] [rbp-14h]
  int ttl; // [rsp+90h] [rbp-10h]
  int i; // [rsp+94h] [rbp-Ch]
  unsigned int m2; // [rsp+98h] [rbp-8h]
  unsigned int m1; // [rsp+9Ch] [rbp-4h]

  sub_4023F0();
  v9 = 0x12345678;
  v10 = 0xABCDEF01;
  v11 = 0x11451419;
  v12 = 0x19198101;
  puts("Ciallo~");
  puts("Plz input your flag:");
  memset(v6, 0, sizeof(v6));
  v7 = 0;
  v8 = 0;
  scanf("%s", v6);
  Block = littleEndian((const char *)v6, &v5);
  for ( i = 0; i < v5 - 1; ++i )
  {
    m1 = *((_DWORD *)Block + i);
    m2 = *((_DWORD *)Block + i + 1);
    ttl = 0;
    for ( j = 0; j <= 31; ++j )
    {
      m1 -= ((m2 << 6) + v9) ^ (ttl + m2 + 11) ^ ((m2 >> 9) + v10);
      m2 -= ((m1 << 6) + v11) ^ (ttl + m1 + 20) ^ ((m1 >> 9) + v12);
      ttl -= 0x61C88647;
    }
    *((_DWORD *)Block + i) = m1;
    *((_DWORD *)Block + i + 1) = m2;
  }
  v4[0] = 0xF05D46E8;
  v4[1] = 0x4785FFEF;
  v4[2] = 0xF401BF82;
  v4[3] = 0xE5FCC60A;
  v4[4] = 0xBE70045D;
  v4[5] = 0x20788733;
  v4[6] = 0x933BA369;
  for ( k = 0; k < v5; ++k )
  {
    if ( v4[k] != *((_DWORD *)Block + k) )
    {
      printf("OH ~ NO!");
      exit(0);
    }
  }
  printf("I love drink tea too ~");
  free(Block);
  return 0;
}
```

我们就可以编写对应的脚本

```Python
def b2dle(byte_array):  
    return [int.from_bytes(byte_array[i:i + 4], byteorder='little', signed=False) for i in range(0, len(byte_array), 4)]  
  
  
def d2ble(dword_array):  
    byte_list = []  
    for dword in dword_array:  
        byte_list.extend(dword.to_bytes(4, byteorder='little'))  
    return byte_list  
  
  
v4 = [0] * 7  
v4[0] = 0xF05D46E8  
v4[1] = 0x4785FFEF  
v4[2] = 0xF401BF82  
v4[3] = 0xE5FCC60A  
v4[4] = 0xBE70045D  
v4[5] = 0x20788733  
v4[6] = 0x933BA369  
dwordenc = v4  
key = [0x12345678, 0xABCDEF01, 0x11451419, 0x19198101]  
delta = 0x61C88647  
ttl0 = -957401312 & 0xFFFFFFFF  
# for i in range(32):  
#     ttl0 = (ttl0 - delta) & 0xFFFFFFFF  
# print(ttl0)  
for i in range(5, -1, -1):  
    m1 = dwordenc[i]  
    m2 = dwordenc[i + 1]  
    ttl = ttl0  
    for j in range(32):  
        ttl = (ttl + delta) & 0xFFFFFFFF  
        m2 = (m2 + (key[3] + (m1 >> 9) ^ (ttl + m1 + 20) ^ (key[2] + (m1 << 6)) )) & 0xFFFFFFFF  
        m1 = (m1 + (key[1] + (m2 >> 9) ^ (ttl + m2 + 11) ^ (key[0] + (m2 << 6)) )) & 0xFFFFFFFF  
    dwordenc[i] = m1 #关键步骤！！！！！！！！！  
    dwordenc[i + 1] = m2 #关键步骤！！！！！！！！！  
flag = d2ble(dwordenc)  
flagbytes = bytearray(flag)  
print(flagbytes)
```

运行后得到flag：`flag{OH_I_L0VE_D3inK_Te4!!!}`

### rc4

```
#RC4 #RE #多解
```

题干：你知道吗，rc4有几种解法
过程：

#### 1.常规解法

我们先看主函数

```C
__int64 __fastcall main()
{
  unsigned __int8 cipher[27]; // [rsp+20h] [rbp-60h] BYREF
  unsigned __int8 data[28]; // [rsp+40h] [rbp-40h] BYREF
  char v3; // [rsp+5Ch] [rbp-24h]
  unsigned __int8 key[8]; // [rsp+68h] [rbp-18h] BYREF
  unsigned __int8 s[264]; // [rsp+70h] [rbp-10h] BYREF
  size_t datalen; // [rsp+178h] [rbp+F8h]

  _main();
  strcpy((char *)key, "ohhhRC4");
  *(_QWORD *)cipher = 0xD6DB345DC17A5FF7uLL;
  *(_QWORD *)&cipher[8] = 0x68DAE1DE2D75D82FLL;
  *(_QWORD *)&cipher[16] = 0xF907EACE4A9B57E0uLL;
  *(_DWORD *)&cipher[23] = 0x5E795EF9;
  datalen = 27;
  printf("请输入flag:");
  scanf("%s", data);
  v3 = 0;
  rc4_init(s, key, 7u);
  rc4_crypt(s, data, datalen);
  if ( !memcmp(data, cipher, datalen) )
    printf("Congulation");
  else
    printf("Wrong");
  putchar(10);
  return 0;
}
```

直接拿里面的数据去套标准rc4模板解密，我们会发现解不出来。我们看一眼加密部分

```C
void __cdecl rc4_crypt(unsigned __int8 *s, unsigned __int8 *data, size_t datalen)
{
  unsigned __int8 tmp; // [rsp+Fh] [rbp-11h]
  size_t k; // [rsp+10h] [rbp-10h]
  int j; // [rsp+18h] [rbp-8h]
  int i; // [rsp+1Ch] [rbp-4h]

  i = 0;
  j = 0;
  for ( k = 0; k < datalen; ++k )
  {
    i = (i + 1) % 256;
    j = (s[i] + j) % 256;
    tmp = s[i];
    s[i] = s[j];
    s[j] = tmp;
    data[k] ^= k ^ s[(unsigned __int8)(s[i] + s[j])];
  }
}
```

果不其然，最后一步它进行了一些修改。
对了，上面的QWORD值还要改成大端序，因为加密时用的是大端序，加密后比较时用的是小端序，QWORD显示的是小端序。
而且很重要一点：给出的密文有28个字节的数据，但是比较时只用了前面的27个字节，==我们还原时要去掉最后的一个字节`F9`==
我们找个模板改一下

```Python
def le2be(hex_str: str) -> str:  
    clean_hex = ''.join(filter(str.isalnum, hex_str)).upper()  
    if len(clean_hex) % 2 != 0:  
        clean_hex = '0' + clean_hex  
    bytes_list = [clean_hex[i:i + 2] for i in range(0, len(clean_hex), 2)]  
    return ''.join(bytes_list[::-1])  
def be2le(hex_str: str) -> str:  
    return le2be(hex_str)
    
def rc4_decrypt(cipher, key):  
    S = list(range(256))  
    j = 0  
    for i in range(256):  
        j = (j + S[i] + key[i % len(key)]) % 256  
        S[i], S[j] = S[j], S[i]  
  
    i = j = 0  
    plain = []  
    for k in range(len(cipher)):  
        i = (i + 1) % 256  
        j = (j + S[i]) % 256  
        S[i], S[j] = S[j], S[i]  
        s_val = S[(S[i] + S[j]) % 256]  
        plain_byte = cipher[k] ^ k ^ s_val #问题出在改了这一步
        plain.append(plain_byte)  
    return bytes(plain)  
  
cipherq = [0]*4  
cipherq[0] = 'D6DB345DC17A5FF7'  
cipherq[1] = '68DAE1DE2D75D82F'  
cipherq[2] = 'F907EACE4A9B57E0'  
cipherq[3] = '5E795E' # 去掉了F9
cipherbe = ''  
for i in range(4):  
    cipherbe += le2be(cipherq[i])  
  
cipher = bytes.fromhex(cipherbe)
key = b"ohhhRC4"
 
flag = rc4_decrypt(cipher, key)  
print(flag.decode())
```

执行后，得到flag：`flag{S0NNE_Rc4_l$_c13@nged}`

#### 2.简单解法

rc4是对称加密算法，我们可以把密文丢进去让源程序帮我们解密。
可以利用上一种解法的脚本，把密文的前27个字节改成大端序（每块单独改）
或者用cyberchef的swap endianess把，word length填2，cipher的每块（注意不要把最后一个字节弄进去）单独转换一下，再拼一起也可以。

用脚本改完后是`F75F7AC15D34DBD62FD8752DDEE1DA68E0579B4ACEEA07F95E795E`，可以丢cyberchef选swap endianess，这样弄一下
![](assets/img/media/pictures/2026_4_2/Pasted%20image%2020251006014630.png)
得到带空格的`f7 5f 7a c1 5d 34 db d6 2f d8 75 2d de e1 da 68 e0 57 9b 4a ce ea 07 f9 5e 79 5e`
方便我们之后修改数据。之后我们下断点，随便输点东西
![](assets/img/media/pictures/2026_4_2/Pasted%20image%2020251006014854.png)
双击data跳转，然后patch bytes修改，先直接粘贴整个
![](assets/img/media/pictures/2026_4_2/Pasted%20image%2020251006014932.png)
再比对一下，把剩下的`e0 57 9b 4a ce ea 07 f9 5e 79 5e`复制进去
![](assets/img/media/pictures/2026_4_2/Pasted%20image%2020251006015044.png)
之后回到伪代码，F8单步运行一下，`rc4_crypt()`就帮我们解密完了。再双击data跳转一下，我们看到了flag
![](assets/img/media/pictures/2026_4_2/Pasted%20image%2020251006014358.png)
我们按A识别为文字
![](assets/img/media/pictures/2026_4_2/Pasted%20image%2020251006014401.png)
就得到flag：`flag{S0NNE_Rc4_l$_c13@nged}`了
PS：有些题因为字符较长，你不能直接看到flag，会看到一个QWORD，把那个东西解析成QWORD就能自动识别为指针，指向的内容就是flag
详见moectf 2025的2048_master_re，用到了XXTEA

### CPPReverse

#pdb# #动态调试# #RE#
题干：CPP？听起来似乎比C厉害点，来锻炼锻炼你的动调能力。（附带PDB文件&提交Flag括号内全大写）
附件：`CPPReverse.zip`，里面有`CPPReverse.pdb`和`CPPReverse.exe`
过程：我们在IDA中加载题目附带的符号表(pdb)，得到主函数

```CPP
// Hidden C++ exception states: #wind=1
int __fastcall main(int argc, const char **argv, const char **envp)
{
  std::ostream *v3; // rax
  std::ostream *v5; // rax
  unsigned __int64 v6; // rax
  const std::_String_iterator<std::_String_val<std::_Simple_types<char> > > *v7; // rax
  std::string *v8; // rax
  std::ostream *v9; // rax
  std::string *v10; // rax
  EncryptClass *v11; // rax
  const std::_String_iterator<std::_String_val<std::_Simple_types<char> > > *v12; // rax
  std::ostream *v13; // rax
  bool v14; // [rsp+28h] [rbp-190h]
  EncryptClass *v15; // [rsp+40h] [rbp-178h]
  EncryptClass *v16; // [rsp+48h] [rbp-170h]
  std::string *_Left; // [rsp+58h] [rbp-160h]
  std::string *_Right; // [rsp+60h] [rbp-158h]
  const std::_String_iterator<std::_String_val<std::_Simple_types<char> > > *v19; // [rsp+68h] [rbp-150h]
  const std::_String_iterator<std::_String_val<std::_Simple_types<char> > > *v20; // [rsp+98h] [rbp-120h]
  std::_String_iterator<std::_String_val<std::_Simple_types<char> > > v21; // [rsp+A0h] [rbp-118h] BYREF
  std::_String_iterator<std::_String_val<std::_Simple_types<char> > > v22; // [rsp+A8h] [rbp-110h] BYREF
  std::_String_iterator<std::_String_val<std::_Simple_types<char> > > v23; // [rsp+B0h] [rbp-108h] BYREF
  std::_String_iterator<std::_String_val<std::_Simple_types<char> > > v24; // [rsp+B8h] [rbp-100h] BYREF
  std::string result; // [rsp+C0h] [rbp-F8h] BYREF
  std::string v26; // [rsp+E0h] [rbp-D8h] BYREF
  std::string v27; // [rsp+100h] [rbp-B8h] BYREF
  std::string v28; // [rsp+120h] [rbp-98h] BYREF
  std::string Input; // [rsp+140h] [rbp-78h] BYREF
  std::string ImpStr; // [rsp+160h] [rbp-58h] BYREF
  std::string Result; // [rsp+180h] [rbp-38h] BYREF

  std::string::string(&Input);
  std::operator<<<std::char_traits<char>>(std::cout, "Please input your flag:");
  std::operator>><char>(std::cin, &Input);
  if ( std::string::size(&Input) >= 6 )
  {
    _Left = std::string::substr(&Input, &result, 0, 5u);
    v14 = std::operator!=<char>(_Left, "flag{") || *std::string::back(&Input) != 125;
    std::string::~string(&result);
    if ( v14 )
    {
      v5 = std::operator<<<std::char_traits<char>>(std::cout, "Wrong format.");
      std::ostream::operator<<(v5, std::endl<char,std::char_traits<char>>);
      system("pause");
      std::string::~string(&Input);
      return 0;
    }
    else
    {
      std::string::string(&ImpStr);
      v6 = std::string::size(&Input);
      _Right = std::string::substr(&Input, &v26, 5u, v6 - 6);// 只使用flag{}中间包含的字符串
      std::string::operator=(&ImpStr, _Right);
      std::string::~string(&v26);
      v19 = std::string::end(&ImpStr, &v21);
      v7 = std::string::begin(&ImpStr, &v22);
      std::reverse<std::_String_iterator<std::_String_val<std::_Simple_types<char>>>>(
        (const std::_String_iterator<std::_String_val<std::_Simple_types<char> > >)v7->_Ptr,
        (const std::_String_iterator<std::_String_val<std::_Simple_types<char> > >)v19->_Ptr);// REVERSE!!!!
      std::string::string(&v27, &ImpStr);
      if ( CheckValidInput(v8) )
      {
        v15 = (EncryptClass *)operator new(0x18u);
        if ( v15 )
        {
          std::unique_ptr<std::_Facet_base>::__autoclassinit2(v15, 0x18u);
          std::string::string(&v28, &ImpStr);
          EncryptClass::EncryptClass(v15, v10);
          v16 = v11;
        }
        else
        {
          v16 = 0;
        }
        EncryptClass::Encrypt(v16, &Result);
        v20 = std::string::end(&Result, &v23);
        v12 = std::string::begin(&Result, &v24);
        std::reverse<std::_String_iterator<std::_String_val<std::_Simple_types<char>>>>(
          (const std::_String_iterator<std::_String_val<std::_Simple_types<char> > >)v12->_Ptr,
          (const std::_String_iterator<std::_String_val<std::_Simple_types<char> > >)v20->_Ptr);
        if ( std::operator==<char>(&Result, &EncFlag) )
          v13 = std::operator<<<std::char_traits<char>>(std::cout, "Congratulation!You input a correct flag.");
        else
          v13 = std::operator<<<std::char_traits<char>>(std::cout, "Oooops.You input a wrong flag.");
        std::ostream::operator<<(v13, std::endl<char,std::char_traits<char>>);
        system("pause");
        std::string::~string(&Result);
        std::string::~string(&ImpStr);
        std::string::~string(&Input);
        return 0;
      }
      else
      {
        v9 = std::operator<<<std::char_traits<char>>(std::cout, "The string must be in hexadecimal format.");
        std::ostream::operator<<(v9, std::endl<char,std::char_traits<char>>);
        system("pause");
        std::string::~string(&ImpStr);
        std::string::~string(&Input);
        return 0;
      }
    }
  }
  else
  {
    v3 = std::operator<<<std::char_traits<char>>(std::cout, "Wrong length.");
    std::ostream::operator<<(v3, std::endl<char,std::char_traits<char>>);
    system("pause");
    std::string::~string(&Input);
    return 0;
  }
}
```

~~我们通过题目信息提示，以及在ida中的观察，得知encflag中的内容需要通过动态调试去获取。~~ 
==~~其实不用的，字符串表里头就能看到，一开始我就猜那玩意是encflag，结果真的是~~==
我们换用x64dbg进行动态调试 ~~（因为IDA太难看了）~~ ，根据IDA中的内容，我们在比较部分下断点

- PS：x64dbg我也加载了符号表，不知道是否有用，反正加载就对了
  跟据[官方文档](https://help.x64dbg.com/en/latest/commands/analysis/symload.html)和[论坛上的回答](https://reverseengineering.stackexchange.com/questions/30371/how-to-load-a-pdb-file-into-x64dbg)
  Attempt to load a symbol from disk.

  ## arguments

  `arg1` Module name (with or without extension) to attempt to load symbols for.

  `arg2` Path to the symbol file.

  `[arg3]` Force load. Set to to skip symbol validation. 1是加载，0是不加载
  对应的指令就是 symload cppreverse, 地址\CPPReverse.pdb, 1

```asm
00007FF702483AD9 | E8 D20E0000              | call cppreverse.7FF7024849B0            |
```

在`cppreverse.7FF7024849B0`里面随便下个断点（随便输入的东西，格式满足flag{}就行）
![](assets/img/media/pictures/2026_4_2/Pasted%20image%2020251005235704.png)
我们发现encflag就是字符串表中的`EE1A9B5AFA59AF28DE5D594F8FB990B1D1345590`，我们就可以根据加密函数

```CPP
// Hidden C++ exception states: #wind=1
EncryptClass *__fastcall EncryptClass::Encrypt(EncryptClass *this, std::string *result)
{
  unsigned __int8 *v2; // rcx
  int i; // [rsp+20h] [rbp-38h]
  unsigned __int8 *v5; // [rsp+28h] [rbp-30h]
  unsigned __int8 *v6; // [rsp+30h] [rbp-28h]

  for ( i = 0; i < std::vector<unsigned char>::size(&this->Data); ++i )
  {
    v5 = std::vector<unsigned char>::operator[](&this->Data, i);
    *v5 += i + 7;
    if ( i > 0 )
    {
      v6 = std::vector<unsigned char>::operator[](&this->Data, i);
      *v6 ^= *std::vector<unsigned char>::operator[](&this->Data, i - 1) - 1;
    }
    if ( !(i % 2) )
    {
      v2 = std::vector<unsigned char>::operator[](&this->Data, i);
      *v2 ^= 7u;
    }
  }
  EncryptClass::VecDataToString(this, result);
  return (EncryptClass *)result;
}
```

编写对应的解密脚本

```Python
enc_flag = "EE1A9B5AFA59AF28DE5D594F8FB990B1D1345590"  
enc_bytes = bytes.fromhex(enc_flag[::-1])  
n = len(enc_bytes)  
dec_bytes = list(enc_bytes)
 
for i in range(n-1, -1, -1):   
    if i % 2 == 0:  
        dec_bytes[i] ^= 7    
    if i > 0:  
        dec_bytes[i] ^= (enc_bytes[i-1] - 1) & 0xFF  
    dec_bytes[i] = (dec_bytes[i] - (i + 7)) & 0xFF  
hex_str = ''.join(f"{b:02X}" for b in dec_bytes)  
reversed_hex = hex_str[::-1] 
print(f"flag{{{reversed_hex}}}")
```

得到flag：`flag{4350505F526576657253655F4578705F55705570}`
PS：里面的东西其实是`CPP_REVERSE_EXP_UPUP`，但题目要我们提供其对应的16进制字符串并全大写

### 螺旋密码机

#安卓# #native# #RE# #apk#
题干：这是一个基于数学序列加密的Android应用,应用使用了某种递归算法进行密码验证，算法设计者声称其"静态分析极其困难"。
然而，所有加密算法在程序执行过程中都必须处理明文数据
过程：拖入jadx看一下，发现主程序叫eznative，说明这是一个native层逆向题，核心代码在so库里面。
看看主程序，果然调用了库。

```java
public final class MainActivity extends ComponentActivity {
    public static final int $stable = 0;
    public final native boolean isDebugging();
    public final native String validateMasterPassword(Context context, String password);
    static {
        System.loadLibrary("native-lib");
    }
```

我们解压一下apk，在lib文件夹里找到`libnative-lib.so`（另一个很明显是绘制图像用的库，直接排除掉）
看一眼函数表，
![](assets/img/media/pictures/2026_4_2/Pasted%20image%2020251006171724.png)
很快发现主函数和解密函数。主函数是

```java
__int64 __fastcall Java_com_example_eznative_MainActivity_validateMasterPassword(
        _JNIEnv *a1,
        __int64 a2,
        __int64 a3,
        __int64 a4)
{
  __int64 v4; // x0
  __int64 v6; // [xsp+48h] [xbp-D8h]
  char v7; // [xsp+64h] [xbp-BCh]
  __int64 StringUTFChars; // [xsp+78h] [xbp-A8h]
  __int64 v11; // [xsp+A0h] [xbp-80h]
  __int64 v12; // [xsp+C8h] [xbp-58h] BYREF
  __int64 v13; // [xsp+D0h] [xbp-50h] BYREF
  char v14; // [xsp+DFh] [xbp-41h] BYREF
  char v15[24]; // [xsp+E0h] [xbp-40h] BYREF
  char v16[32]; // [xsp+F8h] [xbp-28h] BYREF
  __int64 v17; // [xsp+118h] [xbp-8h]

  v17 = *(_QWORD *)(_ReadStatusReg(TPIDR_EL0) + 40);
  StringUTFChars = _JNIEnv::GetStringUTFChars(a1, a4, 0);
  __android_log_print(4, "FibValidator", asc_188FC);// 收到密码验证请求
  if ( StringUTFChars )
  {
    std::string::basic_string[abi:ne180000]<0>();
    v4 = sub_2944C(v15);
    __android_log_print(4, "FibValidator", aZu, v4);
    if ( (FibonacciValidator::validate(&v14, v15) & 1) != 0 )
    {
      __android_log_print(4, "FibValidator", aFlag);// 验证成功，返回flag
      v7 = 0;
      v13 = sub_297BC(v15);
      v12 = sub_29808(v15);
      while ( (sub_2985C(&v13, &v12) & 1) != 0 )
      {
        v7 += *(_BYTE *)sub_29890(&v13);
        sub_298A8(&v13);
      }
      __memset_chk(v16, 0, 32, 32);
      decryptFlag(byte_18C04, v16, v7, 249);    // 这里调用解密函数
      __android_log_print(4, "FibValidator", aFlagS, v16);// 解密后的flag %s
      v6 = _JNIEnv::NewStringUTF(a1, v16);
      _JNIEnv::ReleaseStringUTFChars(a1, a4, StringUTFChars);
      v11 = v6;
    }
    else
    {
      __android_log_print(4, "FibValidator", asc_1802D);// 验证失败
      _JNIEnv::ReleaseStringUTFChars(a1, a4, StringUTFChars);
      v11 = _JNIEnv::NewStringUTF(a1, "ERROR: INVALID_PASSWORD");
    }
    std::string::~string(v15);
  }
  else
  {
    v11 = _JNIEnv::NewStringUTF(a1, "ERROR: NULL_INPUT");
  }
  _ReadStatusReg(TPIDR_EL0);
  return v11;
}
```

解密函数是

```java
__int64 __fastcall decryptFlag(const unsigned __int8 *a1, char *a2, char a3, char a4)
{
  bool v5; // [xsp+Ch] [xbp-24h]
  int i; // [xsp+10h] [xbp-20h]
  unsigned __int8 v7; // [xsp+15h] [xbp-1Bh]

  v7 = a3 ^ a4 ^ 0x42;
  __android_log_print(4, "FibValidator", byte_17736, 66, (unsigned __int8)(a3 ^ a4), v7);
  for ( i = 0; ; ++i )
  {
    v5 = 0;
    if ( i < 31 )
      v5 = a1[i] != 0;
    if ( !v5 )
      break;
    a2[i] = a1[i] ^ v7;
  }
  a2[i] = 0;
  return __android_log_print(4, "FibValidator", byte_1712D, (unsigned int)i);
}
```

这里我们会想，这么多过程值, v16, v7，我们怎么获取呢？其实并不需要。仔细观察一下解密函数，其内部逻辑是对密文进行异或解密。
而v7是一个key，过程中会计算出来。但是，==我们知道flag的格式是`flag{}`，那么，我们可以直接通过第一个字节与`'f'`异或去得到key==
同时主程序没有对输入检测用的flag进行明显的提取操作（指提取括号中间内容），便可以大胆去猜测。
提取一下`byte_18C04`的数据，很快写出脚本

```Python
cip = bytearray.fromhex("EE E4 E9 EF F3 CC F1 E6 BC E5 B9 EB D7 C4 B8 BC EC BB FA D7 C5 BC FB FC BB FA F5")  
key = cip[0] ^ ord('f')  
for i in range(len(cip)):  
    cip[i] ^=  key  
print(cip)
```

得到flag：`flag{Dyn4m1c_L04d3r_M4st3r}`
PS：我猜，大部分==***ez***​==native题都是弄一堆花里胡哨的，真正加密部分极其简单，主要是为了让人心里害怕，导致不愿意思考。==也要大胆猜测并尝试去解密。==
题干所谓"静态分析极其困难"是个烟雾弹，误导人，吓唬人用的

### flower

#RE# #花指令#
~~w2最阴间的一个，你指望才学RE的新手去识别一堆花指令？？？~~ 
题干：让花开的到处都是的魔法
过程：题干告诉我们 ~~整个程序充斥着~~ 有很多花指令。。。
~~看官方WP，我不会~~

## week3

### Trap

#反调试#
~~这是week3最简单的一道~~
题干：小心脚下。
过程：主函数

```C
int __fastcall main(int argc, const char **argv, const char **envp)
{
  __int64 v3; // rdx
  __int64 v4; // r8
  char Buf1[112]; // [rsp+20h] [rbp-88h] BYREF

  memset(Buf1, 0, 0x64u);
  sub_140001470("Input the key to escape the trap:", argv, envp);
  sub_140001180("%s", Buf1);
  sub_1400014E0(Buf1);
  if ( !memcmp(Buf1, &unk_140005078, 0x20u) )
    sub_140001470("Congratulation!\n", v3, v4);
  else
    sub_140001470("Oooooops!\n", v3, v4);
  system("pause");
  return 0;
}
```

我们识别一下各个函数用途

```C
int __fastcall main(int argc, const char **argv, const char **envp)
{
  _BYTE Buf1[112]; // [rsp+20h] [rbp-88h] BYREF

  memset(Buf1, 0, 0x64u);
  printf("Input the key to escape the trap:");
  scanf("%s", Buf1);
  enc(Buf1);
  if ( !memcmp(Buf1, byte_7FF718EF5078, 32u) )
    printf("Congratulation!\n");
  else
    printf("Oooooops!\n");
  system("pause");
  return 0;
}
```

加密函数

```C
__int64 __fastcall enc(__int64 a1)
{
  __int64 result; // rax
  int i; // [rsp+0h] [rbp-38h]
  __int64 v3; // [rsp+8h] [rbp-30h]

  v3 = -1;
  do
    ++v3;
  while ( *(a1 + v3) );
  for ( i = 0; ; ++i )
  {
    result = v3;
    if ( i >= v3 )
      break;
    if ( i % 2 )
      *(a1 + i) ^= byte_7FF718EF5680[i % 11] + 1;
    else
      *(a1 + i) ^= byte_7FF718EF5680[i % 11];
    if ( i )
      *(a1 + i) ^= *(a1 + i - 1);
  }
  return result;
}
```

密钥不能直接看到，而且有反调试，调试时密钥会被修改。我们看一下xref，找到赋值的函数

```C
__int64 __fastcall sub_7FF718EF1200(__int64 a1)
{
  int i; // [rsp+20h] [rbp-38h]
  int j; // [rsp+24h] [rbp-34h]
  _BYTE v4[16]; // [rsp+28h] [rbp-30h] BYREF

  qmemcpy(v4, "1i4W", 4);
  v4[4] = 0x99;
  v4[5] = 0x35;
  v4[6] = 0x77;
  v4[7] = 0x11;
  qmemcpy(&v4[8], "6Rv", 3);
  if ( IsDebuggerPresent() )
  {
    for ( i = 0; i < 32; ++i )
      byte_7FF718EF5078[i] ^= i;
    for ( j = 0; j < 11; ++j )
      v4[j] ^= j + 1;
  }
  qmemcpy(byte_7FF718EF5680, v4, sizeof(byte_7FF718EF5680));
  return a1;
}
```

汇编看着更轻松一点（是为了直接获取密钥对应的ASCII码）

```asm
.text:00007FF718EF1215                 mov     [rsp+58h+var_20], rax
.text:00007FF718EF121A                 mov     [rsp+58h+var_30], 31h ; '1'
.text:00007FF718EF121F                 mov     [rsp+58h+var_2F], 69h ; 'i'
.text:00007FF718EF1224                 mov     [rsp+58h+var_2E], 34h ; '4'
.text:00007FF718EF1229                 mov     [rsp+58h+var_2D], 57h ; 'W'
.text:00007FF718EF122E                 mov     [rsp+58h+var_2C], 99h
.text:00007FF718EF1233                 mov     [rsp+58h+var_2B], 35h ; '5'
.text:00007FF718EF1238                 mov     [rsp+58h+var_2A], 77h ; 'w'
.text:00007FF718EF123D                 mov     [rsp+58h+var_29], 11h
.text:00007FF718EF1242                 mov     [rsp+58h+var_28], 36h ; '6'
.text:00007FF718EF1247                 mov     [rsp+58h+var_27], 52h ; 'R'
.text:00007FF718EF124C                 mov     [rsp+58h+var_26], 76h ; 'v'
```

我们再直接获取一下密文，对应解密脚本就能轻松写出

```Python
cip = bytearray.fromhex("57 51 04 3B D9 B6 F1 96 FF C4 F2 96 CC A6 B4 1B 4D 01 60 32 04 2C 5B 43 47 72 B4 B5 B0 96 D0 FE")  
  
v4 = bytearray.fromhex("31 69 34 57 99 35 77 11 36 52 76")  
for i in range(31,-1, -1):  
    if i!=0:  
        cip[i] = (cip[i] ^ cip[i-1]) & 0xFF  
    if i%2 !=0:  
        cip[i] = (cip[i] ^ (v4[i%11] + 1)) & 0xFF  
    else:  
        cip[i] = (cip[i] ^ v4[i%11]) & 0xFF  
print(cip)
```

得到flag：`flag{Y0u_h@V3_E5c4ped_Fr0m_7r4p}`

### babyre

#ollvm# #反混淆# #混淆# #控制流平坦化#
题干：简单的混淆
过程：~~主函数就露出了鸡脚~~
主函数这里就发现了明显混淆

```C
__int64 __fastcall main(int a1, char **a2, char **a3)
{
  int v3; // edx
  int v4; // eax
  int v6; // [rsp+Ch] [rbp-E4h]
  char s[112]; // [rsp+10h] [rbp-E0h] BYREF
  char dest[108]; // [rsp+80h] [rbp-70h] BYREF
  int v9; // [rsp+ECh] [rbp-4h]

  v9 = 0;
  memcpy(dest, &unk_402080, 0x64u);
  v6 = 1187407642;
  while ( 1 )
  {
    while ( v6 == 156068477 )
      v6 = 520062110;
    if ( v6 == 187656653 )
      break;
    switch ( v6 )
    {
      case 300196589:
        v6 = 1580249157;
        break;
      case 355303831:
        printf("Fail!\n");
        v6 = 156068477;
        break;
      case 520062110:
        v6 = 187656653;
        break;
      case 1187407642:
        v6 = 1912110999;
        break;
      case 1580249157:
        v6 = 187656653;
        break;
      case 1912110999:
        memset(s, 0, 0x64u);
        __isoc99_scanf(&unk_4020E4, s);
        sub_401650(s);
        v3 = strcmp(s, dest);
        v4 = 355303831;
        if ( !v3 )
          v4 = 1998933451;
        v6 = v4;
        break;
      default:
        printf("Success!\n");
        v6 = 300196589;
        break;
    }
  }
  return 0;
}
```

剩下加密部分不放了，一大坨。我们搜寻得知这是ollvm的控制流平坦化混淆，我们能在[这里](https://xz.aliyun.com/news/17017)找到怎么去除的方法。
按照这上面说的方法，以及参考反混淆脚本的[github仓库](https://github.com/cq674350529/deflat)里的脚本使用方法，把里面的控制流混淆都给去掉。
去掉后主函数终于是人能看的了

```C
__int64 __fastcall main(int a1, char **a2, char **a3)
{
  char s[112]; // [rsp+10h] [rbp-E0h] BYREF
  char dest[108]; // [rsp+80h] [rbp-70h] BYREF
  int v6; // [rsp+ECh] [rbp-4h]

  v6 = 0;
  memcpy(dest, &unk_402080, 0x64u);
  memset(s, 0, 0x64u);
  __isoc99_scanf(&unk_4020E4, s);
  sub_401650(s);
  if ( !strcmp(s, dest) )
    printf("Success!\n");
  else
    printf("Fail!\n");
  return 0;
}
```

加密函数sub_401650()

```C
__int64 __fastcall sub_401650(char *a1)
{
  size_t v1; // rax
  size_t v2; // rsi
  __int64 result; // rax
  _QWORD v4[2]; // [rsp+0h] [rbp-60h] BYREF
  int i; // [rsp+10h] [rbp-50h]
  int v6; // [rsp+14h] [rbp-4Ch]
  int v7; // [rsp+18h] [rbp-48h]
  int v8; // [rsp+1Ch] [rbp-44h]
  char *s; // [rsp+20h] [rbp-40h]
  _DWORD *v10; // [rsp+28h] [rbp-38h]
  _DWORD *v11; // [rsp+30h] [rbp-30h]
  size_t v12; // [rsp+38h] [rbp-28h]
  __int64 v13; // [rsp+40h] [rbp-20h]
  int v14; // [rsp+4Ch] [rbp-14h]
  size_t v15; // [rsp+50h] [rbp-10h]
  int v16; // [rsp+58h] [rbp-8h]
  int v17; // [rsp+5Ch] [rbp-4h]

  v4[1] = a1;
  v10 = &v4[-2];
  v11 = &v4[-2];
  s = a1;
  sub_401180();
  v6 = 0;
  v7 = 0;
  v8 = 0;
  for ( i = 1979836470; ; i = 1979836470 )
  {
    v12 = v8;
    i = 1754662794;
    v1 = strlen(s);
    if ( v12 >= v1 )
      break;
    v7 = (v7 + 1) % 256;
    v6 = ((unsigned __int8)byte_404070[v7] + v6) % 256;
    *v11 = (unsigned __int8)byte_404070[v7];
    v13 = v6;
    i = 237542168;
    byte_404070[v7] = byte_404070[v6];
    byte_404070[v6] = *v11;
    *v10 = (unsigned __int8)byte_404070[((unsigned __int8)byte_404070[v6] + (unsigned __int8)byte_404070[v7]) % 256];
    s[v8] ^= *v10;
    v14 = ++v8;
  }
  v8 = 0;
  for ( i = 837974569; ; i = 837974569 )
  {
    v15 = v8;
    i = 818217443;
    v2 = strlen(s);
    result = 746392057;
    if ( v15 >= v2 )
      break;
    v16 = (unsigned __int8)(8 * s[v8]);
    i = 2062792640;
    s[v8] = ((int)(unsigned __int8)s[v8] >> 5) | v16;
    v17 = ++v8;
  }
  return result;
}
```

里面的sub_401180()

```C
__int64 sub_401180()
{
  __int64 result; // rax
  size_t v1; // rax
  _QWORD v2[2]; // [rsp+0h] [rbp-50h] BYREF
  int i; // [rsp+10h] [rbp-40h]
  unsigned int v4; // [rsp+14h] [rbp-3Ch]
  int v5; // [rsp+18h] [rbp-38h]
  unsigned int v6; // [rsp+1Ch] [rbp-34h]
  _DWORD *v7; // [rsp+20h] [rbp-30h]
  void *dest; // [rsp+28h] [rbp-28h]
  unsigned int v9; // [rsp+30h] [rbp-20h]
  char v10; // [rsp+37h] [rbp-19h]
  unsigned int v11; // [rsp+38h] [rbp-18h]
  unsigned int v12; // [rsp+3Ch] [rbp-14h]
  unsigned int v13; // [rsp+40h] [rbp-10h]
  int v14; // [rsp+44h] [rbp-Ch]
  char v15; // [rsp+4Bh] [rbp-5h]
  unsigned int v16; // [rsp+4Ch] [rbp-4h]

  v7 = &v2[-2];
  dest = &v2[-14];
  memcpy(&v2[-14], &unk_402010, 0x64u);
  v6 = 0;
  for ( i = 667366193; ; i = 667366193 )
  {
    v9 = v6;
    i = 193224699;
    if ( v6 >= 0x100 )
      break;
    v10 = v6;
    v11 = v6;
    byte_404070[v6] = v6;
    v12 = ++v6;
  }
  v4 = 0;
  v5 = 0;
  v6 = 0;
  for ( i = 1373896932; ; i = 1373896932 )
  {
    v13 = v6;
    i = 1678250905;
    result = 1815621806;
    if ( v6 >= 0x100 )
      break;
    *v7 = (unsigned __int8)byte_404070[v6];
    v14 = *v7 + v5;
    v15 = *((_BYTE *)dest + v4);
    i = 747178249;
    v5 = (unsigned __int8)(v15 + v14);
    byte_404070[v6] = byte_404070[(unsigned __int8)(v15 + v14)];
    byte_404070[v5] = *v7;
    v2[0] = ++v4;
    v1 = strlen((const char *)dest);
    if ( v2[0] >= v1 )
    {
      v4 = 0;
      i = 1483442800;
    }
    v16 = ++v6;
  }
  return result;
}
```

还是不太好看。我们观察一下加密函数，有明显的`%256`，猜测这是RC4。
我们找一下key(unk_402010)的字节数据`12 34 56 78 90 AB CD EF`，
密文(unk_402080)的数据`C6 AC EE 8B 57 04 64 3A A7 3B 84 67 AC D7 8E D8 1D 03 85 55 F6 51 90`（记得把这两个都定义为数组，方便提取数据）
找个RC4的脚本，但套进去发现不对。再看看，RC4加密完后，里面多了个密文循环移位的操作，我们加上这个处理一下。
我解析一下位移操作

```C
v8 = 0
for ( i = 0x31F27A29; ; i = 0x31F27A29 )
{
    v15 = v8;
    i = 0x30C501E3;
    v2 = strlen(s);
    result = 0x2C7D09F9;
    if ( v15 >= v2 )
      break;
    v16 = (8 * s[v8]);//相当于s[v8] << 3 (2^3=8)
    i = 0x7AF3B7C0;
    s[v8] = (s[v8] >> 5) | v16;
	v17 = ++v8;
}
```

手动重构一下，好看很多。这里v15实际上才是真正的i，最后v17没什么用，v8进行了自增

```C
int len = strlen(s);
for ( int i = 0; i < len; i++)
{
	v16 = s[v8] << 3;
	s[v8] = (s[v8] >> 5) | v16;//合并左边和右边的内容
}
```

最终解密脚本

```Python
def ror3(x):  
    return ((x >> 3) | ((x & 0x07) << 5)) & 0xFF  
  
def ksa(key):  
    s = list(range(256))  
    j = 0  
    key_len = len(key)  
    for i in range(256):  
        j = (key[i % key_len] + s[i] + j) % 256  
        s[i], s[j] = s[j], s[i]  
    return s  
  
  
def decrypt(ciphertext, key):  
    shifted = [ror3(b) for b in ciphertext]  
    s_box = ksa(key)  
  
    i = j = 0  
    plaintext = []  
    for idx in range(len(shifted)):  
        i = (i + 1) % 256  
        j = (j + s_box[i]) % 256  
        s_box[i], s_box[j] = s_box[j], s_box[i]  
        k = s_box[(s_box[i] + s_box[j]) % 256]  
        plaintext.append(shifted[idx] ^ k)  
    return bytes(plaintext)  
  
cipher_hex = "C6 AC EE 8B 57 04 64 3A A7 3B 84 67 AC D7 8E D8 1D 03 85 55 F6 51 90"  
cipher_bytes = bytes.fromhex(cipher_hex)  
  
key = bytes.fromhex("12 34 56 78 90 AB CD EF")  
  
plaintext = decrypt(cipher_bytes, key)  
print(plaintext)
```

### BrokenData

#AES# #md5#
题干：似乎丢失了什么数据。
过程：题目提示flag有问题。先看看主函数

```C
int __fastcall main(int argc, const char **argv, const char **envp)
{
  __int64 v3; // rdx
  __int64 v4; // r8
  __int64 v6; // rdx
  __int64 v7; // r8
  __int64 v8; // [rsp+20h] [rbp-1B8h]
  char v9[24]; // [rsp+30h] [rbp-1A8h] BYREF
  _BYTE v10[40]; // [rsp+48h] [rbp-190h] BYREF
  char v11[112]; // [rsp+70h] [rbp-168h] BYREF
  _BYTE Buf2[208]; // [rsp+E0h] [rbp-F8h] BYREF

  strcpy(v9, "%es_&ou_fou**_ke%");
  memset(v10, 0, 0x20u);
  memset(v11, 0, 0x64u);
  sub_140001D10("Input the flag:", argv, envp);
  sub_140001DE0("%s", v11);
  v8 = -1;
  do
    ++v8;
  while ( v11[v8] );
  if ( v8 == 56 )
  {
    sub_1400022A0(v9, 17, v10);
    v9[0] = v11[10];
    v9[4] = v11[10];
    v9[16] = v11[10];
    sub_1400022A0(v9, 17, &v10[16]);
    memset(Buf2, 0, sizeof(Buf2));
    sub_140001E60(v11, v10, Buf2);
    if ( !memcmp(&unk_1400061C0, Buf2, 0xD0u) )
      sub_140001D10("Correct.\n", v6, v7);
    else
      sub_140001D10("Wrong.\n", v6, v7);
    system("pause");
    return 0;
  }
  else
  {
    sub_140001D10("Wrong length.\n", v3, v4);
    system("pause");
    return 0;
  }
}
```

我们稍微优化识别一下各个函数

```C
int __fastcall main(int argc, const char **argv, const char **envp)
{
  __int64 len_of_input; // [rsp+20h] [rbp-1B8h]
  char key[24]; // [rsp+30h] [rbp-1A8h] BYREF
  char _Key[40]; // [rsp+48h] [rbp-190h] BYREF
  char input[112]; // [rsp+70h] [rbp-168h] BYREF
  _BYTE string208[208]; // [rsp+E0h] [rbp-F8h] BYREF

  strcpy(key, "%es_&ou_fou**_ke%");
  memset(_Key, 0, 32u);
  memset(input, 0, 100u);
  printf("Input the flag:");
  scanf("%s", input);
  len_of_input = -1;
  do
    ++len_of_input;
  while ( input[len_of_input] );
  if ( len_of_input == 56 )
  {
    md5(key, 17u, _Key);
    key[0] = input[10];
    key[4] = input[10];
    key[16] = input[10];
    md5(key, 17u, &_Key[16]);
    memset(string208, 0, sizeof(string208));
    div2_13Group(input, _Key, string208); 
    if ( !memcmp(encflag, string208, 208u) )
      printf("Correct.\n");
    else
      printf("Wrong.\n");
    system("pause");
    return 0;
  }
  else
  {
    printf("Wrong length.\n");
    system("pause");
    return 0;
  }
}
```

我们发现代码逻辑中回去修补密钥。为了语义通顺，替换的字符只能是"Y"或"y"。我们看一下分组部分

```C
void __fastcall div2_13Group(__int64 inp, __int64 _Key, __int64 strings_1)
{
  char v3; // al
  int j; // [rsp+30h] [rbp-48h]
  int i; // [rsp+34h] [rbp-44h]
  int v6; // [rsp+38h] [rbp-40h]
  int v7; // [rsp+3Ch] [rbp-3Ch]
  _BYTE v8[16]; // [rsp+40h] [rbp-38h] BYREF

  v6 = 0;
  for ( i = 0; i < 13; ++i )
  {
    v7 = dword_7FF670B96180[i];
    memset(v8, 0, sizeof(v8));
    for ( j = 0; j < 16; ++j )
    {
      if ( j >= v7 )
        v3 = *(inp + 5);
      else
        v3 = *(inp + j + v6);
      v8[j] = v3;
    }
    if ( i % 2 )
      AES(_Key, 16u, v8, v8, 16u);
    else
      AES((_Key + 16), 16u, v8, v8, 16u);
    qmemcpy((strings_1 + 16 * i), v8, 0x10u);
    v6 += v7;
  }
}
```

分奇偶块进行加密，分别使用了密钥的前16个字节和后面16个字节的，整个密钥我们通过动态调试获取。我们分别尝试用y和Y去作为替换字符，获取生成密钥。
y:`A06A8E86423C9E2723527BDA518D9727 766935490BCDF949ECFFD658C1EFCBE9`
Y:`A06A8E86423C9E2723527BDA518D9727 4D71796780A1F124278C8B7C2BB17D84`
我们写一下解密脚本，加密部分用到是标准AES算法，不过是分奇偶块进行加密的。经过尝试，大写Y为正确的替换字符，用对应的密钥。

```Python
from Crypto.Cipher import AES  
  
password1 = bytes.fromhex("A06A8E86423C9E2723527BDA518D9727")  
password2 = bytes.fromhex("4D71796780A1F124278C8B7C2BB17D84")  
  
hex_string = "816DF2DA3B09CF85BC3FDC25ED4552AF3ADD51696B5E972EF6F42B3FC901BCFBD1A5588E1FE6B836A71EA19D3E42526C91FDDEDC1BD2883C894819572F30033D02328EFC3C5B8A47CAB43ED38E055DCD8DE7FC29CABADE9E6C3B96920B547FC6714D2A828F420C542C84F867347EC877E11B7FDB8B879701016314054DBFCC10524987BC5E16EA0F7DF5758D05B043F257FD7E40FEB45CA8C0638B0880E62B4D41777107FC71FE2177078BE27D9EB604640DBDE159105FA78008AB963C34C6BB810DE4FA539922C1C9A7E9397510DAA4"  
en_text = bytes.fromhex(hex_string)  
  
total_blocks = len(en_text) // 16  
decrypted_blocks = []  
for i in range(total_blocks):  
    start = i * 16  
    end = start + 16  
    block = en_text[start:end]  
  
    if i % 2 == 1:
        cipher = AES.new(password1, AES.MODE_ECB)  
    else:
        cipher = AES.new(password2, AES.MODE_ECB)  
  
    decrypted_block = cipher.decrypt(block)  
    decrypted_blocks.append(decrypted_block)  
    print(f"Block {i:2d}: {decrypted_block}")  
  
full_text = b''.join(decrypted_blocks)  
print(full_text.decode('utf-8', errors='replace'))  
```

我们执行后获取的东西是`fWWWWWWWWWWWWWWWlag{WWWWWWWWWWWWWow!_YWWWWWWWWWWou_restorWWWWWWWed_the_dataWWWWW_and_tWWWWWWWWWWhisWWWWWWWWWWWWW_iWWWWWWWWWWWWWWs_yWWWWWWWWWWWWWoWWWWWWWWWWWWWWWur_rewaWWWWWWWWWrWWWWWWWWWWWWWWWd}WWWWWWWWWWWWWW`
去除多余的W获得flag：`flag{Wow!_You_restored_the_data_and_this_is_your_reward}`

### Fisher

#hook#
题干：这里有个渔夫想要用金钱购买你的鱼饵，你有足够好的鱼饵售卖吗。
过程：我对以下函数进行了重命名

```C

```

加密部分

```C
__int64 __fastcall subb_TEA(unsigned int *a1, _DWORD *a2)
{
  __int64 result; // rax
  unsigned int v3; // [rsp+0h] [rbp-28h]
  unsigned int v4; // [rsp+4h] [rbp-24h]
  int v5; // [rsp+8h] [rbp-20h]
  unsigned int i; // [rsp+Ch] [rbp-1Ch]

  v3 = *a1;
  v4 = a1[1];
  v5 = 0;
  for ( i = 0; i < 0x20; ++i ) // 32
  {
    v5 -= 0x6D6DD6D7;
    v3 += (a2[1] + (v4 >> 5)) ^ (v5 + v4) ^ (*a2 + 16 * v4);
    v4 += (a2[3] + (v3 >> 5)) ^ (v5 + v3) ^ (a2[2] + 16 * v3);
  }
  *a1 = v3;
  result = 4;
  a1[1] = v4;
  return result;
}
```

解密脚本

```Python

```

### wtf

#javascript#

```Python

```

### ezmath

#z3#

```C
int __fastcall main(int argc, const char **argv, const char **envp)
{
  char v4; // [rsp+68h] [rbp-58h]
  char v5; // [rsp+69h] [rbp-57h]
  char v6; // [rsp+6Ah] [rbp-56h]
  char v7; // [rsp+6Bh] [rbp-55h]
  char v8; // [rsp+6Ch] [rbp-54h]
  char v9; // [rsp+6Dh] [rbp-53h]
  char v10; // [rsp+6Fh] [rbp-51h]
  char v11; // [rsp+70h] [rbp-50h]
  char v12; // [rsp+71h] [rbp-4Fh]
  char v13; // [rsp+72h] [rbp-4Eh]
  char v14; // [rsp+73h] [rbp-4Dh]
  char v15; // [rsp+74h] [rbp-4Ch]
  char v16; // [rsp+75h] [rbp-4Bh]
  char v17; // [rsp+76h] [rbp-4Ah]
  char v18; // [rsp+77h] [rbp-49h]
  char v19; // [rsp+78h] [rbp-48h]
  char v20; // [rsp+79h] [rbp-47h]
  char v21; // [rsp+7Ah] [rbp-46h]
  char v22; // [rsp+7Bh] [rbp-45h]
  char v23; // [rsp+7Ch] [rbp-44h]
  char v24; // [rsp+7Dh] [rbp-43h]
  char v25; // [rsp+7Eh] [rbp-42h]
  char v26; // [rsp+80h] [rbp-40h]
  char v27; // [rsp+81h] [rbp-3Fh]
  char v28; // [rsp+82h] [rbp-3Eh]
  char v29; // [rsp+83h] [rbp-3Dh]
  char v30; // [rsp+84h] [rbp-3Ch]
  char v31; // [rsp+85h] [rbp-3Bh]
  char v32; // [rsp+86h] [rbp-3Ah]
  char v33; // [rsp+87h] [rbp-39h]
  char v34; // [rsp+88h] [rbp-38h]
  char v35; // [rsp+89h] [rbp-37h]
  char v36; // [rsp+8Ah] [rbp-36h]
  char v37; // [rsp+8Bh] [rbp-35h]
  char v38; // [rsp+8Ch] [rbp-34h]
  char v39; // [rsp+8Dh] [rbp-33h]
  char v40; // [rsp+8Eh] [rbp-32h]
  char v41; // [rsp+8Fh] [rbp-31h]
  char v42; // [rsp+90h] [rbp-30h]
  char v43; // [rsp+91h] [rbp-2Fh]
  char v44; // [rsp+92h] [rbp-2Eh]
  char v45; // [rsp+93h] [rbp-2Dh]
  char v46; // [rsp+94h] [rbp-2Ch]
  char v47; // [rsp+95h] [rbp-2Bh]
  char v48; // [rsp+96h] [rbp-2Ah]
  char v49; // [rsp+97h] [rbp-29h]
  char v50; // [rsp+98h] [rbp-28h]
  char v51; // [rsp+99h] [rbp-27h]
  char v52; // [rsp+9Ah] [rbp-26h]
  char v53; // [rsp+9Bh] [rbp-25h]
  char v54; // [rsp+9Ch] [rbp-24h]
  char v55; // [rsp+9Dh] [rbp-23h]
  char v56; // [rsp+9Eh] [rbp-22h]
  char v57; // [rsp+9Fh] [rbp-21h]
  char v58; // [rsp+A0h] [rbp-20h]
  char v59; // [rsp+A1h] [rbp-1Fh]
  char v60; // [rsp+A2h] [rbp-1Eh]
  char v61; // [rsp+A3h] [rbp-1Dh]
  char v62; // [rsp+A4h] [rbp-1Ch]
  char v63; // [rsp+A5h] [rbp-1Bh]
  char v64; // [rsp+A6h] [rbp-1Ah]
  char v65; // [rsp+A7h] [rbp-19h]
  char v66; // [rsp+A8h] [rbp-18h]
  char v67; // [rsp+A9h] [rbp-17h]
  char v68; // [rsp+AAh] [rbp-16h]
  char v69; // [rsp+ABh] [rbp-15h]
  char v70; // [rsp+ACh] [rbp-14h]
  char v71; // [rsp+ADh] [rbp-13h]
  char v72; // [rsp+AEh] [rbp-12h]
  char v73; // [rsp+AFh] [rbp-11h]
  char v74; // [rsp+B0h] [rbp-10h]
  char v75; // [rsp+B1h] [rbp-Fh]
  char v76; // [rsp+B2h] [rbp-Eh]
  char v77; // [rsp+B3h] [rbp-Dh]
  char v78; // [rsp+B4h] [rbp-Ch]
  char v79; // [rsp+B5h] [rbp-Bh]
  char v80; // [rsp+B6h] [rbp-Ah]
  char *s; // [rsp+B8h] [rbp-8h]

  if ( argc == 2 )
  {
    s = (char *)argv[1];
    if ( (unsigned int)strlen(s) == 80 )
    {
      v80 = s[1];
      v79 = s[2];
      v78 = s[3];
      v77 = s[4];
      v76 = s[5];
      v75 = s[6];
      v74 = s[7];
      v73 = s[8];
      v72 = s[9];
      v71 = s[10];
      v70 = s[11];
      v69 = s[12];
      v68 = s[13];
      v67 = s[14];
      v66 = s[15];
      v65 = s[16];
      v64 = s[17];
      v63 = s[18];
      v62 = s[19];
      v61 = s[20];
      v60 = s[21];
      v59 = s[22];
      v58 = s[23];
      v57 = s[24];
      v56 = s[25];
      v55 = s[26];
      v54 = s[27];
      v53 = s[28];
      v52 = s[29];
      v51 = s[30];
      v50 = s[31];
      v49 = s[32];
      v48 = s[33];
      v47 = s[34];
      v46 = s[35];
      v45 = s[36];
      v44 = s[37];
      v43 = s[38];
      v42 = s[39];
      v41 = s[40];
      v40 = s[41];
      v39 = s[42];
      v38 = s[43];
      v37 = s[44];
      v36 = s[45];
      v35 = s[46];
      v34 = s[47];
      v33 = s[48];
      v32 = s[49];
      v31 = s[50];
      v30 = s[51];
      v29 = s[52];
      v28 = s[53];
      v27 = s[54];
      v26 = s[55];
      v25 = s[57];
      v24 = s[58];
      v23 = s[59];
      v22 = s[60];
      v21 = s[61];
      v20 = s[62];
      v19 = s[63];
      v18 = s[64];
      v17 = s[65];
      v16 = s[66];
      v15 = s[67];
      v14 = s[68];
      v13 = s[69];
      v12 = s[70];
      v11 = s[71];
      v10 = s[72];
      v9 = s[74];
      v8 = s[75];
      v7 = s[76];
      v6 = s[77];
      v5 = s[78];
      v4 = s[79];
      if ( 63 * v9 == 0x81
        && 111 * *s - 67 * v55 - 2 * v9 == 0xA1
        && -85 * v9 + 14 * v55 + 85 * v6 == 0xFA
        && 61 * v55 - 45 * v37 + 61 * v6 - 11 * v9 == 0xF2
        && -2 * v9 + 57 * v37 + 43 * v60 + 4 * v55 + 48 * v6 == 48
        && -94 * v60 + 11 * v77 + 2 * v37 + 123 * v55 + -77 * v9 + 55 * v22 + 29 * v6 == 118
        && -21 * v9 + 23 * v22 + 26 * v37 + 113 * v46 + 38 * v55 - 16 * v60 + 3 * v6 == 0xFB
        && -13 * v22 - 7 * v37 + 23 * v46 + 115 * v55 + -105 * v60 - 115 * v59 + -48 * v6 - 122 * v9 == 0xFD
        && 37 * v60 + 95 * v59 + -25 * v36 + 2 * (5 * v46 + 36 * v37 + v55) + 94 * v9 - 116 * v22 - 77 * v6 == 0xFC
        && 105 * v36 - 38 * v37 + -75 * v42 - 27 * v55 + -39 * v60 + 4 * v59 + -83 * v9 + 98 * v22 + 58 * v6 == 88
        && -101 * v37 - 31 * v41 + -59 * v42 - 86 * v46 + 118 * v60 + 66 * v59 + -124 * v9 - 48 * v22 + 75 * v6 == 38
        && 9 * v32 - 62 * v36 + 18 * v37 + 63 * v41 + 38 * v42 - 119 * v55 + 106 * v71 - 125 * v60 + -19 * v9 - 66 * v22 == 0xAF
        && -49 * v32 - 101 * v36 + -75 * v55 + 77 * v46 + -99 * v37 - 60 * v41 + -19 * v9 + 43 * v21 - 97 * v6 == 0xAC
        && 38 * v9 - 13 * v21 + 50 * v42 + 63 * v55 + -89 * v37 - 119 * v41 + 91 * v25 - 4 * v36 - 124 * v6 == 0x92
        && 85 * v21 - 52 * v36 + 66 * v37 - 5 * v42 + -68 * v60 + 106 * v59 - v54 + -89 * v6 + 113 * v9 == 0xC6
        && 85 * v19 - 74 * v36 + -37 * v41 - 59 * v42 + 78 * v46 - 41 * v54 + 43 * v59 - 47 * v55 - 32 * v9 == 68
        && 100 * v22 + 123 * v32 + 65 * v74 + 55 * v59 + 90 * v54 + 24 * v55 + 124 * v9 - 121 * v19 - 41 * v6 == 0xAA
        && 45 * v35 - 17 * v37 + 112 * v41 - 98 * v46 + 35 * v55 - 103 * v59 + v74 + 24 * v6 + 13 * v9 == 0xBD
        && 51 * v22 + 8 * v25 + -93 * v74 - 106 * v60 + -97 * v49 + 62 * v59 + -107 * v37 + 21 * v42 + 27 * v21 == 106
        && -67 * v49 + 8 * v54 + 92 * v74 + 59 * v55 + 93 * v41 - 113 * v42 + -108 * v22 - 8 * v25 - 115 * v13 == 15
        && -18 * v41 - 58 * v42 + 29 * v61 - 108 * v55 + 35 * v49 - 8 * v54 + 100 * v25 - 37 * v35 - 21 * v19 == 34
        && -54 * v37 - 72 * v46 + 11 * v54 + 10 * v60 + 21 * v80 + 110 * v74 + 21 * v21 + 63 * v22 - 76 * v19 == 0x92
        && 120 * v13 - 111 * v32 + 6 * v37 - 105 * v41 + 45 * v76 - 66 * v55 + -68 * v49 + 24 * v54 + 15 * v9 == 0xAC
        && 97 * v49 + 9 * v54 + -50 * v80 + 81 * v61 + -33 * v37 - 5 * v41 + -117 * v31 - 55 * v35 + 117 * v9 == 11
        && -62 * v25 - 61 * v36 + -30 * v46 - 78 * v49 + 43 * v61 + 63 * v74 + -83 * v9 + 97 * v10 + 39 * v6 == 15
        && 85 * v32
         + 115 * v37
         + -115 * v41
         + 119 * v46
         + 23 * v57
         + 16 * v54
         + 59 * v10
         + 32 * v31
         + -123 * v8
         - 120 * v9 == 0xF0
        && 121 * v23 + 117 * v25 + -118 * v36 - 99 * v46 + -87 * v55 + 113 * v49 + -42 * v10 + 109 * v21 - 74 * v6 == 0xC2
        && -51 * v25 - 95 * v31 + -45 * v41 - 38 * v46 + -25 * v74 + 117 * v59 + 61 * v19 - 52 * v22 - 83 * v7 == 82
        && 45 * v10 - 44 * v13 + -56 * v32 + 95 * v50 + -116 * v80 - 72 * v76 + 95 * v7 - 86 * v8 - 85 * v6 == 0xF5
        && -52 * v35 + 119 * v47 + 89 * v49 - 25 * v50 + 118 * v74 - 14 * v76 + 63 * v31 - 112 * v32 + 47 * v23 == 0x8C
        && -27 * v22 - 23 * v30 + -19 * v42 - 29 * v61 + 125 * v76 - 84 * v74 + -9 * v19 - 58 * v21 - 11 * v13 == 7
        && -87 * v41
         + 3 * v46
         + -3 * v79
         + 38 * v74
         + 100 * v51
         + 39 * v61
         + 105 * v31
         + 73 * v35
         + -20 * v6
         + 119 * v13 == 0x89
        && 91 * v35 - 97 * v46 + -45 * v47 - 125 * v59 + 71 * v76 + 62 * v74 + -71 * v9 - 37 * v18 + 100 * v8 == 49
        && -99 * v19 - 117 * v30 + -118 * v41 - 81 * v43 + -72 * v60 - 82 * v59 + -108 * v13 + 46 * v18 + 62 * v7 == 20
        && -69 * v22 - 33 * v25 + -86 * v35 - 104 * v41 + 5 * v43 + 91 * v70 + 87 * v18 + 55 * v19 - 36 * v9 == 0xAD
        && -113 * v10 - 13 * v15 + 98 * v19 - 26 * v31 + 84 * v32 + 71 * v36 + -86 * v79 - 93 * v47 - 124 * v7 == 0xDF
        && 116 * v25 + 126 * v47 + 63 * v54 + 104 * v59 + -113 * v80 - 49 * v79 + 61 * v65 + 11 * v70 + 9 * v6 == 125
        && -55 * v70 + 39 * v66 + -61 * v36 + 4 * (55 * v43 + 20 * v49 + 29 * v46 + v47) + -2 * v13 + 127 * v30 == 68
        && -51 * v10 + 79 * v22 + 42 * v23 + 60 * v32 + 25 * v52 + 2 * v59 + -12 * v61 - 80 * v76 - 6 * v8 == 0xF6
        && 113 * v8 - 109 * v16 + 13 * v52 + 4 * v49 + 68 * v31 - 84 * v42 + v25 + -53 * v6 + 42 * v7 == 104
        && -122 * v49 + 85 * v50 + 49 * v55 - 17 * v61 + -48 * v79 - 4 * v74 + -50 * v65 - 16 * v70 - 9 * v12 == 65
        && -100 * v46 - 83 * v50 + -20 * v54 - 58 * v59 + -95 * v61 + 96 * v65 + -93 * v33 - 61 * v36 + 97 * v21 == 0x97
        && 71 * v39
         - 89 * v40
         + 100 * v41
         + 95 * v43
         + -67 * v49
         - 49 * v50
         + 82 * v66
         - 107 * v55
         + 122 * v7
         - 38 * v23 == 0xF4
        && -125 * v69 - 7 * v49 + 60 * v43 - 98 * v47 + 16 * v23 + 109 * v42 + -12 * v18 + 108 * v22 + 126 * v12 == 0xEC
        && 114 * v21 - 25 * v22 + 38 * v30 - 20 * v52 + -104 * v80 + 59 * v64 + 11 * v8 + 46 * v19 - 27 * v7 == 0x9C
        && -39 * v16 - 33 * v30 + -92 * v31 + 46 * v33 + 91 * v69 - 26 * v41 + 65 * v11 + 68 * v12 - 29 * v10 == 0xB2
        && 74 * v80 - 71 * v76 + -13 * v61 - 105 * v66 + -49 * v47 - 30 * v59 + 115 * v29 + 84 * v46 - 51 * v16 == 4
        && -66 * v33 - 49 * v43 + 81 * v60 - 91 * v69 + -123 * v73 - 61 * v70 + 61 * v21 + 77 * v32 + 115 * v9 == 0x92
        && 77 * v60
         - 116 * v73
         + 2 * v52
         - 41 * v53
         + -110 * v39
         + 8 * v46
         + -115 * v22
         - 48 * v34
         + -59 * v10
         - 108 * v12 == 12
        && -13 * v25 - 46 * v50 + 99 * v55 - 27 * v61 + 43 * v65 + 53 * v68 + 88 * v74 - 21 * v70 + 5 * v12 == 77
        && 36 * v53 + 41 * v54 + 57 * v62 - 31 * v65 + 14 * v80 + 85 * v76 + 41 * v33 - 78 * v39 - 88 * v9 == 122
        && 55 * v59 + 124 * v54 + 9 * v37 + 66 * v46 + -127 * v20 + 61 * v30 + -100 * v9 + 114 * v10 + 109 * v8 == 0xE4
        && -111 * v9 + 31 * v17 + -24 * v65 - 38 * v49 + v41 + 62 * v22 - 57 * v33 + -98 * v6 - 48 * v7 == 0xC0
        && 44 * v19 + 126 * v37 + 81 * v45 - 53 * v53 + -118 * v80 - 43 * v54 + -89 * v11 - 31 * v12 + 111 * v7 == 0x95
        && -90 * v22 - 49 * v28 + -45 * v80 + 97 * v73 + 96 * v45 - 20 * v49 + -50 * v39 - 9 * v44 + 78 * v11 + 43 * v21 == 0xB7
        && -18 * v13 - 99 * v22 + 65 * v24 - 119 * v29 + 36 * v30 - 42 * v35 + -107 * v65 - 91 * v62 + 3 * v10 == 0xDE
        && 27 * v23 - 110 * v24 + -93 * v78 + 62 * v73 + 28 * v42 - 60 * v46 + -108 * v15 - 109 * v20 + 53 * v11 == 72
        && -121 * s[56] + 61 * v53 + -4 * v62 - 10 * v73 + -109 * v74 + 96 * v79 + 2 * (103 * v6 + 121 * v21 + v16) == 119
        && -126 * v39 + 107 * v46 + 49 * v54 - 91 * v58 + 27 * v68 - 53 * v74 + 37 * v80 + 123 * v76 + 7 * v16 == 0xD7
        && 83 * v42 + 113 * v43 + -7 * v68 - 92 * v65 + 83 * v59 - 124 * v60 + -105 * v27 + 44 * v32 - 106 * v23 == 35
        && 17 * v72 + 46 * v68 + 102 * v47 - 15 * v59 + (v37 << 7) + 91 * v44 + -119 * v15 + 27 * v16 - 53 * v9 == 0x80
        && -6 * v45 + 92 * v46 + 73 * v76 - 101 * v64 + -36 * v53 + 33 * v58 + -115 * v28 - 122 * v35 - 108 * v16 == 0xDF
        && -23 * v26 - 100 * v29 + 86 * v36 - 29 * v43 + 3 * v59 - 88 * v70 + 95 * v18 - 99 * v24 + 82 * v15 == 83
        && 27 * v78 + 38 * v79 + -26 * v70 + 58 * v76 + -84 * v54 + 7 * v64 + -35 * v21 - 43 * v35 + 45 * v4 == 43
        && 53 * v43 + 27 * v59 + 120 * v69 - 2 * v70 + -72 * v80 - 59 * v75 + -114 * v13 - 8 * v16 - 86 * v11 == 43
        && -7 * v67 + 75 * v55 + 86 * v52 - 6 * v53 + -3 * v30 - 38 * v35 + -122 * v7 + 98 * v17 + 92 * v4 == 0xB7
        && -73 * v63 - 117 * v61 + 78 * v32 - 32 * v58 + -47 * v6 + 2 * (58 * v7 + 78 * v10 + 61 * v22 + v16) == 118
        && 2 * v11 + 57 * v14 + 51 * v55 - 41 * v72 + 8 * v36 - 113 * v50 + 67 * v16 - 35 * v19 + 55 * v10 == 121
        && 13 * v45 + 42 * v46 + -83 * v70 - 21 * v62 + -117 * v56 + 73 * v59 + 11 * v16 + 105 * v44 - 37 * v13 == 0xD9
        && -122 * v42 + 122 * v46 + -79 * v48 - 110 * v55 + -110 * v79 - 119 * v75 + 66 * v21 - 123 * v29 - 104 * v13 == 0xE2
        && 37 * v42 - 63 * v50 + -68 * v58 - 23 * v61 + -12 * v65 - 3 * v71 + -77 * v14 - 99 * v30 + v11 == 0xE9
        && 15 * v20 + 74 * v26 + 29 * v31 - 123 * v58 + 13 * v72 - 57 * v77 + 88 * v7 - 109 * v9 - 103 * v4 == 0xCF
        && 18 * v37 - 4 * v43 + -13 * v45 - 49 * v48 + -35 * v56 - 125 * v57 + -5 * v62 + 8 * v58 - 14 * v26 == 0xBF
        && 51 * v79 - 23 * v63 + -81 * v47 - 90 * v62 + -31 * v40 + 86 * v46 + 59 * v17 - 40 * v31 + (v16 << 6) == 20
        && 108 * v79 + 79 * v69 + -39 * v62 + 61 * v66 + -13 * v48 - 126 * v52 + 115 * v19 + 107 * v23 - s[73] == 0xF5
        && 121 * v23 - 76 * v25 + -23 * v27 - 45 * v38 + -46 * v56 + 110 * v50 + 58 * v18 + 35 * v20 + 68 * v7 == 0xF1
        && 113 * v22 + 98 * v35 + -51 * v52 - 53 * v56 + -126 * v67 - 24 * v74 + -47 * *s + 121 * v79 - 8 * v19 == 0x8B
        && (v19 << 7) + 48 * v35 + -74 * v47 + (v48 << 6) + 68 * v76 - 73 * v75 + -61 * v58 - 120 * v73 + 19 * v5 == 0x97
        && -126 * v23 - 93 * v36 + 87 * v51 + 22 * v52 + 118 * v54 - 102 * v58 + -16 * v69 - 2 * v79 - (v11 << 6) == 47
        && -32 * v33 + 35 * v34 + 38 * v35 + 89 * v38 + 10 * v45 + 34 * v46 + 20 * v50 - 19 * v53 + -85 * v5 - 47 * v22 == 0xAD )
      {
        puts("Correct!");
        return 0;
      }
      else
      {
        puts("Nope.");
        return 1;
      }
    }
    else
    {
      fprintf(_bss_start, "Wrong length (expect %d)\n", 80);
      return 3;
    }
  }
  else
  {
    fprintf(_bss_start, "Usage: %s \"flag{...}\"\n", *argv);
    return 2;
  }
}
```

## week4

### Maze

过程：这不是一道常规的简单迷宫题，你可以理解为几个支路上尽头有机关，你要推几次才能把走到终点的路打通。推荐用mazesolver求解，自己写脚本应该会折磨一点。
这是我重新标识过的迷宫代码

```C
char __fastcall maze(char a1)
{
  char result; // al
  unsigned int tmpCoor; // [rsp+0h] [rbp-28h]

  switch ( a1 )
  {
    case 'a':
      tmpCoor = coordinate - 1;
      break;
    case 'd':
      tmpCoor = coordinate + 1;
      break;
    case 's':
      tmpCoor = coordinate + 18;
      break;
    case 'w':
      tmpCoor = coordinate - 18;
      break;
    default:
      return 0;
  }
  if ( tmpCoor >= 324 )
    return 0;
  switch ( ifvalid[tmpCoor] )
  {
    case 0:
      coordinate = tmpCoor;
      result = 0;
      break;
    case 4:
      if ( ifvalid[79] )
        --ifvalid[79];
      result = 0;
      break;
    case 5:
      if ( ifvalid[195] )
        --ifvalid[195];
      result = 0;
      break;
    case 6:
      if ( ifvalid[281] )
        --ifvalid[281];
      result = 0;
      break;
    case 9:
      result = 0;
      break;
    case 100:
      coordinate = tmpCoor;
      result = 1;
      break;
    default:
      result = 0;
      break;
  }
  return result;
}
```

提取到的迷宫长这样

```
9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 
9 0 9 4 0 0 0 0 9 0 0 0 0 5 0 0 9 9 
9 0 9 9 9 0 9 0 9 9 9 9 9 0 9 0 9 9 
9 0 0 0 0 0 9 0 9 0 0 0 9 0 9 0 9 9 
9 9 9 9 9 9 9 1 9 0 9 0 9 0 9 9 9 9 
9 0 0 0 9 0 9 0 0 0 9 0 9 0 0 0 9 9 
9 0 9 0 9 0 9 9 9 9 9 0 9 0 9 0 9 9 
9 0 9 0 0 0 0 0 9 0 0 0 9 0 9 0 9 9 
9 0 9 9 9 9 9 9 9 0 9 9 9 9 9 0 9 9 
9 6 9 0 0 0 0 0 9 0 0 0 0 0 0 0 9 9 
9 0 9 0 9 9 9 0 9 9 9 9 9 9 9 2 9 9 
9 0 9 0 0 0 9 0 0 0 9 0 0 0 0 0 9 9 
9 0 9 9 9 0 9 9 9 0 9 0 9 9 9 9 9 9 
9 0 0 0 9 0 0 0 9 0 0 0 9 0 0 0 9 9 
9 0 9 0 9 9 9 0 9 9 9 9 9 0 9 0 9 9 
9 0 9 0 0 0 0 0 0 0 0 3 0 0 9 0 0 9 
9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 d 9 
9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 
```

观察代码，结合自己测试，我们要分别到4，5，6三个地方前面，对着这三个墙分别推1，2，3次，才能分别打通1，2，3处的墙。
用mazesolver看看（已打通时）
![](assets/img/media/pictures/2026_4_2/Pasted%20image%2020251018131000.png)
未打通时
![](assets/img/media/pictures/2026_4_2/屏幕截图%202025-10-18%20112808.png)
我们用mazesolver多次更改起点终点（分别到4，5，6三个墙前头），每个墙推几下
到4  ssddddwwa  然后推一下  a
![](assets/img/media/pictures/2026_4_2/Pasted%20image%2020251018131140.png)
到5  dddssssddwwddssssaassddddddwwwwaawww  推两下 ww
![](assets/img/media/pictures/2026_4_2/Pasted%20image%2020251018131205.png)
到6  sssddssssssaaaassaawwaawwaaaassddssddssaaaawwaawww  推三下 www
![](assets/img/media/pictures/2026_4_2/Pasted%20image%2020251018131225.png)
组合起来是这个

```
ssddddwwaadddssssddwwddssssaassddddddwwwwaawwwwwsssddssssssaaaassaawwaawwaaaassddssddssaaaawwaawwwwwwsssddssddddddddddwwddssds
```

主函数是

```C
int __fastcall main(int argc, const char **argv, const char **envp)
{
  int i; // [rsp+20h] [rbp-138h]
  unsigned __int64 v5; // [rsp+28h] [rbp-130h]
  char path[256]; // [rsp+40h] [rbp-118h] BYREF

  _dcrt_uninitialize_environments_nolock(argc, argv, envp);
  printf("Input the shortest path:");
  memset(path, 0, sizeof(path));
  scanf("%s", path);
  for ( i = 0; ; ++i )
  {
    v5 = -1;
    do
      ++v5;
    while ( path[v5] );
    if ( i >= v5 )
      break;
    if ( maze(path[i]) )
    {
      printf("Congratulation, the flag is 'flag{MD5(your path)}', notice: upper case\n");
      system("pause");
      return 0;
    }
  }
  printf("Oooops, you're on the wrong path\n");
  system("pause");
  return 0;
}
```

然后根据提示，把字符转成md5`10fa984b6294b96d8d5699ed7d4ef61a`(flag格式的[参考来源](https://xz.aliyun.com/news/12746))
然后写个大小写脚本转换一下

```Python
inp = input()  
for i in range(len(inp)):  
    if ord(inp[i]) >= ord('a') and ord(inp[i]) <= ord('z'):  
        print(chr(ord(inp[i])-32), end='')  
    else:  
        print(inp[i], end='')
```

得到flag：`flag{10FA984B6294B96D8D5699ED7D4EF61A}`

```
00000000418B00005087008100810000
418B0000000000005087000000000000
000000D230E100000000000000C26060
B4054696F5E6F6964716A796C6169627
5637F216471646E2F256D61676000041
2100000000000000000000000000B100
00008200000082F42E12600000000000
008080000000002010B405E6F6963727
5667F256D616760000310B0000000000
0000000000000000C000000020000000
205576E91D0000000000008080000000
002010B40593F216471646F256D61676
00003103000000000000000000000000
00B000000002000000026F1FD4B60000
000000008080000000002010B40583F2
16471646F256D616760000218A000000
00000000000000000000B00000000200
```

### Hachimi

过程：

```
#### 1. 密钥相关计算：`*(a2 + 4 * (ttl & 3)) + ttl`

- `a2`是密钥数组的起始地址（结合上下文，`v7`数组是 4 个 DWORD 的密钥，即`a2 = v7`）。
- `ttl`是随轮次变化的中间变量（初始为 0，每轮减去`delta=0x61C88647`），用于动态选择密钥和引入轮次差异。
- `(ttl & 3)`：`ttl`与 3 按位与，结果只能是`0-3`（因为 3 的二进制是`11`），用于从 4 个密钥元素中选择当前轮次使用的密钥（`v7[0]~v7[3]`）。
- `4 * (ttl & 3)`：因为每个密钥元素是 4 字节（DWORD），所以通过地址偏移`4*索引`访问对应的密钥元素（例如`索引=0`时，访问`v7[0]`）。
- `*(a2 + 4 * (ttl & 3))`：取出当前轮次选中的密钥元素。
- 整体：`选中的密钥元素 + 当前ttl值`，实现密钥与轮次变量的混合。
```

### SMC

```cpp
__int64 __fastcall sub_7FF6333B1240(__int64 a1, __int64 a2)
{
  __int64 result; // rax
  unsigned __int8 v3; // [rsp+0h] [rbp-258h]
  unsigned __int8 v4; // [rsp+1h] [rbp-257h]
  char v5; // [rsp+2h] [rbp-256h]
  char v6; // [rsp+3h] [rbp-255h]
  int k; // [rsp+8h] [rbp-250h]
  unsigned int n; // [rsp+Ch] [rbp-24Ch]
  unsigned int i; // [rsp+10h] [rbp-248h]
  int j; // [rsp+14h] [rbp-244h]
  unsigned int m; // [rsp+18h] [rbp-240h]
  int v12; // [rsp+1Ch] [rbp-23Ch]
  __int64 v13; // [rsp+28h] [rbp-230h]
  _BYTE v14[256]; // [rsp+40h] [rbp-218h] BYREF
  _BYTE v15[256]; // [rsp+140h] [rbp-118h] BYREF

  memset(v14, 0, sizeof(v14));
  for ( i = 0; i < 0x100; ++i )
    v14[i] = i;
  memset(v15, 0, sizeof(v15));
  for ( j = 0; j < 256; ++j )
    v15[j] = *(_BYTE *)(a2 + j % 4u) ^ 0x11;
  v12 = 0;
  for ( k = 0; k < 256; ++k )
  {
    v12 = ((unsigned __int8)v15[k] + (unsigned __int8)v14[k] + v12) % 256;
    v5 = v14[k];
    v14[k] = v14[v12];
    v14[v12] = v5;
  }
  v3 = 0;
  v4 = 0;
  v13 = -1;
  do
    ++v13;
  while ( *(_BYTE *)(a1 + v13) );
  for ( m = 0; m < (unsigned int)v13; ++m )
  {
    v4 += v14[++v3];
    v6 = v14[v3];
    v14[v3] = v14[v4];
    v14[v4] = v6;
    *(_BYTE *)(a1 + m) ^= v14[(unsigned __int8)(v14[v4] + v14[v3])];
  }
  for ( n = 0; ; ++n )
  {
    result = (unsigned int)v13;
    if ( n >= (unsigned int)v13 )
      break;
    if ( n )
      *(_BYTE *)(a1 + (int)n) ^= *(_BYTE *)(a1 + (int)(n - 1));
  }
  return result;
}
```

识别一下（一定要保留强制转换，以免出现错误）

```cpp
__int64 __fastcall sub_7FF6D6331240(byte *a1, __int64 a2)
{
  __int64 result; // rax
  unsigned __int8 v3; // [rsp+0h] [rbp-258h]
  unsigned __int8 v4; // [rsp+1h] [rbp-257h]
  char v5; // [rsp+2h] [rbp-256h]
  char v6; // [rsp+3h] [rbp-255h]
  int k; // [rsp+8h] [rbp-250h]
  unsigned int n; // [rsp+Ch] [rbp-24Ch]
  unsigned int i; // [rsp+10h] [rbp-248h]
  int j; // [rsp+14h] [rbp-244h]
  unsigned int m; // [rsp+18h] [rbp-240h]
  int v12; // [rsp+1Ch] [rbp-23Ch]
  __int64 v13; // [rsp+28h] [rbp-230h]
  _BYTE v14[256]; // [rsp+40h] [rbp-218h] BYREF
  _BYTE v15[256]; // [rsp+140h] [rbp-118h] BYREF

  memset(v14, 0, sizeof(v14));
  for ( i = 0; i < 0x100; ++i )
    v14[i] = i;
  memset(v15, 0, sizeof(v15));
  for ( j = 0; j < 256; ++j )
    v15[j] = *(_BYTE *)(a2 + j % 4u) ^ 0x11;
  v12 = 0;
  for ( k = 0; k < 256; ++k )
  {
    v12 = ((unsigned __int8)v15[k] + (unsigned __int8)v14[k] + v12) % 256;
    v5 = v14[k];
    v14[k] = v14[v12];
    v14[v12] = v5;
  }
  v3 = 0;
  v4 = 0;
  v13 = -1;
  do
    ++v13;
  while ( a1[v13] );
  for ( m = 0; m < (unsigned int)v13; ++m )
  {
    v4 += v14[++v3];
    v6 = v14[v3];
    v14[v3] = v14[v4];
    v14[v4] = v6;
    a1[m] ^= v14[(unsigned __int8)(v14[v4] + v14[v3])];
  }
  for ( n = 0; ; ++n )
  {
    result = (unsigned int)v13;
    if ( n >= (unsigned int)v13 )
      break;
    if ( n )
      a1[n] ^= a1[n - 1];
  }
  return result;
}
```
