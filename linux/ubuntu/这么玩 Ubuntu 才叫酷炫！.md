---
link: https://juejin.cn/post/6999500110929330207
title: 这么玩 Ubuntu 才叫酷炫！
description: 1. 前言 我们在工作或学习过程中，有许多时候会接触到 Linux，然后便想要自己亲自试试这个系统，但苦于没有系统学习过，所以想要打造自己的工作环境难免会遇到许多困难。为了防止大家踩我踩过的坑，才有了
keywords: Linux,Ubuntu
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2021-08-23T05:56:35.000Z
publisher: 稀土掘金
stats: paragraph=297 sentences=168, words=773
---
> **这是我参与 8 月更文挑战的第 23 天，活动详情查看：[8 月更文挑战](https://juejin.cn/post/6987962113788493831 "https://juejin.cn/post/6987962113788493831")**

## 1. 前言

我们在工作或学习过程中，有许多时候会接触到 Linux，然后便想要自己亲自试试这个系统，但苦于没有系统学习过，所以想要打造自己的工作环境难免会遇到许多困难。为了防止大家踩我踩过的坑，才有了这篇博客。本博客将主要从 **系统设置、常用软件安装、开发环境搭建、系统美化** 几个方面着手，手把手教你如何从零打造自己的 Ubuntu （基于 Ubuntu 20.04.4 LTS ）工作环境。

先展示下我自己的工作环境：

* 桌面

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b2794e17acc94b4ea6ae8d0c4c7065a2~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

* 程序坞

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cb2713998d8341d3baac010e2a710458~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

* 文件

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f86f4811b51a4e5e9b79a9dc694f2f63~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

## 2. 系统设置

### 2.1 换镜像源

即将官方自带的源换成国内镜像源，如阿里、华为、网易等；

打开 **软件和更新** ，然后选择下载自 **其他站点**，选择你要设置的镜像源站点即可，也可以点击右侧选择最佳服务器，会根据你所处地区然后自动选择网速最快的；

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fef3a3c2f5dd4c6b905d82104be62d88~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

### 2.2 安装更新

换源之后，执行如下命令用于更新系统；

```shell
sudo apt update
sudo apt-get  upgrade
复制代码
```

### 2.3 使用本地时区

使用双系统时，往往两者时间会不正确，可以通过如下命令让 Ubuntu 使用本地时区；

```shell
timedatectl set-local-rtc 1
复制代码
```

### 2.4 卸载自带的"无用"软件

此处说的"无用"软件，因人而异，笔者只是提供建议，建议卸载软件列表如下（可能还有些没咋用的没列出，读者可以自行决定是否卸载，卸载之后如果有用到，还可以重新安装）；

* thunderbird 自带邮件
* rhythmbox 音乐
* gnome-mahjongg 对对碰游戏
* gnome-mines 扫雷
* gnome-sudoku 数独
* aisleriot 纸牌
* simple-scan 扫描器
* onboard 屏幕键盘
* brasero 光盘可怜工具
* cheese 茄子相机
* libreoffice 套件，用 WPS 代替

## 3. 常用软件安装

### 3.1 输入法

* 搜狗输入法 for Linux

软件由搜狗输入法团队和 Ubuntu Kylin 团队共同开发，能够满足我们日常输入需求；

* 安装

此处推荐下载安装包后安装，[下载地址](https://link.juejin.cn?target=https%3A%2F%2Fpinyin.sogou.com%2Flinux%2F%3Fr%3Dpinyin "https://pinyin.sogou.com/linux/?r=pinyin")，下载后的安装包如下；

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ffd18008fef34d7d98ed4196c1d56e80~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

然后在当前文件夹下打开终端，输入如下命令安装即可；

```shell
sudo dpkg -i sogoupinyin_2.3.1.0112_amd64.deb
复制代码
```

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/eff150d9ecc34221999afa87cf394a34~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

接着到 **设置 - > 区域和语言** 点击已安装的语言；

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b19ddef8b55a429aa22754ad3603f402~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

把键盘输入法设置为 **fcitx**，然后应用到整个系统即可，然后注销或者重启即可使用； ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0f9e5c2351a94737a80abad47d596070~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

* 注意

当安装时，可能会报依赖相关问题，可以在执行上述命令后，在终端中输入如下命令修复依赖安装，然后再次执行上面的安装命令即可；

```shell
sudo apt-get --fix-broken install
复制代码
```

### 3.2 浏览器

* Chrome 说到浏览器，虽然 Ubuntu 中预装了 Firefox，但笔者更习惯用 Chrome ，作为全球范围内份额最高的浏览器，自然不用说啥，其实微软新出的 Edge 浏览器也很不错，只是目前还没有支持 Linux，后续可能会进一步支持。
* 安装

建议使用 deb 安装包安装，先 [下载](https://link.juejin.cn?target=https%3A%2F%2Fwww.google.cn%2Fintl%2Fzh-CN%2Fchrome%2F "https://www.google.cn/intl/zh-CN/chrome/") 安装包，然后在本地终端使用如下命令进行安装；

```shell
sudo dpkg -i google-chrome-stable_current_amd64.deb
复制代码
```

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a5dd0203eb7d4456bb0e66ababef0a15~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

* 成品

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/16a9ced818024a48b4089f88fdfebd75~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

### 3.3 QQ

去官网 [下载](https://link.juejin.cn?target=https%3A%2F%2Fim.qq.com%2Flinuxqq%2Fdownload.html "https://im.qq.com/linuxqq/download.html") 对应安装包，然后在终端使用如下命令安装；

```shell
sudo dpkg -i linuxqq_2.0.0-b2-1082_amd64.deb
复制代码
```

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f1dfd47b76ed4a078a05b0b9d7e3e861~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

* 成品

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/788854529a6b49258ae564b4903bcfde~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

### 3.4 微信

* 安装

此处推荐使用网页版即可，wine 版虽然网上也有，但有许多 bug，所以推荐使用网页版；

* 创建快捷方式

打开网页版微信，然后打开浏览器设置中的 **更多工具 -> 创建快捷方式** 即可，然后在桌面就会出现快捷方式，信任即可；

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/bd88141eb44e4ff8a11f570754bce4fd~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

* 成品

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/bbf152188ec949e2ad241078c34f7c7f~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

### 3.5 音乐播放器

* 网易云音乐

作为网易旗下的音乐软件，虽然版权问题流失了许多用户，但不影响它越来越受广大用户喜欢。而我们在学习工作之余听听音乐，放松下调整状体也是极好的。

* 安装

[下载](https://link.juejin.cn?target=https%3A%2F%2Fmusic.163.com%2F%23%2Fdownload "https://music.163.com/#/download") Linux 版本安装包，然后使用如下命令在终端中安装即可；

```shell
sudo dpkg -i netease-cloud-music_1.2.1_amd64_ubuntu_20190428.deb
复制代码
```

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a888e2308b2e4111bafa1152ccdc9ef3~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

* 成品

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8ecf138f61d84b9eae1fbaa993f79a48~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

### 3.6 截图工具

* flameshot flameshot，能够用于标注、模糊，同时还支持上传到 imgur 图床的新式截图工具；
* 安装

```shell
sudo apt install flameshot
复制代码
```

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2b831fa06d2541e6814744d27ac4f299~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

* 设置快捷键

依次进入 **设置 -> 设备 -> 键盘** ，然后滑到最底端会有一个 `+` 按钮，点击 `+` 添加自定义快捷键，并设置名称和命令，然后点击右上角添加就可以设置自己想要的截图快捷键，比如我设置的是 `Ctrl + Alt + A`，如下图所示；

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/06c5cf4c8f4a494d813157a95442aa50~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

* 成品

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/78a19a3402644215a0fb35dd213f12bf~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

### 3.7 图片编辑器

* Gimp

GIMP是 GNU Image Manipulation Program（GNU图像处理程序）的缩写，它是 Peter Mattis 和 Spencer Kimhall 开发的免费照片和图像处理和创作工具，功能十分强大。支持多种图像处理工具、全通道、多级撤销操作恢复旧貌与映像修饰等功能。支持数目众多的效果插件（plug-ins），完全可以与 Windows 平台下著名的图像处理软件 Photoshop 媲美；

* 安装

直接使用如下命令安装即可；

```shell
sudo apt-get install gimp
复制代码
```

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b109af85a34f4bfb9d4104c4b995a920~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

* 成品

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8b85c96a73554feabb342f5bd1e28812~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

### 3.8 文字处理

* WPS

Ubuntu 中自带了 LibreOffice 办公套件，但此处更推荐使用 WPS，更加符合我们国人的使用习惯。

* 安装

先下载安装包，然后依然在本地终端使用如下命令进行安装；

```shell
sudo dpkg -i wps-office_11.1.0.9505_amd64.deb
复制代码
```

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c1a7beab642a49ca82f032ce9eda2862~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

* 成品

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2646128d84654719babb7eaeb2a7e501~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

### 3.9 Markdown 编辑器

* Typora

作为一款极简的 Markdown 编辑器，合并了写作和预览。支持表格、代码编辑，拖拽插图等，非常好用；喜欢它的原因也就是：美观、免费，而且跨平台；

* 安装

依次使用如下命令即可安装；

```shell
wget -qO - https://typora.io/linux/public-key.asc | sudo apt-key add -

sudo add-apt-repository 'deb https://typora.io/linux ./'

sudo apt-get update

sudo apt-get install typora
复制代码
```

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a2b45f25623048ef9494b1bfb6399039~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

* 成品

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2a4c86e2853c437aa8952e95d5b52476~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

### 3.10 文档阅读器

* Okular

可以说是 Linux 中最好的 PDF 阅读器，而且还支持常用的 CHM、EPub 等格式文档的查看；

* 安装

直接在终端中使用如下命令安装即可；

```shell
sudo apt-get install okular
复制代码
```

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/61e02d31e91442ce8f27853da5a96c31~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

* 成品

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/489297ede28345418693a727125fa284~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

### 3.11 思维导图

* XMind

做事或者写作之前，都喜欢先做一个总体的思维导图，然后根据思维导图去细化每一部分，而 XMind 正是这么一款工具；

* 安装

去 [下载](https://link.juejin.cn?target=https%3A%2F%2Fwww.xmind.cn%2Fdownload%2F "https://www.xmind.cn/download/") 安装包之后，在终端中使用如下命令行安装即可；

```shell
sudo dpkg -i XMind-2020-for-Linux-amd-64bit-10.1.2-202004142327.deb
复制代码
```

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5248b2720e2a49b1a275a073fe9a6436~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

* 成品![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/03cbef1ef78b4b1ab2585b32638b925a~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

### 3.12 多媒体播放器

* VLC

支持众多音频与视频解码器及文件格式，并支持 DVD 影音光盘，VCD 影音光盘及各类流式协议。也能作为 unicast 或 multicast 的流式服务器在 IPv4 或 IPv6 的高速网络连接下使用。融合了 FFmpeg 计划的解码器与 libdvdcss 程序库使其有播放多媒体文件及加密 DVD 影碟的功能，是 Linux 中多媒体播放器的不二之选；

* 安装

直接在终端中使用如下命令安装即可；

```shell
sudo snap install vlc
复制代码
```

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f434d91bac00482c967ad9c816c0927e~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

* 成品

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/20f72befdcc245c8a8c4ad22302fc376~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

### 3.13 录屏软件

* Kazam

小巧而强大，易安装，即可选择区域录制，也可选择全屏录制，同时还兼具截图功能；

* 安装

直接在终端中使用如下命令安装即可：

```shell
sudo apt install kazam
复制代码
```

* 成品

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6e997abbcbb04f5cab8b190d9b30e4d8~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

### 3.14 视频编辑软件

* OpenShot

OpenShot 是 Linux 上的一个多用途视频编辑器，可以帮助你创建具有过渡和效果的视频。

* 安装

直接在终端中使用如下命令安装即可；

```shell
sudo apt install openshot
复制代码
```

* 成品

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f01be4582ed342d08a887d89502f621e~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

### 3.15 下载工具

* uGet

Uget（原名：Urlgfe）是一个基于 GTK+ 编写的自由和开放源码的下载管理器 ，跨平台（Windows和GNU / Linux的），支持断点续传和分类下载 ，uGet 同时 具备 多线程下载能力；

* 安装

```shell
sudo apt-get install uget
复制代码
```

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9427a50ae3e348788aadc0236633b414~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

* 成品

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9df33d98cdd0440fba2287957b89b2b1~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

## 4. 开发环境搭建

### 4.1 git 安装

直接在终端中使用如下命令安装即可；

```shell
sudo apt install git
复制代码
```

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cfe54fd7385e453f85b74750b4c3b3b9~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

### 4.2 Node.js 安装

* 安装

从淘宝镜像源 [下载](https://link.juejin.cn?target=https%3A%2F%2Fnpm.taobao.org%2Fmirrors%2Fnode%2F "https://npm.taobao.org/mirrors/node/") 对应版本压缩包，然后解压到你要存储的路径，比如我的是 `/home/cunyu/soft/node-v12.16.2`；

* 配置

安装（解压）好后，在 `/etc/profile` 配置文件中配置；

```profile
export NODEJS_HOME=/home/cunyu/soft/node-v12.16.2
export PATH=$NODEJS_HOME/bin:$PATH
复制代码
```

* 换淘宝镜像源

```shell
npm config set registry http://registry.npm.taobao.org/
复制代码
```

* 验证

```shell
node -v
npm -v
复制代码
```

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ab8c88491a4b47e681752e48ba3e506d~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

### 4.3 JDK 安装

* 安装

先去 [下载](https://link.juejin.cn?target=https%3A%2F%2Fwww.oracle.com%2Fjava%2Ftechnologies%2Fjavase-downloads.html "https://www.oracle.com/java/technologies/javase-downloads.html") 要安装的 JDK 版本，然后解压到本地，移到你要存放的位置；

```shell
tar -zxvf jdk-11.0.7_linux-x64_bin.tar.gz
复制代码
```

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6350ed91d4e44024b10fc523f1579b81~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3d2672f8257e48c9a2fc3121eb6c7490~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

* 配置

打开配置文件 `/etc/profile`，然后加入如下内容；

```profile
export JAVA_HOME=/home/cunyu/Soft/jdk11.0.7
export PATH=$JAVA_HOME:$PATH
复制代码
```

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/135cdca1125a491a8452e22e98233423~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/eb5fe0eb8cbb4c5e8b2a05a61d9ea37d~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

* 验证

安装和配置之后，在终端中输入如下命令进行验证是否成功；

```shell
查看版本
java -version
编译
java
执行
javac
复制代码
```

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/64a2bda164ca42f2872b58f731dad88f~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

### 4.4 MiniConda 安装

* 安装

直接去 [下载](https://link.juejin.cn?target=https%3A%2F%2Fdocs.conda.io%2Fen%2Flatest%2Fminiconda.html "https://docs.conda.io/en/latest/miniconda.html") 对应版本安装包，此处以 3.7 版本为例，然后在终端中对齐赋予执行权限后执行；

```shell
chmod +x Miniconda3-latest-Linux-x86_64.sh

sh Miniconda3-latest-Linux-x86_64.sh
复制代码
```

执行后会让你阅读相关权限，然后输入 yes 同意，然后会让你指定安装目录，默认就直接回车就好，最后等安装好即可；

* 配置

打开 `~/.bashrc` 文件，在其中加入如下 内容；

```ini

export  PATH="/home/cunyu/miniconda3/bin:"$PATH
复制代码
```

* 验证

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ff062c05a1c54353bb957078667b7ce4~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

### 4.5 MySQL 安装

#### 最新版本

* 安装

```shell
sudo apt-get install mysql-server
复制代码
```

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b8e532e78b554fb2b774165349b7bcf2~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

* 设置 root 用户密码

上述安装过程结束后，直接登录，此时 root 用户没有设密码，需要输入密码时直接回车；

```shell
myslq -u root -p
复制代码
```

登录后，使用如下命令来修改 root 用户密码，退出后再次登录输入你修改的密码即可；

```shell
SET PASSWORD FOR 'root'@localhost = PASSWORD('你要设置的密码');
复制代码
```

#### 指定版本

* 通过如下命令查看可安装的版本；

```shell
apt-cache search mysql | grep mysql-server
复制代码
```

* 然后利用如下命令安装即可；

```shell
sudo apt install mysql-server-5.7
复制代码
```

#### 注意

登陆时如果需要加上 `sudo` 才能登陆，则通过 `sudo` 登陆后，使用如下命令更新，即可退出后不用再使用 `sudo` 登陆

```sql
use mysql;
select User,Host,plugin from user;
update user set Plugin='mysql_native_password'
复制代码
```

### 4.6 VS Code 安装

* 安装

直接去 [下载](https://link.juejin.cn?target=https%3A%2F%2Fcode.visualstudio.com%2F "https://code.visualstudio.com/") 对应安装包，然后在终端使用如下命令安装；

```shell
sudo dpkg -i code_1.44.2-1587059832_amd64.deb
复制代码
```

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7b873fe05d50428c8a1ae48869264ddb~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

* 成品

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/062ad8393f4543ed8d2db7dbad35c810~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

### 4.7 IntelliJ IDEA 安装

* 安装

从官网 [下载](https://link.juejin.cn?target=https%3A%2F%2Fwww.jetbrains.com%2Fidea%2Fdownload%2F%23section%3Dlinux "https://www.jetbrains.com/idea/download/#section=linux") 对应安装包，然后将其解压；

```shell
tar -zxvf ideaIU-2020.1.tar.gz
复制代码
```

进入 **IDEA/bin** 目录下，执行 `idea.sh` 即可；

* 生成快捷方式

打开 IDEA 后，进入 **Configure -> Create Desktop Entry**，然后输入 root 用户密码即可；

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/88ed92df94c54f3f831a307c7b684cf2~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

### 4.8 Pycharm 安装

* 安装

从官网 [下载](https://link.juejin.cn?target=https%3A%2F%2Fwww.jetbrains.com%2Fpycharm%2Fdownload%2F%23section%3Dlinux "https://www.jetbrains.com/pycharm/download/#section=linux") 对应安装包，然后将其解压；

```shell
tar -zxvf pycharm-professional-2020.1.tar.gz
复制代码
```

进入 **Pycharm/bin** 目录下，执行 `pycharm.sh` 即可；

* 生成快捷方式

打开 Pycharm 后，进入 **Configure -> Create Desktop Entry**，然后输入 root 用户密码即可；

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a63707b05c4e4b3dad9ef295b13ab0e9~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

## 5. 系统美化

### 5.1 Grub 美化

* 对于安装双系统或多系统的读者，想要自己的启动页面变得好看一点，那么你一定不能错过。

到 [这里](https://link.juejin.cn?target=https%3A%2F%2Fwww.gnome-look.org%2Fbrowse%2Fcat%2F109%2Ford%2Flatest%2F "https://www.gnome-look.org/browse/cat/109/ord/latest/") 去选择自己喜欢的主题，然后进行安装，这里笔者选择的是 [Grub-theme-vimix](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fvinceliuice%2Fgrub2-themes "https://github.com/vinceliuice/grub2-themes")；

* 安装

把 Grub-theme-vimix 克隆到本地，然后使用如下命令进行安装即可；

```shell
sudo ./install.sh -t -2 -b
复制代码
```

### 5.2 锁屏界面美化

直接修改相关配置文件： `/etc/alternatives/gdm3.css` ，然后修改代码中的 `#lockDialogGroup` 部分如下即可；

```css
#lockDialogGroup {
  background: #2c001e
  url(file:///home/cunyu/imgs/lock.png);
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center;
}
复制代码
```

### 5.3 美化工具安装

美化之前，需要安装 gnome-tweak-tool，安装命令如下；

```shell
sudo apt-get install gnome-tweak-tool
复制代码
```

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4f78ad83cd344d52b1f7507874d4f279~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

### 5.4 主题安装

* 下载

推荐使用这套，macos 风格的主题，[下载链接](https://link.juejin.cn?target=https%3A%2F%2Fwww.gnome-look.org%2Fp%2F1275087%2F "https://www.gnome-look.org/p/1275087/")；

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a4f9678be51247b888717cfb29bf5c2e~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

* 安装

把下载好的压缩包解压之后，移动到 `/usr/share/themes` 目录下即可；

### 5.5 图标安装

* 下载

推荐这款图标，[下载链接](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fkeeferrourke%2Fla-capitaine-icon-theme "https://github.com/keeferrourke/la-capitaine-icon-theme")；

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6f7bd477c8d3419bb01814d73d9a2e31~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

* 安装

把下载好的压缩包解压之后，移动到 `/usr/share/icons` 目录下即可；

### 5.6 终端安装

* 安装

Ubuntu 自带终端已经很强大，但我更推荐使用 ZSH；

在终端中使用如下命令安装即可；

```shell
sudo apt install zsh
复制代码
```

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f489b37145974603b6977b297c2fff5b~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

* 设为默认 Shell

```shell
chsh -s /usr/bin/zsh
复制代码
```

* 美化

安装 [oh-my-zsh](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Frobbyrussell%2Foh-my-zsh "https://github.com/robbyrussell/oh-my-zsh")，通过如下任一命令安装；

```shell
via crul
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
via wget
sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
复制代码
```

也可以通过如下命令安装；

```shell
git clone git://github.com/robbyrussell/oh-my-zsh.git  ~/.oh-my-zsh
cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc
复制代码
```

安装好之后，对 zsh 进行配置，打开 `~/.zshrc` 配置文件，进行配置；

进入 [主题列表](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fohmyzsh%2Fohmyzsh%2Fwiki%2FThemes "https://github.com/ohmyzsh/ohmyzsh/wiki/Themes")，挑选自己喜欢的主题，然后在上述配置中改成你喜欢的主题，比如我的主题是 Powerlevel10k，使用如下命令进行下载，然后在 `.zshrc` 中配置 `ZSH_THEME="powerlevel10k/powerlevel10k"`。

```shell
git clone --depth=1 https://gitee.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/themes/powerlevel10k
复制代码
```

使用该主题时会乱码，所以需要安装字体，到[字体下载链接](https://link.juejin.cn?target=https%3A%2F%2Fwww.nerdfonts.com%2Ffont-downloads "https://www.nerdfonts.com/font-downloads")选择喜欢的字体进行下载，然后解压后双击安装即可，然后到终端的设置中将字体设为刚才安装的字体之一，然后重启终端即可。

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/77b971aae28447f3a16def8a057b6b3d~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

* 插件安装
  - `zsh-syntax-highlighting` ：语法高亮；

```shell
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $ZSH_CUSTOM/plugins/zsh-syntax-highlighting
复制代码
```

  - `zsh-autosuggestions` ：自动补全；

```shell
git clone git://github.com/zsh-users/zsh-autosuggestions $ZSH_CUSTOM/plugins/zsh-autosuggestions
复制代码
```

```ini

ZSH_THEME="powerlevel10k/powerlevel10k"

git clone git://github.com/zsh-users/zsh-autosuggestions ~/.oh-my-zsh

plugins=(
    git zsh-syntax-highlighting zsh-autosuggestions extract z
)
复制代码
```

## 6. 总结

至此，Ubuntu18.04 的安装结束，接下来你就可以尽情的捣鼓安装好的系统了 QAQ。
