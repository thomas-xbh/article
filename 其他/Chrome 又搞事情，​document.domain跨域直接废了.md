---
link: https://blog.csdn.net/lgno2/article/details/122738799
title: Chrome 又搞事情，​document.domain跨域直接废了
description: 大家好，我是 漫步，今儿个又给大家来分析浏览器策略了～关注公众号前端开发博客，回复“加群”加入我们一起学习，天天进步天天出新策略，业务都要改废了 ...一句话描述document.do..._document.domain 已弃用
keywords: document.domain 已弃用
author: 前端开发博客 Csdn认证博客专家 Csdn认证企业博客 码龄14年 暂无认证
date: 2022-01-28T07:11:15.000Z
publisher: null
stats: paragraph=55 sentences=59, words=183
---
大家好，我是 **漫步** ，今儿个又给大家来分析浏览器策略了～

关注公众号 前端开发博客，回复"加群"

加入我们一起学习，天天进步

天天出新策略，业务都要改废了 ...

## 一句话描述

`document.domain` 将变为可读属性。

> 别着急，预计最早变化时间是 `Chrome 101` 版本，现在最新版是 `97`。

## 对我们有啥影响？

如果你的业务里有通过更改 `document.domain` 来进行跨域的场景，马上就芭比Q了，得快点进行改造了。

## 用 `document.domain` 是咋跨域的？

跨域老生常谈的话题了，大家应该都清楚的很，再来啰嗦一遍。

简单的说：

如果网页的： `&#x534F;&#x8BAE;&#x3001;&#x57DF;&#x540D;&#x3001;&#x7AEF;&#x53E3;` 有一个不一样，它们的资源无法互相访问。

我们通过了一些手段，可以绕过这个限制，让非同源的资源也能互相访问，这就是跨域。

那么用 `document.domain` 之前是咋跨域的呢？

假如我们有这样一个场景：

我们在一个页面 `https://a.conardli.com` 嵌入了一个 `iframe` 页面 `https://b.conardli.com`。它们的二级域名都是 `conardli.com`。

这时候，因为浏览器的同源策略限制，我们主页面和 `iframe` 肯定是无法通信的。
![](https://img-blog.csdnimg.cn/img_convert/b97bdf3da1144df58051a4c8baa2c6ce.png)

但是，当两个页面的 `document.domain` 都设置为 `conard.com` 也就是二级域名的时候，浏览器就将两个来源视为同源。

这时候主页面就可以和 `iframe` 进行通信了（比如访问 `iframe` 的 `document`）。
![](https://img-blog.csdnimg.cn/img_convert/e12f51ba5fac6b7999bb4383e4334c0f.png)

另外，还有个场景，我们本地调试的时候可能经常会用到：相同域名、不同端口间的跨域。

比如，上面两个网页，在还没上线的时候，可能我们这时候要在本地调试功能，可能把它们部署在本地的不同端口上：

* `http://localhost:8888/`
* `http://localhost:6666/`

默认情况下它俩肯定也是不能跨域通信的，这时候我们可以把它们的 `document.domain` 都设置为 `localhost`，就可以跨过端口不同的限制了。

## 用的好好地，为啥要禁用捏？

不安全呀。
![](https://img-blog.csdnimg.cn/img_convert/f506448dec985ec02b0a13a0ed620cfb.png)

你觉的二级域名一样的域名一定属于同一个业务吗？

那可不一定，比如一个第三方的页面托管服务，它可能只有一个二级域名 `xxx.com`。

不同的个人用户使用它的服务的时候可以定制自己的域名。 `&#x5C0F;&#x660E;.xxx.com`、 `&#x5C0F;&#x82B1;.xxx.com`。

这时候，这种跨域的方式就可能被滥用了。

所以， `Chrome` 决定要禁用掉它。

## 有啥替代方案啊？

不慌，还有 `postMessage`。
![](https://img-blog.csdnimg.cn/img_convert/c7c4e3cb9e272c84683dfc2535829424.png)

大多数场景里面， `postMessage()`或 `Channel Messaging API` 都可以替代 `document.domain`。

主页面： `https://a.conardli.com`

```go
// 给 https://b.conardli.com 发消息
iframe.postMessage('哈喽', 'https://b.conardli.com');

// 接收信息
iframe.addEventListener('message', (event) => {
  // 把不想要的信息过滤掉
  if (event.origin !== 'https://b.conardli.com') return;

  if (event.data === 'succeeded') {
    // 干点别的 ...
  }
});
```

`iframe` 页面： `https://b.conardli.com`

```go
// 接收信息
window.addEventListener('message', (event) => {
  // 拒绝掉乱七八糟的信息
  if (event.origin !== 'https://a.conardli.com') return;

  // 恢复消息
  event.source.postMessage('succeeded', event.origin);
});
```

## 我就想用 `document.domain` 咋整？

可能你的网站改造起来有点麻烦，或者你对 `document.domain` 情有独钟？

给你的网页增加下面这个 `Header` 就可以了：

```go
Origin-Agent-Cluster: ?0
```

但是， `Origin-Agent-Cluster` 这个 `Header` 的作用可不止于此，有了解的小伙伴可以在评论区告诉我，没有的话我就挖个坑下回给大家讲讲。

## 参考

* https://developer.chrome.com/blog/immutable-document-domain/

```undefined```

```go
推荐阅读
```





![](https://img-blog.csdnimg.cn/img_convert/52541f900ad22ffef39e5abf8c395582.png)

_关注前端开发博客，每日分享干货！_

关注前端开发博客，在后台回复以下关键字可以获取资源。

1. 回复「小抄」，领取Vue、JavaScript 和 WebComponent 小抄 PDF
2. 回复「Vue脑图」获取 Vue 相关脑图
3. 回复「思维图」获取 JavaScript 相关思维图
4. 回复「简历」获取简历制作建议
5. 回复「简历模板」获取精选的简历模板
6. 回复「加群」进入500人前端精英群
7. 回复「电子书」下载我整理的大量前端资源，含面试、Vue实战项目、CSS和JavaScript电子书等。
8. 回复「知识点」下载高清JavaScript知识点图谱

![](https://img-blog.csdnimg.cn/img_convert/49e4e9d19e9671dbbe7be4bf1d83c8c0.png)

**👍🏻 点赞 + 在看 支持小编**
