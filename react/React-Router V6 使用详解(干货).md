---
link: https://juejin.cn/post/7033313711947251743
title: React-Router V6 使用详解(干货)
description: 最新的React-Router V6使用介绍 V6版本与原有V5版本的比较 <Link to> <Navigate> <Routes>
keywords: React.js
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2021-11-22T08:50:22.000Z
publisher: 稀土掘金
stats: paragraph=62 sentences=35, words=487
---
```less
Remix 团队在2020年6月发布了第一个V6.0.0-beta.0版本的React-Router，也预示着V6版本的正式开始，相比V5版本的确有很多方面的升级。
本文将结合V6特性和V5如何升级V6两方面来为大家详细讲解React-Router的使用。
(使用版本：[V6.0.2稳定版](https:
```

## **一、基本用法**

npm： `$ npm install react-router-dom@6`

yarn `$ yarn add react-router-dom@6`

```
&#x76EE;&#x524D;&#x5B98;&#x65B9;&#x4ECE;5&#x5F00;&#x59CB;&#x5DF2;&#x7ECF;&#x653E;&#x5F03;&#x539F;&#x6709;&#x7684;react-router&#x5E93;&#xFF0C;&#x7EDF;&#x4E00;&#x547D;&#x540D;&#x4E3A;react-router-dom
```

```lua
React-Router本身在React开发中就是一个组件，因此在使用时基本遵循组件开发相关原则。这里采用create-react-app来创建一个基础的demo工程演示使用过程。
```

