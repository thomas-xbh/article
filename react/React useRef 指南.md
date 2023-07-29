---
link: https://juejin.cn/post/7160215726773501965
title: React useRef 指南
description: 在本文中，我们将讨论使用 ref 访问 DOM 的 React `useRef` 钩子，以及 `ref` 和 `useRef` 之间的区别。
keywords: React.js,前端,掘金·日新计划
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2022-10-30T08:14:16.000Z
publisher: 稀土掘金
stats: paragraph=47 sentences=14, words=362
---
在各种 JavaScript 库和框架中，React 因其开发人员友好性和支持而受到认可。大多数开发人员发现 React 非常舒适，而且具有可扩展性，因为它提供了 hook。hook 是 React 自带的内置 API，允许开发人员与 React 中的 React 状态和生命周期特性进行交互。hook 不能在类内部工作，因此它们只能在函数组件内部使用。开发人员还可以创建自定义 hook。

在本文中，我们将讨论使用 ref 访问 DOM 的 React `useRef` 钩子，以及 `ref` 和 `useRef` 之间的区别。

## 1. useRef 是什么？

React 中包含的各种 hook 之一是 `useRef` hook，它用于在函数组件中引用对象，并在重新呈现之间保留被引用对象的状态。 `useRef` 有一个名为 `current` 的属性，用于在任何时候检索被引用对象的值，同时也接受一个初始值作为参数。你可以通过更新当前值来更改引用对象的值。

下面演示了如何创建一个引用对象：

```js
import { useRef } from 'react'

const myComponent = () => {
  const refObj = useRef(initialValue)

  return (

  )
}
复制代码
```

在上面的代码片段中，我们有一个对象 `refObj`，我们想在应用程序中引用它，为了访问这个值或更新它的值，我们可以像这样调用 `current` 属性：

```js

const handleRefUpdate = () => {

    const value = refObj.current

   refObj.current = newValue
}
复制代码
```

下面两点是你应该需要注意的：

* 被引用对象的值在重新渲染之间保持不变。
* 更新被引用对象的值不会触发重新渲染。

## 2. 使用 Ref 访问 DOM 元素

记住 DOM 元素也是对象，我们可以使用 `useRef` 引用它们。但现在，我们需要利用 `ref`。 `ref` 是一个 HTML 属性，它将一个被引用的对象分配给一个 DOM 元素。让我们看看这是如何的闪现：

```js
import {useRef} from 'react'

const myComponent = () => {
  const elementRef = useRef()

  return (
 		<input ref={elementRef} type="text" />
  )
}
复制代码
```

在上面的代码片段中，我们创建了一个新的引用对象 `elementRef`，并使用 `ref` 属性将其分配给一个 `input` 标签。我们可以访问 `input` 标签的值并像这样更新它的值：

```js
const handleInput = () => {

  const textValue = elementRef.current.value

  elementRef.current.value = "Hello World"
}
复制代码
```

在上面的代码片段中，我们创建了一个函数，该函数获取 `input` 元素的当前值并将其赋值给 `textValue`。我们还将 `input` 元素的值更新为 `"Hello World"`。

## 3. Ref 和 useRef 的区别

正如本文前几节所讨论的，我们可以清楚地理解 `useRef` 用于创建引用对象，而 `ref` 用于访问 DOM 节点或将 `render` 方法中的 react 组件分配给引用对象。另外，可以使用 `useRef` hook 或 `createRef` 函数创建 `ref`，这是其他方法无法实现的。

`useRef` 可以用来引用任何类型的对象，React `ref` 只是一个用于引用 DOM 元素的 DOM 属性。

## 4. Ref 和 useRef 实践

既然我们已经了解了 r `ef` 和 `useRef` 的工作原理及其区别，那么让我们看看如何在实际应用程序中使用它们。例如，我们希望为弹出窗口实现一个点击离开事件监听器。我们可以使用 `ref` 来访问弹出框的 DOM 元素，并在弹出框外进行单击时监听。

在 react 应用程序环境中，你可以创建一个名为"hooks"的文件夹，这个文件夹将包含自定义 hook。在文件夹中创建一个新文件 `useClickAway`，并在文件中输入以下代码：

