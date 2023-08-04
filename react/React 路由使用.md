---
link: https://juejin.cn/post/7102047013818073096
title: React 路由使用
description: Router react-router-dom是一个处理页面跳转的三方库，在使用之前需要先安装到我们的项目中： 简单路由 使用路由时需要为组件指定一个路由的path，最终会以path为基础，进行页面的
keywords: 前端,React.js
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2022-05-26T14:10:19.000Z
publisher: 稀土掘金
stats: paragraph=79 sentences=70, words=645
---
## Router

`react-router-dom`是一个处理页面跳转的三方库，在使用之前需要先安装到我们的项目中：

```shell
npm
npm install react-router-dom@6
yarn
yarn add react-router-dom@6
复制代码
```

## 简单路由

使用路由时需要为组件指定一个路由的 `path`，最终会以 `path`为基础，进行页面的跳转。具体使用先看个简单示例，该示例比较简单就是两个 `Tab`页面的来回切换。

```js

import {Link} from 'react-router-dom'
function App() {
  return (
    <div>
      <h1>路由练习h1>
      <nav>
        {/* link 页面展示时，是个a标签 */}
        <Link className ='link' to='/Tab1'> Tab1Link> ///覆盖：渲染tab1组件
        <Link className = 'link' to='/Tab2'> Tab2 Link> ///覆盖：渲染tab2组件
      nav>
    div>
    );
}

export default function Tab1(params) {
    return (

        <main style={{ padding: "1rem 0" }}>
          <h2>我是Tab1h2>
        main>
      );
}

export default function Tab2(params) {
    return (
        <main style={{ padding: "1rem 0" }}>
          <h2>我是Tab2h2>
        main>
      );
}

import {BrowserRouter,Routes,Route} from 'react-router-dom'
import Tab1 from './pages/Tab1.jsx'
import Tab2 from './pages/Tab2.jsx'

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <BrowserRouter>
     <Routes>
       <Route path = '/' element = {<App/>} /> ///兄弟路由
       <Route path = '/Tab1' element = {<Tab1/>} />///兄弟路由
       <Route path = '/Tab2' element = {<Tab2/>} />///兄弟路由
     Routes>
    BrowserRouter>
  React.StrictMode>
);
复制代码
```

最终交互时，上述路由配置会出现彼此覆盖的情况，如下图：

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5539c74d20a34c3f8f589ff1ec7827fb~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

为了保证 `App`组件，不会在 `Tab1`和 `Tab2`切换时被覆盖需要使用嵌套路由。

## 嵌套路由

嵌套路由，可以保证子路由共享父路由的界面而不会覆盖。为此 `React`提供了 `Outlet`组件，将其用于父组件中可以为子路由的元素占位，并最终渲染子路由的元素。

`Outlet`渲染一个子路由的元素

```js
import {Link,Outlet} from 'react-router-dom'
function App() {
  return (
    <div>
      <h1>路由练习h1>
      <nav>
        {/* link 页面展示时，是个a标签 */}
        <Link className ='link' to='/Tab1'> Tab1Link>
        <Link className ='link' to='/Tab2'> Tab2 Link>
      nav>
      {/* 此时尚不能实现共享APP UI的同时渲染出 Tab1 和 Tab2，还需要使用 <Outlet/>
        保证父路由，在子路由交换时，仍然存在
      */}
      <Outlet/>
    div>
    );
}

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <BrowserRouter>
      <Routes>
       <Route path='/' element={<App />} >
          {/* 孩子路由，url为:  / + 孩子的path */}
          <Route path='Tab1' element={<Tab1 />} />
          <Route path='Tab2' element={<Tab2 />} />
        Route>
      Routes>
    BrowserRouter>
  React.StrictMode>
);
复制代码
```

最终效果如下图：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/103c2477f8124e0ebce25dd1a9895043~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

## 未匹配路由

通过 `path='*'`,实现没有其他路由匹配时，对其进行匹配。

