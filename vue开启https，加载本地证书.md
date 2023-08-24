---
link: https://blog.csdn.net/xfjpeter/article/details/121480873
title: vue开启https，加载本地证书
description: vue开启https，加载本地证书1. 通过mkcert创建本地证书1.1 安装mkcert安装方式：npm包地址：https://www.npmjs.com/package/mkcert安装命令：npm install -g mkcert判断是否安装成功，输入命令：mkcert --version，如果能看到版本号，说明安装成功，可以进行下一步1.2 生成证书生成一个ca证书，mkcert create-ca，生成之后会看到一个ca.crt和ca.key文件利用刚刚生成的ca证书，_vue https 证书安装
keywords: vue https 证书安装
author: 潇洒哥Gg Csdn认证博客专家 Csdn认证企业博客 码龄9年 暂无认证
date: 2022-01-27T07:23:25.000Z
publisher: null
stats: paragraph=6 sentences=2, words=18
---
.js 中，你可以使用 `require` 函数来

本地图片。例如： ```html ``` 在上面的代码中，我们使用 `require` 函数来

`@/assets/images` 目录下的本地图片。`imageUrl` 计算属性会根据 `imageName` 变量的值动态生成图片的路径。这样就可以

指定的本地图片了。 注意：`require` 函数只能用于

模块（例如图片、文本等），不能用于

普通的 JavaScript 文件。另外，如果你的图片名称是由变量指定的，需要使用字符串模板语法来拼接完整路径。
