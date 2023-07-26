---
link: https://blog.51cto.com/u_15740728/5910070
title: 2022 年超详细过程步骤讲解 CentOS 7 安装jdk1.8_记笔记/程序bug的技术博客_51CTO博客
description: 2022 年超详细过程步骤讲解 CentOS 7 安装jdk1.8，2022年超详细过程步骤讲解CentOS7安装jdk1.8
keywords: 2022 年超详细过程步骤讲解 CentOS 7 安装jdk1.8,博客,51CTO博客
author: null
date: null
publisher: null
stats: paragraph=55 sentences=75, words=144
---
> linux系统下安装jdk以及环境变量的设置、真的是比window下方便一万倍

## 1、卸载系统自带jdk

### 1.1 查看系统自带jdk

```clike
java -version
```

![](https://s2.51cto.com/images/blog/202212/04210733_638c9b95ee55e39636.png?x-oss-process=image/watermark,size_14,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_30,g_se,x_10,y_10,shadow_20,type_ZmFuZ3poZW5naGVpdGk=)

### 1.2 查看java相关文件

```clike
rpm -qa | grep java
```

![](https://s2.51cto.com/images/blog/202212/04210734_638c9b962872068608.png?x-oss-process=image/watermark,size_14,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_30,g_se,x_10,y_10,shadow_20,type_ZmFuZ3poZW5naGVpdGk=)

### 1.3 删除openjdk相关文件

**我上边有四个对应文件**

> 命令介绍： rpm 管理套件
-e 删除指定的套件 --nodeps 不验证套件档的相互关联性

```clike
rpm -e --nodeps java-1.8.0-openjdk-1.8.0.102-4.b14.el7.x86_64
rpm -e --nodeps java-1.8.0-openjdk-headless-1.8.0.102-4.b14.el7.x86_64
rpm -e --nodeps java-1.7.0-openjdk-headless-1.7.0.111-2.6.7.8.el7.x86_64
rpm -e --nodeps java-1.7.0-openjdk-1.7.0.111-2.6.7.8.el7.x86_64
```

![](https://s2.51cto.com/images/blog/202212/04210734_638c9b961b20283342.png?x-oss-process=image/watermark,size_14,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_30,g_se,x_10,y_10,shadow_20,type_ZmFuZ3poZW5naGVpdGk=)

### 1.4 查看是否删除成功

```clike
java -version
```

![](https://s2.51cto.com/images/blog/202212/04210734_638c9b960701b78058.png?x-oss-process=image/watermark,size_14,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_30,g_se,x_10,y_10,shadow_20,type_ZmFuZ3poZW5naGVpdGk=)

## 2、下载对应的jdk

> 可以加博主、这个下载巨慢

官网：[http://www.oracle.com/technetwork/java/javase/downloads/index.html](http://www.oracle.com/technetwork/java/javase/downloads/index.html)

![](https://s2.51cto.com/images/blog/202212/04210734_638c9b962ffc865436.png?x-oss-process=image/watermark,size_14,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_30,g_se,x_10,y_10,shadow_20,type_ZmFuZ3poZW5naGVpdGk=) 还要注册一个oracle的账号：（注册后、就可以下载了） ![](https://s2.51cto.com/images/blog/202212/04210734_638c9b963cfc88176.png?x-oss-process=image/watermark,size_14,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_30,g_se,x_10,y_10,shadow_20,type_ZmFuZ3poZW5naGVpdGk=)

## 3、将jdk上传到虚拟机中

![](https://s2.51cto.com/images/blog/202212/04210734_638c9b961b53b28115.png?x-oss-process=image/watermark,size_14,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_30,g_se,x_10,y_10,shadow_20,type_ZmFuZ3poZW5naGVpdGk=)

## 4、备份jdk的安装包

### 4.1 备份一份

```clike
cp jdk-8u261-linux-x64.tar.gz /usr/local/src/
```

![](https://s2.51cto.com/images/blog/202212/04210734_638c9b961a05f82742.png?x-oss-process=image/watermark,size_14,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_30,g_se,x_10,y_10,shadow_20,type_ZmFuZ3poZW5naGVpdGk=)

### 4.2 实际安装到

![](https://s2.51cto.com/images/blog/202212/04210734_638c9b961a03187111.png?x-oss-process=image/watermark,size_14,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_30,g_se,x_10,y_10,shadow_20,type_ZmFuZ3poZW5naGVpdGk=)

在local目录下新建java文件

```clike
mkdir java
```

```clike
/usr/local/java
```

## 5、解压安装

我这里安装在 `/usr/local/java`下、方便统一管理安装的软件

### 5.1 解压

```clike
tar -zxvf jdk-8u144-linux-x64.tar.gz
```

![](https://s2.51cto.com/images/blog/202212/04210734_638c9b961c5c228207.png?x-oss-process=image/watermark,size_14,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_30,g_se,x_10,y_10,shadow_20,type_ZmFuZ3poZW5naGVpdGk=)

### 5.2 删除压缩包

![](https://s2.51cto.com/images/blog/202212/04210734_638c9b961e5ed20859.png?x-oss-process=image/watermark,size_14,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_30,g_se,x_10,y_10,shadow_20,type_ZmFuZ3poZW5naGVpdGk=)

## 6、配置jdk的环境变量

编辑全局变量

```clike
vim /etc/profile
```

在文本的最后粘贴如下代码

> JAVA_HOME=/usr/local/java/jdk1.8.0_261 是你自己的安装目录

```clike
#java environment
export JAVA_HOME=/usr/local/java/jdk1.8.0_261
export CLASSPATH=.:${JAVA_HOME}/jre/lib/rt.jar:${JAVA_HOME}/lib/dt.jar:${JAVA_HOME}/lib/tools.jar
export PATH=$PATH:${JAVA_HOME}/bin

```

![](https://s2.51cto.com/images/blog/202212/04210734_638c9b961f8a020874.png?x-oss-process=image/watermark,size_14,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_30,g_se,x_10,y_10,shadow_20,type_ZmFuZ3poZW5naGVpdGk=)

![](https://s2.51cto.com/images/blog/202212/04210734_638c9b962ffc858095.png?x-oss-process=image/watermark,size_14,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_30,g_se,x_10,y_10,shadow_20,type_ZmFuZ3poZW5naGVpdGk=)

## 7、将环境变量设置生效、同时查看是否安装成功

配置的环境生效：

```clike
source /etc/profile
```

查看是否安装成功

```clike
java -version
```

![](https://s2.51cto.com/images/blog/202212/04210734_638c9b968ad8518293.png?x-oss-process=image/watermark,size_14,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_30,g_se,x_10,y_10,shadow_20,type_ZmFuZ3poZW5naGVpdGk=)

## 8、后语

**如果需要jdk的安装包、请加下方博主联系方式 新电脑重装虚拟机jdk1.8 特此记录**
