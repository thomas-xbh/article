---
link: https://juejin.cn/post/7129779854286782477
title: react-router-dom6 路由懒加载与组件懒加载
description: react-router-dom6 路由懒加载与组件懒加载 本文教学如何配置最新路由的懒加载 以及 组件懒加载 组件懒加载 组件引入方式 需要用到懒加载的组件：此时我这个组件默认是关闭的，类似于弹窗之
keywords: React.js
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2022-08-09T07:47:24.000Z
publisher: 稀土掘金
stats: paragraph=30 sentences=18, words=105
---
## react-router-dom6 路由懒加载与组件懒加载

本文教学如何配置最新路由的懒加载 以及 组件懒加载

### 组件懒加载

组件引入方式

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9938f1f4b8dc47569113be08ba1f4fa4~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp?)

需要用到懒加载的组件：此时我这个组件默认是关闭的，类似于弹窗之类的，就需要最好做一个懒加载

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c8485df510314ae38655584186a46ee8~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp?)

再配置一个过渡组件，就是等待我们懒加载的组件还没过来时运行此组件
我这边是配置在index.tsx里面

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7e789387a3554211aa871367fea29c16~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp?)

这样就实现了组件懒加载，在我们需要将该组件懒加载的时候就采用lazy的方式导入即可

### 路由懒加载

**有了组件懒加载在index.tsx中配置的过渡组件，我们这同样可以复用**
**如果我们不想用也可以自己定制Suspense，嵌套在路由组件前即可如 lztest**
**route index.tsx**

```typescript
import { lazy, Suspense } from "react";
import { Navigate } from "react-router-dom";

const TodoList = lazy(() => import("../commponet/TodoList"));
const LazyTest = lazy(() => import("../page/LazyTest"));

interface Router {
  name?: string;
  path: string;
  children?: Array<Router>;
  element: any;
}

const routes: Array<Router> = [
  {
    path: "/tsdemo",
    element: <TodoList />,
  },
  {
    path: "/lztest/:name/:age/:sex",

    element: (
      <Suspense fallback={<h2>加载中....h2>}>
        <LazyTest />
      Suspense>
    ),
  },
  {
    path: "/",
    element: <Navigate to="/tsdemo" />,
  },
];

export default routes;

复制代码
```

### 测试结果

先降低网速

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/20e0094287894b388117c06f37fdb16e~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp?)

刷新

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/25a57ff3160742caab3d20c1a78c3f48~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp?)

点击控制懒加载组件显示
我们会发现此时会加载一个文件，这个文件就是对应的懒加载文件
说明我们成功实现了懒加载

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ffc295415abf4a8499169e7864a1f354~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp?)

点击切换路由
第一个是加载组件的懒加载文件 第二个是加载路由组件的懒加载文件

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cf3b171a2a1b4f8e82596b3ddce6adbc~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp?)

成功！

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a776f80ec4524795b9ee2f691dde21c2~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp?)
