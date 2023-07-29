---
link: https://juejin.cn/post/7074938135544594463
title: 重新认识 React.useCallback
description: useCallback的常见误区：是为了减少不必要的函数创建吗？结合memo使用就能做到性能优化了？
keywords: React.js
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2022-03-14T12:57:02.000Z
publisher: 稀土掘金
stats: paragraph=118 sentences=59, words=952
---
## 官方解释

> Returns a [memoized](https://link.juejin.cn?target=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FMemoization "https://en.wikipedia.org/wiki/Memoization") callback. Pass an inline callback and an array of dependencies. useCallback will return a memoized version of the callback that only changes if one of the dependencies has changed. This is useful when passing callbacks to optimized child components that rely on reference equality to prevent unnecessary renders.

说人话就是，传递一个内联回调和一组依赖项。useCallback 将返回回调的一个备忘录版本，只有在其中一个依赖项发生更改时才会更改。这在将回调传递给依赖引用相等性的优化子组件以防止不必要的渲染。

## 知识考验

先来做一道题灵魂拷问一下自己～

> 假设此代码出现在 React Hook 组件中， 在每次 render 的情况下

1. ComponentA 中的 函数 a 会被创建多少次？
2. ComponentB 中的 匿名函数 () => {} 会被创建多少次？ 选项：

* a. 1 & 1
* b. 1 & c 变化时才会重新创建

```js
const ComponentA = () => {
  const a = () => {}
  return <button onClick={a}>Abutton>
}

const ComponentB = () => {
  const b = useCallback(() => {}, [c])
  return <button onClick={b}>Bbutton>
}
复制代码
```

## 误区一

> useCallback 当依赖没有变化时，不会创建新的函数，直接返回旧的函数。所有的函数都应该使用 useCallback 来包裹，减少不必要的函数创建。

其实换种写法就会容易理解：

```js

const b = () => {};

const a = useCallback(b, []);
复制代码
```

再来看看 useCallBack 的部分源码：

```js
function updateCallback(callback: T, deps: Array | void | null): T {
  const hook = updateWorkInProgressHook();
  const nextDeps = deps === undefined ? null : deps;
  const prevState = hook.memoizedState;

  if (prevState !== null) {
    if (nextDeps !== null) {

      const prevDeps: Array | null = prevState[1];

      if (areHookInputsEqual(nextDeps, prevDeps)) {

        return prevState[0];
      }
    }
  }

  hook.memoizedState = [callback, nextDeps];
  return callback;
}
复制代码
```

**结论：无论使不使用 useCallback 每次渲染都是会新创建一个函数的，不会因依赖没有变化而减少，只是返回的函数是新创建的函数还是已经缓存下来的函数。**

## 使用场景

既然 useCallback 的目的不是为了减少函数创建的次数，那什么场景应该使用 useCallback 呢？

> This is useful when passing callbacks to optimized child components that rely on reference equality to prevent unnecessary renders.

useCallback 其实是利用 memoize 减少不必要的子组件重新渲染，需要搭配 React.memo 或 shouldComponentUpdate 一起使用。

```js
import React, { useState } from 'react'

const Button = ({ handleClick }) => {
    return (
        <button onClick={handleClick}>Click!button>
    )
}

const Index = () => {
    const [clickCount, increaseCount] = useState(0);

    const handleClick = () => {
        increaseCount(count => count + 1);
    }

    return (
        <div>
            <p>{clickCount}p>
            <Button handleClick={ handleClick } />
        div>
    )
}

export default Index
复制代码
```

会发现每次点击【Click!】按钮都会造成 Button 组件的重新渲染，原因如下：

1. Index组件state发生变化，导致组件重新渲染；
2. 进而导致子组件Button也重新渲染。

React Class 组件可以使用 shouldComponentUpdate 或 PureComponent 来阻止该组件的渲染，Hook 组件可以使用高阶组件 React.memo 来实现该功能。

```js

import React, { useState, memo } from 'react'

const Button = memo（({ handleClick }) => {
    return (
        <button onClick={handleClick}>Click!button>
    )
}）

const Index = () => {
    const [clickCount, increaseCount] = useState(0);

    const handleClick = () => {
        increaseCount(count => count + 1);
    }

    return (
        <div>
            <p>{clickCount}p>
            <Button handleClick={ handleClick } />
        div>
    )
}

export default Index
复制代码
```

发现点击【Click!】按钮依然会造成 Button 组件的重新渲染，原因如下：

1. hook组件不同于 class 组件，class 组件中的方法会以实例的方式保存起来，内部的方法只会在实例化的时候创建一次，但 hook 是一个函数，每次 render 相当于重新执行了一遍该函数，导致内部函数 handleClick 每次都重新创建；
2. 进而导致子组件Button也重新渲染。

目标很明确，只要每次返回的函数都是同一个即可，这时 useCallback 就派上用场了！

```js
import React, { useState, useCallback, memo } from 'react'

const Button = memo(({ handleClick }) => {
    return (
        <button onClick={handleClick}>Click!button>
    )
})

const Index = () => {
    const [clickCount, increaseCount] = useState(0);

    const handleClick = useCallback(() => {
        increaseCount(count => count + 1);
    },[])

    return (
        <div>
            <p>{clickCount}p>
            <Button handleClick={ handleClick } />
        div>
    )
}

export default Index
复制代码
```

特殊情况：如果 useCallback 的依赖是时刻变化的，那 useCallback 就无效了 上面的例子改一下写法就会产生这样的问题

```js
import React, { useState, useCallback, memo } from 'react'

const Button = memo(({ handleClick }) => {
    return (
        <button onClick={handleClick}>Click!button>
    )
})

const Index = () => {
    const [clickCount, increaseCount] = useState(0);

    const handleClick = useCallback(() => {
        increaseCount(clickCount + 1);
    }, [clickCount])

    return (
        <div>
            <p>{clickCount}p>
            <Button handleClick={ handleClick } />
        div>
    )
}

export default Index
复制代码
```

解法一：官网给出的方案[使用 useEffect 和 useRef 结合](https://link.juejin.cn?target=https%3A%2F%2Freactjs.org%2Fdocs%2Fhooks-faq.html%23how-to-read-an-often-changing-value-from-usecallback "https://reactjs.org/docs/hooks-faq.html#how-to-read-an-often-changing-value-from-usecallback")

```js
import React, { useState, useEffect, useRef, useCallback } from 'react'

const Button = memo(({ handleClick }) => {
	return (
		<button onClick={handleClick}>Click!button>
	)
})

const Index = () => {
	const [clickCount, increaseCount] = useState(0);
	const countRef = useRef();

	useEffect(() => {
		countRef.current = clickCount
	})

	const handleClick = useCallback(() => {

		increaseCount(countRef.current + 1);
	},[])

	return (
		<div>
			<p>{clickCount}p>
			<Button handleClick={ handleClick } />
		div>
	)
}

export default Index
复制代码
```

官方给出的例子中也可以抽出一个常用的 hook，缺点就是依然需要关注相关的依赖。

解法二：[ahooks 之 useMemoizedFn / usePersistFn](https://link.juejin.cn?target=https%3A%2F%2Fahooks.js.org%2Fhooks%2Fuse-memoized-fn "https://ahooks.js.org/hooks/use-memoized-fn")

```js
import React, { useState, memo } from 'react'
import { useMemoizedFn } from 'ahooks'

const Button = memo(({ handleClick }) => {
	return (
		<button onClick={handleClick}>Click!button>
	)
})

const Index = () => {
	const [clickCount, increaseCount] = useState(0);

	const handleClick = useMemoizedFn(() => {
		increaseCount(clickCount + 1);
	})

	return (
		<div>
			<p>{clickCount}p>
			<Button handleClick={ handleClick } />
		div>
	)
}

export default Index
复制代码
```

也一起来看看 useMemoizedFn 的源码看看它是怎么实现的：

```typescript
import { useMemo, useRef } from 'react';

function useMemoizedFn(fn) {
  const fnRef = useRef(fn);

  fnRef.current = useMemo(() => fn, [fn]);

  const memoizedFn = useRef();

  if (!memoizedFn.current) {
    memoizedFn.current = function (this, ...args) {

      return fnRef.current.apply(this, args);
    };
  }

  return memoizedFn.current;
}

export default useMemoizedFn;
复制代码
```

## 「 不要过早的性能优化 」

经常能够看到很多性能优化的都会说，尽量减少不必要的渲染。就会有人想着那就给所有的 函数 和 组件都加上 useCallback 和 memo 就完美了。

如果真的所有东西都缓存起来更好的话，React 为什么不内置这个功能，而是暴露出来供开发使用呢？

在进行性能优化之前我们先来思考两个问题：

1. 是不是只要阻止组件产生不必要的 render 就一定代表优化成功？
2. useCallback还是有一些缺点的，例如执行了更多的代码，并且会保留旧函数的引用，导致浏览器无法GC（经典的闭包问题）是否就代表使用就是不好的？ 其中的问题就是如何去找到一个平衡点，并且有可量化的数据来证明哪种方案更优。而不是我觉得这么做就可以解决该性能问题。

那什么数据可以表明加了缓存之后是会有用的呢？—— **组件的渲染时间前后的对比**

### 工具：React Developer Tools

安装：[插件安装地址](https://link.juejin.cn?target=https%3A%2F%2Fchrome.google.com%2Fwebstore%2Fdetail%2Freact-developer-tools%2Ffmkadmapgofadopljbjfkapdkoienihi%3Futm_source%3Dchrome-ntp-icon "https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?utm_source=chrome-ntp-icon") 介绍：

1. 基本面板

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/aac5913f4ba64a0faf76106fd92b74ef~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image)

1. 打开设置可以记录组件 rendered 的原因

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1e19a00c4f9c4b84aed8b03de8db0af9~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image)

1. 可以高亮发生 render 的组件

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/370b9f24df50460a86289febd0fbe52e~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image)

### 案例一：长列表渲染使用前后渲染时间对比

这里模拟了渲染 1000 个列表的场景，并录制点击按钮后的渲染情况。 优化前：Index 和 LongList 没有添加任何的 memoized。每次点击「Add Count」按钮都会重新渲染 LongList。

```typescript
import React, { useState } from 'react';

const Long = new Array(1000).fill(1);

type LongListType = {
  onAdd: () => void;
};

const LongList: React.FC<LongListType> = ({ onAdd }) => (
  <div>
    <button onClick={onAdd}>Add Countbutton>
    {Long.map((i) => (
      <div>List 1div>
    ))}
  div>
);

const Index = () => {
  const [count, setCount] = useState(0);

  const onAdd = () => {
    setCount((count) => count + 1);
  };

  return (
    <>
      <p>Count: {count}p>
      <LongList onAdd={onAdd} />
    </>
  );
};

export default Index;
复制代码
```

从图中可以看到点击事件后的 render 总共花费了 6.8 ms，鼠标移至对应的组件上可以看到这个组件渲染的原因 ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/54a87f67a6ac4e74a927b86f0256000d~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image) 优化后：给 LongList 添加 memo，给 onAdd 添加 useCallback后，每次点击按钮不会导致 LongList 组件重新渲染。

