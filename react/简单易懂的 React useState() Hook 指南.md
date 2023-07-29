---
link: https://juejin.cn/post/6844903999083118606
title: 简单易懂的 React useState() Hook 指南（长文建议收藏）
description: 状态是隐藏在组件中的信息，组件可以在父组件不知道的情况下修改其状态。我更偏爱函数组件，因为它们足够简单，要使函数组件具有状态管理，可以useState() Hook。 本文会逐步讲解如何使用useState() Hook。此外，还会介绍一些常见useState() 坑。 可以找…
keywords: React.js,JavaScript
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2019-11-18T00:05:04.000Z
publisher: 稀土掘金
stats: paragraph=167 sentences=67, words=987
---
> 作者：Dmitri Pavlutin
译者：前端小智
来源：dmitripavlutin.com

状态是隐藏在组件中的信息，组件可以在父组件不知道的情况下修改其状态。我更偏爱函数组件，因为它们足够简单，要使函数组件具有状态管理，可以 `useState()` Hook。

本文会逐步讲解如何使用 `useState()` Hook。此外，还会介绍一些常见 `useState()` 坑。

## 1.使用 `useState()` 进行状态管理

无状态的函数组件没有状态，如下所示(部分代码)：

```js
import React from 'react';

function Bulbs() {
  return <div className="bulb-off" />;
}
复制代码
```

运行效果：

这时，要如何添加一个按钮来打开/关闭灯泡呢？ 为此，咱们需要具有状态的函数组件，也就是有状态函数组件。

`useState()`是实现灯泡开关状态的 Hoook，将状态添加到函数组件需要 `4`个步骤:启用状态、初始化、读取和更新。

要将 `<bulbs></bulbs>` 转换为有状态组件，需要告诉 React：从 `'react'`包中导入 `useState`钩子，然后在组件函数的顶部调用 `useState()`。

大致如下所示：

```js
import React, { useState } from 'react';

function Bulbs() {
  ... = useState(...);
  return <div className="bulb-off" />;
}
复制代码
```

在 `Bulbs`函数的第一行调用 `useState()`（暂时不要考Hook的参数和返回值）。 重要的是，在组件内部调用 Hook 会使该函数成为有状态的函数组件。

启用状态后，下一步是初始化它。

始时，灯泡关闭，对应到状态应使用 `false`初始化 Hook：

```js
import React, { useState } from 'react';

function Bulbs() {
  ... = useState(false);
  return <div className="bulb-off" />;
}
复制代码
```

`useState(false)`用 `false`初始化状态。

启用和初始化状态之后，如何读取它?来看看 `useState(false)`返回什么。

当 hook `useState(initialState)`被调用时，它返回一个数组，该数组的第一项是状态值

```ini
const stateArray = useState(false)
stateArray[0]
复制代码
```

咱们读取组件的状态

```ini
function Bulbs() {
  const stateArray = useState(false)
  return className={stateArray[0] ? 'bulb-on' : 'bulb-off'} />
}
复制代码
```

`useState(false)`返回一个数组，第一项包含状态值，该值当前为 `false`(因为状态已用 `false`初始化)。

咱们可以使用数组解构来将状态值提取到变量 `on`上：

```csharp
import React, { useState } from 'react';

function Bulbs() {
  const [on] = useState(false);
  return on ? 'bulb-on' : 'bulb-off'} />;
}
复制代码
```

`on`状态变量保存状态值。

状态已经启用并初始化，现在可以读取它了。但是如何更新呢?再来看看 `useState(initialState)`返回什么。

####1.4 更新状态

**用值更新状态**

咱们已经知道， `useState(initialState)`返回一个数组，其中第一项是状态值，第二项是一个更新状态的函数。

```scss
const [state, setState] = useState(initialState);

setState(newState);

复制代码
```

要更新组件的状态，请使用新状态调用更新器函数 `setState(newState)`。组件重新渲染后，状态接收新值 `newState`。

