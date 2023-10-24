---
link: https://blog.csdn.net/m0_60676278/article/details/129578901
title: redux、react-redux、redux-thunk、redux-saga、@reduxjs/toolkit理解与使用，区别与使用场景？
description: 无论你是一个全新的Redux用户设置你的第一个项目，还是一个有经验的用户谁想要简化现有的应用程序，Redux工具包可以使我们的的Redux代码更好。redux是react中进行state状态管理的JS库，一般是管理多个组件中共享数据的，它并不是react的插件，是一个独立的库vue和angular等等一些框架都是可以使用的。我们不能解决每一个用例，但本着创建-反应-应用的精神，我们可以尝试提供一些抽象的设置过程和处理最常见的用例的工具，以及包括一些有用的实用程序，让用户简化他们的应用程序代码。
keywords: redux、react-redux、redux-thunk、redux-saga、@reduxjs/toolkit理解与使用，区别与使用场景？
author: 米奇妙妙Wuu Csdn认证博客专家 Csdn认证企业博客 码龄2年 暂无认证
date: 2023-10-20T08:46:24.000Z
publisher: null
stats: paragraph=66 sentences=101, words=695
---
## 1.redux

Redux的核心概念其实很简单：将需要修改的state都存入到store里，发起一个action用来描述发生了什么，用reducers描述action如何改变state tree 。创建store的时候需要传入reducer，真正能改变store中数据的是store.dispatch API。