```js
import React, { useEffect } from 'react'

export default function useClickAway(ref: any, callback: Function) {
  useEffect(() => {
    function handleClickAway(event: any) {
      if (ref.current && !ref.current.contains(event.target)) {
      	callback();
    	}
    }
    document.addEventListener("mousedown", handleClickAway);
    return () => {
    	document.removeEventListener("mousedown", handleClickAway);
    };
  }, [ref]);
 };
复制代码
```

在上面的代码片段中，我们创建了一个自定义 hook，它接受一个引用对象作为 `ref` 和一个回调函数，然后我们执行一个事件监听器来检查鼠标何时被单击，如果单击不在当前的 `ref` 上，那么我们就触发回调函数。

## 5. Ref 和 useRef 的使用场景

现在你已经充分了解了 `ref` 和 `useRef` 是什么以及它们是如何工作的。你现在可能在考虑什么时候使用，什么时候避免使用。 `ref` 和 `useRef` 都很容易被误用，这样做的代价非常高。

以下是一些可供参考的使用场景：

* 与 `input` 元素交互：通过使用引用，可以访问 `input` 元素并执行聚焦、变化跟踪或自动完成等功能。
* 与第三方 UI 库交互： `ref` 可用于与第三方 UI 库创建的元素交互，使用标准 DOM 方法访问这些元素可能比较困难。例如，如果你使用第三方库生成滑块，你可以使用 `ref` 来访问滑块的 DOM 元素，而不必知道滑块库的源代码结构。
* 媒体播放：你还可以使用引用访问媒体资源，如图像、音频或视频，并与它们的渲染方式进行交互。例如，当元素进入视口时，自动播放视频或延迟加载图像。
* 复杂动画触发：传统上，CSS keyframes 或 timeout 用来确定何时启动动画。在某些情况下（可能更加复杂），可以使用 `ref` 来观察 DOM 元素并确定何时启动动画。
* 在某些情况下，比如下面这种情况，你不应该使用引用：
  - 即使在使用 `ref` 的简单解决方案的情况下，也不需要编写更昂贵的代码来完成相同的任务。例如，使用条件渲染来隐藏或显示 DOM 元素，而不是引用。
  - 有时，使用引用的概念非常有趣，以至于你忽略了对元素的修改对应用程序生命周期的影响。你应该记住，对引用的更改不会导致重新渲染，并且引用在渲染之间保持其对象的值。因此，在状态变化需要触发重新渲染的情况下，避免使用引用是明智的。
* DOM 元素（不应与功能性组件混淆）可以使用 `ref` 属性引用。因为，与类组件或 DOM 元素不同，函数组件没有实例。例如：

```js
import {useRef} from 'react'

const FunctionalComponent = () => {
  return (
  	Hello World<>
  )
}

const myComponent = () => {
	const elementRef = useRef()

  return (
  	<FunctionalComponent ref={elementRef} />
  )
}
复制代码
```

因为组件 `FunctionalComponent` 没有实例，所以上面代码片段中的引用将不起作用。相反，我们可以将 `FunctionalComponent` 转换为类组件，或者在 `FunctionalComponent` 组件中使用 `forwardRef`。

## 6. 小结

在本文中，我们讨论了如何使用 `useRef` hook 创建引用，该 hook 接受一个初始值，并修改引用对象的 `current` 属性的值以更新其值。之后我们看到了如何使用 `current` 值和 `ref` 来访问 DOM 元素并与它们的属性交互。再接着我们介绍了如何创建一个自定义 hook，该 hook 接受引用 DOM 元素和一个回调函数，在应用程序中使用 `ref` 和 `useRef` 来观监听 DOM 元素上的单击事件。最后，我们还讨论了 `ref` 和 `useRef` 的用法，以及何时使用它们，何时不使用它们。

在了解了如何使用 `ref` 和 `useRef` 来跟踪和更新可变值而无需重新渲染父组件之后，你可以通过查看 React 官方文档了解更多关于 [Refs](https://link.juejin.cn?target=https%3A%2F%2Freactjs.org%2Fdocs%2Frefs-and-the-dom.html "https://reactjs.org/docs/refs-and-the-dom.html")和 [useRefs](https://link.juejin.cn?target=https%3A%2F%2Freactjs.org%2Fdocs%2Fhooks-reference.html%23useref "https://reactjs.org/docs/hooks-reference.html#useref")的内容，甚至可以尝试其他 React hook。