当点击 `&#x5F00;&#x706F;`按钮时将灯泡开关状态更新为 `true`，点击 `&#x5173;&#x706F;`时更新为 `false`。

```js
import React, { useState } from 'react';

function Bulbs() {
  const [on, setOn] = useState(false);

  const lightOn = () => setOn(true);
  const lightOff = () => setOn(false);

  return (
    <>
      <div className={on ? 'bulb-on' : 'bulb-off'} />
      <button onClick={lightOn}>开灯button>
      <button onClick={lightOff}>关灯button>
    </>
  );
}
复制代码
```

单击开灯按钮时， `lightOn()`函数将 `on`更新为 `true`: `setOn(true)`。单击关灯时也会发生相同的情况，只是状态更新为 `false`。

状态一旦改变，React 就会重新渲染组件， `on`变量获取新的状态值。

状态更新作为对提供一些新信息的事件的响应。这些事件包括按钮单击、HTTP 请求完成等，确保在事件回调或其他 Hook 回调中调用状态更新函数。

**使用回调更新状态**

当使用前一个状态计算新状态时，可以使用回调更新该状态:

```ini
const [state, setState] = useState(initialState)
...

setState(prevState => nextState)

...

复制代码
```

下面是一些事例：

```scss

const [toggled, setToggled] = useState(false);
setToggled(toggled => !toggled);

const [count, setCount] = useState(0);
setCount(count => count + 1);

const [items, setItems] = useState([]);
setItems(items => [...items, 'New Item']);
复制代码
```

接着，通过这种方式重新实现上面电灯的示例：

```js
import React, { useState } from 'react';

function Bulbs() {
  const [on, setOn] = useState(false);

  const lightSwitch = () => setOn(on => !on);

  return (
    <>
      <div className={on ? 'bulb-on' : 'bulb-off'} />
      <button onClick={lightSwitch}>开灯/关灯button>
    </>
  );
}
复制代码
```

`setOn(on => !on)`使用函数更新状态。

* 调用 `useState()` Hook 来启用函数组件中的状态。
* `useState(initialValue)`的第一个参数 `initialValue`是状态的初始值。
* `[state, setState] = useState(initialValue)`返回一个包含 `2`个元素的数组:状态值和状态更新函数。
* 使用新值调用状态更新器函数 `setState(newState)`更新状态。或者，可以使用一个回调 `setState(prev => next)`来调用状态更新器，该回调将返回基于先前状态的新状态。
* 调用状态更新器后，React 确保重新渲染组件，以使新状态变为当前状态。

## 2. 多种状态

通过多次调用 `useState()`，一个函数组件可以拥有多个状态。

```scss
function MyComponent() {
  const [state1, setState1] = useState(initial1);
  const [state2, setState2] = useState(initial2);
  const [state3, setState3] = useState(initial3);

}
复制代码
```

需要注意的，要确保对 `useState()`的多次调用在渲染之间始终保持相同的顺序(后面会讲)。

我们添加一个按钮 `&#x6DFB;&#x52A0;&#x706F;&#x6CE1;`，并添加一个新状态来保存灯泡数量，单击该按钮时，将添加一个新灯泡。

新的状态 `count` 包含灯泡的数量，初始值为 `1`：

```js
import React, { useState } from 'react';

function Bulbs() {
  const [on, setOn] = useState(false);
  const [count, setCount] = useState(1);

  const lightSwitch = () => setOn(on => !on);
  const addBulbs = () => setCount(count => count + 1);

  const bulb = <div className={on ? 'bulb-on' : 'bulb-off'} />;
  const bulbs = Array(count).fill(bulb);

  return (
    <>
      <div className="bulbs">{bulbs}div>
      <button onClick={lightSwitch}>开/关button>
      <button onClick={addBulbs}>添加灯泡button>
    </>
  );
}
复制代码
```

* [on, setOn] = useState(false) 管理开/关状态
* `[count, setCount] = useState(1)`管理灯泡数量。

多个状态可以在一个组件中正确工作。

## 3.状态的延迟初始化

