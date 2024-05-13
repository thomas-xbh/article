---
link: https://juejin.cn/post/7107976218900693000
title: mobx6结合react使用React + TypeScript实践react-mobx6+使用案例mobx、mobx-react和mobx-react-lite新手入门React + MobX6从入门到实践（十二）TypeScript 在 React 中使用总结Typescript配合React实践TypeScript 实践Vue3.0 前的 TypeScript 最佳入门实践TypeScript 2.8下的终极React组件模式
description: mobx6在react项目中结合typescript的集成使用案例分析说明，新的mobx6与之前的版本存在很大不不同，弃用了装饰器的使用
keywords: 前端,React.js
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2022-06-11T13:39:28.000Z
publisher: 稀土掘金
stats: paragraph=51 sentences=76, words=555
---
先来看看Mobx官方对其描述：MobX 是一个身经百战的库，它通过运用透明的函数式响应编程（Transparent Functional Reactive Programming，TFRP）使状态管理变得简单和可扩展。

Mobx与Redux都是非常优秀的React状态管理方案，对于这两个状态管理方案的优点与不同，在此不做详细对比，此文主要基于Mobx6集成React和Typescript在项目中具体的使用方式进行说明。

[Mobx6](https://link.juejin.cn?target=https%3A%2F%2Fwww.mobxjs.com%2F "https://www.mobxjs.com/")相比于[Mobx5](https://link.juejin.cn?target=https%3A%2F%2Fcn.mobx.js.org%2F "https://cn.mobx.js.org/")和[Mobx4](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2FSangKa%2FMobX-Docs-CN%2Ftree%2F4.0.0%2Fdocs "https://github.com/SangKa/MobX-Docs-CN/tree/4.0.0/docs")在具体的使用方式上都有很大的不同，在 MobX 6 中不推荐使用[装饰器](https://link.juejin.cn?target=https%3A%2F%2Fso.csdn.net%2Fso%2Fsearch%3Fq%3D%25E8%25A3%2585%25E9%25A5%25B0%25E5%2599%25A8%26spm%3D1001.2101.3001.7020 "https://so.csdn.net/so/search?q=%E8%A3%85%E9%A5%B0%E5%99%A8&spm=1001.2101.3001.7020")语法，因为它不是 ES 标准，并且标准化过程要花费很长时间，但是通过配置仍然可以[启用装饰器](https://link.juejin.cn?target=https%3A%2F%2Fwww.mobxjs.com%2Fenabling-decorators "https://www.mobxjs.com/enabling-decorators")语法。

本案例采用create-react-app来快速创建react与typescript的环境。需要安装依赖如下：

* mobx
* mobx-react或者mobx-react-lite

src目录下新建mobxStore文件夹，并依次创建index.ts,timer.ts,user.ts文件。

```ts

import { makeAutoObservable } from 'mobx';

export interface TimerStoreType {
  timer: number;
  addTimer: Function;
  resetTimer: Function;
}
class TimerStore implements TimerStoreType {
  timer = 0;
  constructor() {

    makeAutoObservable(this);
  }

  addTimer() {
    this.timer += 1;
  }

  resetTimer() {
    this.timer = 0;
  }
}

export { TimerStore };

```

```ts

import { makeObservable, observable, computed, action } from 'mobx';

export interface UserStoreType {
  name: string;
  age: number;
  doubleAge: number;
  editName: (str: string) => void;
}

class UserStore {
  name = '张三';
  age = 18;
  get doubleAge() {
    return this.age * 2;
  }
  constructor() {

    makeObservable(this, {
      name: observable,
      age: observable,
      doubleAge: computed,
      editName: action,
    });
  }

  editName(str: string) {
    this.name = str;
  }
}

export { UserStore };

```

```ts

import { createContext } from 'react';
import { TimerStore, TimerStoreType } from './timer';
import { UserStore, UserStoreType } from './user';

export type { TimerStoreType, UserStoreType };

export const stores = {
  timerStore: new TimerStore(),
  usertore: new UserStore(),
};

export const useStore = createContext(stores);

```

```ts

import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import AppMobx from './AppMobx';

import { Provider } from 'mobx-react';
import { stores } from './mobxStore/index';
import reportWebVitals from './reportWebVitals';

ReactDOM.render(
  <React.StrictMode>
    <Provider {...stores}>
      <AppMobx>AppMobx>
    Provider>
  React.StrictMode>,
  document.getElementById('root')
);
reportWebVitals();
```

```ts

import React from 'react';

import ClassPage from './pages/MobxDemo/ClassPage';

import FnPage from './pages/MobxDemo/FnPage';
function AppMobx() {
  return (
    <div>
      <h3>mobx class组件用法h3>
      <ClassPage>ClassPage>
      <hr />
      <h3>mobx function组件用法h3>
      <FnPage>FnPage>
    div>
  );
}
export default AppMobx;
```

```ts
import React from 'react';
import { observer, inject } from 'mobx-react';
import { TimerStoreType } from '../../mobxStore/index';

interface IProps {

  timerStore?: TimerStoreType;
}
interface IState {}
('timerStore')

class ClassPage extends React.Component<IProps, IState> {
  constructor(props: IProps) {
    super(props);
    this.addMobx = this.addMobx.bind(this);
    this.resetMobx = this.resetMobx.bind(this);
    this.state = {};
  }
  addMobx() {
    this.props.timerStore?.addTimer();
  }
  resetMobx() {
    this.props.timerStore?.resetTimer();
  }
  render() {
    const { timerStore } = this.props;
    return (
      <div>
        <p>当前timer值：{timerStore?.timer}p>
        <button onClick={this.addMobx}>mobx值+1button>
        <button onClick={this.resetMobx}>重置button>
      div>
    );
  }
}
export default ClassPage;
```

class组件的函数式用法如下：

```ts
import React from 'react';
import { observer, inject } from 'mobx-react';
import { TimerStoreType } from '../../mobxStore/index';
interface IProps {
  timerStore?: TimerStoreType;
}
interface IState {}
class ClassPage extends React.Component<IProps, IState> {
  constructor(props: IProps) {
    super(props);
    this.addMobx = this.addMobx.bind(this);
    this.resetMobx = this.resetMobx.bind(this);
    this.state = {};
  }
  addMobx() {
    this.props.timerStore?.addTimer();
  }
  resetMobx() {
    this.props.timerStore?.resetTimer();
  }
  render() {
    const { timerStore } = this.props;
    return (
      <div>
        <p>当前timer值：{timerStore?.timer}p>
        <button onClick={this.addMobx}>mobx值+1button>
        <button onClick={this.resetMobx}>重置button>
      div>
    );
  }
}

export default inject('timerStore')(observer(ClassPage));
```

function组件使用参考如下：

```ts
import React, { useContext } from 'react';

import { observer } from 'mobx-react';

import { useStore } from '../../mobxStore/index';

interface IProps {}

const FnPage: React.FC = (props: IProps) => {

  const { timerStore } = useContext(useStore);
  return (
    <div>
      <p>当前timer值：{timerStore.timer}p>
      <button onClick={() => timerStore.addTimer()}>mobx值+1button>
      <button onClick={() => timerStore.resetTimer()}>重制button>
    div>
  );
};
export default observer(FnPage);
```

此文只是仅仅介绍了Mobx6集成React和Typescript的使用案例说明，供有需要了解的同学们参考。下篇文章会对mobx和mobx-react两个模块的核心api进行源码的分析，只要了解了源码的实现，我们才能在具体的项目中更好的使用mobx来进行状态管理。
