---
link: https://juejin.cn/post/6844903846666321934
title: 一篇文章总结redux、react-redux、redux-saga
description: 不愿清醒，宁愿一直沉迷放纵。 不知归路，宁愿一世无悔追逐。 --- 王小波 redux是的诞生是为了给 React 应用提供「可预测化的状态管理」机制。 提供subscribe，dispatch，getState这些方法。 按步骤手把手实战。 不就ok了吗？这就是 react-…
keywords: React.js
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2019-05-19T01:26:16.000Z
publisher: 稀土掘金
stats: paragraph=107 sentences=50, words=1078
---
不愿清醒，宁愿一直沉迷放纵。 不知归路，宁愿一世无悔追逐。 --- 王小波

> 本篇主要将react全家桶的产品非常精炼的提取了核心内容，精华程度堪比精油。各位大人，既然来了，客官您坐，来人，给客官看茶~~

React有props和state:

这就意味着如果是一个数据状态非常复杂的应用，更多的时候发现 **React根本无法让两个组件互相交流**，使用对方的数据，react的通过层级传递数据的这种方法是非常难受的，这个时候，迫切需要一个机制， **把所有的state集中到组件顶部，能够灵活的将所有state各取所需的分发给所有的组件**，是的，这就是redux

## 使用步骤

## 按步骤手把手实战。

上述步骤，对应的序号，我会在相关代码标出

```
npm install redux -S

<span class="hljs-keyword">import</span> { createStore } <span class="hljs-keyword">from</span> <span class="hljs-string">'redux'</span>

<span class="hljs-keyword">const</span> reducer = <span class="hljs-function">(<span class="hljs-params">state = {count: <span class="hljs-number">0</span>}, action</span>) =></span> {----------> &#x2474;
  <span class="hljs-keyword">switch</span> (action.type){
    <span class="hljs-keyword">case</span> <span class="hljs-string">'INCREASE'</span>: <span class="hljs-keyword">return</span> {<span class="hljs-attr">count</span>: state.count + <span class="hljs-number">1</span>};
    <span class="hljs-keyword">case</span> <span class="hljs-string">'DECREASE'</span>: <span class="hljs-keyword">return</span> {<span class="hljs-attr">count</span>: state.count - <span class="hljs-number">1</span>};
    <span class="hljs-keyword">default</span>: <span class="hljs-keyword">return</span> state;
  }
}

<span class="hljs-keyword">const</span> actions = {---------->&#x2475;
  increase: <span class="hljs-function"><span class="hljs-params">()</span> =></span> ({<span class="hljs-attr">type</span>: <span class="hljs-string">'INCREASE'</span>}),
  <span class="hljs-attr">decrease</span>: <span class="hljs-function"><span class="hljs-params">()</span> =></span> ({<span class="hljs-attr">type</span>: <span class="hljs-string">'DECREASE'</span>})
}

<span class="hljs-keyword">const</span> store = createStore(reducer);---------->&#x2476;

store.subscribe(<span class="hljs-function"><span class="hljs-params">()</span> =></span>
  <span class="hljs-built_in">console</span>.log(store.getState())
);

store.dispatch(actions.increase())
store.dispatch(actions.increase())
store.dispatch(actions.increase())
```

自己画了一张非常简陋的流程图，方便理解redux的工作流程

刚开始就说了，如果把store直接集成到React应用的顶层props里面，只要各个子组件能访问到顶层props就行了，比如这样：

```
<顶层组件 store="{store}">
  <app>
</app></顶层组件>
```

不就ok了吗？这就是 react-redux。Redux 官方提供的 React 绑定库。 具有高效且灵活的特性。

## **React Redux 将组件区分为 容器组件 和 UI 组件**

## 两个核心

* **Provider**看我上边那个代码的 **顶层组件**4个字。对，你没有猜错。这个顶级组件就是Provider,一般我们都将顶层组件包裹在Provider组件之中，这样的话，所有组件就都可以在react-redux的控制之下了, **但是store必须作为参数放到Provider组件中去**

```
<provider store="{store}">
    <app>
<provider>
</provider></app></provider>
```

```
&#x8FD9;&#x4E2A;&#x7EC4;&#x4EF6;&#x7684;&#x76EE;&#x7684;&#x662F;&#x8BA9;&#x6240;&#x6709;&#x7EC4;&#x4EF6;&#x90FD;&#x80FD;&#x591F;&#x8BBF;&#x95EE;&#x5230;Redux&#x4E2D;&#x7684;&#x6570;&#x636E;&#x3002;
```
* **connect**这个才是react-redux中比较难的部分，我们详细解释一下 首先，先记住下边的这行代码：

