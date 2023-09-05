---
link: https://juejin.cn/post/6844903702935896077
title: 快速了解helmet的使用
description: helmet是express的中间件，通过设置各种header来为express应用提供安全保护。虽然不能完全杜绝安全问题，但确实能提供某种程度的保护。 helmet的使用非常简单。首先使用npm安装helmet： helmet包含了多个中间件，每个中间件既可以单独使用，也可以…
keywords: 前端,安全,XSS,DNS
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2018-11-01T09:05:36.000Z
publisher: 稀土掘金
stats: paragraph=38 sentences=25, words=107
---
`helmet`是 `express`的中间件，通过设置各种 `header`来为 `express`应用提供安全保护。虽然不能完全杜绝安全问题，但确实能提供某种程度的保护。

`helmet`的使用非常简单。首先使用 `npm`安装 `helmet`：

```
npm install helmet --save
```

其次在 `express`应用中使用该中间件：

```
const express = require(<span class="hljs-string">'express'</span>)
const helmet = require(<span class="hljs-string">'helmet'</span>)

const app = express()

app.use(helmet())
```

`helmet`包含了多个中间件，每个中间件既可以单独使用，也可以通过 `helmet()`集中配置。以是否禁用缓存的 `noCache`中间件为例：

单独使用中间件：

```
app.use(helmet.noCache());
```

在 `helmet`函数中配置：

```
app.use(helmet({
    noCache: <span class="hljs-literal">true</span>
}));
```

我们接下来分别看一下几个主要的中间件的作用.

攻击者可以针对 `X-Powered-By`中暴露的服务器语言的漏洞进行攻击。

`hidePoweredBy`可以隐藏或混淆响应头中的 `X-Powered-By`字段以迷惑攻击者。

```
app.use(helmet.hidePoweredBy());
```

也可以通过设置假的字段值来欺骗攻击者：

```
app.use(helmet.hidePoweredBy({<span class="hljs-built_in">set</span>To: <span class="hljs-string">'PHP 4.2.0'</span>}));
```

攻击者骗取用户点击一个以iframe的方式隐藏的页面，来获取用户的信息。

`frameguard`通过设置 `x-frame-options`来允许iframe的域。

```
app.use(helmet.frameguard({action: <span class="hljs-string">'deny'</span>}));
```

设置 `X-XSS-Protection`提供基本的XSS防护，避免基本的反射性XSS攻击。

```
// Sets <span class="hljs-string">"X-XSS-Protection: 1; mode=block"</span>.

app.use(helmet.xssFilter());
```

## noSniff

如果响应头中 `Content-Type`没有指定，浏览器默认会自动尝试识别响应体的内容以正确解析响应的文件。

设置 `X-Content-Type-Options`为 `nosniff`后，浏览器不再进行自动识别。这意味着响应的文件类型如果与 `Content-Type`中声明的不一致，将会被浏览器屏蔽掉。

```
app.use(helmet.noSniff());
```

有些站点可能提供了HTML文件的下载，部分IE浏览器中，该文件会在站点的上下文打开，存在脚本注入的风险。

设置 `X-Download-Options`为 `noopen`不允许在在站点的上下文打开下载的HTML文件。

```
app.use(helmet.ieNoOpen());
```

设置 `Strict-Transport-Security`告知用户在一定的时间段使用 `https`访问。防止降级攻击和 `cookie`劫持。

如下设置未来的90天内只使用 `https`访问。

```
app.use(helmet.hsts({maxAge: 7776000}));
```

`dns-prefetch`在提升网站性能的同时，潜在地会导致用户隐私泄露、dns服务过载、页面统计失真等问题。

`dnsPrefetchControl`通过将 `X-DNS-Prefetch-Control`设置为 `off`禁止浏览器进行DNS预解析。

```
app.use(helmet.dnsPrefetchControl())
```

以上简单介绍了 `helmet`的用法和几个默认开启的中间件。 `helmet`通过添加各种响应头来提供基本的安全防护。剩余未介绍到的中间件可以查阅其官方文档。
