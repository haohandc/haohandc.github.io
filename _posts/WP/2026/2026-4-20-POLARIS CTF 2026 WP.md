---
title: POLARIS CTF 2026 WP
date: "2026-03-31T15:23:10+08:00"
lastmod: "2026-04-20T12:06:46+08:00"
desciption: 实际比赛中我只做出来了一小部分。赛后复盘发现是我知识掌握有问题，简单的题也没做出来。之后应该不会了。还有不少题我一时半会儿应该是搞不定的。
categories: [WP,"2026"]
tags: [逆向]
---

# POLARIS CTF 2026 WP

部分题目参考以下WP：

[第一届 Polaris CTF 招新赛 Reverse wp](https://xz.aliyun.com/news/91875)

[https://www.sanjiuctf.cn/?p=3220](https://www.sanjiuctf.cn/?p=3220)

[AIL0的WP](https://www.cnblogs.com/AIL0)

# illusion

​#RC4#​ #AES#​ #AES128ECB#​ #hook#

## fake flag

以这道题为例子，只是看 main 函数，只会得到 fake flag。

？能看到的是一个 RC4，但是解密后获得的 flag 有一个不可见字符，比要求的少一个。

```C
int __fastcall main(int argc, const char **argv, const char **envp)
{
  __int64 v3; // r9
  char **argv_1; // rdx
  const char **envp_1; // r8
  __int64 v6; // r9
  __int64 n25; // rax
  char **argv_2; // rdx
  const char **envp_2; // r8
  __int64 v10; // r9
  unsigned __int64 v12; // [rsp+20h] [rbp-60h] BYREF
  int n1045879079; // [rsp+28h] [rbp-58h]
  unsigned int v14; // [rsp+2Ch] [rbp-54h]
  __int16 n21529; // [rsp+30h] [rbp-50h]
  char v16; // [rsp+32h] [rbp-4Eh]
  char nev_gona_give_up[24]; // [rsp+38h] [rbp-48h] BYREF
  char inp[25]; // [rsp+50h] [rbp-30h] BYREF

  sub_7FF7BD0A1930("w3lc0me to the Re w0r1d.\nP1z input your flag: ", argv, envp, v3);
  strcpy(nev_gona_give_up, "nev_gona_give_up");
  sub_7FF7BD0A1990("%25s", inp);
  if ( *inp != 'mx' )
    goto LABEL_12;
  if ( inp[2] != 'c' )
    goto LABEL_12;
  if ( inp[3] != 't' )
    goto LABEL_12;
  if ( inp[4] != 'f' )
    goto LABEL_12;
  if ( inp[5] != '{' )
    goto LABEL_12;
  if ( inp[24] != '}' )
    goto LABEL_12;
  n25 = -1;
  do
    ++n25;
  while ( inp[n25] );
  if ( n25 != 25 )
  {
LABEL_12:
    sub_7FF7BD0A1930(Text, argv_1, envp_1, v6);
    exit(0);
  }
  v12 = 0xE72C8F0A84FB0AD5uLL;
  v16 = 0;
  n1045879079 = 0x3E56D927;
  v14 = 0xAB296CF3;
  n21529 = 0x5419;
  strncpy(
    Destination,                                // "aaabbbaaabbbaaabbb"
    &inp[6],
    18u);
  unk_7FF7BD0C8B12 = 0;                         // "aaabbbaaabbbaaabbb"
  if ( !checkFlag(
          Destination,                          // "aaabbbaaabbbaaabbb"
          nev_gona_give_up,
          &v12) )
  {
    MessageBoxA(0, Text, "Illusion", 0);
    exit(0);
  }
  sub_7FF7BD0A1930(&w3lc0me_to_the_Re_w0r1d__nP1z_input_your_flag___, argv_2, envp_2, v10);
  return 0;
}
```

加密部分

```C
char __fastcall checkFlag(char *Destination, char *key, __int64 cip_1)
{
  __int64 j; // rbx
  double (__cdecl *j_0)(double); // r9
  __int64 lenOfCip; // r10
  __m128i si128; // xmm2
  char *v10; // rdx
  __m128 v11; // xmm3
  unsigned int n8; // ecx
  __int64 n256; // rbp
  unsigned int v14; // eax
  __m128i v15; // xmm0
  __m128i v16; // xmm1
  __m128i v17; // xmm0
  __m128i v18; // xmm1
  __m128i v19; // xmm0
  __m128i v20; // xmm0
  __m128i v21; // xmm0
  __m128i v22; // xmm1
  __m128i v23; // xmm1
  unsigned __int8 *v24; // r8
  unsigned __int64 i0; // rdi
  unsigned __int64 counter1; // rcx
  int v27; // esi
  unsigned __int8 v28; // cl
  unsigned __int8 i2; // r11
  int i; // r9d
  _BYTE *v31; // rcx
  int v32; // r8d
  char sboxValue; // al
  __int64 i2_0; // r8
  unsigned __int8 i_1; // dl
  _BYTE flag[32]; // [rsp+0h] [rbp-148h]
  _BYTE sbox[4]; // [rsp+20h] [rbp-128h] BYREF
  char v39; // [rsp+24h] [rbp-124h] BYREF

  LODWORD(j) = 0;
  LODWORD(j_0) = 0;
  lenOfCip = -1;
  do
    ++lenOfCip;
  while ( Destination[lenOfCip] );
  si128 = _mm_load_si128(&xmmword_7FF7BD0BD420);
  v10 = &v39;
  v11 = _mm_load_si128(&xmmword_7FF7BD0BD430);
  n8 = 8;
  n256 = 256;
  do
  {
    v10 += 16;
    v14 = n8 + 4;
    v15 = _mm_and_ps(_mm_add_epi32(_mm_shuffle_epi32(_mm_cvtsi32_si128(n8 - 8), 0), si128), v11);
    v16 = _mm_and_ps(_mm_add_epi32(_mm_shuffle_epi32(_mm_cvtsi32_si128(n8 - 4), 0), si128), v11);
    v17 = _mm_packus_epi16(v15, v15);
    v18 = _mm_packus_epi16(v16, v16);
    *(v10 - 5) = _mm_cvtsi128_si32(_mm_packus_epi16(v17, v17));
    *(v10 - 4) = _mm_cvtsi128_si32(_mm_packus_epi16(v18, v18));
    v19 = _mm_cvtsi32_si128(n8);
    n8 += 16;
    v20 = _mm_and_ps(_mm_add_epi32(_mm_shuffle_epi32(v19, 0), si128), v11);
    v21 = _mm_packus_epi16(v20, v20);
    v22 = _mm_and_ps(_mm_add_epi32(_mm_shuffle_epi32(_mm_cvtsi32_si128(v14), 0), si128), v11);
    *(v10 - 3) = _mm_cvtsi128_si32(_mm_packus_epi16(v21, v21));
    v23 = _mm_packus_epi16(v22, v22);
    *(v10 - 2) = _mm_cvtsi128_si32(_mm_packus_epi16(v23, v23));
  }
  while ( (n8 - 8) < 256 );
  v24 = sbox;
  i0 = 0;
  do
  {
    counter1 = -1;
    do
      ++counter1;
    while ( key[counter1] );
    v27 = *v24;
    LODWORD(j_0) = (v27 + key[i0 % counter1] + j_0) % 256;
    ++i0;
    v28 = sbox[j_0];
    sbox[j_0] = v27;
    *v24++ = v28;
    --n256;
  }
  while ( n256 );
  i2 = 0;
  i = 0;
  if ( lenOfCip <= 0 )
    return 0;
  do
  {
    i = (i + 1) % 256;
    v31 = &sbox[i];
    v32 = *v31;
    j = (j + v32) % 256;
    sboxValue = sbox[j];
    sbox[j] = v32;
    *v31 = sboxValue;
    i2_0 = i2++;
    flag[i2_0] = Destination[i2_0] ^ sbox[(sbox[j] + sboxValue)];
  }
  while ( i2 < lenOfCip );
  i_1 = 0;
  while ( flag[i_1] == *(i_1 + cip_1) )
  {
    if ( ++i_1 >= lenOfCip )
      return 0;
  }
  return 1;
}
```

这里是一个标准的 RC4 加密的实现。密文密钥都已给出。套的模板。

```python
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
        plain_byte = cipher[k] ^ s_val  
        plain.append(plain_byte)  
    return bytes(plain)  
  
  
  
cipherbe = "D5 0A FB 84 0A 8F 2C E7 27 D9 56 3E F3 6C 29 AB 19 54"  
  
cipher = bytes.fromhex(cipherbe)  
key = b"nev_gona_give_up"  
  
flag = rc4_decrypt(cipher, key)  
print("xmctf{"+flag.decode()+"}")
```

<iframe src="/widgets/run-python-code/" data-subtype="widget" border="0" frameborder="no" framespacing="0" allowfullscreen="true"></iframe>

## 找本题的 hook

上述代码段得到的 fake flag 长度不对，提交也不对。这表明这道题存在 hook 部分，我们看到的不是真正的加密逻辑。

函数列表中没有 TLS 回调函数等。所以找初始化函数。

### 寻找初始化函数

上述跑完后得到的是 fake flag。主函数中不存在其他东西，说明本题存在 hook。不在主函数中，在 CRT 初始化函数表时 hook 了某些东西。

- 简单来说，CRT（C Runtime Library，C 运行时库）的启动代码会在程序入口点（通常是 `start`​ 或 `mainCRTStartup`​）执行，并在调用 `main` 函数之前完成以下工作：

  1. ​**初始化 C 运行时库**：建立程序运行所必需的基础环境。
  2. ​**调用全局/静态对象的构造函数**​：对于 C++ 程序，所有在 `main` 函数之前定义的全局对象和静态对象都需要先被构造出来。
  3. **调用用户提供的** **​`main`​**​**函数**

去找一下初始化函数（一般为 start）

```cpp
__int64 start()
{
  _security_init_cookie();
  return __scrt_common_main_seh();
}
```

补充一下 security cookie 有什么用

- ​`__security_init_cookie`​ 是微软 C/C++ 编译器提供的一个安全机制的初始化函数。它的核心作用是​**在程序启动时，生成一个难以预测的随机数值（称为“安全 Cookie”）** ，用于后续在运行时检测和阻止栈缓冲区溢出攻击。

  这个机制通过编译选项 `/GS`​（缓冲区安全检查）启用，并在大多数 MSVC 项目的 Release 配置中**默认开启**。

看 CRT 初始化函数

```cpp
__int64 __fastcall __scrt_common_main_seh()
{
  unsigned int v0; // ebx
  __int64 v1; // rcx
  char v2; // si
  __int64 v3; // rcx
  __int64 v5; // rcx
  _QWORD *v6; // rax
  __int64 v7; // rcx
  void (__fastcall **v8)(_QWORD, __int64); // rbx
  _tls_callback_type *v9; // rax
  __int64 v10; // rcx
  _tls_callback_type *v11; // rbx
  const char **v12; // rdi
  __int64 v13; // rcx
  const char **v14; // rbx
  __int64 v15; // rcx
  int *v16; // rax
  __int64 v17; // rcx
  __int64 v18; // rcx

  if ( !_scrt_initialize_crt(1) )
  {
    _scrt_fastfail(7);
    goto LABEL_19;
  }
  v2 = 0;
  LOBYTE(v0) = _scrt_acquire_startup_lock(v1);
  v3 = dword_1400290A0;
  if ( dword_1400290A0 == 1 )
  {
LABEL_19:
    _scrt_fastfail(7);
    goto LABEL_20;
  }
  if ( dword_1400290A0 )
  {
    v2 = 1;
  }
  else
  {
    dword_1400290A0 = 1;
    if ( initterm_e(&qword_14001D2E0, &qword_14001D318) )
      return 255;
    initterm(&First, &Last);
    dword_1400290A0 = 2;
  }
  LOBYTE(v3) = v0;
  _scrt_release_startup_lock(v3);
  v6 = sub_140002A14(v5);
  v8 = v6;
  if ( *v6 && _scrt_is_nonwritable_in_current_image(v6) )
    (*v8)(0, 2);
  v9 = sub_140002A1C(v7);
  v11 = v9;
  if ( *v9 && _scrt_is_nonwritable_in_current_image(v9) )
    register_thread_local_exe_atexit_callback(*v11);
  v12 = unknown_libname_34(v10);
  v14 = *sub_14000DF50(v13);
  v16 = sub_14000DF48(v15);
  v0 = main(*v16, v14, v12);
  if ( !_scrt_is_managed_app(v17) )
LABEL_20:
    exit(v0);
  if ( !v2 )
    cexit();
  LOBYTE(v18) = 1;
  _scrt_uninitialize_crt(v18, 0);
  return v0;
}
```

---

### 在 `__scrt_common_main_seh` 中发现 Hook 安装点

​`__scrt_common_main_seh` 是 CRT 的入口函数，它会执行：

- 初始化 C 运行时库
- 调用 `initterm`​ / `initterm_e` 处理全局/静态对象的构造函数和函数指针表

查看这一部分

```cpp
  else
  {
    dword_1400290A0 = 1;
    if ( initterm_e(&qword_14001D2E0, &qword_14001D318) )
      return 255;
    initterm(&First, &Last);// 遍历从 First 到 Last 的函数指针并调用
    dword_1400290A0 = 2;
  }
```

再去看 `First`​ 和 `Last` 指针

```x86asm
.rdata:000000014001D2C0 00 00 00 00 00 00 00 00           First dq 0                              ; DATA XREF: __scrt_common_main_seh(void)+75↑o
.rdata:000000014001D2C8 88 22 00 40 01 00 00 00           dq offset ?pre_cpp_initialization@@YAXXZ ; pre_cpp_initialization(void)
.rdata:000000014001D2D0 00 10 00 40 01 00 00 00           dq offset sub_140001000
.rdata:000000014001D2D8                                   ; const _PVFV Last
.rdata:000000014001D2D8 00 00 00 00 00 00 00 00           Last dq 0   
```

​`sub_140001000`​ 修改了 `MessageBoxA`​ 的前几条指令，实现 inline hook。  
因此，**在** **​`main`​**​ **函数执行前，hook 已经被安装好了**。

### 查看 hook 的部分

```cpp
int sub_140001000()
{
  int (__stdcall *MessageBoxA)(HWND, LPCSTR, LPCSTR, UINT); // rax
  _DWORD *v1; // rcx
  __int128 v3; // [rsp+20h] [rbp-18h]
  DWORD flOldProtect; // [rsp+40h] [rbp+8h] BYREF

  MessageBoxA = NtCurrentPeb();
  if ( (*(MessageBoxA + 188) & 0x70) == 0 )
  {
    MessageBoxA = GetModuleHandleA("user32.dll");
    if ( MessageBoxA )
    {
      MessageBoxA = GetProcAddress(MessageBoxA, "MessageBoxA");
      lpAddress = MessageBoxA;
      if ( MessageBoxA )
      {
        VirtualProtect(MessageBoxA, 0xEu, 0x40u, &flOldProtect);
        v1 = lpAddress;
        LOWORD(v3) = -18360;
        WORD5(v3) = -7937;
        qword_140028AE8 = *lpAddress;
        dword_140028AF0 = *(lpAddress + 2);
        word_140028AF4 = *(lpAddress + 6);
        qword_140028AF8 = lpAddress;
        *(&v3 + 2) = sub_1400010F0;
        *lpAddress = v3;
        v1[2] = DWORD2(v3);
        *(v1 + 6) = 0;
        LODWORD(MessageBoxA) = VirtualProtect(lpAddress, 0xEu, flOldProtect, &flOldProtect);
      }
    }
  }
  return MessageBoxA;
}
```

## 实际加密函数

我们找到被 hook 进去的函数

```cpp
__int64 __fastcall sub_1400010F0(__int64 a1, char *a2, const char *a3, unsigned int a4)
{
  _BYTE *v8; // r14
  _BYTE *v9; // rdi
  __int64 v10; // rsi
  bool v11; // r8
  bool v12; // cl
  bool v13; // dl
  bool v14; // al
  bool v15; // cl
  bool v16; // al
  bool v17; // dl
  bool v18; // al
  bool v19; // cl
  bool v20; // al
  bool v21; // dl
  bool v22; // al
  bool v23; // cl
  bool v24; // al
  bool v25; // dl
  bool v26; // al
  bool v27; // cl
  bool v28; // al
  bool v29; // dl
  bool v30; // al
  bool v31; // cl
  bool v32; // al
  bool v33; // dl
  bool v34; // al
  bool v35; // cl
  bool v36; // al
  bool v37; // dl
  bool v38; // al
  bool v39; // cl
  bool v40; // al
  bool v41; // dl
  bool v42; // al
  bool v43; // cl
  bool v44; // al
  bool v45; // dl
  bool v46; // al
  bool v47; // cl
  bool v48; // al
  bool v49; // dl
  bool v50; // cl
  bool v51; // al
  bool v52; // dl
  bool v53; // al
  bool v54; // cl
  bool v55; // al
  bool v56; // dl
  bool v57; // al
  bool v58; // cl
  bool v59; // al
  bool v60; // dl
  bool v61; // al
  bool v62; // cl
  bool v63; // al
  bool v64; // dl
  bool v65; // al
  bool v66; // cl
  bool v67; // al
  bool v68; // dl
  char *v69; // rcx
  char v70; // al
  _DWORD *v71; // r10
  DWORD flOldProtect; // [rsp+20h] [rbp-E0h] BYREF
  __int128 Src; // [rsp+28h] [rbp-D8h] BYREF
  __int16 v75; // [rsp+38h] [rbp-C8h]
  _BYTE v76[192]; // [rsp+40h] [rbp-C0h] BYREF
  _DWORD v77[4]; // [rsp+100h] [rbp+0h] BYREF
  _OWORD v78[4]; // [rsp+110h] [rbp+10h] BYREF

  v77[0] = 0x34123412;
  v77[1] = 0x34123412;
  v77[2] = 0x34123412;
  Src = *Destination;
  v77[3] = 0x21534541;
  v75 = word_140028B10;
  v8 = (sub_140001690)(&Src);
  sub_1400019F0(v76, v77);
  v9 = v8;
  v10 = 2;
  do
  {
    sub_1400019E0(v76, v9);
    v9 += 16;
    --v10;
  }
  while ( v10 );
  v11 = 0;
  v12 = 0;
  v13 = 0;
  if ( v8[1] == 123 )
    v12 = *v8 == 0xF2;
  v14 = v12;
  v15 = 0;
  if ( v8[2] == 126 )
    v13 = v14;
  v16 = v13;
  v17 = 0;
  if ( v8[3] == 117 )
    v15 = v16;
  v18 = v15;
  v19 = 0;
  if ( v8[4] == 0xB4 )
    v17 = v18;
  v20 = v17;
  v21 = 0;
  if ( v8[5] == 92 )
    v19 = v20;
  v22 = v19;
  v23 = 0;
  if ( v8[6] == 8 )
    v21 = v22;
  v24 = v21;
  v25 = 0;
  if ( v8[7] == 0xFA )
    v23 = v24;
  v26 = v23;
  v27 = 0;
  if ( v8[8] == 25 )
    v25 = v26;
  v28 = v25;
  v29 = 0;
  if ( v8[9] == 60 )
    v27 = v28;
  v30 = v27;
  v31 = 0;
  if ( v8[10] == 0x8A )
    v29 = v30;
  v32 = v29;
  v33 = 0;
  if ( v8[11] == 74 )
    v31 = v32;
  v34 = v31;
  v35 = 0;
  if ( v8[12] == 4 )
    v33 = v34;
  v36 = v33;
  v37 = 0;
  if ( v8[13] == 0xF8 )
    v35 = v36;
  v38 = v35;
  v39 = 0;
  if ( v8[14] == 31 )
    v37 = v38;
  v40 = v37;
  v41 = 0;
  if ( v8[15] == 103 )
    v39 = v40;
  v42 = v39;
  v43 = 0;
  if ( v8[16] == 27 )
    v41 = v42;
  v44 = v41;
  v45 = 0;
  if ( v8[17] == 5 )
    v43 = v44;
  v46 = v43;
  v47 = 0;
  if ( v8[18] == 0x9C )
    v45 = v46;
  v48 = v45;
  v49 = 0;
  if ( v8[19] == 0xE7 )
    v47 = v48;
  if ( v8[20] == 39 )
    v49 = v47;
  v50 = 0;
  v51 = v49;
  v52 = 0;
  if ( v8[21] == 64 )
    v50 = v51;
  v53 = v50;
  v54 = 0;
  if ( v8[22] == 120 )
    v52 = v53;
  v55 = v52;
  v56 = 0;
  if ( v8[23] == 109 )
    v54 = v55;
  v57 = v54;
  v58 = 0;
  if ( v8[24] == 40 )
    v56 = v57;
  v59 = v56;
  v60 = 0;
  if ( v8[25] == 0xF6 )
    v58 = v59;
  v61 = v58;
  v62 = 0;
  if ( v8[26] == 0xA8 )
    v60 = v61;
  v63 = v60;
  v64 = 0;
  if ( v8[27] == 0xB8 )
    v62 = v63;
  v65 = v62;
  v66 = 0;
  if ( v8[28] == 6 )
    v64 = v65;
  v67 = v64;
  v68 = 0;
  if ( v8[29] == 0xC6 )
    v66 = v67;
  if ( v8[30] == 0xC5 )
    v68 = v66;
  if ( v8[31] == 81 )
    v11 = v68;
  memset(v78, 0, sizeof(v78));
  if ( v11 )
  {
    *&v78[0] = 0xE6D5E9B9C3BBD1D0uLL;
    a3 = "real world";
    WORD4(v78[0]) = 0xB3BE;
    BYTE10(v78[0]) = 0;
  }
  else
  {
    v69 = (v78 - a2);
    do
    {
      v70 = *a2;
      v69[a2] = *a2;
      ++a2;
    }
    while ( v70 );
  }
  VirtualProtect(lpAddress, 0xEu, 0x40u, &flOldProtect);
  v71 = lpAddress;
  *lpAddress = qword_140028AE8;
  v71[2] = dword_140028AF0;
  *(v71 + 6) = word_140028AF4;
  VirtualProtect(lpAddress, 0xEu, flOldProtect, &flOldProtect);
  return qword_140028AF8(a1, v78, a3, a4);
}
```

核心部分

```cpp
  v77[0] = 0x34123412;
  v77[1] = 0x34123412;
  v77[2] = 0x34123412;
  Src = *Destination;
  v77[3] = 0x21534541;
  v75 = word_140028B10;
  v8 = sub_140001690(&Src, 18);
  sub_1400019F0(v76, v77);
  v9 = v8;
  v10 = 2;
  do
  {
    sub_1400019E0(v76, v9);
    v9 += 16;
    --v10;
  }
  while ( v10 );
```

很容易猜测 `v77`​ 数组就是存放密钥的数组，长度为 16 字节。`v9 += 16;` 说明明文分组为 16 字节。

> ### 1. 分组大小（最直观）
>
> |算法|分组大小（字节）|密钥长度（字节）|
> | ----------| ----------------| ----------------|
> |AES|16|16/24/32|
> |DES / 3DES|8|8 / 24|
> |SM4|16|16|
> |Blowfish|8|4-56（可变）|
> |Twofish|16|1-32（可变）|
> |IDEA|8|16|
>
> - 看到 **8 字节分组** → 大概率是 DES/3DES/IDEA/Blowfish（但 Blowfish 也支持 8 字节分组，密钥可变）。
> - 看到 **16 字节分组** → 可能是 AES、SM4、Twofish 等。

虽然说能猜出来是 AES，但是核心特征识别的不是很清楚。下表是 AI 的回复

| -                      | 特征                                | sub\_140001A00 的实现 | AES                     | SM4    | 结论 |
| ---------------------- | ----------------------------------- | --------------------- | ----------------------- | ------ |
| 算法结构               | SPN（代换-置换网络）-13             | SPN                   | 非平衡 Feistel 网络--25 | AES    |
| 轮数                   | 9 次主循环 + 1 次最终轮 \= 10 轮-13 | 10/12/14 轮           | 32 轮--51               | AES    |
| 列混合 (MixColumns)    | 存在复杂的 MixColumns 运算          | 有                    | 无                      | AES    |
| 行移位 (ShiftRows)     | 存在显式的 ShiftRows 运算           | 有                    | 无                      | AES    |
| 数据块处理方式         | 一次性处理整个 16 字节矩阵-13       | 矩阵变换              | 32 位字运算-23          | AES    |
| S 盒 (S-Box)           | byte\_14001D440（值需进一步验证）   | 256 字节固定值        | 256 字节固定值          | 需验证 |
| 轮密钥加 (AddRoundKey) | \*v26 \^\= v26[v6]（异或操作）      | 有                    | 有                      | 相同   |
| 密钥扩展               | sub\_140001D00（需进一步分析）      | 独立的扩展算法        | 独立的扩展算法          | 需分析 |

其实这里应该直接使用 FindCrypt 插件进行辨别。如右图直接能识别出来这是AES。再结合以上特征可以确定是AES128。

上面`v8`进行比较的部分就能提取出密文。这个可以让AI做。注意密钥是以小端序存储的，要用相同方式读取。

‍

![](/assets/img/media/2026_4_20/image-20260409192640-crixh9e.png)

key:`12 34 12 34 12 34 12 34 12 34 12 34 41 45 53 21`

cip:`F2 7B 7E 75 B4 5C 08 FA 19 3C 8A 4A 04 F8 1F 67 1B 05 9C E7 27 40 78 6D 28 F6 A8 B8 06 C6 C5 51`

可以选择cyberchef直接去解密。

![](/assets/img/media/2026_4_20/image-20260409194731-zm4k4ro.png)

flag:`xmctf{R3a1_w0rld_M47ters}`

# FunPyVM

#VM# #Python#
提供了打包后的exe和一个bin文件，接报后用pylingual反编译main.pyc。

```python
# Decompiled with PyLingual (https://pylingual.io)  
# Internal filename: 'main.py'  
# Bytecode version: 3.13.0rc3 (3571)  
# Source timestamp: 1970-01-01 00:00:00 UTC (0)  
  
import sys  
import os  
import bitstring  
from kernelVM import CustomVM  
if __name__ == '__main__':  
    if getattr(sys, 'frozen', False):  
        current_dir = os.path.dirname(sys.executable)  
    else:  
        current_dir = os.path.dirname(os.path.abspath(__file__))  
    filename = os.path.join(current_dir, 'opcode.bin')  
    try:  
        stream = bitstring.ConstBitStream(filename=filename)  
        bytecode = stream.tobytes()  
    except Exception as e:  
        print('Error: Could not read \'opcode.bin\' in the current directory.')  
        print('Please ensure \'opcode.bin\' is placed next to the executable.')  
        sys.exit(1)  
    vm = CustomVM()  
    print('--- VM Start ---')  
    vm.run(bytecode)  
    print('\n--- VM End ---')
```

对于py题，首先搜一下有没有，考虑import导入的库是出题人自己写的。

kernalVM里面的customVM是自己写的，找了一下找到了相关文件。使用pylingual进行反编译，网页和本地的均出现反编译错误。

- 自己找到的错误：虚拟机执行的while被弄成了if，导致只读取了第一个opcode就退出了，然后结尾的match case的其他情况（defalut）没能反编译出来，raise unknown opcode的命令没被包括进去。

```python
# Decompiled with PyLingual (https://pylingual.io)
# Internal filename: 'kernelVM.py'
# Bytecode version: 3.13.0rc3 (3571)
# Source timestamp: 1970-01-01 00:00:00 UTC (0)

import sys
from random import randint
class CustomVM:
    def __init__(self):
        self.R0 = 0
        self.R1 = 0
        self.PC = 0
        self.heap = {}
        self.heapNum = 61444
        self.first_create_done = False
    def run(self, bytecode):
        # ***<module>.CustomVM.run: Failure: Compilation Error
        self.PC = 0
        length = len(bytecode)
        while self.PC < length:
            opcode = bytecode[self.PC]
            instruction_length = 1
            if opcode in [16, 17, 18, 32, 48, 49, 50, 51, 52, 53, 54, 55, 64, 65, 66, 67, 68, 69, 70, 80, 82]:
                instruction_length = 2
            opnum = bytecode[self.PC + 1] if instruction_length == 2 else 0
            match opcode:
                case 16:#0x10
                    if not self.first_create_done:
                        self.R0 = 0
                        self.first_create_done = True
                        self.heap[0] = [0] * opnum
                        print("init")

                    else:
                        self.R0 = self.heapNum
                        self.heapNum += randint(1, 10)
                        self.heap[self.R0] = [0] * opnum
                        print(f"heapNum += {self.R0}")

                    self.PC += 2
                case 17:#0x11
                    if self.R0 in self.heap and opnum < len(self.heap[self.R0]):
                        self.R1 = self.heap[self.R0][opnum]
                    else:
                        if 0 in self.heap and opnum < len(self.heap[0]):
                            self.R1 = self.heap[0][opnum]
                        else:
                            self.R1 = 0
                    self.PC += 2
                case 18:#0x12
                    if self.R0 in self.heap:
                        if opnum < len(self.heap[self.R0]):
                            self.heap[self.R0][opnum] = self.R1
                        else:
                            if 0 in self.heap and opnum < len(self.heap[0]):
                                self.heap[0][opnum] = self.R1
                            else:
                                raise IndexError(f'VM Memory Access Violation: store {opnum} out of bounds for both chunk 0x{self.R0:X} and fallback chunk 0')
                        self.PC += 2
                    else:
                        raise ValueError(f'VM Segmentation Fault: store {opnum} to unallocated memory pointer 0x{self.R0:X}')
                case 19:#0x13
                    if self.R0 in self.heap and self.R0!= 0:
                        del self.heap[self.R0]
                    self.PC += 1
                case 32:#0x20
                    self.R1 = opnum & 255 # mov
                    self.PC += 2
                case 48:#0x30
                    self.R1 = self.R1 + opnum & 255 # add
                    self.PC += 2
                case 49:#0x31
                    self.R1 = (self.R1 ^ opnum) & 255 # or
                    self.PC += 2
                case 50:#0x32
                    self.R1 = self.R1 >> opnum & 255 # shr
                    self.PC += 2
                case 51:#0x33
                    self.R1 = self.R1 << opnum & 255 #shl
                    self.PC += 2
                case 52:#0x34
                    if self.R0 in self.heap:
                        if opnum < len(self.heap[self.R0]):
                            self.heap[self.R0][opnum] = self.heap[self.R0][opnum] + self.R1 & 255
                        else:
                            raise IndexError(f'VM Memory Access Violation: addin {opnum} out of bounds.')
                        self.PC += 2
                    else:
                        raise ValueError(f'VM Segmentation Fault: addin {opnum} to unallocated memory.')
                case 53:#0x35
                    if self.R0 in self.heap:
                        if opnum < len(self.heap[self.R0]):
                            self.heap[self.R0][opnum] = (self.heap[self.R0][opnum] ^ self.R1) & 255
                        else:
                            raise IndexError(f'VM Memory Access Violation: xorin {opnum} out of bounds.')
                        self.PC += 2
                    else:
                        raise ValueError(f'VM Segmentation Fault: xorin {opnum} to unallocated memory.')
                case 54:#0x36
                    if self.R0 in self.heap:
                        if opnum < len(self.heap[self.R0]):
                            self.heap[self.R0][opnum] = self.heap[self.R0][opnum] >> self.R1 & 255
                        else:
                            raise IndexError(f'VM Memory Access Violation: zyin {opnum} out of bounds.')
                        self.PC += 2
                    else:
                        raise ValueError(f'VM Segmentation Fault: zyin {opnum} to unallocated memory.')
                case 55:#0x37
                    if self.R0 in self.heap:
                        if opnum < len(self.heap[self.R0]):
                            self.heap[self.R0][opnum] = self.heap[self.R0][opnum] << self.R1 & 255
                        else:
                            raise IndexError(f'VM Memory Access Violation: yyin {opnum} out of bounds.')
                        self.PC += 2
                    else:
                        raise ValueError(f'VM Segmentation Fault: yyin {opnum} to unallocated memory.')
                case 56:#0x38
                    self.R1 = 0 if self.R1!= 0 else 1 # not R1
                    self.PC += 1 #
                case 64:#0x40
                    self.R1 = 1 if self.R1 == opnum else 0 # cmp R1,
                    self.PC += 2
                case 65:#0x41
                    self.PC += 2 + opnum # jmp $+2+opnum
                case 66:#0x42
                    if self.R1 == 0:
                        self.PC += 2 + opnum
                    else:
                        self.PC += 2
                case 67:#0x43
                    if self.R1!= 0:
                        self.PC += 2 + opnum
                    else:
                        self.PC += 2
                case 68:#0x44
                    self.PC += 2 - opnum
                case 69:#0x45
                    if self.R1 == 0:
                        self.PC += 2 - opnum
                    else:
                        self.PC += 2
                case 70:#0x46
                    if self.R1!= 0:
                        self.PC += 2 - opnum
                    else:
                        self.PC += 2
                case 80:#0x50
                    if self.R0 in self.heap:
                        if opnum < len(self.heap[self.R0]):
                            temp = self.heap[self.R0][opnum]
                            self.heap[self.R0][opnum] = self.R1
                            self.R1 = temp
                        else:
                            raise IndexError(f'VM Memory Access Violation: swap {opnum} out of bounds.')
                        self.PC += 2
                    else:
                        raise ValueError(f'VM Segmentation Fault: swap {opnum} on unallocated memory.')
                case 81:#0x51
                    self.R0, self.R1 = (self.R1, self.R0)
                    self.PC += 1
                case 82:#0x52
                    if opnum == 1:
                        user_input = input('Input: ')
                        if self.R0 in self.heap:
                            for i, char in enumerate(user_input):
                                if i < len(self.heap[self.R0]):
                                    self.heap[self.R0][i] = ord(char)
                                else:
                                    raise IndexError('VM Memory Access Violation: input string too long for allocated buffer.')
                        else:
                            raise ValueError('VM Segmentation Fault: int 1 (input) to unallocated memory.')
                    else:
                        if opnum == 2 and self.R0 in self.heap:
                                mem_block = self.heap[self.R0]
                                out_chars = []
                                for c in mem_block:
                                    if c == 0:
                                        break
                                    else:
                                        out_chars.append(chr(c))
                                print(''.join(out_chars), flush=True)
                    self.PC += 2
                case _:
                    raise ValueError(f'Unknown opcode 0x{opcode:02X} at PC={self.PC}')# PC -1
if __name__ == '__main__':
    if len(sys.argv) < 2:
        print('Usage: python kernelVM.py <program.bin>')
        sys.exit(1)
    filename = sys.argv[1]
    with open(filename, 'rb') as f:
        bytecode = f.read()
    vm = CustomVM()
    print('--- VM Start ---')
    vm.run(bytecode)
    print('\n--- VM End ---')
```

## 第一个思路（废弃）

​`10 64 10 32 52 01 11 00`

### 0x10 = 16

```Python
                case 16:
                    if not self.first_create_done:
                        self.R0 = 0
                        self.first_create_done = True
                        self.heap[0] = [0] * opnum
                    else:
                        self.R0 = self.heapNum
                        self.heapNum += randint(1, 10)
                        self.heap[self.R0] = [0] * opnum
                    self.PC += 2
```

第一次执行上面的。

### 0x20 = 32

```python
                case 32:
                    self.R1 = opnum & 255
                    self.PC += 2
```

指令长度2，操作数1

​`mov R1, opnum`

然后0x52,0x01未知opcode。

### 0x52 = 82

```python
                case 82:
                    if opnum == 1:
                        user_input = input('Input: ')
                        if self.R0 in self.heap:
                            for i, char in enumerate(user_input):
                                if i < len(self.heap[self.R0]):
                                    self.heap[self.R0][i] = ord(char)
                                else:
                                    raise IndexError('VM Memory Access Violation: input string too long for allocated buffer.')
                        else:
                            raise ValueError('VM Segmentation Fault: int 1 (input) to unallocated memory.')
                    else:
                        if opnum == 2 and self.R0 in self.heap:
                                mem_block = self.heap[self.R0]
                                out_chars = []
                                for c in mem_block:
                                    if c == 0:
                                        break
                                    else:
                                        out_chars.append(chr(c))
                                print(''.join(out_chars), flush=True)
                    self.PC += 2
```

读取用户输入，然后存到堆里。

### 0x40 = 64

```python
                case 64:
                    self.R1 = 1 if self.R1 == opnum else 0
                    self.PC += 2
```

‍

### 0x43 = 67

```python
                case 67:
                    if self.R1!= 0:
                        self.PC += 2 + opnum
                    else:
                        self.PC += 2
```

‍

### 0x11 = 17

```python
                case 17:
                    if self.R0 in self.heap and opnum < len(self.heap[self.R0]):
                        self.R1 = self.heap[self.R0][opnum]
                    else:
                        if 0 in self.heap and opnum < len(self.heap[0]):
                            self.R1 = self.heap[0][opnum]
                        else:
                            self.R1 = 0
                    self.PC += 2
```

### 0x31 = 49

```python
                case 49:
                    self.R1 = (self.R1 ^ opnum) & 255
                    self.PC += 2
```

‍

‍

‍

## 第二个思路

直接修改customVm，每个地方trace一下。借助了一下AI的力量，因为自己对于识别指令还不是很熟，只能识别出部分。

```python
# Decompiled with PyLingual (https://pylingual.io)
# Internal filename: 'kernelVM.py'
# Bytecode version: 3.13.0rc3 (3571)
# Source timestamp: 1970-01-01 00:00:00 UTC (0)

import sys
from random import randint
class CustomVM:
    def __init__(self):
        self.R0 = 0
        self.R1 = 0
        self.PC = 0
        self.heap = {}
        self.heapNum = 61444
        self.first_create_done = False
    def run(self, bytecode):
        # ***<module>.CustomVM.run: Failure: Compilation Error
        self.PC = 0
        length = len(bytecode)
        while self.PC < length:
            opcode = bytecode[self.PC]
            instruction_length = 1
            if opcode in [16, 17, 18, 32, 48, 49, 50, 51, 52, 53, 54, 55, 64, 65, 66, 67, 68, 69, 70, 80, 82]:
                instruction_length = 2
            opnum = bytecode[self.PC + 1] if instruction_length == 2 else 0
            match opcode:
                case 16:#0x10
                    print(f"[PC={self.PC:04X}] ALLOC(0x10) | size={opnum} | R0={self.R0} R1={self.R1}")
                    if not self.first_create_done:
                        self.R0 = 0
                        self.first_create_done = True
                        self.heap[0] = [0] * opnum
                        print("init")
                        print(f"  -> 初始化0号堆, 长度={opnum}")

                    else:
                        self.R0 = self.heapNum
                        self.heapNum += randint(1, 10)
                        self.heap[self.R0] = [0] * opnum
                        print(f"heapNum += {self.R0}")
                        print(f"  -> 分配堆地址=0x{self.R0:X}, 长度={opnum}")

                    self.PC += 2
                case 17:#0x11
                    print(f"[PC={self.PC:04X}] LOAD(0x11) | offset={opnum} | R0=0x{self.R0:X} R1={self.R1}")
                    if self.R0 in self.heap and opnum < len(self.heap[self.R0]):
                        self.R1 = self.heap[self.R0][opnum]
                    else:
                        if 0 in self.heap and opnum < len(self.heap[0]):
                            self.R1 = self.heap[0][opnum]
                        else:
                            self.R1 = 0
                    print(f"  -> 读取结果 R1={self.R1}")
                    self.PC += 2
                case 18:#0x12
                    print(f"[PC={self.PC:04X}] STORE(0x12) | offset={opnum} | R0=0x{self.R0:X} R1={self.R1}")
                    if self.R0 in self.heap:
                        if opnum < len(self.heap[self.R0]):
                            self.heap[self.R0][opnum] = self.R1
                        else:
                            if 0 in self.heap and opnum < len(self.heap[0]):
                                self.heap[0][opnum] = self.R1
                            else:
                                raise IndexError(f'VM Memory Access Violation: store {opnum} out of bounds for both chunk 0x{self.R0:X} and fallback chunk 0')
                        self.PC += 2
                    else:
                        raise ValueError(f'VM Segmentation Fault: store {opnum} to unallocated memory pointer 0x{self.R0:X}')
                case 19:#0x13
                    print(f"[PC={self.PC:04X}] FREE(0x13) | R0=0x{self.R0:X} R1={self.R1}")
                    if self.R0 in self.heap and self.R0!= 0:
                        del self.heap[self.R0]
                        print(f"  -> 释放地址0x{self.R0:X}成功")
                    self.PC += 1
                case 32:#0x20
                    old = self.R1
                    self.R1 = opnum & 255 # mov
                    print(f"[PC={self.PC:04X}] MOV(0x20) | R1 = {opnum} | 旧={old} 新={self.R1}")
                    self.PC += 2
                case 48:#0x30
                    old = self.R1
                    self.R1 = self.R1 + opnum & 255 # add
                    print(f"[PC={self.PC:04X}] ADD(0x30) | R1 += {opnum} | 旧={old} 新={self.R1}")
                    self.PC += 2
                case 49:#0x31
                    old = self.R1
                    self.R1 = (self.R1 ^ opnum) & 255 # or
                    print(f"[PC={self.PC:04X}] XOR(0x31) | R1 ^= {opnum} | 旧={old} 新={self.R1}")
                    self.PC += 2
                case 50:  # 0x32 SHR
                    old = self.R1
                    self.R1 = self.R1 >> opnum & 255
                    print(f"[PC={self.PC:04X}] SHR(0x32) | R1 >>= {opnum} | 旧={old} 新={self.R1}")
                    self.PC += 2
                case 51:  # 0x33 SHL
                    old = self.R1
                    self.R1 = self.R1 << opnum & 255
                    print(f"[PC={self.PC:04X}] SHL(0x33) | R1 <<= {opnum} | 旧={old} 新={self.R1}")
                    self.PC += 2
                case 52:#0x34
                    print(f"[PC={self.PC:04X}] ADDIN(0x34) | [R0+{opnum}] += R1({self.R1}) | R0=0x{self.R0:X}")
                    if self.R0 in self.heap:
                        if opnum < len(self.heap[self.R0]):
                            self.heap[self.R0][opnum] = self.heap[self.R0][opnum] + self.R1 & 255

                        else:
                            raise IndexError(f'VM Memory Access Violation: addin {opnum} out of bounds.')
                        self.PC += 2
                    else:
                        raise ValueError(f'VM Segmentation Fault: addin {opnum} to unallocated memory.')
                case 53:#0x35
                    print(f"[PC={self.PC:04X}] XORIN(0x35) | [R0+{opnum}] ^= R1({self.R1}) | R0=0x{self.R0:X}")

                    if self.R0 in self.heap:
                        if opnum < len(self.heap[self.R0]):
                            self.heap[self.R0][opnum] = (self.heap[self.R0][opnum] ^ self.R1) & 255
                        else:
                            raise IndexError(f'VM Memory Access Violation: xorin {opnum} out of bounds.')
                        self.PC += 2
                    else:
                        raise ValueError(f'VM Segmentation Fault: xorin {opnum} to unallocated memory.')
                case 54:#0x36
                    print(f"[PC={self.PC:04X}] SHRIN(0x35) | [R0+{opnum}] >>= R1({self.R1}) | R0=0x{self.R0:X}")

                    if self.R0 in self.heap:
                        if opnum < len(self.heap[self.R0]):
                            self.heap[self.R0][opnum] = self.heap[self.R0][opnum] >> self.R1 & 255
                        else:
                            raise IndexError(f'VM Memory Access Violation: zyin {opnum} out of bounds.')
                        self.PC += 2
                    else:
                        raise ValueError(f'VM Segmentation Fault: zyin {opnum} to unallocated memory.')
                case 55:#0x37
                    print(f"[PC={self.PC:04X}] SHLIN(0x35) | [R0+{opnum}] <<= R1({self.R1}) | R0=0x{self.R0:X}")

                    if self.R0 in self.heap:
                        if opnum < len(self.heap[self.R0]):
                            self.heap[self.R0][opnum] = self.heap[self.R0][opnum] << self.R1 & 255
                        else:
                            raise IndexError(f'VM Memory Access Violation: yyin {opnum} out of bounds.')
                        self.PC += 2
                    else:
                        raise ValueError(f'VM Segmentation Fault: yyin {opnum} to unallocated memory.')
                case 56:  # 0x38 NOT
                    old = self.R1
                    self.R1 = 0 if self.R1 != 0 else 1
                    print(f"[PC={self.PC:04X}] NOT(0x38) | R1 = !R1 | 旧={old} 新={self.R1}")
                    self.PC += 1
                case 64:  # 0x40 CMP
                    old = self.R1
                    self.R1 = 1 if self.R1 == opnum else 0
                    print(f"[PC={self.PC:04X}] CMP(0x40) | R1 == {opnum} ? 1:0 | 结果={self.R1}")
                    self.PC += 2
                case 65:#0x41
                    new_pc = self.PC + 2 + opnum
                    print(f"[PC={self.PC:04X}] JMP(0x41) | offset={opnum} | 新PC={new_pc:04X}")
                    self.PC = new_pc
                case 66:#0x42
                    new_pc = self.PC + 2 + opnum if self.R1 == 0 else self.PC + 2
                    print(f"[PC={self.PC:04X}] JE(0x42) | R1={self.R1} | 跳转={new_pc:04X}")
                    if self.R1 == 0:
                        self.PC += 2 + opnum
                    else:
                        self.PC += 2
                case 67:#0x43
                    new_pc = self.PC + 2 + opnum if self.R1 == 0 else self.PC + 2
                    print(f"[PC={self.PC:04X}] JNE(0x42) | R1={self.R1} | 跳转={new_pc:04X}")
                    if self.R1!= 0:
                        self.PC += 2 + opnum
                    else:
                        self.PC += 2
                case 68:  # 0x44 JMPB 无条件回跳
                    new_pc = self.PC + 2 - opnum
                    print(f"[PC={self.PC:04X}] JMPB(0x44) | 无条件回跳 | 偏移={opnum} | 新PC={new_pc:04X}")
                    self.PC = new_pc
                case 69:  # 0x45 JEB 相等回跳
                    if self.R1 == 0:
                        new_pc = self.PC + 2 - opnum
                        print(f"[PC={self.PC:04X}] JEB(0x45) | R1=0 → 回跳 | 新PC={new_pc:04X}")
                        self.PC = new_pc
                    else:
                        print(f"[PC={self.PC:04X}] JEB(0x45) | R1≠0 → 不回跳 | PC+2")
                        self.PC += 2
                case 70:  # 0x46 JNEB 不等回跳
                    if self.R1 != 0:
                        new_pc = self.PC + 2 - opnum
                        print(f"[PC={self.PC:04X}] JNEB(0x46) | R1≠0 → 回跳 | 新PC={new_pc:04X}")
                        self.PC = new_pc
                    else:
                        print(f"[PC={self.PC:04X}] JNEB(0x46) | R1=0 → 不回跳 | PC+2")
                        self.PC += 2
                case 80:#0x50
                    print(f"[PC={self.PC:04X}] SWAP(0x50) | [R0+{opnum}] <-> R1 | R0=0x{self.R0:X} R1={self.R1}")
                    if self.R0 in self.heap:
                        if opnum < len(self.heap[self.R0]):
                            temp = self.heap[self.R0][opnum]
                            self.heap[self.R0][opnum] = self.R1
                            self.R1 = temp
                        else:
                            raise IndexError(f'VM Memory Access Violation: swap {opnum} out of bounds.')
                        self.PC += 2
                    else:
                        raise ValueError(f'VM Segmentation Fault: swap {opnum} on unallocated memory.')
                case 81:#0x51
                    print(f"[PC={self.PC:04X}] XCHG(0x51) | R0 <-> R1 | 旧 R0={self.R0} R1={self.R1}")
                    self.R0, self.R1 = (self.R1, self.R0)
                    print(f"  -> 新 R0={self.R0} R1={self.R1}")
                    self.PC += 1
                case 82:#0x52
                    print(f"[PC={self.PC:04X}] INT(0x52) | 中断号={opnum} | R0=0x{self.R0:X}")
                    if opnum == 1:
                        user_input = input('Input: ')
                        if self.R0 in self.heap:
                            for i, char in enumerate(user_input):
                                if i < len(self.heap[self.R0]):
                                    self.heap[self.R0][i] = ord(char)
                                else:
                                    raise IndexError('VM Memory Access Violation: input string too long for allocated buffer.')
                        else:
                            raise ValueError('VM Segmentation Fault: int 1 (input) to unallocated memory.')
                    else:
                        if opnum == 2 and self.R0 in self.heap:
                            print(f"  -> 输出堆0x{self.R0:X}字符串")
                            mem_block = self.heap[self.R0]
                            out_chars = []
                            for c in mem_block:
                                if c == 0:
                                    break
                                else:
                                    out_chars.append(chr(c))
                            print(''.join(out_chars), flush=True)
                    self.PC += 2
                case _:
                    raise ValueError(f'Unknown opcode 0x{opcode:02X} at PC={self.PC}')# PC -1
if __name__ == '__main__':
    if len(sys.argv) < 2:
        print('Usage: python kernelVM.py <program.bin>')
        sys.exit(1)
    filename = sys.argv[1]
    with open(filename, 'rb') as f:
        bytecode = f.read()
    vm = CustomVM()
    print('--- VM Start ---')
    vm.run(bytecode)
    print('\n--- VM End ---')
```

随便输入点东西，看看trace的结果

```
E:\_CTF\模板\.venv\Scripts\python.exe E:\_CTF\题目\polarisctf\FunPyVM\decompiled_main.py 
--- VM Start ---
[PC=0000] ALLOC(0x10) | size=100 | R0=0 R1=0
init
  -> 初始化0号堆, 长度=100
[PC=0002] ALLOC(0x10) | size=50 | R0=0 R1=0
heapNum += 61444
  -> 分配堆地址=0xF004, 长度=50
[PC=0004] INT(0x52) | 中断号=1 | R0=0xF004
Input: 123
[PC=0006] LOAD(0x11) | offset=0 | R0=0xF004 R1=0
  -> 读取结果 R1=49
[PC=0008] CMP(0x40) | R1 == 0 ? 1:0 | 结果=0
[PC=000A] JNE(0x42) | R1=0 | 跳转=0012
[PC=000C] LOAD(0x11) | offset=0 | R0=0xF004 R1=0
  -> 读取结果 R1=49
[PC=000E] XOR(0x31) | R1 ^= 85 | 旧=49 新=100
[PC=0010] STORE(0x12) | offset=0 | R0=0xF004 R1=100
[PC=0012] LOAD(0x11) | offset=1 | R0=0xF004 R1=100
  -> 读取结果 R1=50
[PC=0014] CMP(0x40) | R1 == 0 ? 1:0 | 结果=0
[PC=0016] JNE(0x42) | R1=0 | 跳转=001E
[PC=0018] LOAD(0x11) | offset=1 | R0=0xF004 R1=0
  -> 读取结果 R1=50
[PC=001A] XOR(0x31) | R1 ^= 85 | 旧=50 新=103
[PC=001C] STORE(0x12) | offset=1 | R0=0xF004 R1=103
[PC=001E] LOAD(0x11) | offset=2 | R0=0xF004 R1=103
  -> 读取结果 R1=51
[PC=0020] CMP(0x40) | R1 == 0 ? 1:0 | 结果=0
[PC=0022] JNE(0x42) | R1=0 | 跳转=002A
[PC=0024] LOAD(0x11) | offset=2 | R0=0xF004 R1=0
  -> 读取结果 R1=51
[PC=0026] XOR(0x31) | R1 ^= 85 | 旧=51 新=102
[PC=0028] STORE(0x12) | offset=2 | R0=0xF004 R1=102
[PC=002A] LOAD(0x11) | offset=3 | R0=0xF004 R1=102
  -> 读取结果 R1=0
[PC=002C] CMP(0x40) | R1 == 0 ? 1:0 | 结果=1
[PC=002E] JNE(0x42) | R1=1 | 跳转=0030
[PC=0036] LOAD(0x11) | offset=4 | R0=0xF004 R1=1
  -> 读取结果 R1=0
[PC=0038] CMP(0x40) | R1 == 0 ? 1:0 | 结果=1
[PC=003A] JNE(0x42) | R1=1 | 跳转=003C
[PC=0042] LOAD(0x11) | offset=5 | R0=0xF004 R1=1
  -> 读取结果 R1=0
[PC=0044] CMP(0x40) | R1 == 0 ? 1:0 | 结果=1
[PC=0046] JNE(0x42) | R1=1 | 跳转=0048
[PC=004E] LOAD(0x11) | offset=6 | R0=0xF004 R1=1
  -> 读取结果 R1=0
[PC=0050] CMP(0x40) | R1 == 0 ? 1:0 | 结果=1
[PC=0052] JNE(0x42) | R1=1 | 跳转=0054
[PC=005A] LOAD(0x11) | offset=7 | R0=0xF004 R1=1
  -> 读取结果 R1=0
[PC=005C] CMP(0x40) | R1 == 0 ? 1:0 | 结果=1
[PC=005E] JNE(0x42) | R1=1 | 跳转=0060
[PC=0066] LOAD(0x11) | offset=8 | R0=0xF004 R1=1
  -> 读取结果 R1=0
[PC=0068] CMP(0x40) | R1 == 0 ? 1:0 | 结果=1
[PC=006A] JNE(0x42) | R1=1 | 跳转=006C
[PC=0072] LOAD(0x11) | offset=9 | R0=0xF004 R1=1
  -> 读取结果 R1=0
[PC=0074] CMP(0x40) | R1 == 0 ? 1:0 | 结果=1
[PC=0076] JNE(0x42) | R1=1 | 跳转=0078
[PC=007E] LOAD(0x11) | offset=10 | R0=0xF004 R1=1
  -> 读取结果 R1=0
[PC=0080] CMP(0x40) | R1 == 0 ? 1:0 | 结果=1
[PC=0082] JNE(0x42) | R1=1 | 跳转=0084
[PC=008A] LOAD(0x11) | offset=11 | R0=0xF004 R1=1
  -> 读取结果 R1=0
[PC=008C] CMP(0x40) | R1 == 0 ? 1:0 | 结果=1
[PC=008E] JNE(0x42) | R1=1 | 跳转=0090
[PC=0096] LOAD(0x11) | offset=12 | R0=0xF004 R1=1
  -> 读取结果 R1=0
[PC=0098] CMP(0x40) | R1 == 0 ? 1:0 | 结果=1
[PC=009A] JNE(0x42) | R1=1 | 跳转=009C
[PC=00A2] LOAD(0x11) | offset=13 | R0=0xF004 R1=1
  -> 读取结果 R1=0
[PC=00A4] CMP(0x40) | R1 == 0 ? 1:0 | 结果=1
[PC=00A6] JNE(0x42) | R1=1 | 跳转=00A8
[PC=00AE] LOAD(0x11) | offset=14 | R0=0xF004 R1=1
  -> 读取结果 R1=0
[PC=00B0] CMP(0x40) | R1 == 0 ? 1:0 | 结果=1
[PC=00B2] JNE(0x42) | R1=1 | 跳转=00B4
[PC=00BA] LOAD(0x11) | offset=15 | R0=0xF004 R1=1
  -> 读取结果 R1=0
[PC=00BC] CMP(0x40) | R1 == 0 ? 1:0 | 结果=1
[PC=00BE] JNE(0x42) | R1=1 | 跳转=00C0
[PC=00C6] LOAD(0x11) | offset=16 | R0=0xF004 R1=1
  -> 读取结果 R1=0
[PC=00C8] CMP(0x40) | R1 == 0 ? 1:0 | 结果=1
[PC=00CA] JNE(0x42) | R1=1 | 跳转=00CC
[PC=00D2] LOAD(0x11) | offset=17 | R0=0xF004 R1=1
  -> 读取结果 R1=0
[PC=00D4] CMP(0x40) | R1 == 0 ? 1:0 | 结果=1
[PC=00D6] JNE(0x42) | R1=1 | 跳转=00D8
[PC=00DE] LOAD(0x11) | offset=18 | R0=0xF004 R1=1
  -> 读取结果 R1=0
[PC=00E0] CMP(0x40) | R1 == 0 ? 1:0 | 结果=1
[PC=00E2] JNE(0x42) | R1=1 | 跳转=00E4
[PC=00EA] LOAD(0x11) | offset=19 | R0=0xF004 R1=1
  -> 读取结果 R1=0
[PC=00EC] CMP(0x40) | R1 == 0 ? 1:0 | 结果=1
[PC=00EE] JNE(0x42) | R1=1 | 跳转=00F0
[PC=00F6] LOAD(0x11) | offset=20 | R0=0xF004 R1=1
  -> 读取结果 R1=0
[PC=00F8] CMP(0x40) | R1 == 0 ? 1:0 | 结果=1
[PC=00FA] JNE(0x42) | R1=1 | 跳转=00FC
[PC=0102] LOAD(0x11) | offset=21 | R0=0xF004 R1=1
  -> 读取结果 R1=0
[PC=0104] CMP(0x40) | R1 == 0 ? 1:0 | 结果=1
[PC=0106] JNE(0x42) | R1=1 | 跳转=0108
[PC=010E] LOAD(0x11) | offset=22 | R0=0xF004 R1=1
  -> 读取结果 R1=0
[PC=0110] CMP(0x40) | R1 == 0 ? 1:0 | 结果=1
[PC=0112] JNE(0x42) | R1=1 | 跳转=0114
[PC=011A] LOAD(0x11) | offset=23 | R0=0xF004 R1=1
  -> 读取结果 R1=0
[PC=011C] CMP(0x40) | R1 == 0 ? 1:0 | 结果=1
[PC=011E] JNE(0x42) | R1=1 | 跳转=0120
[PC=0126] LOAD(0x11) | offset=24 | R0=0xF004 R1=1
  -> 读取结果 R1=0
[PC=0128] CMP(0x40) | R1 == 0 ? 1:0 | 结果=1
[PC=012A] JNE(0x42) | R1=1 | 跳转=012C
[PC=0132] LOAD(0x11) | offset=25 | R0=0xF004 R1=1
  -> 读取结果 R1=0
[PC=0134] CMP(0x40) | R1 == 0 ? 1:0 | 结果=1
[PC=0136] JNE(0x42) | R1=1 | 跳转=0138
[PC=013E] LOAD(0x11) | offset=26 | R0=0xF004 R1=1
  -> 读取结果 R1=0
[PC=0140] CMP(0x40) | R1 == 0 ? 1:0 | 结果=1
[PC=0142] JNE(0x42) | R1=1 | 跳转=0144
[PC=014A] LOAD(0x11) | offset=0 | R0=0xF004 R1=1
  -> 读取结果 R1=100
[PC=014C] CMP(0x40) | R1 == 0 ? 1:0 | 结果=0
[PC=014E] JNE(0x42) | R1=0 | 跳转=0156
[PC=0150] LOAD(0x11) | offset=0 | R0=0xF004 R1=0
  -> 读取结果 R1=100
[PC=0152] ADD(0x30) | R1 += 7 | 旧=100 新=107
[PC=0154] STORE(0x12) | offset=0 | R0=0xF004 R1=107
[PC=0156] LOAD(0x11) | offset=1 | R0=0xF004 R1=107
  -> 读取结果 R1=103
[PC=0158] CMP(0x40) | R1 == 0 ? 1:0 | 结果=0
[PC=015A] JNE(0x42) | R1=0 | 跳转=0162
[PC=015C] LOAD(0x11) | offset=1 | R0=0xF004 R1=0
  -> 读取结果 R1=103
[PC=015E] ADD(0x30) | R1 += 10 | 旧=103 新=113
[PC=0160] STORE(0x12) | offset=1 | R0=0xF004 R1=113
[PC=0162] LOAD(0x11) | offset=2 | R0=0xF004 R1=113
  -> 读取结果 R1=102
[PC=0164] CMP(0x40) | R1 == 0 ? 1:0 | 结果=0
[PC=0166] JNE(0x42) | R1=0 | 跳转=016E
[PC=0168] LOAD(0x11) | offset=2 | R0=0xF004 R1=0
  -> 读取结果 R1=102
[PC=016A] ADD(0x30) | R1 += 13 | 旧=102 新=115
[PC=016C] STORE(0x12) | offset=2 | R0=0xF004 R1=115
[PC=016E] LOAD(0x11) | offset=3 | R0=0xF004 R1=115
  -> 读取结果 R1=0
[PC=0170] CMP(0x40) | R1 == 0 ? 1:0 | 结果=1
[PC=0172] JNE(0x42) | R1=1 | 跳转=0174
[PC=017A] LOAD(0x11) | offset=4 | R0=0xF004 R1=1
  -> 读取结果 R1=0
[PC=017C] CMP(0x40) | R1 == 0 ? 1:0 | 结果=1
[PC=017E] JNE(0x42) | R1=1 | 跳转=0180
[PC=0186] LOAD(0x11) | offset=5 | R0=0xF004 R1=1
  -> 读取结果 R1=0
[PC=0188] CMP(0x40) | R1 == 0 ? 1:0 | 结果=1
[PC=018A] JNE(0x42) | R1=1 | 跳转=018C
[PC=0192] LOAD(0x11) | offset=6 | R0=0xF004 R1=1
  -> 读取结果 R1=0
[PC=0194] CMP(0x40) | R1 == 0 ? 1:0 | 结果=1
[PC=0196] JNE(0x42) | R1=1 | 跳转=0198
[PC=019E] LOAD(0x11) | offset=7 | R0=0xF004 R1=1
  -> 读取结果 R1=0
[PC=01A0] CMP(0x40) | R1 == 0 ? 1:0 | 结果=1
[PC=01A2] JNE(0x42) | R1=1 | 跳转=01A4
[PC=01AA] LOAD(0x11) | offset=8 | R0=0xF004 R1=1
  -> 读取结果 R1=0
[PC=01AC] CMP(0x40) | R1 == 0 ? 1:0 | 结果=1
[PC=01AE] JNE(0x42) | R1=1 | 跳转=01B0
[PC=01B6] LOAD(0x11) | offset=9 | R0=0xF004 R1=1
  -> 读取结果 R1=0
[PC=01B8] CMP(0x40) | R1 == 0 ? 1:0 | 结果=1
[PC=01BA] JNE(0x42) | R1=1 | 跳转=01BC
[PC=01C2] LOAD(0x11) | offset=10 | R0=0xF004 R1=1
  -> 读取结果 R1=0
[PC=01C4] CMP(0x40) | R1 == 0 ? 1:0 | 结果=1
[PC=01C6] JNE(0x42) | R1=1 | 跳转=01C8
[PC=01CE] LOAD(0x11) | offset=11 | R0=0xF004 R1=1
  -> 读取结果 R1=0
[PC=01D0] CMP(0x40) | R1 == 0 ? 1:0 | 结果=1
[PC=01D2] JNE(0x42) | R1=1 | 跳转=01D4
[PC=01DA] LOAD(0x11) | offset=12 | R0=0xF004 R1=1
  -> 读取结果 R1=0
[PC=01DC] CMP(0x40) | R1 == 0 ? 1:0 | 结果=1
[PC=01DE] JNE(0x42) | R1=1 | 跳转=01E0
[PC=01E6] LOAD(0x11) | offset=13 | R0=0xF004 R1=1
  -> 读取结果 R1=0
[PC=01E8] CMP(0x40) | R1 == 0 ? 1:0 | 结果=1
[PC=01EA] JNE(0x42) | R1=1 | 跳转=01EC
[PC=01F2] LOAD(0x11) | offset=14 | R0=0xF004 R1=1
  -> 读取结果 R1=0
[PC=01F4] CMP(0x40) | R1 == 0 ? 1:0 | 结果=1
[PC=01F6] JNE(0x42) | R1=1 | 跳转=01F8
[PC=01FE] LOAD(0x11) | offset=15 | R0=0xF004 R1=1
  -> 读取结果 R1=0
[PC=0200] CMP(0x40) | R1 == 0 ? 1:0 | 结果=1
[PC=0202] JNE(0x42) | R1=1 | 跳转=0204
[PC=020A] LOAD(0x11) | offset=16 | R0=0xF004 R1=1
  -> 读取结果 R1=0
[PC=020C] CMP(0x40) | R1 == 0 ? 1:0 | 结果=1
[PC=020E] JNE(0x42) | R1=1 | 跳转=0210
[PC=0216] LOAD(0x11) | offset=17 | R0=0xF004 R1=1
  -> 读取结果 R1=0
[PC=0218] CMP(0x40) | R1 == 0 ? 1:0 | 结果=1
[PC=021A] JNE(0x42) | R1=1 | 跳转=021C
[PC=0222] LOAD(0x11) | offset=18 | R0=0xF004 R1=1
  -> 读取结果 R1=0
[PC=0224] CMP(0x40) | R1 == 0 ? 1:0 | 结果=1
[PC=0226] JNE(0x42) | R1=1 | 跳转=0228
[PC=022E] LOAD(0x11) | offset=19 | R0=0xF004 R1=1
  -> 读取结果 R1=0
[PC=0230] CMP(0x40) | R1 == 0 ? 1:0 | 结果=1
[PC=0232] JNE(0x42) | R1=1 | 跳转=0234
[PC=023A] LOAD(0x11) | offset=20 | R0=0xF004 R1=1
  -> 读取结果 R1=0
[PC=023C] CMP(0x40) | R1 == 0 ? 1:0 | 结果=1
[PC=023E] JNE(0x42) | R1=1 | 跳转=0240
[PC=0246] LOAD(0x11) | offset=21 | R0=0xF004 R1=1
  -> 读取结果 R1=0
[PC=0248] CMP(0x40) | R1 == 0 ? 1:0 | 结果=1
[PC=024A] JNE(0x42) | R1=1 | 跳转=024C
[PC=0252] LOAD(0x11) | offset=22 | R0=0xF004 R1=1
  -> 读取结果 R1=0
[PC=0254] CMP(0x40) | R1 == 0 ? 1:0 | 结果=1
[PC=0256] JNE(0x42) | R1=1 | 跳转=0258
[PC=025E] LOAD(0x11) | offset=23 | R0=0xF004 R1=1
  -> 读取结果 R1=0
[PC=0260] CMP(0x40) | R1 == 0 ? 1:0 | 结果=1
[PC=0262] JNE(0x42) | R1=1 | 跳转=0264
[PC=026A] LOAD(0x11) | offset=24 | R0=0xF004 R1=1
  -> 读取结果 R1=0
[PC=026C] CMP(0x40) | R1 == 0 ? 1:0 | 结果=1
[PC=026E] JNE(0x42) | R1=1 | 跳转=0270
[PC=0276] LOAD(0x11) | offset=25 | R0=0xF004 R1=1
  -> 读取结果 R1=0
[PC=0278] CMP(0x40) | R1 == 0 ? 1:0 | 结果=1
[PC=027A] JNE(0x42) | R1=1 | 跳转=027C
[PC=0282] LOAD(0x11) | offset=26 | R0=0xF004 R1=1
  -> 读取结果 R1=0
[PC=0284] CMP(0x40) | R1 == 0 ? 1:0 | 结果=1
[PC=0286] JNE(0x42) | R1=1 | 跳转=0288
[PC=028E] LOAD(0x11) | offset=0 | R0=0xF004 R1=1
  -> 读取结果 R1=107
[PC=0290] CMP(0x40) | R1 == 41 ? 1:0 | 结果=0
[PC=0292] JE(0x42) | R1=0 | 跳转=0346
[PC=0346] ALLOC(0x10) | size=3 | R0=61444 R1=0
heapNum += 61451
  -> 分配堆地址=0xF00B, 长度=3
[PC=0348] MOV(0x20) | R1 = 78 | 旧=0 新=78
[PC=034A] STORE(0x12) | offset=0 | R0=0xF00B R1=78
[PC=034C] MOV(0x20) | R1 = 111 | 旧=78 新=111
[PC=034E] STORE(0x12) | offset=1 | R0=0xF00B R1=111
[PC=0350] MOV(0x20) | R1 = 0 | 旧=111 新=0
[PC=0352] STORE(0x12) | offset=2 | R0=0xF00B R1=0
[PC=0354] INT(0x52) | 中断号=2 | R0=0xF00B
  -> 输出堆0xF00B字符串
No
[PC=0356] FREE(0x13) | R0=0xF00B R1=0
  -> 释放地址0xF00B成功

--- VM End ---

进程已结束，退出代码为 0

```

观察一下结果，密文应该是27个字节。交给AI去读了一下，存在一个add指令计算总偏移，实际上是检测长度是否符合，不符合跳过比较部分。  
构造27个字节的试了一下xmctf{aaabbbcccaaabbbcccab}

aaaaaaaaaaaaaaaaaaaaaaaaaaa

## 参考他人WP后（延续第二个思路）

### 新的trace方案

重新修改trace流程，可读性更好

第一版（有部分问题）

```py
import sys
from random import randint

class CustomVM:
    def __init__(self):
        self.R0 = 0
        self.R1 = 0
        self.PC = 0
        self.heap = {}
        self.heapNum = 61444
        self.first_create_done = False

    # ---------- 反汇编工具 ----------
    def _disassemble(self, bytecode):
        """生成静态反汇编文本（列表形式）"""
        length = len(bytecode)
        pc = 0
        lines = []
        # 操作码 -> (助记符, 是否有操作数)
        op_info = {
            0x10: ("ALLOC", True),
            0x11: ("LOAD", True),
            0x12: ("STORE", True),
            0x13: ("FREE", False),
            0x20: ("MOVI", True),
            0x30: ("ADDI", True),
            0x31: ("XORI", True),
            0x32: ("SHR", True),
            0x33: ("SHL", True),
            0x34: ("ADD_MEM", True),
            0x35: ("XOR_MEM", True),
            0x36: ("SHR_MEM", True),
            0x37: ("SHL_MEM", True),
            0x38: ("NOT", False),
            0x40: ("CMPEQ", True),
            0x41: ("JMP", True),
            0x42: ("JZ", True),
            0x43: ("JNZ", True),
            0x44: ("JMPB", True),
            0x45: ("JEB", True),
            0x46: ("JNEB", True),
            0x50: ("SWAP", True),
            0x51: ("XCHG", False),
            0x52: ("SYSCALL", True),
        }

        while pc < length:
            opcode = bytecode[pc]
            has_operand = op_info.get(opcode, (None, False))[1]
            mnemonic = op_info.get(opcode, (f"UNKN_{opcode:02X}", False))[0]

            if has_operand and pc + 1 < length:
                operand = bytecode[pc + 1]
                inst_len = 2
            else:
                operand = None
                inst_len = 1

            # 构造基本行：地址 字节序列 助记符 操作数
            if operand is not None:
                hex_bytes = f"{opcode:02X} {operand:02X}"
                op_str = f"0x{operand:02X}"
                line = f"0x{pc:04X}: {hex_bytes:<5} {mnemonic} {op_str}"
            else:
                hex_bytes = f"{opcode:02X}"
                line = f"0x{pc:04X}: {hex_bytes:<5} {mnemonic}"

            # 为跳转指令添加目标地址注释
            if opcode in (0x41, 0x42, 0x43):          # JMP, JZ, JNZ
                target = pc + 2 + operand
                line += f" ; -> 0x{target:04X}"
            elif opcode in (0x44, 0x45, 0x46):        # JMPB, JEB, JNEB
                target = pc + 2 - operand
                line += f" ; -> 0x{target:04X}"
            elif opcode == 0x52:                      # SYSCALL
                if operand == 1:
                    line += " ; READ_INPUT"
                elif operand == 2:
                    line += " ; PRINT_OUTPUT"

            lines.append(line)
            pc += inst_len

        return lines

    # ---------- 虚拟机执行 ----------
    def run(self, bytecode):
        # 1. 输出反汇编列表
        print("; size = {} bytes".format(len(bytecode)))
        print("; instructions = {}".format(len(self._disassemble(bytecode))))
        print()
        for line in self._disassemble(bytecode):
            print(line)
        print("\n--- VM Execution Start ---\n")

        # 2. 执行字节码
        self.PC = 0
        length = len(bytecode)
        while self.PC < length:
            opcode = bytecode[self.PC]
            instruction_length = 1
            if opcode in [16, 17, 18, 32, 48, 49, 50, 51, 52, 53, 54, 55, 64, 65, 66, 67, 68, 69, 70, 80, 82]:
                instruction_length = 2
            opnum = bytecode[self.PC + 1] if instruction_length == 2 else 0

            # ---------- 指令实现（移除原有动态打印，仅保留必要的输入/输出/错误提示）----------
            match opcode:
                case 16:  # 0x10 ALLOC
                    if not self.first_create_done:
                        self.R0 = 0
                        self.first_create_done = True
                        self.heap[0] = [0] * opnum
                    else:
                        self.R0 = self.heapNum
                        self.heapNum += randint(1, 10)
                        self.heap[self.R0] = [0] * opnum
                    self.PC += 2

                case 17:  # 0x11 LOAD
                    if self.R0 in self.heap and opnum < len(self.heap[self.R0]):
                        self.R1 = self.heap[self.R0][opnum]
                    else:
                        if 0 in self.heap and opnum < len(self.heap[0]):
                            self.R1 = self.heap[0][opnum]
                        else:
                            self.R1 = 0
                    self.PC += 2

                case 18:  # 0x12 STORE
                    if self.R0 in self.heap:
                        if opnum < len(self.heap[self.R0]):
                            self.heap[self.R0][opnum] = self.R1
                        else:
                            if 0 in self.heap and opnum < len(self.heap[0]):
                                self.heap[0][opnum] = self.R1
                            else:
                                raise IndexError(f'VM Memory Access Violation: store {opnum} out of bounds for both chunk 0x{self.R0:X} and fallback chunk 0')
                    else:
                        raise ValueError(f'VM Segmentation Fault: store {opnum} to unallocated memory pointer 0x{self.R0:X}')
                    self.PC += 2

                case 19:  # 0x13 FREE
                    if self.R0 in self.heap and self.R0 != 0:
                        del self.heap[self.R0]
                    self.PC += 1

                case 32:  # 0x20 MOVI
                    self.R1 = opnum & 255
                    self.PC += 2

                case 48:  # 0x30 ADDI
                    self.R1 = (self.R1 + opnum) & 255
                    self.PC += 2

                case 49:  # 0x31 XORI
                    self.R1 = (self.R1 ^ opnum) & 255
                    self.PC += 2

                case 50:  # 0x32 SHR
                    self.R1 = (self.R1 >> opnum) & 255
                    self.PC += 2

                case 51:  # 0x33 SHL
                    self.R1 = (self.R1 << opnum) & 255
                    self.PC += 2

                case 52:  # 0x34 ADD_MEM
                    if self.R0 in self.heap:
                        if opnum < len(self.heap[self.R0]):
                            self.heap[self.R0][opnum] = (self.heap[self.R0][opnum] + self.R1) & 255
                        else:
                            raise IndexError(f'VM Memory Access Violation: addin {opnum} out of bounds.')
                    else:
                        raise ValueError(f'VM Segmentation Fault: addin {opnum} to unallocated memory.')
                    self.PC += 2

                case 53:  # 0x35 XOR_MEM
                    if self.R0 in self.heap:
                        if opnum < len(self.heap[self.R0]):
                            self.heap[self.R0][opnum] = (self.heap[self.R0][opnum] ^ self.R1) & 255
                        else:
                            raise IndexError(f'VM Memory Access Violation: xorin {opnum} out of bounds.')
                    else:
                        raise ValueError(f'VM Segmentation Fault: xorin {opnum} to unallocated memory.')
                    self.PC += 2

                case 54:  # 0x36 SHR_MEM
                    if self.R0 in self.heap:
                        if opnum < len(self.heap[self.R0]):
                            self.heap[self.R0][opnum] = (self.heap[self.R0][opnum] >> self.R1) & 255
                        else:
                            raise IndexError(f'VM Memory Access Violation: zyin {opnum} out of bounds.')
                    else:
                        raise ValueError(f'VM Segmentation Fault: zyin {opnum} to unallocated memory.')
                    self.PC += 2

                case 55:  # 0x37 SHL_MEM
                    if self.R0 in self.heap:
                        if opnum < len(self.heap[self.R0]):
                            self.heap[self.R0][opnum] = (self.heap[self.R0][opnum] << self.R1) & 255
                        else:
                            raise IndexError(f'VM Memory Access Violation: yyin {opnum} out of bounds.')
                    else:
                        raise ValueError(f'VM Segmentation Fault: yyin {opnum} to unallocated memory.')
                    self.PC += 2

                case 56:  # 0x38 NOT
                    self.R1 = 0 if self.R1 != 0 else 1
                    self.PC += 1

                case 64:  # 0x40 CMPEQ
                    self.R1 = 1 if self.R1 == opnum else 0
                    self.PC += 2

                case 65:  # 0x41 JMP
                    self.PC = self.PC + 2 + opnum

                case 66:  # 0x42 JZ (相等跳转)
                    if self.R1 == 0:
                        self.PC += 2 + opnum
                    else:
                        self.PC += 2

                case 67:  # 0x43 JNZ (不等跳转)
                    if self.R1 != 0:
                        self.PC += 2 + opnum
                    else:
                        self.PC += 2

                case 68:  # 0x44 JMPB
                    self.PC = self.PC + 2 - opnum

                case 69:  # 0x45 JEB (相等回跳)
                    if self.R1 == 0:
                        self.PC = self.PC + 2 - opnum
                    else:
                        self.PC += 2

                case 70:  # 0x46 JNEB (不等回跳)
                    if self.R1 != 0:
                        self.PC = self.PC + 2 - opnum
                    else:
                        self.PC += 2

                case 80:  # 0x50 SWAP
                    if self.R0 in self.heap:
                        if opnum < len(self.heap[self.R0]):
                            temp = self.heap[self.R0][opnum]
                            self.heap[self.R0][opnum] = self.R1
                            self.R1 = temp
                        else:
                            raise IndexError(f'VM Memory Access Violation: swap {opnum} out of bounds.')
                    else:
                        raise ValueError(f'VM Segmentation Fault: swap {opnum} on unallocated memory.')
                    self.PC += 2

                case 81:  # 0x51 XCHG
                    self.R0, self.R1 = self.R1, self.R0
                    self.PC += 1

                case 82:  # 0x52 SYSCALL
                    if opnum == 1:
                        user_input = input('Input: ')
                        if self.R0 in self.heap:
                            for i, char in enumerate(user_input):
                                if i < len(self.heap[self.R0]):
                                    self.heap[self.R0][i] = ord(char)
                                else:
                                    raise IndexError('VM Memory Access Violation: input string too long for allocated buffer.')
                        else:
                            raise ValueError('VM Segmentation Fault: int 1 (input) to unallocated memory.')
                    elif opnum == 2 and self.R0 in self.heap:
                        mem_block = self.heap[self.R0]
                        out_chars = []
                        for c in mem_block:
                            if c == 0:
                                break
                            out_chars.append(chr(c))
                        print(''.join(out_chars), flush=True)
                    self.PC += 2

                case _:
                    raise ValueError(f'Unknown opcode 0x{opcode:02X} at PC={self.PC}')

        print("\n--- VM Execution End ---")

# ---------- 主程序（保持不变）----------
if __name__ == '__main__':
    if len(sys.argv) < 2:
        print('Usage: python kernelVM.py <program.bin>')
        sys.exit(1)
    filename = sys.argv[1]
    with open(filename, 'rb') as f:
        bytecode = f.read()
    vm = CustomVM()
    vm.run(bytecode)
```

新的

```py
import sys
from random import randint

class CustomVM:
    def __init__(self):
        self.R0 = 0
        self.R1 = 0
        self.PC = 0
        self.heap = {}
        self.heapNum = 61444
        self.first_create_done = False
        # 是否输出动态跟踪信息
        self.trace = True

    # ---------- 反汇编工具 ----------
    def _disassemble(self, bytecode):
        """生成静态反汇编文本（列表形式），返回 (lines, labels, instructions)"""
        LEN2_OPS = {
            0x10, 0x11, 0x12, 0x20,
            0x30, 0x31, 0x32, 0x33, 0x34, 0x35, 0x36, 0x37,
            0x40, 0x41, 0x42, 0x43, 0x44, 0x45, 0x46,
            0x50, 0x52,
        }
        OPINFO = {
            0x10: ("ALLOC", True),
            0x11: ("LOAD", True),
            0x12: ("STORE", True),
            0x13: ("FREE", False),
            0x20: ("MOVI", True),
            0x30: ("ADDI", True),
            0x31: ("XORI", True),
            0x32: ("SHR", True),
            0x33: ("SHL", True),
            0x34: ("ADD_MEM", True),
            0x35: ("XOR_MEM", True),
            0x36: ("SHR_MEM", True),
            0x37: ("SHL_MEM", True),
            0x38: ("NOT", False),
            0x40: ("CMPEQ", True),
            0x41: ("JMP", True),
            0x42: ("JZ", True),
            0x43: ("JNZ", True),
            0x44: ("JMPB", True),
            0x45: ("JEB", True),
            0x46: ("JNEB", True),
            0x50: ("SWAP", True),
            0x51: ("XCHG", False),
            0x52: ("SYSCALL", True),
        }
        SYSCALL_NAMES = {
            0x01: "READ_INPUT",
            0x02: "PRINT_STRING",
        }

        length = len(bytecode)
        pc = 0
        insns = []
        # 先收集所有指令
        while pc < length:
            opcode = bytecode[pc]
            has_operand = OPINFO.get(opcode, (None, False))[1]
            mnemonic = OPINFO.get(opcode, (f"UNKN_{opcode:02X}", False))[0]
            if has_operand and pc + 1 < length:
                operand = bytecode[pc + 1]
                inst_len = 2
            else:
                operand = None
                inst_len = 1
            insns.append({
                "pc": pc,
                "opcode": opcode,
                "len": inst_len,
                "arg": operand,
                "name": mnemonic,
                "has_arg": has_operand,
            })
            pc += inst_len

        # 计算跳转目标标签
        labels = {}
        for insn in insns:
            if insn["name"] in {"JZ", "JNZ", "JMP", "JMPB", "JEB", "JNEB"} and insn["arg"] is not None:
                if insn["name"] in {"JMP", "JZ", "JNZ"}:
                    target = insn["pc"] + 2 + insn["arg"]
                else:  # JMPB, JEB, JNEB
                    target = insn["pc"] + 2 - insn["arg"]
                insn["target"] = target
                labels.setdefault(target, f"loc_{target:04X}")

        # 生成反汇编文本
        lines = []
        for insn in insns:
            if insn["pc"] in labels:
                lines.append(f"{labels[insn['pc']]}:")
            # 十六进制字节
            if insn["len"] == 2:
                hex_bytes = f"{insn['opcode']:02X} {insn['arg']:02X}"
            else:
                hex_bytes = f"{insn['opcode']:02X}"
            # 助记符 + 操作数
            name = insn["name"]
            arg = insn["arg"]
            if name == "SYSCALL" and arg is not None:
                detail = f"SYSCALL 0x{arg:02X} ; {SYSCALL_NAMES.get(arg, 'UNKNOWN_SYSCALL')}"
            elif name in {"JZ", "JNZ", "JMP", "JMPB", "JEB", "JNEB"} and arg is not None:
                target = insn.get("target", insn["pc"] + (2 + arg if name in {"JMP","JZ","JNZ"} else 2 - arg))
                label = labels.get(target, f"loc_{target:04X}")
                detail = f"{name} 0x{arg:02X} ; -> {label} (0x{target:04X})"
            elif insn["has_arg"] and arg is not None:
                detail = f"{name} 0x{arg:02X}"
            else:
                detail = name
            line = f"  0x{insn['pc']:04X}: {hex_bytes:<5}  {detail}"
            lines.append(line)
        return lines, labels, insns

    # ---------- 动态跟踪辅助 ----------
    def _trace_pre(self, insn, opnum):
        """输出指令执行前的信息"""
        if not self.trace:
            return
        name = insn["name"]
        arg = insn["arg"]
        if arg is None:
            arg_str = ""
        else:
            arg_str = f" 0x{arg:02X}"
        # 寄存器状态
        regs = f"R0=0x{self.R0:04X} R1=0x{self.R1:02X}"
        # 基础输出
        print(f"[{insn['pc']:04X}] {name}{arg_str:<10} {regs}", end="")
        # 对内存访问指令额外打印信息
        if name in ("LOAD", "STORE", "ADD_MEM", "XOR_MEM", "SHR_MEM", "SHL_MEM", "SWAP"):
            if self.R0 in self.heap:
                heap_len = len(self.heap[self.R0])
                if arg < heap_len:
                    mem_val = self.heap[self.R0][arg]
                    print(f"  heap[0x{self.R0:X}][{arg}] = 0x{mem_val:02X}", end="")
                else:
                    print(f"  heap[0x{self.R0:X}][{arg}] out of bounds", end="")
            else:
                print(f"  heap[0x{self.R0:X}] not allocated", end="")
        print()

    def _trace_post(self, insn, opnum):
        """输出指令执行后的信息（可选，一般合并到_pre中）"""
        if not self.trace:
            return
        # 对于XCHG等指令，寄存器变化已经在下一条指令的_pre中体现，这里不额外输出
        pass

    # ---------- 虚拟机执行 ----------
    def run(self, bytecode):
        # 1. 静态反汇编并输出
        lines, labels, insns = self._disassemble(bytecode)
        print(f"; size = {len(bytecode)} bytes")
        print(f"; instructions = {len(insns)}")
        print()
        for line in lines:
            print(line)
        print("\n--- VM Execution Start ---\n")

        # 2. 动态执行
        self.PC = 0
        length = len(bytecode)
        # 构建指令映射表，便于快速获取当前指令信息
        pc_to_insn = {insn["pc"]: insn for insn in insns}

        while self.PC < length:
            insn = pc_to_insn.get(self.PC)
            if insn is None:
                raise ValueError(f"Invalid PC {self.PC:04X}")

            opcode = insn["opcode"]
            opnum = insn["arg"] if insn["has_arg"] else 0

            # 动态跟踪：执行前
            self._trace_pre(insn, opnum)

            # 执行指令
            match opcode:
                case 16:  # 0x10 ALLOC
                    if not self.first_create_done:
                        self.R0 = 0
                        self.first_create_done = True
                        self.heap[0] = [0] * opnum
                    else:
                        self.R0 = self.heapNum
                        self.heapNum += randint(1, 10)
                        self.heap[self.R0] = [0] * opnum
                    self.PC += 2

                case 17:  # 0x11 LOAD
                    if self.R0 in self.heap and opnum < len(self.heap[self.R0]):
                        self.R1 = self.heap[self.R0][opnum]
                    else:
                        if 0 in self.heap and opnum < len(self.heap[0]):
                            self.R1 = self.heap[0][opnum]
                        else:
                            self.R1 = 0
                    self.PC += 2

                case 18:  # 0x12 STORE
                    if self.R0 in self.heap:
                        if opnum < len(self.heap[self.R0]):
                            self.heap[self.R0][opnum] = self.R1
                        else:
                            if 0 in self.heap and opnum < len(self.heap[0]):
                                self.heap[0][opnum] = self.R1
                            else:
                                raise IndexError(f'VM Memory Access Violation: store {opnum} out of bounds')
                    else:
                        raise ValueError(f'VM Segmentation Fault: store {opnum} to unallocated memory')
                    self.PC += 2

                case 19:  # 0x13 FREE
                    if self.R0 in self.heap and self.R0 != 0:
                        del self.heap[self.R0]
                    self.PC += 1

                case 32:  # 0x20 MOVI
                    self.R1 = opnum & 255
                    self.PC += 2

                case 48:  # 0x30 ADDI
                    self.R1 = (self.R1 + opnum) & 255
                    self.PC += 2

                case 49:  # 0x31 XORI
                    self.R1 = (self.R1 ^ opnum) & 255
                    self.PC += 2

                case 50:  # 0x32 SHR
                    self.R1 = (self.R1 >> opnum) & 255
                    self.PC += 2

                case 51:  # 0x33 SHL
                    self.R1 = (self.R1 << opnum) & 255
                    self.PC += 2

                case 52:  # 0x34 ADD_MEM
                    if self.R0 in self.heap:
                        if opnum < len(self.heap[self.R0]):
                            self.heap[self.R0][opnum] = (self.heap[self.R0][opnum] + self.R1) & 255
                        else:
                            raise IndexError(f'VM Memory Access Violation: addin {opnum} out of bounds.')
                    else:
                        raise ValueError(f'VM Segmentation Fault: addin {opnum} to unallocated memory.')
                    self.PC += 2

                case 53:  # 0x35 XOR_MEM
                    if self.R0 in self.heap:
                        if opnum < len(self.heap[self.R0]):
                            self.heap[self.R0][opnum] = (self.heap[self.R0][opnum] ^ self.R1) & 255
                        else:
                            raise IndexError(f'VM Memory Access Violation: xorin {opnum} out of bounds.')
                    else:
                        raise ValueError(f'VM Segmentation Fault: xorin {opnum} to unallocated memory.')
                    self.PC += 2

                case 54:  # 0x36 SHR_MEM
                    if self.R0 in self.heap:
                        if opnum < len(self.heap[self.R0]):
                            self.heap[self.R0][opnum] = (self.heap[self.R0][opnum] >> self.R1) & 255
                        else:
                            raise IndexError(f'VM Memory Access Violation: shr_mem {opnum} out of bounds.')
                    else:
                        raise ValueError(f'VM Segmentation Fault: shr_mem {opnum} to unallocated memory.')
                    self.PC += 2

                case 55:  # 0x37 SHL_MEM
                    if self.R0 in self.heap:
                        if opnum < len(self.heap[self.R0]):
                            self.heap[self.R0][opnum] = (self.heap[self.R0][opnum] << self.R1) & 255
                        else:
                            raise IndexError(f'VM Memory Access Violation: shl_mem {opnum} out of bounds.')
                    else:
                        raise ValueError(f'VM Segmentation Fault: shl_mem {opnum} to unallocated memory.')
                    self.PC += 2

                case 56:  # 0x38 NOT
                    self.R1 = 0 if self.R1 != 0 else 1
                    self.PC += 1

                case 64:  # 0x40 CMPEQ
                    self.R1 = 1 if self.R1 == opnum else 0
                    self.PC += 2

                case 65:  # 0x41 JMP
                    self.PC = self.PC + 2 + opnum

                case 66:  # 0x42 JZ
                    if self.R1 == 0:
                        self.PC += 2 + opnum
                    else:
                        self.PC += 2

                case 67:  # 0x43 JNZ
                    if self.R1 != 0:
                        self.PC += 2 + opnum
                    else:
                        self.PC += 2

                case 68:  # 0x44 JMPB
                    self.PC = self.PC + 2 - opnum

                case 69:  # 0x45 JEB
                    if self.R1 == 0:
                        self.PC = self.PC + 2 - opnum
                    else:
                        self.PC += 2

                case 70:  # 0x46 JNEB
                    if self.R1 != 0:
                        self.PC = self.PC + 2 - opnum
                    else:
                        self.PC += 2

                case 80:  # 0x50 SWAP
                    if self.R0 in self.heap:
                        if opnum < len(self.heap[self.R0]):
                            temp = self.heap[self.R0][opnum]
                            self.heap[self.R0][opnum] = self.R1
                            self.R1 = temp
                        else:
                            raise IndexError(f'VM Memory Access Violation: swap {opnum} out of bounds.')
                    else:
                        raise ValueError(f'VM Segmentation Fault: swap {opnum} on unallocated memory.')
                    self.PC += 2

                case 81:  # 0x51 XCHG
                    self.R0, self.R1 = self.R1, self.R0
                    self.PC += 1

                case 82:  # 0x52 SYSCALL
                    if opnum == 1:
                        user_input = input('Input: ')
                        if self.R0 in self.heap:
                            for i, char in enumerate(user_input):
                                if i < len(self.heap[self.R0]):
                                    self.heap[self.R0][i] = ord(char)
                                else:
                                    raise IndexError('VM Memory Access Violation: input string too long')
                        else:
                            raise ValueError('VM Segmentation Fault: int 1 to unallocated memory')
                    elif opnum == 2 and self.R0 in self.heap:
                        mem_block = self.heap[self.R0]
                        out_chars = []
                        for c in mem_block:
                            if c == 0:
                                break
                            out_chars.append(chr(c))
                        print(''.join(out_chars), flush=True)
                    self.PC += 2

                case _:
                    raise ValueError(f'Unknown opcode 0x{opcode:02X} at PC={self.PC}')

            # 可选：执行后输出（这里不重复输出，因为下一条指令的_pre会显示新寄存器状态）

        print("\n--- VM Execution End ---")

if __name__ == '__main__':
    if len(sys.argv) < 2:
        print('Usage: python kernelVM.py <program.bin>')
        sys.exit(1)
    filename = sys.argv[1]
    with open(filename, 'rb') as f:
        bytecode = f.read()
    vm = CustomVM()
    vm.run(bytecode)
```

### fake flag

新的trace结果

```plaintext
E:\_CTF\模板\.venv\Scripts\python.exe "E:\_CTF\题目\POLARIS CTF 2026\FunPyVM\decompiled_main.py" 
--- VM Start ---
; size = 855 bytes
; instructions = 428

0x0000: 10 64 ALLOC 0x64
0x0002: 10 32 ALLOC 0x32
0x0004: 52 01 SYSCALL 0x01 ; READ_INPUT
0x0006: 11 00 LOAD 0x00
0x0008: 40 00 CMPEQ 0x00
0x000A: 43 06 JNZ 0x06 ; -> 0x0012
0x000C: 11 00 LOAD 0x00
0x000E: 31 55 XORI 0x55
0x0010: 12 00 STORE 0x00
0x0012: 11 01 LOAD 0x01
0x0014: 40 00 CMPEQ 0x00
0x0016: 43 06 JNZ 0x06 ; -> 0x001E
0x0018: 11 01 LOAD 0x01
0x001A: 31 55 XORI 0x55
0x001C: 12 01 STORE 0x01
0x001E: 11 02 LOAD 0x02
0x0020: 40 00 CMPEQ 0x00
0x0022: 43 06 JNZ 0x06 ; -> 0x002A
0x0024: 11 02 LOAD 0x02
0x0026: 31 55 XORI 0x55
0x0028: 12 02 STORE 0x02
0x002A: 11 03 LOAD 0x03
0x002C: 40 00 CMPEQ 0x00
0x002E: 43 06 JNZ 0x06 ; -> 0x0036
0x0030: 11 03 LOAD 0x03
0x0032: 31 55 XORI 0x55
0x0034: 12 03 STORE 0x03
0x0036: 11 04 LOAD 0x04
0x0038: 40 00 CMPEQ 0x00
0x003A: 43 06 JNZ 0x06 ; -> 0x0042
0x003C: 11 04 LOAD 0x04
0x003E: 31 55 XORI 0x55
0x0040: 12 04 STORE 0x04
0x0042: 11 05 LOAD 0x05
0x0044: 40 00 CMPEQ 0x00
0x0046: 43 06 JNZ 0x06 ; -> 0x004E
0x0048: 11 05 LOAD 0x05
0x004A: 31 55 XORI 0x55
0x004C: 12 05 STORE 0x05
0x004E: 11 06 LOAD 0x06
0x0050: 40 00 CMPEQ 0x00
0x0052: 43 06 JNZ 0x06 ; -> 0x005A
0x0054: 11 06 LOAD 0x06
0x0056: 31 55 XORI 0x55
0x0058: 12 06 STORE 0x06
0x005A: 11 07 LOAD 0x07
0x005C: 40 00 CMPEQ 0x00
0x005E: 43 06 JNZ 0x06 ; -> 0x0066
0x0060: 11 07 LOAD 0x07
0x0062: 31 55 XORI 0x55
0x0064: 12 07 STORE 0x07
0x0066: 11 08 LOAD 0x08
0x0068: 40 00 CMPEQ 0x00
0x006A: 43 06 JNZ 0x06 ; -> 0x0072
0x006C: 11 08 LOAD 0x08
0x006E: 31 55 XORI 0x55
0x0070: 12 08 STORE 0x08
0x0072: 11 09 LOAD 0x09
0x0074: 40 00 CMPEQ 0x00
0x0076: 43 06 JNZ 0x06 ; -> 0x007E
0x0078: 11 09 LOAD 0x09
0x007A: 31 55 XORI 0x55
0x007C: 12 09 STORE 0x09
0x007E: 11 0A LOAD 0x0A
0x0080: 40 00 CMPEQ 0x00
0x0082: 43 06 JNZ 0x06 ; -> 0x008A
0x0084: 11 0A LOAD 0x0A
0x0086: 31 55 XORI 0x55
0x0088: 12 0A STORE 0x0A
0x008A: 11 0B LOAD 0x0B
0x008C: 40 00 CMPEQ 0x00
0x008E: 43 06 JNZ 0x06 ; -> 0x0096
0x0090: 11 0B LOAD 0x0B
0x0092: 31 55 XORI 0x55
0x0094: 12 0B STORE 0x0B
0x0096: 11 0C LOAD 0x0C
0x0098: 40 00 CMPEQ 0x00
0x009A: 43 06 JNZ 0x06 ; -> 0x00A2
0x009C: 11 0C LOAD 0x0C
0x009E: 31 55 XORI 0x55
0x00A0: 12 0C STORE 0x0C
0x00A2: 11 0D LOAD 0x0D
0x00A4: 40 00 CMPEQ 0x00
0x00A6: 43 06 JNZ 0x06 ; -> 0x00AE
0x00A8: 11 0D LOAD 0x0D
0x00AA: 31 55 XORI 0x55
0x00AC: 12 0D STORE 0x0D
0x00AE: 11 0E LOAD 0x0E
0x00B0: 40 00 CMPEQ 0x00
0x00B2: 43 06 JNZ 0x06 ; -> 0x00BA
0x00B4: 11 0E LOAD 0x0E
0x00B6: 31 55 XORI 0x55
0x00B8: 12 0E STORE 0x0E
0x00BA: 11 0F LOAD 0x0F
0x00BC: 40 00 CMPEQ 0x00
0x00BE: 43 06 JNZ 0x06 ; -> 0x00C6
0x00C0: 11 0F LOAD 0x0F
0x00C2: 31 55 XORI 0x55
0x00C4: 12 0F STORE 0x0F
0x00C6: 11 10 LOAD 0x10
0x00C8: 40 00 CMPEQ 0x00
0x00CA: 43 06 JNZ 0x06 ; -> 0x00D2
0x00CC: 11 10 LOAD 0x10
0x00CE: 31 55 XORI 0x55
0x00D0: 12 10 STORE 0x10
0x00D2: 11 11 LOAD 0x11
0x00D4: 40 00 CMPEQ 0x00
0x00D6: 43 06 JNZ 0x06 ; -> 0x00DE
0x00D8: 11 11 LOAD 0x11
0x00DA: 31 55 XORI 0x55
0x00DC: 12 11 STORE 0x11
0x00DE: 11 12 LOAD 0x12
0x00E0: 40 00 CMPEQ 0x00
0x00E2: 43 06 JNZ 0x06 ; -> 0x00EA
0x00E4: 11 12 LOAD 0x12
0x00E6: 31 55 XORI 0x55
0x00E8: 12 12 STORE 0x12
0x00EA: 11 13 LOAD 0x13
0x00EC: 40 00 CMPEQ 0x00
0x00EE: 43 06 JNZ 0x06 ; -> 0x00F6
0x00F0: 11 13 LOAD 0x13
0x00F2: 31 55 XORI 0x55
0x00F4: 12 13 STORE 0x13
0x00F6: 11 14 LOAD 0x14
0x00F8: 40 00 CMPEQ 0x00
0x00FA: 43 06 JNZ 0x06 ; -> 0x0102
0x00FC: 11 14 LOAD 0x14
0x00FE: 31 55 XORI 0x55
0x0100: 12 14 STORE 0x14
0x0102: 11 15 LOAD 0x15
0x0104: 40 00 CMPEQ 0x00
0x0106: 43 06 JNZ 0x06 ; -> 0x010E
0x0108: 11 15 LOAD 0x15
0x010A: 31 55 XORI 0x55
0x010C: 12 15 STORE 0x15
0x010E: 11 16 LOAD 0x16
0x0110: 40 00 CMPEQ 0x00
0x0112: 43 06 JNZ 0x06 ; -> 0x011A
0x0114: 11 16 LOAD 0x16
0x0116: 31 55 XORI 0x55
0x0118: 12 16 STORE 0x16
0x011A: 11 17 LOAD 0x17
0x011C: 40 00 CMPEQ 0x00
0x011E: 43 06 JNZ 0x06 ; -> 0x0126
0x0120: 11 17 LOAD 0x17
0x0122: 31 55 XORI 0x55
0x0124: 12 17 STORE 0x17
0x0126: 11 18 LOAD 0x18
0x0128: 40 00 CMPEQ 0x00
0x012A: 43 06 JNZ 0x06 ; -> 0x0132
0x012C: 11 18 LOAD 0x18
0x012E: 31 55 XORI 0x55
0x0130: 12 18 STORE 0x18
0x0132: 11 19 LOAD 0x19
0x0134: 40 00 CMPEQ 0x00
0x0136: 43 06 JNZ 0x06 ; -> 0x013E
0x0138: 11 19 LOAD 0x19
0x013A: 31 55 XORI 0x55
0x013C: 12 19 STORE 0x19
0x013E: 11 1A LOAD 0x1A
0x0140: 40 00 CMPEQ 0x00
0x0142: 43 06 JNZ 0x06 ; -> 0x014A
0x0144: 11 1A LOAD 0x1A
0x0146: 31 55 XORI 0x55
0x0148: 12 1A STORE 0x1A
0x014A: 11 00 LOAD 0x00
0x014C: 40 00 CMPEQ 0x00
0x014E: 43 06 JNZ 0x06 ; -> 0x0156
0x0150: 11 00 LOAD 0x00 //这里开始下一步加密
0x0152: 30 07 ADDI 0x07
0x0154: 12 00 STORE 0x00
0x0156: 11 01 LOAD 0x01
0x0158: 40 00 CMPEQ 0x00
0x015A: 43 06 JNZ 0x06 ; -> 0x0162
0x015C: 11 01 LOAD 0x01
0x015E: 30 0A ADDI 0x0A
0x0160: 12 01 STORE 0x01
0x0162: 11 02 LOAD 0x02
0x0164: 40 00 CMPEQ 0x00
0x0166: 43 06 JNZ 0x06 ; -> 0x016E
0x0168: 11 02 LOAD 0x02
0x016A: 30 0D ADDI 0x0D
0x016C: 12 02 STORE 0x02
0x016E: 11 03 LOAD 0x03
0x0170: 40 00 CMPEQ 0x00
0x0172: 43 06 JNZ 0x06 ; -> 0x017A
0x0174: 11 03 LOAD 0x03
0x0176: 30 10 ADDI 0x10
0x0178: 12 03 STORE 0x03
0x017A: 11 04 LOAD 0x04
0x017C: 40 00 CMPEQ 0x00
0x017E: 43 06 JNZ 0x06 ; -> 0x0186
0x0180: 11 04 LOAD 0x04
0x0182: 30 13 ADDI 0x13
0x0184: 12 04 STORE 0x04
0x0186: 11 05 LOAD 0x05
0x0188: 40 00 CMPEQ 0x00
0x018A: 43 06 JNZ 0x06 ; -> 0x0192
0x018C: 11 05 LOAD 0x05
0x018E: 30 16 ADDI 0x16
0x0190: 12 05 STORE 0x05
0x0192: 11 06 LOAD 0x06
0x0194: 40 00 CMPEQ 0x00
0x0196: 43 06 JNZ 0x06 ; -> 0x019E
0x0198: 11 06 LOAD 0x06
0x019A: 30 19 ADDI 0x19
0x019C: 12 06 STORE 0x06
0x019E: 11 07 LOAD 0x07
0x01A0: 40 00 CMPEQ 0x00
0x01A2: 43 06 JNZ 0x06 ; -> 0x01AA
0x01A4: 11 07 LOAD 0x07
0x01A6: 30 1C ADDI 0x1C
0x01A8: 12 07 STORE 0x07
0x01AA: 11 08 LOAD 0x08
0x01AC: 40 00 CMPEQ 0x00
0x01AE: 43 06 JNZ 0x06 ; -> 0x01B6
0x01B0: 11 08 LOAD 0x08
0x01B2: 30 1F ADDI 0x1F
0x01B4: 12 08 STORE 0x08
0x01B6: 11 09 LOAD 0x09
0x01B8: 40 00 CMPEQ 0x00
0x01BA: 43 06 JNZ 0x06 ; -> 0x01C2
0x01BC: 11 09 LOAD 0x09
0x01BE: 30 22 ADDI 0x22
0x01C0: 12 09 STORE 0x09
0x01C2: 11 0A LOAD 0x0A
0x01C4: 40 00 CMPEQ 0x00
0x01C6: 43 06 JNZ 0x06 ; -> 0x01CE
0x01C8: 11 0A LOAD 0x0A
0x01CA: 30 25 ADDI 0x25
0x01CC: 12 0A STORE 0x0A
0x01CE: 11 0B LOAD 0x0B
0x01D0: 40 00 CMPEQ 0x00
0x01D2: 43 06 JNZ 0x06 ; -> 0x01DA
0x01D4: 11 0B LOAD 0x0B
0x01D6: 30 28 ADDI 0x28
0x01D8: 12 0B STORE 0x0B
0x01DA: 11 0C LOAD 0x0C
0x01DC: 40 00 CMPEQ 0x00
0x01DE: 43 06 JNZ 0x06 ; -> 0x01E6
0x01E0: 11 0C LOAD 0x0C
0x01E2: 30 2B ADDI 0x2B
0x01E4: 12 0C STORE 0x0C
0x01E6: 11 0D LOAD 0x0D
0x01E8: 40 00 CMPEQ 0x00
0x01EA: 43 06 JNZ 0x06 ; -> 0x01F2
0x01EC: 11 0D LOAD 0x0D
0x01EE: 30 2E ADDI 0x2E
0x01F0: 12 0D STORE 0x0D
0x01F2: 11 0E LOAD 0x0E
0x01F4: 40 00 CMPEQ 0x00
0x01F6: 43 06 JNZ 0x06 ; -> 0x01FE
0x01F8: 11 0E LOAD 0x0E
0x01FA: 30 31 ADDI 0x31
0x01FC: 12 0E STORE 0x0E
0x01FE: 11 0F LOAD 0x0F
0x0200: 40 00 CMPEQ 0x00
0x0202: 43 06 JNZ 0x06 ; -> 0x020A
0x0204: 11 0F LOAD 0x0F
0x0206: 30 34 ADDI 0x34
0x0208: 12 0F STORE 0x0F
0x020A: 11 10 LOAD 0x10
0x020C: 40 00 CMPEQ 0x00
0x020E: 43 06 JNZ 0x06 ; -> 0x0216
0x0210: 11 10 LOAD 0x10
0x0212: 30 37 ADDI 0x37
0x0214: 12 10 STORE 0x10
0x0216: 11 11 LOAD 0x11
0x0218: 40 00 CMPEQ 0x00
0x021A: 43 06 JNZ 0x06 ; -> 0x0222
0x021C: 11 11 LOAD 0x11
0x021E: 30 3A ADDI 0x3A
0x0220: 12 11 STORE 0x11
0x0222: 11 12 LOAD 0x12
0x0224: 40 00 CMPEQ 0x00
0x0226: 43 06 JNZ 0x06 ; -> 0x022E
0x0228: 11 12 LOAD 0x12
0x022A: 30 3D ADDI 0x3D
0x022C: 12 12 STORE 0x12
0x022E: 11 13 LOAD 0x13
0x0230: 40 00 CMPEQ 0x00
0x0232: 43 06 JNZ 0x06 ; -> 0x023A
0x0234: 11 13 LOAD 0x13
0x0236: 30 40 ADDI 0x40
0x0238: 12 13 STORE 0x13
0x023A: 11 14 LOAD 0x14
0x023C: 40 00 CMPEQ 0x00
0x023E: 43 06 JNZ 0x06 ; -> 0x0246
0x0240: 11 14 LOAD 0x14
0x0242: 30 43 ADDI 0x43
0x0244: 12 14 STORE 0x14
0x0246: 11 15 LOAD 0x15
0x0248: 40 00 CMPEQ 0x00
0x024A: 43 06 JNZ 0x06 ; -> 0x0252
0x024C: 11 15 LOAD 0x15
0x024E: 30 46 ADDI 0x46
0x0250: 12 15 STORE 0x15
0x0252: 11 16 LOAD 0x16
0x0254: 40 00 CMPEQ 0x00
0x0256: 43 06 JNZ 0x06 ; -> 0x025E
0x0258: 11 16 LOAD 0x16
0x025A: 30 49 ADDI 0x49
0x025C: 12 16 STORE 0x16
0x025E: 11 17 LOAD 0x17
0x0260: 40 00 CMPEQ 0x00
0x0262: 43 06 JNZ 0x06 ; -> 0x026A
0x0264: 11 17 LOAD 0x17
0x0266: 30 4C ADDI 0x4C
0x0268: 12 17 STORE 0x17
0x026A: 11 18 LOAD 0x18
0x026C: 40 00 CMPEQ 0x00
0x026E: 43 06 JNZ 0x06 ; -> 0x0276
0x0270: 11 18 LOAD 0x18
0x0272: 30 4F ADDI 0x4F
0x0274: 12 18 STORE 0x18
0x0276: 11 19 LOAD 0x19
0x0278: 40 00 CMPEQ 0x00
0x027A: 43 06 JNZ 0x06 ; -> 0x0282
0x027C: 11 19 LOAD 0x19
0x027E: 30 52 ADDI 0x52
0x0280: 12 19 STORE 0x19
0x0282: 11 1A LOAD 0x1A
0x0284: 40 00 CMPEQ 0x00
0x0286: 43 06 JNZ 0x06 ; -> 0x028E
0x0288: 11 1A LOAD 0x1A
0x028A: 30 55 ADDI 0x55
0x028C: 12 1A STORE 0x1A
0x028E: 11 00 LOAD 0x00
0x0290: 40 29 CMPEQ 0x29
0x0292: 42 B2 JZ 0xB2 ; -> 0x0346
0x0294: 11 01 LOAD 0x01
0x0296: 40 47 CMPEQ 0x47
0x0298: 42 AC JZ 0xAC ; -> 0x0346
0x029A: 11 02 LOAD 0x02
0x029C: 40 39 CMPEQ 0x39
0x029E: 42 A6 JZ 0xA6 ; -> 0x0346
0x02A0: 11 03 LOAD 0x03
0x02A2: 40 1A CMPEQ 0x1A
0x02A4: 42 A0 JZ 0xA0 ; -> 0x0346
0x02A6: 11 04 LOAD 0x04
0x02A8: 40 3F CMPEQ 0x3F
0x02AA: 42 9A JZ 0x9A ; -> 0x0346
0x02AC: 11 05 LOAD 0x05
0x02AE: 40 50 CMPEQ 0x50
0x02B0: 42 94 JZ 0x94 ; -> 0x0346
0x02B2: 11 06 LOAD 0x06
0x02B4: 40 39 CMPEQ 0x39
0x02B6: 42 8E JZ 0x8E ; -> 0x0346
0x02B8: 11 07 LOAD 0x07
0x02BA: 40 26 CMPEQ 0x26
0x02BC: 42 88 JZ 0x88 ; -> 0x0346
0x02BE: 11 08 LOAD 0x08
0x02C0: 40 40 CMPEQ 0x40
0x02C2: 42 82 JZ 0x82 ; -> 0x0346
0x02C4: 11 09 LOAD 0x09
0x02C6: 40 5F CMPEQ 0x5F
0x02C8: 42 7C JZ 0x7C ; -> 0x0346
0x02CA: 11 0A LOAD 0x0A
0x02CC: 40 61 CMPEQ 0x61
0x02CE: 42 76 JZ 0x76 ; -> 0x0346
0x02D0: 11 0B LOAD 0x0B
0x02D2: 40 63 CMPEQ 0x63
0x02D4: 42 70 JZ 0x70 ; -> 0x0346
0x02D6: 11 0C LOAD 0x0C
0x02D8: 40 69 CMPEQ 0x69
0x02DA: 42 6A JZ 0x6A ; -> 0x0346
0x02DC: 11 0D LOAD 0x0D
0x02DE: 40 38 CMPEQ 0x38
0x02E0: 42 64 JZ 0x64 ; -> 0x0346
0x02E2: 11 0E LOAD 0x0E
0x02E4: 40 52 CMPEQ 0x52
0x02E6: 42 5E JZ 0x5E ; -> 0x0346
0x02E8: 11 0F LOAD 0x0F
0x02EA: 40 71 CMPEQ 0x71
0x02EC: 42 58 JZ 0x58 ; -> 0x0346
0x02EE: 11 10 LOAD 0x10
0x02F0: 40 73 CMPEQ 0x73
0x02F2: 42 52 JZ 0x52 ; -> 0x0346
0x02F4: 11 11 LOAD 0x11
0x02F6: 40 60 CMPEQ 0x60
0x02F8: 42 4C JZ 0x4C ; -> 0x0346
0x02FA: 11 12 LOAD 0x12
0x02FC: 40 47 CMPEQ 0x47
0x02FE: 42 46 JZ 0x46 ; -> 0x0346
0x0300: 11 13 LOAD 0x13
0x0302: 40 7C CMPEQ 0x7C
0x0304: 42 40 JZ 0x40 ; -> 0x0346
0x0306: 11 14 LOAD 0x14
0x0308: 40 69 CMPEQ 0x69
0x030A: 42 3A JZ 0x3A ; -> 0x0346
0x030C: 11 15 LOAD 0x15
0x030E: 40 50 CMPEQ 0x50
0x0310: 42 34 JZ 0x34 ; -> 0x0346
0x0312: 11 16 LOAD 0x16
0x0314: 40 6A CMPEQ 0x6A
0x0316: 42 2E JZ 0x2E ; -> 0x0346
0x0318: 11 17 LOAD 0x17
0x031A: 40 73 CMPEQ 0x73
0x031C: 42 28 JZ 0x28 ; -> 0x0346
0x031E: 11 18 LOAD 0x18
0x0320: 40 6F CMPEQ 0x6F
0x0322: 42 22 JZ 0x22 ; -> 0x0346
0x0324: 11 19 LOAD 0x19
0x0326: 40 82 CMPEQ 0x82
0x0328: 42 1C JZ 0x1C ; -> 0x0346
0x032A: 11 1A LOAD 0x1A
0x032C: 40 BF CMPEQ 0xBF
0x032E: 42 16 JZ 0x16 ; -> 0x0346
0x0330: 10 04 ALLOC 0x04
0x0332: 20 79 MOVI 0x79
0x0334: 12 00 STORE 0x00
0x0336: 20 65 MOVI 0x65
0x0338: 12 01 STORE 0x01
0x033A: 20 73 MOVI 0x73
0x033C: 12 02 STORE 0x02
0x033E: 20 00 MOVI 0x00
0x0340: 12 03 STORE 0x03
0x0342: 52 02 SYSCALL 0x02 ; PRINT_OUTPUT
0x0344: 41 10 JMP 0x10 ; -> 0x0356
0x0346: 10 03 ALLOC 0x03
0x0348: 20 4E MOVI 0x4E
0x034A: 12 00 STORE 0x00
0x034C: 20 6F MOVI 0x6F
0x034E: 12 01 STORE 0x01
0x0350: 20 00 MOVI 0x00
0x0352: 12 02 STORE 0x02
0x0354: 52 02 SYSCALL 0x02 ; PRINT_OUTPUT
0x0356: 13    FREE

```

加密部分

```plaintext
0x0086: 31 55 XORI 0x55

0x0152: 30 07 ADDI 0x07
...
0x015E: 30 0A ADDI 0x0A
...
```

这样看，密文比较部分就很明显了，

```plaintext
0x0290: 40 29 CMPEQ 0x29
0x0292: 42 B2 JZ 0xB2 ; -> 0x0346
0x0294: 11 01 LOAD 0x01
0x0296: 40 47 CMPEQ 0x47
0x0298: 42 AC JZ 0xAC ; -> 0x0346
0x029A: 11 02 LOAD 0x02
0x029C: 40 39 CMPEQ 0x39
0x029E: 42 A6 JZ 0xA6 ; -> 0x0346
0x02A0: 11 03 LOAD 0x03
0x02A2: 40 1A CMPEQ 0x1A
0x02A4: 42 A0 JZ 0xA0 ; -> 0x0346
0x02A6: 11 04 LOAD 0x04
0x02A8: 40 3F CMPEQ 0x3F
0x02AA: 42 9A JZ 0x9A ; -> 0x0346
0x02AC: 11 05 LOAD 0x05
0x02AE: 40 50 CMPEQ 0x50
0x02B0: 42 94 JZ 0x94 ; -> 0x0346
0x02B2: 11 06 LOAD 0x06
0x02B4: 40 39 CMPEQ 0x39
0x02B6: 42 8E JZ 0x8E ; -> 0x0346
0x02B8: 11 07 LOAD 0x07
0x02BA: 40 26 CMPEQ 0x26
0x02BC: 42 88 JZ 0x88 ; -> 0x0346
0x02BE: 11 08 LOAD 0x08
0x02C0: 40 40 CMPEQ 0x40
0x02C2: 42 82 JZ 0x82 ; -> 0x0346
0x02C4: 11 09 LOAD 0x09
0x02C6: 40 5F CMPEQ 0x5F
0x02C8: 42 7C JZ 0x7C ; -> 0x0346
0x02CA: 11 0A LOAD 0x0A
0x02CC: 40 61 CMPEQ 0x61
0x02CE: 42 76 JZ 0x76 ; -> 0x0346
0x02D0: 11 0B LOAD 0x0B
0x02D2: 40 63 CMPEQ 0x63
0x02D4: 42 70 JZ 0x70 ; -> 0x0346
0x02D6: 11 0C LOAD 0x0C
0x02D8: 40 69 CMPEQ 0x69
0x02DA: 42 6A JZ 0x6A ; -> 0x0346
0x02DC: 11 0D LOAD 0x0D
0x02DE: 40 38 CMPEQ 0x38
0x02E0: 42 64 JZ 0x64 ; -> 0x0346
0x02E2: 11 0E LOAD 0x0E
0x02E4: 40 52 CMPEQ 0x52
0x02E6: 42 5E JZ 0x5E ; -> 0x0346
0x02E8: 11 0F LOAD 0x0F
0x02EA: 40 71 CMPEQ 0x71
0x02EC: 42 58 JZ 0x58 ; -> 0x0346
0x02EE: 11 10 LOAD 0x10
0x02F0: 40 73 CMPEQ 0x73
0x02F2: 42 52 JZ 0x52 ; -> 0x0346
0x02F4: 11 11 LOAD 0x11
0x02F6: 40 60 CMPEQ 0x60
0x02F8: 42 4C JZ 0x4C ; -> 0x0346
0x02FA: 11 12 LOAD 0x12
0x02FC: 40 47 CMPEQ 0x47
0x02FE: 42 46 JZ 0x46 ; -> 0x0346
0x0300: 11 13 LOAD 0x13
0x0302: 40 7C CMPEQ 0x7C
0x0304: 42 40 JZ 0x40 ; -> 0x0346
0x0306: 11 14 LOAD 0x14
0x0308: 40 69 CMPEQ 0x69
0x030A: 42 3A JZ 0x3A ; -> 0x0346
0x030C: 11 15 LOAD 0x15
0x030E: 40 50 CMPEQ 0x50
0x0310: 42 34 JZ 0x34 ; -> 0x0346
0x0312: 11 16 LOAD 0x16
0x0314: 40 6A CMPEQ 0x6A
0x0316: 42 2E JZ 0x2E ; -> 0x0346
0x0318: 11 17 LOAD 0x17
0x031A: 40 73 CMPEQ 0x73
0x031C: 42 28 JZ 0x28 ; -> 0x0346
0x031E: 11 18 LOAD 0x18
0x0320: 40 6F CMPEQ 0x6F
0x0322: 42 22 JZ 0x22 ; -> 0x0346
0x0324: 11 19 LOAD 0x19
0x0326: 40 82 CMPEQ 0x82
0x0328: 42 1C JZ 0x1C ; -> 0x0346
0x032A: 11 1A LOAD 0x1A
0x032C: 40 BF CMPEQ 0xBF
0x032E: 42 16 JZ 0x16 ; -> 0x0346
```

上述部分的exp

```py
startNum = 7
cipher = [
    0x29, 0x47, 0x39, 0x1A, 0x3F, 0x50, 0x39, 0x26, 0x40, 0x5F,
    0x61, 0x63, 0x69, 0x38, 0x52, 0x71, 0x73, 0x60, 0x47, 0x7C,
    0x69, 0x50, 0x6A, 0x73, 0x6F, 0x82, 0xBF
]
plain = ""
xorNum = 0x55
for i in range(0, len(cipher)):
    plain += chr((cipher[i] - startNum) ^ xorNum)
    startNum += 3
print(plain)
```

这是fake flag：`why_you_think_this_is_true?`

### real flag

我不确定如果自己找，是否能快速找到真正的opcode。main.py里面的东西很可能不是真正被执行的，或者运行过程中被hook了。

#### 如何找到real opcode

##### 观察main.exe的行为

- 方案一

  通过Procmon观察main.exe

  ```plaintext
  20:51:05.7922816	main.exe	14212	IRP_MJ_CREATE	E:\_CTF\题目\POLARIS CTF 2026\FunPyVM	SUCCESS	Desired Access: Execute/Traverse, Synchronize, Disposition: Open, Options: Directory, Synchronous IO Non-Alert, Attributes: n/a, ShareMode: Read, Write, AllocationSize: n/a, OpenResult: Opened
  20:51:06.8215146	main.exe	14212	IRP_MJ_CREATE	E:\_CTF\题目\POLARIS CTF 2026\FunPyVM	SUCCESS	Desired Access: Read Data/List Directory, Synchronize, Disposition: Open, Options: Directory, Synchronous IO Non-Alert, Attributes: n/a, ShareMode: Read, Write, Delete, AllocationSize: n/a, OpenResult: Opened
  20:51:06.8215906	main.exe	14212	IRP_MJ_CLEANUP	E:\_CTF\题目\POLARIS CTF 2026\FunPyVM	SUCCESS	
  20:51:06.8216038	main.exe	14212	IRP_MJ_CLOSE	E:\_CTF\题目\POLARIS CTF 2026\FunPyVM	SUCCESS	
  20:51:06.9695889	main.exe	7540	IRP_MJ_CREATE	E:\_CTF\题目\POLARIS CTF 2026\FunPyVM	SUCCESS	Desired Access: Execute/Traverse, Synchronize, Disposition: Open, Options: Directory, Synchronous IO Non-Alert, Attributes: n/a, ShareMode: Read, Write, AllocationSize: n/a, OpenResult: Opened
  20:51:06.9777758	main.exe	7540	IRP_MJ_CREATE	E:\_CTF\题目\POLARIS CTF 2026\FunPyVM	SUCCESS	Desired Access: Read Data/List Directory, Synchronize, Disposition: Open, Options: Directory, Synchronous IO Non-Alert, Attributes: n/a, ShareMode: Read, Write, Delete, AllocationSize: n/a, OpenResult: Opened
  20:51:06.9778540	main.exe	7540	IRP_MJ_CLEANUP	E:\_CTF\题目\POLARIS CTF 2026\FunPyVM	SUCCESS	
  20:51:06.9779607	main.exe	7540	IRP_MJ_CLOSE	E:\_CTF\题目\POLARIS CTF 2026\FunPyVM	SUCCESS	
  20:51:17.7729976	main.exe	7540	IRP_MJ_CLEANUP	E:\_CTF\题目\POLARIS CTF 2026\FunPyVM	SUCCESS	
  20:51:17.7730200	main.exe	7540	IRP_MJ_CLOSE	E:\_CTF\题目\POLARIS CTF 2026\FunPyVM	SUCCESS	
  ```

  会发现main.exe没有读取任何外部文件。说明real opcode在其程序内部，opcode.bin是障眼法。

- 方案二

  直接创建个新的空文件，命名为opcode.bin，会发现main.exe仍然能正常运行。

  ![](/assets/img/media/2026_4_20/image-20260416143014-sb40yzj.png)

##### 寻找解包的文件

```plaintext
 E:\_CTF\题目\POLARIS CTF 2026\FunPyVM\main.exe_extracted 的目录

2026/03/29  10:19    <DIR>          .
2026/04/16  14:29    <DIR>          ..
2026/03/29  10:18            22,888 api-ms-win-core-console-l1-1-0.dll
2026/03/29  10:18            22,872 api-ms-win-core-datetime-l1-1-0.dll
2026/03/29  10:18            22,872 api-ms-win-core-debug-l1-1-0.dll
2026/03/29  10:18            22,896 api-ms-win-core-errorhandling-l1-1-0.dll
2026/03/29  10:18            22,872 api-ms-win-core-fibers-l1-1-0.dll
2026/03/29  10:18            27,008 api-ms-win-core-file-l1-1-0.dll
2026/03/29  10:18            22,872 api-ms-win-core-file-l1-2-0.dll
2026/03/29  10:18            20,960 api-ms-win-core-file-l2-1-0.dll
2026/03/29  10:18            22,872 api-ms-win-core-handle-l1-1-0.dll
2026/03/29  10:18            22,872 api-ms-win-core-heap-l1-1-0.dll
2026/03/29  10:18            22,872 api-ms-win-core-interlocked-l1-1-0.dll
2026/03/29  10:18            22,872 api-ms-win-core-libraryloader-l1-1-0.dll
2026/03/29  10:18            22,872 api-ms-win-core-localization-l1-2-0.dll
2026/03/29  10:18            22,872 api-ms-win-core-memory-l1-1-0.dll
2026/03/29  10:18            22,872 api-ms-win-core-namedpipe-l1-1-0.dll
2026/03/29  10:18            22,872 api-ms-win-core-processenvironment-l1-1-0.dll
2026/03/29  10:18            22,896 api-ms-win-core-processthreads-l1-1-0.dll
2026/03/29  10:18            22,896 api-ms-win-core-processthreads-l1-1-1.dll
2026/03/29  10:18            22,888 api-ms-win-core-profile-l1-1-0.dll
2026/03/29  10:18            22,872 api-ms-win-core-rtlsupport-l1-1-0.dll
2026/03/29  10:18            22,888 api-ms-win-core-string-l1-1-0.dll
2026/03/29  10:18            22,872 api-ms-win-core-synch-l1-1-0.dll
2026/03/29  10:18            22,872 api-ms-win-core-synch-l1-2-0.dll
2026/03/29  10:18            22,872 api-ms-win-core-sysinfo-l1-1-0.dll
2026/03/29  10:18            22,872 api-ms-win-core-timezone-l1-1-0.dll
2026/03/29  10:18            22,872 api-ms-win-core-util-l1-1-0.dll
2026/03/29  10:18            22,888 api-ms-win-crt-conio-l1-1-0.dll
2026/03/29  10:18            26,968 api-ms-win-crt-convert-l1-1-0.dll
2026/03/29  10:18            22,872 api-ms-win-crt-environment-l1-1-0.dll
2026/03/29  10:18            22,872 api-ms-win-crt-filesystem-l1-1-0.dll
2026/03/29  10:18            22,872 api-ms-win-crt-heap-l1-1-0.dll
2026/03/29  10:18            22,912 api-ms-win-crt-locale-l1-1-0.dll
2026/03/29  10:18            31,064 api-ms-win-crt-math-l1-1-0.dll
2026/03/29  10:18            22,912 api-ms-win-crt-process-l1-1-0.dll
2026/03/29  10:18            26,968 api-ms-win-crt-runtime-l1-1-0.dll
2026/03/29  10:18            26,968 api-ms-win-crt-stdio-l1-1-0.dll
2026/03/29  10:18            26,968 api-ms-win-crt-string-l1-1-0.dll
2026/03/29  10:18            22,872 api-ms-win-crt-time-l1-1-0.dll
2026/03/29  10:18            22,872 api-ms-win-crt-utility-l1-1-0.dll
2026/03/29  10:18         1,402,463 base_library.zip
2026/03/29  10:17    <DIR>          bitarray
2026/03/29  10:18         1,633,136 libcrypto-3.dll
2026/03/29  10:18            29,968 libffi-8.dll
2026/03/29  10:18           229,232 libssl-3.dll
2026/03/29  10:18             1,356 main.pyc
2026/03/29  10:18             2,390 ntbase.pyd
2026/03/29  10:18            94,040 pyexpat.pyd
2026/03/29  10:18             1,897 pyiboot01_bootstrap.pyc
2026/03/29  10:18             4,930 pyimod01_archive.pyc
2026/03/29  10:18            31,802 pyimod02_importers.pyc
2026/03/29  10:18             6,450 pyimod03_ctypes.pyc
2026/03/29  10:18             1,679 pyimod04_pywin32.pyc
2026/03/29  10:18             2,795 pyi_rth_inspect.pyc
2026/03/29  10:18             1,880 pyi_rth_multiprocessing.pyc
2026/03/29  10:18             1,559 pyi_rth_pkgutil.pyc
2026/03/29  10:18         1,858,904 python313.dll
2026/03/29  10:18         2,582,447 PYZ.pyz
2026/03/29  10:18    <DIR>          PYZ.pyz_extracted
2026/03/29  10:18            28,504 select.pyd
2026/03/29  10:18               305 struct.pyc
2026/03/29  10:18         1,122,768 ucrtbase.dll
2026/03/29  10:18           268,632 unicodedata.pyd
2026/03/29  10:18           120,400 VCRUNTIME140.dll
2026/03/29  10:18            49,776 VCRUNTIME140_1.dll
2026/03/29  10:18            40,792 _asyncio.pyd
2026/03/29  10:18            52,056 _bz2.pyd
2026/03/29  10:18            65,880 _ctypes.pyd
2026/03/29  10:18           123,224 _decimal.pyd
2026/03/29  10:18            39,256 _hashlib.pyd
2026/03/29  10:18            89,944 _lzma.pyd
2026/03/29  10:18            30,040 _multiprocessing.pyd
2026/03/29  10:18            36,696 _overlapped.pyd
2026/03/29  10:18            29,016 _queue.pyd
2026/03/29  10:18            47,448 _socket.pyd
2026/03/29  10:18            70,488 _ssl.pyd
2026/03/29  10:18            31,576 _wmi.pyd
```

​`bitarray`​和`PYZ.pyz_extracted`目录中很显然，里面的文件应该都不是opcode。

> ## 参考题目:FunPyVM和思路
>
> 对于python题，如果得到了fake flag，需要关注其他文件。
>
> pyd文件：PE文件，大小一般在10kb以上。10kb以下的很可能有问题，与flag有关。
>
> pyc：看main.pyc(一般是)用了什么就去反编译什么
>
> dll：例如，不会出现`ntbase.pyd`这种东西。ntbase是一个dll，不可能是pyd。

可以观察到`2,390 ntbase.pyd`明显大小有异常。

![](/assets/img/media/2026_4_20/image-20260416144147-a0enth1.png)

其文件很明显是opcode，不是正常的PE文件。所以这就是opcode。

#### 解题

修改main.exe去指定读取这个文件

```py
# Decompiled with PyLingual (https://pylingual.io)
# Internal filename: 'main.py'
# Bytecode version: 3.13.0rc3 (3571)
# Source timestamp: 1970-01-01 00:00:00 UTC (0)

import sys
import os
import bitstring
from kernelVM import CustomVM

if __name__ == '__main__':
    if getattr(sys, 'frozen', False):
        current_dir = os.path.dirname(sys.executable)
    else:
        current_dir = os.path.dirname(os.path.abspath(__file__))
    filename = os.path.join(current_dir, 'ntbase.pyd')
    try:
        stream = bitstring.ConstBitStream(filename=filename)
        bytecode = stream.tobytes()
    except Exception as e:
        print('Error: Could not read \'opcode.bin\' in the current directory.')
        print('Please ensure \'opcode.bin\' is placed next to the executable.')
        sys.exit(1)
    # print(bytecode)
    vm = CustomVM()
    print('--- VM Start ---')
    vm.run(bytecode)
    print('\n--- VM End ---')

```

然后看trace的结果

- 第一版

  ```plaintext
  E:\_CTF\模板\.venv\Scripts\python.exe "E:\_CTF\题目\POLARIS CTF 2026\FunPyVM\decompiled_main.py" 
  --- VM Start ---
  ; size = 2390 bytes
  ; instructions = 1370

  0x0000: 10 64 ALLOC 0x64
  0x0002: 10 32 ALLOC 0x32
  0x0004: 20 00 MOVI 0x00
  0x0006: 51    XCHG
  0x0007: 12 63 STORE 0x63
  0x0009: 11 01 LOAD 0x01
  0x000B: 50 63 SWAP 0x63
  0x000D: 51    XCHG
  0x000E: 11 63 LOAD 0x63
  0x0010: 51    XCHG
  0x0011: 12 63 STORE 0x63
  0x0013: 20 00 MOVI 0x00
  0x0015: 51    XCHG
  0x0016: 12 02 STORE 0x02
  0x0018: 11 63 LOAD 0x63
  0x001A: 51    XCHG
  0x001B: 20 42 MOVI 0x42
  0x001D: 30 13 ADDI 0x13
  0x001F: 12 63 STORE 0x63
  0x0021: 20 00 MOVI 0x00
  0x0023: 51    XCHG
  0x0024: 12 63 STORE 0x63
  0x0026: 11 02 LOAD 0x02
  0x0028: 50 63 SWAP 0x63
  0x002A: 51    XCHG
  0x002B: 11 63 LOAD 0x63
  0x002D: 51    XCHG
  0x002E: 12 63 STORE 0x63
  0x0030: 20 00 MOVI 0x00
  0x0032: 51    XCHG
  0x0033: 11 01 LOAD 0x01
  0x0035: 51    XCHG
  0x0036: 11 63 LOAD 0x63
  0x0038: 51    XCHG
  0x0039: 52 01 SYSCALL 0x01 ; READ_INPUT
  0x003B: 11 00 LOAD 0x00
  0x003D: 40 00 CMPEQ 0x00
  0x003F: 43 06 JNZ 0x06 ; -> 0x0047
  0x0041: 11 00 LOAD 0x00
  0x0043: 30 0A ADDI 0x0A
  0x0045: 12 00 STORE 0x00
  0x0047: 11 01 LOAD 0x01
  0x0049: 40 00 CMPEQ 0x00
  0x004B: 43 3F JNZ 0x3F ; -> 0x008C
  0x004D: 11 01 LOAD 0x01
  0x004F: 30 0A ADDI 0x0A
  0x0051: 12 01 STORE 0x01
  0x0053: 11 00 LOAD 0x00
  0x0055: 20 00 MOVI 0x00
  0x0057: 51    XCHG
  0x0058: 51    XCHG
  0x0059: 12 63 STORE 0x63
  0x005B: 20 00 MOVI 0x00
  0x005D: 51    XCHG
  0x005E: 12 02 STORE 0x02
  0x0060: 11 63 LOAD 0x63
  0x0062: 51    XCHG
  0x0063: 20 00 MOVI 0x00
  0x0065: 51    XCHG
  0x0066: 12 63 STORE 0x63
  0x0068: 11 01 LOAD 0x01
  0x006A: 50 63 SWAP 0x63
  0x006C: 51    XCHG
  0x006D: 11 63 LOAD 0x63
  0x006F: 51    XCHG
  0x0070: 12 63 STORE 0x63
  0x0072: 20 00 MOVI 0x00
  0x0074: 51    XCHG
  0x0075: 11 01 LOAD 0x01
  0x0077: 51    XCHG
  0x0078: 11 63 LOAD 0x63
  0x007A: 51    XCHG
  0x007B: 20 00 MOVI 0x00
  0x007D: 51    XCHG
  0x007E: 12 63 STORE 0x63
  0x0080: 11 02 LOAD 0x02
  0x0082: 50 63 SWAP 0x63
  0x0084: 51    XCHG
  0x0085: 11 63 LOAD 0x63
  0x0087: 51    XCHG
  0x0088: 11 00 LOAD 0x00
  0x008A: 35 01 XOR_MEM 0x01
  0x008C: 11 02 LOAD 0x02
  0x008E: 40 00 CMPEQ 0x00
  0x0090: 43 3F JNZ 0x3F ; -> 0x00D1
  0x0092: 11 02 LOAD 0x02
  0x0094: 30 0A ADDI 0x0A
  0x0096: 12 02 STORE 0x02
  0x0098: 11 01 LOAD 0x01
  0x009A: 20 00 MOVI 0x00
  0x009C: 51    XCHG
  0x009D: 51    XCHG
  0x009E: 12 63 STORE 0x63
  0x00A0: 20 00 MOVI 0x00
  0x00A2: 51    XCHG
  0x00A3: 12 02 STORE 0x02
  0x00A5: 11 63 LOAD 0x63
  0x00A7: 51    XCHG
  0x00A8: 20 00 MOVI 0x00
  0x00AA: 51    XCHG
  0x00AB: 12 63 STORE 0x63
  0x00AD: 11 01 LOAD 0x01
  0x00AF: 50 63 SWAP 0x63
  0x00B1: 51    XCHG
  0x00B2: 11 63 LOAD 0x63
  0x00B4: 51    XCHG
  0x00B5: 12 63 STORE 0x63
  0x00B7: 20 00 MOVI 0x00
  0x00B9: 51    XCHG
  0x00BA: 11 01 LOAD 0x01
  0x00BC: 51    XCHG
  0x00BD: 11 63 LOAD 0x63
  0x00BF: 51    XCHG
  0x00C0: 20 00 MOVI 0x00
  0x00C2: 51    XCHG
  0x00C3: 12 63 STORE 0x63
  0x00C5: 11 02 LOAD 0x02
  0x00C7: 50 63 SWAP 0x63
  0x00C9: 51    XCHG
  0x00CA: 11 63 LOAD 0x63
  0x00CC: 51    XCHG
  0x00CD: 11 01 LOAD 0x01
  0x00CF: 35 02 XOR_MEM 0x02
  0x00D1: 11 03 LOAD 0x03
  0x00D3: 40 00 CMPEQ 0x00
  0x00D5: 43 3F JNZ 0x3F ; -> 0x0116
  0x00D7: 11 03 LOAD 0x03
  0x00D9: 30 0A ADDI 0x0A
  0x00DB: 12 03 STORE 0x03
  0x00DD: 11 02 LOAD 0x02
  0x00DF: 20 00 MOVI 0x00
  0x00E1: 51    XCHG
  0x00E2: 51    XCHG
  0x00E3: 12 63 STORE 0x63
  0x00E5: 20 00 MOVI 0x00
  0x00E7: 51    XCHG
  0x00E8: 12 02 STORE 0x02
  0x00EA: 11 63 LOAD 0x63
  0x00EC: 51    XCHG
  0x00ED: 20 00 MOVI 0x00
  0x00EF: 51    XCHG
  0x00F0: 12 63 STORE 0x63
  0x00F2: 11 01 LOAD 0x01
  0x00F4: 50 63 SWAP 0x63
  0x00F6: 51    XCHG
  0x00F7: 11 63 LOAD 0x63
  0x00F9: 51    XCHG
  0x00FA: 12 63 STORE 0x63
  0x00FC: 20 00 MOVI 0x00
  0x00FE: 51    XCHG
  0x00FF: 11 01 LOAD 0x01
  0x0101: 51    XCHG
  0x0102: 11 63 LOAD 0x63
  0x0104: 51    XCHG
  0x0105: 20 00 MOVI 0x00
  0x0107: 51    XCHG
  0x0108: 12 63 STORE 0x63
  0x010A: 11 02 LOAD 0x02
  0x010C: 50 63 SWAP 0x63
  0x010E: 51    XCHG
  0x010F: 11 63 LOAD 0x63
  0x0111: 51    XCHG
  0x0112: 11 02 LOAD 0x02
  0x0114: 35 03 XOR_MEM 0x03
  0x0116: 11 04 LOAD 0x04
  0x0118: 40 00 CMPEQ 0x00
  0x011A: 43 3F JNZ 0x3F ; -> 0x015B
  0x011C: 11 04 LOAD 0x04
  0x011E: 30 0A ADDI 0x0A
  0x0120: 12 04 STORE 0x04
  0x0122: 11 03 LOAD 0x03
  0x0124: 20 00 MOVI 0x00
  0x0126: 51    XCHG
  0x0127: 51    XCHG
  0x0128: 12 63 STORE 0x63
  0x012A: 20 00 MOVI 0x00
  0x012C: 51    XCHG
  0x012D: 12 02 STORE 0x02
  0x012F: 11 63 LOAD 0x63
  0x0131: 51    XCHG
  0x0132: 20 00 MOVI 0x00
  0x0134: 51    XCHG
  0x0135: 12 63 STORE 0x63
  0x0137: 11 01 LOAD 0x01
  0x0139: 50 63 SWAP 0x63
  0x013B: 51    XCHG
  0x013C: 11 63 LOAD 0x63
  0x013E: 51    XCHG
  0x013F: 12 63 STORE 0x63
  0x0141: 20 00 MOVI 0x00
  0x0143: 51    XCHG
  0x0144: 11 01 LOAD 0x01
  0x0146: 51    XCHG
  0x0147: 11 63 LOAD 0x63
  0x0149: 51    XCHG
  0x014A: 20 00 MOVI 0x00
  0x014C: 51    XCHG
  0x014D: 12 63 STORE 0x63
  0x014F: 11 02 LOAD 0x02
  0x0151: 50 63 SWAP 0x63
  0x0153: 51    XCHG
  0x0154: 11 63 LOAD 0x63
  0x0156: 51    XCHG
  0x0157: 11 03 LOAD 0x03
  0x0159: 35 04 XOR_MEM 0x04
  0x015B: 11 05 LOAD 0x05
  0x015D: 40 00 CMPEQ 0x00
  0x015F: 43 3F JNZ 0x3F ; -> 0x01A0
  0x0161: 11 05 LOAD 0x05
  0x0163: 30 0A ADDI 0x0A
  0x0165: 12 05 STORE 0x05
  0x0167: 11 04 LOAD 0x04
  0x0169: 20 00 MOVI 0x00
  0x016B: 51    XCHG
  0x016C: 51    XCHG
  0x016D: 12 63 STORE 0x63
  0x016F: 20 00 MOVI 0x00
  0x0171: 51    XCHG
  0x0172: 12 02 STORE 0x02
  0x0174: 11 63 LOAD 0x63
  0x0176: 51    XCHG
  0x0177: 20 00 MOVI 0x00
  0x0179: 51    XCHG
  0x017A: 12 63 STORE 0x63
  0x017C: 11 01 LOAD 0x01
  0x017E: 50 63 SWAP 0x63
  0x0180: 51    XCHG
  0x0181: 11 63 LOAD 0x63
  0x0183: 51    XCHG
  0x0184: 12 63 STORE 0x63
  0x0186: 20 00 MOVI 0x00
  0x0188: 51    XCHG
  0x0189: 11 01 LOAD 0x01
  0x018B: 51    XCHG
  0x018C: 11 63 LOAD 0x63
  0x018E: 51    XCHG
  0x018F: 20 00 MOVI 0x00
  0x0191: 51    XCHG
  0x0192: 12 63 STORE 0x63
  0x0194: 11 02 LOAD 0x02
  0x0196: 50 63 SWAP 0x63
  0x0198: 51    XCHG
  0x0199: 11 63 LOAD 0x63
  0x019B: 51    XCHG
  0x019C: 11 04 LOAD 0x04
  0x019E: 35 05 XOR_MEM 0x05
  0x01A0: 11 06 LOAD 0x06
  0x01A2: 40 00 CMPEQ 0x00
  0x01A4: 43 3F JNZ 0x3F ; -> 0x01E5
  0x01A6: 11 06 LOAD 0x06
  0x01A8: 30 0A ADDI 0x0A
  0x01AA: 12 06 STORE 0x06
  0x01AC: 11 05 LOAD 0x05
  0x01AE: 20 00 MOVI 0x00
  0x01B0: 51    XCHG
  0x01B1: 51    XCHG
  0x01B2: 12 63 STORE 0x63
  0x01B4: 20 00 MOVI 0x00
  0x01B6: 51    XCHG
  0x01B7: 12 02 STORE 0x02
  0x01B9: 11 63 LOAD 0x63
  0x01BB: 51    XCHG
  0x01BC: 20 00 MOVI 0x00
  0x01BE: 51    XCHG
  0x01BF: 12 63 STORE 0x63
  0x01C1: 11 01 LOAD 0x01
  0x01C3: 50 63 SWAP 0x63
  0x01C5: 51    XCHG
  0x01C6: 11 63 LOAD 0x63
  0x01C8: 51    XCHG
  0x01C9: 12 63 STORE 0x63
  0x01CB: 20 00 MOVI 0x00
  0x01CD: 51    XCHG
  0x01CE: 11 01 LOAD 0x01
  0x01D0: 51    XCHG
  0x01D1: 11 63 LOAD 0x63
  0x01D3: 51    XCHG
  0x01D4: 20 00 MOVI 0x00
  0x01D6: 51    XCHG
  0x01D7: 12 63 STORE 0x63
  0x01D9: 11 02 LOAD 0x02
  0x01DB: 50 63 SWAP 0x63
  0x01DD: 51    XCHG
  0x01DE: 11 63 LOAD 0x63
  0x01E0: 51    XCHG
  0x01E1: 11 05 LOAD 0x05
  0x01E3: 35 06 XOR_MEM 0x06
  0x01E5: 11 07 LOAD 0x07
  0x01E7: 40 00 CMPEQ 0x00
  0x01E9: 43 3F JNZ 0x3F ; -> 0x022A
  0x01EB: 11 07 LOAD 0x07
  0x01ED: 30 0A ADDI 0x0A
  0x01EF: 12 07 STORE 0x07
  0x01F1: 11 06 LOAD 0x06
  0x01F3: 20 00 MOVI 0x00
  0x01F5: 51    XCHG
  0x01F6: 51    XCHG
  0x01F7: 12 63 STORE 0x63
  0x01F9: 20 00 MOVI 0x00
  0x01FB: 51    XCHG
  0x01FC: 12 02 STORE 0x02
  0x01FE: 11 63 LOAD 0x63
  0x0200: 51    XCHG
  0x0201: 20 00 MOVI 0x00
  0x0203: 51    XCHG
  0x0204: 12 63 STORE 0x63
  0x0206: 11 01 LOAD 0x01
  0x0208: 50 63 SWAP 0x63
  0x020A: 51    XCHG
  0x020B: 11 63 LOAD 0x63
  0x020D: 51    XCHG
  0x020E: 12 63 STORE 0x63
  0x0210: 20 00 MOVI 0x00
  0x0212: 51    XCHG
  0x0213: 11 01 LOAD 0x01
  0x0215: 51    XCHG
  0x0216: 11 63 LOAD 0x63
  0x0218: 51    XCHG
  0x0219: 20 00 MOVI 0x00
  0x021B: 51    XCHG
  0x021C: 12 63 STORE 0x63
  0x021E: 11 02 LOAD 0x02
  0x0220: 50 63 SWAP 0x63
  0x0222: 51    XCHG
  0x0223: 11 63 LOAD 0x63
  0x0225: 51    XCHG
  0x0226: 11 06 LOAD 0x06
  0x0228: 35 07 XOR_MEM 0x07
  0x022A: 11 08 LOAD 0x08
  0x022C: 40 00 CMPEQ 0x00
  0x022E: 43 3F JNZ 0x3F ; -> 0x026F
  0x0230: 11 08 LOAD 0x08
  0x0232: 30 0A ADDI 0x0A
  0x0234: 12 08 STORE 0x08
  0x0236: 11 07 LOAD 0x07
  0x0238: 20 00 MOVI 0x00
  0x023A: 51    XCHG
  0x023B: 51    XCHG
  0x023C: 12 63 STORE 0x63
  0x023E: 20 00 MOVI 0x00
  0x0240: 51    XCHG
  0x0241: 12 02 STORE 0x02
  0x0243: 11 63 LOAD 0x63
  0x0245: 51    XCHG
  0x0246: 20 00 MOVI 0x00
  0x0248: 51    XCHG
  0x0249: 12 63 STORE 0x63
  0x024B: 11 01 LOAD 0x01
  0x024D: 50 63 SWAP 0x63
  0x024F: 51    XCHG
  0x0250: 11 63 LOAD 0x63
  0x0252: 51    XCHG
  0x0253: 12 63 STORE 0x63
  0x0255: 20 00 MOVI 0x00
  0x0257: 51    XCHG
  0x0258: 11 01 LOAD 0x01
  0x025A: 51    XCHG
  0x025B: 11 63 LOAD 0x63
  0x025D: 51    XCHG
  0x025E: 20 00 MOVI 0x00
  0x0260: 51    XCHG
  0x0261: 12 63 STORE 0x63
  0x0263: 11 02 LOAD 0x02
  0x0265: 50 63 SWAP 0x63
  0x0267: 51    XCHG
  0x0268: 11 63 LOAD 0x63
  0x026A: 51    XCHG
  0x026B: 11 07 LOAD 0x07
  0x026D: 35 08 XOR_MEM 0x08
  0x026F: 11 09 LOAD 0x09
  0x0271: 40 00 CMPEQ 0x00
  0x0273: 43 3F JNZ 0x3F ; -> 0x02B4
  0x0275: 11 09 LOAD 0x09
  0x0277: 30 0A ADDI 0x0A
  0x0279: 12 09 STORE 0x09
  0x027B: 11 08 LOAD 0x08
  0x027D: 20 00 MOVI 0x00
  0x027F: 51    XCHG
  0x0280: 51    XCHG
  0x0281: 12 63 STORE 0x63
  0x0283: 20 00 MOVI 0x00
  0x0285: 51    XCHG
  0x0286: 12 02 STORE 0x02
  0x0288: 11 63 LOAD 0x63
  0x028A: 51    XCHG
  0x028B: 20 00 MOVI 0x00
  0x028D: 51    XCHG
  0x028E: 12 63 STORE 0x63
  0x0290: 11 01 LOAD 0x01
  0x0292: 50 63 SWAP 0x63
  0x0294: 51    XCHG
  0x0295: 11 63 LOAD 0x63
  0x0297: 51    XCHG
  0x0298: 12 63 STORE 0x63
  0x029A: 20 00 MOVI 0x00
  0x029C: 51    XCHG
  0x029D: 11 01 LOAD 0x01
  0x029F: 51    XCHG
  0x02A0: 11 63 LOAD 0x63
  0x02A2: 51    XCHG
  0x02A3: 20 00 MOVI 0x00
  0x02A5: 51    XCHG
  0x02A6: 12 63 STORE 0x63
  0x02A8: 11 02 LOAD 0x02
  0x02AA: 50 63 SWAP 0x63
  0x02AC: 51    XCHG
  0x02AD: 11 63 LOAD 0x63
  0x02AF: 51    XCHG
  0x02B0: 11 08 LOAD 0x08
  0x02B2: 35 09 XOR_MEM 0x09
  0x02B4: 11 0A LOAD 0x0A
  0x02B6: 40 00 CMPEQ 0x00
  0x02B8: 43 3F JNZ 0x3F ; -> 0x02F9
  0x02BA: 11 0A LOAD 0x0A
  0x02BC: 30 0A ADDI 0x0A
  0x02BE: 12 0A STORE 0x0A
  0x02C0: 11 09 LOAD 0x09
  0x02C2: 20 00 MOVI 0x00
  0x02C4: 51    XCHG
  0x02C5: 51    XCHG
  0x02C6: 12 63 STORE 0x63
  0x02C8: 20 00 MOVI 0x00
  0x02CA: 51    XCHG
  0x02CB: 12 02 STORE 0x02
  0x02CD: 11 63 LOAD 0x63
  0x02CF: 51    XCHG
  0x02D0: 20 00 MOVI 0x00
  0x02D2: 51    XCHG
  0x02D3: 12 63 STORE 0x63
  0x02D5: 11 01 LOAD 0x01
  0x02D7: 50 63 SWAP 0x63
  0x02D9: 51    XCHG
  0x02DA: 11 63 LOAD 0x63
  0x02DC: 51    XCHG
  0x02DD: 12 63 STORE 0x63
  0x02DF: 20 00 MOVI 0x00
  0x02E1: 51    XCHG
  0x02E2: 11 01 LOAD 0x01
  0x02E4: 51    XCHG
  0x02E5: 11 63 LOAD 0x63
  0x02E7: 51    XCHG
  0x02E8: 20 00 MOVI 0x00
  0x02EA: 51    XCHG
  0x02EB: 12 63 STORE 0x63
  0x02ED: 11 02 LOAD 0x02
  0x02EF: 50 63 SWAP 0x63
  0x02F1: 51    XCHG
  0x02F2: 11 63 LOAD 0x63
  0x02F4: 51    XCHG
  0x02F5: 11 09 LOAD 0x09
  0x02F7: 35 0A XOR_MEM 0x0A
  0x02F9: 11 0B LOAD 0x0B
  0x02FB: 40 00 CMPEQ 0x00
  0x02FD: 43 3F JNZ 0x3F ; -> 0x033E
  0x02FF: 11 0B LOAD 0x0B
  0x0301: 30 0A ADDI 0x0A
  0x0303: 12 0B STORE 0x0B
  0x0305: 11 0A LOAD 0x0A
  0x0307: 20 00 MOVI 0x00
  0x0309: 51    XCHG
  0x030A: 51    XCHG
  0x030B: 12 63 STORE 0x63
  0x030D: 20 00 MOVI 0x00
  0x030F: 51    XCHG
  0x0310: 12 02 STORE 0x02
  0x0312: 11 63 LOAD 0x63
  0x0314: 51    XCHG
  0x0315: 20 00 MOVI 0x00
  0x0317: 51    XCHG
  0x0318: 12 63 STORE 0x63
  0x031A: 11 01 LOAD 0x01
  0x031C: 50 63 SWAP 0x63
  0x031E: 51    XCHG
  0x031F: 11 63 LOAD 0x63
  0x0321: 51    XCHG
  0x0322: 12 63 STORE 0x63
  0x0324: 20 00 MOVI 0x00
  0x0326: 51    XCHG
  0x0327: 11 01 LOAD 0x01
  0x0329: 51    XCHG
  0x032A: 11 63 LOAD 0x63
  0x032C: 51    XCHG
  0x032D: 20 00 MOVI 0x00
  0x032F: 51    XCHG
  0x0330: 12 63 STORE 0x63
  0x0332: 11 02 LOAD 0x02
  0x0334: 50 63 SWAP 0x63
  0x0336: 51    XCHG
  0x0337: 11 63 LOAD 0x63
  0x0339: 51    XCHG
  0x033A: 11 0A LOAD 0x0A
  0x033C: 35 0B XOR_MEM 0x0B
  0x033E: 11 0C LOAD 0x0C
  0x0340: 40 00 CMPEQ 0x00
  0x0342: 43 3F JNZ 0x3F ; -> 0x0383
  0x0344: 11 0C LOAD 0x0C
  0x0346: 30 0A ADDI 0x0A
  0x0348: 12 0C STORE 0x0C
  0x034A: 11 0B LOAD 0x0B
  0x034C: 20 00 MOVI 0x00
  0x034E: 51    XCHG
  0x034F: 51    XCHG
  0x0350: 12 63 STORE 0x63
  0x0352: 20 00 MOVI 0x00
  0x0354: 51    XCHG
  0x0355: 12 02 STORE 0x02
  0x0357: 11 63 LOAD 0x63
  0x0359: 51    XCHG
  0x035A: 20 00 MOVI 0x00
  0x035C: 51    XCHG
  0x035D: 12 63 STORE 0x63
  0x035F: 11 01 LOAD 0x01
  0x0361: 50 63 SWAP 0x63
  0x0363: 51    XCHG
  0x0364: 11 63 LOAD 0x63
  0x0366: 51    XCHG
  0x0367: 12 63 STORE 0x63
  0x0369: 20 00 MOVI 0x00
  0x036B: 51    XCHG
  0x036C: 11 01 LOAD 0x01
  0x036E: 51    XCHG
  0x036F: 11 63 LOAD 0x63
  0x0371: 51    XCHG
  0x0372: 20 00 MOVI 0x00
  0x0374: 51    XCHG
  0x0375: 12 63 STORE 0x63
  0x0377: 11 02 LOAD 0x02
  0x0379: 50 63 SWAP 0x63
  0x037B: 51    XCHG
  0x037C: 11 63 LOAD 0x63
  0x037E: 51    XCHG
  0x037F: 11 0B LOAD 0x0B
  0x0381: 35 0C XOR_MEM 0x0C
  0x0383: 11 0D LOAD 0x0D
  0x0385: 40 00 CMPEQ 0x00
  0x0387: 43 3F JNZ 0x3F ; -> 0x03C8
  0x0389: 11 0D LOAD 0x0D
  0x038B: 30 0A ADDI 0x0A
  0x038D: 12 0D STORE 0x0D
  0x038F: 11 0C LOAD 0x0C
  0x0391: 20 00 MOVI 0x00
  0x0393: 51    XCHG
  0x0394: 51    XCHG
  0x0395: 12 63 STORE 0x63
  0x0397: 20 00 MOVI 0x00
  0x0399: 51    XCHG
  0x039A: 12 02 STORE 0x02
  0x039C: 11 63 LOAD 0x63
  0x039E: 51    XCHG
  0x039F: 20 00 MOVI 0x00
  0x03A1: 51    XCHG
  0x03A2: 12 63 STORE 0x63
  0x03A4: 11 01 LOAD 0x01
  0x03A6: 50 63 SWAP 0x63
  0x03A8: 51    XCHG
  0x03A9: 11 63 LOAD 0x63
  0x03AB: 51    XCHG
  0x03AC: 12 63 STORE 0x63
  0x03AE: 20 00 MOVI 0x00
  0x03B0: 51    XCHG
  0x03B1: 11 01 LOAD 0x01
  0x03B3: 51    XCHG
  0x03B4: 11 63 LOAD 0x63
  0x03B6: 51    XCHG
  0x03B7: 20 00 MOVI 0x00
  0x03B9: 51    XCHG
  0x03BA: 12 63 STORE 0x63
  0x03BC: 11 02 LOAD 0x02
  0x03BE: 50 63 SWAP 0x63
  0x03C0: 51    XCHG
  0x03C1: 11 63 LOAD 0x63
  0x03C3: 51    XCHG
  0x03C4: 11 0C LOAD 0x0C
  0x03C6: 35 0D XOR_MEM 0x0D
  0x03C8: 11 0E LOAD 0x0E
  0x03CA: 40 00 CMPEQ 0x00
  0x03CC: 43 3F JNZ 0x3F ; -> 0x040D
  0x03CE: 11 0E LOAD 0x0E
  0x03D0: 30 0A ADDI 0x0A
  0x03D2: 12 0E STORE 0x0E
  0x03D4: 11 0D LOAD 0x0D
  0x03D6: 20 00 MOVI 0x00
  0x03D8: 51    XCHG
  0x03D9: 51    XCHG
  0x03DA: 12 63 STORE 0x63
  0x03DC: 20 00 MOVI 0x00
  0x03DE: 51    XCHG
  0x03DF: 12 02 STORE 0x02
  0x03E1: 11 63 LOAD 0x63
  0x03E3: 51    XCHG
  0x03E4: 20 00 MOVI 0x00
  0x03E6: 51    XCHG
  0x03E7: 12 63 STORE 0x63
  0x03E9: 11 01 LOAD 0x01
  0x03EB: 50 63 SWAP 0x63
  0x03ED: 51    XCHG
  0x03EE: 11 63 LOAD 0x63
  0x03F0: 51    XCHG
  0x03F1: 12 63 STORE 0x63
  0x03F3: 20 00 MOVI 0x00
  0x03F5: 51    XCHG
  0x03F6: 11 01 LOAD 0x01
  0x03F8: 51    XCHG
  0x03F9: 11 63 LOAD 0x63
  0x03FB: 51    XCHG
  0x03FC: 20 00 MOVI 0x00
  0x03FE: 51    XCHG
  0x03FF: 12 63 STORE 0x63
  0x0401: 11 02 LOAD 0x02
  0x0403: 50 63 SWAP 0x63
  0x0405: 51    XCHG
  0x0406: 11 63 LOAD 0x63
  0x0408: 51    XCHG
  0x0409: 11 0D LOAD 0x0D
  0x040B: 35 0E XOR_MEM 0x0E
  0x040D: 11 0F LOAD 0x0F
  0x040F: 40 00 CMPEQ 0x00
  0x0411: 43 3F JNZ 0x3F ; -> 0x0452
  0x0413: 11 0F LOAD 0x0F
  0x0415: 30 0A ADDI 0x0A
  0x0417: 12 0F STORE 0x0F
  0x0419: 11 0E LOAD 0x0E
  0x041B: 20 00 MOVI 0x00
  0x041D: 51    XCHG
  0x041E: 51    XCHG
  0x041F: 12 63 STORE 0x63
  0x0421: 20 00 MOVI 0x00
  0x0423: 51    XCHG
  0x0424: 12 02 STORE 0x02
  0x0426: 11 63 LOAD 0x63
  0x0428: 51    XCHG
  0x0429: 20 00 MOVI 0x00
  0x042B: 51    XCHG
  0x042C: 12 63 STORE 0x63
  0x042E: 11 01 LOAD 0x01
  0x0430: 50 63 SWAP 0x63
  0x0432: 51    XCHG
  0x0433: 11 63 LOAD 0x63
  0x0435: 51    XCHG
  0x0436: 12 63 STORE 0x63
  0x0438: 20 00 MOVI 0x00
  0x043A: 51    XCHG
  0x043B: 11 01 LOAD 0x01
  0x043D: 51    XCHG
  0x043E: 11 63 LOAD 0x63
  0x0440: 51    XCHG
  0x0441: 20 00 MOVI 0x00
  0x0443: 51    XCHG
  0x0444: 12 63 STORE 0x63
  0x0446: 11 02 LOAD 0x02
  0x0448: 50 63 SWAP 0x63
  0x044A: 51    XCHG
  0x044B: 11 63 LOAD 0x63
  0x044D: 51    XCHG
  0x044E: 11 0E LOAD 0x0E
  0x0450: 35 0F XOR_MEM 0x0F
  0x0452: 11 10 LOAD 0x10
  0x0454: 40 00 CMPEQ 0x00
  0x0456: 43 3F JNZ 0x3F ; -> 0x0497
  0x0458: 11 10 LOAD 0x10
  0x045A: 30 0A ADDI 0x0A
  0x045C: 12 10 STORE 0x10
  0x045E: 11 0F LOAD 0x0F
  0x0460: 20 00 MOVI 0x00
  0x0462: 51    XCHG
  0x0463: 51    XCHG
  0x0464: 12 63 STORE 0x63
  0x0466: 20 00 MOVI 0x00
  0x0468: 51    XCHG
  0x0469: 12 02 STORE 0x02
  0x046B: 11 63 LOAD 0x63
  0x046D: 51    XCHG
  0x046E: 20 00 MOVI 0x00
  0x0470: 51    XCHG
  0x0471: 12 63 STORE 0x63
  0x0473: 11 01 LOAD 0x01
  0x0475: 50 63 SWAP 0x63
  0x0477: 51    XCHG
  0x0478: 11 63 LOAD 0x63
  0x047A: 51    XCHG
  0x047B: 12 63 STORE 0x63
  0x047D: 20 00 MOVI 0x00
  0x047F: 51    XCHG
  0x0480: 11 01 LOAD 0x01
  0x0482: 51    XCHG
  0x0483: 11 63 LOAD 0x63
  0x0485: 51    XCHG
  0x0486: 20 00 MOVI 0x00
  0x0488: 51    XCHG
  0x0489: 12 63 STORE 0x63
  0x048B: 11 02 LOAD 0x02
  0x048D: 50 63 SWAP 0x63
  0x048F: 51    XCHG
  0x0490: 11 63 LOAD 0x63
  0x0492: 51    XCHG
  0x0493: 11 0F LOAD 0x0F
  0x0495: 35 10 XOR_MEM 0x10
  0x0497: 11 11 LOAD 0x11
  0x0499: 40 00 CMPEQ 0x00
  0x049B: 43 3F JNZ 0x3F ; -> 0x04DC
  0x049D: 11 11 LOAD 0x11
  0x049F: 30 0A ADDI 0x0A
  0x04A1: 12 11 STORE 0x11
  0x04A3: 11 10 LOAD 0x10
  0x04A5: 20 00 MOVI 0x00
  0x04A7: 51    XCHG
  0x04A8: 51    XCHG
  0x04A9: 12 63 STORE 0x63
  0x04AB: 20 00 MOVI 0x00
  0x04AD: 51    XCHG
  0x04AE: 12 02 STORE 0x02
  0x04B0: 11 63 LOAD 0x63
  0x04B2: 51    XCHG
  0x04B3: 20 00 MOVI 0x00
  0x04B5: 51    XCHG
  0x04B6: 12 63 STORE 0x63
  0x04B8: 11 01 LOAD 0x01
  0x04BA: 50 63 SWAP 0x63
  0x04BC: 51    XCHG
  0x04BD: 11 63 LOAD 0x63
  0x04BF: 51    XCHG
  0x04C0: 12 63 STORE 0x63
  0x04C2: 20 00 MOVI 0x00
  0x04C4: 51    XCHG
  0x04C5: 11 01 LOAD 0x01
  0x04C7: 51    XCHG
  0x04C8: 11 63 LOAD 0x63
  0x04CA: 51    XCHG
  0x04CB: 20 00 MOVI 0x00
  0x04CD: 51    XCHG
  0x04CE: 12 63 STORE 0x63
  0x04D0: 11 02 LOAD 0x02
  0x04D2: 50 63 SWAP 0x63
  0x04D4: 51    XCHG
  0x04D5: 11 63 LOAD 0x63
  0x04D7: 51    XCHG
  0x04D8: 11 10 LOAD 0x10
  0x04DA: 35 11 XOR_MEM 0x11
  0x04DC: 11 12 LOAD 0x12
  0x04DE: 40 00 CMPEQ 0x00
  0x04E0: 43 3F JNZ 0x3F ; -> 0x0521
  0x04E2: 11 12 LOAD 0x12
  0x04E4: 30 0A ADDI 0x0A
  0x04E6: 12 12 STORE 0x12
  0x04E8: 11 11 LOAD 0x11
  0x04EA: 20 00 MOVI 0x00
  0x04EC: 51    XCHG
  0x04ED: 51    XCHG
  0x04EE: 12 63 STORE 0x63
  0x04F0: 20 00 MOVI 0x00
  0x04F2: 51    XCHG
  0x04F3: 12 02 STORE 0x02
  0x04F5: 11 63 LOAD 0x63
  0x04F7: 51    XCHG
  0x04F8: 20 00 MOVI 0x00
  0x04FA: 51    XCHG
  0x04FB: 12 63 STORE 0x63
  0x04FD: 11 01 LOAD 0x01
  0x04FF: 50 63 SWAP 0x63
  0x0501: 51    XCHG
  0x0502: 11 63 LOAD 0x63
  0x0504: 51    XCHG
  0x0505: 12 63 STORE 0x63
  0x0507: 20 00 MOVI 0x00
  0x0509: 51    XCHG
  0x050A: 11 01 LOAD 0x01
  0x050C: 51    XCHG
  0x050D: 11 63 LOAD 0x63
  0x050F: 51    XCHG
  0x0510: 20 00 MOVI 0x00
  0x0512: 51    XCHG
  0x0513: 12 63 STORE 0x63
  0x0515: 11 02 LOAD 0x02
  0x0517: 50 63 SWAP 0x63
  0x0519: 51    XCHG
  0x051A: 11 63 LOAD 0x63
  0x051C: 51    XCHG
  0x051D: 11 11 LOAD 0x11
  0x051F: 35 12 XOR_MEM 0x12
  0x0521: 11 13 LOAD 0x13
  0x0523: 40 00 CMPEQ 0x00
  0x0525: 43 3F JNZ 0x3F ; -> 0x0566
  0x0527: 11 13 LOAD 0x13
  0x0529: 30 0A ADDI 0x0A
  0x052B: 12 13 STORE 0x13
  0x052D: 11 12 LOAD 0x12
  0x052F: 20 00 MOVI 0x00
  0x0531: 51    XCHG
  0x0532: 51    XCHG
  0x0533: 12 63 STORE 0x63
  0x0535: 20 00 MOVI 0x00
  0x0537: 51    XCHG
  0x0538: 12 02 STORE 0x02
  0x053A: 11 63 LOAD 0x63
  0x053C: 51    XCHG
  0x053D: 20 00 MOVI 0x00
  0x053F: 51    XCHG
  0x0540: 12 63 STORE 0x63
  0x0542: 11 01 LOAD 0x01
  0x0544: 50 63 SWAP 0x63
  0x0546: 51    XCHG
  0x0547: 11 63 LOAD 0x63
  0x0549: 51    XCHG
  0x054A: 12 63 STORE 0x63
  0x054C: 20 00 MOVI 0x00
  0x054E: 51    XCHG
  0x054F: 11 01 LOAD 0x01
  0x0551: 51    XCHG
  0x0552: 11 63 LOAD 0x63
  0x0554: 51    XCHG
  0x0555: 20 00 MOVI 0x00
  0x0557: 51    XCHG
  0x0558: 12 63 STORE 0x63
  0x055A: 11 02 LOAD 0x02
  0x055C: 50 63 SWAP 0x63
  0x055E: 51    XCHG
  0x055F: 11 63 LOAD 0x63
  0x0561: 51    XCHG
  0x0562: 11 12 LOAD 0x12
  0x0564: 35 13 XOR_MEM 0x13
  0x0566: 11 14 LOAD 0x14
  0x0568: 40 00 CMPEQ 0x00
  0x056A: 43 3F JNZ 0x3F ; -> 0x05AB
  0x056C: 11 14 LOAD 0x14
  0x056E: 30 0A ADDI 0x0A
  0x0570: 12 14 STORE 0x14
  0x0572: 11 13 LOAD 0x13
  0x0574: 20 00 MOVI 0x00
  0x0576: 51    XCHG
  0x0577: 51    XCHG
  0x0578: 12 63 STORE 0x63
  0x057A: 20 00 MOVI 0x00
  0x057C: 51    XCHG
  0x057D: 12 02 STORE 0x02
  0x057F: 11 63 LOAD 0x63
  0x0581: 51    XCHG
  0x0582: 20 00 MOVI 0x00
  0x0584: 51    XCHG
  0x0585: 12 63 STORE 0x63
  0x0587: 11 01 LOAD 0x01
  0x0589: 50 63 SWAP 0x63
  0x058B: 51    XCHG
  0x058C: 11 63 LOAD 0x63
  0x058E: 51    XCHG
  0x058F: 12 63 STORE 0x63
  0x0591: 20 00 MOVI 0x00
  0x0593: 51    XCHG
  0x0594: 11 01 LOAD 0x01
  0x0596: 51    XCHG
  0x0597: 11 63 LOAD 0x63
  0x0599: 51    XCHG
  0x059A: 20 00 MOVI 0x00
  0x059C: 51    XCHG
  0x059D: 12 63 STORE 0x63
  0x059F: 11 02 LOAD 0x02
  0x05A1: 50 63 SWAP 0x63
  0x05A3: 51    XCHG
  0x05A4: 11 63 LOAD 0x63
  0x05A6: 51    XCHG
  0x05A7: 11 13 LOAD 0x13
  0x05A9: 35 14 XOR_MEM 0x14
  0x05AB: 11 15 LOAD 0x15
  0x05AD: 40 00 CMPEQ 0x00
  0x05AF: 43 3F JNZ 0x3F ; -> 0x05F0
  0x05B1: 11 15 LOAD 0x15
  0x05B3: 30 0A ADDI 0x0A
  0x05B5: 12 15 STORE 0x15
  0x05B7: 11 14 LOAD 0x14
  0x05B9: 20 00 MOVI 0x00
  0x05BB: 51    XCHG
  0x05BC: 51    XCHG
  0x05BD: 12 63 STORE 0x63
  0x05BF: 20 00 MOVI 0x00
  0x05C1: 51    XCHG
  0x05C2: 12 02 STORE 0x02
  0x05C4: 11 63 LOAD 0x63
  0x05C6: 51    XCHG
  0x05C7: 20 00 MOVI 0x00
  0x05C9: 51    XCHG
  0x05CA: 12 63 STORE 0x63
  0x05CC: 11 01 LOAD 0x01
  0x05CE: 50 63 SWAP 0x63
  0x05D0: 51    XCHG
  0x05D1: 11 63 LOAD 0x63
  0x05D3: 51    XCHG
  0x05D4: 12 63 STORE 0x63
  0x05D6: 20 00 MOVI 0x00
  0x05D8: 51    XCHG
  0x05D9: 11 01 LOAD 0x01
  0x05DB: 51    XCHG
  0x05DC: 11 63 LOAD 0x63
  0x05DE: 51    XCHG
  0x05DF: 20 00 MOVI 0x00
  0x05E1: 51    XCHG
  0x05E2: 12 63 STORE 0x63
  0x05E4: 11 02 LOAD 0x02
  0x05E6: 50 63 SWAP 0x63
  0x05E8: 51    XCHG
  0x05E9: 11 63 LOAD 0x63
  0x05EB: 51    XCHG
  0x05EC: 11 14 LOAD 0x14
  0x05EE: 35 15 XOR_MEM 0x15
  0x05F0: 11 16 LOAD 0x16
  0x05F2: 40 00 CMPEQ 0x00
  0x05F4: 43 3F JNZ 0x3F ; -> 0x0635
  0x05F6: 11 16 LOAD 0x16
  0x05F8: 30 0A ADDI 0x0A
  0x05FA: 12 16 STORE 0x16
  0x05FC: 11 15 LOAD 0x15
  0x05FE: 20 00 MOVI 0x00
  0x0600: 51    XCHG
  0x0601: 51    XCHG
  0x0602: 12 63 STORE 0x63
  0x0604: 20 00 MOVI 0x00
  0x0606: 51    XCHG
  0x0607: 12 02 STORE 0x02
  0x0609: 11 63 LOAD 0x63
  0x060B: 51    XCHG
  0x060C: 20 00 MOVI 0x00
  0x060E: 51    XCHG
  0x060F: 12 63 STORE 0x63
  0x0611: 11 01 LOAD 0x01
  0x0613: 50 63 SWAP 0x63
  0x0615: 51    XCHG
  0x0616: 11 63 LOAD 0x63
  0x0618: 51    XCHG
  0x0619: 12 63 STORE 0x63
  0x061B: 20 00 MOVI 0x00
  0x061D: 51    XCHG
  0x061E: 11 01 LOAD 0x01
  0x0620: 51    XCHG
  0x0621: 11 63 LOAD 0x63
  0x0623: 51    XCHG
  0x0624: 20 00 MOVI 0x00
  0x0626: 51    XCHG
  0x0627: 12 63 STORE 0x63
  0x0629: 11 02 LOAD 0x02
  0x062B: 50 63 SWAP 0x63
  0x062D: 51    XCHG
  0x062E: 11 63 LOAD 0x63
  0x0630: 51    XCHG
  0x0631: 11 15 LOAD 0x15
  0x0633: 35 16 XOR_MEM 0x16
  0x0635: 11 17 LOAD 0x17
  0x0637: 40 00 CMPEQ 0x00
  0x0639: 43 3F JNZ 0x3F ; -> 0x067A
  0x063B: 11 17 LOAD 0x17
  0x063D: 30 0A ADDI 0x0A
  0x063F: 12 17 STORE 0x17
  0x0641: 11 16 LOAD 0x16
  0x0643: 20 00 MOVI 0x00
  0x0645: 51    XCHG
  0x0646: 51    XCHG
  0x0647: 12 63 STORE 0x63
  0x0649: 20 00 MOVI 0x00
  0x064B: 51    XCHG
  0x064C: 12 02 STORE 0x02
  0x064E: 11 63 LOAD 0x63
  0x0650: 51    XCHG
  0x0651: 20 00 MOVI 0x00
  0x0653: 51    XCHG
  0x0654: 12 63 STORE 0x63
  0x0656: 11 01 LOAD 0x01
  0x0658: 50 63 SWAP 0x63
  0x065A: 51    XCHG
  0x065B: 11 63 LOAD 0x63
  0x065D: 51    XCHG
  0x065E: 12 63 STORE 0x63
  0x0660: 20 00 MOVI 0x00
  0x0662: 51    XCHG
  0x0663: 11 01 LOAD 0x01
  0x0665: 51    XCHG
  0x0666: 11 63 LOAD 0x63
  0x0668: 51    XCHG
  0x0669: 20 00 MOVI 0x00
  0x066B: 51    XCHG
  0x066C: 12 63 STORE 0x63
  0x066E: 11 02 LOAD 0x02
  0x0670: 50 63 SWAP 0x63
  0x0672: 51    XCHG
  0x0673: 11 63 LOAD 0x63
  0x0675: 51    XCHG
  0x0676: 11 16 LOAD 0x16
  0x0678: 35 17 XOR_MEM 0x17
  0x067A: 11 18 LOAD 0x18
  0x067C: 40 00 CMPEQ 0x00
  0x067E: 43 3F JNZ 0x3F ; -> 0x06BF
  0x0680: 11 18 LOAD 0x18
  0x0682: 30 0A ADDI 0x0A
  0x0684: 12 18 STORE 0x18
  0x0686: 11 17 LOAD 0x17
  0x0688: 20 00 MOVI 0x00
  0x068A: 51    XCHG
  0x068B: 51    XCHG
  0x068C: 12 63 STORE 0x63
  0x068E: 20 00 MOVI 0x00
  0x0690: 51    XCHG
  0x0691: 12 02 STORE 0x02
  0x0693: 11 63 LOAD 0x63
  0x0695: 51    XCHG
  0x0696: 20 00 MOVI 0x00
  0x0698: 51    XCHG
  0x0699: 12 63 STORE 0x63
  0x069B: 11 01 LOAD 0x01
  0x069D: 50 63 SWAP 0x63
  0x069F: 51    XCHG
  0x06A0: 11 63 LOAD 0x63
  0x06A2: 51    XCHG
  0x06A3: 12 63 STORE 0x63
  0x06A5: 20 00 MOVI 0x00
  0x06A7: 51    XCHG
  0x06A8: 11 01 LOAD 0x01
  0x06AA: 51    XCHG
  0x06AB: 11 63 LOAD 0x63
  0x06AD: 51    XCHG
  0x06AE: 20 00 MOVI 0x00
  0x06B0: 51    XCHG
  0x06B1: 12 63 STORE 0x63
  0x06B3: 11 02 LOAD 0x02
  0x06B5: 50 63 SWAP 0x63
  0x06B7: 51    XCHG
  0x06B8: 11 63 LOAD 0x63
  0x06BA: 51    XCHG
  0x06BB: 11 17 LOAD 0x17
  0x06BD: 35 18 XOR_MEM 0x18
  0x06BF: 11 19 LOAD 0x19
  0x06C1: 40 00 CMPEQ 0x00
  0x06C3: 43 3F JNZ 0x3F ; -> 0x0704
  0x06C5: 11 19 LOAD 0x19
  0x06C7: 30 0A ADDI 0x0A
  0x06C9: 12 19 STORE 0x19
  0x06CB: 11 18 LOAD 0x18
  0x06CD: 20 00 MOVI 0x00
  0x06CF: 51    XCHG
  0x06D0: 51    XCHG
  0x06D1: 12 63 STORE 0x63
  0x06D3: 20 00 MOVI 0x00
  0x06D5: 51    XCHG
  0x06D6: 12 02 STORE 0x02
  0x06D8: 11 63 LOAD 0x63
  0x06DA: 51    XCHG
  0x06DB: 20 00 MOVI 0x00
  0x06DD: 51    XCHG
  0x06DE: 12 63 STORE 0x63
  0x06E0: 11 01 LOAD 0x01
  0x06E2: 50 63 SWAP 0x63
  0x06E4: 51    XCHG
  0x06E5: 11 63 LOAD 0x63
  0x06E7: 51    XCHG
  0x06E8: 12 63 STORE 0x63
  0x06EA: 20 00 MOVI 0x00
  0x06EC: 51    XCHG
  0x06ED: 11 01 LOAD 0x01
  0x06EF: 51    XCHG
  0x06F0: 11 63 LOAD 0x63
  0x06F2: 51    XCHG
  0x06F3: 20 00 MOVI 0x00
  0x06F5: 51    XCHG
  0x06F6: 12 63 STORE 0x63
  0x06F8: 11 02 LOAD 0x02
  0x06FA: 50 63 SWAP 0x63
  0x06FC: 51    XCHG
  0x06FD: 11 63 LOAD 0x63
  0x06FF: 51    XCHG
  0x0700: 11 18 LOAD 0x18
  0x0702: 35 19 XOR_MEM 0x19
  0x0704: 11 1A LOAD 0x1A
  0x0706: 40 00 CMPEQ 0x00
  0x0708: 43 3F JNZ 0x3F ; -> 0x0749
  0x070A: 11 1A LOAD 0x1A
  0x070C: 30 0A ADDI 0x0A
  0x070E: 12 1A STORE 0x1A
  0x0710: 11 19 LOAD 0x19
  0x0712: 20 00 MOVI 0x00
  0x0714: 51    XCHG
  0x0715: 51    XCHG
  0x0716: 12 63 STORE 0x63
  0x0718: 20 00 MOVI 0x00
  0x071A: 51    XCHG
  0x071B: 12 02 STORE 0x02
  0x071D: 11 63 LOAD 0x63
  0x071F: 51    XCHG
  0x0720: 20 00 MOVI 0x00
  0x0722: 51    XCHG
  0x0723: 12 63 STORE 0x63
  0x0725: 11 01 LOAD 0x01
  0x0727: 50 63 SWAP 0x63
  0x0729: 51    XCHG
  0x072A: 11 63 LOAD 0x63
  0x072C: 51    XCHG
  0x072D: 12 63 STORE 0x63
  0x072F: 20 00 MOVI 0x00
  0x0731: 51    XCHG
  0x0732: 11 01 LOAD 0x01
  0x0734: 51    XCHG
  0x0735: 11 63 LOAD 0x63
  0x0737: 51    XCHG
  0x0738: 20 00 MOVI 0x00
  0x073A: 51    XCHG
  0x073B: 12 63 STORE 0x63
  0x073D: 11 02 LOAD 0x02
  0x073F: 50 63 SWAP 0x63
  0x0741: 51    XCHG
  0x0742: 11 63 LOAD 0x63
  0x0744: 51    XCHG
  0x0745: 11 19 LOAD 0x19
  0x0747: 35 1A XOR_MEM 0x1A
  0x0749: 11 00 LOAD 0x00
  0x074B: 40 00 CMPEQ 0x00
  0x074D: 43 06 JNZ 0x06 ; -> 0x0755
  0x074F: 11 00 LOAD 0x00
  0x0751: 30 10 ADDI 0x10
  0x0753: 12 00 STORE 0x00
  0x0755: 11 02 LOAD 0x02
  0x0757: 40 00 CMPEQ 0x00
  0x0759: 43 06 JNZ 0x06 ; -> 0x0761
  0x075B: 11 02 LOAD 0x02
  0x075D: 30 10 ADDI 0x10
  0x075F: 12 02 STORE 0x02
  0x0761: 11 04 LOAD 0x04
  0x0763: 40 00 CMPEQ 0x00
  0x0765: 43 06 JNZ 0x06 ; -> 0x076D
  0x0767: 11 04 LOAD 0x04
  0x0769: 30 10 ADDI 0x10
  0x076B: 12 04 STORE 0x04
  0x076D: 11 06 LOAD 0x06
  0x076F: 40 00 CMPEQ 0x00
  0x0771: 43 06 JNZ 0x06 ; -> 0x0779
  0x0773: 11 06 LOAD 0x06
  0x0775: 30 10 ADDI 0x10
  0x0777: 12 06 STORE 0x06
  0x0779: 11 08 LOAD 0x08
  0x077B: 40 00 CMPEQ 0x00
  0x077D: 43 06 JNZ 0x06 ; -> 0x0785
  0x077F: 11 08 LOAD 0x08
  0x0781: 30 10 ADDI 0x10
  0x0783: 12 08 STORE 0x08
  0x0785: 11 0A LOAD 0x0A
  0x0787: 40 00 CMPEQ 0x00
  0x0789: 43 06 JNZ 0x06 ; -> 0x0791
  0x078B: 11 0A LOAD 0x0A
  0x078D: 30 10 ADDI 0x10
  0x078F: 12 0A STORE 0x0A
  0x0791: 11 0C LOAD 0x0C
  0x0793: 40 00 CMPEQ 0x00
  0x0795: 43 06 JNZ 0x06 ; -> 0x079D
  0x0797: 11 0C LOAD 0x0C
  0x0799: 30 10 ADDI 0x10
  0x079B: 12 0C STORE 0x0C
  0x079D: 11 0E LOAD 0x0E
  0x079F: 40 00 CMPEQ 0x00
  0x07A1: 43 06 JNZ 0x06 ; -> 0x07A9
  0x07A3: 11 0E LOAD 0x0E
  0x07A5: 30 10 ADDI 0x10
  0x07A7: 12 0E STORE 0x0E
  0x07A9: 11 10 LOAD 0x10
  0x07AB: 40 00 CMPEQ 0x00
  0x07AD: 43 06 JNZ 0x06 ; -> 0x07B5
  0x07AF: 11 10 LOAD 0x10
  0x07B1: 30 10 ADDI 0x10
  0x07B3: 12 10 STORE 0x10
  0x07B5: 11 12 LOAD 0x12
  0x07B7: 40 00 CMPEQ 0x00
  0x07B9: 43 06 JNZ 0x06 ; -> 0x07C1
  0x07BB: 11 12 LOAD 0x12
  0x07BD: 30 10 ADDI 0x10
  0x07BF: 12 12 STORE 0x12
  0x07C1: 11 14 LOAD 0x14
  0x07C3: 40 00 CMPEQ 0x00
  0x07C5: 43 06 JNZ 0x06 ; -> 0x07CD
  0x07C7: 11 14 LOAD 0x14
  0x07C9: 30 10 ADDI 0x10
  0x07CB: 12 14 STORE 0x14
  0x07CD: 11 16 LOAD 0x16
  0x07CF: 40 00 CMPEQ 0x00
  0x07D1: 43 06 JNZ 0x06 ; -> 0x07D9
  0x07D3: 11 16 LOAD 0x16
  0x07D5: 30 10 ADDI 0x10
  0x07D7: 12 16 STORE 0x16
  0x07D9: 11 18 LOAD 0x18
  0x07DB: 40 00 CMPEQ 0x00
  0x07DD: 43 06 JNZ 0x06 ; -> 0x07E5
  0x07DF: 11 18 LOAD 0x18
  0x07E1: 30 10 ADDI 0x10
  0x07E3: 12 18 STORE 0x18
  0x07E5: 11 1A LOAD 0x1A
  0x07E7: 40 00 CMPEQ 0x00
  0x07E9: 43 06 JNZ 0x06 ; -> 0x07F1
  0x07EB: 11 1A LOAD 0x1A
  0x07ED: 30 10 ADDI 0x10
  0x07EF: 12 1A STORE 0x1A
  0x07F1: 11 01 LOAD 0x01
  0x07F3: 40 00 CMPEQ 0x00
  0x07F5: 43 06 JNZ 0x06 ; -> 0x07FD
  0x07F7: 11 01 LOAD 0x01
  0x07F9: 30 31 ADDI 0x31
  0x07FB: 12 01 STORE 0x01
  0x07FD: 11 03 LOAD 0x03
  0x07FF: 40 00 CMPEQ 0x00
  0x0801: 43 06 JNZ 0x06 ; -> 0x0809
  0x0803: 11 03 LOAD 0x03
  0x0805: 30 31 ADDI 0x31
  0x0807: 12 03 STORE 0x03
  0x0809: 11 05 LOAD 0x05
  0x080B: 40 00 CMPEQ 0x00
  0x080D: 43 06 JNZ 0x06 ; -> 0x0815
  0x080F: 11 05 LOAD 0x05
  0x0811: 30 31 ADDI 0x31
  0x0813: 12 05 STORE 0x05
  0x0815: 11 07 LOAD 0x07
  0x0817: 40 00 CMPEQ 0x00
  0x0819: 43 06 JNZ 0x06 ; -> 0x0821
  0x081B: 11 07 LOAD 0x07
  0x081D: 30 31 ADDI 0x31
  0x081F: 12 07 STORE 0x07
  0x0821: 11 09 LOAD 0x09
  0x0823: 40 00 CMPEQ 0x00
  0x0825: 43 06 JNZ 0x06 ; -> 0x082D
  0x0827: 11 09 LOAD 0x09
  0x0829: 30 31 ADDI 0x31
  0x082B: 12 09 STORE 0x09
  0x082D: 11 0B LOAD 0x0B
  0x082F: 40 00 CMPEQ 0x00
  0x0831: 43 06 JNZ 0x06 ; -> 0x0839
  0x0833: 11 0B LOAD 0x0B
  0x0835: 30 31 ADDI 0x31
  0x0837: 12 0B STORE 0x0B
  0x0839: 11 0D LOAD 0x0D
  0x083B: 40 00 CMPEQ 0x00
  0x083D: 43 06 JNZ 0x06 ; -> 0x0845
  0x083F: 11 0D LOAD 0x0D
  0x0841: 30 31 ADDI 0x31
  0x0843: 12 0D STORE 0x0D
  0x0845: 11 0F LOAD 0x0F
  0x0847: 40 00 CMPEQ 0x00
  0x0849: 43 06 JNZ 0x06 ; -> 0x0851
  0x084B: 11 0F LOAD 0x0F
  0x084D: 30 31 ADDI 0x31
  0x084F: 12 0F STORE 0x0F
  0x0851: 11 11 LOAD 0x11
  0x0853: 40 00 CMPEQ 0x00
  0x0855: 43 06 JNZ 0x06 ; -> 0x085D
  0x0857: 11 11 LOAD 0x11
  0x0859: 30 31 ADDI 0x31
  0x085B: 12 11 STORE 0x11
  0x085D: 11 13 LOAD 0x13
  0x085F: 40 00 CMPEQ 0x00
  0x0861: 43 06 JNZ 0x06 ; -> 0x0869
  0x0863: 11 13 LOAD 0x13
  0x0865: 30 31 ADDI 0x31
  0x0867: 12 13 STORE 0x13
  0x0869: 11 15 LOAD 0x15
  0x086B: 40 00 CMPEQ 0x00
  0x086D: 43 06 JNZ 0x06 ; -> 0x0875
  0x086F: 11 15 LOAD 0x15
  0x0871: 30 31 ADDI 0x31
  0x0873: 12 15 STORE 0x15
  0x0875: 11 17 LOAD 0x17
  0x0877: 40 00 CMPEQ 0x00
  0x0879: 43 06 JNZ 0x06 ; -> 0x0881
  0x087B: 11 17 LOAD 0x17
  0x087D: 30 31 ADDI 0x31
  0x087F: 12 17 STORE 0x17
  0x0881: 11 19 LOAD 0x19
  0x0883: 40 00 CMPEQ 0x00
  0x0885: 43 06 JNZ 0x06 ; -> 0x088D
  0x0887: 11 19 LOAD 0x19
  0x0889: 30 31 ADDI 0x31
  0x088B: 12 19 STORE 0x19
  0x088D: 11 00 LOAD 0x00
  0x088F: 40 60 CMPEQ 0x60
  0x0891: 42 B2 JZ 0xB2 ; -> 0x0945
  0x0893: 11 01 LOAD 0x01
  0x0895: 40 9B CMPEQ 0x9B
  0x0897: 42 AC JZ 0xAC ; -> 0x0945
  0x0899: 11 02 LOAD 0x02
  0x089B: 40 22 CMPEQ 0x22
  0x089D: 42 A6 JZ 0xA6 ; -> 0x0945
  0x089F: 11 03 LOAD 0x03
  0x08A1: 40 AC CMPEQ 0xAC
  0x08A3: 42 A0 JZ 0xA0 ; -> 0x0945
  0x08A5: 11 04 LOAD 0x04
  0x08A7: 40 40 CMPEQ 0x40
  0x08A9: 42 9A JZ 0x9A ; -> 0x0945
  0x08AB: 11 05 LOAD 0x05
  0x08AD: 40 79 CMPEQ 0x79
  0x08AF: 42 94 JZ 0x94 ; -> 0x0945
  0x08B1: 11 06 LOAD 0x06
  0x08B3: 40 36 CMPEQ 0x36
  0x08B5: 42 8E JZ 0x8E ; -> 0x0945
  0x08B7: 11 07 LOAD 0x07
  0x08B9: 40 80 CMPEQ 0x80
  0x08BB: 42 88 JZ 0x88 ; -> 0x0945
  0x08BD: 11 08 LOAD 0x08
  0x08BF: 40 82 CMPEQ 0x82
  0x08C1: 42 82 JZ 0x82 ; -> 0x0945
  0x08C3: 11 09 LOAD 0x09
  0x08C5: 40 4A CMPEQ 0x4A
  0x08C7: 42 7C JZ 0x7C ; -> 0x0945
  0x08C9: 11 0A LOAD 0x0A
  0x08CB: 40 74 CMPEQ 0x74
  0x08CD: 42 76 JZ 0x76 ; -> 0x0945
  0x08CF: 11 0B LOAD 0x0B
  0x08D1: 40 18 CMPEQ 0x18
  0x08D3: 42 70 JZ 0x70 ; -> 0x0945
  0x08D5: 11 0C LOAD 0x0C
  0x08D7: 40 97 CMPEQ 0x97
  0x08D9: 42 6A JZ 0x6A ; -> 0x0945
  0x08DB: 11 0D LOAD 0x0D
  0x08DD: 40 25 CMPEQ 0x25
  0x08DF: 42 64 JZ 0x64 ; -> 0x0945
  0x08E1: 11 0E LOAD 0x0E
  0x08E3: 40 B3 CMPEQ 0xB3
  0x08E5: 42 5E JZ 0x5E ; -> 0x0945
  0x08E7: 11 0F LOAD 0x0F
  0x08E9: 40 23 CMPEQ 0x23
  0x08EB: 42 58 JZ 0x58 ; -> 0x0945
  0x08ED: 11 10 LOAD 0x10
  0x08EF: 40 A9 CMPEQ 0xA9
  0x08F1: 42 52 JZ 0x52 ; -> 0x0945
  0x08F3: 11 11 LOAD 0x11
  0x08F5: 40 D3 CMPEQ 0xD3
  0x08F7: 42 4C JZ 0x4C ; -> 0x0945
  0x08F9: 11 12 LOAD 0x12
  0x08FB: 40 32 CMPEQ 0x32
  0x08FD: 42 46 JZ 0x46 ; -> 0x0945
  0x08FF: 11 13 LOAD 0x13
  0x0901: 40 4A CMPEQ 0x4A
  0x0903: 42 40 JZ 0x40 ; -> 0x0945
  0x0905: 11 14 LOAD 0x14
  0x0907: 40 86 CMPEQ 0x86
  0x0909: 42 3A JZ 0x3A ; -> 0x0945
  0x090B: 11 15 LOAD 0x15
  0x090D: 40 57 CMPEQ 0x57
  0x090F: 42 34 JZ 0x34 ; -> 0x0945
  0x0911: 11 16 LOAD 0x16
  0x0913: 40 75 CMPEQ 0x75
  0x0915: 42 2E JZ 0x2E ; -> 0x0945
  0x0917: 11 17 LOAD 0x17
  0x0919: 40 4A CMPEQ 0x4A
  0x091B: 42 28 JZ 0x28 ; -> 0x0945
  0x091D: 11 18 LOAD 0x18
  0x091F: 40 8A CMPEQ 0x8A
  0x0921: 42 22 JZ 0x22 ; -> 0x0945
  0x0923: 11 19 LOAD 0x19
  0x0925: 40 61 CMPEQ 0x61
  0x0927: 42 1C JZ 0x1C ; -> 0x0945
  0x0929: 11 1A LOAD 0x1A
  0x092B: 40 5F CMPEQ 0x5F
  0x092D: 42 16 JZ 0x16 ; -> 0x0945
  0x092F: 10 04 ALLOC 0x04
  0x0931: 20 79 MOVI 0x79
  0x0933: 12 00 STORE 0x00
  0x0935: 20 65 MOVI 0x65
  0x0937: 12 01 STORE 0x01
  0x0939: 20 73 MOVI 0x73
  0x093B: 12 02 STORE 0x02
  0x093D: 20 00 MOVI 0x00
  0x093F: 12 03 STORE 0x03
  0x0941: 52 02 SYSCALL 0x02 ; PRINT_OUTPUT
  0x0943: 41 10 JMP 0x10 ; -> 0x0955
  0x0945: 10 03 ALLOC 0x03
  0x0947: 20 4E MOVI 0x4E
  0x0949: 12 00 STORE 0x00
  0x094B: 20 6F MOVI 0x6F
  0x094D: 12 01 STORE 0x01
  0x094F: 20 00 MOVI 0x00
  0x0951: 12 02 STORE 0x02
  0x0953: 52 02 SYSCALL 0x02 ; PRINT_OUTPUT
  0x0955: 13    FREE
  ```

- 第二版

  ```plaintext
  E:\_CTF\模板\.venv\Scripts\python.exe "E:\_CTF\题目\POLARIS CTF 2026\FunPyVM\decompiled_main.py" 
  --- VM Start ---
  ; size = 2390 bytes
  ; instructions = 1370

    0x0000: 10 64  ALLOC 0x64
    0x0002: 10 32  ALLOC 0x32
    0x0004: 20 00  MOVI 0x00
    0x0006: 51     XCHG
    0x0007: 12 63  STORE 0x63
    0x0009: 11 01  LOAD 0x01
    0x000B: 50 63  SWAP 0x63
    0x000D: 51     XCHG
    0x000E: 11 63  LOAD 0x63
    0x0010: 51     XCHG
    0x0011: 12 63  STORE 0x63
    0x0013: 20 00  MOVI 0x00
    0x0015: 51     XCHG
    0x0016: 12 02  STORE 0x02
    0x0018: 11 63  LOAD 0x63
    0x001A: 51     XCHG
    0x001B: 20 42  MOVI 0x42
    0x001D: 30 13  ADDI 0x13
    0x001F: 12 63  STORE 0x63
    0x0021: 20 00  MOVI 0x00
    0x0023: 51     XCHG
    0x0024: 12 63  STORE 0x63
    0x0026: 11 02  LOAD 0x02
    0x0028: 50 63  SWAP 0x63
    0x002A: 51     XCHG
    0x002B: 11 63  LOAD 0x63
    0x002D: 51     XCHG
    0x002E: 12 63  STORE 0x63
    0x0030: 20 00  MOVI 0x00
    0x0032: 51     XCHG
    0x0033: 11 01  LOAD 0x01
    0x0035: 51     XCHG
    0x0036: 11 63  LOAD 0x63
    0x0038: 51     XCHG
    0x0039: 52 01  SYSCALL 0x01 ; READ_INPUT
    0x003B: 11 00  LOAD 0x00
    0x003D: 40 00  CMPEQ 0x00 //检测输入是否为0
    0x003F: 43 06  JNZ 0x06 ; -> loc_0047 (0x0047)
    0x0041: 11 00  LOAD 0x00 
    0x0043: 30 0A  ADDI 0x0A //+= 0xA
    0x0045: 12 00  STORE 0x00 //存储到内存0x00处
  loc_0047:
    0x0047: 11 01  LOAD 0x01 //同理
    0x0049: 40 00  CMPEQ 0x00
    0x004B: 43 3F  JNZ 0x3F ; -> loc_008C (0x008C)
    0x004D: 11 01  LOAD 0x01  
    0x004F: 30 0A  ADDI 0x0A // += 0xA
    0x0051: 12 01  STORE 0x01 // R1（这里是第二位）被存到内存0x01处
    0x0053: 11 00  LOAD 0x00  //下面这一坨没啥用，混淆视线来的
    0x0055: 20 00  MOVI 0x00
    0x0057: 51     XCHG
    0x0058: 51     XCHG
    0x0059: 12 63  STORE 0x63
    0x005B: 20 00  MOVI 0x00
    0x005D: 51     XCHG
    0x005E: 12 02  STORE 0x02
    0x0060: 11 63  LOAD 0x63
    0x0062: 51     XCHG
    0x0063: 20 00  MOVI 0x00
    0x0065: 51     XCHG
    0x0066: 12 63  STORE 0x63
    0x0068: 11 01  LOAD 0x01
    0x006A: 50 63  SWAP 0x63
    0x006C: 51     XCHG
    0x006D: 11 63  LOAD 0x63
    0x006F: 51     XCHG
    0x0070: 12 63  STORE 0x63
    0x0072: 20 00  MOVI 0x00
    0x0074: 51     XCHG
    0x0075: 11 01  LOAD 0x01
    0x0077: 51     XCHG
    0x0078: 11 63  LOAD 0x63
    0x007A: 51     XCHG
    0x007B: 20 00  MOVI 0x00
    0x007D: 51     XCHG
    0x007E: 12 63  STORE 0x63
    0x0080: 11 02  LOAD 0x02
    0x0082: 50 63  SWAP 0x63
    0x0084: 51     XCHG
    0x0085: 11 63  LOAD 0x63
    0x0087: 51     XCHG
    0x0088: 11 00  LOAD 0x00    // 加载第一位到R1
    0x008A: 35 01  XOR_MEM 0x01 // 0x01处的内存值 ^= R1。逻辑是后一位 ^= 前一位
  loc_008C:
    0x008C: 11 02  LOAD 0x02
    0x008E: 40 00  CMPEQ 0x00
    0x0090: 43 3F  JNZ 0x3F ; -> loc_00D1 (0x00D1)
    0x0092: 11 02  LOAD 0x02
    0x0094: 30 0A  ADDI 0x0A
    0x0096: 12 02  STORE 0x02
    0x0098: 11 01  LOAD 0x01
    0x009A: 20 00  MOVI 0x00
    0x009C: 51     XCHG
    0x009D: 51     XCHG
    0x009E: 12 63  STORE 0x63
    0x00A0: 20 00  MOVI 0x00
    0x00A2: 51     XCHG
    0x00A3: 12 02  STORE 0x02
    0x00A5: 11 63  LOAD 0x63
    0x00A7: 51     XCHG
    0x00A8: 20 00  MOVI 0x00
    0x00AA: 51     XCHG
    0x00AB: 12 63  STORE 0x63
    0x00AD: 11 01  LOAD 0x01
    0x00AF: 50 63  SWAP 0x63
    0x00B1: 51     XCHG
    0x00B2: 11 63  LOAD 0x63
    0x00B4: 51     XCHG
    0x00B5: 12 63  STORE 0x63
    0x00B7: 20 00  MOVI 0x00
    0x00B9: 51     XCHG
    0x00BA: 11 01  LOAD 0x01
    0x00BC: 51     XCHG
    0x00BD: 11 63  LOAD 0x63
    0x00BF: 51     XCHG
    0x00C0: 20 00  MOVI 0x00
    0x00C2: 51     XCHG
    0x00C3: 12 63  STORE 0x63
    0x00C5: 11 02  LOAD 0x02
    0x00C7: 50 63  SWAP 0x63
    0x00C9: 51     XCHG
    0x00CA: 11 63  LOAD 0x63
    0x00CC: 51     XCHG
    0x00CD: 11 01  LOAD 0x01
    0x00CF: 35 02  XOR_MEM 0x02
  loc_00D1:
    0x00D1: 11 03  LOAD 0x03
    0x00D3: 40 00  CMPEQ 0x00
    0x00D5: 43 3F  JNZ 0x3F ; -> loc_0116 (0x0116)
    0x00D7: 11 03  LOAD 0x03
    0x00D9: 30 0A  ADDI 0x0A
    0x00DB: 12 03  STORE 0x03
    0x00DD: 11 02  LOAD 0x02
    0x00DF: 20 00  MOVI 0x00
    0x00E1: 51     XCHG
    0x00E2: 51     XCHG
    0x00E3: 12 63  STORE 0x63
    0x00E5: 20 00  MOVI 0x00
    0x00E7: 51     XCHG
    0x00E8: 12 02  STORE 0x02
    0x00EA: 11 63  LOAD 0x63
    0x00EC: 51     XCHG
    0x00ED: 20 00  MOVI 0x00
    0x00EF: 51     XCHG
    0x00F0: 12 63  STORE 0x63
    0x00F2: 11 01  LOAD 0x01
    0x00F4: 50 63  SWAP 0x63
    0x00F6: 51     XCHG
    0x00F7: 11 63  LOAD 0x63
    0x00F9: 51     XCHG
    0x00FA: 12 63  STORE 0x63
    0x00FC: 20 00  MOVI 0x00
    0x00FE: 51     XCHG
    0x00FF: 11 01  LOAD 0x01
    0x0101: 51     XCHG
    0x0102: 11 63  LOAD 0x63
    0x0104: 51     XCHG
    0x0105: 20 00  MOVI 0x00
    0x0107: 51     XCHG
    0x0108: 12 63  STORE 0x63
    0x010A: 11 02  LOAD 0x02
    0x010C: 50 63  SWAP 0x63
    0x010E: 51     XCHG
    0x010F: 11 63  LOAD 0x63
    0x0111: 51     XCHG
    0x0112: 11 02  LOAD 0x02
    0x0114: 35 03  XOR_MEM 0x03
  loc_0116:
    0x0116: 11 04  LOAD 0x04
    0x0118: 40 00  CMPEQ 0x00
    0x011A: 43 3F  JNZ 0x3F ; -> loc_015B (0x015B)
    0x011C: 11 04  LOAD 0x04
    0x011E: 30 0A  ADDI 0x0A
    0x0120: 12 04  STORE 0x04
    0x0122: 11 03  LOAD 0x03
    0x0124: 20 00  MOVI 0x00
    0x0126: 51     XCHG
    0x0127: 51     XCHG
    0x0128: 12 63  STORE 0x63
    0x012A: 20 00  MOVI 0x00
    0x012C: 51     XCHG
    0x012D: 12 02  STORE 0x02
    0x012F: 11 63  LOAD 0x63
    0x0131: 51     XCHG
    0x0132: 20 00  MOVI 0x00
    0x0134: 51     XCHG
    0x0135: 12 63  STORE 0x63
    0x0137: 11 01  LOAD 0x01
    0x0139: 50 63  SWAP 0x63
    0x013B: 51     XCHG
    0x013C: 11 63  LOAD 0x63
    0x013E: 51     XCHG
    0x013F: 12 63  STORE 0x63
    0x0141: 20 00  MOVI 0x00
    0x0143: 51     XCHG
    0x0144: 11 01  LOAD 0x01
    0x0146: 51     XCHG
    0x0147: 11 63  LOAD 0x63
    0x0149: 51     XCHG
    0x014A: 20 00  MOVI 0x00
    0x014C: 51     XCHG
    0x014D: 12 63  STORE 0x63
    0x014F: 11 02  LOAD 0x02
    0x0151: 50 63  SWAP 0x63
    0x0153: 51     XCHG
    0x0154: 11 63  LOAD 0x63
    0x0156: 51     XCHG
    0x0157: 11 03  LOAD 0x03
    0x0159: 35 04  XOR_MEM 0x04
  loc_015B:
    0x015B: 11 05  LOAD 0x05
    0x015D: 40 00  CMPEQ 0x00
    0x015F: 43 3F  JNZ 0x3F ; -> loc_01A0 (0x01A0)
    0x0161: 11 05  LOAD 0x05
    0x0163: 30 0A  ADDI 0x0A
    0x0165: 12 05  STORE 0x05
    0x0167: 11 04  LOAD 0x04
    0x0169: 20 00  MOVI 0x00
    0x016B: 51     XCHG
    0x016C: 51     XCHG
    0x016D: 12 63  STORE 0x63
    0x016F: 20 00  MOVI 0x00
    0x0171: 51     XCHG
    0x0172: 12 02  STORE 0x02
    0x0174: 11 63  LOAD 0x63
    0x0176: 51     XCHG
    0x0177: 20 00  MOVI 0x00
    0x0179: 51     XCHG
    0x017A: 12 63  STORE 0x63
    0x017C: 11 01  LOAD 0x01
    0x017E: 50 63  SWAP 0x63
    0x0180: 51     XCHG
    0x0181: 11 63  LOAD 0x63
    0x0183: 51     XCHG
    0x0184: 12 63  STORE 0x63
    0x0186: 20 00  MOVI 0x00
    0x0188: 51     XCHG
    0x0189: 11 01  LOAD 0x01
    0x018B: 51     XCHG
    0x018C: 11 63  LOAD 0x63
    0x018E: 51     XCHG
    0x018F: 20 00  MOVI 0x00
    0x0191: 51     XCHG
    0x0192: 12 63  STORE 0x63
    0x0194: 11 02  LOAD 0x02
    0x0196: 50 63  SWAP 0x63
    0x0198: 51     XCHG
    0x0199: 11 63  LOAD 0x63
    0x019B: 51     XCHG
    0x019C: 11 04  LOAD 0x04
    0x019E: 35 05  XOR_MEM 0x05
  loc_01A0:
    0x01A0: 11 06  LOAD 0x06
    0x01A2: 40 00  CMPEQ 0x00
    0x01A4: 43 3F  JNZ 0x3F ; -> loc_01E5 (0x01E5)
    0x01A6: 11 06  LOAD 0x06
    0x01A8: 30 0A  ADDI 0x0A
    0x01AA: 12 06  STORE 0x06
    0x01AC: 11 05  LOAD 0x05
    0x01AE: 20 00  MOVI 0x00
    0x01B0: 51     XCHG
    0x01B1: 51     XCHG
    0x01B2: 12 63  STORE 0x63
    0x01B4: 20 00  MOVI 0x00
    0x01B6: 51     XCHG
    0x01B7: 12 02  STORE 0x02
    0x01B9: 11 63  LOAD 0x63
    0x01BB: 51     XCHG
    0x01BC: 20 00  MOVI 0x00
    0x01BE: 51     XCHG
    0x01BF: 12 63  STORE 0x63
    0x01C1: 11 01  LOAD 0x01
    0x01C3: 50 63  SWAP 0x63
    0x01C5: 51     XCHG
    0x01C6: 11 63  LOAD 0x63
    0x01C8: 51     XCHG
    0x01C9: 12 63  STORE 0x63
    0x01CB: 20 00  MOVI 0x00
    0x01CD: 51     XCHG
    0x01CE: 11 01  LOAD 0x01
    0x01D0: 51     XCHG
    0x01D1: 11 63  LOAD 0x63
    0x01D3: 51     XCHG
    0x01D4: 20 00  MOVI 0x00
    0x01D6: 51     XCHG
    0x01D7: 12 63  STORE 0x63
    0x01D9: 11 02  LOAD 0x02
    0x01DB: 50 63  SWAP 0x63
    0x01DD: 51     XCHG
    0x01DE: 11 63  LOAD 0x63
    0x01E0: 51     XCHG
    0x01E1: 11 05  LOAD 0x05
    0x01E3: 35 06  XOR_MEM 0x06
  loc_01E5:
    0x01E5: 11 07  LOAD 0x07
    0x01E7: 40 00  CMPEQ 0x00
    0x01E9: 43 3F  JNZ 0x3F ; -> loc_022A (0x022A)
    0x01EB: 11 07  LOAD 0x07
    0x01ED: 30 0A  ADDI 0x0A
    0x01EF: 12 07  STORE 0x07
    0x01F1: 11 06  LOAD 0x06
    0x01F3: 20 00  MOVI 0x00
    0x01F5: 51     XCHG
    0x01F6: 51     XCHG
    0x01F7: 12 63  STORE 0x63
    0x01F9: 20 00  MOVI 0x00
    0x01FB: 51     XCHG
    0x01FC: 12 02  STORE 0x02
    0x01FE: 11 63  LOAD 0x63
    0x0200: 51     XCHG
    0x0201: 20 00  MOVI 0x00
    0x0203: 51     XCHG
    0x0204: 12 63  STORE 0x63
    0x0206: 11 01  LOAD 0x01
    0x0208: 50 63  SWAP 0x63
    0x020A: 51     XCHG
    0x020B: 11 63  LOAD 0x63
    0x020D: 51     XCHG
    0x020E: 12 63  STORE 0x63
    0x0210: 20 00  MOVI 0x00
    0x0212: 51     XCHG
    0x0213: 11 01  LOAD 0x01
    0x0215: 51     XCHG
    0x0216: 11 63  LOAD 0x63
    0x0218: 51     XCHG
    0x0219: 20 00  MOVI 0x00
    0x021B: 51     XCHG
    0x021C: 12 63  STORE 0x63
    0x021E: 11 02  LOAD 0x02
    0x0220: 50 63  SWAP 0x63
    0x0222: 51     XCHG
    0x0223: 11 63  LOAD 0x63
    0x0225: 51     XCHG
    0x0226: 11 06  LOAD 0x06
    0x0228: 35 07  XOR_MEM 0x07
  loc_022A:
    0x022A: 11 08  LOAD 0x08
    0x022C: 40 00  CMPEQ 0x00
    0x022E: 43 3F  JNZ 0x3F ; -> loc_026F (0x026F)
    0x0230: 11 08  LOAD 0x08
    0x0232: 30 0A  ADDI 0x0A
    0x0234: 12 08  STORE 0x08
    0x0236: 11 07  LOAD 0x07
    0x0238: 20 00  MOVI 0x00
    0x023A: 51     XCHG
    0x023B: 51     XCHG
    0x023C: 12 63  STORE 0x63
    0x023E: 20 00  MOVI 0x00
    0x0240: 51     XCHG
    0x0241: 12 02  STORE 0x02
    0x0243: 11 63  LOAD 0x63
    0x0245: 51     XCHG
    0x0246: 20 00  MOVI 0x00
    0x0248: 51     XCHG
    0x0249: 12 63  STORE 0x63
    0x024B: 11 01  LOAD 0x01
    0x024D: 50 63  SWAP 0x63
    0x024F: 51     XCHG
    0x0250: 11 63  LOAD 0x63
    0x0252: 51     XCHG
    0x0253: 12 63  STORE 0x63
    0x0255: 20 00  MOVI 0x00
    0x0257: 51     XCHG
    0x0258: 11 01  LOAD 0x01
    0x025A: 51     XCHG
    0x025B: 11 63  LOAD 0x63
    0x025D: 51     XCHG
    0x025E: 20 00  MOVI 0x00
    0x0260: 51     XCHG
    0x0261: 12 63  STORE 0x63
    0x0263: 11 02  LOAD 0x02
    0x0265: 50 63  SWAP 0x63
    0x0267: 51     XCHG
    0x0268: 11 63  LOAD 0x63
    0x026A: 51     XCHG
    0x026B: 11 07  LOAD 0x07
    0x026D: 35 08  XOR_MEM 0x08
  loc_026F:
    0x026F: 11 09  LOAD 0x09
    0x0271: 40 00  CMPEQ 0x00
    0x0273: 43 3F  JNZ 0x3F ; -> loc_02B4 (0x02B4)
    0x0275: 11 09  LOAD 0x09
    0x0277: 30 0A  ADDI 0x0A
    0x0279: 12 09  STORE 0x09
    0x027B: 11 08  LOAD 0x08
    0x027D: 20 00  MOVI 0x00
    0x027F: 51     XCHG
    0x0280: 51     XCHG
    0x0281: 12 63  STORE 0x63
    0x0283: 20 00  MOVI 0x00
    0x0285: 51     XCHG
    0x0286: 12 02  STORE 0x02
    0x0288: 11 63  LOAD 0x63
    0x028A: 51     XCHG
    0x028B: 20 00  MOVI 0x00
    0x028D: 51     XCHG
    0x028E: 12 63  STORE 0x63
    0x0290: 11 01  LOAD 0x01
    0x0292: 50 63  SWAP 0x63
    0x0294: 51     XCHG
    0x0295: 11 63  LOAD 0x63
    0x0297: 51     XCHG
    0x0298: 12 63  STORE 0x63
    0x029A: 20 00  MOVI 0x00
    0x029C: 51     XCHG
    0x029D: 11 01  LOAD 0x01
    0x029F: 51     XCHG
    0x02A0: 11 63  LOAD 0x63
    0x02A2: 51     XCHG
    0x02A3: 20 00  MOVI 0x00
    0x02A5: 51     XCHG
    0x02A6: 12 63  STORE 0x63
    0x02A8: 11 02  LOAD 0x02
    0x02AA: 50 63  SWAP 0x63
    0x02AC: 51     XCHG
    0x02AD: 11 63  LOAD 0x63
    0x02AF: 51     XCHG
    0x02B0: 11 08  LOAD 0x08
    0x02B2: 35 09  XOR_MEM 0x09
  loc_02B4:
    0x02B4: 11 0A  LOAD 0x0A
    0x02B6: 40 00  CMPEQ 0x00
    0x02B8: 43 3F  JNZ 0x3F ; -> loc_02F9 (0x02F9)
    0x02BA: 11 0A  LOAD 0x0A
    0x02BC: 30 0A  ADDI 0x0A
    0x02BE: 12 0A  STORE 0x0A
    0x02C0: 11 09  LOAD 0x09
    0x02C2: 20 00  MOVI 0x00
    0x02C4: 51     XCHG
    0x02C5: 51     XCHG
    0x02C6: 12 63  STORE 0x63
    0x02C8: 20 00  MOVI 0x00
    0x02CA: 51     XCHG
    0x02CB: 12 02  STORE 0x02
    0x02CD: 11 63  LOAD 0x63
    0x02CF: 51     XCHG
    0x02D0: 20 00  MOVI 0x00
    0x02D2: 51     XCHG
    0x02D3: 12 63  STORE 0x63
    0x02D5: 11 01  LOAD 0x01
    0x02D7: 50 63  SWAP 0x63
    0x02D9: 51     XCHG
    0x02DA: 11 63  LOAD 0x63
    0x02DC: 51     XCHG
    0x02DD: 12 63  STORE 0x63
    0x02DF: 20 00  MOVI 0x00
    0x02E1: 51     XCHG
    0x02E2: 11 01  LOAD 0x01
    0x02E4: 51     XCHG
    0x02E5: 11 63  LOAD 0x63
    0x02E7: 51     XCHG
    0x02E8: 20 00  MOVI 0x00
    0x02EA: 51     XCHG
    0x02EB: 12 63  STORE 0x63
    0x02ED: 11 02  LOAD 0x02
    0x02EF: 50 63  SWAP 0x63
    0x02F1: 51     XCHG
    0x02F2: 11 63  LOAD 0x63
    0x02F4: 51     XCHG
    0x02F5: 11 09  LOAD 0x09
    0x02F7: 35 0A  XOR_MEM 0x0A
  loc_02F9:
    0x02F9: 11 0B  LOAD 0x0B
    0x02FB: 40 00  CMPEQ 0x00
    0x02FD: 43 3F  JNZ 0x3F ; -> loc_033E (0x033E)
    0x02FF: 11 0B  LOAD 0x0B
    0x0301: 30 0A  ADDI 0x0A
    0x0303: 12 0B  STORE 0x0B
    0x0305: 11 0A  LOAD 0x0A
    0x0307: 20 00  MOVI 0x00
    0x0309: 51     XCHG
    0x030A: 51     XCHG
    0x030B: 12 63  STORE 0x63
    0x030D: 20 00  MOVI 0x00
    0x030F: 51     XCHG
    0x0310: 12 02  STORE 0x02
    0x0312: 11 63  LOAD 0x63
    0x0314: 51     XCHG
    0x0315: 20 00  MOVI 0x00
    0x0317: 51     XCHG
    0x0318: 12 63  STORE 0x63
    0x031A: 11 01  LOAD 0x01
    0x031C: 50 63  SWAP 0x63
    0x031E: 51     XCHG
    0x031F: 11 63  LOAD 0x63
    0x0321: 51     XCHG
    0x0322: 12 63  STORE 0x63
    0x0324: 20 00  MOVI 0x00
    0x0326: 51     XCHG
    0x0327: 11 01  LOAD 0x01
    0x0329: 51     XCHG
    0x032A: 11 63  LOAD 0x63
    0x032C: 51     XCHG
    0x032D: 20 00  MOVI 0x00
    0x032F: 51     XCHG
    0x0330: 12 63  STORE 0x63
    0x0332: 11 02  LOAD 0x02
    0x0334: 50 63  SWAP 0x63
    0x0336: 51     XCHG
    0x0337: 11 63  LOAD 0x63
    0x0339: 51     XCHG
    0x033A: 11 0A  LOAD 0x0A
    0x033C: 35 0B  XOR_MEM 0x0B
  loc_033E:
    0x033E: 11 0C  LOAD 0x0C
    0x0340: 40 00  CMPEQ 0x00
    0x0342: 43 3F  JNZ 0x3F ; -> loc_0383 (0x0383)
    0x0344: 11 0C  LOAD 0x0C
    0x0346: 30 0A  ADDI 0x0A
    0x0348: 12 0C  STORE 0x0C
    0x034A: 11 0B  LOAD 0x0B
    0x034C: 20 00  MOVI 0x00
    0x034E: 51     XCHG
    0x034F: 51     XCHG
    0x0350: 12 63  STORE 0x63
    0x0352: 20 00  MOVI 0x00
    0x0354: 51     XCHG
    0x0355: 12 02  STORE 0x02
    0x0357: 11 63  LOAD 0x63
    0x0359: 51     XCHG
    0x035A: 20 00  MOVI 0x00
    0x035C: 51     XCHG
    0x035D: 12 63  STORE 0x63
    0x035F: 11 01  LOAD 0x01
    0x0361: 50 63  SWAP 0x63
    0x0363: 51     XCHG
    0x0364: 11 63  LOAD 0x63
    0x0366: 51     XCHG
    0x0367: 12 63  STORE 0x63
    0x0369: 20 00  MOVI 0x00
    0x036B: 51     XCHG
    0x036C: 11 01  LOAD 0x01
    0x036E: 51     XCHG
    0x036F: 11 63  LOAD 0x63
    0x0371: 51     XCHG
    0x0372: 20 00  MOVI 0x00
    0x0374: 51     XCHG
    0x0375: 12 63  STORE 0x63
    0x0377: 11 02  LOAD 0x02
    0x0379: 50 63  SWAP 0x63
    0x037B: 51     XCHG
    0x037C: 11 63  LOAD 0x63
    0x037E: 51     XCHG
    0x037F: 11 0B  LOAD 0x0B
    0x0381: 35 0C  XOR_MEM 0x0C
  loc_0383:
    0x0383: 11 0D  LOAD 0x0D
    0x0385: 40 00  CMPEQ 0x00
    0x0387: 43 3F  JNZ 0x3F ; -> loc_03C8 (0x03C8)
    0x0389: 11 0D  LOAD 0x0D
    0x038B: 30 0A  ADDI 0x0A
    0x038D: 12 0D  STORE 0x0D
    0x038F: 11 0C  LOAD 0x0C
    0x0391: 20 00  MOVI 0x00
    0x0393: 51     XCHG
    0x0394: 51     XCHG
    0x0395: 12 63  STORE 0x63
    0x0397: 20 00  MOVI 0x00
    0x0399: 51     XCHG
    0x039A: 12 02  STORE 0x02
    0x039C: 11 63  LOAD 0x63
    0x039E: 51     XCHG
    0x039F: 20 00  MOVI 0x00
    0x03A1: 51     XCHG
    0x03A2: 12 63  STORE 0x63
    0x03A4: 11 01  LOAD 0x01
    0x03A6: 50 63  SWAP 0x63
    0x03A8: 51     XCHG
    0x03A9: 11 63  LOAD 0x63
    0x03AB: 51     XCHG
    0x03AC: 12 63  STORE 0x63
    0x03AE: 20 00  MOVI 0x00
    0x03B0: 51     XCHG
    0x03B1: 11 01  LOAD 0x01
    0x03B3: 51     XCHG
    0x03B4: 11 63  LOAD 0x63
    0x03B6: 51     XCHG
    0x03B7: 20 00  MOVI 0x00
    0x03B9: 51     XCHG
    0x03BA: 12 63  STORE 0x63
    0x03BC: 11 02  LOAD 0x02
    0x03BE: 50 63  SWAP 0x63
    0x03C0: 51     XCHG
    0x03C1: 11 63  LOAD 0x63
    0x03C3: 51     XCHG
    0x03C4: 11 0C  LOAD 0x0C
    0x03C6: 35 0D  XOR_MEM 0x0D
  loc_03C8:
    0x03C8: 11 0E  LOAD 0x0E
    0x03CA: 40 00  CMPEQ 0x00
    0x03CC: 43 3F  JNZ 0x3F ; -> loc_040D (0x040D)
    0x03CE: 11 0E  LOAD 0x0E
    0x03D0: 30 0A  ADDI 0x0A
    0x03D2: 12 0E  STORE 0x0E
    0x03D4: 11 0D  LOAD 0x0D
    0x03D6: 20 00  MOVI 0x00
    0x03D8: 51     XCHG
    0x03D9: 51     XCHG
    0x03DA: 12 63  STORE 0x63
    0x03DC: 20 00  MOVI 0x00
    0x03DE: 51     XCHG
    0x03DF: 12 02  STORE 0x02
    0x03E1: 11 63  LOAD 0x63
    0x03E3: 51     XCHG
    0x03E4: 20 00  MOVI 0x00
    0x03E6: 51     XCHG
    0x03E7: 12 63  STORE 0x63
    0x03E9: 11 01  LOAD 0x01
    0x03EB: 50 63  SWAP 0x63
    0x03ED: 51     XCHG
    0x03EE: 11 63  LOAD 0x63
    0x03F0: 51     XCHG
    0x03F1: 12 63  STORE 0x63
    0x03F3: 20 00  MOVI 0x00
    0x03F5: 51     XCHG
    0x03F6: 11 01  LOAD 0x01
    0x03F8: 51     XCHG
    0x03F9: 11 63  LOAD 0x63
    0x03FB: 51     XCHG
    0x03FC: 20 00  MOVI 0x00
    0x03FE: 51     XCHG
    0x03FF: 12 63  STORE 0x63
    0x0401: 11 02  LOAD 0x02
    0x0403: 50 63  SWAP 0x63
    0x0405: 51     XCHG
    0x0406: 11 63  LOAD 0x63
    0x0408: 51     XCHG
    0x0409: 11 0D  LOAD 0x0D
    0x040B: 35 0E  XOR_MEM 0x0E
  loc_040D:
    0x040D: 11 0F  LOAD 0x0F
    0x040F: 40 00  CMPEQ 0x00
    0x0411: 43 3F  JNZ 0x3F ; -> loc_0452 (0x0452)
    0x0413: 11 0F  LOAD 0x0F
    0x0415: 30 0A  ADDI 0x0A
    0x0417: 12 0F  STORE 0x0F
    0x0419: 11 0E  LOAD 0x0E
    0x041B: 20 00  MOVI 0x00
    0x041D: 51     XCHG
    0x041E: 51     XCHG
    0x041F: 12 63  STORE 0x63
    0x0421: 20 00  MOVI 0x00
    0x0423: 51     XCHG
    0x0424: 12 02  STORE 0x02
    0x0426: 11 63  LOAD 0x63
    0x0428: 51     XCHG
    0x0429: 20 00  MOVI 0x00
    0x042B: 51     XCHG
    0x042C: 12 63  STORE 0x63
    0x042E: 11 01  LOAD 0x01
    0x0430: 50 63  SWAP 0x63
    0x0432: 51     XCHG
    0x0433: 11 63  LOAD 0x63
    0x0435: 51     XCHG
    0x0436: 12 63  STORE 0x63
    0x0438: 20 00  MOVI 0x00
    0x043A: 51     XCHG
    0x043B: 11 01  LOAD 0x01
    0x043D: 51     XCHG
    0x043E: 11 63  LOAD 0x63
    0x0440: 51     XCHG
    0x0441: 20 00  MOVI 0x00
    0x0443: 51     XCHG
    0x0444: 12 63  STORE 0x63
    0x0446: 11 02  LOAD 0x02
    0x0448: 50 63  SWAP 0x63
    0x044A: 51     XCHG
    0x044B: 11 63  LOAD 0x63
    0x044D: 51     XCHG
    0x044E: 11 0E  LOAD 0x0E
    0x0450: 35 0F  XOR_MEM 0x0F
  loc_0452:
    0x0452: 11 10  LOAD 0x10
    0x0454: 40 00  CMPEQ 0x00
    0x0456: 43 3F  JNZ 0x3F ; -> loc_0497 (0x0497)
    0x0458: 11 10  LOAD 0x10
    0x045A: 30 0A  ADDI 0x0A
    0x045C: 12 10  STORE 0x10
    0x045E: 11 0F  LOAD 0x0F
    0x0460: 20 00  MOVI 0x00
    0x0462: 51     XCHG
    0x0463: 51     XCHG
    0x0464: 12 63  STORE 0x63
    0x0466: 20 00  MOVI 0x00
    0x0468: 51     XCHG
    0x0469: 12 02  STORE 0x02
    0x046B: 11 63  LOAD 0x63
    0x046D: 51     XCHG
    0x046E: 20 00  MOVI 0x00
    0x0470: 51     XCHG
    0x0471: 12 63  STORE 0x63
    0x0473: 11 01  LOAD 0x01
    0x0475: 50 63  SWAP 0x63
    0x0477: 51     XCHG
    0x0478: 11 63  LOAD 0x63
    0x047A: 51     XCHG
    0x047B: 12 63  STORE 0x63
    0x047D: 20 00  MOVI 0x00
    0x047F: 51     XCHG
    0x0480: 11 01  LOAD 0x01
    0x0482: 51     XCHG
    0x0483: 11 63  LOAD 0x63
    0x0485: 51     XCHG
    0x0486: 20 00  MOVI 0x00
    0x0488: 51     XCHG
    0x0489: 12 63  STORE 0x63
    0x048B: 11 02  LOAD 0x02
    0x048D: 50 63  SWAP 0x63
    0x048F: 51     XCHG
    0x0490: 11 63  LOAD 0x63
    0x0492: 51     XCHG
    0x0493: 11 0F  LOAD 0x0F
    0x0495: 35 10  XOR_MEM 0x10
  loc_0497:
    0x0497: 11 11  LOAD 0x11
    0x0499: 40 00  CMPEQ 0x00
    0x049B: 43 3F  JNZ 0x3F ; -> loc_04DC (0x04DC)
    0x049D: 11 11  LOAD 0x11
    0x049F: 30 0A  ADDI 0x0A
    0x04A1: 12 11  STORE 0x11
    0x04A3: 11 10  LOAD 0x10
    0x04A5: 20 00  MOVI 0x00
    0x04A7: 51     XCHG
    0x04A8: 51     XCHG
    0x04A9: 12 63  STORE 0x63
    0x04AB: 20 00  MOVI 0x00
    0x04AD: 51     XCHG
    0x04AE: 12 02  STORE 0x02
    0x04B0: 11 63  LOAD 0x63
    0x04B2: 51     XCHG
    0x04B3: 20 00  MOVI 0x00
    0x04B5: 51     XCHG
    0x04B6: 12 63  STORE 0x63
    0x04B8: 11 01  LOAD 0x01
    0x04BA: 50 63  SWAP 0x63
    0x04BC: 51     XCHG
    0x04BD: 11 63  LOAD 0x63
    0x04BF: 51     XCHG
    0x04C0: 12 63  STORE 0x63
    0x04C2: 20 00  MOVI 0x00
    0x04C4: 51     XCHG
    0x04C5: 11 01  LOAD 0x01
    0x04C7: 51     XCHG
    0x04C8: 11 63  LOAD 0x63
    0x04CA: 51     XCHG
    0x04CB: 20 00  MOVI 0x00
    0x04CD: 51     XCHG
    0x04CE: 12 63  STORE 0x63
    0x04D0: 11 02  LOAD 0x02
    0x04D2: 50 63  SWAP 0x63
    0x04D4: 51     XCHG
    0x04D5: 11 63  LOAD 0x63
    0x04D7: 51     XCHG
    0x04D8: 11 10  LOAD 0x10
    0x04DA: 35 11  XOR_MEM 0x11
  loc_04DC:
    0x04DC: 11 12  LOAD 0x12
    0x04DE: 40 00  CMPEQ 0x00
    0x04E0: 43 3F  JNZ 0x3F ; -> loc_0521 (0x0521)
    0x04E2: 11 12  LOAD 0x12
    0x04E4: 30 0A  ADDI 0x0A
    0x04E6: 12 12  STORE 0x12
    0x04E8: 11 11  LOAD 0x11
    0x04EA: 20 00  MOVI 0x00
    0x04EC: 51     XCHG
    0x04ED: 51     XCHG
    0x04EE: 12 63  STORE 0x63
    0x04F0: 20 00  MOVI 0x00
    0x04F2: 51     XCHG
    0x04F3: 12 02  STORE 0x02
    0x04F5: 11 63  LOAD 0x63
    0x04F7: 51     XCHG
    0x04F8: 20 00  MOVI 0x00
    0x04FA: 51     XCHG
    0x04FB: 12 63  STORE 0x63
    0x04FD: 11 01  LOAD 0x01
    0x04FF: 50 63  SWAP 0x63
    0x0501: 51     XCHG
    0x0502: 11 63  LOAD 0x63
    0x0504: 51     XCHG
    0x0505: 12 63  STORE 0x63
    0x0507: 20 00  MOVI 0x00
    0x0509: 51     XCHG
    0x050A: 11 01  LOAD 0x01
    0x050C: 51     XCHG
    0x050D: 11 63  LOAD 0x63
    0x050F: 51     XCHG
    0x0510: 20 00  MOVI 0x00
    0x0512: 51     XCHG
    0x0513: 12 63  STORE 0x63
    0x0515: 11 02  LOAD 0x02
    0x0517: 50 63  SWAP 0x63
    0x0519: 51     XCHG
    0x051A: 11 63  LOAD 0x63
    0x051C: 51     XCHG
    0x051D: 11 11  LOAD 0x11
    0x051F: 35 12  XOR_MEM 0x12
  loc_0521:
    0x0521: 11 13  LOAD 0x13
    0x0523: 40 00  CMPEQ 0x00
    0x0525: 43 3F  JNZ 0x3F ; -> loc_0566 (0x0566)
    0x0527: 11 13  LOAD 0x13
    0x0529: 30 0A  ADDI 0x0A
    0x052B: 12 13  STORE 0x13
    0x052D: 11 12  LOAD 0x12
    0x052F: 20 00  MOVI 0x00
    0x0531: 51     XCHG
    0x0532: 51     XCHG
    0x0533: 12 63  STORE 0x63
    0x0535: 20 00  MOVI 0x00
    0x0537: 51     XCHG
    0x0538: 12 02  STORE 0x02
    0x053A: 11 63  LOAD 0x63
    0x053C: 51     XCHG
    0x053D: 20 00  MOVI 0x00
    0x053F: 51     XCHG
    0x0540: 12 63  STORE 0x63
    0x0542: 11 01  LOAD 0x01
    0x0544: 50 63  SWAP 0x63
    0x0546: 51     XCHG
    0x0547: 11 63  LOAD 0x63
    0x0549: 51     XCHG
    0x054A: 12 63  STORE 0x63
    0x054C: 20 00  MOVI 0x00
    0x054E: 51     XCHG
    0x054F: 11 01  LOAD 0x01
    0x0551: 51     XCHG
    0x0552: 11 63  LOAD 0x63
    0x0554: 51     XCHG
    0x0555: 20 00  MOVI 0x00
    0x0557: 51     XCHG
    0x0558: 12 63  STORE 0x63
    0x055A: 11 02  LOAD 0x02
    0x055C: 50 63  SWAP 0x63
    0x055E: 51     XCHG
    0x055F: 11 63  LOAD 0x63
    0x0561: 51     XCHG
    0x0562: 11 12  LOAD 0x12
    0x0564: 35 13  XOR_MEM 0x13
  loc_0566:
    0x0566: 11 14  LOAD 0x14
    0x0568: 40 00  CMPEQ 0x00
    0x056A: 43 3F  JNZ 0x3F ; -> loc_05AB (0x05AB)
    0x056C: 11 14  LOAD 0x14
    0x056E: 30 0A  ADDI 0x0A
    0x0570: 12 14  STORE 0x14
    0x0572: 11 13  LOAD 0x13
    0x0574: 20 00  MOVI 0x00
    0x0576: 51     XCHG
    0x0577: 51     XCHG
    0x0578: 12 63  STORE 0x63
    0x057A: 20 00  MOVI 0x00
    0x057C: 51     XCHG
    0x057D: 12 02  STORE 0x02
    0x057F: 11 63  LOAD 0x63
    0x0581: 51     XCHG
    0x0582: 20 00  MOVI 0x00
    0x0584: 51     XCHG
    0x0585: 12 63  STORE 0x63
    0x0587: 11 01  LOAD 0x01
    0x0589: 50 63  SWAP 0x63
    0x058B: 51     XCHG
    0x058C: 11 63  LOAD 0x63
    0x058E: 51     XCHG
    0x058F: 12 63  STORE 0x63
    0x0591: 20 00  MOVI 0x00
    0x0593: 51     XCHG
    0x0594: 11 01  LOAD 0x01
    0x0596: 51     XCHG
    0x0597: 11 63  LOAD 0x63
    0x0599: 51     XCHG
    0x059A: 20 00  MOVI 0x00
    0x059C: 51     XCHG
    0x059D: 12 63  STORE 0x63
    0x059F: 11 02  LOAD 0x02
    0x05A1: 50 63  SWAP 0x63
    0x05A3: 51     XCHG
    0x05A4: 11 63  LOAD 0x63
    0x05A6: 51     XCHG
    0x05A7: 11 13  LOAD 0x13
    0x05A9: 35 14  XOR_MEM 0x14
  loc_05AB:
    0x05AB: 11 15  LOAD 0x15
    0x05AD: 40 00  CMPEQ 0x00
    0x05AF: 43 3F  JNZ 0x3F ; -> loc_05F0 (0x05F0)
    0x05B1: 11 15  LOAD 0x15
    0x05B3: 30 0A  ADDI 0x0A
    0x05B5: 12 15  STORE 0x15
    0x05B7: 11 14  LOAD 0x14
    0x05B9: 20 00  MOVI 0x00
    0x05BB: 51     XCHG
    0x05BC: 51     XCHG
    0x05BD: 12 63  STORE 0x63
    0x05BF: 20 00  MOVI 0x00
    0x05C1: 51     XCHG
    0x05C2: 12 02  STORE 0x02
    0x05C4: 11 63  LOAD 0x63
    0x05C6: 51     XCHG
    0x05C7: 20 00  MOVI 0x00
    0x05C9: 51     XCHG
    0x05CA: 12 63  STORE 0x63
    0x05CC: 11 01  LOAD 0x01
    0x05CE: 50 63  SWAP 0x63
    0x05D0: 51     XCHG
    0x05D1: 11 63  LOAD 0x63
    0x05D3: 51     XCHG
    0x05D4: 12 63  STORE 0x63
    0x05D6: 20 00  MOVI 0x00
    0x05D8: 51     XCHG
    0x05D9: 11 01  LOAD 0x01
    0x05DB: 51     XCHG
    0x05DC: 11 63  LOAD 0x63
    0x05DE: 51     XCHG
    0x05DF: 20 00  MOVI 0x00
    0x05E1: 51     XCHG
    0x05E2: 12 63  STORE 0x63
    0x05E4: 11 02  LOAD 0x02
    0x05E6: 50 63  SWAP 0x63
    0x05E8: 51     XCHG
    0x05E9: 11 63  LOAD 0x63
    0x05EB: 51     XCHG
    0x05EC: 11 14  LOAD 0x14
    0x05EE: 35 15  XOR_MEM 0x15
  loc_05F0:
    0x05F0: 11 16  LOAD 0x16
    0x05F2: 40 00  CMPEQ 0x00
    0x05F4: 43 3F  JNZ 0x3F ; -> loc_0635 (0x0635)
    0x05F6: 11 16  LOAD 0x16
    0x05F8: 30 0A  ADDI 0x0A
    0x05FA: 12 16  STORE 0x16
    0x05FC: 11 15  LOAD 0x15
    0x05FE: 20 00  MOVI 0x00
    0x0600: 51     XCHG
    0x0601: 51     XCHG
    0x0602: 12 63  STORE 0x63
    0x0604: 20 00  MOVI 0x00
    0x0606: 51     XCHG
    0x0607: 12 02  STORE 0x02
    0x0609: 11 63  LOAD 0x63
    0x060B: 51     XCHG
    0x060C: 20 00  MOVI 0x00
    0x060E: 51     XCHG
    0x060F: 12 63  STORE 0x63
    0x0611: 11 01  LOAD 0x01
    0x0613: 50 63  SWAP 0x63
    0x0615: 51     XCHG
    0x0616: 11 63  LOAD 0x63
    0x0618: 51     XCHG
    0x0619: 12 63  STORE 0x63
    0x061B: 20 00  MOVI 0x00
    0x061D: 51     XCHG
    0x061E: 11 01  LOAD 0x01
    0x0620: 51     XCHG
    0x0621: 11 63  LOAD 0x63
    0x0623: 51     XCHG
    0x0624: 20 00  MOVI 0x00
    0x0626: 51     XCHG
    0x0627: 12 63  STORE 0x63
    0x0629: 11 02  LOAD 0x02
    0x062B: 50 63  SWAP 0x63
    0x062D: 51     XCHG
    0x062E: 11 63  LOAD 0x63
    0x0630: 51     XCHG
    0x0631: 11 15  LOAD 0x15
    0x0633: 35 16  XOR_MEM 0x16
  loc_0635:
    0x0635: 11 17  LOAD 0x17
    0x0637: 40 00  CMPEQ 0x00
    0x0639: 43 3F  JNZ 0x3F ; -> loc_067A (0x067A)
    0x063B: 11 17  LOAD 0x17
    0x063D: 30 0A  ADDI 0x0A
    0x063F: 12 17  STORE 0x17
    0x0641: 11 16  LOAD 0x16
    0x0643: 20 00  MOVI 0x00
    0x0645: 51     XCHG
    0x0646: 51     XCHG
    0x0647: 12 63  STORE 0x63
    0x0649: 20 00  MOVI 0x00
    0x064B: 51     XCHG
    0x064C: 12 02  STORE 0x02
    0x064E: 11 63  LOAD 0x63
    0x0650: 51     XCHG
    0x0651: 20 00  MOVI 0x00
    0x0653: 51     XCHG
    0x0654: 12 63  STORE 0x63
    0x0656: 11 01  LOAD 0x01
    0x0658: 50 63  SWAP 0x63
    0x065A: 51     XCHG
    0x065B: 11 63  LOAD 0x63
    0x065D: 51     XCHG
    0x065E: 12 63  STORE 0x63
    0x0660: 20 00  MOVI 0x00
    0x0662: 51     XCHG
    0x0663: 11 01  LOAD 0x01
    0x0665: 51     XCHG
    0x0666: 11 63  LOAD 0x63
    0x0668: 51     XCHG
    0x0669: 20 00  MOVI 0x00
    0x066B: 51     XCHG
    0x066C: 12 63  STORE 0x63
    0x066E: 11 02  LOAD 0x02
    0x0670: 50 63  SWAP 0x63
    0x0672: 51     XCHG
    0x0673: 11 63  LOAD 0x63
    0x0675: 51     XCHG
    0x0676: 11 16  LOAD 0x16
    0x0678: 35 17  XOR_MEM 0x17
  loc_067A:
    0x067A: 11 18  LOAD 0x18
    0x067C: 40 00  CMPEQ 0x00
    0x067E: 43 3F  JNZ 0x3F ; -> loc_06BF (0x06BF)
    0x0680: 11 18  LOAD 0x18
    0x0682: 30 0A  ADDI 0x0A
    0x0684: 12 18  STORE 0x18
    0x0686: 11 17  LOAD 0x17
    0x0688: 20 00  MOVI 0x00
    0x068A: 51     XCHG
    0x068B: 51     XCHG
    0x068C: 12 63  STORE 0x63
    0x068E: 20 00  MOVI 0x00
    0x0690: 51     XCHG
    0x0691: 12 02  STORE 0x02
    0x0693: 11 63  LOAD 0x63
    0x0695: 51     XCHG
    0x0696: 20 00  MOVI 0x00
    0x0698: 51     XCHG
    0x0699: 12 63  STORE 0x63
    0x069B: 11 01  LOAD 0x01
    0x069D: 50 63  SWAP 0x63
    0x069F: 51     XCHG
    0x06A0: 11 63  LOAD 0x63
    0x06A2: 51     XCHG
    0x06A3: 12 63  STORE 0x63
    0x06A5: 20 00  MOVI 0x00
    0x06A7: 51     XCHG
    0x06A8: 11 01  LOAD 0x01
    0x06AA: 51     XCHG
    0x06AB: 11 63  LOAD 0x63
    0x06AD: 51     XCHG
    0x06AE: 20 00  MOVI 0x00
    0x06B0: 51     XCHG
    0x06B1: 12 63  STORE 0x63
    0x06B3: 11 02  LOAD 0x02
    0x06B5: 50 63  SWAP 0x63
    0x06B7: 51     XCHG
    0x06B8: 11 63  LOAD 0x63
    0x06BA: 51     XCHG
    0x06BB: 11 17  LOAD 0x17
    0x06BD: 35 18  XOR_MEM 0x18
  loc_06BF:
    0x06BF: 11 19  LOAD 0x19
    0x06C1: 40 00  CMPEQ 0x00
    0x06C3: 43 3F  JNZ 0x3F ; -> loc_0704 (0x0704)
    0x06C5: 11 19  LOAD 0x19
    0x06C7: 30 0A  ADDI 0x0A
    0x06C9: 12 19  STORE 0x19
    0x06CB: 11 18  LOAD 0x18
    0x06CD: 20 00  MOVI 0x00
    0x06CF: 51     XCHG
    0x06D0: 51     XCHG
    0x06D1: 12 63  STORE 0x63
    0x06D3: 20 00  MOVI 0x00
    0x06D5: 51     XCHG
    0x06D6: 12 02  STORE 0x02
    0x06D8: 11 63  LOAD 0x63
    0x06DA: 51     XCHG
    0x06DB: 20 00  MOVI 0x00
    0x06DD: 51     XCHG
    0x06DE: 12 63  STORE 0x63
    0x06E0: 11 01  LOAD 0x01
    0x06E2: 50 63  SWAP 0x63
    0x06E4: 51     XCHG
    0x06E5: 11 63  LOAD 0x63
    0x06E7: 51     XCHG
    0x06E8: 12 63  STORE 0x63
    0x06EA: 20 00  MOVI 0x00
    0x06EC: 51     XCHG
    0x06ED: 11 01  LOAD 0x01
    0x06EF: 51     XCHG
    0x06F0: 11 63  LOAD 0x63
    0x06F2: 51     XCHG
    0x06F3: 20 00  MOVI 0x00
    0x06F5: 51     XCHG
    0x06F6: 12 63  STORE 0x63
    0x06F8: 11 02  LOAD 0x02
    0x06FA: 50 63  SWAP 0x63
    0x06FC: 51     XCHG
    0x06FD: 11 63  LOAD 0x63
    0x06FF: 51     XCHG
    0x0700: 11 18  LOAD 0x18
    0x0702: 35 19  XOR_MEM 0x19
  loc_0704:
    0x0704: 11 1A  LOAD 0x1A
    0x0706: 40 00  CMPEQ 0x00
    0x0708: 43 3F  JNZ 0x3F ; -> loc_0749 (0x0749)
    0x070A: 11 1A  LOAD 0x1A
    0x070C: 30 0A  ADDI 0x0A
    0x070E: 12 1A  STORE 0x1A
    0x0710: 11 19  LOAD 0x19
    0x0712: 20 00  MOVI 0x00
    0x0714: 51     XCHG
    0x0715: 51     XCHG
    0x0716: 12 63  STORE 0x63
    0x0718: 20 00  MOVI 0x00
    0x071A: 51     XCHG
    0x071B: 12 02  STORE 0x02
    0x071D: 11 63  LOAD 0x63
    0x071F: 51     XCHG
    0x0720: 20 00  MOVI 0x00
    0x0722: 51     XCHG
    0x0723: 12 63  STORE 0x63
    0x0725: 11 01  LOAD 0x01
    0x0727: 50 63  SWAP 0x63
    0x0729: 51     XCHG
    0x072A: 11 63  LOAD 0x63
    0x072C: 51     XCHG
    0x072D: 12 63  STORE 0x63
    0x072F: 20 00  MOVI 0x00
    0x0731: 51     XCHG
    0x0732: 11 01  LOAD 0x01
    0x0734: 51     XCHG
    0x0735: 11 63  LOAD 0x63
    0x0737: 51     XCHG
    0x0738: 20 00  MOVI 0x00
    0x073A: 51     XCHG
    0x073B: 12 63  STORE 0x63
    0x073D: 11 02  LOAD 0x02
    0x073F: 50 63  SWAP 0x63
    0x0741: 51     XCHG
    0x0742: 11 63  LOAD 0x63
    0x0744: 51     XCHG
    0x0745: 11 19  LOAD 0x19
    0x0747: 35 1A  XOR_MEM 0x1A
  loc_0749:
    0x0749: 11 00  LOAD 0x00  //第0位
    0x074B: 40 00  CMPEQ 0x00
    0x074D: 43 06  JNZ 0x06 ; -> loc_0755 (0x0755)
    0x074F: 11 00  LOAD 0x00
    0x0751: 30 10  ADDI 0x10
    0x0753: 12 00  STORE 0x00
  loc_0755:
    0x0755: 11 02  LOAD 0x02  //第2位  奇数位+=0x10
    0x0757: 40 00  CMPEQ 0x00
    0x0759: 43 06  JNZ 0x06 ; -> loc_0761 (0x0761)
    0x075B: 11 02  LOAD 0x02
    0x075D: 30 10  ADDI 0x10
    0x075F: 12 02  STORE 0x02
  loc_0761:
    0x0761: 11 04  LOAD 0x04
    0x0763: 40 00  CMPEQ 0x00
    0x0765: 43 06  JNZ 0x06 ; -> loc_076D (0x076D)
    0x0767: 11 04  LOAD 0x04
    0x0769: 30 10  ADDI 0x10
    0x076B: 12 04  STORE 0x04
  loc_076D:
    0x076D: 11 06  LOAD 0x06
    0x076F: 40 00  CMPEQ 0x00
    0x0771: 43 06  JNZ 0x06 ; -> loc_0779 (0x0779)
    0x0773: 11 06  LOAD 0x06
    0x0775: 30 10  ADDI 0x10
    0x0777: 12 06  STORE 0x06
  loc_0779:
    0x0779: 11 08  LOAD 0x08
    0x077B: 40 00  CMPEQ 0x00
    0x077D: 43 06  JNZ 0x06 ; -> loc_0785 (0x0785)
    0x077F: 11 08  LOAD 0x08
    0x0781: 30 10  ADDI 0x10
    0x0783: 12 08  STORE 0x08
  loc_0785:
    0x0785: 11 0A  LOAD 0x0A
    0x0787: 40 00  CMPEQ 0x00
    0x0789: 43 06  JNZ 0x06 ; -> loc_0791 (0x0791)
    0x078B: 11 0A  LOAD 0x0A
    0x078D: 30 10  ADDI 0x10
    0x078F: 12 0A  STORE 0x0A
  loc_0791:
    0x0791: 11 0C  LOAD 0x0C
    0x0793: 40 00  CMPEQ 0x00
    0x0795: 43 06  JNZ 0x06 ; -> loc_079D (0x079D)
    0x0797: 11 0C  LOAD 0x0C
    0x0799: 30 10  ADDI 0x10
    0x079B: 12 0C  STORE 0x0C
  loc_079D:
    0x079D: 11 0E  LOAD 0x0E
    0x079F: 40 00  CMPEQ 0x00
    0x07A1: 43 06  JNZ 0x06 ; -> loc_07A9 (0x07A9)
    0x07A3: 11 0E  LOAD 0x0E
    0x07A5: 30 10  ADDI 0x10
    0x07A7: 12 0E  STORE 0x0E
  loc_07A9:
    0x07A9: 11 10  LOAD 0x10
    0x07AB: 40 00  CMPEQ 0x00
    0x07AD: 43 06  JNZ 0x06 ; -> loc_07B5 (0x07B5)
    0x07AF: 11 10  LOAD 0x10
    0x07B1: 30 10  ADDI 0x10
    0x07B3: 12 10  STORE 0x10
  loc_07B5:
    0x07B5: 11 12  LOAD 0x12
    0x07B7: 40 00  CMPEQ 0x00
    0x07B9: 43 06  JNZ 0x06 ; -> loc_07C1 (0x07C1)
    0x07BB: 11 12  LOAD 0x12
    0x07BD: 30 10  ADDI 0x10
    0x07BF: 12 12  STORE 0x12
  loc_07C1:
    0x07C1: 11 14  LOAD 0x14
    0x07C3: 40 00  CMPEQ 0x00
    0x07C5: 43 06  JNZ 0x06 ; -> loc_07CD (0x07CD)
    0x07C7: 11 14  LOAD 0x14
    0x07C9: 30 10  ADDI 0x10
    0x07CB: 12 14  STORE 0x14
  loc_07CD:
    0x07CD: 11 16  LOAD 0x16
    0x07CF: 40 00  CMPEQ 0x00
    0x07D1: 43 06  JNZ 0x06 ; -> loc_07D9 (0x07D9)
    0x07D3: 11 16  LOAD 0x16
    0x07D5: 30 10  ADDI 0x10
    0x07D7: 12 16  STORE 0x16
  loc_07D9:
    0x07D9: 11 18  LOAD 0x18
    0x07DB: 40 00  CMPEQ 0x00
    0x07DD: 43 06  JNZ 0x06 ; -> loc_07E5 (0x07E5)
    0x07DF: 11 18  LOAD 0x18
    0x07E1: 30 10  ADDI 0x10
    0x07E3: 12 18  STORE 0x18
  loc_07E5:
    0x07E5: 11 1A  LOAD 0x1A
    0x07E7: 40 00  CMPEQ 0x00
    0x07E9: 43 06  JNZ 0x06 ; -> loc_07F1 (0x07F1)
    0x07EB: 11 1A  LOAD 0x1A
    0x07ED: 30 10  ADDI 0x10
    0x07EF: 12 1A  STORE 0x1A
  loc_07F1:
    0x07F1: 11 01  LOAD 0x01 //第1位
    0x07F3: 40 00  CMPEQ 0x00
    0x07F5: 43 06  JNZ 0x06 ; -> loc_07FD (0x07FD)
    0x07F7: 11 01  LOAD 0x01
    0x07F9: 30 31  ADDI 0x31
    0x07FB: 12 01  STORE 0x01
  loc_07FD:
    0x07FD: 11 03  LOAD 0x03  //第3位 偶数位+=0x31
    0x07FF: 40 00  CMPEQ 0x00
    0x0801: 43 06  JNZ 0x06 ; -> loc_0809 (0x0809)
    0x0803: 11 03  LOAD 0x03
    0x0805: 30 31  ADDI 0x31
    0x0807: 12 03  STORE 0x03
  loc_0809:
    0x0809: 11 05  LOAD 0x05
    0x080B: 40 00  CMPEQ 0x00
    0x080D: 43 06  JNZ 0x06 ; -> loc_0815 (0x0815)
    0x080F: 11 05  LOAD 0x05
    0x0811: 30 31  ADDI 0x31
    0x0813: 12 05  STORE 0x05
  loc_0815:
    0x0815: 11 07  LOAD 0x07
    0x0817: 40 00  CMPEQ 0x00
    0x0819: 43 06  JNZ 0x06 ; -> loc_0821 (0x0821)
    0x081B: 11 07  LOAD 0x07
    0x081D: 30 31  ADDI 0x31
    0x081F: 12 07  STORE 0x07
  loc_0821:
    0x0821: 11 09  LOAD 0x09
    0x0823: 40 00  CMPEQ 0x00
    0x0825: 43 06  JNZ 0x06 ; -> loc_082D (0x082D)
    0x0827: 11 09  LOAD 0x09
    0x0829: 30 31  ADDI 0x31
    0x082B: 12 09  STORE 0x09
  loc_082D:
    0x082D: 11 0B  LOAD 0x0B
    0x082F: 40 00  CMPEQ 0x00
    0x0831: 43 06  JNZ 0x06 ; -> loc_0839 (0x0839)
    0x0833: 11 0B  LOAD 0x0B
    0x0835: 30 31  ADDI 0x31
    0x0837: 12 0B  STORE 0x0B
  loc_0839:
    0x0839: 11 0D  LOAD 0x0D
    0x083B: 40 00  CMPEQ 0x00
    0x083D: 43 06  JNZ 0x06 ; -> loc_0845 (0x0845)
    0x083F: 11 0D  LOAD 0x0D
    0x0841: 30 31  ADDI 0x31
    0x0843: 12 0D  STORE 0x0D
  loc_0845:
    0x0845: 11 0F  LOAD 0x0F
    0x0847: 40 00  CMPEQ 0x00
    0x0849: 43 06  JNZ 0x06 ; -> loc_0851 (0x0851)
    0x084B: 11 0F  LOAD 0x0F
    0x084D: 30 31  ADDI 0x31
    0x084F: 12 0F  STORE 0x0F
  loc_0851:
    0x0851: 11 11  LOAD 0x11
    0x0853: 40 00  CMPEQ 0x00
    0x0855: 43 06  JNZ 0x06 ; -> loc_085D (0x085D)
    0x0857: 11 11  LOAD 0x11
    0x0859: 30 31  ADDI 0x31
    0x085B: 12 11  STORE 0x11
  loc_085D:
    0x085D: 11 13  LOAD 0x13
    0x085F: 40 00  CMPEQ 0x00
    0x0861: 43 06  JNZ 0x06 ; -> loc_0869 (0x0869)
    0x0863: 11 13  LOAD 0x13
    0x0865: 30 31  ADDI 0x31
    0x0867: 12 13  STORE 0x13
  loc_0869:
    0x0869: 11 15  LOAD 0x15
    0x086B: 40 00  CMPEQ 0x00
    0x086D: 43 06  JNZ 0x06 ; -> loc_0875 (0x0875)
    0x086F: 11 15  LOAD 0x15
    0x0871: 30 31  ADDI 0x31
    0x0873: 12 15  STORE 0x15
  loc_0875:
    0x0875: 11 17  LOAD 0x17
    0x0877: 40 00  CMPEQ 0x00
    0x0879: 43 06  JNZ 0x06 ; -> loc_0881 (0x0881)
    0x087B: 11 17  LOAD 0x17
    0x087D: 30 31  ADDI 0x31
    0x087F: 12 17  STORE 0x17
  loc_0881:
    0x0881: 11 19  LOAD 0x19
    0x0883: 40 00  CMPEQ 0x00
    0x0885: 43 06  JNZ 0x06 ; -> loc_088D (0x088D)
    0x0887: 11 19  LOAD 0x19
    0x0889: 30 31  ADDI 0x31
    0x088B: 12 19  STORE 0x19
  loc_088D:
    0x088D: 11 00  LOAD 0x00
    0x088F: 40 60  CMPEQ 0x60
    0x0891: 42 B2  JZ 0xB2 ; -> loc_0945 (0x0945)
    0x0893: 11 01  LOAD 0x01
    0x0895: 40 9B  CMPEQ 0x9B
    0x0897: 42 AC  JZ 0xAC ; -> loc_0945 (0x0945)
    0x0899: 11 02  LOAD 0x02
    0x089B: 40 22  CMPEQ 0x22
    0x089D: 42 A6  JZ 0xA6 ; -> loc_0945 (0x0945)
    0x089F: 11 03  LOAD 0x03
    0x08A1: 40 AC  CMPEQ 0xAC
    0x08A3: 42 A0  JZ 0xA0 ; -> loc_0945 (0x0945)
    0x08A5: 11 04  LOAD 0x04
    0x08A7: 40 40  CMPEQ 0x40
    0x08A9: 42 9A  JZ 0x9A ; -> loc_0945 (0x0945)
    0x08AB: 11 05  LOAD 0x05
    0x08AD: 40 79  CMPEQ 0x79
    0x08AF: 42 94  JZ 0x94 ; -> loc_0945 (0x0945)
    0x08B1: 11 06  LOAD 0x06
    0x08B3: 40 36  CMPEQ 0x36
    0x08B5: 42 8E  JZ 0x8E ; -> loc_0945 (0x0945)
    0x08B7: 11 07  LOAD 0x07
    0x08B9: 40 80  CMPEQ 0x80
    0x08BB: 42 88  JZ 0x88 ; -> loc_0945 (0x0945)
    0x08BD: 11 08  LOAD 0x08
    0x08BF: 40 82  CMPEQ 0x82
    0x08C1: 42 82  JZ 0x82 ; -> loc_0945 (0x0945)
    0x08C3: 11 09  LOAD 0x09
    0x08C5: 40 4A  CMPEQ 0x4A
    0x08C7: 42 7C  JZ 0x7C ; -> loc_0945 (0x0945)
    0x08C9: 11 0A  LOAD 0x0A
    0x08CB: 40 74  CMPEQ 0x74
    0x08CD: 42 76  JZ 0x76 ; -> loc_0945 (0x0945)
    0x08CF: 11 0B  LOAD 0x0B
    0x08D1: 40 18  CMPEQ 0x18
    0x08D3: 42 70  JZ 0x70 ; -> loc_0945 (0x0945)
    0x08D5: 11 0C  LOAD 0x0C
    0x08D7: 40 97  CMPEQ 0x97
    0x08D9: 42 6A  JZ 0x6A ; -> loc_0945 (0x0945)
    0x08DB: 11 0D  LOAD 0x0D
    0x08DD: 40 25  CMPEQ 0x25
    0x08DF: 42 64  JZ 0x64 ; -> loc_0945 (0x0945)
    0x08E1: 11 0E  LOAD 0x0E
    0x08E3: 40 B3  CMPEQ 0xB3
    0x08E5: 42 5E  JZ 0x5E ; -> loc_0945 (0x0945)
    0x08E7: 11 0F  LOAD 0x0F
    0x08E9: 40 23  CMPEQ 0x23
    0x08EB: 42 58  JZ 0x58 ; -> loc_0945 (0x0945)
    0x08ED: 11 10  LOAD 0x10
    0x08EF: 40 A9  CMPEQ 0xA9
    0x08F1: 42 52  JZ 0x52 ; -> loc_0945 (0x0945)
    0x08F3: 11 11  LOAD 0x11
    0x08F5: 40 D3  CMPEQ 0xD3
    0x08F7: 42 4C  JZ 0x4C ; -> loc_0945 (0x0945)
    0x08F9: 11 12  LOAD 0x12
    0x08FB: 40 32  CMPEQ 0x32
    0x08FD: 42 46  JZ 0x46 ; -> loc_0945 (0x0945)
    0x08FF: 11 13  LOAD 0x13
    0x0901: 40 4A  CMPEQ 0x4A
    0x0903: 42 40  JZ 0x40 ; -> loc_0945 (0x0945)
    0x0905: 11 14  LOAD 0x14
    0x0907: 40 86  CMPEQ 0x86
    0x0909: 42 3A  JZ 0x3A ; -> loc_0945 (0x0945)
    0x090B: 11 15  LOAD 0x15
    0x090D: 40 57  CMPEQ 0x57
    0x090F: 42 34  JZ 0x34 ; -> loc_0945 (0x0945)
    0x0911: 11 16  LOAD 0x16
    0x0913: 40 75  CMPEQ 0x75
    0x0915: 42 2E  JZ 0x2E ; -> loc_0945 (0x0945)
    0x0917: 11 17  LOAD 0x17
    0x0919: 40 4A  CMPEQ 0x4A
    0x091B: 42 28  JZ 0x28 ; -> loc_0945 (0x0945)
    0x091D: 11 18  LOAD 0x18
    0x091F: 40 8A  CMPEQ 0x8A
    0x0921: 42 22  JZ 0x22 ; -> loc_0945 (0x0945)
    0x0923: 11 19  LOAD 0x19
    0x0925: 40 61  CMPEQ 0x61
    0x0927: 42 1C  JZ 0x1C ; -> loc_0945 (0x0945)
    0x0929: 11 1A  LOAD 0x1A
    0x092B: 40 5F  CMPEQ 0x5F
    0x092D: 42 16  JZ 0x16 ; -> loc_0945 (0x0945)
    0x092F: 10 04  ALLOC 0x04
    0x0931: 20 79  MOVI 0x79
    0x0933: 12 00  STORE 0x00
    0x0935: 20 65  MOVI 0x65
    0x0937: 12 01  STORE 0x01
    0x0939: 20 73  MOVI 0x73
    0x093B: 12 02  STORE 0x02
    0x093D: 20 00  MOVI 0x00
    0x093F: 12 03  STORE 0x03
    0x0941: 52 02  SYSCALL 0x02 ; PRINT_STRING
    0x0943: 41 10  JMP 0x10 ; -> loc_0955 (0x0955)
  loc_0945:
    0x0945: 10 03  ALLOC 0x03
    0x0947: 20 4E  MOVI 0x4E
    0x0949: 12 00  STORE 0x00
    0x094B: 20 6F  MOVI 0x6F
    0x094D: 12 01  STORE 0x01
    0x094F: 20 00  MOVI 0x00
    0x0951: 12 02  STORE 0x02
    0x0953: 52 02  SYSCALL 0x02 ; PRINT_STRING
  loc_0955:
    0x0955: 13     FREE

  ```

#### exp

```py
cipher = [
    0x60, 0x9B, 0x22, 0xAC, 0x40, 0x79, 0x36, 0x80, 0x82, 0x4A,
    0x74, 0x18, 0x97, 0x25, 0xB3, 0x23, 0xA9, 0xD3, 0x32, 0x4A,
    0x86, 0x57, 0x75, 0x4A, 0x8A, 0x61, 0x5F
]
for i in range(len(cipher)):
    if i%2 != 0: # 注意这里是索引
        cipher[i] = (cipher[i] - 0x31) & 0xFF
    else:
        cipher[i] = (cipher[i] - 0x10) & 0xFF
plain = [0]*len(cipher)
plain[0] = cipher[0] - 0xA
for i in range(1, len(cipher)):
    plain[i] = (cipher[i] ^ cipher[i-1]) & 0xFF
    plain[i] = (plain[i] - 0xA) & 0xFF
print(bytearray(plain))
# F0n_And_3asyViMGa1v1eF9rY@u
```

得到flag：`xmctf{F0n_And_3asyViMGa1v1eF9rY@u}`

# ez_uds

​#uds#​ #nc# 连接提供的容器，返回以下信息：

```
==============================
   ECU UDS Security Server
==============================

Supported Service:
  0x27  SecurityAccess

Usage:
  27 01             -> Request Seed
  27 02 <4byteskey> -> Send Key

Example:
  27 01
  27 02 12 34 56 78

Type 'exit' to disconnect.
================================
```

题目提供了key的计算方法

```python
def generate_seed():
    return random.randint(0, 0xFFFFFFFF)

def calculate_key(seed):
    key = seed ^ 0xA5A5A5A5
    key = ((key << 3) | (key >> 29)) & 0xFFFFFFFF
    key = (key + 0x12345678) & 0xFFFFFFFF
    return key
```

我们去请求seed，返回内容

```
Input HEX (e.g. 2701 or 270212345678): 27 01
67 01 23 97 59 F2
```

这里的67 01为返回的报文的头信息，后面的4字节为key。我们根据提供的算法去算出来key，然后去发送密码。

```
Input HEX (e.g. 2701 or 270212345678): 270243cc3934

[+] Access Granted
polarisctf{4eabdf54-c7cc-472e-82df-35ff22e9d27d}
```

错误的密码返回的是3字节内容。

```
Input HEX (e.g. 2701 or 270212345678): 2702204fe5
7F 27 13
```

# ez_login

​#Python#​ #web#  
​![](/assets/img/media/2026_4_20/Pasted image 20260329120854.png)  
打开容器提供的域名和对应端口，下载看看题目附件app.py。找到admin用户的密码是admin123，登录后获得flag（应该是动态flag）

```
# 你好, admin!

**🎉 权限验证成功：**  
xmctf{65d0ac86-cc9f-4450-bb05-40381ffea3f6}
```

![](/assets/img/media/2026_4_20/Pasted image 20260329120918.png)

# ezFinger

​#stm32#

stm32的bin文件。[stm32逆向参考博客](https://www.cnblogs.com/hetianlab/p/17901626.html)  
不是，flag就是把那两个函数识别出来交上去？？？怪我眼瞎。记得做的时候没有`flag格式xmctf{名称1_名称2}`这一部分，我都不知道出题人要我交啥，又没有main函数。

> ## 3.ezFinger
>
> ​`` `[IOT]` ``
>
> 描述是
>
> ```plain
> sub_8003498 和 sub_8000EC0 处对应的函数名是什么？flag格式xmctf{名称1_名称2}
> ```
>
> 对应函数名：
>
> sub\_8003498 -\> HAL\_RCC\_GetSysClockFreq
>
> ```plain
> HAL: Hardware Abstraction Layer（硬件抽象层）。
>
> RCC: Reset and Clock Control（复位与时钟控制）。
>
> GetSysClockFreq: 获取系统时钟频率。
> ```
>
> ```c
> nsigned int sub_8003498()
> {
>   int v1; // r3
>   __int64 v2; // r0
>   unsigned __int64 v3; // r0
>
>   if ( (MEMORY[0x40023808] & 0xC) == 4 )
>     return 8000000;
>   if ( (MEMORY[0x40023808] & 0xC) != 8 )
>     return 16000000;
>   v1 = (MEMORY[0x40023804] >> 6) & 0x1FF;
>   HIDWORD(v2) = (__int64)((unsigned int)(32 * v1) - (unsigned __int64)(unsigned int)v1) >> 26;   #高 32 位
>   LODWORD(v2) = 1984 * v1;   #低 32 位
>   if ( (MEMORY[0x40023804] & 0x400000) != 0 )
>     v3 = (8 * (v2 - ((unsigned int)(32 * v1) - (unsigned __int64)(unsigned int)v1)) + (unsigned int)v1) << 9;
>   else
>     v3 = (8 * (v2 - ((unsigned int)(32 * v1) - (unsigned __int64)(unsigned int)v1)) + (unsigned int)v1) << 10;
>   return sub_80002D0(v3, HIDWORD(v3), MEMORY[0x40023804] & 0x3F, 0) / (2 * ((HIWORD(MEMORY[0x40023804]) & 3u) + 1));
> }
> ```
>
> 这是一个  系统频率计算器
>
> ​`if ( (MEMORY[0x40023808] & 0xC) == 4 ) `      ##直接内存访问##
>
> 这里在看 `SWS`​ 位。如果是 `4`​，说明系统直接在用 ​**HSE（外部高速时钟）** 。
>
> 它直接返回 `8000000` (8MHz)，因为程序员知道外接的是 8M 晶振。
>
> if ( (MEMORY[0x40023808] & 0xC) !\= 8 )
>
> 如果不是 `8`​（PLL），那就默认是 ​**HSI（内部高速时钟）** 。
>
> 返回 `16000000` (16MHz)，这是芯片内置振荡器的标准频率。
>
> sub\_8003498 直接读 RCC-\>CFGR / RCC-\>PLLCFGR，按 SWS/PLLM/PLLN/PLLP 计算系统时钟，且被上层时钟配置函数用于更新
> SystemCoreClock，这就是 HAL\_RCC\_GetSysClockFreq 的典型实现。
>
> ```plain
> CFGR 是 Clock Configuration Register（时钟配置寄存器）
> SWS (System clock switch status) —— 系统时钟切换状态  状态指示灯    00: 正在用 HSI（内部 16MHz =）。
>
>                                                                01: 正在用 HSE（外部晶振）。
>
>                                                                10: 正在用 PLL
>                                                                
> PLLCFGR 是 Main PLL configuration register（主 PLL 配置寄存器）。
> 由于晶振（HSE/HSI）提供的频率通常只有 8MHz 或 16MHz，想让 CPU 跑到 168MHz 甚至更高，就需要这个加压泵（PLL）。
>
> ```
>
> ```plain
> PLLSRC	选择 HSI(16M) 或 HSE(8M)	初始动力
> PLLM除法 ($\div M$)
> PLLN乘法 ($\times N$)
> PLLP除法 ($\div P$)调整到额定速度
> SWS	确认 PLL 已选定	供 CPU 使用
> ```
>
> sub\_8000EC0 -\> digitalWrite
>
> ```c
> unsigned int __fastcall sub_8000EC0(unsigned int n0x5F, int a2)
> {
>   int v2; // r4
>   int v4; // r0
>
>   if ( n0x5F > 0x5F )
>     v2 = -1;
>   else
>     v2 = aInMKi[n0x5F];  #引脚映射表     作用：硬件芯片（如 STM32）有 PA1, PB5 这种物理叫法，但用户喜欢用 1, 2, 3 这种数字。这个 aInMKi 数组就是一个“户口本”，记录了逻辑号 13 对应的是哪个物理端口和哪个引脚。编码方式：这里的 v2 通常是一个字节，高 4 位存端口（Port），低 4 位存引脚（Pin）。
>   if ( v2 != -1 )
>   {
>     n0x5F = sub_8000F10(v2, &unk_200001B8);
>     if ( n0x5F )
>     {
>       v4 = sub_8000F64((unsigned __int8)v2 >> 4); #提取端口索引 
>       return sub_800128E(v4, (unsigned __int16)(1 << (v2 & 0xF)), a2);  #低 4 位     BSRR 寄存器写入
>     }
>   }
>   return n0x5F;
> }
> ```
>
> sub\_8000EC0 的参数是逻辑 pin 号(芯片封装（Package）上的物理位置。)和高低电平，通过 pin 映射表找到端口/引脚，再往 GPIO BSRR 写置位/复位，语义就是
> digitalWrite(pin, value)。
>
> ```plain
> 如果要设置高电平：往 BSRR 的低 16 位写掩码。
> 如果要设置低电平：往 BSRR 的高 16 位写掩码。
> ```
>
> 告诉单片机的某个引脚（Pin），现在是输出高电压还是低电压。
>
> 所以 flag 是：
>
> xmctf{HAL\_RCC\_GetSysClockFreq\_digitalWrite}

# 移动的秘密

​#自定义加密#​ #md5#

这里用了移位，只进行了>>1右移移位操作，最后比较hash值。问题：会丢失一个精度位，应该需要一定的穷举去爆破。
借助IDA插件FindCrypt和AI确定了后面的函数是md5。

```C
__int64 __fastcall main(__int64 a1, char **a2, char **a3)
{
  int v3; // ebp
  size_t len; // rax
  __int64 i; // rcx
  __int64 v6; // rcx
  __int64 i_1; // rax
  __m128i tempSi128; // [rsp+0h] [rbp-128h] BYREF
  __int64 v10; // [rsp+10h] [rbp-118h]
  char fhashVal[16]; // [rsp+60h] [rbp-C8h] BYREF
  __m128i cip; // [rsp+70h] [rbp-B8h]
  char input[32]; // [rsp+80h] [rbp-A8h] BYREF
  _BYTE v14[29]; // [rsp+A0h] [rbp-88h]
  _BYTE v15[16]; // [rsp+C0h] [rbp-68h]
  __int128 v16; // [rsp+D0h] [rbp-58h]
  __int128 v17; // [rsp+E0h] [rbp-48h]
  __int128 v18; // [rsp+F0h] [rbp-38h]
  unsigned __int64 v19; // [rsp+108h] [rbp-20h]

  v19 = __readfsqword(0x28u);
  sub_5555555560A0();
  memset(input, 0, 29);
  *v15 = 0;
  v16 = 0;
  v17 = 0;
  v18 = 0;
  cip = _mm_load_si128(&xmmword_555555557070);
  puts("This is j1ya's challenge.Try to break it.");
  __printf_chk(1, "Enter the flag: ");
  if ( __isoc99_scanf("%29s", input) == 1 )
  {
    v3 = 1;
    len = strlen(input);
    if ( len )
    {
      for ( i = 0; i != len; ++i )
        v15[i] = input[i] >> 1;
      v6 = 0;
      *v14 = _mm_load_si128(&xmmword_555555557080);
      *&v14[13] = _mm_load_si128(&xmmword_555555557090);
      while ( v14[v6] == v15[v6] )
      {
        if ( v6 == len - 1 )
          goto LABEL_10;
        ++v6;
      }
      v3 = 0;
    }
LABEL_10:
    v10 = 0;
    tempSi128 = _mm_load_si128(&MD5_Constants_555555557060);
    hashFn(&tempSi128, input, len);
    HashFinal(fhashVal, tempSi128.cipher);
    i_1 = 0;
    while ( fhashVal[i_1] == cip.cipher[i_1] )
    {
      if ( ++i_1 == 16 )
      {
        if ( v3 )
        {
          puts("right");
          return 0;
        }
        break;
      }
    }
    puts("wrong");
  }
  return 0;
}
```

最后判断flag是否正确依靠校验md5值和v3标志进行，而v3由上面的代码决定，那么上面这一部分就是出现了密文的。核心部分是

```C
      for ( i = 0; i != len; ++i )
        v15[i] = input[i] >> 1;
```

直接把两个xmmword依靠逻辑拼起来有点麻烦，所以调试获取了一下v14的数据。
最后比较的md5值是小端序存储的，需要转化为大端序，不能直接用。

原来自己手动写的，考虑中间29-7=22，`2^22`种可能，把这个数字转化为一个二进制字符串，依次判断某一位是否为1，决定是否`|1`。效率低，算的慢一点。（极致低效？反正1分钟以内也能算出来）

```python
import hashlib  
cip = [  0x3C, 0x36, 0x31, 0x3A, 0x33, 0x3D, 0x3B, 0x32, 0x36, 0x31,  
  0x18, 0x36, 0x32, 0x2F, 0x19, 0x2F, 0x38, 0x37, 0x36, 0x30,  
  0x39, 0x18, 0x39, 0x2F, 0x18, 0x18, 0x19, 0x19, 0x3E]  
md5 = "3A22C098710019B31C328A861429D3AD".lower()  
  
for i in range(0, 2**22):  
    plain = ""  
    b = str(bin(i))[2:].zfill(22)  
    for j in range(0, 22):  
        ind = j+6  
        if(j <= len(b)-1):  
            if b[j] == '1':  
                plain += chr(cip[ind] << 1 | 1)  
            else:  
                plain += chr(cip[ind] << 1)  
        else:  
            plain += chr(cip[j] << 1)  
    plain = "xmctf{" + plain + "}"  
    plMd5 = hashlib.md5(plain.encode()).hexdigest()  
    if i % 10**6 == 0:  
        print(plain)  
    if plMd5 == md5:  
        print(plain + " !")  
        break
```

这是后面结合AI写的，加一个位用来当掩码，然后去决定那一位到底进不进行`|1`的操作。

```python
import hashlib  
  
cip = [0x3C, 0x36, 0x31, 0x3A, 0x33, 0x3D, 0x3B, 0x32, 0x36, 0x31,  
       0x18, 0x36, 0x32, 0x2F, 0x19, 0x2F, 0x38, 0x37, 0x36, 0x30,  
       0x39, 0x18, 0x39, 0x2F, 0x18, 0x18, 0x19, 0x19, 0x3E]  
  
target_md5 = "3a22c098710019b31c328a861429d3ad"  # 目标MD5（小写）  
known_prefix = "xmctf{"  
  
for i in range(0, 2 ** 22):  
    temp_suffix = ""  
    for bit_pos in range(0, 22):  
        cip_index = 6 + bit_pos  
        if (i >> bit_pos) & 1:  
            temp_suffix += chr(cip[cip_index] << 1 | 1)  
        else:  
            temp_suffix += chr(cip[cip_index] << 1)  
  
    candidate_flag = known_prefix + temp_suffix + "}"  
    computed_md5 = hashlib.md5(candidate_flag.encode()).hexdigest()  
  
    if i % 10**5 == 0:  
        print(f"尝试: {candidate_flag}")  
  
    if computed_md5 == target_md5:  
        print(f"[Flag]: {candidate_flag}")  
        break
```

`xmctf{welc0me_2_polar1s_1022}`

# Hulua

​#Lua#

## 提取字节码

先通过字符串列表找到并重命名main

```cpp
int __fastcall main(int argc, const char **argv, const char **envp)
{
  Stream *Stream; // rax
  char Buffer[68]; // [rsp+30h] [rbp-60h] BYREF
  unsigned int v6; // [rsp+74h] [rbp-1Ch]
  __int64 v7; // [rsp+78h] [rbp-18h]
  size_t v8; // [rsp+80h] [rbp-10h]
  int v9; // [rsp+8Ch] [rbp-4h]

  sub_140026690(argc, argv, envp);
  v9 = 0;
  printf("Please enter the flag: ");
  Stream = psub_1400317D0();
  if ( !fgets(Buffer, 64, Stream) )
    return 1;
  v8 = strlen(Buffer);
  if ( v8 && Buffer[v8 - 1] == 10 )
    Buffer[v8 - 1] = 0;
  v7 = sub_140016A80();
  sub_140016B70(v7);
  sub_140003070(v7, Buffer);
  sub_1400038C0(v7, "user_input");
  v6 = sub_140016310(v7, &dword_140033000, dword_1400333DC, "check", 0);
  if ( sub_14000152F(v7, v6) )
  {
    v6 = sub_140003F20(v7, 0, 1, 0, 0, 0);
    if ( sub_14000152F(v7, v6) )
    {
      if ( sub_140001FC0(v7, 0xFFFFFFFFLL) == 1 )
      {
        v9 = sub_140002960(v7, 0xFFFFFFFFLL);
        if ( v9 )
          printf("\n[+] Congratulations! The flag is correct.\n");
        else
          printf("\n[-] Wrong flag. Try again.\n");
      }
      else
      {
        printf("\n[!] Error: Script returned non-boolean value.\n");
      }
    }
  }
  sub_14000EA30(v7);
  system("pause");
  return 0;
}
```

通过题目提示，以及main函数内容，上下文，可以知道这里有个Lua虚拟机。

这一行识别一下，第二个参数是个数组，`v6 = sub_140016310(v7, &dword_140033000, dword_1400333DC, "check", 0);`​第三个指数组的长度。重新识别后长这样`v6 = sub_140016310(v7, &byte_140033000, i, "check", 0);`

## 解密字节码

这是编译后的字节码，应以`\x1bLua`​开头，但这里并没有。猜测可能对字节码做了处理，搜索`byte_140033000`​的交叉引用找到函数`sub_1400014A4`

```cpp
unsigned __int64 sub_1400014A4()
{
  unsigned __int64 i_1; // rax
  unsigned __int64 i_2; // [rsp+18h] [rbp-18h]
  unsigned __int64 i; // [rsp+28h] [rbp-8h]

  i_2 = ::i;
  for ( i = 0; ; ++i )
  {
    i_1 = i;
    if ( i >= i_2 )
      break;
    *(&byte_140033000 + i) ^= aHulua[i % 5];
  }
  return i_1;
}
```

对Lua字节码进行了异或处理。i是全局变量，上面重新识别后对应数组长度。

处理的代码

```py
opcode = [
0x73, 0x39, 0x19, 0x14, 0x32, 0x68, 0x6c, 0xff, 0x78, 0x6b, 0x72, 0x7f, 0x68, 0x7d, 0x65, 0x60, 0x7d, 0x14, 0x23, 0x61, 0x68, 0x75, 0x6c, 0x75, 0x61, 0x68, 0x75, 0x6c, 0x75, 0x61, 0x40, 0x2, 0x2c, 0x74, 0x61, 0x68, 0x75, 0x6c, 0x75, 0x61, 0x68, 0x75, 0x6c, 0x75, 0x60, 0x61, 0x57, 0x6c, 0x75, 0x61, 0x6b, 0x75, 0xec, 0x75, 0x20, 0x68, 0x75, 0x6c, 0xf4, 0x21, 0x68, 0x75, 0x80, 0x75, 0x61, 0x68, 0x59, 0x2d, 0x75, 0x61, 0x2e, 0xf4, 0x2c, 0x75, 0x6e, 0xa8, 0xb5, 0x6e, 0x7b, 0x21, 0x68, 0xf5, 0x2f, 0x74, 0x61, 0x68, 0x13, 0x6d, 0x75, 0x60, 0x2e, 0xf4, 0x2c, 0x75, 0x0, 0x69, 0xf5, 0x6e, 0x3a, 0x61, 0xa9, 0x77, 0x62, 0x35, 0x61, 0xe8, 0x36, 0x6d, 0x75, 0x61, 0xe, 0x74, 0x6c, 0x74, 0x21, 0x69, 0x75, 0x6e, 0xf5, 0x60, 0xe8, 0x75, 0x8, 0xf4, 0x61, 0x69, 0xf5, 0x6d, 0xf5, 0x60, 0xa8, 0x74, 0xec, 0x77, 0x67, 0xea, 0x35, 0x6c, 0xd1, 0xe0, 0xe8, 0x74, 0xac, 0x74, 0x61, 0x6a, 0x75, 0x6e, 0x75, 0x60, 0x8c, 0xf4, 0x6c, 0x74, 0x6e, 0xa8, 0x74, 0x6f, 0x7b, 0xe1, 0x68, 0xf5, 0x6f, 0x77, 0xe1, 0x68, 0x53, 0x6e, 0x75, 0x60, 0x66, 0x35, 0x6c, 0xf5, 0x62, 0x6a, 0x75, 0x6c, 0x53, 0x63, 0x68, 0x74, 0x4a, 0x75, 0xe1, 0x68, 0x70, 0x6c, 0x75, 0x61, 0x6c, 0x6e, 0x5b, 0x4d, 0x41, 0x5e, 0x31, 0x4c, 0x43, 0x52, 0x48, 0x42, 0x58, 0x55, 0x57, 0x5e, 0x55, 0x5f, 0x47, 0x41, 0x5b, 0x45, 0x4c, 0x46, 0x53, 0x48, 0x46, 0x5a, 0x61, 0x1, 0x50, 0x37, 0x4c, 0x4d, 0x23, 0x48, 0x42, 0x5b, 0x55, 0x23, 0x2d, 0x55, 0x5a, 0x4d, 0x41, 0x5e, 0x44, 0x4c, 0x4d, 0x57, 0x48, 0x43, 0x54, 0x55, 0x24, 0x5d, 0x55, 0x5a, 0x46, 0x41, 0x2d, 0x30, 0x4c, 0x4d, 0x55, 0x48, 0x46, 0x59, 0x55, 0x57, 0x2e, 0x55, 0x59, 0x4d, 0x41, 0x2b, 0x4d, 0x4c, 0x40, 0x50, 0x48, 0x45, 0x2a, 0x55, 0x57, 0x2d, 0x55, 0x55, 0x41, 0x41, 0x5f, 0x45, 0x4c, 0x30, 0x56, 0x48, 0x47, 0x5a, 0x55, 0x58, 0x58, 0x55, 0x2e, 0x43, 0x41, 0x5f, 0x40, 0x4c, 0x30, 0x22, 0x48, 0x47, 0x54, 0x55, 0x20, 0x2e, 0x55, 0x5d, 0x41, 0x41, 0x2d, 0x47, 0x4c, 0x30, 0x52, 0x6c, 0x7e, 0x19, 0x6, 0x4, 0x1a, 0x2a, 0x5, 0x1b, 0x11, 0x1d, 0x1, 0x6c, 0x66, 0x41, 0x68, 0x75, 0x6c, 0x75, 0x61, 0x68, 0x75, 0x6d, 0x75, 0x61, 0x68, 0x74, 0x6c, 0x77, 0x61, 0x68, 0x75, 0x6c, 0x65, 0x61, 0x68, 0x75, 0x5f, 0x75, 0x61, 0x68, 0x77, 0x6c, 0x60, 0x31, 0x68, 0x75, 0x6c, 0xfe, 0x61, 0x68, 0x75, 0xa7, 0x75, 0x61, 0x68, 0x73, 0x6d, 0x35, 0x61, 0x6f, 0x34, 0x2c, 0x77, 0x21, 0x69, 0x75, 0x6c, 0xf4, 0xe0, 0x68, 0x75, 0xad, 0xb4, 0x61, 0x68, 0x51, 0x6d, 0x75, 0x63, 0x83, 0x35, 0x6c, 0x75, 0x40, 0x69, 0xf5, 0x6d, 0x34, 0x60, 0x69, 0x75, 0xed, 0x34, 0x60, 0x68, 0xb4, 0xed, 0x75, 0x61, 0x0, 0x74, 0x6c, 0xf5, 0xeb, 0x68, 0x77, 0x68, 0x12, 0x20, 0x97, 0xa, 0x2d, 0x74, 0x60, 0x68, 0xf4, 0x6d, 0x74, 0x61, 0xa9, 0x34, 0x6d, 0x75, 0x60, 0xea, 0x75, 0x6c, 0xdd, 0xe0, 0x6a, 0xf5, 0xf9, 0x77, 0xe0, 0x6c, 0xe7, 0xee, 0x35, 0x64, 0xef, 0xf7, 0xee, 0x74, 0xa6, 0x2a, 0x77, 0x6d, 0xa7, 0xa3, 0xea, 0x77, 0xbe, 0xf7, 0xe3, 0x6d, 0x20, 0xed, 0xb4, 0x64, 0xaf, 0x37, 0x6d, 0x74, 0x66, 0x2b, 0x77, 0x6d, 0xff, 0x61, 0xeb, 0x77, 0xe6, 0xb5, 0xe3, 0x6c, 0xd2, 0xad, 0x89, 0x1e, 0xe9, 0x74, 0x6d, 0x75, 0xa0, 0x69, 0x74, 0x6c, 0x7e, 0x63, 0x68, 0x75, 0x27, 0x77, 0x61, 0x68, 0xf3, 0x6e, 0x35, 0x61, 0xef, 0x37, 0x2c, 0x70, 0xa1, 0x6a, 0xf5, 0x6c, 0x74, 0xe2, 0x68, 0x75, 0x2d, 0xb6, 0x61, 0x68, 0xd1, 0x6e, 0x75, 0x63, 0x3, 0x37, 0x6c, 0x75, 0xe0, 0xea, 0x75, 0x6c, 0x94, 0x63, 0xe8, 0x71, 0x6d, 0xf6, 0x61, 0x68, 0xdd, 0x6e, 0x73, 0xe1, 0xfa, 0xf6, 0x2c, 0x76, 0xf4, 0xe9, 0x34, 0x6b, 0xf2, 0xe2, 0x69, 0x74, 0xfe, 0xf6, 0xe2, 0x6b, 0xa0, 0xed, 0x34, 0x66, 0xef, 0xb6, 0x6d, 0x74, 0xa6, 0xeb, 0x74, 0x6d, 0xff, 0xa1, 0xeb, 0x76, 0xe6, 0xf5, 0x62, 0x6b, 0xf2, 0xef, 0x74, 0x60, 0xaf, 0xb6, 0x6d, 0x74, 0xf3, 0xab, 0x76, 0x6b, 0xe0, 0xe2, 0x29, 0x72, 0xab, 0xf6, 0x62, 0x69, 0x72, 0x28, 0xf6, 0x65, 0x73, 0xb1, 0x6f, 0x7d, 0x7a, 0xac, 0x34, 0x64, 0x33, 0x65, 0x2a, 0x75, 0x2b, 0x31, 0xa3, 0x60, 0xf5, 0x68, 0x75, 0x65, 0xae, 0x71, 0x2c, 0x75, 0xa6, 0xec, 0xb7, 0x65, 0x75, 0x64, 0x68, 0x7d, 0x88, 0x71, 0x61, 0x69, 0x11, 0x28, 0x75, 0x61, 0xcf, 0x37, 0x95, 0xa, 0xe7, 0x6a, 0x37, 0x6c, 0xf2, 0xa3, 0x2a, 0x70, 0xac, 0x77, 0x61, 0x6c, 0xd0, 0x6e, 0x75, 0x60, 0xce, 0x77, 0x6c, 0x75, 0x47, 0x68, 0xf5, 0x6c, 0x79, 0x61, 0x68, 0x75, 0x68, 0x72, 0x12, 0x1c, 0x7, 0x5, 0x1b, 0x6, 0x6c, 0x70, 0xe, 0xc, 0x15, 0xd, 0x66, 0x6d, 0x75, 0x61, 0x68, 0x75, 0x6c, 0x75, 0x61, 0x7b, 0x8a, 0x93, 0x8a, 0x9e, 0x97, 0x8a, 0x93, 0x8a, 0x72, 0x68, 0x75, 0x6c, 0x75, 0x61, 0x68, 0x75, 0x6c, 0x66, 0x9e, 0x68, 0x75, 0x6c, 0x75, 0x61, 0x68, 0x75, 0x7f, 0x75, 0x60, 0x68, 0x75, 0x6c, 0x75, 0x61, 0x68, 0x66, 0xa, 0x75, 0x61, 0x68, 0x75, 0x6c, 0x75, 0x61, 0x6c, 0x73, 0x18, 0x14, 0x3, 0x4, 0x10, 0x68, 0x72, 0x8, 0x6, 0x6, 0x9, 0x7, 0x15, 0x6c, 0x70, 0xf, 0x1d, 0x0, 0x1a, 0x71, 0x6b, 0x16, 0xe, 0x6, 0x16, 0xd, 0x1, 0x60, 0x68, 0x75, 0x6c, 0x75, 0x61, 0x68, 0x75, 0x6c, 0x75, 0x61, 0x68, 0x75, 0x6c, 0x75, 0x61, 0x68, 0x75, 0x6c, 0x75, 0x61, 0x68, 0x75, 0x53, 0x75, 0x61, 0x68, 0x30, 0x6c, 0x75, 0x61, 0x69, 0x75, 0x67, 0x61, 0x61, 0x68, 0x75, 0x2d, 0x75, 0x61, 0x68, 0xf3, 0x2c, 0x35, 0x61, 0xef, 0xf5, 0x2c, 0x74, 0xa1, 0x68, 0x75, 0x6c, 0x74, 0xa0, 0x68, 0x75, 0xc8, 0x75, 0xe0, 0x69, 0x7b, 0x6c, 0x77, 0xe1, 0xe8, 0x74, 0xec, 0x75, 0xa7, 0x29, 0x35, 0x6c, 0xb2, 0x60, 0xa9, 0x76, 0x6a, 0x37, 0x20, 0x68, 0x35, 0x6e, 0xf5, 0x63, 0xe9, 0xf7, 0x6d, 0x75, 0x45, 0x6a, 0xf5, 0x6d, 0x91, 0xe0, 0x68, 0x75, 0x21, 0xb5, 0x60, 0x6b, 0xdc, 0x2c, 0x75, 0x61, 0x42, 0x74, 0x91, 0xa, 0x7, 0x68, 0x75, 0x6d, 0x53, 0x61, 0xe8, 0x75, 0x6b, 0x75, 0x61, 0x68, 0x71, 0x6d, 0x71, 0x66, 0x1b, 0x1, 0x1e, 0x1c, 0xf, 0xf, 0x71, 0x6b, 0x12, 0xc, 0x9, 0x1, 0xf, 0x1d, 0x65, 0x6c, 0x50, 0x14, 0x5e, 0x65, 0x6d, 0x16, 0x4, 0x14, 0x13, 0x6c, 0x7c, 0x18, 0x1a, 0xf, 0x1d, 0x18, 0xe, 0x10, 0x13, 0x7b, 0x65, 0x6c, 0x75, 0x61, 0x68, 0x75, 0x6c, 0x75, 0x60, 0x68, 0x75, 0x6c, 0x75, 0x61, 0x68, 0x75, 0x6c, 0x75, 0x61, 0x68, 0x75, 0x6c, 0x75, 0x61, 0x68, 0x75, 0x6c, 0x75, 0x61, 0x68, 0x75, 0x6c, 0x75, 0x61, 0x68, 0x75, 0x6c, 0x75, 0x61, 0x68, 0x75, 0x6c
    ]
key = bytearray(b'hulua')
plain = bytearray()
for i in range(len(opcode)):
    plain.append(opcode[i] ^ key[i % len(key)])
print(plain)
print(plain.hex())
with open("decrypted.luac", "wb") as f:
    f.write(bytes(plain))
```

直接`print(plain)`​可以看到`bytearray(b'\x1bLua`，符合Lua字节码的开头。然后后面紧跟的字节是0x53，代表lua版本为5.3。

## 反汇编

处理后的内容

```plaintext
1B 4C 75 61 53 00 19 93 0D 0A 1A 0A 04 08 04 08
08 78 56 00 00 00 00 00 00 00 00 00 00 00 28 77
40 01 00 00 00 00 00 00 00 00 00 00 01 09 22 00
00 00 03 00 80 00 41 00 00 00 81 40 00 00 EC 00
00 00 2C 41 00 00 46 81 40 00 0F C0 C0 02 0E 40
00 80 43 01 00 00 66 01 00 01 46 81 40 00 61 01
80 02 4F 00 C1 02 0E 40 00 80 43 01 00 00 66 01
00 01 40 01 00 02 80 01 80 00 64 81 00 01 80 01
80 01 C0 01 80 02 06 82 40 00 A4 81 80 01 C0 01
00 02 00 02 00 01 E4 81 00 01 0F C0 01 03 0E 80
00 80 03 02 80 00 26 02 00 01 0E 40 00 80 03 02
00 00 26 02 00 01 26 00 80 00 05 00 00 00 04 1B
37 38 20 36 44 20 36 33 20 37 34 20 36 36 20 33
32 20 33 30 20 33 32 20 33 36 14 60 38 42 20 38
42 20 37 37 20 42 45 20 36 38 20 36 31 20 38 36
20 36 38 20 45 35 20 36 33 20 45 45 20 38 34 20
33 35 20 36 46 20 35 38 20 43 38 20 35 31 20 30
46 20 36 45 20 39 34 20 37 30 20 45 37 20 32 36
20 39 30 20 42 36 20 37 35 20 45 43 20 32 38 20
41 46 20 31 34 20 45 32 20 45 33 04 0B 75 73 65
72 5F 69 6E 70 75 74 00 13 20 00 00 00 00 00 00
00 01 00 00 00 01 00 02 00 00 00 00 10 00 00 00
33 00 00 00 02 00 15 50 00 00 00 8B 00 00 00 CB
00 00 00 06 01 40 00 07 41 40 02 40 01 00 00 81
81 00 00 C1 C1 00 00 24 01 00 02 EB 40 00 00 21
01 80 01 41 01 01 00 81 41 01 00 C1 81 00 00 68
01 00 80 8A 00 02 04 67 41 FF 7F 41 01 01 00 81
01 01 00 C1 41 01 00 01 82 00 00 A8 81 02 80 95
02 81 04 92 82 40 05 87 82 82 01 C7 42 02 01 D2
C2 82 02 D2 82 82 05 55 81 C1 05 C7 42 01 01 07
43 02 01 8A 00 83 02 8A C0 82 04 A7 C1 FC 7F 81
01 01 00 C1 01 01 00 0B 02 00 00 4B 02 00 00 86
02 40 00 87 42 40 05 C0 02 80 00 01 83 00 00 41
C3 00 00 A4 02 00 02 6B 42 00 00 81 82 00 00 E1
02 80 04 01 83 00 00 A8 02 06 80 92 83 40 03 95
81 41 07 87 83 01 01 92 83 83 03 D5 81 41 07 87
C3 01 01 C7 83 01 01 8A C0 83 03 8A 80 03 03 87
83 01 01 C7 C3 01 01 92 C3 03 07 95 83 41 07 C7
83 03 01 07 44 83 04 1B C4 03 08 1B C4 41 08 46
04 42 00 47 44 C2 08 80 04 00 04 C6 04 40 00 C7
84 C2 09 00 05 00 08 E4 04 00 01 64 44 00 00 A7
42 F9 7F 86 02 42 00 87 C2 42 05 C0 02 00 04 A5
02 00 01 A6 02 00 00 26 00 80 00 0C 00 00 00 04
07 73 74 72 69 6E 67 04 05 62 79 74 65 13 01 00
00 00 00 00 00 00 13 FF FF FF FF FF FF FF FF 13
00 00 00 00 00 00 00 00 13 FF 00 00 00 00 00 00
00 13 00 01 00 00 00 00 00 00 13 66 00 00 00 00
00 00 00 04 06 74 61 62 6C 65 04 07 69 6E 73 65
72 74 04 05 63 68 61 72 04 07 63 6F 6E 63 61 74
01 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 3F 00 00 00 45 00 00 00 01
00 0B 14 00 00 00 41 00 00 00 86 40 40 00 87 80
40 01 C0 00 00 00 01 C1 00 00 A4 00 81 01 0E 00
02 80 80 01 80 00 C6 41 40 00 C7 01 C1 03 06 42
41 00 40 02 80 02 81 82 01 00 24 02 80 01 E4 81
00 00 4D C0 01 03 A9 40 00 00 2A 01 FD 7F 66 00
00 01 26 00 80 00 07 00 00 00 04 01 04 07 73 74
72 69 6E 67 04 07 67 6D 61 74 63 68 04 04 25 78
2B 04 05 63 68 61 72 04 09 74 6F 6E 75 6D 62 65
72 13 10 00 00 00 00 00 00 00 01 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
00 00 00 00 00 00 00 00 00 00 00 00

```

这之后，可以尝试反编译，不过unluac反编译失败，只能反汇编。或者luac反汇编

unluac反汇编：`java -jar unluac_2025_12_23.jar --disassemble decrypted.luac > dd.asm`

luac：`luac53.exe -l -l decrypted.luac > bt.txt`

第一个的结果

```x86asm
.version	5.3

.format	0
.int_size	4
.size_t_size	8
.instruction_size	4
.integer_format	8
.float_format	8
.endianness	LITTLE

.function	main

.source	null
.linedefined	0
.lastlinedefined	0
.numparams	0
.is_vararg	1
.maxstacksize	9

.upvalue	null	0	true

.constant	k0	"78 6D 63 74 66 32 30 32 36"
.constant	k1	L"8B 8B 77 BE 68 61 86 68 E5 63 EE 84 35 6F 58 C8 51 0F 6E 94 70 E7 26 90 B6 75 EC 28 AF 14 E2 E3"
.constant	k2	"user_input"
.constant	k3	nil
.constant	k4	32

loadbool      r0     1     0
loadk         r1    k0 ; k0 = "78 6D 63 74 66 32 30" (truncated)
loadk         r2    k1 ; k1 = L"8B 8B 77 BE 68 61 86" (truncated)
closure       r3    f0
closure       r4    f1
gettabup      r5    u0    k2 ; k2 = "user_input"
mul           r0    r5    k3 ; k3 = nil
sub           r0    k0    r1 ; k0 = "78 6D 63 74 66 32 30" (truncated)
loadbool      r5     0     0
return        r5     2
gettabup      r5    u0    k2 ; k2 = "user_input"
le             5    r5    r0
mul           r1    r5    k4 ; k4 = 32
sub           r0    k0    r1 ; k0 = "78 6D 63 74 66 32 30" (truncated)
loadbool      r5     0     0
return        r5     2
move          r5    r4
move          r6    r1
call          r5     2     2
move          r6    r3
move          r7    r5
gettabup      r8    u0    k2 ; k2 = "user_input"
call          r6     3     2
move          r7    r4
move          r8    r2
call          r7     2     2
mul           r0    r6    r7
sub           r0    k0    r2 ; k0 = "78 6D 63 74 66 32 30" (truncated)
loadbool      r8     1     0
return        r8     2
sub           r0    k0    r1 ; k0 = "78 6D 63 74 66 32 30" (truncated)
loadbool      r8     0     0
return        r8     2
return        r0     1

.function	main/f0

.source	null
.linedefined	16
.lastlinedefined	51
.numparams	2
.is_vararg	0
.maxstacksize	21

.upvalue	null	0	false

.constant	k0	"string"
.constant	k1	"byte"
.constant	k2	1
.constant	k3	-1
.constant	k4	0
.constant	k5	255
.constant	k6	256
.constant	k7	102
.constant	k8	"table"
.constant	k9	"insert"
.constant	k10	"char"
.constant	k11	"concat"

newtable      r2     0     0
newtable      r3     0     0
gettabup      r4    u0    k0 ; k0 = "string"
gettable      r4    r4    k1 ; k1 = "byte"
move          r5    r0
loadk         r6    k2 ; k2 = 1
loadk         r7    k3 ; k3 = -1
call          r4     4     0
setlist       r3     0     1
le             4    r3    r0
loadk         r5    k4 ; k4 = 0
loadk         r6    k5 ; k5 = 255
loadk         r7    k2 ; k2 = 1
forprep       r5   l16
.label	l15
settable      r2    r8    r8
.label	l16
forloop       r5   l15
loadk         r5    k4 ; k4 = 0
loadk         r6    k4 ; k4 = 0
loadk         r7    k5 ; k5 = 255
loadk         r8    k2 ; k2 = 1
forprep       r6   l33
.label	l22
bor          r10    r9    r4
div          r10   r10    k2 ; k2 = 1
gettable     r10    r3   r10
gettable     r11    r2    r9
div          r11    r5   r11
div          r11   r11   r10
bor           r5   r11    k6 ; k6 = 256
gettable     r11    r2    r5
gettable     r12    r2    r9
settable      r2    r5   r12
settable      r2    r9   r11
.label	l33
forloop       r6   l22
loadk         r6    k4 ; k4 = 0
loadk         r7    k4 ; k4 = 0
newtable      r8     0     0
newtable      r9     0     0
gettabup     r10    u0    k0 ; k0 = "string"
gettable     r10   r10    k1 ; k1 = "byte"
move         r11    r1
loadk        r12    k2 ; k2 = 1
loadk        r13    k3 ; k3 = -1
call         r10     4     0
setlist       r9     0     1
loadk        r10    k2 ; k2 = 1
le            11    r9    r0
loadk        r12    k2 ; k2 = 1
forprep      r10   l74
.label	l49
div          r14    r6    k2 ; k2 = 1
bor           r6   r14    k6 ; k6 = 256
gettable     r14    r2    r6
div          r14    r7   r14
bor           r7   r14    k6 ; k6 = 256
gettable     r14    r2    r7
gettable     r15    r2    r6
settable      r2    r7   r15
settable      r2    r6   r14
gettable     r14    r2    r6
gettable     r15    r2    r7
div          r14   r14   r15
bor          r14   r14    k6 ; k6 = 256
gettable     r15    r2   r14
gettable     r16    r9   r13
not          r16   r16 c= 15
not          r16   r16 c= 263
gettabup     r17    u0    k8 ; k8 = "table"
gettable     r17   r17    k9 ; k9 = "insert"
move         r18    r8
gettabup     r19    u0    k0 ; k0 = "string"
gettable     r19   r19   k10 ; k10 = "char"
move         r20   r16
call         r19     2     0
call         r17     0     1
.label	l74
forloop      r10   l49
gettabup     r10    u0    k8 ; k8 = "table"
gettable     r10   r10   k11 ; k11 = "concat"
move         r11    r8
tailcall     r10     2
return       r10     0
return        r0     1

.function	main/f1

.source	null
.linedefined	63
.lastlinedefined	69
.numparams	1
.is_vararg	0
.maxstacksize	11

.upvalue	null	0	false

.constant	k0	""
.constant	k1	"string"
.constant	k2	"gmatch"
.constant	k3	"%x+"
.constant	k4	"char"
.constant	k5	"tonumber"
.constant	k6	16

loadk         r1    k0 ; k0 = ""
gettabup      r2    u0    k1 ; k1 = "string"
gettable      r2    r2    k2 ; k2 = "gmatch"
move          r3    r0
loadk         r4    k3 ; k3 = "%x+"
call          r2     3     4
sub           r0    k0    r8 ; k0 = ""
.label	l8
move          r6    r1
gettabup      r7    u0    k1 ; k1 = "string"
gettable      r7    r7    k4 ; k4 = "char"
gettabup      r8    u0    k5 ; k5 = "tonumber"
move          r9    r5
loadk        r10    k6 ; k6 = 16
call          r8     3     0
call          r7     0     2
add           r1    r6    r7
tforcall      r2     1
tforloop      r4    l8
return        r1     2
return        r0     1


```

第二个luac反汇编的结果

```x86asm

main <?:0,0> (34 instructions at 0000000000c090b0)
0+ params, 9 slots, 1 upvalue, 0 locals, 5 constants, 2 functions
	1	[-]	LOADBOOL 	0 1 0
	2	[-]	LOADK    	1 -1	; "78 6D 63 74 66 32 30 32 36"
	3	[-]	LOADK    	2 -2	; "8B 8B 77 BE 68 61 86 68 E5 63 EE 84 35 6F 58 C8 51 0F 6E 94 70 E7 26 90 B6 75 EC 28 AF 14 E2 E3"
	4	[-]	CLOSURE  	3 0	; 0000000000c092a0
	5	[-]	CLOSURE  	4 1	; 0000000000c09560
	6	[-]	GETTABUP 	5 0 -3	; - "user_input"
	7	[-]	MUL      	0 5 -4	; - nil
	8	[-]	SUB      	0 -1 1	; "78 6D 63 74 66 32 30 32 36" -
	9	[-]	LOADBOOL 	5 0 0
	10	[-]	RETURN   	5 2
	11	[-]	GETTABUP 	5 0 -3	; - "user_input"
	12	[-]	LE       	5 5 0
	13	[-]	MUL      	1 5 -5	; - 32
	14	[-]	SUB      	0 -1 1	; "78 6D 63 74 66 32 30 32 36" -
	15	[-]	LOADBOOL 	5 0 0
	16	[-]	RETURN   	5 2
	17	[-]	MOVE     	5 4
	18	[-]	MOVE     	6 1
	19	[-]	CALL     	5 2 2
	20	[-]	MOVE     	6 3
	21	[-]	MOVE     	7 5
	22	[-]	GETTABUP 	8 0 -3	; - "user_input"
	23	[-]	CALL     	6 3 2
	24	[-]	MOVE     	7 4
	25	[-]	MOVE     	8 2
	26	[-]	CALL     	7 2 2
	27	[-]	MUL      	0 6 7
	28	[-]	SUB      	0 -1 2	; "78 6D 63 74 66 32 30 32 36" -
	29	[-]	LOADBOOL 	8 1 0
	30	[-]	RETURN   	8 2
	31	[-]	SUB      	0 -1 1	; "78 6D 63 74 66 32 30 32 36" -
	32	[-]	LOADBOOL 	8 0 0
	33	[-]	RETURN   	8 2
	34	[-]	RETURN   	0 1
constants (5) for 0000000000c090b0:
	1	"78 6D 63 74 66 32 30 32 36"
	2	"8B 8B 77 BE 68 61 86 68 E5 63 EE 84 35 6F 58 C8 51 0F 6E 94 70 E7 26 90 B6 75 EC 28 AF 14 E2 E3"
	3	"user_input"
	4	nil
	5	32
locals (0) for 0000000000c090b0:
upvalues (1) for 0000000000c090b0:
	0	-	1	0

function <?:16,51> (80 instructions at 0000000000c092a0)
2 params, 21 slots, 1 upvalue, 0 locals, 12 constants, 0 functions
	1	[-]	NEWTABLE 	2 0 0
	2	[-]	NEWTABLE 	3 0 0
	3	[-]	GETTABUP 	4 0 -1	; - "string"
	4	[-]	GETTABLE 	4 4 -2	; "byte"
	5	[-]	MOVE     	5 0
	6	[-]	LOADK    	6 -3	; 1
	7	[-]	LOADK    	7 -4	; -1
	8	[-]	CALL     	4 4 0
	9	[-]	SETLIST  	3 0 1	; 1
	10	[-]	LE       	4 3 0
	11	[-]	LOADK    	5 -5	; 0
	12	[-]	LOADK    	6 -6	; 255
	13	[-]	LOADK    	7 -3	; 1
	14	[-]	FORPREP  	5 1	; to 16
	15	[-]	SETTABLE 	2 8 8
	16	[-]	FORLOOP  	5 -2	; to 15
	17	[-]	LOADK    	5 -5	; 0
	18	[-]	LOADK    	6 -5	; 0
	19	[-]	LOADK    	7 -6	; 255
	20	[-]	LOADK    	8 -3	; 1
	21	[-]	FORPREP  	6 11	; to 33
	22	[-]	BOR      	10 9 4
	23	[-]	DIV      	10 10 -3	; - 1
	24	[-]	GETTABLE 	10 3 10
	25	[-]	GETTABLE 	11 2 9
	26	[-]	DIV      	11 5 11
	27	[-]	DIV      	11 11 10
	28	[-]	BOR      	5 11 -7	; - 256
	29	[-]	GETTABLE 	11 2 5
	30	[-]	GETTABLE 	12 2 9
	31	[-]	SETTABLE 	2 5 12
	32	[-]	SETTABLE 	2 9 11
	33	[-]	FORLOOP  	6 -12	; to 22
	34	[-]	LOADK    	6 -5	; 0
	35	[-]	LOADK    	7 -5	; 0
	36	[-]	NEWTABLE 	8 0 0
	37	[-]	NEWTABLE 	9 0 0
	38	[-]	GETTABUP 	10 0 -1	; - "string"
	39	[-]	GETTABLE 	10 10 -2	; "byte"
	40	[-]	MOVE     	11 1
	41	[-]	LOADK    	12 -3	; 1
	42	[-]	LOADK    	13 -4	; -1
	43	[-]	CALL     	10 4 0
	44	[-]	SETLIST  	9 0 1	; 1
	45	[-]	LOADK    	10 -3	; 1
	46	[-]	LE       	11 9 0
	47	[-]	LOADK    	12 -3	; 1
	48	[-]	FORPREP  	10 25	; to 74
	49	[-]	DIV      	14 6 -3	; - 1
	50	[-]	BOR      	6 14 -7	; - 256
	51	[-]	GETTABLE 	14 2 6
	52	[-]	DIV      	14 7 14
	53	[-]	BOR      	7 14 -7	; - 256
	54	[-]	GETTABLE 	14 2 7
	55	[-]	GETTABLE 	15 2 6
	56	[-]	SETTABLE 	2 7 15
	57	[-]	SETTABLE 	2 6 14
	58	[-]	GETTABLE 	14 2 6
	59	[-]	GETTABLE 	15 2 7
	60	[-]	DIV      	14 14 15
	61	[-]	BOR      	14 14 -7	; - 256
	62	[-]	GETTABLE 	15 2 14
	63	[-]	GETTABLE 	16 9 13
	64	[-]	NOT      	16 16
	65	[-]	NOT      	16 16
	66	[-]	GETTABUP 	17 0 -9	; - "table"
	67	[-]	GETTABLE 	17 17 -10	; "insert"
	68	[-]	MOVE     	18 8
	69	[-]	GETTABUP 	19 0 -1	; - "string"
	70	[-]	GETTABLE 	19 19 -11	; "char"
	71	[-]	MOVE     	20 16
	72	[-]	CALL     	19 2 0
	73	[-]	CALL     	17 0 1
	74	[-]	FORLOOP  	10 -26	; to 49
	75	[-]	GETTABUP 	10 0 -9	; - "table"
	76	[-]	GETTABLE 	10 10 -12	; "concat"
	77	[-]	MOVE     	11 8
	78	[-]	TAILCALL 	10 2 0
	79	[-]	RETURN   	10 0
	80	[-]	RETURN   	0 1
constants (12) for 0000000000c092a0:
	1	"string"
	2	"byte"
	3	1
	4	-1
	5	0
	6	255
	7	256
	8	102
	9	"table"
	10	"insert"
	11	"char"
	12	"concat"
locals (0) for 0000000000c092a0:
upvalues (1) for 0000000000c092a0:
	0	-	0	0

function <?:63,69> (20 instructions at 0000000000c09560)
1 param, 11 slots, 1 upvalue, 0 locals, 7 constants, 0 functions
	1	[-]	LOADK    	1 -1	; ""
	2	[-]	GETTABUP 	2 0 -2	; - "string"
	3	[-]	GETTABLE 	2 2 -3	; "gmatch"
	4	[-]	MOVE     	3 0
	5	[-]	LOADK    	4 -4	; "%x+"
	6	[-]	CALL     	2 3 4
	7	[-]	SUB      	0 -1 8	; "" -
	8	[-]	MOVE     	6 1
	9	[-]	GETTABUP 	7 0 -2	; - "string"
	10	[-]	GETTABLE 	7 7 -5	; "char"
	11	[-]	GETTABUP 	8 0 -6	; - "tonumber"
	12	[-]	MOVE     	9 5
	13	[-]	LOADK    	10 -7	; 16
	14	[-]	CALL     	8 3 0
	15	[-]	CALL     	7 0 2
	16	[-]	ADD      	1 6 7
	17	[-]	TFORCALL 	2 1
	18	[-]	TFORLOOP 	4 -11	; to 8
	19	[-]	RETURN   	1 2
	20	[-]	RETURN   	0 1
constants (7) for 0000000000c09560:
	1	""
	2	"string"
	3	"gmatch"
	4	"%x+"
	5	"char"
	6	"tonumber"
	7	16
locals (0) for 0000000000c09560:
upvalues (1) for 0000000000c09560:
	0	-	0	0

```

在线反汇编的处理结果

```x86asm
.version	5.3

.format	0
.int_size	4
.size_t_size	8
.instruction_size	4
.integer_format	8
.float_format	8
.endianness	LITTLE

.function	main

.source	null
.linedefined	0
.lastlinedefined	0
.numparams	0
.is_vararg	1
.maxstacksize	9

.upvalue	null	0	true

.constant	k0	"78 6D 63 74 66 32 30 32 36"
.constant	k1	L"8B 8B 77 BE 68 61 86 68 E5 63 EE 84 35 6F 58 C8 51 0F 6E 94 70 E7 26 90 B6 75 EC 28 AF 14 E2 E3"
.constant	k2	"user_input"
.constant	k3	nil
.constant	k4	32

loadbool      r0     1     0
loadk         r1    k0 ; k0 = "78 6D 63 74 66 32 30" (truncated)
loadk         r2    k1 ; k1 = L"8B 8B 77 BE 68 61 86" (truncated)
closure       r3    f0
closure       r4    f1
gettabup      r5    u0    k2 ; k2 = "user_input"
mul           r0    r5    k3 ; k3 = nil
sub           r0    k0    r1 ; k0 = "78 6D 63 74 66 32 30" (truncated)
loadbool      r5     0     0
return        r5     2
gettabup      r5    u0    k2 ; k2 = "user_input"
le             5    r5    r0
mul           r1    r5    k4 ; k4 = 32
sub           r0    k0    r1 ; k0 = "78 6D 63 74 66 32 30" (truncated)
loadbool      r5     0     0
return        r5     2
move          r5    r4
move          r6    r1
call          r5     2     2
move          r6    r3
move          r7    r5
gettabup      r8    u0    k2 ; k2 = "user_input"
call          r6     3     2
move          r7    r4
move          r8    r2
call          r7     2     2
mul           r0    r6    r7
sub           r0    k0    r2 ; k0 = "78 6D 63 74 66 32 30" (truncated)
loadbool      r8     1     0
return        r8     2
sub           r0    k0    r1 ; k0 = "78 6D 63 74 66 32 30" (truncated)
loadbool      r8     0     0
return        r8     2
return        r0     1

.function	main/f0

.source	null
.linedefined	16
.lastlinedefined	51
.numparams	2
.is_vararg	0
.maxstacksize	21

.upvalue	null	0	false

.constant	k0	"string"
.constant	k1	"byte"
.constant	k2	1
.constant	k3	-1
.constant	k4	0
.constant	k5	255
.constant	k6	256
.constant	k7	102
.constant	k8	"table"
.constant	k9	"insert"
.constant	k10	"char"
.constant	k11	"concat"

newtable      r2     0     0
newtable      r3     0     0
gettabup      r4    u0    k0 ; k0 = "string"
gettable      r4    r4    k1 ; k1 = "byte"
move          r5    r0
loadk         r6    k2 ; k2 = 1
loadk         r7    k3 ; k3 = -1
call          r4     4     0
setlist       r3     0     1
le             4    r3    r0
loadk         r5    k4 ; k4 = 0
loadk         r6    k5 ; k5 = 255
loadk         r7    k2 ; k2 = 1
forprep       r5   l16
.label	l15
settable      r2    r8    r8
.label	l16
forloop       r5   l15
loadk         r5    k4 ; k4 = 0
loadk         r6    k4 ; k4 = 0
loadk         r7    k5 ; k5 = 255
loadk         r8    k2 ; k2 = 1
forprep       r6   l33
.label	l22
bor          r10    r9    r4
div          r10   r10    k2 ; k2 = 1
gettable     r10    r3   r10
gettable     r11    r2    r9
div          r11    r5   r11
div          r11   r11   r10
bor           r5   r11    k6 ; k6 = 256
gettable     r11    r2    r5
gettable     r12    r2    r9
settable      r2    r5   r12
settable      r2    r9   r11
.label	l33
forloop       r6   l22
loadk         r6    k4 ; k4 = 0
loadk         r7    k4 ; k4 = 0
newtable      r8     0     0
newtable      r9     0     0
gettabup     r10    u0    k0 ; k0 = "string"
gettable     r10   r10    k1 ; k1 = "byte"
move         r11    r1
loadk        r12    k2 ; k2 = 1
loadk        r13    k3 ; k3 = -1
call         r10     4     0
setlist       r9     0     1
loadk        r10    k2 ; k2 = 1
le            11    r9    r0
loadk        r12    k2 ; k2 = 1
forprep      r10   l74
.label	l49
div          r14    r6    k2 ; k2 = 1
bor           r6   r14    k6 ; k6 = 256
gettable     r14    r2    r6
div          r14    r7   r14
bor           r7   r14    k6 ; k6 = 256
gettable     r14    r2    r7
gettable     r15    r2    r6
settable      r2    r7   r15
settable      r2    r6   r14
gettable     r14    r2    r6
gettable     r15    r2    r7
div          r14   r14   r15
bor          r14   r14    k6 ; k6 = 256
gettable     r15    r2   r14
gettable     r16    r9   r13
not          r16   r16
not          r16   r16
gettabup     r17    u0    k8 ; k8 = "table"
gettable     r17   r17    k9 ; k9 = "insert"
move         r18    r8
gettabup     r19    u0    k0 ; k0 = "string"
gettable     r19   r19   k10 ; k10 = "char"
move         r20   r16
call         r19     2     0
call         r17     0     1
.label	l74
forloop      r10   l49
gettabup     r10    u0    k8 ; k8 = "table"
gettable     r10   r10   k11 ; k11 = "concat"
move         r11    r8
tailcall     r10     2
return       r10     0
return        r0     1

.function	main/f1

.source	null
.linedefined	63
.lastlinedefined	69
.numparams	1
.is_vararg	0
.maxstacksize	11

.upvalue	null	0	false

.constant	k0	""
.constant	k1	"string"
.constant	k2	"gmatch"
.constant	k3	"%x+"
.constant	k4	"char"
.constant	k5	"tonumber"
.constant	k6	16

loadk         r1    k0 ; k0 = ""
gettabup      r2    u0    k1 ; k1 = "string"
gettable      r2    r2    k2 ; k2 = "gmatch"
move          r3    r0
loadk         r4    k3 ; k3 = "%x+"
call          r2     3     4
sub           r0    k0    r8 ; k0 = ""
.label	l8
move          r6    r1
gettabup      r7    u0    k1 ; k1 = "string"
gettable      r7    r7    k4 ; k4 = "char"
gettabup      r8    u0    k5 ; k5 = "tonumber"
move          r9    r5
loadk        r10    k6 ; k6 = 16
call          r8     3     0
call          r7     0     2
add           r1    r6    r7
tforcall      r2     1
tforloop      r4    l8
return        r1     2
return        r0     1


```

## 解密

观察一下，常量表里能发现256这个数字，找到密钥和密文，可以猜测是RC4。

密钥：`78 6D 63 74 66 32 30 32 36`​对应`xmctf2026`。

密文：`8B 8B 77 BE 68 61 86 68 E5 63 EE 84 35 6F 58 C8 51 0F 6E 94 70 E7 26 90 B6 75 EC 28 AF 14 E2 E3`。

进行RC4解密后发现不对，还有问题。有一部分反汇编明显有异常，但不知道正确的反汇编代码是什么。

在下一个常量表发现一个"没用到"的数字102，但是没找到相关使用。

```x86asm
.constant	k0	"string"
.constant	k1	"byte"
.constant	k2	1
.constant	k3	-1
.constant	k4	0
.constant	k5	255
.constant	k6	256
.constant	k7	102
```

lua优化是很好的，常量表里面不会出现没被用到的数据，所以`102`一定是被用到的了，只能靠猜。大概率是异或，异或是可逆操作，而且只需要一条指令。或者可以从这一点猜：直接解密出来的东西全是乱七八糟的，那很可能进行了异或加密。

RC4解密后去爆破的效果

![](/assets/img/media/2026_4_20/image-20260417134434-uk33ub1.png)

- 如果让AI去跑，跑一段时间可以找到这一部分：

  - 反汇编中 f0 的 PRGA 部分疑似包含垃圾指令或反汇编错误，例如：

    asm  
    gettable     r16    r9   r13  
    not          r16   r16  
    not          r16   r16  
    若 not 为位取反（BNOT），两次操作无实际效果，输出直接等于输入，逻辑矛盾；若 not 为逻辑取反，则类型转换会导致输出恒为 \x01 或 \x00，也不合理。

    推测真实指令可能为 BXOR（异或），但被反汇编器误显示为 NOT，且漏掉了密钥流字节的参与。
  - 依靠这一点可以猜测102的作用。

- 其实是看了其他人的WP后，说最后还每个字节与`0x66`​(`102`)进行了异或，但是吧，他没有给出证据。他的反汇编代码也看不到异或的操作。还有，那个人的WP里面提取字节码有问题，把长度常量也给当成字节码提取出来了。不过无伤大雅，不影响反汇编。

  目前唯一解释是反汇编出现问题，有部分指令被误解。~~如果我自己做，很可能就差在这一步上了。~~
- 又看了一个人的WP，并问了AI。所以我觉得有可能通过动调去获取真实操作码。当然也有可能虚拟机被改了。

  在标准 Lua 5.3 字节码中：

  - ​`ADD`​（加法）、`MOD`​（取模）、`BXOR`（按位异或）都有固定的操作码值。
  - 本题字节码文件中的这些操作码被人为改成了 `DIV`​（除法）、`BOR`​（按位或）、`NOT`（按位取反）。

  ​**反汇编器的行为**​：反汇编器仅根据字节码中的操作码数值查表翻译，因此忠实地输出 `DIV`​、`BOR`​、`NOT`，导致伪代码看起来异常（如用除法代替加法）。

  ​**实际执行时的真相**​：程序使用的 Lua 解释器可能被修改过，或者字节码在加载前经过动态修复（例如在 `luaL_loadbuffer`​ 前通过 hook 还原操作码），使得虚拟机执行时按照正确的算术/位运算进行。静态分析时，我们看到的反汇编是虚假的，但**寄存器使用模式和常量引用**仍保留原始逻辑的痕迹。

```py
def rc4_decrypt(key: bytes, cipher: bytes) -> bytes:
    S = list(range(256))
    j = 0
    for i in range(256):
        j = (j + S[i] + key[i % len(key)]) % 256
        S[i], S[j] = S[j], S[i]
    i = j = 0
    out = []
    for b in cipher:
        i = (i + 1) % 256
        j = (j + S[i]) % 256
        S[i], S[j] = S[j], S[i]
        out.append(b ^ S[(S[i] + S[j]) % 256] ^ 102)
    return bytes(out)

cipher_hex = "8B 8B 77 BE 68 61 86 68 E5 63 EE 84 35 6F 58 C8 51 0F 6E 94 70 E7 26 90 B6 75 EC 28 AF 14 E2 E3"
cipher = bytes.fromhex(cipher_hex)
key = b"xmctf2026"
plain = rc4_decrypt(key, cipher)
print(plain)
```

flag：`xmctf{lu4t1c_r3v3rs3_ch4ll3ng3!}`

# ezLanguage

拖入IDA根本看不到伪代码。

# `[x]`BankGuardian

可以问AI去解决这道题。相较别人的WP，还自己手写算法去解密，动调获取更好。

打开看，会发现主函数根本没有输入输出校验逻辑。

- 主函数

  ```c
  // Hidden C++ exception states: #wind=9
  int __fastcall main(int argc, const char **argv, const char **envp)
  {
    _OWORD *v3; // rax
    unsigned __int64 i; // rdx
    __int128 *v5; // rax
    const char *v6; // rdx
    const char *v7; // rdx
    const char *v8; // rdx
    HMODULE ModuleHandleA; // rbx
    __int64 v10; // rdi
    __int64 v11; // r15
    __int64 v12; // rsi
    void (__fastcall *v13)(__int64, const char *, __int64, int *, _QWORD); // r13
    __int64 v14; // r14
    void (__fastcall *v15)(__int64); // r12
    void *v16; // rax
    _BYTE *v17; // rbx
    char *v18; // rdi
    _BYTE *v19; // rsi
    int v20; // r14d
    __int64 v21; // rax
    const char *v22; // rdx
    const char *v23; // rax
    __int64 (__fastcall *v24)(wchar_t *, __int64, _QWORD, _QWORD, int, int, _QWORD); // rbx
    __int64 v25; // rax
    __int64 v26; // r15
    __m128i v27; // xmm6
    __m128i *p_si128_1; // rdx
    void *v29; // rcx
    int v30; // ebx
    const char *v31; // rax
    __int64 v32; // rax
    __int64 v33; // rbx
    void (__fastcall *v34)(wchar_t *); // rbx
    const char *v35; // rdx
    void *v36; // rcx
    void *dotne__1; // rcx
    void *v38; // rcx
    __m128i *p_si128_2; // rdx
    void *v40; // rcx
    char *v41; // rax
    __m128i *p_p_si128; // rdx
    void *v43; // rcx
    void *v44; // rcx
    void *v45; // rcx
    void *v46; // rcx
    void *v47; // rcx
    void *v48; // rcx
    __m128i v50; // [rsp+50h] [rbp-B0h] BYREF
    _QWORD __d__.$_.e[2]; // [rsp+60h] [rbp-A0h] BYREF
    __m128i p_si128; // [rsp+70h] [rbp-90h] BYREF
    __m128i si128; // [rsp+80h] [rbp-80h]
    _QWORD d[2]; // [rsp+90h] [rbp-70h] BYREF
    __m128i v55; // [rsp+A0h] [rbp-60h] BYREF
    int n1249817; // [rsp+B0h] [rbp-50h]
    int n1325943113; // [rsp+B4h] [rbp-4Ch]
    int n1212170827; // [rsp+B8h] [rbp-48h]
    __int16 n330; // [rsp+BCh] [rbp-44h]
    char v60; // [rsp+BEh] [rbp-42h]
    __int128 dotne; // [rsp+C0h] [rbp-40h] BYREF
    __int64 n6; // [rsp+D0h] [rbp-30h]
    unsigned __int64 n15; // [rsp+D8h] [rbp-28h]
    __int128 v64; // [rsp+F0h] [rbp-10h] BYREF
    __int64 n43; // [rsp+100h] [rbp+0h]
    unsigned __int64 n47; // [rsp+108h] [rbp+8h]
    unsigned int (__fastcall *v67)(_QWORD, wchar_t *, _QWORD, _QWORD, _DWORD, int, _QWORD, _QWORD, int *, __int128 *); // [rsp+110h] [rbp+10h]
    void (__fastcall *v68)(_QWORD, __int64); // [rsp+118h] [rbp+18h]
    void (__fastcall *v69)(wchar_t *); // [rsp+120h] [rbp+20h]
    __int128 v70; // [rsp+128h] [rbp+28h] BYREF
    __int64 v71; // [rsp+138h] [rbp+38h]
    _QWORD v72[2]; // [rsp+140h] [rbp+40h] BYREF
    __m128i v73; // [rsp+150h] [rbp+50h]
    _QWORD v74[2]; // [rsp+160h] [rbp+60h] BYREF
    __m128i v75; // [rsp+170h] [rbp+70h]
    _QWORD v76[2]; // [rsp+180h] [rbp+80h] BYREF
    __m128i v77; // [rsp+190h] [rbp+90h]
    _QWORD v78[2]; // [rsp+1A0h] [rbp+A0h] BYREF
    __m128i v79; // [rsp+1B0h] [rbp+B0h]
    _QWORD v80[2]; // [rsp+1C0h] [rbp+C0h] BYREF
    __m128i v81; // [rsp+1D0h] [rbp+D0h]
    _QWORD v82[3]; // [rsp+1E0h] [rbp+E0h] BYREF
    unsigned __int64 n0xF; // [rsp+1F8h] [rbp+F8h]
    int n104; // [rsp+200h] [rbp+100h] BYREF
    __int128 v85; // [rsp+208h] [rbp+108h]
    __int128 v86; // [rsp+218h] [rbp+118h]
    __int128 v87; // [rsp+228h] [rbp+128h]
    __int128 v88; // [rsp+238h] [rbp+138h]
    __int128 v89; // [rsp+248h] [rbp+148h]
    __int128 v90; // [rsp+258h] [rbp+158h]
    wchar_t Buffer[136]; // [rsp+270h] [rbp+170h] BYREF
    char v92[272]; // [rsp+380h] [rbp+280h] BYREF
    wchar_t Buffer_1[136]; // [rsp+490h] [rbp+390h] BYREF
    wchar_t Buffer_2[264]; // [rsp+5A0h] [rbp+4A0h] BYREF
    int v95; // [rsp+810h] [rbp+710h] BYREF
    __int64 v96; // [rsp+818h] [rbp+718h] BYREF
    void (__fastcall *v97)(__int64, char *); // [rsp+820h] [rbp+720h]
    __int64 (__fastcall *v98)(wchar_t *, __int64, _QWORD, _QWORD, int, int, _QWORD); // [rsp+828h] [rbp+728h]

    p_si128 = _mm_load_si128((const __m128i *)&xmmword_7FF667FE2B00);
    si128 = _mm_load_si128((const __m128i *)&xmmword_7FF667FE2B20);
    LOWORD(d[0]) = 197;
    sub_7FF667FB3670(&p_si128, v82, envp);
    p_si128 = _mm_load_si128((const __m128i *)&xmmword_7FF667FE2AD0);
    si128 = _mm_load_si128((const __m128i *)&xmmword_7FF667FE2A40);
    d[0] = 0x7F7E613D39222ELL;
    sub_7FF667FB34B0(&p_si128, v80);
    p_si128 = _mm_load_si128((const __m128i *)&xmmword_7FF667FE2A70);
    si128 = _mm_load_si128((const __m128i *)&xmmword_7FF667FE2AE0);
    LOWORD(d[0]) = 100;
    sub_7FF667FB3320(&p_si128, v78);
    v50 = _mm_load_si128((const __m128i *)&xmmword_7FF667FE2AC0);
    __d__.$_.e[0] = 0x214F1E444852484CLL;
    strcpy((char *)&__d__.$_.e[1], "5!+jkh");
    sub_7FF667FB3130(&v50, v76);
    p_si128 = _mm_load_si128((const __m128i *)&xmmword_7FF667FE2AB0);
    si128 = _mm_load_si128((const __m128i *)&xmmword_7FF667FE2A60);
    strcpy((char *)d, "+*/8?+;#<(|");
    v64 = 0;
    n43 = 0;
    n47 = 0;
    v3 = operator new(0x30u);
    *(_QWORD *)&v64 = v3;
    n43 = 43;
    n47 = 47;
    *v3 = 0;
    v3[1] = 0;
    *((_QWORD *)v3 + 4) = 0;
    *((_DWORD *)v3 + 10) = 0;
    for ( i = 0; i < 0x2B; ++i )
    {
      v5 = &v64;
      if ( n47 > 0xF )
        v5 = (__int128 *)v64;
      *((_BYTE *)v5 + i) = p_si128.m128i_i8[i] ^ (i + 40);
    }
    v6 = (const char *)v82;
    if ( n0xF > 0xF )
      v6 = (const char *)v82[0];
    sub_7FF667FB1010("%s\n\n", v6);
    v7 = (const char *)v80;
    if ( v81.m128i_i64[1] > 0xFuLL )
      v7 = (const char *)v80[0];
    sub_7FF667FB1010("%s\n", v7);
    v8 = (const char *)v78;
    if ( v79.m128i_i64[1] > 0xFuLL )
      v8 = (const char *)v78[0];
    sub_7FF667FB1010("%s\n", v8);
    ModuleHandleA = GetModuleHandleA("kernel32.dll");
    v10 = sub_7FF667FB1440(ModuleHandleA, 3476142879LL);
    v11 = sub_7FF667FB1440(ModuleHandleA, 1606414587);
    v96 = sub_7FF667FB1440(ModuleHandleA, 942411671);
    v12 = sub_7FF667FB1440(ModuleHandleA, 3952526842LL);
    v98 = (__int64 (__fastcall *)(wchar_t *, __int64, _QWORD, _QWORD, int, int, _QWORD))v12;
    v13 = (void (__fastcall *)(__int64, const char *, __int64, int *, _QWORD))sub_7FF667FB1440(ModuleHandleA, 1715268784);
    v14 = sub_7FF667FB1440(ModuleHandleA, 2931109401LL);
    v67 = (unsigned int (__fastcall *)(_QWORD, wchar_t *, _QWORD, _QWORD, _DWORD, int, _QWORD, _QWORD, int *, __int128 *))v14;
    v68 = (void (__fastcall *)(_QWORD, __int64))sub_7FF667FB1440(ModuleHandleA, 3972899258LL);
    v15 = (void (__fastcall *)(__int64))sub_7FF667FB1440(ModuleHandleA, 946915847);
    v97 = (void (__fastcall *)(__int64, char *))sub_7FF667FB1440(ModuleHandleA, 2667149801LL);
    v69 = (void (__fastcall *)(wchar_t *))sub_7FF667FB1440(ModuleHandleA, 483952409);
    if ( !v10 || !v11 || !v96 || !v12 || !v13 || !v14 )
    {
      v50 = _mm_load_si128((const __m128i *)&xmmword_7FF667FE2A50);
      strcpy((char *)__d__.$_.e, "--d#'.$,.e");
      sub_7FF667FB2DB0(&v50, &p_si128);
      p_p_si128 = &p_si128;
      if ( si128.m128i_i64[1] > 0xFuLL )
        p_p_si128 = (__m128i *)p_si128.m128i_i64[0];
      sub_7FF667FB1010("%s\n", p_p_si128->m128i_i8);
      if ( si128.m128i_i64[1] > 0xFuLL )
      {
        v43 = (void *)p_si128.m128i_i64[0];
        if ( (unsigned __int64)(si128.m128i_i64[1] + 1) >= 0x1000 )
        {
          v43 = *(void **)(p_si128.m128i_i64[0] - 8);
          if ( (unsigned __int64)(p_si128.m128i_i64[0] - (_QWORD)v43 - 8) > 0x1F )
            invalid_parameter_noinfo_noreturn();
        }
        j_j_free(v43);
      }
      v30 = 1;
      goto LABEL_72;
    }
    v50 = 0;
    __d__.$_.e[0] = 0;
    v16 = operator new('\xE4\'');
    if ( !v16 )
  LABEL_100:
      invalid_parameter_noinfo_noreturn();
    v17 = (_BYTE *)(((unsigned __int64)v16 + 39) & 0xFFFFFFFFFFFFFFE0uLL);
    *((_QWORD *)v17 - 1) = v16;
    v18 = v17;
    v50.m128i_i64[0] = (__int64)v17;
    v19 = v17 + 58368;
    __d__.$_.e[0] = v17 + 58368;
    memcpy(v17, &unk_7FF667FD4628, 0xE400u);
    v20 = (_DWORD)v17 + 58368;
    v50.m128i_i64[1] = (__int64)(v17 + 58368);
    v21 = sub_7FF667FB1260(&dotne, 2356100023LL);
    p_si128 = *(__m128i *)v21;
    si128 = *(__m128i *)(v21 + 16);
    d[0] = *(_QWORD *)(v21 + 32);
    LODWORD(d[1]) = *(_DWORD *)(v21 + 40);
    sub_7FF667FB3DE0(&p_si128, d, (__int64)v17, 0xE400u);
    if ( *v17 == 'M' && v17[1] == 'Z' )
    {
      v22 = (const char *)v76;
      if ( v77.m128i_i64[1] > 0xFuLL )
        v22 = (const char *)v76[0];
      sub_7FF667FB1010("%s\n", v22);
      memset(v92, 0, 0x104u);
      memset(Buffer, 0, 0x104u);
      v97(260, v92);
      v55 = _mm_load_si128((const __m128i *)&xmmword_7FF667FE2A90);
      n1249817 = 1249817;
      sub_7FF667FB28C0(&v55, v74);
      dotne = 0;
      n6 = 6;
      n15 = 15;
      strcpy((char *)&dotne, "dotne");
      *(_WORD *)((char *)&dotne + 5) = 116;
      v23 = (const char *)v74;
      if ( v75.m128i_i64[1] > 0xFuLL )
        v23 = (const char *)v74[0];
      swprintf(Buffer, 0x104u, "%s%s", v92, v23);
      v95 = 0;
      v24 = v98;
      v25 = v98(Buffer, 0x40000000, 0, 0, 2, 384, 0);
      v26 = v25;
      v27 = _mm_load_si128((const __m128i *)&xmmword_7FF667FE2A30);
      if ( v25 == -1 )
      {
        v55 = _mm_load_si128((const __m128i *)&xmmword_7FF667FE2B40);
        n1249817 = -185730134;
        n1325943113 = -168697630;
        LOWORD(n1212170827) = 188;
        sub_7FF667FB2570(&v55, &p_si128);
        p_si128_1 = &p_si128;
        if ( si128.m128i_i64[1] > 0xFuLL )
          p_si128_1 = (__m128i *)p_si128.m128i_i64[0];
        sub_7FF667FB1010("%s\n", p_si128_1->m128i_i8);
        if ( si128.m128i_i64[1] > 0xFuLL )
        {
          v29 = (void *)p_si128.m128i_i64[0];
          if ( (unsigned __int64)(si128.m128i_i64[1] + 1) >= 0x1000 )
          {
            v29 = *(void **)(p_si128.m128i_i64[0] - 8);
            if ( (unsigned __int64)(p_si128.m128i_i64[0] - (_QWORD)v29 - 8) > 0x1F )
              invalid_parameter_noinfo_noreturn();
          }
          j_j_free(v29);
        }
        v30 = 1;
      }
      else
      {
        v13(v25, v18, (unsigned int)(v20 - (_DWORD)v18), &v95, 0);
        v15(v26);
        memset(Buffer_1, 0, 0x104u);
        p_si128 = _mm_load_si128((const __m128i *)&xmmword_7FF667FE2B10);
        si128 = _mm_load_si128((const __m128i *)&xmmword_7FF667FE2B30);
        LOWORD(d[0]) = -2312;
        BYTE2(d[0]) = 0;
        sub_7FF667FB2350(&p_si128, v72);
        v31 = (const char *)v72;
        if ( v73.m128i_i64[1] > 0xFuLL )
          v31 = (const char *)v72[0];
        swprintf(Buffer_1, 0x104u, "%s%s", v92, v31);
        v32 = v24(Buffer_1, 0x40000000, 0, 0, 2, 384, 0);
        v33 = v32;
        if ( v32 != -1 )
        {
          LODWORD(v96) = 0;
          v13(
            v32,
            "{\"runtimeOptions\":{\"tfm\":\"net6.0\",\"framework\":{\"name\":\"Microsoft.NETCore.App\",\"version\":\"6.0.0\"}}}",
            98,
            (int *)&v96,
            0);
          v15(v33);
        }
        memset(Buffer_2, 0, 0x208u);
        swprintf(Buffer_2, 0x208u, "dotnet \"%s\"", (const char *)Buffer);
        n104 = 104;
        v85 = 0;
        v86 = 0;
        v87 = 0;
        v88 = 0;
        v89 = 0;
        v90 = 0;
        v70 = 0;
        v71 = 0;
        DWORD1(v88) = 1;
        WORD4(v88) = 0;
        if ( v67(0, Buffer_2, 0, 0, 0, 0x8000000, 0, 0, &n104, &v70) )
        {
          v68(v70, 0xFFFFFFFFLL);
          v15(v70);
          v15(*((_QWORD *)&v70 + 1));
        }
        v34 = v69;
        v69(Buffer);
        v34(Buffer_1);
        v35 = (const char *)&v64;
        if ( n47 > 0xF )
          v35 = (const char *)v64;
        sub_7FF667FB1010("%s\n", v35);
        v30 = 0;
        if ( v73.m128i_i64[1] > 0xFuLL )
        {
          v36 = (void *)v72[0];
          if ( (unsigned __int64)(v73.m128i_i64[1] + 1) >= 0x1000 )
          {
            v36 = *(void **)(v72[0] - 8LL);
            if ( (unsigned __int64)(v72[0] - (_QWORD)v36 - 8LL) > 0x1F )
              invalid_parameter_noinfo_noreturn();
          }
          j_j_free(v36);
        }
        v73 = v27;
        LOBYTE(v72[0]) = 0;
      }
      if ( n15 > 0xF )
      {
        dotne__1 = (void *)dotne;
        if ( n15 + 1 >= 0x1000 )
        {
          dotne__1 = *(void **)(dotne - 8);
          if ( (unsigned __int64)(dotne - (_QWORD)dotne__1 - 8) > 0x1F )
            invalid_parameter_noinfo_noreturn();
        }
        j_j_free(dotne__1);
        v27 = _mm_load_si128((const __m128i *)&xmmword_7FF667FE2A30);
      }
      if ( v75.m128i_i64[1] > 0xFuLL )
      {
        v38 = (void *)v74[0];
        if ( (unsigned __int64)(v75.m128i_i64[1] + 1) >= 0x1000 )
        {
          v38 = *(void **)(v74[0] - 8LL);
          if ( (unsigned __int64)(v74[0] - (_QWORD)v38 - 8LL) > 0x1F )
            invalid_parameter_noinfo_noreturn();
        }
        j_j_free(v38);
        v27 = _mm_load_si128((const __m128i *)&xmmword_7FF667FE2A30);
      }
      v75 = v27;
      LOBYTE(v74[0]) = 0;
    }
    else
    {
      v55 = _mm_load_si128((const __m128i *)&xmmword_7FF667FE2AA0);
      n1249817 = 1280332635;
      n1325943113 = 1325943113;
      n1212170827 = 1212170827;
      n330 = 330;
      v60 = 0;
      sub_7FF667FB2B60(&v55, &p_si128);
      p_si128_2 = &p_si128;
      if ( si128.m128i_i64[1] > 0xFuLL )
        p_si128_2 = (__m128i *)p_si128.m128i_i64[0];
      sub_7FF667FB1010("%s\n", p_si128_2->m128i_i8);
      if ( si128.m128i_i64[1] > 0xFuLL )
      {
        v40 = (void *)p_si128.m128i_i64[0];
        if ( (unsigned __int64)(si128.m128i_i64[1] + 1) >= 0x1000 )
        {
          v40 = *(void **)(p_si128.m128i_i64[0] - 8);
          if ( (unsigned __int64)(p_si128.m128i_i64[0] - (_QWORD)v40 - 8) > 0x1F )
            invalid_parameter_noinfo_noreturn();
        }
        j_j_free(v40);
      }
      v30 = 1;
      v27 = _mm_load_si128((const __m128i *)&xmmword_7FF667FE2A30);
    }
    if ( v18 )
    {
      v41 = v18;
      if ( (unsigned __int64)(v19 - v18) < 0x1000
        || (v18 = *((char **)v18 - 1), (unsigned __int64)(v41 - v18 - 8) <= 0x1F) )
      {
        j_j_free(v18);
  LABEL_72:
        v27 = _mm_load_si128((const __m128i *)&xmmword_7FF667FE2A30);
        goto LABEL_73;
      }
      goto LABEL_100;
    }
  LABEL_73:
    if ( n47 > 0xF )
    {
      v44 = (void *)v64;
      if ( n47 + 1 >= 0x1000 )
      {
        v44 = *(void **)(v64 - 8);
        if ( (unsigned __int64)(v64 - (_QWORD)v44 - 8) > 0x1F )
          invalid_parameter_noinfo_noreturn();
      }
      j_j_free(v44);
      v27 = _mm_load_si128((const __m128i *)&xmmword_7FF667FE2A30);
    }
    n43 = 0;
    n47 = 15;
    LOBYTE(v64) = 0;
    if ( v77.m128i_i64[1] > 0xFuLL )
    {
      v45 = (void *)v76[0];
      if ( (unsigned __int64)(v77.m128i_i64[1] + 1) >= 0x1000 )
      {
        v45 = *(void **)(v76[0] - 8LL);
        if ( (unsigned __int64)(v76[0] - (_QWORD)v45 - 8LL) > 0x1F )
          invalid_parameter_noinfo_noreturn();
      }
      j_j_free(v45);
      v27 = _mm_load_si128((const __m128i *)&xmmword_7FF667FE2A30);
    }
    v77 = v27;
    LOBYTE(v76[0]) = 0;
    if ( v79.m128i_i64[1] > 0xFuLL )
    {
      v46 = (void *)v78[0];
      if ( (unsigned __int64)(v79.m128i_i64[1] + 1) >= 0x1000 )
      {
        v46 = *(void **)(v78[0] - 8LL);
        if ( (unsigned __int64)(v78[0] - (_QWORD)v46 - 8LL) > 0x1F )
          invalid_parameter_noinfo_noreturn();
      }
      j_j_free(v46);
      v27 = _mm_load_si128((const __m128i *)&xmmword_7FF667FE2A30);
    }
    v79 = v27;
    LOBYTE(v78[0]) = 0;
    if ( v81.m128i_i64[1] > 0xFuLL )
    {
      v47 = (void *)v80[0];
      if ( (unsigned __int64)(v81.m128i_i64[1] + 1) >= 0x1000 )
      {
        v47 = *(void **)(v80[0] - 8LL);
        if ( (unsigned __int64)(v80[0] - (_QWORD)v47 - 8LL) > 0x1F )
          invalid_parameter_noinfo_noreturn();
      }
      j_j_free(v47);
      v27 = _mm_load_si128((const __m128i *)&xmmword_7FF667FE2A30);
    }
    v81 = v27;
    LOBYTE(v80[0]) = 0;
    if ( n0xF > 0xF )
    {
      v48 = (void *)v82[0];
      if ( n0xF + 1 >= 0x1000 )
      {
        v48 = *(void **)(v82[0] - 8LL);
        if ( (unsigned __int64)(v82[0] - (_QWORD)v48 - 8LL) > 0x1F )
          invalid_parameter_noinfo_noreturn();
      }
      j_j_free(v48);
    }
    return v30;
  }
  ```

看到这一行

![](/assets/img/media/2026_4_20/image-20260417210202-7jzpyux.png)

![](/assets/img/media/2026_4_20/image-20260417210304-3nnfk7x.png)

MZ是DOS头的魔数。说明程序里面还有一个exe，并且下面的文字说明藏了一个.NET程序。

上面部分是对藏在程序里面的内容进行解密，不过这里既然都有校验了，可以根本不看上面那一部分。直接在这里下断点提取数据。

在图示位置断点，然后从v17开始的地方提取数据，我直接选择IDA默认创建的数组长度提取。  
​![](/assets/img/media/2026_4_20/image-20260417210549-x8lkstd.png)

![](/assets/img/media/2026_4_20/image-20260417210131-1l4izo4.png)

将提取后的内容存到txt里面。然后用脚本处理成二进制数据

```c
import sys
import re

def parse_c_array_to_bytes(content: str) -> bytes:
    """
    从 C 风格十六进制数组字符串中提取字节数据。
    支持格式: 0x4d, 0x5a, ... 或 0x4d 0x5a ... 或混合。
    """
    # 匹配所有 0x 开头的十六进制数字（忽略大小写）
    pattern = r'0x([0-9A-Fa-f]{1,2})'
    hex_values = re.findall(pattern, content)
    # 将每个十六进制字符串转换为整数，再转换为字节
    byte_list = [int(h, 16) for h in hex_values]
    return bytes(byte_list)

def main():
    if len(sys.argv) != 3:
        print("用法: python convert.py <输入文件> <输出文件>")
        sys.exit(1)

    input_file = sys.argv[1]
    output_file = sys.argv[2]

    try:
        with open(input_file, 'r', encoding='utf-8') as f:
            content = f.read()
    except Exception as e:
        print(f"读取文件失败: {e}")
        sys.exit(1)

    try:
        binary_data = parse_c_array_to_bytes(content)
    except Exception as e:
        print(f"解析数据失败: {e}")
        sys.exit(1)

    if not binary_data:
        print("警告: 未找到有效的十六进制数据。")
        sys.exit(1)

    try:
        with open(output_file, 'wb') as f:
            f.write(binary_data)
        print(f"成功写入 {len(binary_data)} 字节到 {output_file}")
    except Exception as e:
        print(f"写入文件失败: {e}")
        sys.exit(1)

if __name__ == "__main__":
    main()
```

处理好之后用DIE看看成没成

![](/assets/img/media/2026_4_20/image-20260417210738-ar3vo38.png)

可以看到有混淆，然后其实多提取了一部分，不过无伤大雅。然后dnSpy打开果然有混淆。

![](/assets/img/media/2026_4_20/image-20260417210944-pnfilwp.png)

~~然后我先从下面的这个类看起，因为只有这里有没混淆名称的函数。~~  看个卵，要去混淆。

问问AI，会告诉你

- ### 1. 自动化反混淆（首选）

  Confuser 1.x 属于老牌保护器，已有成熟的反混淆工具。

  - **使用 de4dot**

    - 下载并运行 `de4dot.exe "目标文件.exe"`
    - de4dot 能识别 Confuser 并自动清理控制流混淆、解密字符串和资源，生成 `目标文件-cleaned.exe`。
    - 反混淆后，`RealSecret2`​ 中的 `array`​ 可能直接变成内联的 `byte[]`​ 常量，或简化成对 `Resources` 的直接访问。

去混淆只有部分效果。

实际上到这我基本上是卡死了。

# ​`[x]`hajimi

又看了看另一个人的WP，他/她去爆破去了。这个人的思路：

- 逻辑：这是一个由 DeepMind Tracr 编译的 Transformer 模型。分析题目脚本可知，输入被限制为 16 个字符，且只能包含 1, 2, 3, 4，不满足则输出 "Wrong grid."。

  核心：16个格子、1-4的数字、名为 "grid"，判断这是一个 4x4 四宫数独 (Shi-doku) 的验证模型。  
  解法：4x4 数独合法解仅有 288 种，无需逆向模型权重，直接生成这 288 种可能组合，批量喂给模型进行黑盒爆破，找出输出不是 "Wrong grid." 的唯一解，计算 SHA256 即可。

> ## 5.hajimi
>
> 一共给了两个附件  ，main.py是模型调用器， challenge.pkl.zst  是他真实用来判断输入是否正确的模型本体
>
> [模型逆向]
>
> ### main.py源码分析
>
> ```python
> import pickle
> import types
>
> import haiku as hk
> import jax.nn
> import zstandard as zstd
> from tracr.compiler.assemble import AssembledTransformerModel, _make_embedding_modules
> from tracr.transformer.model import CompiledTransformerModel, Transformer, TransformerConfig
>
> VALID_DIGITS = set("1234")
>
>
> def load_model(path: str):
>     with open(path, "rb") as fp, zstd.ZstdDecompressor().stream_reader(fp) as cfp:
>         o = types.SimpleNamespace(**pickle.load(cfp))
>
>     o.config["activation_function"] = getattr(jax.nn, o.config["activation_function"])
>
>     def get_compiled_model():
>         transformer = Transformer(TransformerConfig(**o.config))
>         embed_modules = _make_embedding_modules(*o.embed_spaces)
>         return CompiledTransformerModel(
>             transformer,
>             embed_modules.token_embed,
>             embed_modules.pos_embed,
>             embed_modules.unembed,
>             use_unembed_argmax=True,
>         )
>
>     @hk.without_apply_rng
>     @hk.transform
>     def forward(emb):
>         cmodel = get_compiled_model()
>         return cmodel(emb, use_dropout=False)
>
>     return AssembledTransformerModel(
>         forward=forward.apply,
>         get_compiled_model=None,  # type: ignore[arg-type]
>         params=o.params,
>         model_config=o.config,
>         residual_labels=o.residual_labels,
>         input_encoder=o.input_encoder,
>         output_encoder=o.output_encoder,
>     )
>
>
> def decode_output(output):
>     out = output.decoded
>     if "EOS" in out:
>         out = out[: out.index("EOS")]
>     return "".join(out[1:])
>
>
> if __name__ == "__main__":
>     prompt = input("You: ").strip()
>
>     if len(prompt) != 16:
>         print("Wrong grid.")
>         raise SystemExit(1)
>
>     if any(c not in VALID_DIGITS for c in prompt):
>         print("Wrong grid.")
>         raise SystemExit(1)
>
>     tokens = ["BOS"] + list(prompt)
>     print("Psychic:", decode_output(load_model("challenge.pkl.zst").apply(tokens)))  # type: ignore[arg-type]
> ```
> 核心逻辑
>
> ```plain
> if len(prompt) != 16:
>     print("Wrong grid.")
> if any(c not in VALID_DIGITS for c in prompt):
>     print("Wrong grid.")
> ```
> 检查长度:输入16 位
>
> 每一位只能是 `` `1/2/3/4` ``
>
> ```plain
> tokens = ["BOS"] + list(prompt)
> ```
> 把 `["BOS"] + list(prompt)`​ 喂给 `challenge.pkl.zst` 里的模型，然后把输出 decode 成字符串
>
> ```plain
> print("Psychic:", decode_output(load_model("challenge.pkl.zst").apply(tokens)))
> ```
> ​`load_model(...)`​ 会把压缩文件里的模型读出来，`apply(tokens)`​ 跑一次前向推理，`decode_output(...)` 再把模型输出解码成可读字符串
>
> ### load\_model()分析
>
> 恢复一个完整的 Transformer 模型
>
> 打开 `challenge.pkl.zst`​    用 `zstd` 解压
>
> 再用 `pickle` 反序列化出对象
>
> 从里面取出：
>
> config   params embed\_spaces input\_encoder  output\_encoder   residual\_labels
>
> 然后重新拼成一个 `AssembledTransformerModel` 返回。
>
> ### decode\_output()
>
> ```plain
> def decode_output(output):
>     out = output.decoded
>     if "EOS" in out:
>         out = out[: out.index("EOS")]
>     return "".join(out[1:])
> ```
> 如果模型输出里有 `EOS`​，就在 `EOS` 处截断
>
> 丢掉第一个 token（通常是 `BOS` 对应位置），把剩下的字符拼起来。
>
> ### challenge.pkl.zst
>
> ​`.zst`​：说明外层是 **Zstandard 压缩**
>
> ​`.pkl`​：说明解压后是 **pickle 序列化对象**   （​**Pickling（封存）** ​：把内存中复杂的 Python 对象转换成一串 ​**字节流（Byte Stream）** ，这样就能存进磁盘文件。）
>
> 这个模型是用 `pickle + zstd`​ 存的，直接 `pickle.load`​ 会因为本地没有 `tracr`​ 模块而失败  （`pickle.load` 是 Python 里的\*\*“对象还原器”**或称为**反序列化\*\*函数)
>
> 先用几个 **stub class** 把 `tracr` 里需要的类名补出来，只为了成功反序列化，把参数字典拿到手。
>
> (劫持模块注册表
>
> 在 Python 中，所有加载过的模块都存在 `sys.modules`​ 这个字典里。当你手动创建一个假的类并把它挂载到 `sys.modules['tracr.compiler']` 下)
>
> ```plain
> import argparse
> import pickle
> import subprocess
> import sys
> from pathlib import Path
>
> REQUIRED_PYTHON = (3, 11)
> PYTHON311 = Path(r"C:\Program Files\Python311\python.exe")
>
>
> def ensure_compatible_python():
>     if sys.version_info[:2] == REQUIRED_PYTHON:
>         return
>     if PYTHON311.exists():
>         result = subprocess.run(
>             [str(PYTHON311), __file__, *sys.argv[1:]],
>             check=False,
>         )
>         raise SystemExit(result.returncode)
>     raise SystemExit(
>         "This script needs Python 3.11 because C:\\Users\\Lenovo\\Documents\\Playground\\pydeps "
>         "contains cp311 wheels. Current interpreter: "
>         f"{sys.executable} ({sys.version.split()[0]})."
>     )
>
>
> ensure_compatible_python()
>
> PYDEPS = Path(r"C:\Users\Lenovo\Documents\Playground\pydeps")
> if str(PYDEPS) not in sys.path:
>     sys.path.insert(0, str(PYDEPS))
>
> import numpy as np
> import zstandard as zstd
>
>
> class DummyArray:
>     def __init__(self, *args, **kwargs):
>         self.args = args
>         self.state = None
>
>     def __setstate__(self, state):
>         self.state = state
>
>
> class DummyObject:
>     def __setstate__(self, state):
>         if isinstance(state, dict):
>             self.__dict__.update(state)
>         else:
>             self.state = state
>
>
> _stub_cache = {}
>
>
> def make_stub(module: str, name: str):
>     key = (module, name)
>     if key not in _stub_cache:
>         _stub_cache[key] = type(name, (DummyObject,), {"__module__": module})
>     return _stub_cache[key]
>
>
> class PickleExtractor(pickle.Unpickler):
>     def find_class(self, module, name):
>         if module == "builtins":
>             return super().find_class(module, name)
>         if (module, name) == ("numpy._core.multiarray", "_reconstruct"):
>             return lambda *args, **kwargs: DummyArray(*args, **kwargs)
>         if (module, name) == ("numpy", "ndarray"):
>             return DummyArray
>         if (module, name) == ("numpy", "dtype"):
>             return np.dtype
>         if (module, name) == ("jax._src.array", "_reconstruct_array"):
>             return lambda *args, **kwargs: DummyArray(*args, **kwargs)
>         if module.startswith("tracr"):
>             return make_stub(module, name)
>         return super().find_class(module, name)
>
>
> def load_payload(path: Path):
>     with path.open("rb") as fp, zstd.ZstdDecompressor().stream_reader(fp) as cfp:
>         return PickleExtractor(cfp).load()
>
>
> def main():
>     parser = argparse.ArgumentParser(
>         description="Extract config and parameter keys from the hajimi challenge pickle."
>     )
>     parser.add_argument(
>         "path",
>         nargs="?",
>         default=r"F:\hajimi\challenge.pkl.zst",
>         help="Path to challenge.pkl.zst",
>     )
>     args = parser.parse_args()
>
>     payload = load_payload(Path(args.path))
>     config = payload["config"]
>     params = payload["params"]
>
>     print("top-level keys:")
>     for key in payload.keys():
>         print(f"  {key}")
>
>     print("\nconfig:")
>     for key in sorted(config.keys()):
>         print(f"  {key} = {config[key]}")
>
>     print("\nparam keys:")
>     for key in sorted(params.keys()):
>         print(f"  {key}")
>
>     print("\nrequired tracr stubs:")
>     for module, name in sorted(_stub_cache):
>         print(f"  {module}.{name}")
>
>
> if __name__ == "__main__":
>     main()
> ```
> 输出是
>
> ```plain
> top-level keys:
>   config
>   params
>   input_encoder
>   output_encoder
>   residual_labels
>   embed_spaces
>
> config:
>   activation_function = relu
>   causal = False
>   dropout_rate = 0.0
>   key_size = 257
>   layer_norm = False
>   mlp_hidden_size = 1290
>   num_heads = 5
>   num_layers = 13
>
> param keys:
>   pos_embed
>   token_embed
>   transformer/layer_0/attn/key
>   transformer/layer_0/attn/linear
>   transformer/layer_0/attn/query
>   transformer/layer_0/attn/value
>   transformer/layer_0/mlp/linear_1
>   transformer/layer_0/mlp/linear_2
>   transformer/layer_1/attn/key
>   transformer/layer_1/attn/linear
>   transformer/layer_1/attn/query
>   transformer/layer_1/attn/value
>   transformer/layer_1/mlp/linear_1
>   transformer/layer_1/mlp/linear_2
>   transformer/layer_10/attn/key
>   transformer/layer_10/attn/linear
>   transformer/layer_10/attn/query
>   transformer/layer_10/attn/value
>   transformer/layer_10/mlp/linear_1
>   transformer/layer_10/mlp/linear_2
>   transformer/layer_11/attn/key
>   transformer/layer_11/attn/linear
>   transformer/layer_11/attn/query
>   transformer/layer_11/attn/value
>   transformer/layer_11/mlp/linear_1
>   transformer/layer_11/mlp/linear_2
>   transformer/layer_12/attn/key
>   transformer/layer_12/attn/linear
>   transformer/layer_12/attn/query
>   transformer/layer_12/attn/value
>   transformer/layer_12/mlp/linear_1
>   transformer/layer_12/mlp/linear_2
>   transformer/layer_2/attn/key
>   transformer/layer_2/attn/linear
>   transformer/layer_2/attn/query
>   transformer/layer_2/attn/value
>   transformer/layer_2/mlp/linear_1
>   transformer/layer_2/mlp/linear_2
>   transformer/layer_3/attn/key
>   transformer/layer_3/attn/linear
>   transformer/layer_3/attn/query
>   transformer/layer_3/attn/value
>   transformer/layer_3/mlp/linear_1
>   transformer/layer_3/mlp/linear_2
>   transformer/layer_4/attn/key
>   transformer/layer_4/attn/linear
>   transformer/layer_4/attn/query
>   transformer/layer_4/attn/value
>   transformer/layer_4/mlp/linear_1
>   transformer/layer_4/mlp/linear_2
>   transformer/layer_5/attn/key
>   transformer/layer_5/attn/linear
>   transformer/layer_5/attn/query
>   transformer/layer_5/attn/value
>   transformer/layer_5/mlp/linear_1
>   transformer/layer_5/mlp/linear_2
>   transformer/layer_6/attn/key
>   transformer/layer_6/attn/linear
>   transformer/layer_6/attn/query
>   transformer/layer_6/attn/value
>   transformer/layer_6/mlp/linear_1
>   transformer/layer_6/mlp/linear_2
>   transformer/layer_7/attn/key
>   transformer/layer_7/attn/linear
>   transformer/layer_7/attn/query
>   transformer/layer_7/attn/value
>   transformer/layer_7/mlp/linear_1
>   transformer/layer_7/mlp/linear_2
>   transformer/layer_8/attn/key
>   transformer/layer_8/attn/linear
>   transformer/layer_8/attn/query
>   transformer/layer_8/attn/value
>   transformer/layer_8/mlp/linear_1
>   transformer/layer_8/mlp/linear_2
>   transformer/layer_9/attn/key
>   transformer/layer_9/attn/linear
>   transformer/layer_9/attn/query
>   transformer/layer_9/attn/value
>   transformer/layer_9/mlp/linear_1
>   transformer/layer_9/mlp/linear_2
>
> required tracr stubs:
>   tracr.craft.bases.BasisDirection
>   tracr.craft.bases.VectorSpaceWithBasis
>   tracr.transformer.encoder.CategoricalEncoder
> ```
> num\_layers \= 13
>
> num\_heads \= 5
>
> key\_size \= 257
>
> mlp\_hidden\_size \= 1290
>
> layer\_norm \= False
>
> causal \= False
>
> 而且参数名是标准 Transformer 结构：
>
> attn/query
>
> ​`attn/key`
>
> ​`attn/value`
>
> ​`attn/linear`
>
> ​`mlp/linear_1`
>
> ​`mlp/linear_2`
>
> 核心前向过程就是：
>
> ```plain
> x = token_embed[token_id] + pos_embed[pos_id]
>
> for layer in range(13):
>     q = x @ Wq + bq
>     k = x @ Wk + bk
>     v = x @ Wv + bv
>
>     q, k, v = reshape_to_heads(...)
>     attn = softmax(q @ k.T / sqrt(257))
>     y = attn @ v
>     y = y @ Wo + bo
>     x = x + y
>
>     y = relu(x @ W1 + b1)
>     y = y @ W2 + b2
>     x = x + y
> ```
> 最后不需要真正实现 Tracr 的完整 unembed，只要读出 residual 里对应 `sequence_map_1:*` 那些维度，做 argmax 就行，因为输出字符就是从这些 basis direction 里读出来的。
>
> ```plain
> (在标准的深度学习模型中，最后一层通常有一个巨大的矩阵（Unembed/Head），把隐藏向量转换成成千上万个词的概率。
>
> 但在 Tracr 模型中：
>
> 输出已在向量中：计算结果（比如你要找的 Flag 字符）其实已经存在于残差流 x 的某些特定索引（Index）里了。
>
> sequence_map_1:* 维度：这是 Tracr 编译器给变量起的“物理地址”。比如第 100 到 126 维可能就对应着变量 sequence_map_1。
>
> 直接读取：你不需要做最后的矩阵乘法，只需要把最后一次迭代后的向量 x 拿出来，盯着第 100 到 126 维看。哪一维的值最大（argmax），那个维度对应的字符就是输出。)
> ```
> Tracr  是 Google DeepMind 开发的一个工具，它能将类似程序的逻辑（用 RASP 语言编写）直接“编译”成 Transformer 的权重![](/assets/img/media/2026_4_20/image-20260416160157-up7g1nl.png)
>
> 把模型跑起来之后（手写 forward 跑），最关键的是看 `residual_labels`​。 (`residual_labels` 是最关键的\*\*“地址簿”​**或**“内存映射表”\*\*。 Transformer 的残差流（Residual Stream）是一个高维向量（比如 1285 维），而 `residual_labels`​ 告诉了你：**向量中的每一个维度具体代表哪一个变量。**   )
>
> ### exp
>
> ```python
> import argparse
> import math
> import pickle
> import subprocess
> import sys
> from pathlib import Path
>
> REQUIRED_PYTHON = (3, 11)
> PYTHON311 = Path(r"C:\Program Files\Python311\python.exe")
> PYDEPS = Path(r"C:\Users\Lenovo\Documents\Playground\pydeps")
>
>
> def ensure_compatible_python():
>     if sys.version_info[:2] == REQUIRED_PYTHON:
>         return
>     if PYTHON311.exists():
>         result = subprocess.run([str(PYTHON311), __file__, *sys.argv[1:]], check=False)
>         raise SystemExit(result.returncode)
>     raise SystemExit(
>         "This script needs Python 3.11 because pydeps contains cp311 wheels. "
>         f"Current interpreter: {sys.executable} ({sys.version.split()[0]})."
>     )
>
>
> ensure_compatible_python()
>
> if str(PYDEPS) not in sys.path:
>     sys.path.insert(0, str(PYDEPS))
>
> import numpy as np
> import zstandard as zstd
>
>
> class DummyObject:
>     def __setstate__(self, state):
>         if isinstance(state, dict):
>             self.__dict__.update(state)
>         else:
>             self.state = state
>
>
> _stub_cache = {}
>
>
> def make_stub(module: str, name: str):
>     key = (module, name)
>     if key not in _stub_cache:
>         _stub_cache[key] = type(name, (DummyObject,), {"__module__": module})
>     return _stub_cache[key]
>
>
> def reconstruct_jax_array(np_reconstruct, np_args, np_state, jax_state):
>     arr = np_reconstruct(*np_args)
>     arr.__setstate__(np_state)
>     return arr
>
>
> class PayloadLoader(pickle.Unpickler):
>     def find_class(self, module, name):
>         if module == "builtins":
>             return super().find_class(module, name)
>         if (module, name) == ("numpy._core.multiarray", "_reconstruct"):
>             return np._core.multiarray._reconstruct
>         if (module, name) == ("numpy", "ndarray"):
>             return np.ndarray
>         if (module, name) == ("numpy", "dtype"):
>             return np.dtype
>         if (module, name) == ("jax._src.array", "_reconstruct_array"):
>             return reconstruct_jax_array
>         if module.startswith("tracr"):
>             return make_stub(module, name)
>         return super().find_class(module, name)
>
>
> def load_payload(path: Path):
>     with path.open("rb") as fp, zstd.ZstdDecompressor().stream_reader(fp) as cfp:
>         return PayloadLoader(cfp).load()
>
>
> def softmax(x, axis=-1):
>     x = x - np.max(x, axis=axis, keepdims=True)
>     e = np.exp(x)
>     return e / np.sum(e, axis=axis, keepdims=True)
>
>
> def run_model(payload, prompt: str):
>     cfg = payload["config"]
>     params = payload["params"]
>     input_map = payload["input_encoder"].encoding_map
>
>     tokens = ["BOS"] + list(prompt)
>     token_ids = np.array([input_map[t] for t in tokens], dtype=np.int64)
>     pos_ids = np.arange(len(tokens), dtype=np.int64)
>
>     x = (
>         params["token_embed"]["embeddings"][token_ids]
>         + params["pos_embed"]["embeddings"][pos_ids]
>     )
>
>     num_heads = cfg["num_heads"]
>     key_size = cfg["key_size"]
>
>     for layer in range(cfg["num_layers"]):
>         base = f"transformer/layer_{layer}"
>
>         q = x @ params[f"{base}/attn/query"]["w"] + params[f"{base}/attn/query"]["b"]
>         k = x @ params[f"{base}/attn/key"]["w"] + params[f"{base}/attn/key"]["b"]
>         v = x @ params[f"{base}/attn/value"]["w"] + params[f"{base}/attn/value"]["b"]
>
>         seq_len = x.shape[0]
>
>         q = q.reshape(seq_len, num_heads, key_size).transpose(1, 0, 2)
>         k = k.reshape(seq_len, num_heads, key_size).transpose(1, 0, 2)
>         v = v.reshape(seq_len, num_heads, key_size).transpose(1, 0, 2)
>
>         scores = q @ k.transpose(0, 2, 1) / math.sqrt(key_size)
>         attn = softmax(scores, axis=-1)
>
>         y = attn @ v
>         y = y.transpose(1, 0, 2).reshape(seq_len, num_heads * key_size)
>         y = y @ params[f"{base}/attn/linear"]["w"] + params[f"{base}/attn/linear"]["b"]
>         x = x + y
>
>         y = x @ params[f"{base}/mlp/linear_1"]["w"] + params[f"{base}/mlp/linear_1"]["b"]
>         y = np.maximum(y, 0.0)
>         y = y @ params[f"{base}/mlp/linear_2"]["w"] + params[f"{base}/mlp/linear_2"]["b"]
>         x = x + y
>
>     return x
>
>
> def get_sequence_map_block(payload, prefix="sequence_map_1:"):
>     labels = payload["residual_labels"]
>     idxs = [i for i, label in enumerate(labels) if label.startswith(prefix)]
>     alphabet = [labels[i].split(":", 1)[1] for i in idxs]
>     return idxs, alphabet
>
>
> def decode_from_residual(payload, residual, prefix="sequence_map_1:"):
>     idxs, alphabet = get_sequence_map_block(payload, prefix=prefix)
>     scores = residual[:, idxs]
>     best = scores.argmax(axis=-1)
>
>     out = []
>     for idx in best[1:]:
>         ch = alphabet[idx]
>         if ch == "EOS":
>             break
>         out.append(ch)
>     return "".join(out), idxs, alphabet
>
>
> def dump_config(payload):
>     print("config:")
>     for key in sorted(payload["config"].keys()):
>         print(f"  {key} = {payload['config'][key]}")
>
>
> def dump_sequence_map(payload, prefix="sequence_map_1:"):
>     idxs, alphabet = get_sequence_map_block(payload, prefix=prefix)
>     print(f"{prefix} block:")
>     for i, ch in zip(idxs, alphabet):
>         print(f"  {i}: {ch!r}")
>
>
> def main():
>     parser = argparse.ArgumentParser(description="Pure NumPy runner for hajimi challenge.pkl.zst")
>     parser.add_argument("prompt", help="16-char grid string, e.g. 1234123412341234")
>     parser.add_argument(
>         "--path",
>         default=r"F:\hajimi\challenge.pkl.zst",
>         help="Path to challenge.pkl.zst",
>     )
>     parser.add_argument(
>         "--dump-config",
>         action="store_true",
>         help="Print model config before running",
>     )
>     parser.add_argument(
>         "--dump-sequence-map",
>         action="store_true",
>         help="Print residual_labels positions for sequence_map_1:*",
>     )
>     args = parser.parse_args()
>
>     payload = load_payload(Path(args.path))
>
>     if args.dump_config:
>         dump_config(payload)
>         print()
>
>     if args.dump_sequence_map:
>         dump_sequence_map(payload)
>         print()
>
>     residual = run_model(payload, args.prompt)
>     decoded, idxs, alphabet = decode_from_residual(payload, residual)
>
>     print("decoded:", decoded)
>     print("sequence_map_indices:", idxs)
>     print("sequence_map_alphabet:", alphabet)
>
>
> if __name__ == "__main__":
>     main()
> ```
> ​`` `python C:\Users\Lenovo\Desktop\run_hajimi_numpy.py  1234341221434321 --dump-values  map_53: --dump-values map_52: --dump-values map_50: --dump-values map_41:` ``
>
> ```plain
> map_53: values:
>   map_53:0 @ 415
>     values = [0.0, 0.0, 1.0, 1.0, 1.0, 1.0, 1.0, 0.0, 1.0, 1.0, 0.0, 1.0, 1.0, 1.0, 1.0, 1.0, 0.0]
>     active_with_bos = [2, 3, 4, 5, 6, 8, 9, 11, 12, 13, 14, 15]
>     active_prompt_positions = [2, 3, 4, 5, 6, 8, 9, 11, 12, 13, 14, 15]
>   map_53:1 @ 416
>     values = [0.0, 1.0, 0.0, 0.0, 0.0, 0.0, 0.0, 1.0, 0.0, 0.0, 1.0, 0.0, 0.0, 0.0, 0.0, 0.0, 1.0]
>     active_with_bos = [1, 7, 10, 16]
>     active_prompt_positions = [1, 7, 10, 16]
>
> map_52: values:
>   map_52:0 @ 413
>     values = [0.0, 1.0, 0.0, 1.0, 1.0, 1.0, 1.0, 1.0, 0.0, 0.0, 1.0, 1.0, 1.0, 1.0, 1.0, 0.0, 1.0]
>     active_with_bos = [1, 3, 4, 5, 6, 7, 10, 11, 12, 13, 14, 16]
>     active_prompt_positions = [1, 3, 4, 5, 6, 7, 10, 11, 12, 13, 14, 16]
>   map_52:1 @ 414
>     values = [0.0, 0.0, 1.0, 0.0, 0.0, 0.0, 0.0, 0.0, 1.0, 1.0, 0.0, 0.0, 0.0, 0.0, 0.0, 1.0, 0.0]
>     active_with_bos = [2, 8, 9, 15]
>     active_prompt_positions = [2, 8, 9, 15]
>
> map_50: values:
>   map_50:0 @ 411
>     values = [0.0, 1.0, 1.0, 0.0, 1.0, 0.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 0.0, 1.0, 0.0, 1.0, 1.0]
>     active_with_bos = [1, 2, 4, 6, 7, 8, 9, 10, 11, 13, 15, 16]
>     active_prompt_positions = [1, 2, 4, 6, 7, 8, 9, 10, 11, 13, 15, 16]
>   map_50:1 @ 412
>     values = [0.0, 0.0, 0.0, 1.0, 0.0, 1.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 1.0, 0.0, 1.0, 0.0, 0.0]
>     active_with_bos = [3, 5, 12, 14]
>     active_prompt_positions = [3, 5, 12, 14]
>
> map_41: values:
>   map_41:0 @ 408
>     values = [0.0, 1.0, 1.0, 1.0, 0.0, 1.0, 0.0, 1.0, 1.0, 1.0, 1.0, 0.0, 1.0, 0.0, 1.0, 1.0, 1.0]
>     active_with_bos = [1, 2, 3, 5, 7, 8, 9, 10, 12, 14, 15, 16]
>     active_prompt_positions = [1, 2, 3, 5, 7, 8, 9, 10, 12, 14, 15, 16]
>   map_41:1 @ 409
>     values = [0.0, 0.0, 0.0, 0.0, 1.0, 0.0, 1.0, 0.0, 0.0, 0.0, 0.0, 1.0, 0.0, 1.0, 0.0, 0.0, 0.0]
>     active_with_bos = [4, 6, 11, 13]
>     active_prompt_positions = [4, 6, 11, 13]
>
> decoded: Grid accepted.
> sequence_map_indices: [1068, 1069, 1070, 1071, 1072, 1073, 1074, 1075, 1076, 1077, 1078, 1079, 1080, 1081, 1082, 1083]
> sequence_map_alphabet: [' ', '.', 'EOS', 'G', 'W', 'a', 'c', 'd', 'e', 'g', 'i', 'n', 'o', 'p', 'r', 't']
> ```
> 这里面能看到很多 Tracr 编译后的符号变量名，比如：
>
> ​`sequence_map_*`
>
> ​`map_*`
>
> ​`selector_width_*`
>
> ​`count_True_*`
>
> 继续观察最终 residual，会发现有四组非常关键的位置掩码：
>
> ![](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20260329200759880.png)
>
> ​`map_53 = 1`​ 出现在位置：`1, 7, 10, 16`
>
> ​`map_52 = 1`​ 出现在位置：`2, 8, 9, 15`
>
> ​`map_50 = 1`​ 出现在位置：`3, 5, 12, 14`
>
> ​`map_41 = 1`​ 出现在位置：`4, 6, 11, 13`
>
> 这四组恰好把 16 个位置分成了四类。进一步对照模型行为，可以恢复出每一类对应的数字分别是：
>
> ​`1`​ → `1, 7, 10, 16`
>
> ​`2`​ → `2, 8, 9, 15`
>
> ​`3`​ → `3, 5, 12, 14`
>
> ​`4`​ → `4, 6, 11, 13`
>
> ![](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20260329195638606.png)
>
> 分组排序
>
> 于是答案串就是：
>
> ```plain
> 1234341221434321
> ```
> 按 4×4 排开就是：
>
> ```plain
> 1 2 3 4
> 3 4 1 2
> 2 1 4 3
> 4 3 2 1
> ```
> ### flag
>
> ```plain
> 1234341221434321
> ```
> 做 SHA-256，得到：
>
> ```plain
> b0a0d1edc0fb5b75770a5dcbe7b0d4fb08e42fd281a94ee67b405e36056f1df1
> ```
> 最终 flag：
>
> ```plain
> xmctf{b0a0d1edc0fb5b75770a5dcbe7b0d4fb08e42fd281a94ee67b405e36056f1df1}
> ```
