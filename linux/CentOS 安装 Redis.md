---
link: null
title: CentOS 安装 Redis-腾讯云开发者社区-腾讯云
description: 修改默认密码，查找 requirepass foobared 将 foobared 修改为你的密码
keywords: 云数据库 Redis
author: null
date: 2022-03-28T07:59:24.000Z
publisher: null
stats: paragraph=14 sentences=4, words=15
---
安装完毕后需要启动

默认端口一般是 6379 ，但也可以改成你想要的端口。

打开配置文件

在这里插入图片描述

修改默认端口，查找 port 6379 修改为相应端口即可

修改默认密码，查找 requirepass foobared 将 foobared 修改为你的密码

使用配置文件启动

使用端口登陆

在这里插入图片描述

输入刚才输入的密码

在这里插入图片描述

使用命令关闭

杀掉进程

* https://www.cnblogs.com/rslai/p/8249812.html
