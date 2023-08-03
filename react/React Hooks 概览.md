---
link: https://juejin.cn/post/6844904093824073742
title: React Hooks 概览
description: React 全部 Hooks 总结，以及 React Redux、React Router 中常用 Hooks 整理。
keywords: React.js
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2020-03-16T06:40:40.000Z
publisher: 稀土掘金
stats: paragraph=66 sentences=26, words=697
---
此文章只是整理了在 React 项目开发中常用的一些 Hooks 及简单用例，具体的用法及使用场景后续会持续跟新，并会加上链接。

## React Hooks

> Hooks 只能用于函数组件当中。

```js
import { useState } from 'react';

const Component = () => {
  const [count, setCount] = useState(0);

  return (
    <button onClick={() => setCount(count + 1)}>clickbutton>
  )
}
复制代码
```

此方法会返回两个值：当期状态和更新状态的函数。效果同 `this.state` 与 `this.setState`，区别是 `useState` 传入的值并不一定要对象，并且在更新的时候不会把当前的 state 与旧的 state 合并。

`useReducer` 接收两个参数，第一个是 reducer 函数，通过该函数可以更新 state，第二个参数为 state 的初始值，是 `useReducer` 返回的数组的第一个值，也是在 reducer 函数第一次被调用时传入的一个参数。

**基础用法**

```js
import { useReducer } from 'react';

const Component = () => {
  const [count, dispatch] = useReducer((count, action) => {
    switch (action) {
      case 'subtract':
        return count - 1;
      case 'add':
        return count + 1;
    }
  }, 0);

  return (
    <div>
      <span>{count}span>
      <button onClick={() => dispatch('subtract')}>subtractbutton>
      <button onClick={() => dispatch('add')}>addbutton>
    div>
  );
};
复制代码
```

在基础用法中，返回一个 dispatch 通过 dispatch 触发不同的 action 来加减 state。这里既然能传 `string` action 那么肯定也能传递更复杂的参数来面对更复杂的场景。

**进阶用法**

```js
import { useReducer } from 'react';

const Component = () => {
  const [userInfo, dispatch] = useReducer(
    (state, { type, payload }) => {
      switch (type) {
        case 'setName':
          return {
            ...state,
            name: payload
          };
        case 'setAge':
          return {
            ...state,
            age: payload
          };
      }
    },
    {
      name: 'Jace',
      age: 18
    }
  );

  return (
    <button onClick={() => dispatch({ type: 'setName', payload: 'John' })}>
      click
    button>
  );
};
复制代码
```

在上述案例 `useReducer` 中，我们将函数的参数改为一个对象，分别有 `type` 和 `payload` 两个参数， `type` 用来决定更新什么数据， `payload` 则是更新的数据。写过 react-redux 的同学可能发这个 reducer 与 react-redux 中的 reducer 很像，我们借助 react-redux 的思想可以实现一个对象部分更改的 reducer ，那么我们便可以使用 React Hooks 的 `useContext` 来实现一个状态管理。

```js
import { useMemo, createContext, useContext, useReducer } from 'react';

const store = createContext([]);

const App = () => {
  const reducerValue = useReducer(
    (state, { type, payload }) => {
      switch (type) {
        case 'setName':
          return {
            ...state,
            name: payload
          };
        case 'setAge':
          return {
            ...state,
            age: payload
          };
      }
    },
    {
      name: 'Jace',
      age: 18
    }
  );
  const [state, dispatch] = reducerValue;

  const storeValue = useMemo(() => reducerValue, reducerValue);

  return (
    <store.Provider value={storeValue}>
      <Child />
    store.Provider>
  );
};

const Child = () => {
  const [state, dispatch] = useContext(store);
  console.log(state);
  return (
    <button onClick={() => dispatch({ type: 'setName', payload: 'John' })}>
      click
    button>
  );
}

复制代码
```

```js
import { useState, useEffect } from 'react';

let timer = null;

const Component = () => {
  const [count, setCount] = useState(0);

  useEffect(() => {
    document.title = `You clicked ${count} times`;

    timer = setInterval(() => {

    }, 1000)

    return () => {

      clearInterval(timer);
    };
  }, [count]);

}
复制代码
```