* src下新建store文件夹,新建index.js作为store的输出文件
* store文件夹下新建index.js文件
* 新建reducer.js ,actionTypes.js文件
* actionTypes文件夹的作用是存放常量名
![](https://img-blog.csdnimg.cn/468026e9257c4fa18ea937ddb0f87ab2.png)
* 在组件中引入store

```js
import React, { Component } from 'react';
import { Input ,Button,List } from 'antd';
import store from './store';
import {CHANGE_INPUT_VALUE,ADD_TODO_ITEM,DELETE_TODO_ITEM} from  './store/actionTypes'
class TodoList extends Component {
   constructor(props) {
       super(props);
       this.state = store.getState();
       this.handleStoreChange = this.handleStoreChange.bind(this);
       this.handleBtnClick = this.handleBtnClick.bind(this);
       this.handleInputChange = this.handleInputChange.bind(this);
       store.subscribe(this.handleStoreChange)
   }

   handleInputChange(e) {
       const action = {
           type: CHANGE_INPUT_VALUE,
           value: e.target.value
       }
       store.dispatch(action)
   }

   handleBtnClick() {
       const action = {
           type: ADD_TODO_ITEM
       }
       store.dispatch(action)
   }

   render() {
       return (
           <div style={{marginTop:'20px',marginLeft:'15px'}}>
               <div>
                   <Input
                       value={this.state.inputValue}
                       placeholder="input"
                       style={{width:'300px'}}
                       onChange={this.handleInputChange}
                   />
                   <Button onClick={this.handleBtnClick} type="primary">Primary</Button>
               </div>
               <List
                   style={{marginTop:'15px',width:'300px'}}
                   bordered
                   dataSource={this.state.list}
                   renderItem={(item,index) => <List.Item onClick={this.handleItemDelete.bind(this,index)}>{item}</List.Item>}
                   />
           </div>
       )
   }

   handleStoreChange() {
       this.setState(store.getState())
   }
   handleItemDelete(index) {
       const action = {
           type: DELETE_TODO_ITEM,
           index
       }
       store.dispatch(action)
   }
}

export default TodoList;
```

* 在store下的index中使用redux-devtool

```js
import { createStore } from 'redux';
import reducer from './reducer'

const store = createStore(
    reducer,
    window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()
);

export default store;
```

* actionTypes.js代码如下

```js
export const  CHANGE_INPUT_VALUE = 'change_input_value';
export const  ADD_TODO_ITEM = 'add_todo_item';
export const  DELETE_TODO_ITEM = 'delete_todo_item';
```

* reducer.js代码如下

```js
import {CHANGE_INPUT_VALUE,ADD_TODO_ITEM,DELETE_TODO_ITEM} from  './actionTypes'
const defaultState = {
   inputValue:'aaa',
   list:['1','2']
}

export default (state = defaultState,action) => {
   if(action.type === CHANGE_INPUT_VALUE) {
       const newState = JSON.parse(JSON.stringify(state));
       newState.inputValue = action.value;
       return newState;
   }
   if(action.type === ADD_TODO_ITEM) {
       const newState = JSON.parse(JSON.stringify(state));
       newState.list.push(newState.inputValue);
       newState.inputValue = '';
       return newState;
   }
   if(action.type === DELETE_TODO_ITEM) {
       const newState = JSON.parse(JSON.stringify(state));
       newState.list.splice(action.index,1);
       return newState;
   }
   return state;
}
```

* 优化：使用actionCreactor.js来统一管理action
![](https://img-blog.csdnimg.cn/363c71cdccd44739ac4232b0f2275ec5.png)

## 2.react-redux

React-Redux是Redux的官方React绑定库。它能够使你的React组件从Redux store中读取数据，并且向store分发actions以更新数据。

1.在入口文件index.js里引入react-redux及store

```js
import React from 'react';
import ReactDOM from 'react-dom';
import 'antd/dist/antd.css';
import './index.css';
import App from './TodoList';
import * as serviceWorker from './serviceWorker';
import store from './store'
import { Provider } from 'react-redux';

const ProviderApp = (
    <Provider store={store}>
        <App></App>
    </Provider>
)

ReactDOM.render(ProviderApp, document.getElementById('root'));
serviceWorker.unregister();
```

2.在组件里做connect

```js
import React, { Component } from 'react';
import { Input ,Button,List } from 'antd';
import {CHANGE_INPUT_VALUE,ADD_TODO_ITEM} from  './store/actionTypes'
import {connect} from 'react-redux';

class TodoList extends Component {
    render() {
        const {handleInputChange,handleBtnClick} = this.props
        return (
            <div style={{marginTop:'20px',marginLeft:'15px'}}>
                <div>
                    <Input
                        value={this.props.inputValue}
                        placeholder="input"
                        style={{width:'300px'}}
                        onChange={handleInputChange}
                    />
                    <Button onClick={handleBtnClick} type="primary">Primary</Button>
                </div>
                <List
                    style={{marginTop:'15px',width:'300px'}}
                    bordered
                    dataSource={this.props.list}
                    renderItem={(item,index) => <List.Item>{item}</List.Item>}
                    />
            </div>
        )
    }

}
const mapStateToProps = (state) => {
    return {
        inputValue: state.inputValue,
        list : state.list
    }
}

const mapDispatchToProps = (dispatch) => {
    return {
        handleInputChange(e) {
            const action = {
                type: CHANGE_INPUT_VALUE,
                value: e.target.value
            }
            dispatch(action)
        },
        handleBtnClick() {
            const action = {
                type: ADD_TODO_ITEM
            }
            dispatch(action)
        },
    }
}

export default connect(mapStateToProps,mapDispatchToProps)(TodoList);
```

## 3.说一下redux和react-redux的区别：

redux是react中进行state状态管理的JS库，一般是管理多个组件中共享数据的，它并不是react的插件，是一个独立的库vue和angular等等一些框架都是可以使用的。

React-Redux是Redux的官方React绑定库。它能够使你的React组件从Redux store中读取数据，并且向store分发actions以更新数据。

## 4.说说你对@reduxjs/toolkit的理解？和react-redux有什么区别？

ReduxToolkit的目的是：在成为编写Redux逻辑的标准方法。

它最初是为了帮助解决关于Redux的三个常见问题而创建的：
1."配置Redux存储太复杂"
2."我必须添加很多软件包来让Redux做任何有用的事情
3."Redux需要太多的样板代码"

我们不能解决每一个用例，但本着创建-反应-应用的精神，我们可以尝试提供一些抽象的设置过程和处理最常见的用例的工具，以及包括一些有用的实用程序，让用户简化他们的应用程序代码。

ReduxToolkit还包括一个强大的数据获取和缓存功能，我们称之为"RTK查询"。它作为一组单独的入口点包含在软件包中。它是可选的，但可以消除手工编写数据获取逻辑的需要。

这些工具应该是有益的所有Redux用户。无论你是一个全新的Redux用户设置你的第一个项目，还是一个有经验的用户谁想要简化现有的应用程序，Redux工具包可以使我们的的Redux代码更好。

* react-redux 是react官方推出的redux绑定库，react-redux将所有组件分为两大类：UI组件和容器组件，其中所有容器组件包裹着UI组件，构成父子关系。容器组件负责和redux交互，里面使用redux API函数，UI组件负责页面渲染，不使用任何redux API。容器组件会给UI组件传递redux中保存对的状态和操作状态的方法
* @reduxjs/toolkit 是Redux 官方强烈推荐，开箱即用的一个高效的 Redux 开发工具集。它旨在成为标准的 Redux 逻辑开发模式，使用 Redux Toolkit 都可以优化你的代码，使其更可维护

## 5.redux-thunk使用

dispatch一个action之后，到达reducer之前，进行一些额外的操作，就需要用到middleware。
可以利用 Redux middleware 来进行日志记录、创建崩溃报告、调用异步接口或者路由等等。
换言之，中间件都是对store.dispatch()的增强。redux-thunk就是用来异步操作，比如接口请求等。

当我们返回的是函数时，store会帮我们调用这个返回的函数，并且把dispatch暴露出来供我们使用。对于redux-thunk的整个流程来说，它是等异步任务执行完成之后，我们再去调用dispatch，然后去store去调用reduces

* 引入redux-thunk

```js
import { applyMiddleware, createStore } from 'redux';
import thunk from 'redux-thunk';
 const store = createStore(
  reducers,
  applyMiddleware(thunk)
);
```

* 在actionCreactor中创建一个带异步函数的方法了

```js
export const getTodoList = () => {
    return () => {
        axios.get('./list').then((res)=>{
            const data = res.data;
            const action  = initListAction(data);
            StorageEvent.dispatch(action);
        })
    }
}
```

## 6.redux-saga使用

redux-saga是一个用于管理redux应用异步操作的中间件，redux-saga通过创建sagas将所有异步操作逻辑收集在一个地方集中处理，可以用来代替redux-thunk中间件。

当我们dispatch的action类型不在reducer中时，redux-saga的监听函数takeEvery就会监听到，等异步任务有结果就执行put方法，相当于dispatch，再一次触发dispatch。对于redux-saga的整个流程来说，它是等执行完action和reducer之后，判断reducer中有没有这个action

在store.js里引入redux-saga

```js
import { createStore, applyMiddleware } from 'redux'
import createSagaMiddleware from 'redux-saga'

import reducer from './reducers'
import mySaga from './sagas'

const sagaMiddleware = createSagaMiddleware()

const store = createStore(
  reducer,
  applyMiddleware(sagaMiddleware)
)

sagaMiddleware.run(mySaga)；
export default store
```

新建 saga.js

```js
import { call, put, takeEvery, takeLatest } from 'redux-saga/effects'
import Api from '...'

function* fetchUser(action) {
   try {
      const user = yield call(Api.fetchUser, action.payload.userId);
      yield put({type: "USER_FETCH_SUCCEEDED", user: user});
   } catch (e) {
      yield put({type: "USER_FETCH_FAILED", message: e.message});
   }
}
function* mySaga() {
  yield takeEvery("USER_FETCH_REQUESTED", fetchUser);
}

export default mySaga;
```

## 7. redux-saga和redux-thunk的区别和使用场景

qubie redux-thunk和redux-saga处理异步任务的时机不一样。
对于redux-saga，相对于在redux的action基础上，重新开辟了一个 async action的分支， **单独**处理异步任务。
saga 自己基本上完全弄了一套 asyc 的事件监听机制，代码量大大增加

从使用体验来看 redux-thunk 更简单，和 redux 本身联系地更紧密。尤其是整个生态都向函数式编程靠拢的今天，redux-thunk 的高阶函数看上去更加契合这个闭环。

在dispatch一个action之后，到达reducer之前，进行一些额外的操作，就需要用到middleware。中间件都是对store.dispatch()的增强。
我们可以利用 Redux middleware 来进行日志记录、创建崩溃报告、调用异步接口或者路由等等。
redux-thunk就是用来异步操作，比如接口请求等。

redux-saga是一个用于管理redux应用异步操作的中间件，redux-saga通过创建sagas将所有异步操作逻辑收集在一个地方集中处理，可以用来代替redux-thunk中间件。
通过定义saga函数进行监控，同时提供一些常用的API
