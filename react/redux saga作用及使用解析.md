---
link: https://juejin.cn/post/7022844937485959204
title: redux saga作用及使用解析
description: redux saga 一.作用 redux saga：用于处理应用程序的副作用，比如异步获取数据和接口的调用，作为redux的一个中间件，可以访问redux的state和dispatch action
keywords: 前端,Redux
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2021-10-25T03:45:37.000Z
publisher: 稀土掘金
stats: paragraph=75 sentences=32, words=201
---
小知识，大挑战！本文正在参与"[程序员必备小知识](https://juejin.cn/post/7008476801634680869 "https://juejin.cn/post/7008476801634680869")"创作活动。

## redux saga

### 一.作用

redux saga：用于处理应用程序的副作用，比如异步获取数据和接口的调用，作为redux的一个中间件，可以访问redux的state和dispatch action。

### 二.基本使用

1.创建一个saga.js文件

1）创建saga

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7ad9c9b11e074f8d8eaeffbebdca1ac1~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image?)

2）需要是这多个saga能同时实行，需要使用all

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e6116c63fd2d40de93df1c69f3e9aafd~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image?) takeEvery：监听多个action，允许多个任务同时实行，被调用的任务无法控制何时被调用， 它们将在每次 action 被匹配时不断的地被调用。

takeLatest：该情况下只允许一个任务的实行，实行的这个任务是最后被启动的那个，如果在它之前有一个任务没有执行结束，它将会被取消。

all：类似Promise中的all操作，提供了一种并行执行异步请求的方式。可以将多个异步操作作为参数参入all函数中，如果有一个call操作失败或者所有call操作都成功返回，则本次all操作执行完毕。

添加一个rootSaga，通过以下的方式添加多个saga，就可以同时启动里面包含的所有saga。

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7db7f94b17d746fc8a595b9cf41ff983~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image?) 2.修改main.js文件

1）引入saga

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5337f4b603ef4586ba70dbf1db86c115~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image?) 2）连接store，并运行

使用 redux-saga 模块的 createSagaMiddleware 工厂函数来创建一个 Saga middleware。

运行 rootSaga 之前，我们必须使用 applyMiddleware 将 middleware 连接至 Store。然后使用 sagaMiddleware.run() 运行 Saga。

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f99d4e81f4694599bfdfeb457ee3aea0~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image?) 3.测试

1）创建文件 saga.spec.js

incrementAsync 是一个 Generator 函数。执行的时候返回一个 iterator object，这个 iterator 的 next 方法返回一个如下格式的对象：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c7526b37a0ae46fa945e8a5bf4a701f6~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image?) 2）将saga.js中的yield delay(1000) 改成 yield call(delay, 1000)

call和 put一样， 返回一个 Effect对象，告诉 middleware 使用给定的参数调用给定的函数。实际上，无论是 put 还是 call 都不执行任何 dispatch 或异步调用，它们只是简单地返回 plain Javascript 对象（js文本对象）。

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/85258733b3e94284b2888afc79021d95~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image?) 这里发生的事情是：

middleware 检查每个被 yield 的 Effect 的类型，然后决定如何实现哪个 Effect。

如果 Effect 类型是 PUT 那 middleware 会 dispatch 一个 action 到 Store。

如果 Effect 类型是 CALL 那么它会调用给定的函数。

3） npm test

### 三.概念

1. takeEvery：takeEvery 允许多个 任务实例同时启动。在某个特定时刻，尽管之前还有一个或多个任务尚未结束，我们还是可以启动一个新的任务。

2.takeLatest：任何时刻只允许一个任务的实行，尽管它之前的任务没有执行完也会被取消，保证每次执行的任务都是最新的任务。

1. Effects

redux-saga 提供多个effects对象，比如call，put，目的是让Generator 将会 yield 包含 指令 的文本对象（plain Objects），redux-saga middleware 将确保执行这些指令并将指令的结果回馈给 Generator。 这样的话，在测试 Generator 时，所有我们需要做的就是，将 yield 后的对象作一个简单的 deepEqual 来检查它是否 yield 了我们期望的指令。

1）call 表示调用异步函数，用来调用接口，支持promise

2）put 表示 dispatch action，用于触发action

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7e76b81b00b04a1ca4d7439f7b46380e~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image?)

### 四.错误处理

1、使用熟悉的 try/catch 语法在 Saga 中捕获错误

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8ca80897908443279205cda8813e9a82~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image?) 2、通过API返回一个错误的标识

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d3f7f27f2a0f495a8bba130cad5423e5~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image?)

### 五、take 和 takeEvery

take的表现同takeEvery一样，都是监听某个action，但与takeEvery不同的是，他不是每次action触发的时候都相应，而只是在执行顺序执行到take语句时才会相应action。

1）takeEvery只是监听每个action，然后执行处理函数。对于何时相应action和 如何相应action，takeEvery并没有控制权。

2）take则不一样，我们可以在generator函数中决定何时相应一个action，以及一个action被触发后做什么操作。

### 六、无阻塞调用

[fork](https://link.juejin.cn?target=http%3A%2F%2Fleonshi.com%2Fredux-saga-in-chinese%2Fdocs%2Fapi%2Findex.html%23forkfn-args "http://leonshi.com/redux-saga-in-chinese/docs/api/index.html#forkfn-args")：当我们 fork 一个 任务，任务会在后台启动，调用者也可以继续它自己的流程，而不用等待被 fork 的任务结束。

fork和cancel

通常fork和cancel配合使用，实现非阻塞任务，take是阻塞状态，也就是实现执行take的时候，无法继续向下执行，fork是非阻塞的，同样可以使用cancel取消一个fork任务

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3e7f7f991e804b36a076e5f7c46846f3~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image?)

当执行yield fork(authorize, user, password),同时执行yield take(['LOGOUT', 'LOGIN_ERROR'])

### 七、同步执行多个任务

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fbea00ba3d55413f8fb638515ddd1b3c~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image?)

第二个yield 的执行需要在第一个yield执行完毕之后才会执行，因此如需同步执行多个任务应该改成如下的写法：

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e4898056ec074762a322a8f6755682c4~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image?)

### 八、多个Effect直接启动race

有时候我们同时启动多个任务，但又不想等待所有任务完成，我们只希望拿到 胜利者：即第一个被 resolve（或 reject）的任务。 race Effect 提供了一个方法，类似于promise.race方法

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c823c0e4009e448da025b5d78b8ff407~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image?)

### 九、取消任务

1）手动取消

yield cancel(task)

该方法不会等待被取消的任务完成。cancelEffect 的行为和 fork 有点类似。 一旦取消发起，它就会尽快返回。一旦取消，任务通常应尽快完成它的清理逻辑然后返回。

2）自动取消

race Effect：除了最先resolve或reject的那个任务，其他的都将被取消

all：一旦有某个任务reject，其他的也将会被拒绝。

### 十、使用channels

actionChannel：当多次触发多个action时，如果前面有阻塞，该helper Effect可以缓存传入的消息，依次处理。

buffers：控制缓存的数量。如果只想处理最近5个存储信息，可以用如下方式。

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3a264fb2869945b8b6979712464d94dd~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image?)

### 十一、使用技巧

1、节流：

引入一个delay函数，如下：

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cdbb8d1b50e242f1a14cfff8e9c65d6d~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image?)

该 Saga 在 500ms 这段时间，最多接受一个 INPUT_CHANGED action，并且可以继续处理 trailing action。

2、防抖

1）引入delay函数

可以在被 fork 的任务里放一个内置的 delay

2）可以使用takeLatest