```
connect(mapStateToProps, mapDispatchToProps)(MyComponent)
```

**mapStateToProps** 这个单词翻译过来就是 **把state映射到props中去** ,其实也就是 **把Redux中的数据映射到React中的props中去。** 举个栗子：

```
    const mapStateToProps = (state) => {
      <span class="hljs-built_in">return</span> {
      	// prop : state.xxx  | &#x610F;&#x601D;&#x662F;&#x5C06;state&#x4E2D;&#x7684;&#x67D0;&#x4E2A;&#x6570;&#x636E;&#x6620;&#x5C04;&#x5230;props&#x4E2D;
        foo: state.bar
      }
    }
```

然后渲染的时候就可以使用this.props.foo

```
class Foo extends Component {
    constructor(props){
        super(props);
    }
    <span class="hljs-function"><span class="hljs-title">render</span></span>(){
        <span class="hljs-built_in">return</span>(
        	// &#x8FD9;&#x6837;&#x5B50;&#x6E32;&#x67D3;&#x7684;&#x5176;&#x5B9E;&#x5C31;&#x662F;state.bar&#x7684;&#x6570;&#x636E;&#x4E86;
            <div>this.props.foo</div>
        )
    }
}
Foo = connect()(Foo);
<span class="hljs-built_in">export</span> default Foo;
```

然后这样就可以完成渲染了

这个单词翻译过来就是就是 **把各种dispatch也变成了props让你可以直接使用**

```
const mapDispatchToProps = (dispatch) => { // &#x9ED8;&#x8BA4;&#x4F20;&#x9012;&#x53C2;&#x6570;&#x5C31;&#x662F;dispatch
  <span class="hljs-built_in">return</span> {
    onClick: () => {
      dispatch({
        <span class="hljs-built_in">type</span>: <span class="hljs-string">'increatment'</span>
      });
    }
  };
}
```

```
class Foo extends Component {
    constructor(props){
        super(props);
    }
    <span class="hljs-function"><span class="hljs-title">render</span></span>(){
        <span class="hljs-built_in">return</span>(

             <button onclick="{this.props.onClick}">&#x70B9;&#x51FB;increase</button>
        )
    }
}
Foo = connect()(Foo);
<span class="hljs-built_in">export</span> default Foo;
```

组件也就改成了上边这样，可以直接通过this.props.onClick，来调用dispatch,这样子就不需要在代码中来进行store.dispatch了

react-redux的基本介绍就到这里了

如果按照原始的redux工作流程，当组件中产生一个action后会直接触发reducer修改state，reducer又是一个纯函数，也就是不能再reducer中进行异步操作；

**而往往实际中，组件中发生的action后，在进入reducer之前需要完成一个异步任务,比如发送ajax请求后拿到数据后，再进入reducer,显然原生的redux是不支持这种操作的**

这个时候急需一个中间件来处理这种业务场景，目前最优雅的处理方式自然就是redux-saga

## 核心讲解

redux-saga提供了一些辅助函数，用来在一些特定的action 被发起到Store时派生任务，下面我先来讲解两个辅助函数： `takeEvery` 和 `takeLatest`

* **takeEvery*

**takeEvery就像一个流水线的洗碗工，过来一个脏盘子就直接执行后面的洗碗函数，一旦你请了这个洗碗工他会一直执行这个工作，绝对不会停止接盘子的监听过程和触发洗盘子函数**

例如：每次点击 按钮去Fetch获取数据时时，我们发起一个 FETCH_REQUESTED 的 action。 我们想通过启动一个任务从服务器获取一些数据，来处理这个action，类似于

```
window.addEventLister(<span class="hljs-string">'xxx'</span>,fn)
```

当dispatch xxx的时候，就会执行fn方法，

首先我们创建一个将执行异步 action 的任务(也就是上边的fn)：

