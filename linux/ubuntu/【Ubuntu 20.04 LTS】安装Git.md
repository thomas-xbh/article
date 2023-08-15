---
link: https://blog.csdn.net/qq_31635851/article/details/123333398
title: 【Ubuntu 20.04 LTS】安装Git
description: 文章目录使用Apt安装Git使用源码安装Git本文将介绍两种方式来在Ubuntu上安装Git，第一种使用Apt安装Git，这种安装的优点就是简单，只需要要一道命令就可以完成安装，另外一种方式是使用源码安装Git，这种方式的优点就是可以安装最新版本的Git，要使用哪一种大家可以根据自己的需要进行选择。使用Apt安装GitGit软件包包含在Ubuntu的默认存储库中，可以使用apt软件包管理器进行安装。这是在Ubuntu上安装Git的最方便，最简单的方法。sudo apt updatesudo apt_乌班图git的默认安装地址
keywords: 乌班图git的默认安装地址
author: 猫巳 Csdn认证博客专家 Csdn认证企业博客 码龄8年 Java领域优质创作者
date: 2022-03-07T09:06:56.000Z
publisher: null
stats: paragraph=33 sentences=17, words=88
---
### 文章目录

* [使用Apt安装Git](#aptgit-3)
* [使用源码安装Git](#git-17)
* [配置全局名称和邮箱](#-55)
* [生成ssh-keygen](#sshkeygen-66)

本文将介绍两种方式来在Ubuntu上安装Git，第一种使用Apt安装Git，这种安装的优点就是简单，只需要要一道命令就可以完成安装，另外一种方式是使用源码安装Git，这种方式的优点就是可以安装最新版本的Git，要使用哪一种大家可以根据自己的需要进行选择。

# 使用Apt安装Git

`Git`软件包包含在 `Ubuntu`的默认存储库中，可以使用 `apt`软件包管理器进行安装。这是在 `Ubuntu`上安装 `Git`的最方便，最简单的方法。

```
sudo apt update
sudo apt install git
```

查看安装版本

```
git --version
```

![](https://img-blog.csdnimg.cn/eae45a9f39ff4b26bfebe202bfeabc34.png)

# 使用源码安装Git

使用源码安装需要先安装依赖包

```
sudo apt update
sudo apt install dh-autoreconf libcurl4-gnutls-dev libexpat1-dev make gettext libz-dev libssl-dev libghc-zlib-dev
```

然后打开我们的浏览器，输入以下地址

```bash
https://github.com/git/git/tags
```

![](https://img-blog.csdnimg.cn/f633c4eac938462eb3430c976470b9b5.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA54yr5bez,size_20,color_FFFFFF,t_70,g_se,x_16)
点击下载或通过 `wget`命令下载

```bash
wget -c https://github.com/git/git/archive/refs/tags/v2.35.1.tar.gz -O - | sudo tar -xz -C /usr/src
```

进入到下载的目录

解压

```bash
cd /usr/src
tar -zxf git-2.35.1.tar.gz
```

安装

```bash
cd git-*
sudo make prefix=/usr/local all
sudo make prefix=/usr/local install
```

等待安装完成后，验证版本

```bash
git --version
```

# 配置全局名称和邮箱

```bash
git config --global user.name "你的名字"
git config --global user.email "你的邮箱"
```

查看

```bash
git config --list
```

# 生成ssh-keygen

```bash
cd ~/ && ssh-keygen -t rsa -C "你的邮箱"
```

看自己需要，第一个是生成路径，第二个是密码，第三确认密码，不设置密码一路回车即可。

查看

```bash
cd ~/.ssh
ls
```

将其中的公钥文件（ `id_rsa.pub`）内容复制到代码托管网站的ssh密钥管理中就可以免密推拉代码了
