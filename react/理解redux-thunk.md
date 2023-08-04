---
link: https://zhuanlan.zhihu.com/p/85403048
title: 理解redux-thunk
description: 前言前面我们已经用了三篇文章详细介绍了 Redux 的概念、原理及 Middleware 机制。今天我们来看一个 Redux 官方出品的 middleware 库： redux-thunk。 可能大部分用了 Redux 的项目都会用到redux-thunk，但你有没…
keywords: Redux,前端开发,JavaScript
author: Fancy
date: 2019-10-06T15:05:00.000Z
publisher: 知乎专栏
stats: paragraph=23 sentences=4, words=157
---
前面我们已经用了三篇文章详细介绍了 Redux 的概念、原理及 Middleware 机制。今天我们来看一个 Redux 官方出品的 middleware 库： `redux-thunk`。
可能大部分用了 Redux 的项目都会用到 `redux-thunk`，但你有没有想过这个库到底是用来干嘛的？如果我不用它行不行？这篇文章我们就来详细聊一下这个库。
其实很早之前我就看过它的代码，看到它的代码量的时候被震惊了，没想到一个 GitHub 上 Star 数过万的项目，总的代码行数只有 10 行左右（我当时看的是 1.x 版本，代码量只有 8 行）。虽然我不喜欢用代码行数来衡量一个项目，但这么少的代码量当时还是觉得挺诧异的。

首先，我们还是来看一下这个库的用法。 `redux-thunk`是作为 `redux`的 middleware 存在的，用法和普通 middleware 的用法是一样的，注册 middleware 的代码如下：

注册后可以这样使用：

可以看到， `redux-thunk`主要的功能就是可以让我们 `dispatch`一个函数，而不只是普通的 Object。后面我们会看到，这一点改变可以给我们巨大的灵活性。
了解了如何使用，接下来我们看一下它的实现原理。

## **什么是 thunk？**

我记得我在很长的时间里都把 `redux-thunk`的名字看成了 `redux-thank`，理解成了感谢 redux。。。其实我觉得这个库最令人迷惑的地方之一就是它的名字。其实 `thunk`是函数编程届的一个专有名词，主要用于_calculation delay_，也就是延迟计算。
用代码演示如下：

可以看到，这种代码的模式是非常简单的，以前我们可能都写过类似这样的代码，只是不知道这种代码叫做 `thunk`而已。

## **redux-thunk 源码**

由于 `redux-thunk`的代码量非常少，我们直接把它的代码贴上来看一下。这里我们看的是最新版的 `v2.3.0`的代码：

如果你看了前几篇文章，对 Redux 及它的 middleware 机制有所了解，那么上面这段代码是非常容易理解的。 `redux-thunk`就是一个标准的 Redux middleware。
它的核心代码其实只有两行，就是判断每个经过它的 `action`：如果是 `function`类型，就调用这个 `function`（并传入 dispatch 和 getState 及 extraArgument 为参数），而不是任由让它到达 reducer，因为 reducer 是个纯函数，Redux 规定到达 reducer 的 action 必须是一个 plain object 类型。<br> `redux-thunk`的原理就这么多，是不是非常简单？

`redux-thunk`的代码和原理非常简单，但我觉得难的部分是_为什么需要这样一个库_。关于 `redux-thunk`的起源可以看一下 Redux 001 号的 issue: **How to dispatch many actions in one action creator[1]**

大概意思就是问如何一次性发起多个 action，然后作者回答我可以让 actionCreator 返回一个函数。然后相关的 PR 如下： **fix issue 001[2]**

那个时候 `redux-thunk`还没有独立，而是写在 `redux`的 action 分发函数中的一个代码分支而已。和现在的 `redux-thunk`逻辑一样，它会判断如果传入的 action 是一个 `function`，就调用这个函数。现在将 `redux-thunk`独立出去，用 middleware 的方式实现，会让 redux 的实现更加统一。
看到这里，其实我们对 `redux-thunk`感到迷惑很大部分原因就是它涉及的 `thunk`等的概念比较陌生而已，其实你大可以将它的名字理解为_redux-function_，也就是它只是让 dispatch 支持函数，仅此而已。

## **为什么需要？**

现在我们理解了 `redux-thunk`可以让我们 dispatch 一个 function，但是这有什么用呢？其实我觉得这是一项基础设施，虽然功能简单，但可扩展性极其强大。

比如很多时候我们需要在一个函数中写多次 dispatch。这也是上面 issue 中提到的问题。比如上面我们示例代码中，我们定义了 login 函数做 API 请求，在请求发出前我们可能需要展示一个全局的 loading bar，在请求结束后我们又需要将请求结果存储到 redux store 中。这都需要用到 redux 的 dispatch。

当然在一个函数中写多个 dispatch 只是我们可以做的事情之一，既然它是一个 function，而且并不要求像 reducer 一样是 pure function，那么我们可以在其中做任意的事情，也就是有副作用(side effect)的事情。

`redux-thunk`是一个非常小的 library，但我觉得理解它的概念对于我们理解 redux 是至关重要的。它和另一个非常流行的库 `redux-saga`一样，都是在 redux 中做_side effect_必不可少的。之后有时间我们会介绍一下 `redux-saga`，如果感兴趣欢迎关注公众号。

最后附上前两篇关于 Redux 的文章，请参考：

_写文章不易，如果这篇文章帮助到了你，请帮忙关注和转发～ 谢谢_

[1]

[2]

最后，还是请大家扫码关注一下我的个人公众号：
