---
link: https://blog.csdn.net/qq_38388811/article/details/109148820
title: CentOS8安装Mysql提示Error: Unable to find a match
description: yum -y install mysql-community-serverLast metadata expiration check: 0:13:40 ago on Sun 26 Apr 2020 11:20:57 AM CST.No match for argument: mysql-community-serverError: Unable to find a match: mysql-community-server解决：先执行：yum module disable mysql再执行.
keywords: CentOS8安装Mysql提示Error: Unable to find a match
author: 调皮李小怪 Csdn认证博客专家 Csdn认证企业博客 码龄6年 高校学生
date: 2023-06-12T02:25:41.000Z
publisher: null
stats: paragraph=4 sentences=3, words=37
---
```
yum -y install mysql-community-server
Last metadata expiration check: 0:13:40 ago on Sun 26 Apr 2020 11:20:57 AM CST.

No match for argument: mysql-community-server
Error: Unable to find a match: mysql-community-server
```

解决：
先执行： **yum module disable mysql**
再执行： **yum -y install mysql-community-server**

![](https://img-blog.csdnimg.cn/20201018193057846.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4Mzg4ODEx,size_16,color_FFFFFF,t_70)
