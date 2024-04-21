---
link: https://blog.csdn.net/qq_38843185/article/details/80024226
title: 如何连接本地仓库与GitHub仓库
description: 文章浏览阅读3.7k次，点赞2次，收藏2次。    1.Git 配置 参考https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/，廖雪峰老师写的很详细。    2.GitHub右上角点击 新建一个仓库，可以勾上README，也可以之后再加入。    3.在本地新建一个文件夹作为本地仓库，右键打开Git Bush.    4.依次..._github和本地连接
keywords: github和本地连接
author: Joy_we1 博客等级 码龄7年
date: 2023-05-13T04:10:16.000Z
publisher: null
stats: paragraph=12 sentences=14, words=38
---
1.Git 配置 参考[https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/)，廖雪峰老师写的很详细。

2.GitHub右上角点击![](https://img-blog.csdn.net/201804202101491?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4ODQzMTg1/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70) 新建一个仓库，可以勾上README，也可以之后再加入。

3.在本地新建一个文件夹作为本地仓库，右键打开Git Bush.

4.依次输入

```java
git init
git remote add origin git@github.com:[your repository]
```

与你的远程仓库建立连接，[]中填写你自己的仓库地址，在建立后可以看到，注意"[]"要删掉。

5.接下来在第一次提交前一定要

```plain
 git pull --rebase origin master
```

不然提交会报错。

6.本地写好文件后用

```plain
git add .
git commit -m "message"
git push -u origin master
```

进行提交，"."表示提交所有，"message"是提交信息，"master"表示提交到主分支，当然也可以提交到其他分支。只有最后一句才会真正提交，add和commit类似于准备和配置。



