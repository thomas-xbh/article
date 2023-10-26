---
link: null
title: gitignore添加忽略文件失效、不起作用 - 进军的蜗牛 - 博客园
description: 文章原文：https://www.cnblogs.com/yalong/p/14986408.html 由于特殊原因，把.gitignore文件下 /dist 注释了，也就是把dist 文件添加到git仓库里了， 现在又不想添加到git仓库里了，但是把/dist 注释放开还不行，就是.gitigno
keywords: null
author: 我的博客 我的园子 账号设置 简洁模式 ... 退出登录
date: 2021-07-08T07:49:00.000Z
publisher: null
stats: paragraph=12 sentences=6, words=57
---
由于特殊原因，把 `.gitignore`文件下 `/dist` 注释了，也就是把 `dist` 文件添加到git仓库里了，
现在又不想添加到git仓库里了，但是把 `/dist` 注释放开还不行，就是 `.gitignore`失效了，查阅资料才知道

> 已跟踪的文件是指那些被纳入了版本控制的文件，在上一次提交中有它们的记录。那么未跟踪文件就是指那些从没提交过的文件。

因为上次已经把 `/dist` 整个提交上去了，所以这时候 `.gitignore`已经不行了

要想实现git 忽略dist文件夹，需要下面几个步骤

`git rm` 或者 `git rm --cached`

`git rm` ： 同时从工作区和索引中删除文件。即本地的文件也被删除了。

`git rm --cached`：从索引中删除文件,但是本地文件还存在， 只是不希望这个文件被版本控制。

这里我使用第二个，具体用法就是 `git rm --cached -r dist`

> -r 的意思是递归处理，如果不加 -r的话，会报错

如果取消某个文件的跟踪，可以不用 `-r` 直接 `git rm --cached dist/index.less`

```
git add .
git commit -m '&#x4FEE;&#x6539;gitignore'
git push
```

以后本地dist目录下文件再变的话，也不会被跟踪到了，其他小伙伴，只需 `git pull` 一下就可以
