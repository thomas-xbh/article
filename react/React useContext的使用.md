---
link: https://juejin.cn/post/7062973798558990372
title: React useContext的使用
description: React中使用useContext步骤 通过React.createContext创建 定义要传递的数据value={} 用Provider包裹要接收数据的组件 在对应组件中通过React.useC
keywords: React.js
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2022-02-10T07:07:13.000Z
publisher: 稀土掘金
stats: paragraph=27 sentences=9, words=201
---
### React 使用useContext步骤

1. 通过 `React.createContext`创建
2. 定义要传递的数据 `value={}`
3. 用 `Provider`包裹要接收数据的组件

```js

const UserContext = createContext()
const App = () => {

  const userInfo = { name: 'Master_y', age: 22 }
  return (

    <UserContext.Provider value={userInfo}>
      <div>
        <Child1 />
      div>
      <div>
        <Child2 />
      div>
    UserContext.Provider>
  )
}
复制代码
```

1. 在对应组件中通过 `React.useContext`的方式接收数据

```js
const Child1 = () => {

  const { name } = useContext(userContext)
  return (
    <div className="container">
      <span>博客名:{name}span>
    div>
  )
}
复制代码
```

```js
const Child2 = () => {

  const { age } = useContext(userContext)
  return (
    <div style={{ width: '100%', display: 'flex', justifyContent: 'center' }}>
      <span>年龄:{age}span>
    div>
  )
}
复制代码
```

## 完整例子

```js
import React, { useContext } from 'react';

const styler = {
  width: '100%',
  display: 'flex',
  justifyContent: 'center',
  color: 'red',
  fontSize: 20,
};

export default () => {

  const UserContext = React.createContext();

  const userInfo = { name: 'Master_y', age: 22 };

  const Child1 = () => {

    const { name } = useContext(UserContext);
    return (
      <div style={styler}>
        <span>博客名:{name}span>
      div>
    );
  };

  const Child2 = () => {

    const { age } = useContext(UserContext);
    return (
      <div style={styler}>
        <span>年龄:{age}span>
      div>
    );
  };

  return (

    <UserContext.Provider value={userInfo}>
      <div>
        <Child1 />
      div>
      <div>
        <Child2 />
      div>
    UserContext.Provider>
  );
};

复制代码
```

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f2572bd2f7f84f64a82053588100dde9~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image?)

## 实际生产中使用

```js
import React from 'react';

export const MyContext = React.createContext();
export const MyProvider = ({ children, value }) => {
  return <MyContext.Provider value={value}>{children}MyContext.Provider>;
};

复制代码
```
