---
title: N1CTF junior 2026 1／2 Reverse (部分)
date: "2026-05-11T19:50:17+08:00"
lastmod: "2026-05-19T08:52:40+08:00"
description: 只做了三个较简单的，还有两个有时间再细看
categories: [WP, "2026"]
tags: [逆向, 安卓, Wincrypt.h, Bitmap, 位图]
---

- 前言：做一下。之前并没有参加这个比赛（但是做了部分题）。现在重新做了一下，不会的地方参考了下别人的WP。还有两个比较难，暂时没做，可能后续有空会补上。
- 参考：

  [N1CTF Junior 2026 1/2 Reverse WriteUp](https://achcyano.github.io/posts/n1ctf2026_1_2__re_wp/)

  [https://xz.aliyun.com/news/91434](https://xz.aliyun.com/news/91434)

  [https://fering11.github.io/CTF/137726d9abb0/#Find-my-time](https://fering11.github.io/CTF/137726d9abb0/#Find-my-time)

# May Be Android

## 获取激活码

jadx打开，能看到

VipManager

```kotlin
package com.example.maybeandroid;

import android.content.Context;
import java.security.InvalidAlgorithmParameterException;
import java.security.InvalidKeyException;
import java.security.NoSuchAlgorithmException;
import java.util.Base64;
import javax.crypto.BadPaddingException;
import javax.crypto.Cipher;
import javax.crypto.IllegalBlockSizeException;
import javax.crypto.NoSuchPaddingException;
import javax.crypto.spec.IvParameterSpec;
import javax.crypto.spec.SecretKeySpec;
import kotlin.Metadata;
import kotlin.jvm.internal.Intrinsics;
import kotlin.text.Charsets;

/* JADX INFO: compiled from: MainActivity.kt */
/* JADX INFO: loaded from: classes2.dex */
@Metadata(d1 = {"\u0000(\n\u0002\u0018\u0002\n\u0002\u0010\u0000\n\u0002\b\u0003\n\u0002\u0010\u000e\n\u0002\b\u0007\n\u0002\u0010\u000b\n\u0000\n\u0002\u0018\u0002\n\u0002\b\u0005\n\u0002\u0010\u0012\n\u0000\bÇ\u0002\u0018\u00002\u00020\u0001B\t\b\u0002¢\u0006\u0004\b\u0002\u0010\u0003J\u000e\u0010\f\u001a\u00020\r2\u0006\u0010\u000e\u001a\u00020\u000fJ\u0016\u0010\u0010\u001a\u00020\r2\u0006\u0010\u000e\u001a\u00020\u000f2\u0006\u0010\u0011\u001a\u00020\u0005J\u0016\u0010\u0012\u001a\u00020\u00052\u0006\u0010\u0013\u001a\u00020\u00052\u0006\u0010\u0014\u001a\u00020\u0015R\u000e\u0010\u0004\u001a\u00020\u0005X\u0082T¢\u0006\u0002\n\u0000R\u000e\u0010\u0006\u001a\u00020\u0005X\u0082T¢\u0006\u0002\n\u0000R\u000e\u0010\u0007\u001a\u00020\u0005X\u0082T¢\u0006\u0002\n\u0000R\u000e\u0010\b\u001a\u00020\u0005X\u0082T¢\u0006\u0002\n\u0000R\u0014\u0010\t\u001a\u00020\u0005X\u0086D¢\u0006\b\n\u0000\u001a\u0004\b\n\u0010\u000b¨\u0006\u0016"}, d2 = {"Lcom/example/maybeandroid/VipManager;", "", "<init>", "()V", "PREFS_NAME", "", "KEY_IS_VIP", "VALID_CODE_ENC", "VIP_KEY", "VIP_SCRIPTS", "getVIP_SCRIPTS", "()Ljava/lang/String;", "isVip", "", "context", "Landroid/content/Context;", "activate", "code", "encrypt", "plainText", "keyBytes", "", "app_release"}, k = 1, mv = {2, 0, 0}, xi = 48)
public final class VipManager {
    public static final int $stable = 0;
    private static final String KEY_IS_VIP = "is_vip";
    private static final String PREFS_NAME = "app_prefs";
    private static final String VALID_CODE_ENC = "ZlZNZBpzLDK7C4yfjrQcGTlqAAr5EotPbAj+0eC9w0MHcOesjCs4nB/qgrcQFuxI";
    private static final String VIP_KEY = "8888888888888888";
    public static final VipManager INSTANCE = new VipManager();
    private static final String VIP_SCRIPTS = "flag_check.py";

    private VipManager() {
    }

    public final String getVIP_SCRIPTS() {
        return VIP_SCRIPTS;
    }

    public final boolean isVip(Context context) {
        Intrinsics.checkNotNullParameter(context, "context");
        return context.getSharedPreferences(PREFS_NAME, 0).getBoolean(KEY_IS_VIP, false);
    }

    public final boolean activate(Context context, String code) {
        Intrinsics.checkNotNullParameter(context, "context");
        Intrinsics.checkNotNullParameter(code, "code");
        if (code.length() == 32) {
            byte[] bytes = VIP_KEY.getBytes(Charsets.UTF_8);
            Intrinsics.checkNotNullExpressionValue(bytes, "getBytes(...)");
            if (Intrinsics.areEqual(encrypt(code, bytes), VALID_CODE_ENC)) {
                context.getSharedPreferences(PREFS_NAME, 0).edit().putBoolean(KEY_IS_VIP, true).apply();
                return true;
            }
        }
        return false;
    }

    public final String encrypt(String plainText, byte[] keyBytes) throws BadPaddingException, NoSuchPaddingException, IllegalBlockSizeException, NoSuchAlgorithmException, InvalidKeyException, InvalidAlgorithmParameterException {
        Intrinsics.checkNotNullParameter(plainText, "plainText");
        Intrinsics.checkNotNullParameter(keyBytes, "keyBytes");
        if (keyBytes.length != 16 && keyBytes.length != 24 && keyBytes.length != 32) {
            throw new IllegalArgumentException(("AES key must be 16/24/32 bytes, but got " + keyBytes.length).toString());
        }
        byte[] bArr = new byte[16];
        for (int i = 0; i < 16; i++) {
            bArr[i] = (byte) i;
        }
        SecretKeySpec secretKeySpec = new SecretKeySpec(keyBytes, "AES");
        IvParameterSpec ivParameterSpec = new IvParameterSpec(bArr);
        Cipher cipher = Cipher.getInstance("AES/CBC/PKCS5Padding");
        cipher.init(1, secretKeySpec, ivParameterSpec);
        byte[] bytes = plainText.getBytes(Charsets.UTF_8);
        Intrinsics.checkNotNullExpressionValue(bytes, "getBytes(...)");
        String strEncodeToString = Base64.getEncoder().encodeToString(cipher.doFinal(bytes));
        Intrinsics.checkNotNullExpressionValue(strEncodeToString, "encodeToString(...)");
        return strEncodeToString;
    }
}
```

观察这个类，里面用到了AES-CBC类加密，再套了个base64.密文，key，初始向量均给出。

```py
import base64
from Crypto.Cipher import AES
from Crypto.Util.Padding import unpad

# APK里的参数
key = b"8888888888888888"
iv = bytes(range(16))  # 等效于 bytes([0,1,2,...,15])
enc_b64 = "ZlZNZBpzLDK7C4yfjrQcGTlqAAr5EotPbAj+0eC9w0MHcOesjCs4nB/qgrcQFuxI"

# 解密
cipher = AES.new(key, AES.MODE_CBC, iv=iv)
plaintext = unpad(cipher.decrypt(base64.b64decode(enc_b64)), AES.block_size)

print("激活码:", plaintext.decode())  # 输出32位字符串
```

激活码：`F4E52DFB41CCC32F8FFFC340A3804383`

## 获取动态生成的py文件

这之后解包一下apk，搜索找到里面的`flag_check.py`​，在`assets\python\lib\python3.14\site-packages`目录下。

```py
import sys
len(sys.argv) != 2 and (print("error arguments provided, exiting.") or exit(0))
f = sys.argv[1]
a = open("😇","r")
b = a ^ "😋"
for i in f: b << "😢"
if not (b == "😃") == (((a ^ "😋") << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢') == "😃"):print("Length error");exit(0)
s = a ^ "🫨"
j = (((a ^ "😋")) == "😃")
c = (((a ^ "😋") << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢") == "😃")
d = a ^ "😁"
for i in range(c): j = (j + s[i] + (d << "😢")) % c; s[i], s[j] = s[j], s[i]
r = []
i,j = (((a ^ "😋")) == "😃"),(((a ^ "😋")) == "😃")
e = a ^ "😤"
for _ in f: i = (i + 1) % c; j = (j + s[i]) % c; s[i], s[j] = s[j], s[i]; g = s[(s[i] + s[j]) % c]; v = ord(_) ^ g; r.append(v);v != (e << "😢") and (print("Wrong ):") or exit(0))
print("Success!")
print("Flag is flag{<your_input>}")

```

发现这个脚本打开了`a = open("😇","r")`一个文件，然而不在解包文件里面，说明应该是动态生成的。

再看看另外一个

VipDecryptor

```kotlin
package com.example.maybeandroid;

import android.content.Context;
import java.io.File;
import kotlin.Metadata;
import kotlin.io.FilesKt;
import kotlin.jvm.internal.Intrinsics;

/* JADX INFO: compiled from: VipDecryptor.kt */
/* JADX INFO: loaded from: classes2.dex */
@Metadata(d1 = {"\u0000\u001e\n\u0002\u0018\u0002\n\u0002\u0010\u0000\n\u0000\n\u0002\u0018\u0002\n\u0002\b\u0003\n\u0002\u0010\u0002\n\u0000\n\u0002\u0010\u0012\n\u0000\b\u0007\u0018\u00002\u00020\u0001B\u000f\u0012\u0006\u0010\u0002\u001a\u00020\u0003¢\u0006\u0004\b\u0004\u0010\u0005J\u0006\u0010\u0006\u001a\u00020\u0007J\t\u0010\b\u001a\u00020\tH\u0082 R\u000e\u0010\u0002\u001a\u00020\u0003X\u0082\u0004¢\u0006\u0002\n\u0000¨\u0006\n"}, d2 = {"Lcom/example/maybeandroid/VipDecryptor;", "", "context", "Landroid/content/Context;", "<init>", "(Landroid/content/Context;)V", "saveDecryptedScript", "", "getDecryptedScript", "", "app_release"}, k = 1, mv = {2, 0, 0}, xi = 48)
public final class VipDecryptor {
    public static final int $stable = 8;
    private final Context context;

    private final native byte[] getDecryptedScript();

    public VipDecryptor(Context context) {
        Intrinsics.checkNotNullParameter(context, "context");
        this.context = context;
        System.loadLibrary("vipdecryptor");
    }

    public final void saveDecryptedScript() {
        byte[] decryptedScript = getDecryptedScript();
        File file = new File(this.context.getFilesDir(), "python/lib/python3.14/site-packages");
        if (!file.exists()) {
            file.mkdirs();
        }
        FilesKt.writeBytes(new File(file, "sitecustomize.py"), decryptedScript);
    }
}
```

### 思路1：分析so文件，提取数据

只有这里出现了往site-packages写入文件的操作，那很可能这个脚本创建了我们需要的文件。构造函数里面加载了库，我们去看那个库。

找到`libvipdecryptor.so`​，看看用到的函数`getDecryptedScript`

```C
__int64 __fastcall Java_com_example_maybeandroid_VipDecryptor_getDecryptedScript(__int64 a1)
{
  __int64 v1; // r15
  char *v2; // r14
  unsigned __int64 n0x670; // r12
  bool v4; // cf
  __int64 v5; // r15

  v1 = sub_1240(16);
  sub_F80(
    sitecustomize_py,                           // "sitecustomize.py"
    v1);
  v2 = malloc(0x6A0u);
  n0x670 = 0;
  do
  {
    sub_12A0(&unk_AC10 + n0x670, &v2[n0x670], v1);
    v4 = n0x670 < 0x670; //1648
    n0x670 += 16LL;
  }
  while ( v4 );
  v5 = (*(*a1 + 0x580LL))(a1, 0x680);
  (*(*a1 + 0x680LL))(a1, v5, 0, 0x680, v2);
  return v5;
}
```

通过findcrypt插件，找到里面解密数据用到了AES。

![](/assets/img/media/2026_5_19/image-20260511210046-j6ye024.png)

既然是AES，那么上面的字符串`sitecustomize.py`就是密钥。没有初始向量，说明是ECB模式加密。

里面出现数字`0x670`，也就是加密的数据大小是1648。

创建对应大小的数组，提取raw bytes出来，写脚本解密。

```py
#!/usr/bin/env python3
from Crypto.Cipher import AES

# 直接用已知的参数
key = b"sitecustomize.py"
data = open("export_results.txt", "rb").read()

# AES-128-ECB 解密
dec = AES.new(key, AES.MODE_ECB).decrypt(data)


# 输出
open("sitecustomize.py", "w", encoding="utf-8").write(dec.decode("utf-8"))
print("✅ 解密完成 → sitecustomize.py")
```

得到的`sitecustomize.py`

```py
import builtins



class Origin:

    def __init__(self):

        self.open = builtins.open



origin = Origin()



class CustomSum:

    def __init__(self):

        self.sum = 0



    def __lshift__(self, other):

        if other == "😢":

            self.sum += 1

            return self

        

    def __eq__(self, value):

        if value == "😃":

            return self.sum

        return False



class Keyget:

    def __init__(self):

        self.key = "y0u_@re_vip_Us3r"

        self.index = 0



    def __lshift__(self, other):

        if other == "😢":

            val = ord(self.key[self.index % len(self.key)])

            self.index += 1

            return val



class GetEnc:

    def __init__(self):

        self.enc_data = bytes.fromhex("738d9ea5a7c5824836d63c872324e36936c1dd7026b2df418268066a936256a7")

        self.index = 0

    def __lshift__(self, other):

        if other == "😢":

            val = self.enc_data[self.index % len(self.enc_data)]

            self.index += 1

            return val ^ 0x55



class oprate:

    def __init__(self,file,mode,*args,**kwargs):

        if file == "😇" and mode == "r":

            return

        try :

            origin.open(file = file, mode = mode, *args, **kwargs)

        except Exception as e:

            print(e)

  

    def __xor__(self, other):

        if other == "😋":

            return CustomSum()

        elif other == "🫨":

            return list(range(256))

        elif other == "😁":

            return Keyget()

        elif other == "😤":

            return GetEnc()

        return self



builtins
```

### 思路2：运行软件，让软件生成py

这个需要收集具备root权限，以访问data/data目录。

再观察一下交叉引用，PythonRunner类里面新建了这个类。

```kotlin

    public final int run(String args) throws JSONException, IOException, ErrnoException {
        Intrinsics.checkNotNullParameter(args, "args");
        JSONArray jSONArray = new JSONArray(args);
        int length = jSONArray.length() + 1;
        String[] strArr = new String[length];
        for (int i = 0; i < length; i++) {
            strArr[i] = "";
        }
        int length2 = jSONArray.length();
        int i2 = 0;
        while (i2 < length2) {
            int i3 = i2 + 1;
            String string = jSONArray.getString(i2);
            Intrinsics.checkNotNullExpressionValue(string, "getString(...)");
            strArr[i3] = string;
            i2 = i3;
        }
        Os.setenv("TMPDIR", this.context.getCacheDir().toString(), false);
        Os.setenv("PYTHONUNBUFFERED", "1", true);
        File fileExtractAssets = extractAssets();
        if (VipManager.INSTANCE.isVip(this.context)) {
            new VipDecryptor(this.context).saveDecryptedScript();  //这里
        }
        Native.INSTANCE.loadOnce();
        redirectStdioToLogcat();
        String string2 = fileExtractAssets.toString();
        Intrinsics.checkNotNullExpressionValue(string2, "toString(...)");
        return runPython(string2, strArr);
    }

```

然后再追过去之前的

```kotlin
    private static final String KEY_IS_VIP = "is_vip";
    
	public final boolean isVip(Context context) {
        Intrinsics.checkNotNullParameter(context, "context");
        return context.getSharedPreferences(PREFS_NAME, 0).getBoolean(KEY_IS_VIP, false);
    }
	
```

也就是激活VIP后，参数随便填个东西就能生成文件。

没有root过的手机，可以用Android Studio。下好，随便创建个项目，然后去下载Google API版本的任意镜像。

![](/assets/img/media/2026_5_19/image-20260511221418-jvehsfo.png)

![](/assets/img/media/2026_5_19/image-20260511221453-859zcmc.png)

过程中会自动接入adb。启动后拖入apk安装。之后输入激活码激活vip。随便填点东西执行一下。之后在cmd确认adb devices接入了虚拟机，然后去

```cmd
adb root
adb shell find /data/data/com.example.maybeandroid -name "sitecustomize.py"
adb shell cat /data/data/com.example.maybeandroid/files/python/lib/python3.14/site-packages/sitecustomize.py
```

获取到文件

```py
import builtins

class Origin:
    def __init__(self):
        self.open = builtins.open

origin = Origin()

class CustomSum:
    def __init__(self):
        self.sum = 0

    def __lshift__(self, other):
        if other == "😢":
            self.sum += 1
            return self

    def __eq__(self, value):
        if value == "😃":
            return self.sum
        return False

class Keyget:
    def __init__(self):
        self.key = "y0u_@re_vip_Us3r"
        self.index = 0

    def __lshift__(self, other):
        if other == "😢":
            val = ord(self.key[self.index % len(self.key)])
            self.index += 1
            return val

class GetEnc:
    def __init__(self):
        self.enc_data = bytes.fromhex("738d9ea5a7c5824836d63c872324e36936c1dd7026b2df418268066a936256a7")
        self.index = 0
    def __lshift__(self, other):
        if other == "😢":
            val = self.enc_data[self.index % len(self.enc_data)]
            self.index += 1
            return val ^ 0x55

class oprate:
    def __init__(self,file,mode,*args,**kwargs):
        if file == "😇" and mode == "r":
            return
        try :
            origin.open(file = file, mode = mode, *args, **kwargs)
        except Exception as e:
            print(e)

    def __xor__(self, other):
        if other == "😋":
            return CustomSum()
        elif other == "🫨":
            return list(range(256))
        elif other == "😁":
            return Keyget()
        elif other == "😤":
            return GetEnc()
        return self

builtins.open = oprate
```

## 分析py

两个文件。

​`flag_check.py`

```py
import sys
len(sys.argv) != 2 and (print("error arguments provided, exiting.") or exit(0))
f = sys.argv[1]
a = open("😇","r")
b = a ^ "😋"
for i in f: b << "😢"
if not (b == "😃") == (((a ^ "😋") << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢' << '😢') == "😃"):print("Length error");exit(0)
s = a ^ "🫨"
j = (((a ^ "😋")) == "😃")
c = (((a ^ "😋") << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢" << "😢") == "😃")
d = a ^ "😁"
for i in range(c): j = (j + s[i] + (d << "😢")) % c; s[i], s[j] = s[j], s[i]
r = []
i,j = (((a ^ "😋")) == "😃"),(((a ^ "😋")) == "😃")
e = a ^ "😤"
for _ in f: i = (i + 1) % c; j = (j + s[i]) % c; s[i], s[j] = s[j], s[i]; g = s[(s[i] + s[j]) % c]; v = ord(_) ^ g; r.append(v);v != (e << "😢") and (print("Wrong ):") or exit(0))
print("Success!")
print("Flag is flag{<your_input>}")

```

​`sitecustomize.py`

```py
import builtins

class Origin:
    def __init__(self):
        self.open = builtins.open

origin = Origin()

class CustomSum:
    def __init__(self):
        self.sum = 0

    def __lshift__(self, other):
        if other == "😢":
            self.sum += 1
            return self

    def __eq__(self, value):
        if value == "😃":
            return self.sum
        return False

class Keyget:
    def __init__(self):
        self.key = "y0u_@re_vip_Us3r"
        self.index = 0

    def __lshift__(self, other):
        if other == "😢":
            val = ord(self.key[self.index % len(self.key)])
            self.index += 1
            return val

class GetEnc:
    def __init__(self):
        self.enc_data = bytes.fromhex("738d9ea5a7c5824836d63c872324e36936c1dd7026b2df418268066a936256a7")
        self.index = 0
    def __lshift__(self, other):
        if other == "😢":
            val = self.enc_data[self.index % len(self.enc_data)]
            self.index += 1
            return val ^ 0x55

class oprate:
    def __init__(self,file,mode,*args,**kwargs):
        if file == "😇" and mode == "r":
            return
        try :
            origin.open(file = file, mode = mode, *args, **kwargs)
        except Exception as e:
            print(e)

    def __xor__(self, other):
        if other == "😋":
            return CustomSum()
        elif other == "🫨":
            return list(range(256))
        elif other == "😁":
            return Keyget()
        elif other == "😤":
            return GetEnc()
        return self

builtins.open = oprate
```

这里的emoji是运算符重载后的新运算符。看到`flag_check.py`这一行

```py
for _ in f: i = (i + 1) % c; j = (j + s[i]) % c; s[i], s[j] = s[j], s[i]; g = s[(s[i] + s[j]) % c]; v = ord(_) ^ g; r.append(v);v != (e << "😢") and (print("Wrong ):") or exit(0))
```

会发现这是rc4的一部分，所以这个加密是rc4，最后多异或了`0x55`。

从`sitecustomize.py`提取关键数据

```py
		self.enc_data = bytes.fromhex("738d9ea5a7c5824836d63c872324e36936c1dd7026b2df418268066a936256a7")
		self.key = "y0u_@re_vip_Us3r"
    def __lshift__(self, other):
        if other == "😢":
            val = self.enc_data[self.index % len(self.enc_data)]
            self.index += 1
            return val ^ 0x55
```

## 获取flag

解密

```py
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
        plain_byte = cipher[k] ^ s_val ^ 0x55
        plain.append(plain_byte)
    return bytes(plain)


cipherbe = "738d9ea5a7c5824836d63c872324e36936c1dd7026b2df418268066a936256a7"

cipher = bytes.fromhex(cipherbe)
key = b"y0u_@re_vip_Us3r"

flag = rc4_decrypt(cipher, key)
print("flag{" + flag.decode() + "}")
```

flag：`flag{5f19b83de29bd46e9e02f7f88bfb4ea2}`

# Wizard Time

## 定位校验函数

主函数

```c
__int64 Il11()
{
  int v0; // eax
  _BYTE v2[64]; // [rsp+20h] [rbp-60h] BYREF
  _BYTE v3[64]; // [rsp+60h] [rbp-20h] BYREF
  _DWORD v4[8]; // [rsp+A0h] [rbp+20h] BYREF
  _BYTE v5[340]; // [rsp+C0h] [rbp+40h] BYREF
  int n130; // [rsp+214h] [rbp+194h]
  unsigned int n17; // [rsp+218h] [rbp+198h]
  int i; // [rsp+21Ch] [rbp+19Ch]

  i1iII1();
  1i1III1llilli1ll1l1ll1li(65001);
  iiII11lIliI(&unk_14000C608, "Welcome to the Magic Spell Tutorial Course at the Magic Academy!\n");
  iiII11lIliI(&unk_14000C688, "You'll practice spells here, correct spells'll show amazing effects.\n");
  iiII11lIliI(&unk_14000C730, "In the first class, you need to learn simple spells related to five basic elements.\n");
  iiII11lIliI(&unk_14000C7C8, "These five basic elements are - water, fire, earth, wind, and ether.\n");
  iiII11lIliI(&unk_14000C860, "You need to master five spells at the same time to complete this lesson.\n");
  for ( i = 0; i <= 4; ++i )
  {
    I111111I(v2, 64, Input_your_magic_string_line_d__, (unsigned int)(i + 1));
    I111111I(v3, 64, "Input your magic string line%d: ", i + 1);
    iiII11lIliI(v2, v3);
    v0 = iIIi1illlIl1i(&v5[65 * i], 65);
    v4[i] = v0;
  }
  iiII11lIliI(&unk_14000C928, "Let's check your learning results.\n");
  n130 = iI1iii1l1ilIi(v5, v4);  //显然就是这里
  n17 = 0;
  if ( n130 == 130 )
  {
    iiII11lIliI(&unk_14000CA30, "Wow, your grades are perfect! Let's see what you'll get.\n");
    n17 = 17;
  }
  else
  {
    iiII11lIliI(&unk_14000C9A0, "Um... It may not look perfect, but why not having a try for it?\n");
    n17 = (int)((double)(10 * n130) / 130.0 + 7.0);
  }
  IlIlIi1II(&unk_14000CA80);
  IlIlIi1II(&unk_14000CB78);
  1l1liil();
  1l1liil();
  iiII11lIliI(&unk_14000CBF2, "IT'S WIZARD TIME!\n");
  Ii1liIilliI(2000);
  1Il11ilIlI1iIii1(n17);
  Ii1liIilliI(2000);
  i11liIiii111l111iil();
  IlIlIi1II(&unk_14000CC10);
  1l1liil();
  return 0;
}
```

里面的函数全部进行了相似符号混淆。定位到主函数后，可以找到这个函数，很显然是校验flag的

```c
__int64 __fastcall iI1iii1l1ilIi(__int64 a1, __int64 a2)
{
  _QWORD v3[131]; // [rsp+20h] [rbp-60h] BYREF
  unsigned int n0x1A; // [rsp+43Ch] [rbp+3BCh]
  char v5; // [rsp+443h] [rbp+3C3h]
  int n63; // [rsp+444h] [rbp+3C4h]
  int n; // [rsp+448h] [rbp+3C8h]
  int m; // [rsp+44Ch] [rbp+3CCh]
  unsigned int v9; // [rsp+450h] [rbp+3D0h]
  int k; // [rsp+454h] [rbp+3D4h]
  int j; // [rsp+458h] [rbp+3D8h]
  int i; // [rsp+45Ch] [rbp+3DCh]

  Il111l11();
  for ( i = 0; i <= 4; ++i )
  {
    if ( (unsigned __int8)IilIlii1l1lIii(65LL * i + a1) != 1 )
    {
      IlIlIi1II(&unk_14000C530);
      11ll();
    }
  }
  I1Iiil(v3, 0, 1040);
  for ( j = 0; j <= 4; ++j )
  {
    n63 = *(_DWORD *)(4LL * j + a2);
    if ( n63 > 63 )
    {
      IlIlIi1II(&unk_14000C560);
      11ll();
    }
    for ( k = 0; k < n63; ++k )
    {
      v5 = *(_BYTE *)(a1 + 65LL * j + k);
      n0x1A = v5 - 97;
      if ( n0x1A >= 0x1A )
      {
        IlIlIi1II(&unk_14000C590);
        11ll();
      }
      1ii[64 * (__int64)j + k] = n0x1A + 1;
      v3[26 * j + (int)n0x1A] += 1LL << k;
    }
  }
  v9 = 0;
  for ( m = 0; m <= 4; ++m )
  {
    for ( n = 0; n <= 25; ++n )
    {
      if ( v3[26 * m + n] == I1iiIi[26 * m + n] )
        ++v9;
    }
  }
  return v9;
}
```

相当丑，且很难阅读。

## 简化

比如这里`v5 = *(_BYTE *)(a1 + 65LL * j + k);`，这个v5不怎么好看

。`*(_BYTE *)`​说明是二级指针，v5是个二维数组。而`_BYTE*`​就是`char*`，65代表长度。

根据这个特征去调整，并重命名函数，部分变量名。

简化后

```c
__int64 __fastcall check_spells(char (*input)[65], int *Val)
{
  size_t Size; // r8
  _QWORD v1[131]; // [rsp+20h] [rbp-60h] BYREF
  unsigned int n0x1A; // [rsp+43Ch] [rbp+3BCh]
  char v6; // [rsp+443h] [rbp+3C3h]
  int n63; // [rsp+444h] [rbp+3C4h]
  int n; // [rsp+448h] [rbp+3C8h]
  int m; // [rsp+44Ch] [rbp+3CCh]
  unsigned int v10; // [rsp+450h] [rbp+3D0h]
  int k; // [rsp+454h] [rbp+3D4h]
  int j; // [rsp+458h] [rbp+3D8h]
  int i; // [rsp+45Ch] [rbp+3DCh]

  memset_0(input, Val, Size);
  for ( i = 0; i <= 4; ++i )
  {
    if ( isValidASCII(&(*input)[65 * i]) != 1 )
    {
      printf("你的咒语格式不正确！\nFormat wrong!\n");
      getchar_1();
    }
  }
  memset_1(v1, 0, 0x410u);
  for ( j = 0; j <= 4; ++j )
  {
    n63 = Val[j];
    if ( n63 > 63 )
    {
      printf("你的咒语太长了！\nSpell too long!\n");
      getchar_1();
    }
    for ( k = 0; k < n63; ++k )
    {
      v6 = (*input)[65 * j + k];
      n0x1A = v6 - 'a';
      if ( n0x1A >= 0x1A )
      {
        printf("内部错误：非法字符！\nInternal error!\n");
        getchar_1();
      }
      v2[64 * j + k] = n0x1A + 1;
      v1[26 * j + n0x1A] += 1LL << k;
    }
  }
  v10 = 0;
  for ( m = 0; m <= 4; ++m )
  {
    for ( n = 0; n <= 25; ++n )
    {
      if ( v1[26 * m + n] == target[26 * m + n] )
        ++v10;
    }
  }
  return v10;
}
```

能很容易看出来这是二维数组了。

## 解析bitmap

```c
    for ( k = 0; k < n63; ++k )
    {
      v6 = (*input)[65 * j + k];
      n0x1A = v6 - 'a';
      if ( n0x1A >= 0x1A )
      {
        printf("内部错误：非法字符！\nInternal error!\n");
        getchar_1();
      }
      v2[64 * j + k] = n0x1A + 1;
      v1[26 * j + n0x1A] += 1LL << k;
    }
```

通过这一部分，可以得知这是把spell以bitmap的方式储存的。

>  NOTE*位图（Bitmap）是一种紧凑的数据表示方法，它使用 二进制位（bit）来高效地存储某种状态、标志或信息。位图的特点是用每一位二进制数据（0 或 1）表示某种属性的存在或不存在，因此对于某类数据的表示非常紧凑和高效。*

```py
IDA_14000c120 = [0x0, 0x0, 0xa0881420800883, 0x4014080154000, 0x8000000000000, 0x540200401540, 0x2281042aa210, 0x5000080a000008, 0x0, 0x1000011000004, 0x0, 0x20, 0x0, 0x2040000000, 0x0, 0x2000000000000, 0x0, 0x0, 0x0, 0x0, 0x0, 0x0, 0x0, 0x0, 0x0, 0x0, 0x40000100009, 0x0, 0x100208000040, 0x2a08491421480, 0x0, 0x20000080004, 0x5412042840902, 0x4000020, 0x0, 0x0, 0x0, 0x85820016200, 0x0, 0x0, 0x0, 0x100200010, 0x0, 0x0, 0x0, 0x8000, 0x0, 0x0, 0x0, 0x0, 0x0, 0x0, 0x501200808100402, 0x0, 0x1802d163005060, 0x20000000000000, 0x1, 0x52490808010, 0x40080000010000, 0x84100004002800, 0x0, 0x2028000002a0284, 0x0, 0x400200040100, 0x0, 0x400008, 0x0, 0x0, 0x0, 0x0, 0x0, 0x0, 0x0, 0x0, 0x0, 0x0, 0x0, 0x0, 0x1, 0x0, 0x0, 0x2a020100800, 0x0, 0x0, 0x54484288102, 0xb030540a4, 0x0, 0x0, 0x0, 0x58c22648, 0x0, 0x0, 0x0, 0x1000001010, 0x0, 0x0, 0x0, 0x0, 0x0, 0x0, 0x0, 0x0, 0x0, 0x0, 0xc003803000718, 0x0, 0x50124824042a003, 0x20000000000000, 0x40000000000000, 0xa4120040880, 0x10000000, 0x282400404814024, 0x0, 0x8000000, 0x0, 0x800000301000, 0x0, 0x10110080080000, 0x0, 0x0, 0x0, 0x40, 0x0, 0x0, 0x0, 0x0, 0x0, 0x0, 0x0, 0x0]

ALPHABET = "abcdefghijklmnopqrstuvwxyz"
def bits_positions(x):
    """返回 x 中所有为1的bit的位置列表"""
    pos = []  # 收集结果
    i = 0  # 当前在哪个bit位

    while x != 0:  # 只要还有1没处理
        if x & 1 == 1:  # x的最低位是不是1？
            pos.append(i)  # 是 → 记下这个位置
        x = x >> 1  # 右移一位，扔掉最低位
        i = i + 1  # 位置+1

    return pos
def decode_line(bitmap_26):
    """26 个 QWORD → 一行字符串"""
    pos_to_char = {}
    max_pos = -1
    for letter_idx, bitmap in enumerate(bitmap_26):
        if bitmap == 0:
            continue
        ch = ALPHABET[letter_idx]
        for p in bits_positions(bitmap):
            pos_to_char[p] = ch
            if p > max_pos:
                max_pos = p
    if max_pos < 0:
        return ""
    return "".join(pos_to_char[p] for p in range(max_pos + 1))
# === 主程序 ===
for i in range(5):
    bitmap = IDA_14000c120[i*26 : (i+1)*26]
    print(decode_line(bitmap))
```

> python的 enumerate：
>
> 对于for letter_idx, bitmap in enumerate(bitmap_26)，letter_idx代表遍历到第i位（从0开始），bitmap代表第i位上的元素。

> 对bitmap个人理解：
>
> 每26个QWORD为一行spell的位图，从a到z。然后单独解析每个QWORD，不同的bit 位不会冲突。

## 得到flag

之后把得到的spell，按行输入到程序里面。

```plaintext
ccjhglfcfgfcfgdgdgdgdgfcjhghjcndgfchcndgdgfcfgfcjpdehchc
agfaphcdgldgdlltldgfapdgdghcdlgdpcdllgldgfalcdgdgdg
eajnfccjljahchcfgjljajnfcchafccfclfacfccfcfghaljajhccdghaja
aghlphlhglldplhghlhgdgllhhglldlghhghpdgdgdg
cchaahrfaaaflchchcfnllchaahjgfcnfchaaafcnfcfnchlchaandehchc
```

![](/assets/img/media/2026_5_19/image-20260513142212-mkwvxql.png)

![](/assets/img/media/2026_5_19/image-20260513142221-gg1pqr3.png)

肉眼识别出来：`317427355F77317A3452645F37314D33`

转换到ASCII，套上`flag{}`​为：`flag{1t'5_w1z4Rd_71M3}`

# Microsoft VS Code

## 主函数

这个涉及到windows.h的wincrypt。

```cpp
// Hidden C++ exception states: #wind=1
int __fastcall main(int argc, const char **argv, const char **envp)
{
  HCRYPTKEY hKey; // [rsp+40h] [rbp-B8h] BYREF
  HCRYPTPROV phProv; // [rsp+48h] [rbp-B0h] BYREF
  DWORD pdwDataLen; // [rsp+50h] [rbp-A8h] BYREF
  BYTE *Buf1; // [rsp+58h] [rbp-A0h]
  int v8; // [rsp+60h] [rbp-98h]
  int v9; // [rsp+64h] [rbp-94h]
  int v10; // [rsp+68h] [rbp-90h]
  DWORD dwFlags; // [rsp+6Ch] [rbp-8Ch]
  DWORD dwBufLen; // [rsp+70h] [rbp-88h]
  int v13; // [rsp+74h] [rbp-84h]
  size_t Size; // [rsp+78h] [rbp-80h]
  void *Src; // [rsp+80h] [rbp-78h]
  void *v16; // [rsp+88h] [rbp-70h]
  void *v17; // [rsp+90h] [rbp-68h]
  BYTE v18[32]; // [rsp+98h] [rbp-60h] BYREF
  BYTE pbData[48]; // [rsp+B8h] [rbp-40h] BYREF

  phProv = 0;
  hKey = 0;
  v8 = byte_14003C018[0];
  v10 = byte_14003C018[0] >> (unk_14003C012
                            + byte_14003C018[0]
                            - byte_14003C014[2]
                            - unk_14003C010 / byte_14003C018[3]
                            - byte_14003C014[0]);
  v9 = unk_14003C011;
  dwFlags = v10 << (byte_14003C018[0] * (unk_14003C010 / unk_14003C011) / unk_14003C011);
  if ( !CryptAcquireContextA(
          &phProv,
          nullptr,
          nullptr,
          byte_14003C014[0] / byte_14003C018[0] + byte_14003C018[4] - unk_14003C010 - unk_14003C011,
          dwFlags) )
    return 1;
  memset(&pbData[1], 0, 0x2Bu);
  pbData[0] = byte_14003C014[0] - byte_14003C014[2];
  pbData[1] = byte_14003C018[4] - byte_14003C018[2];
  *&pbData[2] = 0;
  *&pbData[4] = unk_14003C011 + ((byte_14003C018[2] - unk_14003C007) << unk_14003C008) - unk_14003C001;
  *&pbData[8] = byte_14003C018[6] - unk_14003C00E - (unk_14003C011 - unk_14003C001);
  memcpy(&pbData[12], &unk_14003C000, *&pbData[8]);
  if ( CryptImportKey(
         phProv,
         pbData,
         unk_14003C003
       ^ byte_14003C018[6]
       ^ byte_14003C018[7]
       & (unk_14003C010
        ^ (byte_14003C018[6] & byte_14003C018[0])),
         0,
         0,
         &hKey) )
  {
    if ( CryptSetKeyParam(hKey, byte_14003C018[4] / byte_14003C018[6], &::pbData, 0) )
    {
      sub_14000AE70(v18);
      sub_1400090A0(&qword_14003D5E0, "Enter your flag: ");
      sub_14000A910(&qword_14003D540, v18);
      if ( unknown_libname_70(v18) > byte_14003C018[7] )
        sub_14000E920(v18, byte_14003C018[7], 0);
      Buf1 = j__malloc_base(byte_14003C018[6] - unk_14003C00E - (unk_14003C011 - unk_14003C001));
      Size = unknown_libname_70(v18);
      Src = unknown_libname_w(v18);
      memcpy(Buf1, Src, Size);
      pdwDataLen = byte_14003C018[6] - unk_14003C00E - (unk_14003C011 - unk_14003C001);
      dwBufLen = byte_14003C018[6] - unk_14003C00E;
      if ( CryptEncrypt(hKey, 0, 1, 0, Buf1, &pdwDataLen, dwBufLen) )
      {
        if ( pdwDataLen == byte_14003C018[6] - unk_14003C00E && !memcmp(Buf1, &Buf2_, byte_14003C018[6] - unk_14003C00E) )
        {
          v16 = sub_1400090A0(&qword_14003D5E0, "Correct!");
          _CallMemberFunction0(v16, sub_14000A650);
        }
        else
        {
          v17 = sub_1400090A0(&qword_14003D5E0, "Wrong!");
          _CallMemberFunction0(v17, sub_14000A650);
        }
        free(Buf1);
        CryptDestroyKey(hKey);
        CryptReleaseContext(phProv, 0);
        exit(0);
      }
      CryptDestroyKey(hKey);
      CryptReleaseContext(phProv, 0);
      v13 = 1;
      sub_14000BB10(v18);
      return v13;
    }
    else
    {
      CryptDestroyKey(hKey);
      CryptReleaseContext(phProv, 0);
      return 1;
    }
  }
  else
  {
    CryptReleaseContext(phProv, 0);
    return 1;
  }
}
```

## `CryptImportKey`

分析`CryptImportKey`，主要关注第二个和第三个参数。

> - [CryptImportKey 函数 （wincrypt.h）](https://learn.microsoft.com/zh-cn/windows/win32/api/wincrypt/nf-wincrypt-cryptimportkey)
>
>   ```cpp
>   BOOL CryptImportKey(
>     [in]  HCRYPTPROV hProv,
>     [in]  const BYTE *pbData,
>     [in]  DWORD      dwDataLen,
>     [in]  HCRYPTKEY  hPubKey,
>     [in]  DWORD      dwFlags,
>     [out] HCRYPTKEY  *phKey
>   );
>   ```
>
>   ​`[in] pbData`
>
>   **BYTE** 数组，其中包含 [PUBLICKEYSTRUC](https://learn.microsoft.com/zh-cn/windows/desktop/api/wincrypt/ns-wincrypt-publickeystruc) BLOB 标头，后跟加密密钥。 此密钥 BLOB 由 [CryptExportKey](https://learn.microsoft.com/zh-cn/windows/desktop/api/wincrypt/nf-wincrypt-cryptexportkey) 函数创建，无论是在此应用程序中，还是由可能在另一台计算机上运行的另一个应用程序创建。
>
>   ​`[in] dwDataLen`
>
>   包含密钥 BLOB 的长度（以字节为单位）。
> - **CryptImportKey** 函数可用于为对称算法导入纯文本密钥;但是，我们建议你改用 [CryptGenKey](https://learn.microsoft.com/zh-cn/windows/desktop/api/wincrypt/nf-wincrypt-cryptgenkey) 函数，以便于使用。 导入纯文本密钥时，*pbData* 参数中传递的密钥 BLOB 的结构是 [PLAINTEXTKEYBLOB](https://learn.microsoft.com/zh-cn/previous-versions/windows/desktop/legacy/jj650836(v=vs.85))。

## 获取BLOB

接下来需要获取BLOB，由于有`IsDebuggerPresent`，IDA patch起来麻烦一点，换用x64dbg配合Scyllahide去获取。

> [CSP学习之导出密钥BLOB 解析](https://www.cnblogs.com/dspeeding/p/3338082.html)
>
> - [blobHEADER 结构 (wincrypt.h)](https://learn.microsoft.com/zh-cn/windows/win32/api/wincrypt/ns-wincrypt-publickeystruc)
>
>   ‍
>
>   BLOB Header的结构
>
>   ```cpp
>   typedef struct _PUBLICKEYSTRUC{
>       BYTE     bType;
>       BYTE     bVersion;
>       WORD  reserved;
>       ALG_ID aiKeyAlg;
>   }BLOBHEARER,
>   PUBLICKEYSTURC;
>   ```
>
>   ​`bType`
>
>   包含密钥 BLOB 类型。
>
>   ​`bVersion`
>
>   包含密钥 BLOB 格式的版本号。
>
>   ​`reserved`
>
>   此成员保留供将来使用，必须设置为零。
>
>   ​`aiKeyAlg`
>
>   包含 [一个ALG_ID](https://learn.microsoft.com/zh-cn/windows/desktop/SecCrypto/alg-id) 值，该值标识密钥 BLOB 包含的密钥的算法。
>
>   PLAINTEXTB BLOB 可以与使用中的 CSP 支持的任意算法或组合键类型一起使用。

对照IDA地址，在x64dbg对应位置下断点。

![](/assets/img/media/2026_5_19/image-20260516140326-z2x0cej.png)

![](/assets/img/media/2026_5_19/image-20260516140344-ymwpnum.png)

这之后，对照IDA部分，找到pbData对应的汇编，去内存看BLOB。同时，`dwDatalen`存于R8，R8寄存器值为0x2C，说明BLOB长度为44。

![](/assets/img/media/2026_5_19/image-20260516140433-x3lmoyt.png)

## 分析BLOB，获取加密类型和密钥

```text
000000000014FED8  08 02 00 00  10 66 00 00  20 00 00 00  00 01 02 03  .....f.. .......  
000000000014FEE8  04 05 06 07  08 09 0A 0B  0C 0D 0E 0F  47 11 7B 13  ............G.{.  
000000000014FEF8  7A 15 72 17  77 19 6D 1B  6F 1D 3E 1F  00 00 00 00  z.r.w.m.o.>.....  
```

然后根据先前的结构体解析对应部分内容，`bType`是0x08，对应的是会话密钥。

这里的`aiKeyAlg`​对应为偏移5~8的部分。内存用小端序，大端序拼好是`0x00006610`，用的是AES256。

而整体BLOB结构是PLAINTEXTKEYBLOB，包含了BLOBHEADER。根据此结构体，这个KeySize是0x20，也就是剩下的32位。

> - [ALG_ID](https://learn.microsoft.com/zh-cn/windows/win32/seccrypto/alg-id)
>
>   部分内容
>
>   |标识符|价值|DESCRIPTION|
>   | -----------------------| ------------| -----------------------------------------|
>   |CALG\_3DES|0x00006603|[三重 DES](https://learn.microsoft.com/zh-cn/windows/win32/SecGloss/t-gly) 加密算法。|
>   |CALG\_3DES\_112|0x00006609|双密钥 [三重 DES](https://learn.microsoft.com/zh-cn/windows/win32/SecGloss/t-gly) 加密，有效密钥长度等于 112 位。|
>   |CALG\_AES|0x00006611|高级加密标准（AES）。 [Microsoft AES 加密提供程序](https://learn.microsoft.com/zh-cn/windows/win32/seccrypto/microsoft-aes-cryptographic-provider)支持此算法。|
>   |CALG\_AES\_128|0x0000660e|128 位 AES。 [Microsoft AES 加密提供程序](https://learn.microsoft.com/zh-cn/windows/win32/seccrypto/microsoft-aes-cryptographic-provider)支持此算法。|
>   |CALG\_AES\_192|0x0000660f|192 位 AES。 [Microsoft AES 加密提供程序](https://learn.microsoft.com/zh-cn/windows/win32/seccrypto/microsoft-aes-cryptographic-provider)支持此算法。|
>   |CALG\_AES\_256|0x00006610|256 位 AES。 [Microsoft AES 加密提供程序](https://learn.microsoft.com/zh-cn/windows/win32/seccrypto/microsoft-aes-cryptographic-provider)支持此算法。|
>
> - [PLAINTEXTKEYBLOB structure](https://learn.microsoft.com/zh-cn/previous-versions/windows/desktop/legacy/jj650836(v=vs.85))
>
>   ```cpp
>   typedef struct _PLAINTEXTKEYBLOB {
>     BLOBHEADER hdr;
>     DWORD      dwKeySize;
>     BYTE       rgbKeyData[];
>   } PLAINTEXTKEYBLOB, *PPLAINTEXTKEYBLOB;
>   ```
>
>   **Members**
>
>   - **hdr**  
>     A **PUBLICKEYSTRUC** that indicates the type of BLOB and the algorithm that the key uses.
>   - **dwKeySize**  
>     The size, in bytes, of the key material.
>   - **rgbKeyData[]**   
>     The key material.

## 获取IV

由于AES256需要初始化向量IV，还要获取这个东西。MS的文档实在是难看，不如反向思考，去找怎么用wincrypt.h的aes256。

然后就能找到[采用WinCrypt和CryptImportKey的硬编码AES-256key](https://cloud.tencent.com/developer/ask/sof/102561804)，里面写了

```cpp
if (CryptImportKey(hCryptProv, (BYTE*)&blob, sizeof(aes256keyBlob), NULL, 0, &hKey))
{
    if(CryptSetKeyParam(hKey, KP_IV, myIV, 0))
    {
        //do decryption here
    }
    else{/*error*/}

    CryptDestroyKey(hKey);
}
else{/*error*/}
```

所以可以知道第三个参数就是被设置的初始化向量。

同理，去`CryptSetKeyParam`下断点，R8对应的内存地址存储了16字节的IV。

> - [CryptSetKeyParam 函数 （wincrypt.h）](https://learn.microsoft.com/zh-cn/windows/win32/api/wincrypt/nf-wincrypt-cryptsetkeyparam)
>
>   ```cpp
>   BOOL CryptSetKeyParam(
>     [in] HCRYPTKEY  hKey,
>     [in] DWORD      dwParam,
>     [in] const BYTE *pbData,
>     [in] DWORD      dwFlags
>   );
>   ```
>
>   ​`[in] dwFlags`
>
>   仅在 *dwParam* KP\_ALGID时才使用。 *dwFlags* 参数用于传入已启用密钥的标志值。 *dwFlags* 参数可以保存值，如密钥大小和其他标志值，在使用 [CryptGenKey](https://learn.microsoft.com/zh-cn/windows/desktop/api/wincrypt/nf-wincrypt-cryptgenkey)生成相同类型的键时允许的值。 有关允许的标志值的信息，请参阅 **CryptGenKey**。
> - [CRYPT_AES_256_KEY_STATE 结构 (wincrypt.h)](https://learn.microsoft.com/zh-cn/windows/win32/api/wincrypt/ns-wincrypt-crypt_aes_256_key_state)
>
>   ```cpp
>   typedef struct _CRYPT_AES_256_KEY_STATE {
>     unsigned char Key[32];
>     unsigned char IV[16];
>     unsigned char EncryptionState[15][16];
>     unsigned char DecryptionState[15][16];
>     unsigned char Feedback[16];
>   } CRYPT_AES_256_KEY_STATE, *PCRYPT_AES_256_KEY_STATE;
>   ```

![](/assets/img/media/2026_5_19/image-20260516141901-gxtukkb.png)

![](/assets/img/media/2026_5_19/image-20260516150216-lunoih7.png)

这一行就是IV。

```text
000000014003C020  FF EE DD CC BB AA 99 88 77 66 55 44 33 22 11 00  ÿîÝÌ»ª..wfUD3"..  
```

## 解密

还需要获取密文。IDA里面只是根据这一行`if ( pdwDataLen == byte_14003C018[6] - unk_14003C00E && !memcmp(Buf1, &Buf2_, byte_14003C018[6] - unk_14003C00E) )`​里面的`byte_14003C018[6] - unk_14003C00E`去算长度很显然不对，还是动调确认一下。

随便输点东西，相同的方法断点

![](/assets/img/media/2026_5_19/image-20260516151346-3uvq503.png)

![](/assets/img/media/2026_5_19/image-20260516151410-xhi4eug.png)

这和IDA直接识别密文数组长度大小是一样的，为0x30。

```py
from Crypto.Cipher import AES
from Crypto.Util.Padding import unpad
key = bytearray.fromhex("00 01 02 03 04 05 06 07  08 09 0A 0B  0C 0D 0E 0F  47 11 7B 13 7A 15 72 17  77 19 6D 1B  6F 1D 3E 1F")
iv = bytearray.fromhex("FF EE DD CC BB AA 99 88 77 66 55 44 33 22 11 00")
cip = bytearray([0xde, 0x58, 0x46, 0x70, 0xeb, 0xc7, 0x47, 0x6, 0x62, 0xea, 0xf6, 0x49, 0x3, 0xb8, 0x8e, 0x35, 0xf1, 0xf8, 0x8f, 0x4e, 0x18, 0xd0, 0x8b, 0xd2, 0x5c, 0xf3, 0x53, 0x2f, 0x9f, 0x36, 0xe4, 0x99, 0xbc, 0xe0, 0x15, 0xcf, 0x95, 0x54, 0x36, 0xd3, 0x8f, 0x9f, 0x6, 0x8f, 0xcf, 0x8b, 0x3f, 0xb])
aes = AES.new(key, AES.MODE_CBC, iv)
plaintext = unpad(aes.decrypt(cip),16)
print(plaintext.decode())
```

最后套上格式，获得flag：`flag{M1cr0SOf7_V5_C0dE,d0_Y0U_Kn0W??}`

#
