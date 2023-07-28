---
link: https://juejin.cn/post/7062735963734147079
title: 一文弄懂redux、react-redux如何使用
description: react技术栈中，状态管理通常用借助redux，redux与react-redux的关系是什么，分别用来解决什么问题？
keywords: Redux,React.js
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2022-02-09T15:43:52.000Z
publisher: 稀土掘金
stats: paragraph=121 sentences=100, words=1068
---
「这是我参与2022首次更文挑战的第6天，活动详情查看：[2022首次更文挑战](https://juejin.cn/post/7052884569032392740?utm_source=feed_5&utm_medium=feed&utm_campaign=gengwen202201_yq#heading-3 "https://juejin.cn/post/7052884569032392740?utm_source=feed_5&utm_medium=feed&utm_campaign=gengwen202201_yq#heading-3")」

> 本文主要记录redux、以及react-redux的基础用法 ![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c16d280d01b74398ac90a250fd9c901e~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image?)

## 一、redux

### 1.1 简介

**（1）概念**

* 是一个专门用来做 `&#x72B6;&#x6001;&#x7BA1;&#x7406;`的js库（ **不是react插件**）
* 可以用在 `vue`、 `react`、 `angular`项目中，但基本与react配合使用
* 作用：集中式管理react应用中的多个组件共享的状态

**（2）什么情况下需要使用redux**

* 某个组件的状态，需要让其他组件可以随时拿到（ `&#x5171;&#x4EAB;`）
* 一个组件需要改变另一个组件的状态（ `&#x901A;&#x4FE1;`）
* 总体原则： **能不用就不用，如果不用比较吃力才用*

### 1.2 工作流程

redux工作流程图如下： ![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/26b98b1864624057838f6b2259c72ba5~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image?)

三个核心概念：

**（1）action**

* 动作的对象
* 包含两个属性
  - `type`： **属性标识**，值为字符串，唯一，必要属性
  - `data`： **数据属性**，值任意类型，可选属性
  - 例子 `{type:'ADD_STUDENT', data: {name: 'tom', age: '18'}}` **（2）reducer**
* 用于 `&#x521D;&#x59CB;&#x5316;&#x3001;&#x52A0;&#x5DE5;&#x72B6;&#x6001;`
* 加工时，根据旧的state和action，产生新的state的 `&#x7EAF;&#x51FD;&#x6570;`

**（3）store**

* 将 `state`、 `action`、 `reducer`联系在一起的对象
* 加工时，根据旧的state和action，产生新的state的纯函数

### 1.3 store

目录结构

```md
│  └─ store
│     ├─ actions // actions，文件夹内以模块区分
│     │  ├─ count.js
│     │  └─ person.js
│     ├─ constants.js // action type唯一标识常量
│     ├─ index.js // 入口文件
│     └─ reducers // reducers，文件夹内以模块区分
│        ├─ conut.js
│        ├─ index.js // reducers统一暴露文件，合并reducers
│        └─ persons.js
复制代码
```

引入 `createStore`，专门用于创建redux中最为核心的store对象，而 `redux-thunk`、 `applyMiddleware`用于支持 `&#x5F02;&#x6B65;action`，后文会讲到什么是 `&#x5F02;&#x6B65;action`

```js

import { createStore, applyMiddleware } from "redux";

import thunk from "redux-thunk";
import reducers from "./reducers";

export default createStore(reducers, applyMiddleware(thunk));
复制代码
```

### 1.4 action

定义action对象中 `type&#x7C7B;&#x578B;`的 `&#x5E38;&#x91CF;&#x503C;`， **消除魔法字符串**

```js

export const INCREMENT = 'increment'
export const DECREMENT = 'decrement'
复制代码
```

创建action，普通action `&#x8FD4;&#x56DE;&#x5BF9;&#x8C61;`， `&#x5F02;&#x6B65;`action `&#x8FD4;&#x56DE;&#x51FD;&#x6570;`

```js

import { INCREMENT, DECREMENT } from "../constants";

export const increment = data => ({ type: INCREMENT, data });
export const decrement = data => ({ type: DECREMENT, data });

export const incrementAsync = (data, time) => {
  return (dispatch) => {
    setTimeout(() => {
      dispatch(increment(data));
    }, time);
  };
};
复制代码
```

#### 异步action

* 延迟的动作不想交给组件本身，想交给action
* 何时需要异步action：想要对状态进行操作，但是具体的数据考异步任务返回

> 异步action不是必须要写的，完全可以自己等待异步任务的结果再去分发同步action

> 创建的action不再返回一个对象，而是一个函数，等异步任务有结果后，一般 **再去分发一个同步的action**去真正操作数据

### 1.5 reducer

* `reducer`函数会接到两个参数，分别为： `&#x4E4B;&#x524D;&#x7684;&#x72B6;&#x6001;(preState)`， `&#x52A8;&#x4F5C;&#x5BF9;&#x8C61;(action)`
* 从 `action`对象中获取 `type`， `data`
* 根据 `type`决定如何加工数据

```js
import {INCREMENT, DECREMENT} from '../constants'

const initState = 0;

export default function count(preState = initState, action) {
  const { type, data } = action;
  switch (type) {
    case INCREMENT:
      return preState + data;
    case DECREMENT:
      return preState - data;
    default:
      return preState;
  }
}
复制代码
```

#### （1）初始化状态

在 `reducer`里面没有设置初始值时， `console.log(preState, action)`，在控制台如下图可以看到输出的 `preState=undefined`， `type=@@redux/INITq.p.v.o.d.w`

> type redux内部处理为随机值，每次输出的都不一样 ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/97485f059ef34a1c9dd317745ae9ef77~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image?) 通过 `const initState = 0;` `export default function count(preState = initState, action) {....default: return preState;}`设置初始值