```js
root.render(
  <React.StrictMode>
    <BrowserRouter>
      <Routes>
      <Route path='/' element={<App />} >
          {/* 孩子路由，url为:  / + 孩子的path */}
          <Route path='Tab1' element={<Tab1 />} />
          <Route path='Tab2' element={<Tab2 />} />
          <Route path = '*' element={<p>
            未匹配到路由时，会跳转此处。
          p>} />
        Route>
      Routes>
    BrowserRouter>
  React.StrictMode>
);
复制代码
```

效果如图：

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e4ee6632dc424f61bdccb07487df8467~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

## 路由传参数

通过路由传递参数到组件中

```js

const dataList = [
    {
        id:20220101,
        content:'笔记1'
    },
    {
        id:20220102,
        content:'笔记2'
    },
    {
        id:20220103,
        content:'笔记3'
    },
]
export default function getTodoList(params) {
    return dataList
}

export function findTodoItem(params) {
    return dataList.find((value)=>value.id === params)
}

export default function Tab2(params) {
    let list = getTodoList()
    return (
        <div>
            <ul>
                {
                    list.map((item) => (
                        <li key={item.id}>
                           {/*子路由形如：'/Tab2/20220103' */}
                            <Link to={`/Tab2/${item.id}`}>{item.content}Link>
                        li>
                    ))
                }
            ul>
           {/*渲染一个子路由的元素*/}
            <Outlet />
        div>
    );
}

root.render(
  <React.StrictMode>
    <BrowserRouter>
      <Routes>
      <Route path='/' element={<App />} >
          {/* 孩子路由，url为:  / + 孩子的path */}
          <Route path='Tab1' element={<Tab1 />} />
          <Route path='Tab2' element={<Tab2 />} >
            <Route path=':itemId' element={<ItemDetail/>}/>
          Route>
          <Route path = '*' element={<p>未匹配到该路由请先设置路由页面 p>} />
        Route>
      Routes>
    BrowserRouter>
  React.StrictMode>
);

import { useParams } from 'react-router-dom'
export function ItemDetail() {

    let params = useParams()
    let content = findTodoItem(parseInt(params.itemId)).content
    return (
        <div>
            <h2>笔记详情h2>
            <p>这是我于{params.itemId},记录的笔记他的内容为{content}p>
        div>
    )
}
复制代码
```

最终效果：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b4e2f187643649218108a6e1ab9a0692~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

## 索引路由

当我们切换至 `Tab1`再切回 `Tab2`后，笔记详情页面将空白，效果如下：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/89303c8c6723463b81e95e0086dec1c5~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

可以通过索引路由填补空白，具体只需：

```js
root.render(
  <React.StrictMode>
    <BrowserRouter>
      <Routes>
      <Route path='/' element={<App />} >
          {/* 孩子路由，url为:  / + 孩子的path */}
          <Route path='Tab1' element={<Tab1 />} />

          <Route path='Tab2' element={<Tab2 />} >
            {/*索引路由 有index 无path*/}
            <Route index element={<p>请选择一个笔记查看它的详情 p>}/>
            <Route path=':itemId' element={<ItemDetail/>}/>
          Route>

          <Route path = '*' element={<p>未匹配到该路由请先设置路由页面 p>} />

        Route>
      Routes>
    BrowserRouter>
  React.StrictMode>
);
复制代码
```

如此当我们重复上述操作时便会呈现如下效果：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/22c9d3183ff5466aaa1c2246ca3c3582~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

当父路由匹配，但其他子路由都不匹配时，由索引路由匹配。索引路由是父路由的默认子路由。 当用户尚未单击导航列表中的一项时，会呈现索引路由。

## 活动链接

与 `Link`功能一致，差异是可以设置点击后的颜色

