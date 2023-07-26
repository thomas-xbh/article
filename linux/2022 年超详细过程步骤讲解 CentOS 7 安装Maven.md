---
link: https://blog.51cto.com/u_15740728/5927912
title: 2022 年超详细过程步骤讲解 CentOS 7 安装Maven。以及Mavne配置文件的修改_记笔记/程序bug的技术博客_51CTO博客
description: 2022 年超详细过程步骤讲解 CentOS 7 安装Maven。以及Mavne配置文件的修改，2022年超详细过程步骤讲解CentOS7安装Maven。以及Mavne配置文件的修改
keywords: 2022 年超详细过程步骤讲解 CentOS 7 安装Maven。以及Mavne配置文件的修改,博客,51CTO博客
author: null
date: null
publisher: null
stats: paragraph=59 sentences=48, words=130
---
# 2022 年超详细过程步骤讲解 CentOS 7 安装Maven。以及Mavne配置文件的修改

推荐原创

[小兔教你学编程](https://blog.51cto.com/u_15740728)<time>2022-12-11 00:02:53</time>©著作权

**_文章标签_ [maven](https://blog.51cto.com/topic/maven.html) [centos](https://blog.51cto.com/topic/centos.html) [java](https://blog.51cto.com/topic/java.html) [apache](https://blog.51cto.com/topic/apache.html) [配置文件](https://blog.51cto.com/topic/peizhiwenjian.html)** **_文章分类_ [开源](https://blog.51cto.com/nav/open-source)**

©著作权归作者所有：来自51CTO博客作者小兔教你学编程的原创作品，请联系作者获取转载授权，否则将追究法律责任



## 1、下载maven

官网：​[​https://maven.apache.org/download.cgi​](https://maven.apache.org/download.cgi)​​ 下载：​ `&#x200B; apache-maven-3.8.6-bin.tar.gz&#x200B;`​

![](https://s2.51cto.com/images/blog/202212/10001308_63935e94d258a21534.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_30,g_se,x_10,y_10,shadow_20,type_ZmFuZ3poZW5naGVpdGk=/resize,m_fixed,w_1184 "在这里插入图片描述")

## 2、创建放置maven的文件夹

统软件统一放置到 ​ `&#x200B;/usr/local/&#x200B;`​

![](https://s2.51cto.com/images/blog/202212/10001309_63935e951141861223.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_30,g_se,x_10,y_10,shadow_20,type_ZmFuZ3poZW5naGVpdGk=/resize,m_fixed,w_1184 "在这里插入图片描述")

## 3、上传文件到对应的maven文件下

![](https://s2.51cto.com/images/blog/202212/10001309_63935e95348a185840.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_30,g_se,x_10,y_10,shadow_20,type_ZmFuZ3poZW5naGVpdGk=/resize,m_fixed,w_1184 "在这里插入图片描述")

## 4、解压、删除安装包

### 4.1 解压

```
tar -zxvf apache-maven-3.8.6-bin.tar.gz
```

![](https://s2.51cto.com/images/blog/202212/10001309_63935e955ac0453633.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_30,g_se,x_10,y_10,shadow_20,type_ZmFuZ3poZW5naGVpdGk=/resize,m_fixed,w_1184 "在这里插入图片描述")

### 4.2 删除

```
rm -rf apache-maven-3.8.6-bin.tar.gz
```

![](https://s2.51cto.com/images/blog/202212/10001309_63935e958b3df10184.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_30,g_se,x_10,y_10,shadow_20,type_ZmFuZ3poZW5naGVpdGk=/resize,m_fixed,w_1184 "在这里插入图片描述")

## 5、配置环境变量

**打开全局配置文件**

```
vim /etc/profile
```

![](https://s2.51cto.com/images/blog/202212/10001309_63935e95b16a072447.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_30,g_se,x_10,y_10,shadow_20,type_ZmFuZ3poZW5naGVpdGk=/resize,m_fixed,w_1184 "在这里插入图片描述")

**在文件末位插入配置**

```
#maven environmentMAVEN_HOME=/usr/local/maven/apache-maven-3.8.6export PATH=${MAVEN_HOME}/bin:${PATH}
```

![](https://s2.51cto.com/images/blog/202212/10001309_63935e95d819285244.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_30,g_se,x_10,y_10,shadow_20,type_ZmFuZ3poZW5naGVpdGk=/resize,m_fixed,w_1184 "在这里插入图片描述")

## 6、文件生效 查看maven

**重载环境变量**

```
source /etc/profile
```

**查看是否安装成功**

```
mvn -v
```

![](https://s2.51cto.com/images/blog/202212/10001310_63935e9611d9767072.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_30,g_se,x_10,y_10,shadow_20,type_ZmFuZ3poZW5naGVpdGk=/resize,m_fixed,w_1184 "在这里插入图片描述")

## 7、替换maven源为阿里云

**进入maven的配置文件**

```
/usr/local/maven/apache-maven-3.8.6/conf/settings.xml
```

![](https://s2.51cto.com/images/blog/202212/10001310_63935e963030a73371.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_30,g_se,x_10,y_10,shadow_20,type_ZmFuZ3poZW5naGVpdGk=/resize,m_fixed,w_1184 "在这里插入图片描述")

### 7.1 默认的源

![](https://s2.51cto.com/images/blog/202212/10001310_63935e9649d1e6568.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_30,g_se,x_10,y_10,shadow_20,type_ZmFuZ3poZW5naGVpdGk=/resize,m_fixed,w_1184 "在这里插入图片描述")

### 7.2 添加阿里云源

找到​ `&#x200B;<mirrors></mirrors>&#x200B;`​标签对，添加一下代码：

```
alimaven     aliyun maven     http://maven.aliyun.com/nexus/content/groups/public/     central
```

![](https://s2.51cto.com/images/blog/202212/10001310_63935e96814fa55460.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_30,g_se,x_10,y_10,shadow_20,type_ZmFuZ3poZW5naGVpdGk=/resize,m_fixed,w_1184 "在这里插入图片描述")

## 8、指定jar包仓库位置

```
/usr/local/maven/repository
```

![](https://s2.51cto.com/images/blog/202212/10001310_63935e96b0a159540.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_30,g_se,x_10,y_10,shadow_20,type_ZmFuZ3poZW5naGVpdGk=/resize,m_fixed,w_1184 "在这里插入图片描述")



新建​ `&#x200B;respsitory&#x200B;`​ 目录

```
mkdir respsitory
```

![](https://s2.51cto.com/images/blog/202212/10001310_63935e96d103e30679.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_30,g_se,x_10,y_10,shadow_20,type_ZmFuZ3poZW5naGVpdGk=/resize,m_fixed,w_1184 "在这里插入图片描述")

## 9、指定JDK版本

```
jdk-1.8                    true           1.8                                 1.8             1.8             1.8
```

![](https://s2.51cto.com/images/blog/202212/10001311_63935e9709cf484667.png?x-oss-process=image/watermark,size_16,text_QDUxQ1RP5Y2a5a6i,color_FFFFFF,t_30,g_se,x_10,y_10,shadow_20,type_ZmFuZ3poZW5naGVpdGk=/resize,m_fixed,w_1184 "在这里插入图片描述")

## 10 、后语

maven安装大致流程
1、安装位置
2、配置全局变量
3、修改配置文件
4、修改阿里云源
5、指定jdk版本
6、指定资源下载位置。

新电脑安装

参考：
​Centos7安装maven​​



* **赞**
* **收藏**
* **评论**
* **举报*

上一篇：[2022年超详细在CentOS 7上安装Node.js方法（源码安装）](https://blog.51cto.com/u_15740728/5927080)

下一篇：[超详细过程步骤讲解 CentOS 7 安装jdk1.8](https://blog.51cto.com/u_15740728/5931205)
