---
link: https://juejin.cn/post/7001661922978299918
title: 这一次，彻底搞懂useEffect
description: 这是我参与8月更文挑战的第29天，活动详情查看：8月更文挑战 什么是useEffect? 1. useEffect执行时机 当做componentDidMount和componentDidUpdate
keywords: 前端,面试
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2021-08-29T01:46:03.000Z
publisher: 稀土掘金
stats: paragraph=29 sentences=13, words=200
---
这是我参与8月更文挑战的第29天，活动详情查看：[8月更文挑战](https://juejin.cn/post/6987962113788493831 "https://juejin.cn/post/6987962113788493831")

## 什么是useEffect?

> 让函数型组件拥有处理副作用的能力，类似生命周期函数。

### 1. useEffect执行时机

> 可以把useEffect看做componentDidMount,componentDidUpdate,componentWillUnmount这三个函数的组合。

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b1b61cb50d774717999e56a7772b6deb~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp)

* 当做componentDidMount和componentDidUpdate的时候

```js
function App() {
    const [count,setCount] = useState(0);

    useEffect(() => {
        console.log('组件挂载完成之后 或 组件数据更新完成之后 执行');
    })
    return (
        <div>
            {count}
            <button onClick={() => setCount(count + 1)}>+1button>
        div>
    )
}
复制代码
```

* 仅当做comonentDidMount的时候

```js
useEffect(() => {
    console.log('仅当做componentDidMount');
},[])
复制代码
```

* 当做componentWillunmount的时候(注意：这里不是仅当做componentWillunmount)

```js
useEffect(() => () => {
    console.log('当做componentWillUnmount');
})
复制代码
```

### 2. useEffect的使用方法示例

1. 为window对象添加滚动事件。（挂载完成后绑定事件，卸载组件后解除绑定）

```js
function App() {
    function onScroll() {
        console.log('监听到页面发生滚动了');
    }
    useEffect(() => {
        window.addEventListener('scroll',onScroll);
        return () => {

            window.removeEventListener('scroll',onScroll);
        }
    })
    return (
        <div>
            App
        div>
    )
}
复制代码
```

1. 设置定时器让count数值每隔一秒增加1。

```js
function App() {

    const [count,setCount] = useState(0);
    useEffect(() => {
        const timeId = setInterval(() => {
           setCount(count => count + 1);
        },1000)
        return () => {
            clearInterval(timeId);
        }
    },[])
    return (
        <div>
            <h1>当前求和为：{count}h1>
        div>
    )
}
复制代码
```

### 3. useEffect解决的问题

1. 将一组相干的业务逻辑归置到了同一个副作用函数中.

2. 简化重复代码,使组件内部代码更加清晰.

### 4：useEffect的第二个参数

* 只有指定数据发生变化的时候才触发effect

```js
useEffect(() => {
    document.title = count;
}, [count])
复制代码
```

### 5：useEffect结合异步函数

> 在useEffect中直接使用async和await是会报错的，推荐使用立即执行函数来包裹异步函数。

```js
function getData() {
    return new Promise(resolve => {
        resolve({msg: 'Hello'})
    })
}
function App() {

    useEffect(() => {
        (async function () {
            const result = await getData();
            console.log(result);
        })()
    },[])

    return (
        <div>
            App
        div>
    )
}
复制代码
```

## 参考文献

* [官方文档](https://link.juejin.cn?target=https%3A%2F%2Fzh-hans.reactjs.org%2Fdocs%2Fhooks-reference.html%23useeffect "https://zh-hans.reactjs.org/docs/hooks-reference.html#useeffect")