```typescript
import React, { useState, useCallback, memo } from 'react';

const Long = new Array(1000).fill(1);

const LongList: React.FC = memo(({ onAdd }) => (
  <div>
    <button onClick={onAdd}>Add Countbutton>
    {Long.map((i) => (
      <div>List 1div>
    ))}
  div>
));

const Index = () => {
  const [count, setCount] = useState(0);

  const onAdd = useCallback(() => {
    setCount((count) => count + 1);
  }, []);

  return (
    <>
      <p>Count: {count}p>
      <LongList onAdd={onAdd} />
    </>
  );
};

export default Index;
复制代码
```

添加 useCallback 和 memo 后，点击 button，可以看到 Index 组件整体的渲染时间为 0.4ms，比未添加整体渲染时间 6.8 ms 减少了 94%。 ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/770cf8c7ebdd4e43a7a3a82aa638d0a4~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image)

### 案例二：使用场景中的三个案例前后渲染对比

代码第一次 Index 组件渲染时长点击Click按钮后Index渲染时长没有使用 useCallback、useMemoizedFn、memo
![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7e0f6db77f9642d699c9b175a142085b~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image)![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/209ea3732cdc42a3ad3494fb4b966983~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image)

使用 useCallback & memo 「优化」后
![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a1ea823f41a64ed28f59b11a9917e212~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image)![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/22ffd65d51844dc3b22ccd669f372f0b~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image)

使用 useMemoizedFn & memo 「优化」后
![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/def7bb8ab2344bad83b3c5a6e92315d7~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image)![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/496d318cd5d945edb9697a095985af67~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image) **结论：优化前的渲染时长比优化后的渲染时长更短**

没有进行任何优化的 Index 初次渲染时长最短，仅需 0.2 ms，优化后的 Index 初次渲染时长均需要 0.4ms点击按钮后的 render 渲染时长几乎无差别，均在 0.4-0.5ms之间

## 结论

1. **无论使不使用 useCallback 每次渲染都是会新创建一个函数的，不会因依赖没有变化而减少，只是返回的函数是新创建的函数还是已经缓存下来的函数。**
2. **useCallback 需要和 memo 结合一起使用，若遇到依赖是经常变化的 state 可使用 useMemoizedFn 进行封装。**
3. **所有的组件渲染优化可使用 React Profiler 工具结合数据来进行测量优化是否有效。**