每当 React 重新渲染组件时，都会执行 `useState(initialState)`。 如果初始状态是原始值（数字，布尔值等），则不会有性能问题。

当初始状态需要昂贵的性能方面的操作时，可以通过为 `useState(computeInitialState)`提供一个函数来使用状态的延迟初始化，如下所示：

```js
function MyComponent({ bigJsonData }) {
  const [value, setValue] = useState(function getInitialState() {
    const object = JSON.parse(bigJsonData);
    return object.initialValue;
  });

}
复制代码
```

## 4. useState() 中的坑

现在咱们基本已经初步掌握了如何使用 `useState()`，尽管如此，咱们必须注意在使用 `useState()`时可能遇到的常见问题。

在使用 `useState()` Hook 时，必须遵循 Hook 的规则

来看看 `useState()`的正确用法和错误用法的例子。

**有效调用 `useState()`**

`useState()`在函数组件的顶层被正确调用

```csharp
function Bulbs() {

  const [on, setOn] = useState(false);

}
复制代码
```

以相同的顺序正确地调用多个 `useState()`调用:

```scss
function Bulbs() {

  const [on, setOn] = useState(false);
  const [count, setCount] = useState(1);

复制代码
```

`useState()`在自定义钩子的顶层被正确调用

```csharp
function toggleHook(initial) {

  const [on, setOn] = useState(initial);
  return [on, () => setOn(!on)];
}

function Bulbs() {
  const [on, toggle] = toggleHook(false);

}
复制代码
```

** `useState()` 的无效调用**

在条件中调用 `useState()`是不正确的：

```scss
function Switch({ isSwitchEnabled }) {
  if (isSwitchEnabled) {

    const [on, setOn] = useState(false);
  }

}
复制代码
```

在嵌套函数中调用 `useState()`也是不对的

```js
function Switch() {
  let on = false;
  let setOn = () => {};

  function enableSwitch() {

    [on, setOn] = useState(false);
  }

  return (
    <button onClick={enableSwitch}>
      Enable light switch state
    button>
  );
}
复制代码
```

闭包是一个从外部作用域捕获变量的函数。

闭包（例如事件处理程序，回调）可能会从函数组件作用域中捕获状态变量。 由于状态变量在渲染之间变化，因此闭包应捕获具有最新状态值的变量。否则，如果闭包捕获了过时的状态值，则可能会遇到过时的状态问题。

来看看一个过时的状态是如何表现出来的。组件 `<delayedcount></delayedcount>`延迟 `3`秒计数按钮点击的次数。

```js
function DelayedCount() {
  const [count, setCount] = useState(0);

  const handleClickAsync = () => {
    setTimeout(function delay() {
      setCount(count + 1);
    }, 3000);
  }

  return (
    <div>
      {count}
      <button onClick={handleClickAsync}>Increase asyncbutton>
    div>
  );
}
复制代码
```

`delay()` 是一个过时的闭包，它从初始渲染（使用 `0`初始化时）中捕获了过时的 `count`变量。

为了解决这个问题，使用函数方法来更新 `count`状态：

```js
function DelayedCount() {
  const [count, setCount] = useState(0);

  const handleClickAsync = () => {
    setTimeout(function delay() {
      setCount(count => count + 1);
    }, 3000);
  }

  return (
    <div>
      {count}
      <button onClick={handleClickAsync}>Increase asyncbutton>
    div>
  );
}
复制代码
```

现在 `etCount(count => count + 1)`在 `delay()`中正确更新计数状态。React 确保将最新状态值作为参数提供给更新状态函数，过时闭包的问题解决了。

`useState()`用于管理简单状态。对于复杂的状态管理，可以使用 `useReducer()` hook。它为需要多个状态操作的状态提供了更好的支持。

假设需要编写一个最喜欢的电影列表。用户可以添加电影，也可以删除已有的电影，实现方式大致如下：