#### （2）reducer是一个纯函数

```js

import { ADD_PERSON } from "../constants";

const initState = [{ id: "001", name: "jona", age: "23" }];

export default function persons(preState = initState, action) {
  const { type, data } = action;
  switch (type) {
    case ADD_PERSON:

      return [data, ...preState];
    default:
      return preState;
  }
}
复制代码
```

补充纯函数概念 ![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/db13bd4a1aca4578a6c87fa616dcdc7b~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image?)

#### （3）合并reducers

通过 `combineReducers`合并，接收的参数是一个对象， **对象的key值与getState()得到的对象的key一致**

```js

import { combineReducers } from "redux";
import count from "./conut";
import persons from "./persons";

export default combineReducers({
  count,
  persons,
});
复制代码
```

### 1.6 getState、dispatch

* 组件通过 `getState()`拿store的数据
* 通过 `dispatch`触发状态更新

```jsx
import store from "../../store";

import { increment } from "../../store/action/count";

  clickIncrement = () => {
    const { value } = this.selectNumber;
    store.dispatch(increment(value * 1));
  };
 render() {
    return (
      <div>
        <h1>当前求和为： {store.getState()}h1>
        ...

        <button onClick={this.clickIncrement}>+button>
      div>
      )
 }
复制代码
```

#### 视图不更新问题

`dispatch`后，发现 `store`的值已经变化了但是 **视图不更新**，如下图 ![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f16040dc2217491e9a7dda67d1ed3390~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image?) redux内部不支持自动更新，需要通过 `subscribe`API监听 `redux`中状态变化，只有变化，就需要 **重新调用render**

```jsx
componentDidMount() {
    store.subscribe(() => {
      this.forceUpdate();
    });
}
复制代码
```

## 二、react-redux

### 2.1. react-redux VS redux

> 为了方便使用，Redux 的作者封装了一个 React 专用的库 React-Redux 这两个仓库的区别如下：

* redux **需要监听store变化更新视图**，利用 `store.subscribe(() => { this.forceUpdate(); })`；react-redux不需要监听
* react-redux **将组件分为UI组件、容器组件**；关于redux的操作都在容器组件中， **单一职责原则**；并通过 `connect(mapStateToProps, mapDispatchToProps)()`连接容器组件与UI组件；redux没有区分

### 2.2 react-redux基本特性

`React-Redux` 将所有组件分成两大类： `UI&#x7EC4;&#x4EF6;`（presentational component）和 `&#x5BB9;&#x5668;&#x7EC4;&#x4EF6;`（container component）

**（1） `UI&#x7EC4;&#x4EF6;` 有以下几个特征：**

* 只负责UI的呈现，不带有任何业务逻辑
* 没有状态（即不使用 `this.state`这个变量）
* 所有数据都由参数（ `this.props`）提供
* 不使用任何 `Redux` 的 `API` **（2） `&#x5BB9;&#x5668;&#x7EC4;&#x4EF6;` 的特征恰恰相反**
* 负责管理数据和业务逻辑，不负责UI的呈现
* 带有内部状态
* 使用 `Redux` 的 `API` **（3） `UI&#x7EC4;&#x4EF6;` 与 `&#x5BB9;&#x5668;&#x7EC4;&#x4EF6;` 关联**
* 所有的 `UI&#x7EC4;&#x4EF6;`都应该包裹一个 `&#x5BB9;&#x5668;&#x7EC4;&#x4EF6;`，它们是 `&#x7236;&#x5B50;&#x5173;&#x7CFB;`
* 容器组件是真正与redux打交道的，里面可以随意调用redux的API
* UI组件中不能使用任何redux API
* 容器组件通过props给UI组件传递（1）redux中所保存的状态（2）用于操作状态的方法

> 总结： `UI&#x7EC4;&#x4EF6;`负责UI的呈现， `&#x5BB9;&#x5668;&#x7EC4;&#x4EF6;`负责管理数据和逻辑，如果一个组件既有UI又有业务逻辑，将它拆分成下面的结构：外面是一个容器组件，里面包了一个UI 组件。前者负责与外部的通信，将数据传给后者，由后者渲染出视图

### 2.3、connect()、mapStateToProps

`React-Redux`提供 `connect`方法，用于从 `UI&#x7EC4;&#x4EF6;`生成 `&#x5BB9;&#x5668;&#x7EC4;&#x4EF6;`

