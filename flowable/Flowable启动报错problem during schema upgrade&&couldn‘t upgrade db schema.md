---
link: https://blog.csdn.net/m0_50176078/article/details/120199403
title: Flowable启动报错problem during schema upgrade&&couldn‘t upgrade db schema:
description: Flowable启动报错problem during schema upgrade&&couldn‘t upgrade db schema:_flowable启动报错
keywords: flowable启动报错
author: Sowler Csdn认证博客专家 Csdn认证企业博客 码龄3年 暂无认证
date: 2022-09-20T08:56:16.000Z
publisher: null
stats: paragraph=11 sentences=39, words=119
---
**1.错误信息**

```java
problem during schema upgrade, statement alter table ACT_RU_VARIABLE add column SCOPE_ID_ varchar(255)
java.sql.SQLSyntaxErrorException: Duplicate column name 'SCOPE_ID_'
	at com.mysql.cj.jdbc.exceptions.SQLError.createSQLException(SQLError.java:120)
	at com.mysql.cj.jdbc.exceptions.SQLError.createSQLException(SQLError.java:97)
	at com.mysql.cj.jdbc.exceptions.SQLExceptionsMapping.translateException(SQLExceptionsMapping.java:122)
```

```java
Caused by: org.springframework.beans.factory.BeanCreationException: Error creating bean with name
'flowableAppEngine': FactoryBean threw exception on object creation; nested exception is org.flowable
.common.engine.api.FlowableException: couldn't upgrade db schema: alter table ACT_RU_VARIABLE
add column SCOPE_ID_ varchar(255)
```

**原因**
启动Flowable的时候数据库里面已经有flowable相关的数据表。但是项目还是去创建相关的数据表。原因是flowable运行后生成的表名都是大写。而MySQL检索的时候是区分大小写的。然后执行时发现2张表名一样，导致出现问题。
**2.Mysql设置lower_case_table_names参数**
lower_case_table_names 是mysql设置大小写是否区分和比较的参数。lower_case_table_names 默认为 0

```java
0：表名字是存储为给定的大小，区分大小写的。
1：表名字存储在磁盘是小写的，比较的时候不区分大小写。
2：存储的时候是按照给定的大小写存储的，比较的时候是按照小写的方式比较。
```

查询参数信息：

```java
show variables like 'lower_case_table_names';
select @@lower_case_table_names;
```

修改值：
1、修改文件my.cnf ，设置 lower_case_table_names = 1
linux如何设置my.cnf文件

```java
查找Mysqld的安装目录
which mysqld
/usr/sbin/mysqld --help|grep 'my.cnf'
编辑my.cnf文件
lower_case_table_names = 1
重启MySQL
```

2、命令修改

```java
set global lower_case_table_names =1;
```