```scss
import React, { useState } from 'react';

function FavoriteMovies() {
  const [movies, setMovies] = useState([{ name: 'Heat' }]);

  const add = movie => setMovies([...movies, movie]);

  const remove = index => {
    setMovies([
      ...movies.slice(0, index),
      ...movies.slice(index + 1)
    ]);
  }

  return (
    // Use add(movie) and remove(index)...

  );
}
复制代码
```

状态列表需要几个操作:添加和删除电影，状态管理细节使组件混乱。

更好的解决方案是将复杂的状态管理提取到 `reducer`中：

```js
import React, { useReducer } from 'react';

function reducer(state, action) {
  switch (action.type) {
    case 'add':
      return [...state, action.item];
    case 'remove':
      return [
        ...state.slice(0, action.index),
        ...state.slice(action.index + 1)
      ];
    default:
      throw new Error();
  }
}

function FavoriteMovies() {
  const [state, dispatch] = useReducer(reducer, [{ name: 'Heat' }]);

  return (

  );
}
复制代码
```

`reducer`管理电影的状态，有两种操作类型：

* `"add"`将新电影插入列表
* `"remove"`从列表中按索引删除电影

还有一个好处:可以将 `reducer` 提取到一个单独的模块中，并在其他组件中重用它。另外，即使没有组件，也可以对 `reducer` 进行单元测试。

这就是关注点分离的威力:组件渲染UI并响应事件，而 `reducer` 执行状态操作。

考虑这样一个场景:咱们想要计算组件渲染的次数。

一种简单的实现方法是初始化 `countRender`状态，并在每次渲染时更新它(使用 `useEffect()` hook)

```js
import React, { useState, useEffect } from 'react';

function CountMyRenders() {
  const [countRender, setCountRender] = useState(0);

  useEffect(function afterRender() {
    setCountRender(countRender => countRender + 1);
  });

  return (
    <div>I've rendered {countRender} timesdiv>
  );
}
复制代码
```

`useEffect()`在每次渲染后调用 `afterRender()`回调。但是一旦 `countRender`状态更新，组件就会重新渲染。这将触发另一个状态更新和另一个重新渲染，依此类推。

可变引用 `useRef()`保存可变数据，这些数据在更改时不会触发重新渲染，使用可变的引用改造一下 `<countmyrenders></countmyrenders>` ：

```js
import React, { useRef, useEffect } from 'react';

function CountMyRenders() {
  const countRenderRef = useRef(1);

  useEffect(function afterRender() {
    countRenderRef.current++;
  });

  return (
    <div>I've rendered {countRenderRef.current} timesdiv>
  );
}
复制代码
```

每次渲染组件时， `countRenderRef`可变引用的值都会使 `countRenderRef.current ++`递增。 重要的是，更改不会触发组件重新渲染。

## 5. 总结

要使函数组件有状态，请在组件的函数体中调用 `useState()`。

`useState(initialState)`的第一个参数是初始状态。返回的数组有两项:当前状态和状态更新函数。

```scss
const [state, setState] = useState(initialState);
复制代码
```

使用 `setState(newState)`来更新状态值。 另外，如果需要根据先前的状态更新状态，可以使用回调函数 `setState(prevState => newState)`。

在单个组件中可以有多个状态:调用多次 `useState()`。

必须确保使用 `useState()`遵循 Hook 规则。

当闭包捕获过时的状态变量时，就会出现过时状态的问题。可以通过使用一个回调来更新状态来解决这个问题，这个回调会根据先前的状态来计算新的状态。

最后，您将使用 `useState()`来管理一个简单的状态。为了处理更复杂的状态，一个更好的的选择是使用 `useReducer()` hook。

## 交流（欢迎加入群，群工作日都会发红包，互动讨论技术）

干货系列文章汇总如下，觉得不错点个Star，欢迎 加群 互相学习。

因为篇幅的限制，今天的分享只到这里。如果大家想了解更多的内容的话，可以去扫一扫每篇文章最下面的二维码，然后关注咱们的微信公众号，了解更多的资讯和有价值的内容。
