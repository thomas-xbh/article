---
link: https://zhuanlan.zhihu.com/p/596900689
title: React Hooks —— useCallback & useMemo & memo
description: useCallback &amp; useMemo &amp; memo本来这篇文章主要是想分享下 useCallback &amp; useMemo 这两个钩子的在 React 性能优化中使用，但是自己在学习的过程中，发现讨论这两个钩子离不开 React.memo。所以就一起…
keywords: React,前端开发,React Hooks
author: 剑大瑞
date: 2023-01-05T13:48:00.000Z
publisher: 知乎专栏
stats: paragraph=35 sentences=24, words=214
---
## useCallback & useMemo & memo

本来这篇文章主要是想分享下 `useCallback` & `useMemo` 这两个钩子的在 `React` 性能优化中使用，但是自己在学习的过程中，发现讨论这两个钩子离不开 `React.memo`。所以就一起聊聊吧。通过本篇文章你会可以了解到：

* `React.memo` 的使用，及如何解决 `React.memo` 穿透的问题
* `useCallback` & `useMemo` 的使用， `useCallback` 与 `useMemo` 的关系

`React.memo` 主要是用来缓存函数组件的。其作用类似于 `React.PureComponent`， `React.PureComponent` 用于优化类组件。

我们先看个例子：

上面的代码中，我们在子组件中通过 `console.log` 输出日志，标识子组件是否渲染。

当你点击父组件中的 `button` 的时候，会发现第二个子组件 `Counter` 的状态(state & props)没有发生变化，竟然也发生了更新。

要解决这个问题非常简单，只需使用 `React.memo` 对子组件进行缓存即可。

这时你再点击父组件，则不会触发子组件的更新了。

默认情况下 `React.memo` 会对被包裹的组件的 `props` 进行浅层的引用比较。如果 `props` 没有变化， `React.memo` 则会复用最近一次的函数组件。否则会更新函数组件。另外 `React.memo` 还可以接受第二个参数 `arePropsEqualFunction`

`arePropsEqualFunction` 函数可以接受两个参数： `prevProps` & `newProps`，即上一次的 `props` 和最新的 `props` 。你可以在 `arePropsEqualFunction` 函数内部通过自定义比较逻辑返回一个布尔值，来决定是否需要更新组件。通常情况下：

* 返回 `true`，表示两个 `props` 相等，组件不需要更新
* 返回 `false` 则相反。

当然在实际的业务开发中，我们组件中的数据源可能有多个渠道。这种情况下，被 `React.memo` 缓存的组件在以下两种情况下仍会执行更新逻辑：

* 在组件自身 `state` 发生变化时
* 当组件中有通过 `useContext` 钩子注入的数据发生变化时

当你点击按钮的时候，会发现 `ChildrenComponent` 组件会更新，但是 `Counter` 组件不会。

这意味着 `useContext` 会对 `React.memo` 的缓存效果造成破坏。在实际业务开发中，我们可以通过将 `React.memo` 注入的数据向上提取的方式，通过 `props` 向目标组件传递数据，解决这个问题。

在实际开发过程中，我们会通过 `props` 传递各种类型的变量。如果传递的变量是引用类型，比如 `{}` 或者 `function`。这就意味着父组件每次渲染都会更新被 `React.memo` 包裹的子组件。这是因为每次渲染的对象的引用地址都在发生变化： `{} !== {}`。

为了解决这两个问题，我们接下来聊聊 `useCallback` & `useMemo`。

我们先看一段代码：

当你点击 `UseMemoHook` 中的 `button` 的时候，会发现没有相关状态引用的 `Counter` 组件也更新了。这是因为每次 `ChildrenComponent` 更新都会产生新的 `changeCounter` 函数。

使用 `useCallback` 包裹作为 `props` 传递的函数：

这次在点击 `UseMemoHook` 中的 `button`，则会发现计数器组件不再被执行更新。

同样的道理，如果是对象类型作为 `props` 传递，我们可以使用 `useMemo` 来进行缓存：

当你点击父组件的 `button` 时，发现被 `React.memo` 包裹的子组件还是发生了更新操作。

为此我们可以使用 `useMemo` 包裹 `list` 的过滤操作：

这时再触发 `App` 中的点击事件， `ChildrenComponent` 则不会发生更新了。

> 另外官方文档中也有提到： `useCallback(fn, [])`其实就是 `useMemo(() => fn, deps)`。只不过 `useCallback`返回的是一个函数，这个函数通常会作为 `props`传递给子组件，而 `useMemo`会直接执行 `fn`，并缓存 `fn`返回的值，其应用场景更广一些，可以用于缓存一些组件中的计算开销昂贵的操作。

通过上文我们知道，在子组件没有使用 `React.memo` 缓存的情况下，只要父组件发生变化，子组件就会执行更新逻辑。另外为了解决 `props` 中引用类型的数据比较问题，比如函数我们引入了 `useCallback` 去缓存函数，以避免 `React.memo` 失效——这意味着实际开发中 `useCallback` 必须配合 `React.memo` 一起使用，单独使用 `useCallback` 毫无意义。

在实际的业务开发中，我们会遇到各种各样的场景：

* 有些组件与某些逻辑或状态耦合，但是需要在项目中复用
* 有的组件渲染可能非常耗时
* 有的组件已被 `React.memo` 缓冲，但是由于某些原因，同时引入了 `useContext`。

这种情况下我们可以使用 `useCallback` 或者 `useMemo` 解决问题：

这两种使用方式，在实际开发中少见。但是笔者确实有在实际开发中遇到，而且从整个项目的代码组织方式上来说收益还可以。

从上面的代码中，可以预见 `React Hooks`真的为用户提供了非常灵活的代码组织方式。写不好就是一顿骂。

* `React.memo` 可以使组件避免部分无意义渲染，但是需要结合 `useCallback` & `useMemo` 一起使用，另外需要注意 `useContext` 的穿透问题。
* `useCallback` & `useMemo` 钩子本质是为了缓存引用类型，保持引用地址不变，且 `useCallback` 必须配合 `React.memo` 一起使用，否则无意义。
* `useMemo` 本质上就是将 `useCallback` 缓存的函数直接执行，二者某些情况下可以相互替换
* `useCallback` & `useMemo` 在某些场景下也可以直接缓存组件或者 `JSX`，实现性能优化。

参考：
