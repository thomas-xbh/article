---
link: https://juejin.cn/post/6844904134345228301
title: CentOS 7 安装 Nginx
description: 最近，在公司经常会进行项目的部署，但是服务器环境都是导师已经搭建好了的，我就是将项目文件放到特定目录。于是，周末在家就进行了 Nginx 的安装学习。之前，在 Windows 上使用过 Nginx，但是在 Linux 环境下 Ngnix 的安装和在 Windows 环境下安装是…
keywords: Linux
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2020-04-22T01:08:19.000Z
publisher: 稀土掘金
stats: paragraph=33 sentences=15, words=150
---
最近，在公司经常会进行项目的部署，但是服务器环境都是导师已经搭建好了的，我就是将项目文件放到特定目录。于是，周末在家就进行了 Nginx 的安装学习。之前，在 Windows 上使用过 Nginx，但是在 Linux 环境下 Ngnix 的安装和在 Windows 环境下安装是有一定区别的。这次进行在 Linux 环境下使用源码包的方式安装 Nginx 遇到了不少的问题，但查阅了一些资料也解决了。希望以下的笔记能帮助你们避开这些问题。

## Linux 的两种安装方式

首先，介绍一下 Linux 的安装方式，可以是 yum 安装，也可以是源码包安装。

* yum 安装：简单方便，不易出错。
* 源码包安装：有点繁琐，但是服务性能好。

yum 安装 nginx 非常简单，就输入一条命令即可。

```
$ sudo yum -y install nginx
$ sudo yum remove nginx
```

使用 yum 进行 Nginx 安装时，Nginx 配置文件在 `/etc/nginx` 目录下。

```
$ sudo systemctl <span class="hljs-built_in">enable</span> nginx
$ sudo service nginx start
$ sudo service nginx stop
$ sudo service nginx restart
$ sudo service nginx reload
```

Nginx 源码包安装方式步骤比较繁琐，并且需要提前安装一些 Nginx 依赖库。

```
$ sudo yum -y install gcc gcc-c++
```

```
$ sudo yum -y install pcre pcre-devel
```

```

$ sudo yum -y install zlib zlib-devel
```

```

$ sudo yum -y install openssl openssl-devel
```

以上安装完成后，进行 nginx 安装。

将准备好的 `nginx-1.11.5.tar.gz` 包，拷贝至 `/usr/local/nginx` 目录下（一般习惯在此目录下进行安装）进行解压缩。

```
$ sudo tar -zxvf  nginx-1.11.5.tar.gz
```

在完成解压缩后，进入 `nginx-1.11.5` 目录进行源码编译安装。

```
$  <span class="hljs-built_in">cd</span> nginx-1.11.5
$ ./configure --prefix=/usr/<span class="hljs-built_in">local</span>/nginx

```

如果前面的依赖库都安装成功后，执行 `./configure --prefix=/usr/local/nginx` 命令会显示一些环境信息。如果出现错误，一般是依赖库没有安装完成，可按照错误提示信息进行所缺的依赖库安装。

进行源码编译并安装 nginx

```
$ make
$ make install
```

源码包安装与 yum 安装的 nginx 服务操作命令也不同。

* 启动服务

```
$ /usr/<span class="hljs-built_in">local</span>/nginx/sbin/nginx
```

* 重新加载服务

```
$ /usr/<span class="hljs-built_in">local</span>/nginx/sbin/nginx <span class="hljs-_">-s</span> reload
```

* 停止服务

```
$ /usr/<span class="hljs-built_in">local</span>/nginx/sbin/nginx <span class="hljs-_">-s</span> stop
```

查看 nginx 服务进程

```
$ ps -ef | grep nginx
```
