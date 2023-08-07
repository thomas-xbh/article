---
link: https://juejin.cn/post/6973835825368793125
title: qs库使用指南
description: &quot;qs&quot; 是一个流行的查询参数序列化和解析库。可以将一个普通的object序列化成一个查询字符串，或者反过来将一个查询字符串解析成一个object，而且支持复杂的嵌套。它上手很容易
keywords: JavaScript
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2021-06-15T02:04:45.000Z
publisher: 稀土掘金
stats: paragraph=25 sentences=32, words=194
---
[qs](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fljharb%2Fqs "https://github.com/ljharb/qs")是一个流行的查询参数序列化和解析库。可以将一个普通的object序列化成一个查询字符串，或者反过来将一个查询字符串解析成一个object，而且支持复杂的嵌套。它上手很容易：

```js
Qs.parse('x[]=1')
Qs.stringify({x: [1]})
复制代码
```

qs的两个方法都接受一个可选的第二参数，可以让我们对结果进行配置，个人觉得比较有用的有以下几个：

ignoreQueryPrefix这个参数可以自动帮我们过滤掉location.search前面的❓，然后再解析， `addQueryPrefix`设为true可以在序列化的时候给我们加上 `?`

```js

Qs.parse('?x=1')
Qs.parse('?x=1', {ignoreQueryPrefix: true})

Qs.stringify({x: "1"})
Qs.parse({x: "1"}, {addQueryPrefix: true})
复制代码
```

数组序列化有几种方式： `indices`, `brackets`, `repeat`, `comma`，用来控制字符串的生成格式。来看下面的例子：

```js
qs.stringify({ a: ['b', 'c'] }, { arrayFormat: 'indices' })

qs.stringify({ a: ['b', 'c'] }, { arrayFormat: 'brackets' })

qs.stringify({ a: ['b', 'c'] }, { arrayFormat: 'repeat' })

qs.stringify({ a: ['b', 'c'] }, { arrayFormat: 'comma' })

复制代码
```

上面的四种方式，序列化得到的结果越来越精简，但是当面对嵌套数组时，却会导致不同程度的信息丢失，而且丢失的越来越严重。以四种方式对 `{ a: [['b'], 'c'] }` stringify 再 parse为例：

```js

qs.parse(qs.stringify({ a: [['b'], 'c'] }, { arrayFormat: 'indices' }))
qs.parse(qs.stringify({ a: [['b'], 'c'] }, { arrayFormat: 'brackets' }))
qs.parse(qs.stringify({ a: [['b'], 'c'] }, { arrayFormat: 'repeat' }))
qs.parse(qs.stringify({ a: [['b'], 'c'] }, { arrayFormat: 'comma' }))
复制代码
```

所以当数据里有嵌套时最好使用 `indices`模式，好在这也是默认模式。

delimiter可以控制将哪种字符作为分隔符，由于cookie的格式是使用 `;`来分隔，一个有用的例子是用来解析cookie：

```js
document.cookie
Qs.parse(document.cookie, {delimiter:'; '})
复制代码
```

正如我们在第一个例子看到的那样，我们把一个数字序列化再还原，得到的并不是一个数字，而是一个字符串：

```js
Qs.parse(Qs.stringify({x:1}))
复制代码
```

```js
Qs.parse('x[0]=1', {
    decoder(str, defaultEncoder, charset, type) {
      if (/^(\d+|\d*\.\d+)$/.test(str)) {
        return parseFloat(str)
      }
      return str
    }
  })
复制代码
```

或者再加上一个解析中文的功能:

```js
      if (/^%[A-Za-z0-9+/]/.test(str)) {
        return decodeURIComponent(str)
      }
复制代码
```

本文完
