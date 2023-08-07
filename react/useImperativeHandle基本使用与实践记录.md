---
link: https://juejin.cn/post/7168652023766712333
title: useImperativeHandle基本使用与实践记录
description: 本文主要记录一下useImperativeHandle的使用场景和如何在项目中利用useImperativeHandle进行实践
keywords: 掘金·日新计划,前端
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2022-11-22T01:51:15.000Z
publisher: 稀土掘金
stats: paragraph=23 sentences=23, words=163
---
开启掘金成长之旅！这是我参与「掘金日新计划 · 12 月更文挑战」的第2天，[点击查看活动详情](https://juejin.cn/post/7167294154827890702 "https://juejin.cn/post/7167294154827890702")

# 背景

最近在工作中的项目代码中看见了useImperativeHandle的使用，之前不了解这个api主要用来做什么，也没有再实践中用过它，于是详细了解了一下它的基本用法与使用场景，本文主要记录一下useImperativeHandle的使用场景和如何在项目中利用useImperativeHandle进行实践。

## useImperativeHandle语法

useImperativeHandle是React Hooks提供的一个Hook API，表示在使用ref时可以自定义暴露给父组件的实例值，这样可能不太好理解，其实简单解释就是：有一个父组件和子组件，父组件想要直接操作子组件内部的DOM元素或者想要直接使用子组件实例的某些属性或方法时，就可以通过useImperativeHandle这个Hook来实现。

```js
useImperativeHandle(ref, createHandle, [deps])
复制代码
```

其中ref为父组件传入的ref值，createHandle是一个函数，返回一个对象，即这个ref的current值，deps为依赖数组，当依赖项发生改变时，会重新执行useImperativeHandle钩子，返回新的对象。

## React.forwardRef()转发Ref

但在讲useImperativeHandle如何使用之前，我们再来回顾一下React.forwardRef()透传Ref，也就是转发Ref，它会创建一个React组件，这个组件能够将其接受的ref属性转发到其组件树下的另一个组件中。它常用的场景为：转发ref到DOM组件。

```js
React.forwardRef((props, ref) => (<></>))
复制代码
```

其中 `React.forwardRef` 接受一个渲染函数作为参数，该函数接受两个参数，分别是props和ref，props代表父组件传入的属性，ref表示父组件传入的ref。

## React.forwardRef()实战

以下为一个简单的示例，定义父组件Father和子组件Child，在子组件Child内部定义了一个input输入框，并传入ref，想实现点击父组件中的按钮能够使子组件内部的输入框自动聚焦，利用React.forward实现代码如下：

```js
import React, { useRef } from "react";

const Child = React.forwardRef((props: any, ref: any) => {
    return (
        <>
        <input ref={ref} />
        {props.children}
        </>
    )
})
const Father = () => {
    const fatherRef = useRef(null);
    const handleClick = () => {
        fatherRef && fatherRef?.current && fatherRef.current.focus();
    }
  return (
    <div style={{ padding: 20 }}>
    <Child ref={fatherRef} />
    <button onClick={handleClick}>点击我操作子组件中的input元素button>
    div>
  );
}
复制代码
```

当点击按钮后，子组件的输入框会自动聚焦，如下图所示：

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8abcbff1626949df93d60f03599dbb94~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp?) 从上述代码可知，通过React.forwardRef()将父组件Father的 `FatherRef`转发到了子组件的input DOM元素上，从而能够通过 `FatherRef`直接操作input元素，使其聚焦。

## useImperativeHandle实战

但是React官方说应当避免使用ref这样的命令式代码，所以推荐useImperativeHandle应当与React.forwardRef一起使用。所以将上述代码改为如下形式，能够实现一样的功能：

```js
const Child = React.forwardRef((props: any, ref: any) => {
    const childRef = useRef(null);

    useImperativeHandle(ref, () => ({
        focus: () => {
            childRef && childRef.current.focus()
        }
    }))
    return (
        <>
        <input ref={childRef} />
        {props.children}
        </>
    )
})
const Father = () => {
    const fatherRef = useRef(null);
    const handleClick = () => {

        fatherRef.current.focus();
    }
  return (
    <div style={{ padding: 20 }}>
    <Child ref={fatherRef} />
    <button onClick={handleClick}>点击我操作子组件中的input元素button>
    div>
  );
}
复制代码
```

上述代码利用useImperativeHandle和React.forwardRef将子组件实例的属性或方法暴露给了父组件，所以父组件能够直接操作子组件中的属性或方法，并且子组件的ref和父组件的ref不再使用同一个ref，这也使开发者更加清晰的区分不同组件的ref了。