```
// put&#xFF1A;&#x4F60;&#x5C31;&#x8BA4;&#x4E3A;put&#x5C31;&#x7B49;&#x4E8E; dispatch&#x5C31;&#x53EF;&#x4EE5;&#x4E86;&#xFF1B;

// call&#xFF1A;&#x53EF;&#x4EE5;&#x7406;&#x89E3;&#x4E3A;&#x5B9E;&#x884C;&#x4E00;&#x4E2A;&#x5F02;&#x6B65;&#x51FD;&#x6570;,&#x662F;&#x963B;&#x585E;&#x578B;&#x7684;&#xFF0C;&#x53EA;&#x6709;&#x8FD0;&#x884C;&#x5B8C;&#x540E;&#x9762;&#x7684;&#x51FD;&#x6570;&#xFF0C;&#x624D;&#x4F1A;&#x7EE7;&#x7EED;&#x5F80;&#x4E0B;&#xFF1B;
// &#x5728;&#x8FD9;&#x91CC;&#x53EF;&#x4EE5;&#x7247;&#x9762;&#x7684;&#x7406;&#x89E3;&#x4E3A;async&#x4E2D;&#x7684;await&#xFF01;&#x4F46;&#x5199;&#x6CD5;&#x76F4;&#x89C2;&#x591A;&#x4E86;&#xFF01;
import { call, put } from <span class="hljs-string">'redux-saga/effects'</span>

<span class="hljs-built_in">export</span> <span class="hljs-keyword">function</span>* fetchData(action) {
   try {
      const apiAjax = (params) => fetch(url, params);
      const data = yield call(apiAjax);
      yield put({<span class="hljs-built_in">type</span>: <span class="hljs-string">"FETCH_SUCCEEDED"</span>, data});
   } catch (error) {
      yield put({<span class="hljs-built_in">type</span>: <span class="hljs-string">"FETCH_FAILED"</span>, error});
   }
}
```

然后在每次 FETCH_REQUESTED action 被发起时启动上面的任务,也就 **相当于每次触发一个名字为 FETCH_REQUESTED 的action就会执行上边的任务**,代码如下

```
import { takeEvery } from <span class="hljs-string">'redux-saga'</span>

<span class="hljs-keyword">function</span>* <span class="hljs-function"><span class="hljs-title">watchFetchData</span></span>() {

  yield* takeEvery(<span class="hljs-string">"FETCH_REQUESTED"</span>, fetchData)
}
```

**注意**：上面的 takeEvery 函数可以使用下面的写法替换

```
<span class="hljs-keyword">function</span>* <span class="hljs-function"><span class="hljs-title">watchFetchData</span></span>() {

   <span class="hljs-keyword">while</span>(<span class="hljs-literal">true</span>){
     yield take(<span class="hljs-string">'FETCH_REQUESTED'</span>);
     yield fork(fetchData);
   }
}
```

* **takeLatest*

在上面的例子中，takeEvery **允许多个 fetchData 实例同时启动**，在某个特定时刻，我们可以启动一个新的 fetchData 任务， 尽管之前还有一个或多个 fetchData 尚未结束

如果我们 **只想得到最新那个请求的响应**（例如，始终显示最新版本的数据），我们可以使用 takeLatest 辅助函数

```
import { takeLatest } from <span class="hljs-string">'redux-saga'</span>

<span class="hljs-keyword">function</span>* <span class="hljs-function"><span class="hljs-title">watchFetchData</span></span>() {
  yield* takeLatest(<span class="hljs-string">'FETCH_REQUESTED'</span>, fetchData)
}
```

**和takeEvery不同，在任何时刻 takeLatest 只允许执行一个 fetchData 任务，并且这个任务是最后被启动的那个，如果之前已经有一个任务在执行，那之前的这个任务会自动被取消**

redux-saga框架提供了很多创建effect的函数，下面我们就来简单的介绍下开发中最常用的几种

* take(pattern)
* put(action)
* call(fn, ...args)
* fork(fn, ...args)
* select(selector, ...args)

take函数可以理解为监听未来的action，它创建了一个命令对象，告诉middleware等待一个特定的action， Generator会暂停，直到一个与pattern匹配的action被发起，才会继续执行下面的语句，也就是说，take是一个阻塞的 effect

用法：

```
<span class="hljs-keyword">function</span>* <span class="hljs-function"><span class="hljs-title">watchFetchData</span></span>() {
   <span class="hljs-keyword">while</span>(<span class="hljs-literal">true</span>) {
   // &#x76D1;&#x542C;&#x4E00;&#x4E2A;<span class="hljs-built_in">type</span>&#x4E3A; <span class="hljs-string">'FETCH_REQUESTED'</span> &#x7684;action&#x7684;&#x6267;&#x884C;&#xFF0C;&#x76F4;&#x5230;&#x7B49;&#x5230;&#x8FD9;&#x4E2A;Action&#x88AB;&#x89E6;&#x53D1;&#xFF0C;&#x624D;&#x4F1A;&#x63A5;&#x7740;&#x6267;&#x884C;&#x4E0B;&#x9762;&#x7684; 		yield fork(fetchData)  &#x8BED;&#x53E5;
     yield take(<span class="hljs-string">'FETCH_REQUESTED'</span>);
     yield fork(fetchData);
   }
}
```

