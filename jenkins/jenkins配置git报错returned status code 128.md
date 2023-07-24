---
link: https://blog.csdn.net/oceanyang520/article/details/100583187
title: jenkins配置git报错returned status code 128
description: jenkins docker版拉取git代码方法一、首先，有两种协议方式，一种是http使用用户名密码，不推荐，另一种是使用ssh协议，免密方式，推荐。二、在jenkins上生成公钥：1、进入容器docker exec -it rongqiname /bin/bash2、执行：ssh-keygen -t rsa一路回车直到结束就ok3、复制公钥，公钥是pub哦..._jenkins gitee returned status code 128:
keywords: jenkins gitee returned status code 128:
author: Ocean__ Csdn认证博客专家 Csdn认证企业博客 码龄12年 暂无认证
date: 2023-05-22T12:09:41.000Z
publisher: null
stats: paragraph=39 sentences=10, words=33
---
jenkins docker版拉取git代码方法

一、首先，有两种协议方式，一种是http使用用户名密码，不推荐，另一种是使用ssh协议，免密方式，推荐。

二、在jenkins上生成公钥：

1、进入容器

docker exec -it rongqiname /bin/bash

2、执行：

ssh-keygen -t rsa

一路回车直到结束就ok

3、复制公钥，公钥是pub哦，私钥是。。。不带pub的

cat ~/.ssh/rsa.pub 这个是公钥文件，，，但是docker中的jenkins貌似不在这里，这个是人家root的，记住，~代表root哦

这时候，你whoami一下，linux系统是jenkins用户，so，继续下面命令：

cat /var/jenkins_home/.ssh/id_rsa.pub

然后复制内容，注意要一字不漏。

4、把复制好的公钥内容，拷贝到git里，但是，git也不一定好配置，请看：

这里以gitee为例，因为朋友用的这个，但是都是大同小异哦，不懂的v我：marinechat。

首先登陆gitee，然后 [https://gitee.com/profile](https://gitee.com/profile)

找到左侧ssh公钥配置

![](https://img-blog.csdnimg.cn/20190906165429336.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L29jZWFueWFuZzUyMA==,size_16,color_FFFFFF,t_70)

然后点击进去，标题不用填，会自动补充的，直接黏贴刚才复制好的公钥：

ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCctTg+b8ur3UJcTC3zb+8vNSYr/WO2OJ3dHFYFLHzJwOSPKjgPcmIMoREhmlKPbY2Rdow7sgnJf1lJNFisi9AzQdG8Trb7bwXLwaVk0uXzN65S2NSaUi9y9MrjwfUhEVNivNlsWkF9UJ/iM7tfG4inYAnTJ9LZrnsLEJ7iHubwREdjPmNrBUycIxq6IfweTzK+e14ex2zwELKa9GysBPBNKTXWjrieuOU8XgaJaOIWobxYF+BUGGS3YUXuW8OdArC4CsllRHbAWZXxFgdCbjMgk4IVngo/FeKvjntSA2nz+eHxQe25NUhCy7PswXS5NgayItoymYwwztchZXQGGbsEH jenkins@bedd012s83b9

类似上面的格式，然后保存就可以了。

现在，jenkins应该有权限可以拉取git的代码了。

这里我说说实现原理，当然了，我也不知道真正的原理，大概是这样：

在git里配置jenkins的公钥，意思大概是git里保存jenkins所在linux的一个密码文件，相当于一个凭证，每当jenkins去请求拉取代码的时候就会验证jenkins有没有在git这里登记过公钥，如果登记过并且正确，ok，通过。

好了，接下来在jenkins里配置任务：

在git那一栏这样：

![](https://img-blog.csdnimg.cn/20190906170445330.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L29jZWFueWFuZzUyMA==,size_16,color_FFFFFF,t_70)

然后就不会提示这个讨人厌的错误了：

![](https://img-blog.csdnimg.cn/20190906170613454.png)

三、原则上，上面步骤就可以了，但是还是不合理，继续补充下面步骤

首先，去掉上面的repository url的值里的ssh:// 直接git开头就好。。。

把jenkins所在linux里（如果是容器，就是容器里）也就是刚才公钥目录下的私钥文件的内容复制一下，黏贴到jenkins页面里：

具体步骤，新建jenkins任务的时候配置git的时候，credentials点击Add，弹出窗口：

![](https://img-blog.csdnimg.cn/20190906174249981.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L29jZWFueWFuZzUyMA==,size_16,color_FFFFFF,t_70)

类型是ssh

username是git

private key选择 enter directly单选按钮

然后黏贴刚才复制的私钥内容。最后点击添加按钮。

试试吧，有问题在说吧。。。