下面代码中， `CountUI`是 `UI&#x7EC4;&#x4EF6;`，利用 `connect`最后导出的是 `&#x5BB9;&#x5668;&#x7EC4;&#x4EF6;`

为了定义业务逻辑，需要给出下面两方面的信息：

* 输入逻辑：外部的数据（即 `state`对象）如何转换为 UI 组件的参数
* 输出逻辑：用户发出的动作如何变为 Action 对象，从 UI 组件传出去

`connect`方法接受两个参数： `mapStateToProps`和 `mapDispatchToProps`。它们定义了 UI 组件的业务逻辑。前者负责输入逻辑，即将 `state`映射到 UI 组件的参数（ `props`），后者负责输出逻辑，即将用户对 UI 组件的操作映射成 `Action`

`mapStateToProps` 接收 `state` 参数， `mapDispatchToProps` 接收 `dispatch` 参数

```jsx

import { connect } from "react-redux";
import CountUI from "../../components/count";
import {
  createIncrementAction,
  createDecrementAction,
  createIncrementAsyncAction,
} from "../../redux/count_action";

const mapStateToProps = (state) => ({ count: state });

const mapDispatchToProps = (dispatch) => ({
  increment: (number) => {
    dispatch(createIncrementAction(number));
  },
  incrementAsync: (number) => {
    dispatch(createIncrementAsyncAction(number, 500));
  },
  decrement: (number) => {
    dispatch(createDecrementAction(number));
  },
});

export default connect(mapStateToProps, mapDispatchToProps)(CountUI);
复制代码
```

```jsx

import React, { Component } from "react";

export default class CountUI extends Component {

  increment = () => {
    const { value } = this.selectNumber;
    this.props.increment(value * 1);
  };

  decrement = () => {
    const { value } = this.selectNumber;
    this.props.decrement(value * 1);
  };

  incrementIfOdd = () => {
    if (this.props.count % 2 === 1) {
      const { value } = this.selectNumber;
      this.props.increment(value * 1);
    }
  };

  incrementAsync = () => {
    const { value } = this.selectNumber;
    this.props.increment(value * 1);
  };

  render() {
    return (
      <div>
        <h1>当前求和为： {this.props.count}h1>
        ...

      div>
    );
  }
}
复制代码
```

### 2.4、mapDispatchToProps

`mapDispatchToProps`是 `connect`函数的第二个参数，用来建立 UI 组件的参数到 `store.dispatch`方法的映射。也就是说，它定义了哪些用户的操作应该当作 `Action`，传给 `Store`

**它可以是一个函数，也可以是一个对象**

（1）如果 `mapDispatchToProps`是一个 **函数**

```jsx

const mapDispatchToProps = (dispatch) => ({
  increment: (number) => {
    dispatch(createIncrementAction(number));
  },
  incrementAsync: (number) => {
    dispatch(createIncrementAsyncAction(number, 500));
  },
  decrement: (number) => {
    dispatch(createDecrementAction(number));
  },
});

export default connect(mapStateToProps, mapDispatchToProps)(CountUI);
复制代码
```

（2）如果 `mapDispatchToProps`是一个 **对象**

键值是一个函数， `Action creator` ，返回的 Action 会由 Redux **自动发出**

```jsx

export default connect(mapStateToProps, {
  increment: createIncrementAction,
  incrementAsync: createIncrementAsyncAction,
  decrement: createDecrementAction,
})(CountUI);
复制代码
```

### 2.5、Provider

`connect`方法生成容器组件以后，需要让容器组件拿到 `state`对象，才能生成 UI 组件的参数。

一种解决方法是将 `state`对象作为参数，传入容器组件。但是，这样做比较麻烦，尤其是容器组件可能在很深的层级，一级级将 `state`传下去就很麻烦。

```jsx

import React, { Component } from "react";
import Count from "./container/count";
import store from "./redux/store";

export default class App extends Component {
  render() {
    return <Count store={store} />;
  }
}
复制代码
```

React-Redux 提供 `Provider`组件，可以让容器组件拿到 `state`

`Provider`在根组件外面包了一层，这样一来， `App`的所有子组件就默认都可以拿到 `state`了

> 它的原理是 `React`组件的[`context`](https://link.juejin.cn?target=https%3A%2F%2Ffacebook.github.io%2Freact%2Fdocs%2Fcontext.html "https://facebook.github.io/react/docs/context.html")属性

> 使原来整个应用成为 `Provider`的子组件 接收 `Redux`的 `store`作为 `props`，通过 `context`对象传递给子孙组件上的 `connect`

```jsx

import React from "react";
import ReactDOM from "react-dom";
import { Provider } from "react-redux";
import store from './redux/store'
import App from "./App";

ReactDOM.render(
  <Provider store={store}>
    <App />
  Provider>,
  document.getElementById("root")
);
复制代码
```

## 写在最后

本文所有的例子在[github](https://link.juejin.cn?target=url "url")，欢迎star，一起学习！
