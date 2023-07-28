---
link: https://blog.csdn.net/qq_43688472/article/details/122722945
title: mysql8设置表名大小写不敏感（lower_case_table_names=1）初始化才可以
description: mysql8设置表名大小写不敏感（lower_case_table_names=1）查看MySQL官方文档，有记录：lower_case_table_names can only be configured when initializing the server. Changing the lower_case_table_names setting after the server is initialized is prohibited.只有在初始化的时候设置 lower_case_table_mysql8 lower_case_table_names=1
keywords: mysql8 lower_case_table_names=1
author: 墨卿风竹 Csdn认证博客专家 Csdn认证企业博客 码龄5年 暂无认证
date: 2022-01-27T12:03:12.000Z
publisher: null
stats: paragraph=9 sentences=9, words=35
---
mysql8设置表名大小写不敏感（lower_case_table_names=1）
![](https://img-blog.csdnimg.cn/01b809d455ab46ba936cb3b085d5d15c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5aKo5Y2_6aOO56u5,size_20,color_FFFFFF,t_70,g_se,x_16)

查看MySQL官方文档，有记录：

lower_case_table_names can only be configured when initializing the server. Changing the lower_case_table_names setting after the server is initialized is prohibited.

只有在初始化的时候设置 lower_case_table_names=1才有效，比如：

–initialize --lower-case-table-names=1

## <a name="_12">;</a> 注意：

mysql8.0 要求我们不能在initialize之后再更改 lower_case_table_names 的值，也就是说，再通过更改 my.cnf 文件是不管用的。

所以...

重装！不用再试了，试过很多方法最终还是重装。。。
