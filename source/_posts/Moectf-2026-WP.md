---
title: Moectf 2026 WP
date: 2026-10-15 16:56:51
tags:
    - Misc
    - WP
categories: CTF
---

本文是Moectf2026的WriteUp，本次比赛中完成的大部分题目是Misc方向，其他方向做出个别题没有写在WP中。

<!-- more -->

**注意**：本篇WP使用*斜体*的部分意为信息/思路/代码**来自AI**

# 一. MISC入门指北

## 1. 解题思路

阅读附件miscru_men_zhi_bei_.pdf，注意到文件末尾给出提示：flag可在下方找到。所以尝试向下选择，选中一片透明字符，将其复制。粘贴可得：

```
01101101 01101111 01100101 01100011 01110100 01100110 01111011 01010111 00110011 00110001
01100011 00110000 01101101 01100101 01011111 00110111 01101111 01011111 01101101 00110001
00110101 01100011 01111101
```

将其在CyberChef中通过From Binary进行二进制转换，直接得到Flag。

## 2. Flag

moectf{W31c0me_7o_m15c}

# 二. Tomato

## 1. 解题思路

下载附件tomato.jpeg，查看图片发现此图片被进行混淆处理。使用[小番茄图片混淆](https://xfqtphx.netlify.app/)上传图片后，进行一次“混淆”操作后得到原图：

{% asset_img tomatoORIG_1.jpg %}

由此直接得到Flag。

## 2. Flag

moectf{Tom@t0_i5_D3lic1ou5!}

# 三. ez_BASE

## 1. 解题思路

下载附件xiaozhitiao.zip，解压得到xiaozhitiao.txt，打开得到一串字符：

> =0HN2U0cAJ2XudHMut2XzYHQo9Vdwk1emR3Yl9Wb

*注意到该段编码类似base64，但正常base64编码的**等于号=应在末尾**，得出结论：这段字符可能是翻转后的Base64。*
于是手动对其进行翻转，得到：

> bW9lY3Rme1kwdV9oQHYzX2tuMHduX2JAc0U2NH0=

在CyberChef中对这段编码通过Form Base64解码得到Flag。

## 2. Flag

moectf{Y0u_h@v3_kn0wn_b@sE64}

# 四. ez_LSB

## 1. 解题思路

下载附件cat.png，由标题得知，本题使用LSB。*从AI处得知LSB的定义与可用的分析工具*后，决定使用在线工具[stegonline](http://stegonline.georgeom.net/)。

在stegonline上传附件，点击Extract Files/Data，选择LSB后开始分析，发现一段可疑的字符：

> flag:bW9  lY3Rme2M  0N19rTjB  XNV9MU0J  9

将`bW9  lY3Rme2M  0N19rTjB  XNV9MU0J  9`使用base64解码即得Flag。

## 2. Flag

moectf{c47_kN0W5_LSB}

# 五. 星走路的旅程-level1

## 1. 解题思路

下载附件level1.jpg，右键查看属性-详细信息，发现：

- 拍摄日期：2026/5/31 10:03
- 照相机制造商：Xiaomi
- 纬度约为26.5532
- 经度约为106.7986

将北京时间10:03换算为UTC为02:03

在[坐标拾取器 | 高德地图API](https://lbs.amap.com/tools/picker)中输入经纬度，发现位置为贵阳龙洞堡国际机场，搜索其IATA代码得到KWE。

组合以上信息得到Flag。

## 2. Flag

moectf{KWE_XIAOMI_0203}

# 六. 空白文档

## 1. 解题思路

下载附件xingde_flag.zip，解压得到xing的flag.docx，直接打开文件，尝试选择，发现一串透明字符，上色后得到提示：`你知道异或吗？key：office`

*询问AI得知，.docx文件可以直接修改后缀作为.zip压缩包打开*

将.docx修改为.zip，解压后~~在AI的提示下~~打开word文件夹中的document.xml，发现一个可疑字符串：

> AgkDChcDFBEOWhEAMFcVNg4cMABXXQQY

结合题目~~和AI~~的提示，发现该字符串类似base64，于是使用CyberChef通过进行base64与XOR解码，key为UTF8的`office`。解码后得到Flag。

## 2. Flag

moectf{wh3re_1s_my_f14g}

# 七. Can_you_find_it?

## 1. 解题思路

下载附件can_you_find_it.tar，直接解压至文件夹can_you_find_it，在此目录下通过wsl打开Unbntu系统。

*使用`jq . manifest.json`查看manifest.json。发现Config为`blobs/sha256/577fd454ca29c1556dbef6c86dc27a93b33f1b6bbdc89f0278b855d2bdf23a15`，同时发现Layers列表。*

*越靠后的Layer可能越晚修改，所以可以调查最后一行Layer，即`blobs/sha256/5f70bf18a086007016e948b04aed3b82103a36bea41755b6cddfaf10ace3c6ef`。*

*AI提示可以使用file命令调查此Layer的文件类型*

此时想到：可以直接尝试批量地file调查sha256中的所有文件，于是将想法交给AI。

*从AI处得到命令：`find blobs/sha256 -type f -exec file {} \;`。*

执行后发现：仅有`5f70bf18a086007016e948b04aed3b82103a36bea41755b6cddfaf10ace3c6ef`显示为data，以及manifest中记载的Config文件为JSON text data。

*AI提示，可以使用jq打开Config文件，于是`jq . blobs/sha256/577fd454ca29c1556dbef6c86dc27a93b33f1b6bbdc89f0278b855d2bdf23a15`*

查看输出，发现：

```json
      "created": "2026-07-13T00:40:33.333284076+08:00",
      "created_by": "COPY flag.png /var/cache/.system/cache_meta.log # buildkit",
      "comment": "buildkit.dockerfile.v0"
```

以及：

```json
     "created": "2026-07-13T00:40:33.878541869+08:00",
      "created_by": "RUN /bin/sh -c echo \"xing的hint：或许你可以查一下系统缓存？\" > /home/hint1.txt # buildkit",
      "comment": "buildkit.dockerfile.v0"
    },
    {
      "created": "2026-07-13T00:40:34.237990128+08:00",
      "created_by": "RUN /bin/sh -c echo \"xing的hint：注意隐藏\" > /var/cache/hint2.txt # buildkit",
      "comment": "buildkit.dockerfile.v0"
```

于是目标变成：寻找`/var/cache/.system/cache_meta.log`。*AI提示，可以通过`tar -tf`来查看类型为POSIX tar archive的文件*

于是逐个排查，发现了cache_meta.log的位置：

通过
```bash
tar -tf b021f16e614673fbd3af42ef46531ed2c4b3b400bba6291345e3c0101d56dd6d
```
得到输出：
```
var/
var/cache/
var/cache/.system/
var/cache/.system/cache_meta.log
```

将`b021f16e614673fbd3af42ef46531ed2c4b3b400bba6291345e3c0101d56dd6d`的后缀改为.tar，打开并提取出cache_meta.log，并改名为flag.png，打开图片：

{% asset_img flag.png %}

发现是一个缺了两角的二维码。在PS中复制定位块并粘贴到缺失处，得到可扫描的二维码：

{% asset_img flag_r.png %}

扫描即得Flag。

## 2. Flag

moectf{1n_7h3_1m4g3_cach3}

# 八. Pixel_Art

## 1. 解题思路

读题，发现题目给出了像素画坐标，于是打开Excel，将行列设为等宽，手动涂色，结果如下：

{% asset_img PixelArt.png %}

旋转并镜像后得到Flag：

{% asset_img PixelArt_r.png %}

## 2. Flag

moectf{i5_7h1s_gr1d_ch4r?}

# 九. 一线生机

## 1. 解题思路

下载附件flag.txt，尝试解压发现有密码。虽然无法解压，但可观察到内部有两个文件：flag.txt和README.md。

*AI提示可以使用`Format-Hex .\flag.zip | Select-Object -First 20`来查看zip的Header。*

于是输入命令，得到的部分flag.txt的Local Header如下：

```text
50 4B 03 04
14 00
09 00
00 00
AF 8A E1 5C
4F F1 E4 B5
2A 00 00 00
1E 00 00 00
08 00 00 00
66 6C 61 67 2E 74 78 74
```

*第三行的09 00代表General Purpose Bit Flag = 0x0009，即0x0001（文件被加密）+0x0008（使用Data Descriptor）。而第四行的00 00代表Compression Method = 0（Stored），即文件没有进行Deflate压缩，而是直接存储。2A表示压缩后大小为42bytes，1E表示压缩前大小为30bytes。压缩后大小增大了12bytes，可能意味着文件内容前被ZipCrypto放置了一个12字节的加密Header。*

同时，得到README.md的Local Header如下：

```text
50 4B 03 04
14 00
09 00
00 00
4F 8A E1 5C
3A 48 59 B1
42 00 00 00
36 00 00 00
09 00 00 00
52 45 41 44 4D 45 2E 6D 64
```

*AI提示，README.md的压缩前后大小仍然恰好相差12bytes，压缩前为0x36即54bytes，压缩后为0x42即66bytes。*

注意到，题目描述为`有时候，一线生机往往藏在最明显的地方`，正好是54bytes，于是基本可以断定，这段话即为README.md的内容。

*至此可以通过bkcrack进行已知明文攻击。转到bkcrack所在目录，通过bcraker进行已知明文攻击：`.\bkcrack.exe -C 加密.zip -c 加密包内要破解的条目名 -p 已知明文文件`*

此处直接新建一个空文档，将54字节的题干输入其中，并命名为README.md，作为已知明文文件，而“加密包内要破解的条目名”同样为README.md。

攻击完成后，得到三个内部key：`39cd809b 10a0fcb2 669a68f7`

*现在已经可以通过三个key生成解密后的zip：`.\bkcrack.exe -C D:\Download\flag.zip -k 39cd809b 10a0fcb2 669a68f7 -D D:\Download\decrypted.zip`*

但实际运行时，却出现了错误：`Zip error: could not find local file header.`

*AI提示，错误原因可能是此zip文件本身是故意构造的非标准zip，所以尝试直接提取flag.txt而不是生成整个解密的zip：`.\bkcrack.exe -C D:\Download\flag.zip -c flag.txt -k 39cd809b 10a0fcb2 669a68f7 -d D:\Download\flag_decrypted.txt`*

至此，flag.txt已被成功提取，得到Flag。

## 2. Flag

moectf{1t_i5_So0o0o0o_Obv1ou5}

# 十. 你会git吗？

## 1. 解题思路

下载附件flag.txt，直接解压得到.git文件夹。

*询问AI得知，这是一个git仓库。在AI提示下，使用`git show HEAD`与`git show ORIG_HEAD`。*

得到提示：`你知道git端暂存区在哪吗？`

*在AI的提示下，使用命令`git status`以及`git ls-files --stage`，得到输出：*

```console
PS D:\Download\flag> git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   flag.txt

Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        deleted:    flag.txt

PS D:\Download\flag> git ls-files --stage
100644 8af10dd72556db612f2bbe18c764267e28b65b84 0       flag.txt
```

现在几乎已经得到flag.txt，最后*使用命令：`git cat-file -p 8af10dd72556db612f2bbe18c764267e28b65b84`*

即可得到flag.txt的内容：

`flag is me,use base64 and put in moectf{}:ZzE3XzE1X3NvXzNhU3k=`

通过base64解码即得Flag。

~~哦对了，其实这题用`git diff`能直接出答案（~~

## 2. Flag

moectf{g17_15_so_3aSy}

# 十一. 半部电台

## 1. 解题思路

下载附件flag_0038.wav，打开音频发现内容很特别。

*注意到，此文件可能为sstv编码*

于是使用在线工具[Online SSTV Decoder](https://sstv-decoder.mathieurenaud.fr/)进行解码，得到Flag如下：

{% asset_img bbdt.png %}

## 2. Flag

moectf{ss7v-is-7h3-m4in-w4y-radi0-4mateurs-7r4n5m1t-im4ges}

# 十二. 星走路的旅程-level2

## 1. 解题思路

下载附件level2.jpg。右键属性-详细信息，一无所获。观察图片中的门店分布：
- 尝试在搜索引擎中搜索“爷爷不泡茶”的分布，未果
- 尝试在搜索引擎中搜索“熊喵来了火锅”的分布，未果
- 尝试在搜索引擎中搜索“刘一手重庆火锅”的分布，未果
- 尝试在搜索引擎中搜索“喜上枣子糕”的分布，发现搜索引擎根本没有这家店的信息
- 尝试在高德地图中搜索“爷爷不泡茶”的分布，未果
- 尝试在高德地图中搜索“熊喵来了火锅”的分布，未果
- 尝试在高德地图中搜索“刘一手重庆火锅”的分布，未果
- 尝试用AI整理几家门店分布地点的交集，未果
- 尝试通过level1的地点与拍摄者的身份分析其行程，未果
- （数日后）尝试在美团中搜索“喜上枣子糕”，成功定位到唯一地点；尝试在高德地图中搜索“喜上枣子糕”，成功定位到唯一地点
- 得到Flag
- 😅

## 2. Flag

moectf{JIANGSU_LIANYUNGANG_LINGZHOUDONGLU}

# 十三. ez_keyboard

## 1. 解题思路

下载附件keyboard.pcap，使用wireshark打开。~~看不懂（~~

找到工具[UsbKeyboardDataHacker](https://github.com/WangYihang/UsbKeyboardDataHacker)，根据`Installation`装好环境，对附件进行分析：

```powershell
poetry run python UsbKeyboardDataHacker.py --input D:\Download\keyboard.pcap
```

得到键盘的操作历史：

```text
<RET>fakeflag{typing<SPACE>begins<SPACE>in<SPACE>lowercase<SPACE>by<SPACE>default.}<RET>moectf{w0w_y0u_c4n_r@d<DEL>4d_k3yb0<CAP>ard_<CAP>traffic}<LeftArrow><LeftArrow><LeftArrow><LeftArrow><LeftArrow><DEL><DEL><DEL>7r@<RightArrow><RightArrow><RightArrow><RightArrow><RightArrow><RET>finnsh.c
```

以此可复原Flag。

## 2. Flag

moectf{w0w_y0u_c4n_r@4d_k3yb0ARD_7r@ffic}

# 十四. 魔理沙偷走了重要的东西

## 1. 解题思路

下载附件marisa.zip，解压得到Marisa.png。结合题给提示，猜测本题可能涉及文件拼接。在Ubuntu中使用binwalk进行分析：

```bash
binwalk Marisa.png
```

查看输出，发现此文件由一个448064 bytes的png和一个从448480处开始的zip组成。

*AI提示，可以直接使用dd命令从指定位置开始截取，也可以使用binwalk提取。所以尝试两条：*

```bash
dd if=Marisa.png of=hidden_dd.zip bs=1 skip=448480
binwalk -e Marisa.png
```

尝试解压由dd截取得的zip文件时，提示“压缩文件夹无效”，使用unzip试图解压hidden_dd.zip时，获得提示：`missing 420 bytes in zipfile`；而binwalk提取出了一个extractions文件夹。

检查extractions文件夹，在`extractions\Marisa.png.extracted\6D7E0`中有一个marisa.png，与原附件不同，这张图片是一张魔理沙的立绘。使用binwalk检查，没有发现其他可能的文件格式。

{% asset_img marisa.png %}

*AI提示，可以对dd截取出的hidden_dd.zip进行`unzip -l`：*

```
Archive:  hidden_dd.zip
error [hidden_dd.zip]:  missing 420 bytes in zipfile
  (attempting to process anyway)
  Length      Date    Time    Name
---------  ---------- -----   ----
      443  2026-06-30 15:08   flag.zip
   248610  2026-06-30 15:10   marisa.png
---------                     -------
   249053                     2 files
```

*以及`7z l -slt hidden_dd.zip`和`7z t hidden_dd.zip`，查看输出后，AI注意到存在：*

```
Path = hidden_dd.zip
Type = zip
ERRORS:
Unavailable start of archive
Offset = -420
Physical Size = 248166
```

*于是重新计算新的起点：448480-420=448060。因此，重新进行dd：*

```bash
dd if=Marisa.png of=hidden.zip bs=1 skip=448060
```

得到hidden.zip，尝试解压，但仍然无法成功。

*AI提示，可以继续尝试使用7z分析：`7z t hidden.zip`，仍存在错误：`ERROR: Headers Error : flag.zip`。而使用`7z l -slt hidden.zip`可发现：*

```
Path = flag.zip
Folder = -
Size = 443
Packed Size = 382
Modified = 2026-06-30 15:07:59.7266418
Created =
Accessed =
Attributes = A
Encrypted = -
Comment =
CRC = 9D201477
Method = Deflate64
Characteristics = NTFS
Host OS = FAT
Version = 21
Volume Index = 0
Offset = 0
```

其中重点为**CRC = 9D201477**。

*既然header有问题，那么直接寻找zip签名：`xxd -g 1 -l 512 hidden.zip`*

命令执行后查看输出，发现前两行为：

```
00000000: ae 42 60 82 15 00 00 00 09 00 00 79 de 5c 77 14  .B`........y.\w.
00000010: 20 9d 7e 01 00 00 bb 01 00 00 08 00 00 00 66 6c   .~...........fl
```

*原本应为50 4b 03 04的Signature变成了ae 42 60 82，而原本应为0x9d201477的CRC变成了0x7e9d2014*

因此，现在需要修复这个zip。

*AI给出修复方法：*

```bash
cp hidden.zip repaired.zip
printf '\x50\x4b\x03\x04' | dd of=repaired.zip bs=1 count=4 conv=notrunc
printf '\x77\x14\x20\x9d' | dd of=repaired.zip bs=1 seek=14 count=4 conv=notrunc
```

即：先将hidden.zip复制为repaired.zip，再对repaired.zip逐字节地修复有问题的signature与CRC。

修复完成后发现：repaired.zip中的flag.zip已经可以解压出来，但仍然无法提取flag.zip中的flag.txt：使用7z尝试解压时，提示需要密码。因此，现在的目标是寻找密码。

我们先前还有一张marisa.png没有使用，于是尝试从中寻找线索。

*可以使用zsteg对其进行分析：`zsteg marisa.png`*

得到输出如下：

```
[?] 56 bytes of extra data after image end (IEND), offset = 0x3caea
extradata:0         .. text: "\n--- Marisas Magic Seal ---\ncHdkMjpfdGhpZWZfbWFyaXNh\n---"
imagedata           .. text: "\t\t\t\n\t\t\n\n\n"
b1,r,lsb,xy         .. text: "pwd1:master_spark"
b1,rgb,lsb,xy       .. file: TeX font metric data (\020I$\022@$\022@)
b1,bgr,lsb,xy       .. <wbStego size=0x9004 ext="\x04\x90I" data="\x04\x80@\x00\x90\x01\x00\x92\b\x04"... even=false hdr=nil enc=nil mix=nil controlbyte=nil>
b2,rgb,lsb,xy       .. file: Commodore PET BASIC program, offset 0x0010, line 0, token (0x1)
b2,bgr,lsb,xy       .. file: GLS_BINARY_MSB_FIRST
b2,bgr,msb,xy       .. file: Applesoft BASIC program data, first line number 130
b3,bgr,lsb,xy       .. file: GLS_BINARY_LSB_FIRST
b4,rgb,lsb,xy       .. text: "\"\"\"32\"SE4sG4"
b4,bgr,lsb,xy       .. file: Targa image data - Map 65536 x 1 x 16 - 1-bit alpha
```

注意到：`pwd1:master_spark`以及`\n--- Marisas Magic Seal ---\ncHdkMjpfdGhpZWZfbWFyaXNh\n---`

`cHdkMjpfdGhpZWZfbWFyaXNh`看起来像一段base64编码，尝试解码，得到：`pwd2:_thief_marisa`

至此，解压密码已得：master_spark_thief_marisa

解压flag.zip，得到flag.txt：

```
Marisas Hidden Treasure!
+++++ +++++ [->++ +++++ +++<] >++++ +++++ .++.< +++[- >---< ]>-.- -.<++
++[-> ++++< ]>+.< +++[- >---< ]>--- --.<+ +++[- >++++ <]>++ +++.< +++[-
>---< ]>--- --.<+ +++++ +[->- ----- -<]>- ----- --.<+ +++++ +[->+ +++++
+<]>+ +++++ +++++ ++.-- ----- --.<+ ++[-> +++<] >+.<+ +++[- >---- <]>--
.--.< +++++ +[->- ----- <]>-- ----- ---.< +++++ +++[- >++++ ++++< ]>++.
<++++ [->-- --<]> ----. <++++ [->++ ++<]> +++++ .<+++ [->-- -<]>- --.--
-.--- ---.+ ++.<+ +++++ [->-- ----< ]>--- ----- ---.< +++++ +++[- >++++
++++< ]>.+. <++++ [->-- --<]> ----- .<+++ +[->+ +++<] >++++ +.<++ +[->-
--<]> ---.< +++++ ++[-> ----- --<]> ----- -.++. <++++ +++[- >++++ +++<]
>++.- ----- -.+++ ++.-- -.<++ +++[- >++++ +<]>. <++++ [->-- --<]> -----
.<+++ +[->+ +++<] >++++ ++++. <
```

*AI提示，这是一段brainfuck*

于是使用在线brainfuck解释器[Esolang Playground](https://esolang-playground.web.app/)对其执行，得到Flag。

（解完之后才发现从unzip和7z命令中能获得的所有信息都能直接在7zip里看到）
~~（真是，绕了好大一圈啊）~~

## 2. Flag

moectf{m4risa_1s_the_b3st_th13f_daze}

---

以上内容完成于2026.9.24

---

# 十五. ez_BASE_revenge

## 1. 解题思路

下载附件ez_base_revenge7.zip，解压得到flag7.txt。打开发现仅有一串emoji字符。

*一长串emoji字符可能是base100编码，所以应尝试base100解码。*

使用[BASE100编码解码-Bugku](https://ctf.bugku.com/tool/base100)对其进行解码，得到一串字符：

> MzgzZk1mVnc4dXBtbWJ3ejRvTFhWREJVQlZpTktGYnUycmNtam5VUVIzYXpmR1NlV3hDRTlWM2J2R0FFdTFzaEd2WHRlYnVWc0ZmSzI=

发现其形式类似base64编码，于是使用CyberChef解码，得到：

> 383fMfVw8upmmbwz4oLXVDBUBViNKFbu2rcmjnUQR3azfGSeWxCE9V3bvGAEu1shGvXtebuVsFfK2

CyberChef页面提示，可以继续使用base58解码，得到：

> NVXWKY3UMZ5TG3JQNIYV6ML2L5ZTAX3DOV2DGXZSGMZTGMZTGN6Q====

CyberChef页面提示，可以继续使用base32解码，直接得到Flag。

## 2. Flag

moectf{3m0j1_1z_s0_cut3_2333333}

# 十六. 一部电台

## 1. 解题思路

下载附件chall.wav，打开发现是一个约36分钟的摩斯电码。使用Audacity查看频谱图，发现摩斯电码信息为纯数字串。手动解码可得：

```text
03612973613402070735616803612973064508102417281403612973162745830361297307356168312731125261394503612973036129733127311206450810162745835261394513811730162745830645081024172814526139453127311231273112242924940735616852613945036129730645081019470948036129735261394516274583613402071627458303612973304600310361297331273112073561680735616816274583613402071627458307356168138117300361297303612973162745836134020716274583312731125261394507356168073561680361297306450810036129730361297316274583036129733127311231273112242924940735616852613945312731121381173003612973194709480735616831273112312731120645081031273112
```

可能为中文电报编码，使用[千千秀字](https://www.qqxiuzi.cn/bianma/dianbao.php)进行解码，得到：

```text
公正诚信和谐公正友善敬业公正平等公正和谐法治自由公正公正法治友善平等自由富强平等友善敬业自由法治法治文明和谐自由公正友善爱国公正自由平等诚信平等公正民主公正法治和谐和谐平等诚信平等和谐富强公正公正平等诚信平等法治自由和谐和谐公正友善公正公正平等公正法治法治文明和谐自由法治富强公正爱国和谐法治法治友善法治
```

搜索发现`核心价值观编码`，使用[核心价值观编码 - Bugku CTF平台](https://ctf.bugku.com/tool/cvecode)解码，即得Flag。

## 2. Flag

moectf{@_Gr4nd_ag3_0f_t3legr4ph7}

# 十七. 霜雪千年

## 1. 解题思路

下载附件flag.zip，尝试解压，发现压缩包是千层嵌套的。于是~~手动打开千层压缩包（别笑，真是手动开的）~~得到flag.txt。打开文件，其内只有一句话：“愿今生再相见”，以及大段空白字符。

> *错误方向：尝试使用whitespace解码*

*联系标题“霜雪千年”以及空白字符，想到SNOW隐写。*

搜素`霜雪千年`可获得信息：`《霜雪千年》是COP于2015年10月14日上传至哔哩哔哩的VOCALOID原创歌曲`

注意到，题目描述为“梨花香，缠着衣角掠过熙攘~”，是歌词第一句；txt文档中有“愿今生再相见”，是歌词最后一句的前半句，所以密码可能为后半句：“消融你眉间悲戚霜雪”。

尝试使用stegsnow解码：

```bash
stegsnow -C -p "消融你眉间悲戚霜雪" flag.txt
```
或无解压：
```bash
stegsnow -p "消融你眉间悲戚霜雪" flag.txt
```

第二句得到了Flag。

## 2. Flag

moectf{Sn0w_@lwa7s_h0lds_5tor7_w1thin}

---

本文完篇于2026.09.24