1. 创建demo `create-react-app my-first-react` 安装react-router组件
2. 启用全局路由模式 全局路由有常用两种路由模式可选：HashRouter 和 BrowserRouter HashRouter：URL中采用的是hash(#)部分去创建路由，类似[www.example.com/#/](https://link.juejin.cn?target=http%3A%2F%2Fwww.example.com%2F%23%2F "http://www.example.com/#/") BrowserRouter：URL采用真实的URL资源 后续有文章会详细讲HashRouter的原理和实现，这里我们采用BrowserRouter来创建路由 **index.js**

```js
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import reportWebVitals from './reportWebVitals';
import { BrowserRouter } from 'react-router-dom';

ReactDOM.render(
  <React.StrictMode>
    <BrowserRouter>
      <App />
    BrowserRouter>
  React.StrictMode>,
  document.getElementById('root')
);

reportWebVitals();
```

这样我们在yarn start 或者 npm run start的时候访问/就可以访问这个组件了，具体效果大家可以自行运行
3. 路由功能 React-Router V6版本常用路由组件和hooks，其他不常用的大家可以看下官网的介绍组件名作用说明 `<routers></routers>`一组路由代替原有 `<switch></switch>`，所有子路由都用基础的Router children来表示 `<router></router>`基础路由Router是可以嵌套的，解决原有V5中严格模式，后面与V5区别会详细介绍 `<link>`导航组件在实际页面中跳转使用 `<outlet></outlet>`自适应渲染组件根据实际路由url自动选择组件 hooks名作用说明 `useParams`返回当前参数根据路径读取参数 `useNavigate`返回当前路由代替原有V5中的 useHistory `useOutlet`返回根据路由生成的element `useLocation`返回当前的location 对象 `useRoutes`同Routers组件一样，只不过是在js中使用 `useSearchParams`用来匹配URL中?后面的搜索参数基础使用示例 **App.js**

```
&#x8FD9;&#x91CC;&#x521B;&#x5EFA;&#x4E86;&#x4E24;&#x4E2A;&#x7EC4;&#x4EF6;Home&#x548C;About&#xFF0C;&#x7136;&#x540E;&#x5206;&#x522B;&#x6CE8;&#x518C;/&#x548C;about&#xFF0C;&#x5728;&#x6BCF;&#x4E2A;&#x9875;&#x9762;&#x8FD8;&#x6709;Link&#x6765;&#x8FDB;&#x884C;&#x5BFC;&#x822A;
```

```js
import './App.css';
import { Routes, Route, Link } from "react-router-dom"
function App() {
  return (
    <div className="App">
      <header className="App-header">
        <Routes>
          <Route path="/" element={<Home />}>Route>
          <Route path="/about" element={<About />}>Route>
        Routes>
      header>
    div>
  );
}
function Home() {
  return <div>
    <main>
      <h2>Welcome to the homepageh2>
    main>
    <nav>
      <Link to="/about">aboutLink>
    nav>
  div>
}
function About() {
  return <div>
    <main>
      <h2>Welcome to the about pageh2>
    main>
    <nav>
      <ol>
        <Link to="/">homeLink>
        <Link to="/about">aboutLink>
      ol>

    nav>
  div>
}
export default App;

```

运行后 ![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2e61a41d8aaa4683ae0c4f3d19f238a1~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?) ![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/839462b07f94448d8ffb586c4e75d54d~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?) 至此，一个最简单的路由demo就正常运行了。
4. 嵌套路由 嵌套路由是V6版本对之前版本一个较大的升级，采用嵌套路由会智能的识别

```js
function App() {
  return (
    <Routes>
      <Route path="user" element={<Users />}>
        <Route path=":id" element={<UserDetail />} />
        <Route path="create" element={<NewUser />} />
      Route>
    Routes>
  );
}
```

当访问 /user/123 的时候，组件树将会变成这样

```js
<App>
    <Users>
        <UserDetail/>
    Users>
App>
```

当访问/user/create的时候，组件树将变成这样

```js
<App>
   <Users>
       <NewUser/>
   Users>
App>
```

如果只是内部组件修改，也可以采用 `<outlet></outlet>`来直接实现，如下所示

```js
function App() {
  return (
    <Routes>
      <Route path="user" element={<Users />}>
        <Route path=":id" element={<UserDetail />} />
        <Route path="create" element={<NewUser />} />
      Route>
    Routes>
  );
}
function Users() {
  return (
    <div>
      <h1>Usersh1>
      <Outlet />
    div>
  );
}
```
5. index路由 index属性解决当嵌套路由有多个子路由但本身无法确认默认渲染哪个子路由的时候，可以增加index属性来指定默认路由

```js
function App() {
  return (
    <Routes>
      <Route path="/" element={<Layout />}>
        <Route index element={<About />} />
        <Route path="user" element={<User />} />
        <Route path="about" element={<About />} />
      Route>
    Routes>
  );
}
```

这样当访问/的时候 `<outlet></outlet>`会默认渲染About组件
6. 路由通配符

```
&#x6574;&#x4E2A;react-router&#x652F;&#x6301;&#x4EE5;&#x4E0B;&#x51E0;&#x79CD;&#x901A;&#x914D;&#x7B26;
```

```js
/groups
/groups/admin
/users/:id
/users/:id/messages
/files
```

注意，以下这些正则方式在V6里面是不支持的

```bash
/users/:id?

/tweets/:id(\d+)
/files/*/cat.jpg
/files-*
```

这里的 `*`只能用在/后面，不能用在实际路径中间 关于NotFound类路由，可以用 `*`来代替

```js
function App() {
  return (
    <Routes>
      <Route path="/" element={<Home />} />
      <Route path="dashboard" element={<Dashboard />} />
      <Route path="*" element={<NotFound />} />
    Routes>
  );
}
```
7. 获取参数 useParams 和useSearchParams 假设现有App路由

```js
function App() {
 return (
   <Routes>
     <Route path="user" element={<Users />}>
       <Route path=":id" element={<UserDetail />} />
       <Route path="create" element={<NewUser />} />
     Route>
   Routes>
 );
}
```

那么在UserDetail内部需要用useParams来获取对应的参数

```js
import { useParams } from "react-router-dom";

export default function UserDetail() {
  let params = useParams();
  return <h2>User: {params.id}h2>;
}
```

useSearchParams相对复杂，他返回的是一个当前值和set方法 ` let [searchParams, setSearchParams] = useSearchParams();` 使用时可以用 `searchParams.get("id")`来获取参数，同时页面内也可以setSearchParams({"id":2})来改变路由，这样当访问 [http://URL/user?id=111](https://link.juejin.cn?target=http%3A%2F%2FURL%2Fuser%3Fid%3D111 "http://URL/user?id=111") 时就可以获取和设置路径
8. useNavigate useNavigate是替代原有V5中的useHistory的新hooks，其用法和useHistory类似，整体使用起来更轻量，他的声明方式如下：

```ts
declare function useNavigate(): NavigateFunction;

interface NavigateFunction {
  (
    to: To,
    options?: { replace?: boolean; state?: State }
  ): void;
  (delta: number): void;
}
```

```js

  let navigate = useNavigate();
  function handleClick() {
    navigate("/home");
  }

  function App() {
     return <Navigate to="/home" replace state={state} />;
  }

 () => navigate(-2)}>
    Go 2 pages back

  <button onClick={() => navigate(-1)}>Go backbutton>
  <button onClick={() => navigate(1)}>
    Go forward
  button>
  <button onClick={() => navigate(2)}>
    Go 2 pages forward
  button>
```

## 二、与V5的区别

V5写法：

```js
 function App() {
   return (
     <Switch>
       <Route exact path="/">
         <Home />
       Route>
       <Route path="/about">
         <About />
       Route>
       <Route path="/users/:id" children={<User />} />
     Switch>
   );
 }
```

V6写法

```js
 function App() {
   return (
     <Routes>
       <Route index path="/" element={<Home />} />
       <Route path="about" element={<About />} />
       <Route path="/users/:id" element={<User />} />
     Routes>
   );
 }
```

V5写法：

```js
 <Switch>
   <Redirect from="about" to="about-us" />
 Switch>
```

V6写法：

```js
 <Route path="about" render={() => <Navigate to="about-us" />}
```

V5版本的to属性只支持绝对位置，如 `<lint to="me"></lint>`表示 `<lint to="/me"></lint>`，如果当时正在Users组件内,想跳转需要 `<lint to="/users/me"></lint>`。在V6中，Link默认支持相对位置，也就是 `<lint to="me"></lint>` 在Users组件内会等价于 `<lint to="/users/me"></lint>`，同时支持'..' 和'.'等相对路径写法。

```js

<Route path="app">
<Route path="dashboard">
 <Route path="stats" />
Route>
Route>

               =>
            =>
         =>
      =>
```

可以参考上面useNavigate使用，这里不再赘述

V6版本的react-router升级了原有嵌套路由写法，并且重新实现了useNavigate来替代useHistory，整体上更加好理解。当然还有些其他属性和方法没有再介绍，大家如果有其他想知道的也可以回复我来补充。

参考链接：

```bash
https://reactrouter.com/docs/en/v6/upgrading/v5
https://reactrouter.com/docs/en/v6/getting-started/overview
```
