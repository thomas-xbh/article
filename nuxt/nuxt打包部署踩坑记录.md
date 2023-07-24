---
link: https://blog.csdn.net/qq_42944436/article/details/113914326
title: nuxt打包部署踩坑记录
description: 用nuxt写的个人博客，一段时间后，发现我发布了篇文章，我的前端页面竟然没有显示。。。_nuxt 打包方式区别
keywords: nuxt 打包方式区别
author: 华山派Developer Csdn认证博客专家 Csdn认证企业博客 码龄5年 企业员工
date: 2021-02-21T02:13:28.000Z
publisher: null
stats: paragraph=7 sentences=5, words=132
---
个人博客是用nuxt编写的，初次是使用npm generate打包成静态网站的。这样就遇到了个问题，我发布了篇文章，我的前端页面竟然没有显示。。。

看了[Nuxt "generate"和"build"打包方式的区别](https://www.xdx97.com/article/639519451045167104)这篇文章后我找到了答案。原来静态打包后，所有服务端渲染的数据在打包的那一刻就固定了。如果数据变化了，就只能重新打包。（个人认为和hexo写博客一样了）

## 解决方案：

**1、使用npm build打包成web应用程序**

```
&#x4F18;&#x70B9;&#xFF1A;&#x4E00;&#x6B21;&#x6253;&#x5305;&#xFF0C;&#x5229;&#x4E8E;seo
&#x7F3A;&#x70B9;&#xFF1A;&#x8981;&#x5F00;node&#x670D;&#x52A1;&#x5668;&#xFF0C;&#x670D;&#x52A1;&#x5668;&#x538B;&#x529B;&#x5927;
&#x6CE8;&#x610F;&#xFF1A;&#x8981;&#x5728;nuxt.config.js&#x6587;&#x4EF6;&#x4E2D;&#x8BBE;&#x7F6E;target: 'server',(&#x9ED8;&#x8BA4;&#x4E24;&#x4E2A;&#x503C;&#xFF1A;server&#x548C;static,&#x5426;&#x5219;&#x540E;&#x7AEF;&#x6570;&#x636E;&#x6539;&#x53D8;&#xFF0C;&#x524D;&#x7AEF;&#x6570;&#x636E;&#x5C06;&#x4E0D;&#x4F1A;&#x66F4;&#x65B0;)
```

**2、npm generate打包，但要在服务器加个计划任务，定时执行npm generate命令**

```
&#x4F18;&#x70B9;&#xFF1A;&#x5229;&#x4E8E;seo&#xFF0C;&#x670D;&#x52A1;&#x5668;&#x538B;&#x529B;&#x5C0F;
&#x7F3A;&#x70B9;&#xFF1A;&#x8981;&#x52A0;&#x5B9A;&#x65F6;&#x4EFB;&#x52A1;&#xFF0C;&#x591A;&#x6B21;&#x6253;&#x5305;
&#x6CE8;&#x610F;&#xFF1A;&#x8981;&#x5728;nuxt.config.js&#x6587;&#x4EF6;&#x4E2D;&#x8BBE;&#x7F6E;target: 'static',(&#x9ED8;&#x8BA4;&#x4E24;&#x4E2A;&#x503C;&#xFF1A;server&#x548C;static)
```