put函数是用来发送action的 effect，你可以简单的 **把它理解成为redux框架中的dispatch函数**，当put一个action后，reducer中就会计算新的state并返回， **注意：** **put 也是阻塞 effect**

用法：

```
<span class="hljs-built_in">export</span> <span class="hljs-keyword">function</span>* <span class="hljs-function"><span class="hljs-title">toggleItemFlow</span></span>() {
    <span class="hljs-built_in">let</span> list = []
    // &#x53D1;&#x9001;&#x4E00;&#x4E2A;<span class="hljs-built_in">type</span>&#x4E3A; <span class="hljs-string">'UPDATE_DATA'</span> &#x7684;Action&#xFF0C;&#x7528;&#x6765;&#x66F4;&#x65B0;&#x6570;&#x636E;&#xFF0C;&#x53C2;&#x6570;&#x4E3A; `data&#xFF1A;list`
    yield put({
      <span class="hljs-built_in">type</span>: actionTypes.UPDATE_DATA,
      data: list
    })
}
```

**call函数你可以把它简单的理解为就是可以调用其他函数的函数**，它命令 middleware 来调用fn 函数， args为函数的参数， **注意：** **fn 函数可以是一个 Generator 函数，也可以是一个返回 Promise 的普通函数**，call 函数也是 **阻塞 effect**

用法：

```
<span class="hljs-built_in">export</span> const delay = ms => new Promise(resolve => <span class="hljs-built_in">set</span>Timeout(resolve, ms))

