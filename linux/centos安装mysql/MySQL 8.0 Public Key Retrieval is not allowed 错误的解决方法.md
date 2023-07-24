---
link: https://blog.csdn.net/u013360850/article/details/80373604
title: MySQL 8.0 Public Key Retrieval is not allowed 错误的解决方法
description: 在使用 MySQL 8.0 时重启应用后提示 com.mysql.jdbc.exceptions.jdbc4.MySQLNonTransientConnectionException: Public Key Retrieval is not allowed  没有仔细研究到底是什么问题，最简单的解决方法是在连接后面添加 allowPublicKeyRetrieval=true...
keywords: MySQL 8.0 Public Key Retrieval is not allowed 错误的解决方法
author: 呜呜呜啦啦啦 Csdn认证博客专家 Csdn认证企业博客 码龄10年 暂无认证
date: 2018-05-19T05:15:30.000Z
publisher: null
stats: paragraph=4 sentences=10, words=37
---
在使用 MySQL 8.0 时重启应用后提示 `com.mysql.jdbc.exceptions.jdbc4.MySQLNonTransientConnectionException: Public Key Retrieval is not allowed`

最简单的解决方法是在连接后面添加 `allowPublicKeyRetrieval=true`

文档中([https://mysql-net.github.io/MySqlConnector/connection-options/](https://mysql-net.github.io/MySqlConnector/connection-options/))给出的解释是：

如果用户使用了 sha256_password 认证，密码在传输过程中必须使用 TLS 协议保护，但是如果 RSA 公钥不可用，可以使用服务器提供的公钥；可以在连接中通过 `ServerRSAPublicKeyFile` 指定服务器的 RSA 公钥，或者 `AllowPublicKeyRetrieval=True`参数以允许客户端从服务器获取公钥；但是需要注意的是 `AllowPublicKeyRetrieval=True`可能会导致恶意的代理通过中间人攻击(MITM)获取到明文密码，所以默认是关闭的，必须显式开启
![](https://img-blog.csdnimg.cn/20190406221957566.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTMzNjA4NTA=,size_16,color_FFFFFF,t_70)
