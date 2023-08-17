---
link: https://developer.aliyun.com/article/972511
title: 【Hexo】域名绑定篇-阿里云开发者社区
description: 【Hexo】域名绑定篇
keywords: 搜索推荐
author: null
date: 2023-08-17T22:35:00.000Z
publisher: null
stats: paragraph=7 sentences=8, words=21
---
我买的是一个很辣鸡的域名 `www.heeh.xyz`，第一年是9块钱，但续费的话就得几十上百了。

> 建议：如果你真的买到了喜欢的域名，请尽可能买的长一点。否则到续费的时候你可能追悔莫及，而且如果你需要备案更换域名也会很麻烦。👀

点开解析添加如下记录：

* `CNAME`记录的记录值设置成域名，也就是你的github主页 `username.github.io`。
* `A`记录的记录值设置为IP地址，通过 `ping username.github.io` 获得。
* 然后你需要在博客的根目录的 `source`文件夹下新建一个 `CNAME`文件（不要有任何后缀），在里面写入自己的域名

最后在你博客的Github仓库里找到设置：

下拉到Pages填入你的域名保存：

> 以我的域名 `heeh.xyz`为例