```js
export default function Tab2(params) {
    let list = getTodoList()
    return (
        <div>
            <ul>
                {
                    list.map((item) => (
                        <li key={item.id}>
                            {/* <Link to={`/Tab2/${item.id}`}>{item.content}Link> */}
                            <NavLink style = { ({isActive})=> ({ color : isActive ? "red" : "" }) } to={`/Tab2/${item.id}`}> {item.content} NavLink>
                        li>
                    ))
                }
            ul>
            <Outlet />
        div>
    );
}
复制代码
```

## 搜索参数

搜索参数类似于 URL 参数，形如 `/login?success=1`

```js
export default function Tab2(params) {
    let list = getTodoList()

    let [searchParams, setSearchParams] = useSearchParams();
    return (
        <div>
            {/* 搜索框: 随着输入设置搜索参数 */}
            <input type="text" onChange = { (event)=>{
                let text = event.target.value
                if (text) {
                    setSearchParams({text})
                } else {
                    setSearchParams({})
                }
            } } />

            <ul>
                { list.filter((item)=>{
                    let txt = searchParams.get('text')
                    if (!txt) return true
                    return item.content.startsWith(txt)
                })
                    .map((item) => (
                        <li key={item.id}>
                            {/* <Link to={`/Tab2/${item.id}`}>{item.content}Link> */}
                            <NavLink style = { ({isActive})=> ({ color : isActive ? "red" : "" }) } to={`/Tab2/${item.id}`}> {item.content} NavLink>
                        li>
                    ))
                }
            ul>

            <Outlet />
        div>
    );
}
复制代码
```

随着我们输入 `apple`, 路由的地址将变为 `/Tab2?text=apple`触发路由重新呈现。

当我们在输入框输入字符时，便会触发列表的过滤显示：

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1d9de97f06f0491f80ba79d111ea1194~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

## 自定义行为

上述UI在交互过程中，当我们点击 `Tab1`和 `Tab2`进行切换时，或者点击 `apple`或 `appet`时，会出现输入框被清空，且列表不再被过滤的问题。

`react-router`提供了 `useLocation`方法，它返回浏览器显示的 `url`信息。通过它可以获取浏览器 `url`中的搜索参数，从而进行暂存，在具体组件内，可以通过 `useSearchParams`获取到暂存的值。具体方式，通过自定义组件包装 `NavLink`或 `Link`来实现。

```js

import React, { Component } from 'react';
import { useLocation,NavLink } from "react-router-dom";
export default function QueryLink({to,...props}) {

  let location = useLocation()

  return <NavLink to={to+location.search} {...props} />
}

<QueryLink className ='link' to='/Tab1'> Tab1QueryLink>
<QueryLink className ='link' to='/Tab2'> Tab2 QueryLink>

 <li key={item.id}>
   <QueryLink style = { ({isActive})=> ({ color : isActive ? "red" : "" }) } to={`/Tab2/${item.id}`}> {item.content} QueryLink>
 li>
复制代码
```

## useNavigate

上述示例中，路由的切换采用 `Link`或者 `NavLink`，但当我们的页面元素不使用 `Link`时，比如使用 `Button`，此时便需要使用采用 `useNavigate`。同上可以配合 `useLocation`保存搜索字段。

```js
export function ItemDetail() {

    let params = useParams()
    let content = findTodoItem(parseInt(params.itemId)).content

    let navigate = useNavigate()

    let location = useLocation()
    return (
        <div>
            <h2>笔记详情h2>
            <p>这是我于{params.itemId}记录的笔记，内容为{content}p>
            <button onClick = {
                (e)=>{
                    deleteTodoItem(params.itemId)
                    navigate('/Tab2/'+location.search)
                }
            }>
                删除笔记
            button>
        div>
    )
}

export function deleteTodoItem(params) {
    dataList = dataList.filter((value)=>value.id !== parseInt(params))
}
复制代码
```

## 参考资料

[reactrouter.com/docs/en/v6/...](https://link.juejin.cn?target=https%3A%2F%2Freactrouter.com%2Fdocs%2Fen%2Fv6%2Fgetting-started%2Ftutorial "https://reactrouter.com/docs/en/v6/getting-started/tutorial")
