---
title: 0xGame2026 Week2 WP
date: 2026-10-15 20:48:35
tags:
    - Osint
    - Misc
    - Crypto
    - WP
categories: CTF
---

本文是0xGame2026第二周的WriteUp

<!-- more -->

# I. Osint

## 一. 天气之子？

### 解题过程

读题，发现题目给出一张kards梗图。根据题目信息，在b站搜索`kards天气之子`，找到线索：[8.22kards锦标赛 sie反制操控天气录像](https://www.bilibili.com/video/BV1J7826GE5C)。根据视频与评论区内容，推测“天气之子”是`sie_`，事件时间即为2026.8.22。

题目要找sie_2025年1月的赛事模式奖杯数，所以打开KARDS，在赛事模式中查看2025年1月的排行榜：

{% asset_img KARDS.jpg %}

sie_的奖杯数为118。至此已经获得flag。

### Flag

0xGame{KARDS_2026.08.22_118}

# II. Misc

## 一. ez_ftp

### 解题过程

下载附件，解压，用wireshark打开。先导出所有FTP-DATA对象。发现有两份backup.zip。

在wireshark中筛选题给IP`192.168.20.10`，查看筛选结果，注意到在约447.94秒处，backup.zip完成了7300字节的传输，在等待第7301字节，随后传输断开。往下，在约536.6秒处，backup.zip从7301字节开始继续传输，直至结束。所以导出的两份backup.zip应当是断点续传前后的两部分。检查Header，发现7300字节的部分有正常的zip魔数，而另外一份没有，符合预期。使用`cat backup.zip backup1.zip > fixed.zip`拼接文件。

逐个尝试打开所得文件，发现只有`brochure.pdf`不带密码，且没有提供有效信息。随意尝试，发现`readme.zip`的密码是`123456`，解压得到`README.md`。打开阅读，发现两点重要内容：

```text
3. 压缩包密码规则：**维护人的工号(大写) + 入职年月(YYYYMM)**，例如工号 `NQ1001`、2020 年 3 月入职，则密码为 `NQ1001202003`。
4. 员工信息见本目录 `roster.xlsx`（涉及薪资等敏感信息已加密，密码与运维 FTP 登录口令相同）。
```

所以应当先尝试寻找FTP登陆口令，再在表格中寻找解压密码。回到wireshark翻看，在约447.14秒附近发现了账号`ops_backup`使用密码`Bk_Ops#2026`成功登录。所以使用`Bk_Ops#2026`打开表格`roster.xlsx`，筛选运维部。

题干中提到`运维小王`，所以压缩包密码应当对应`王磊`，根据readme中的构造规则，得到`backup.zip`的密码`NQ2077202107`，解压出flag.txt，得到flag。

### Flag

0xGame{e2717ba6-a1dc-4171-a741-bc89be5d7b2e}

## 二. ez_chain

### 解题过程

开启容器，nc连接：
```text
Can you make the isSolved() function return true?

[1] - Create an account which will be used to deploy the challenge contract
[2] - Deploy the challenge contract using your generated account
[3] - Get your flag once you meet the requirement
[4] - Show the contract source code
[-] input your choice: 
```
先选择1，获得account和token，然后在水龙头中输入account获取ETH。再选择2，输入token后得到contract address。

注册metamask账号，添加题给网络，ChainID为35541。

打开Remix，这段操作直接按题干提示照做。

> 打开 Remix 的 **Deploy & Run Transactions** 面板，在 **ENVIRONMENT** 中选择 **Browser Extension**（浏览器扩展钱包），选择 MetaMask 连接

再次连接容器，选择4获得源码。在Remix的deployed contracts中add contract，输入刚刚获得的contract address并添加。再选择start coding，将题给源码粘贴并编译。

阅读源码，发现期望字符串是`HelloChainFlag`，输入并Transact。

{% asset_img Remix.png %}

连接容器，选择3，输入token。得到flag。

### Flag

（可能是动态的）
0xGame{d9d728b6-376c-4a0e-98ad-abbb6a99be92}

## 三. 挽尊糕手

### 解题过程

下载附件，解压。打开`L0veStory.txt`，发现大量零宽字符，怀疑零宽字符隐写。

查看`March-7th.png`，发现这是一张三月七的图片。搜索发现，这张使用file、binwalk检查没有特别发现。使用zsteg检查发现：
```text
b1,rgb,lsb,xy       .. text: "the password is that girl's birthday"
```
再使用exiftool检查发现有：
```text
Image Description               : https://www.somd5.com/

User Comment                    : https://330k.github.io/misc_tools/unicode_steganography.html
```
这两个网站分别指向md5在线破解和零宽字符解码。先使用零宽字符对`L0veStory.txt`解码，得到《Never gonna give you up》的歌词和大段空白字符。联系题目描述，想到SNOW隐写。

使用stegsnow，联系前面zsteg得出的提示，依次尝试三月七生日的不同写法：`March-7th`，`0307`，`03.07`，`03/07`，`3.7`，`3/7`等。最终用`3.7`得到了原文：

```hex
504B03041400080008002B93085D00000000000000000000000008002000666C61672E74787475780B00010400000000040000000055540D00071204776AD21F786A8EFC766A33B5B030324A4D4A3234314F354A4C4C4B36303234304A36344C4C313449354E0500504B07083C2FA7A42200000020000000504B010214031400080008002B93085D3C2FA7A42200000020000000080018000000000000000000B68100000000666C61672E74787475780B00010400000000040000000055540500011204776A504B050600000000010001004E000000780000000000
```

这是一个压缩包的hex，转为文件保存后，解压得到flag.txt，打开得到一串md5：
```text
58822ebb147e2aafc02102c11ad14e3e
```
用刚刚得到的somd5破解，得到flag。

### Flag

0xGame{where_is_true_lovvvvve?}

## 四. 小伊卡...不胖...不胖...（已黑化）

### 解题过程

下载附件，解压，得到损坏的png。依旧是宽高篡改，使用工具`Deformed-Image-Restorer.py`恢复，得到flag。

### Flag

0xGame{d0_yOu_tru1y_underst@nd_(Rc?_640_640}

## 五. Zip医生

### 解题过程

下载附件。尝试解压，有密码。先用xxd检查Hex，得到header：
```hex
504b 0304 1400 0100 0000 67b4 235d 9041 2b22 9a02 0000 8e02 0000 1600 0000
```
结合题干，确定压缩方式为stored，加密方式为ZipCrypto。用7zip打开压缩包，发现两个项目：`locked.py`和`locked.cpython-313.pyc`，推测此pyc就是由同压缩包内的py编译得到。同时观察到`locked.py`的原大小是 488 字节，`locked.cpython-313.pyc`的原大小是 654 字节。可以考虑已知明文攻击。

由于pyc的header开头统一为4字节的版本魔数加上4字节的时间戳模式flags，于是选定此处作为突破口。由文件名可知，此文件中pyc的python版本是3.13，则魔数为`f3 0d 0d 0a`。由于没有特殊标明，时间戳模式应当为默认的`00 00 00 00`。

此外，源文件`locked.py`的原大小为488字节，即0x1E8，则`locked.cpython-313.pyc`的12-15offset处应当为小端序的0x1E8，即`E8 01 00 00`。

另外，`locked.cpython-313.pyc`在-1offset处的校验字节也是已知的，是CRC32的第一个字节0x22，可以作为补充。不过bkcrack会自动获取CRC32并补全这条，可以不用管。

至此，已经凑齐了已知明文攻击所需的至少12字节，使用bkcrack进行攻击：
```bash
bkcrack -C secret.zip -c locked.cpython-313.pyc -x 0 f30d0d0a00000000 -x 12 e8010000
```
指定pyc文件的offset0处开始为`f30d0d0a00000000`，offset12处开始为`e8010000`，破解得到zip的key：`64917537 61b24fcf b564419c`，通过key获取zip内的py和pyc文件。

查看`locked.py`发现这是一个RSA。n，e，c已知，而p，q未知。使用RsaCtfTool破解：
```bash
RsaCtfTool -n 115792089237316195423570985008687907853269984665640564039457584007913129639937 -e 65537 --attack all --private --decrypt 19411058501260029511744403653712197844299720287315419561082722362568892501402
```
得到输出：
```text
Decrypted data :
HEX : 0x307847616d657b7079635f31735f6e30745f73305f7333637572337d
INT (big endian) : 5104469840972784149098881674139286390638096044764542821853656396669
INT (little endian) : 13185200569465843211405831468497297165905047272199925669990744160304
utf-8 : 0xGame{pyc_1s_n0t_s0_s3cur3}
utf-16 : 砰慇敭灻捹ㅟ彳の彴び獟挳牵紳
STR : b'0xGame{pyc_1s_n0t_s0_s3cur3}'
```
获得flag。

### Flag

0xGame{pyc_1s_n0t_s0_s3cur3}

# III. Crypto

## 一. 群友群友群？

### 解题过程

下载密文与hint，结合题干，搜索发现密文为`猪圈密码`变体。

对照密码表进行解密：

{% asset_img 猪圈变体.png %}

得到`WELCOMETOSIGNUP`，套上外壳即为flag。

### Flag

0xGame{WELCOMETOSIGNUP}