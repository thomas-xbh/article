---
link: null
title: CentOS安装Mysql8.0（2022最新）
description: # centos7安装mysql8.0（2022年最新） > 给新手小贴士：接下来的指令，没有“mysql>”开头的都是在shell中执行的，例如我的直接在这里执行： > >...
keywords: null
author: null
date: null
publisher: 简书
stats: paragraph=95 sentences=27, words=317
---
# centos7安装mysql8.0（2022年最新）

> 给新手小贴士：接下来的指令，没有"mysql>"开头的都是在shell中执行的，例如我的直接在这里执行：

> ```shell

> [root@iZhp3a7bhi9b65ns1ej25rZ ~]# yum install mysql-community-server -y

> ```

## 一、下载与安装

#### 1、删除现有的mysql

```shell

for i in $(rpm -qa|grep mysql);do rpm -e $i --nodeps;done

```

```shell

rm -rf /var/lib/mysql && rm -rf /etc/my.cnf

```

#### 2、下载

```shell

yum localinstall https://repo.mysql.com//mysql80-community-release-el7-1.noarch.rpm

```

> 如果出现问题可能是yum源的问题，查一下centos如何换yum源 或者直接复制报错内容搜索一下即可，在下就不赘述了

#### 3、安装

```shell

yum install mysql-community-server -y

```

> 如果出现GPG key ERROR，就用这个：

```shell

yum install mysql-community-server -y --nogpgcheck

```

#### 4、 启动

```shell

systemctl start mysqld

```

#### 5、 设置自启动

```shell

systemctl enable mysqld

```

## 二、登录与初始化密码

#### 1、查看初始默认密码, 我这里的默认密码为"anSuEjHf9J#k"

```shell

grep 'temporary password' /var/log/mysqld.log

结果如下：

2022-04-22T04:22:18.693491Z 6 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: anSuEjHf9J#k

```

#### 2、用初始密码登录

```

mysql -panSuEjHf9J#k

```

#### 注意注意！！接下来一定要先重置密码，不然quit之后可能就登不上了

> 首先，无论使用什么指令，都会报错让你重置密码

> 例如：

> mysql> select version();

> ERROR 1820 (HY000): You must reset your password using ALTER USER statement before executing this statement.

> mysql> use mysql;

> ERROR 1820 (HY000): You must reset your password using ALTER USER statement before executing this statement.

> mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY '123456';

> ERROR 1819 (HY000): Your password does not satisfy the current policy requirements

#### 3、所以我们先重置一个密码（符号、大小写、数字、8位以上）

```shell

mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'Root@123456';

Query OK, 0 rows affected (0.03 sec)

刷一下

mysql> flush privileges;

Query OK, 0 rows affected (0.03 sec)

```

> **接下来有些人喜欢用123456来作为mysql密码，那么我们改一下mysql8的密码规则：**

> 先quit退出 重新登陆

> mysql -pRoot@123456

> 设置密码规则

> mysql> set global validate_password.policy=0;

> Query OK, 0 rows affected (0.00 sec)

> mysql> set global validate_password.length=1;

> Query OK, 0 rows affected (0.00 sec)

> 修改新密码：

> mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY '123456';

> flush privileges;

#### 4、修改设置远程登陆（默认是只能localhost登录）非常重要！！

```shell

mysql> update mysql.user set host='%' where user="root";

Query OK, 1 row affected (0.10 sec)

Rows matched: 1 Changed: 1 Warnings: 0

mysql> flush privileges;

Query OK, 0 rows affected (0.14 sec)

```

#### 5、搞定！

```shell

mysql> select host,user,authentication_string from mysql.user;

+-----------+------------------+------------------------------------------------------------------------+

| host | user | authentication_string |

+-----------+------------------+------------------------------------------------------------------------+

| % | mysql.infoschema | $A$005$THISISACOMBINATIONOFINVALIDSALTANDPASSWORDTHATMUSTNEVERBRBEUSED |

| localhost | mysql.session | $A$005$THISISACOMBINATIONOFINVALIDSALTANDPASSWORDTHATMUSTNEVERBRBEUSED |

| localhost | mysql.sys | $A$005$THISISACOMBINATIONOFINVALIDSALTANDPASSWORDTHATMUSTNEVERBRBEUSED |

| localhost | root | $A$005${7J0=4Dc7Jym8eI/FU4jimKWFvkD9XmoAkF1ca5.Un0bc6zgmPtU.0 |

+-----------+------------------+------------------------------------------------------------------------+

4 rows in set (0.00 sec)

```

### 接下来就可以快乐使用mysql了