如果 `useEffect` 第二个参数数组内的值发生了变化，那么 `useEffect` 第一个参数的回调将会被再执行一遍，这里要注意的 **useEffect 的返回值函数并不只是再组件卸载的时候执行，而是在这个 useEffect 被更新的时候也会调用，例如上述 count 发生变化后，useEffect 返回的方法也会被执行，具体原因请见[Using the Effect Hook – React (reactjs.org)](https://link.juejin.cn?target=https%3A%2F%2Freactjs.org%2Fdocs%2Fhooks-effect.html%23explanation-why-effects-run-on-each-update "https://reactjs.org/docs/hooks-effect.html#explanation-why-effects-run-on-each-update")**。

`useLayoutEffect` 与 `useEffect` 的API相同，区别： `useEffect` 在浏览器渲染后执行， `useLayoutEffect` 在浏览器渲染之前执行，由于JS是单线程，所以 `useLayoutEffect` 还会阻塞浏览器的渲染。区别就是这，那么应用场景肯定是从区别中得到的， `useLayoutEffect` 在渲染前执行，也就是说我们如果有状态变了需要依据该状态来操作DOM，为了避免状态变化导致组件渲染，然后更新 `DOM` 后又渲染，给用户肉眼能看到的闪烁，我们可以在这种情况下使用 `useLayoutEffect`。

> 当然这个不只是状态的改变，在任何导致组件重新渲染，而且又要改变 `DOM` 的情况下都是 `useLayoutEffect` 的使用场景。当然这种场景不多， `useLayoutEffect` 也不能多用，且使用时同步操作时长不能过长，不然会给用户带来明显的卡顿。

细心的同学有可能发现我在上面写 `useEffect` 中有一个 `timer` 变量，我将其定义在了函数组件外面，这样写简单使用是没问题的，但是如果该组件在同一页面有多个实例，那么组件外部的这个变量将会成共用的，会带来一个冲突，所以我们需要一个能在函数组件声明周期内部的变量，可以使用 `useState` 中的 state 但是 state 发生变化组件也会随之刷新，在有些情况是不需要刷新的，只是想单纯的存一个值，例如计时器的 `timer` 以及子组件的 Ref 实例等等。

```js
import React, { useRef, useState, useEffect } from 'react';

const Compnent = () => {
  const timer = useRef(null);
  const [count, setCount] = useState(0);

  useEffect(() => {
    clearInterval(timer.current);
    timer.current = setTimeout(() => {
      setCount(count + 1);
    }, 1000);
  }, [count]);

  return <div>UseRef count: {count}div>;
}
复制代码
```

`useRef` 只接受一个参数，就是 初始值，之后可以通过赋值 `ref.current` 来更改，我们可以将一些不影响组件声明周期的参数放在 ref 中，还可以将 ref 直接传递给子组件，子元素。

```js
const ref = useRef();

<div ref={ref}>Hellodiv>

<Child ref={ref} />
复制代码
```

或许有同学这时候会想到，当子组件为 Class 组件时，ref 获取的是 Class 组件的实例，上面包含 Class 的所有方法属性等。但当子组件为 Function 组件时，ref 能拿到什么，总不可能是 function 内定义的方法、变量。

```js
import React, { useRef, useState, useImperativeHandle } from 'react';

const App = () => {
  const ref = useRef();
  return (
      <Child ref={ref} />
  );
};

const Child = React.forwardRef((props, ref) => {
  const inputRef = useRef();
  const [value, setValue] = useState(1);

  useImperativeHandle(ref, () => ({
    value,
    setValue,
    input: inputRef.current
  }));

  return (
    <input value={value} inputRef={inputRef} />
  );
})
复制代码
```

使用 `useImperativeHandle` 钩子可以自定义将子组件中任何的变量，挂载到 ref 上。 `React.forwardRef` 方法可以让组件能接收到 ref ，然后再使用或者透传到更下层。

```js
import React, { useCallback } from 'react';

const Component = () => {
  const setUserInfo = payload => {};

  const updateUserInfo = useCallback(payload => {
    setUserInfo(Object.assign({}, userInfo, payload));
  }, [userInfo]);

  return (
    <UserCard updateUserInfo={updateUserInfo}/>
  )
}
复制代码
```

useCallback 会在二个参数的依赖项发生改变后才重新更新，如果将此函数传递到子组件时，每次父组件渲染此函数更新，就会导致子组件也重新渲染，可以通过传递第二个参数以避免一些非必要性的渲染。

```js
import React, { useMemo } from 'react';

const Component = () => {
  const [count, setCount] = useState(0);

  const sum = useMemo(() => {

    return sum;
  }, [count]);

  return <div>{sum}div>
}
复制代码
```

useMemo 的用法跟 useCallback 一样，区别就是一个返回的是缓存的方法，一个返回的是缓存的值。上述如果依赖值 count 不发生变化，计算 sum 的逻辑也就只会执行一次，从而性能。

使用 Hooks 能为开发提升不少效率，但并不代表就要抛弃 Class Component，依旧还有很多场景我们还得用到它，比如需要封装一个公共的可继承的组件，当然通过自定义 hooks 也能将一些共用的逻辑进行封装，以便再多个组件内共用。

> 在 React 的使用中很多第三方库也只有自己的 Hooks 都是基于 React 本身的 Hooks 上封装的，推荐一篇我后来写的文章 [在 React 中自定义 Hooks 的应用场景](https://juejin.im/post/6864871906567749645 "https://juejin.im/post/6864871906567749645") ，此篇文章主要讲自定义 Hooks 的应用场景。
