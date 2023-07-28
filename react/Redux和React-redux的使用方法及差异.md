---
link: https://juejin.cn/post/7079632416523943967
title: Redux和React-redux的使用方法及差异
description: 本文已参与[新人创作礼]活动，一起开启掘金创作之路。 当我们进行企业级项目开发时，父组件与子孙后代组件 和 两个没有联系的组件之间状态进行传递比较复杂 他们之间要进行数据通信，平时使用的props传值
keywords: 前端
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2022-03-27T04:35:24.000Z
publisher: 稀土掘金
stats: paragraph=49 sentences=33, words=91
---
本文已参与[新人创作礼]活动，一起开启掘金创作之路。

**当我们进行企业级项目开发时，父组件与子孙后代组件 和 两个没有联系的组件之间状态进行传递比较复杂 他们之间要进行数据通信，平时使用的props传值就会变得难以维护，因此我们需要一个中间容器 ，去管理我们的在开发中的状态及逻辑，当需要使用这些状态时，再从中间容器里取出需要的状态**

##### 一：Redux:是JS 应用的状态容器，提供可预测的状态管理

###### **Redux的工作流程:**

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8e6ba22382bd46ec9a33dc3508e41e45~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image?)

###### **Redux的使用方法:**

###### 1. 首先创建一个store,只能有一个store

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/22e8a14bee0949dba8f35fbf6652ed9a~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image?)

###### 2. 创建一个reducer纯函数，里面用来返回store中的值

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7da33ceb686f48e7b4e255efa7767669~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image?)

###### 3. 将action提取到actionCreators中，

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2790f1f762614867896084dabe9dc7b9~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image?)

并且将action操作提取成常量 ，放在actionTypes中

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/186d12d54d2846dca0ff81da88ca0b28~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image?)

###### 4.外部组件调用 `dispatch&#xFF08;action&#xFF09;&#xFF0C;reducer` 将根据相应的 `action type` 执行相应的操作，将最新的数据返回给state

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/48fc15b5ba684e50ba23e8407939593b~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image?)

###### 5. `store` 中数据发生变化后，在组件中需要通过 `store.subscribe(this.storeChange)` 去订阅 `store` 中的数据变化，并且将其值set进组件的state中，这样组件中的数据就得到了更新

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6f2de35c3612420083c5c48050b6940c~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image?)

##### 另：使用 `axio` 获取接口与 `Redux` 结合

###### 1. 需要安装 `react-thunk` ,然后进行中间件的配置

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/921a067d61f64fd39a097494f3a0e3db~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image?)

###### 2. 添加一个异步 `action`

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/aa25cfa201104f91b0e4b8e9c4e046d2~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image?)

###### 3. 在组件的生命周期函数 `componentDidMount` 中去获取初始化数据， `store.dispatch&#xFF08;&#xFF09;` 里可以直接接受异步请求函数，也可

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d2ba04b772fd40ad850a2e28de85559f~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image?)

##### 另：react-saga 也是进行异步请求数据的中间件，

###### 1. 需要安装react-saga ,然后进行中间件的配置

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c2ce7939e79545dea3ad1b6e52a25573~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image?)

###### 2. 新建一个sagas.js文件

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7e8f5cc4ed1b48c68bcb8831a16d3a88~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image?)

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2833e97f3da44dc6a6dc444d9ee2d063~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image?)

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b42732ecb4484b95888375d21d562ee0~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image?)

也可实现异步请求结合redux，更新state

##### 二：React-redux：他是react的一个插件，用于简化redux在react 中的使用，原理和redux一样 ，只是使用方法有所不同

**使用方法:**

###### 1. 安装react-redux，用provider将需要共享数据的组件包裹起来，就不需要在每个组件中去订阅store的变化，直接在组件中就可以使用

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2886b95c786f43a1b95c5eda727500d2~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image?)

###### 2. 创建store

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/88d2a0c512d94e5687df749e7b7721b1~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image?)

###### 3. 创建reducer

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/412c4eaa97ce4e5dbbcd73910dde02d3~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image?)

###### 4. react-redux与redux在组件中调用方式不同，首先创建一个映射关系stateToProps ，将state的值转换为组件中的props的值，第二步创建提个组件的事件方法，去dispatch(action)更新state,最后使用connect将两个参数结合起来，同时把当前组件传入进去

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/856039528b3749b9b5095e8e6fe442c0~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image?)

当然 ，在现在hooks技术日益强大之时，我们开发时经常使用useContext+useReducer进行数据共享

需要源码的话：

[github.com/susu1997/re...](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fsusu1997%2Fredux_practice "https://github.com/susu1997/redux_practice") redux

[github.com/susu1997/re...](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fsusu1997%2Freact-redux-practice "https://github.com/susu1997/react-redux-practice") react-redux

本文是学习了技术胖老师课程的记录， 技术胖老师个人网站：[jspang.com/](https://link.juejin.cn?target=https%3A%2F%2Fjspang.com%2F "https://jspang.com/")

若文中有误，忘大佬指出