<span class="hljs-built_in">export</span> <span class="hljs-keyword">function</span>* <span class="hljs-function"><span class="hljs-title">removeItem</span></span>() {
  try {
    // &#x8FD9;&#x91CC;call &#x51FD;&#x6570;&#x5C31;&#x8C03;&#x7528;&#x4E86; delay &#x51FD;&#x6570;&#xFF0C;delay &#x51FD;&#x6570;&#x4E3A;&#x4E00;&#x4E2A;&#x8FD4;&#x56DE;promise &#x7684;&#x51FD;&#x6570;
    <span class="hljs-built_in">return</span> yield call(delay, 500)
  } catch (err) {
    yield put({<span class="hljs-built_in">type</span>: actionTypes.ERROR})
  }
}
```

fork 函数和 call 函数很像， **都是用来调用其他函数的，但是fork函数是非阻塞函数**，也就是说， **程序执行完 `yield fork(fn&#xFF0C; args)` 这一行代码后，会立即接着执行下一行代码语句，而不会等待fn函数返回结果后**，在执行下面的语句

用法：

```
import { fork } from <span class="hljs-string">'redux-saga/effects'</span>

<span class="hljs-built_in">export</span> default <span class="hljs-keyword">function</span>* <span class="hljs-function"><span class="hljs-title">rootSaga</span></span>() {
  // &#x4E0B;&#x9762;&#x7684;&#x56DB;&#x4E2A; Generator &#x51FD;&#x6570;&#x4F1A;&#x4E00;&#x6B21;&#x6267;&#x884C;&#xFF0C;&#x4E0D;&#x4F1A;&#x963B;&#x585E;&#x6267;&#x884C;
  yield fork(addItemFlow)
  yield fork(removeItemFlow)
  yield fork(toggleItemFlow)
  yield fork(modifyItem)
}
```

select 函数是用来指示 middleware调用提供的选择器获取Store上的state数据，你也可以简单的把它理解为 **redux框架中获取store上的 state数据一样的功能** ： `store.getState()`

用法：

```
<span class="hljs-built_in">export</span> <span class="hljs-keyword">function</span>* <span class="hljs-function"><span class="hljs-title">toggleItemFlow</span></span>() {
     // &#x901A;&#x8FC7; select effect &#x6765;&#x83B7;&#x53D6; &#x5168;&#x5C40; state&#x4E0A;&#x7684; `getTodoList` &#x4E2D;&#x7684; list
     <span class="hljs-built_in">let</span> tempList = yield select(state => state.getTodoList.list)
}
```

**index.js **

```
import React from <span class="hljs-string">'react'</span>;
import ReactDOM from <span class="hljs-string">'react-dom'</span>;
import {createStore, applyMiddleware} from <span class="hljs-string">'redux'</span>
import createSagaMiddleware from <span class="hljs-string">'redux-saga'</span>

import rootSaga from <span class="hljs-string">'./sagas'</span>
import Counter from <span class="hljs-string">'./Counter'</span>
import rootReducer from <span class="hljs-string">'./reducers'</span>

const sagaMiddleware = createSagaMiddleware() // &#x521B;&#x5EFA;&#x4E86;&#x4E00;&#x4E2A;saga&#x4E2D;&#x95F4;&#x4EF6;&#x5B9E;&#x4F8B;

// &#x4E0B;&#x8FB9;&#x8FD9;&#x53E5;&#x8BDD;&#x548C;&#x4E0B;&#x8FB9;&#x7684;&#x4E24;&#x884C;&#x4EE3;&#x7801;&#x521B;&#x5EFA;store&#x7684;&#x65B9;&#x5F0F;&#x662F;&#x4E00;&#x6837;&#x7684;
// const store = createStore(reducers,applyMiddlecare(middlewares))

const createStoreWithMiddleware = applyMiddleware(middlewares)(createStore)
const store = createStoreWithMiddleware(rootReducer)

sagaMiddleware.run(rootSaga)

const action = <span class="hljs-built_in">type</span> => store.dispatch({ <span class="hljs-built_in">type</span> })

<span class="hljs-keyword">function</span> <span class="hljs-function"><span class="hljs-title">render</span></span>() {
  ReactDOM.render(
    <counter value="{store.getState()}" onincrement="{()" => action(<span class="hljs-string">'INCREMENT'</span>)}
      onDecrement={() => action(<span class="hljs-string">'DECREMENT'</span>)}
      onIncrementAsync={() => action(<span class="hljs-string">'INCREMENT_ASYNC'</span>)} />,
    document.getElementById(<span class="hljs-string">'root'</span>)
  )
}

render()

store.subscribe(render)
</counter>
```

**sagas.js**

```
import { put, call, take,fork } from <span class="hljs-string">'redux-saga/effects'</span>;
import { takeEvery, takeLatest } from <span class="hljs-string">'redux-saga'</span>

<span class="hljs-built_in">export</span> const delay = ms => new Promise(resolve => <span class="hljs-built_in">set</span>Timeout(resolve, ms));

<span class="hljs-keyword">function</span>* <span class="hljs-function"><span class="hljs-title">incrementAsync</span></span>() {
  // &#x5EF6;&#x8FDF; 1s &#x5728;&#x6267;&#x884C; + 1&#x64CD;&#x4F5C;
  yield call(delay, 1000);
  yield put({ <span class="hljs-built_in">type</span>: <span class="hljs-string">'INCREMENT'</span> });
}

<span class="hljs-built_in">export</span> default <span class="hljs-keyword">function</span>* <span class="hljs-function"><span class="hljs-title">rootSaga</span></span>() {
  // <span class="hljs-keyword">while</span>(<span class="hljs-literal">true</span>){
  //   yield take(<span class="hljs-string">'INCREMENT_ASYNC'</span>);
  //   yield fork(incrementAsync);
  // }

  // &#x4E0B;&#x9762;&#x7684;&#x5199;&#x6CD5;&#x4E0E;&#x4E0A;&#x9762;&#x7684;&#x5199;&#x6CD5;&#x4E0A;&#x7B49;&#x6548;
  yield* takeEvery(<span class="hljs-string">"INCREMENT_ASYNC"</span>, incrementAsync)
}
```

**reducer.js**

```
<span class="hljs-built_in">export</span> default <span class="hljs-keyword">function</span> counter(state = 0, action) {
  switch (action.type) {
    <span class="hljs-keyword">case</span> <span class="hljs-string">'INCREMENT'</span>:
      <span class="hljs-built_in">return</span> state + 1
    <span class="hljs-keyword">case</span> <span class="hljs-string">'DECREMENT'</span>:
      <span class="hljs-built_in">return</span> state - 1
    <span class="hljs-keyword">case</span> <span class="hljs-string">'INCREMENT_ASYNC'</span>:
      <span class="hljs-built_in">return</span> state
    default:
      <span class="hljs-built_in">return</span> state
  }
}
```

从上面的代码结构可以看出，redux-saga的使用方式还是比较简单的，相比较之前的redux框架的CounterApp，多了一个sagas的文件，reducers文件还是之前的使用方式

ok,故事到这里就接近尾声了，以上主要介绍了redux,react-redux和redux-saga目前redux全家桶主流的一些产品,接下来,主要会产出一下根据源码, **手写一下redux和react-redux的轮子**
