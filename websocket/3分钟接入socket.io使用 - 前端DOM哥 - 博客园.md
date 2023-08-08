---
link: null
title: 3分钟接入socket.io使用 - 前端DOM哥 - 博客园
description: 传统的客户端和服务器通信协议是HTTP：客户端发起请求，服务端进行响应，服务端从不主动勾搭客户端。这种模式有个明显软肋，就是同步状态。而实际应用中有大量需要客户端和服务器实时同步状态的场景，比如聊天室、股票行情、在线共享文档等都需要客户端实时拿到服务器的最新状态。socket.io 是一个类库，内部
keywords: null
author: 我的博客 我的园子 账号设置 简洁模式 ... 退出登录
date: 2020-02-02T14:00:00.000Z
publisher: null
stats: paragraph=38 sentences=33, words=187
---
传统的客户端和服务器通信协议是HTTP：客户端发起请求，服务端进行响应，服务端从不主动勾搭客户端。

这种模式有个明显软肋，就是同步状态。而实际应用中有大量需要客户端和服务器实时同步状态的场景，比如聊天室、股票行情、在线共享文档等都需要客户端实时拿到服务器的最新状态。

针对这种实时同步的需求，一种简单的方式是轮询，比如每隔5s发一次http请求去拿服务器最新的状态数据。但这种方式会存在数据延迟，浪费带宽等副作用。

更完美的方式是使用WebSocket，浏览器原生支持，W3C标准协议，客户端和服务器建立持久性连接可以互发消息。

socket.io 是一个类库，内部封装了WebSocket，可以在浏览器与服务器之间建立实时通信。

如果某些旧版本的浏览器不支持WebSocket，socket.io会使用轮询代替。另外它还具有可发送二进制消息、多路复用、创建房间等特性，因此相比直接使用原生WebSocket，socket.io是更好的选择。

开发一个实时应用主要分两部分：服务端和客户端，socket.io分别提供了相应的npm包供我们方便地调用。

接下来就通过一个生动形象且有趣的栗子分别介绍这两大块。

现在假设李白，瑶，吕布，后羿，貂蝉5个人加入了一个叫 KPL 的房间，在文章结束时我们将拥有一个麻雀虽小五脏俱全的峡谷英雄在线聊天室

首先安装socket.io提供的服务端npm包：

```sh
npm i socket.io
```

**可以与 Express 框架配合使用：**

```js
const http = require('http')
const app = require('express')()
const server = http.createServer(app)
const io = require('socket.io')(server)
server.listen(3000)
```

**也可以与 Koa 框架配合使用**

```js
const http = require('http')
const Koa = require('koa')
const app = new Koa()
const server = http.createServer(app.callback())
const io = require('socket.io')(server)
server.listen(3000)
```

使用起来就是这么简单。接下来就可以写业务逻辑啦

```js
io.on('connect', client => { // client 即是连接上来的一个客户端
  console.log(client.id) // id 是区分客户端的唯一标识

  client.on('disconnect', () => {}) // 客户端断开连接时调用(可能是关掉页面，网络不通了等)
})
```

`connect` 和 `disconnect` 是 socket.io 内置的事件类型，用于在客户端连接和断开的时候做一些事情。

在客户端建立连接时需要把他们加入到一个房间里去，类似创建了一个聊天室

```diff
  console.log(client.id)
+ client.join('KPL') // 将客户端加入到 KPL 房间内
  client.on('disconnect', () => {})
```

紧接着瑶进来秒发了首条消息：我打野，不给就送

服务器在收到这条振奋人心的消息后需要立即同步给其他四位队友

```diff
  client.join('KPL')
+ client.on('talk', message => {
+   client.to('KPL').emit('talk', message) // 发送给房间里的每个人，除了发送者
+ })
  client.on('disconnect', () => {})
```

服务端的功能到这基本上就开发完了。创建了一个房间，并在收到成员消息时立即同步给房间里的其他成员

socket.io 为客户端提供了另一个npm包，直接安装

```sh
npm i socket.io-client
```

接下来就可以在页面上建立到服务器的连接啦

```js
import io from 'socket.io-client'

const socket = io() // 建立连接
```

向服务器发送消息

```diff
  const socket = io()
+ socket.emit('talk', '我打野，不给就送')
```

接收服务器发来的消息

```diff
  const socket = io()
+ socket.on('talk', message => {
+ })
```

李白看到了瑶的消息，强忍住问候对方家人的冲动，像哄那啥似地说道：

客户端的功能到这基本上也开发完了。核心api就是on和emit用于收发消息，既简单又优雅。

至此一个可以实时发送接收消息的聊天室就完成了，虽然简陋，但核心功能完备。

瑶最终倔强地打了野，李白选择了上路，3分钟被对面捶到高地，后羿在家里等鸟，吕布和貂蝉躲在蓝buff旁边的草丛里聊天，就这样在李白和瑶互相拉票举报对方的全局消息中游戏结束
