---
link: https://zhuanlan.zhihu.com/p/348701319
title: useLayoutEffect和useEffect的区别
description: react hook面世已经有一段时间了，相信很多人都已经在代码中用上了hooks。而对于 useEffect 和 useLayoutEffect，我们使用的最多的应该就是useEffect。那他们两个到底有什么不一样的地方？使用方式这两个函数的使…
keywords: 前端框架,React,Hook
author: Buuug内推字节跳动全岗位～
date: 2022-03-13T14:36:00.000Z
publisher: 知乎专栏
stats: paragraph=22 sentences=1, words=90
---
react hook面世已经有一段时间了，相信很多人都已经在代码中用上了hooks。而对于 `useEffect` 和 `useLayoutEffect`，我们使用的最多的应该就是 `useEffect`。那他们两个到底有什么不一样的地方？

这两个函数的使用方式其实非常简单，他们都接受一个函数一个数组，只有在数组里面的值改变的情况下才会再次执行 effect。所以对于使用方式我就不过多介绍了，不清楚的可以先参考[官网](https://link.zhihu.com/?target=https%3A//zh-hans.reactjs.org/docs/hooks-reference.html)。

* `useEffect` 是异步执行的，而 `useLayoutEffect`是同步执行的。
* `useEffect` 的执行时机是浏览器完成渲染之后，而 `useLayoutEffect` 的执行时机是浏览器把内容真正渲染到界面之前，和 `componentDidMount` 等价。

我们用一个很简单的例子

这是它的效果

而换成 `useLayoutEffect` 之后闪烁现象就消失了

看到这里我相信你应该能理解他们的区别了，因为 `useEffect` 是渲染完之后异步执行的，所以会导致 hello world 先被渲染到了屏幕上，再变成 world hello，就会出现闪烁现象。而 `useLayoutEffect` 是渲染之前同步执行的，所以会等它执行完再渲染上去，就避免了闪烁现象。也就是说我们最好把操作 dom 的相关操作放到 `useLayouteEffect` 中去，避免导致闪烁。

也正是因为 `useLayoutEffect` 可能会导致渲染结果不一样的关系，如果你在 ssr 的时候使用这个函数会有一个 warning。

这是因为 `useLayoutEffect` 是不会在服务端执行的，所以就有可能导致 ssr 渲染出来的内容和实际的首屏内容并不一致。而解决这个问题也很简单：

当你使用 `useLayoutEffect` 的时候就用 `useCustomLayoutEffect` 代替。这样在服务端就会用 `useEffect` ，这样就不会报 warning 了。

那么 `useEffect` 和 `useLayoutEffect` 到底是在什么时候被调用的呢？我们去源码中一探究竟。

首先找到 `useEffect` 调用的入口

调用 `updateEffectImpl` 时传入的 `hookEffectTag` 为 Passive$1, 所以我们找一下：Passive$1。

然后我们找到是在这里传入了 Passive$1 类型来调用 `useEffect` 。

那我们继续顺藤摸瓜找 `commitPassiveHookEffects`

老样子，找 `flushPassiveEffectsImpl`

再往上一层是 `commitBeforeMutationEffects`，这里面调用 `flushPassiveEffects`的方法是 `scheduleCallback`，这是一个调度操作，是异步执行的。

继续顺着 `commitBeforeMutationEffects`方法往上找的话，我们可以找到最终调用 useEffect 的地方是 `commitRootImpl` ，这是我们 commit 阶段会调用的一个函数，所以就是在这里面对 `useEffect` 进行了调度，在完成渲染工作以后去异步执行了 `useEffect` 。

老样子，从入口找起

这里传进去的 `hookEffectTag` 是 `Layout`，那么我们找一下 `Layout`。

而在这里我们可以看到，class 组件的 `componentDidMount`生命周期也是在这里被调用的，所以其实 `useLayoutEffect`是和 `componentDidMount`等价的。

而一直往上找最后还是会找到 `commitRootImpl`方法中去，同时在这个过程中并没有找到什么调度的方法，所以 `useLayoutEffect`会同步执行。
