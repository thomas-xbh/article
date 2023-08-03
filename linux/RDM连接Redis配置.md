---
link: https://blog.csdn.net/LeftStrang/article/details/121490667
title: RDM连接Redis配置
description: RDM连接Redis遇到“无法连接Redis服务器”的情况下怎么解决_rdm redis
keywords: rdm redis
author: Leftstrange Csdn认证博客专家 Csdn认证企业博客 码龄12年 上海子归智能技术有限公司
date: 2021-11-23T05:13:13.000Z
publisher: null
stats: paragraph=8 sentences=27, words=65
---
1. RDM官网下载地址（官网最新版现在是收费的）：[https://rdm.dev/](https://rdm.dev/ "https://rdm.dev/")
2. 也可以去网上下载破解版，网上破解码有很多。

安装RDM，直接下一步安装到底

![](https://img-blog.csdnimg.cn/03e278207f844c6ab22bb5890f136ed8.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAbGVmdHN0cmFuZ2U=,size_14,color_FFFFFF,t_70,g_se,x_16)

安装完成之后配置连接

![](https://img-blog.csdnimg.cn/5642748985454377b19cf790cf072e47.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAbGVmdHN0cmFuZ2U=,size_20,color_FFFFFF,t_70,g_se,x_16)

会遇到"无法连接Redis服务器"的问题，解决问下如下：

进行Redis配置，Redis配置默认是本地连接所以要修改配置文件：

1. Linux 版本修改"redis.conf"文件，Windows系统修改"redis.windows.conf" 文件
2. 修改内容(修改前关闭所以Redis相关程序)：
  1. 老版本Redis把"bind 127.0.0.1 " 改为 "# bind 127.0.0.1 "
  2. 新版本Redis把bind 127.0.0.1 " 改为 "bind 0.0.0.0 "
  3. 把"protected-mode yes" 改为 "protected-mode no"
  4. 添加"daemonize yes"
  5.

```
&#x914D;&#x7F6E;&#x5185;&#x5BB9;
# bind 127.0.0.1
bind 0.0.0.0
protected-mode no
daemonize yes
```
3. 打开redis-server.exe
4. 打开redis-cli（ **注：特殊情况下命令执行虽然是OK但是还是无法正常连接的话，redis-cli使用管理员运行**），执行下面命令："config set requirepass 123456" 命令为设置访问密码，只有设置了访问密码之后RDM才能远程连接。
