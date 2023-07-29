---
link: https://juejin.cn/post/6844903892522631175
title: React中setState的用法
description: 这种方式是我们最常用到的修改state的方式. 将setState()认为是一次请求而不是一次立即执行更新组件的命令。为了更为可观的性能，React可能会推迟它，稍后会一次性更新这些组件。React不会保证在setState之后，能够立刻拿到改变的结果。 setState()不…
keywords: React.js
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2019-07-18T08:57:55.000Z
publisher: 稀土掘金
stats: paragraph=30 sentences=63, words=574
---
```
    &#x6BCF;&#x4E2A;&#x4EBA;&#x90FD;&#x81F3;&#x5C11;&#x6709;&#x8FD9;&#x4E48;&#x4E00;&#x4E2A;&#x631A;&#x53CB;&#xFF0C;
    &#x4F60;&#x548C;&#x4ED6;&#x5728;&#x4EBA;&#x751F;&#x7684;&#x62D0;&#x70B9;&#x9047;&#x5230;&#xFF0C;
    &#x60CA;&#x53F9;&#x4E8E;&#x5F7C;&#x6B64;&#x7684;&#x4E0D;&#x540C;&#x6216;&#x8005;&#x76F8;&#x4F3C;&#xFF0C;
    &#x6709;&#x8FC7;&#x4E0D;&#x5C11;&#x5E73;&#x6DE1;&#x65E0;&#x5947;&#x5374;&#x503C;&#x5F97;&#x7EAA;&#x5FF5;&#x7684;&#x65F6;&#x5149;&#xFF0C;
    &#x4EFB;&#x767D;&#x4E91;&#x82CD;&#x72D7;&#xFF0C;
    &#x98CE;&#x4E91;&#x53D8;&#x5E7B;&#x3002;

                --&#x7535;&#x5F71;&#x300A;&#x89E6;&#x4E0D;&#x53EF;&#x53CA;&#x300B;
```

> 第一个参数是一个updater函数；第二个参数是个回调函数（可选）

示例如下：

```
this.setState((prevState, props) => {
  <span class="hljs-built_in">return</span> {counter: prevState.counter + props.step};
});
```

updater这个函数格式是固定的，这个函数用上一个 state 作为第一个参数，将此次更新被应用时的 props 做为第二个参数(可选)，这两个对象setState会自动传递到函数中去

修改开关状态变量经常会用updater来改变状态，例如：

```
this.setState(prevState => ({
  isToggleOn: !prevState.isToggleOn
}));
```

> 第一个参数是一个对象；第二个参数同上也是回调函数（可选）

示例如下：

```
this.setState({number: 2})
```

这种方式是我们最常用到的修改state的方式.

> setState()将需要处理的变化塞入组件的state对象中， 并告诉该组件及其子组件需要用更新的状态来重新渲染。这是用于响应事件处理和服务端响应的更新用户界面的主要方式。

将setState()认为是一次请求而不是一次立即执行更新组件的命令。为了更为可观的性能，React可能会推迟它，稍后会一次性更新这些组件。React不会保证在setState之后，能够立刻拿到改变的结果。

setState()不是立刻更新组件。其可能是批处理或推迟更新。这使得在调用setState()后立刻读取this.state的一个潜在陷阱。代替地，使用 **componentDidUpdate** 或一个 **setState回调（setState(updater, callback)）**，当中的每个方法都会保证在更新被应用之后触发。若你需要基于之前的状态来设置状态，阅读下面关于updater参数的介绍。

下面说一下对state的赋值，分为三种情况：

适用的类型有：数字，字符串，布尔值，null， undefined。示例如下：

```
this.setState({
  count: 1,
  title: <span class="hljs-string">'React'</span>,
  success: <span class="hljs-literal">true</span>
})
```

如有一个数组类型的状态books，当向books中增加一本书时，使用数组的concat方法或ES6的数组扩展语法（spread syntax）：

```
// &#x65B9;&#x6CD5;&#x4E00;&#xFF1A;&#x5C06;state&#x5148;&#x8D4B;&#x503C;&#x7ED9;&#x53E6;&#x5916;&#x7684;&#x53D8;&#x91CF;&#xFF0C;&#x7136;&#x540E;&#x4F7F;&#x7528;concat&#x521B;&#x5EFA;&#x65B0;&#x6570;&#x7EC4;
var books = this.state.books;
this.setState({
  books: books.concat([<span class="hljs-string">'&#x4E09;&#x4F53;'</span>]);
})
// &#x65B9;&#x6CD5;&#x4E8C;&#xFF1A;&#x4F7F;&#x7528;preState&#x3001;concat&#x521B;&#x5EFA;&#x65B0;&#x6570;&#x7EC4;
this.setState(preState => ({
  books: preState.books.concat([<span class="hljs-string">'&#x4E09;&#x4F53;'</span>]);
}))
// &#x65B9;&#x6CD5;&#x4E09;&#xFF1A;ES6&#x6570;&#x7EC4;&#x6269;&#x5C55; spread syntax
this.setState(preState => ({
  books: [...preState.books, <span class="hljs-string">'&#x4E09;&#x4F53;'</span>];
}))
```

当从books中截取部分元素作为新状态时，使用数组的slice方法：（利用splice返回的数组也是同理）

```
// &#x65B9;&#x6CD5;&#x4E00;&#xFF1A;&#x5C06;state&#x5148;&#x8D4B;&#x503C;&#x7ED9;&#x53E6;&#x5916;&#x7684;&#x53D8;&#x91CF;&#xFF0C;&#x7136;&#x540E;&#x4F7F;&#x7528;slice&#x521B;&#x5EFA;&#x65B0;&#x6570;&#x7EC4;
var books = this.state.books;
this.setState({
  books: books.slice(1,3);
})
// &#x65B9;&#x6CD5;&#x4E8C;&#xFF1A;&#x4F7F;&#x7528;preState&#x3001;slice&#x521B;&#x5EFA;&#x65B0;&#x6570;&#x7EC4;
this.setState(preState => ({
  books: preState.books.slice(1,3);
}))
```

当从books中过滤部分元素后，作为新状态时，使用数组的filter方法：

```
// &#x65B9;&#x6CD5;&#x4E00;&#xFF1A;&#x5C06;state&#x5148;&#x8D4B;&#x503C;&#x7ED9;&#x53E6;&#x5916;&#x7684;&#x53D8;&#x91CF;&#xFF0C;&#x7136;&#x540E;&#x4F7F;&#x7528;filter&#x521B;&#x5EFA;&#x65B0;&#x6570;&#x7EC4;
var books = this.state.books;
this.setState({
  books: books.filter(item => {
    <span class="hljs-built_in">return</span> item != <span class="hljs-string">'&#x4E09;&#x4F53;'</span>;
  });
})
// &#x65B9;&#x6CD5;&#x4E8C;&#xFF1A;&#x4F7F;&#x7528;preState&#x3001;filter&#x521B;&#x5EFA;&#x65B0;&#x6570;&#x7EC4;
this.setState(preState => ({
  books: preState.books.filter(item => {
    <span class="hljs-built_in">return</span> item != <span class="hljs-string">'&#x4E09;&#x4F53;'</span>;
  });
}))
```

> 注意不要使用push、pop、shift、unshift等方法修改数组类型的状态，因为这些方法都是在原数组的基础上修改，而concat、slice、splice、filter能返回一个新的数组。

使用ES6 的Object.assgin方法

```
// &#x65B9;&#x6CD5;&#x4E00;&#xFF1A;&#x5C06;state&#x5148;&#x8D4B;&#x503C;&#x7ED9;&#x53E6;&#x5916;&#x7684;&#x53D8;&#x91CF;&#xFF0C;&#x7136;&#x540E;&#x4F7F;&#x7528;Object.assign&#x521B;&#x5EFA;&#x65B0;&#x5BF9;&#x8C61;
var owner = this.state.owner;
this.setState({
  owner: Object.assign({}, owner, {name: <span class="hljs-string">'Jason'</span>});
})
// &#x65B9;&#x6CD5;&#x4E8C;&#xFF1A;&#x4F7F;&#x7528;preState&#x3001;Object.assign&#x521B;&#x5EFA;&#x65B0;&#x5BF9;&#x8C61;
this.setState(preState => ({
  owner: Object.assign({}, preState.owner, {name: <span class="hljs-string">'Jason'</span>});
}))
```

使用对象扩展语法(object spread properties)

```
// &#x65B9;&#x6CD5;&#x4E00;&#xFF1A;&#x5C06;state&#x5148;&#x8D4B;&#x503C;&#x7ED9;&#x53E6;&#x5916;&#x7684;&#x53D8;&#x91CF;&#xFF0C;&#x7136;&#x540E;&#x4F7F;&#x7528;&#x5BF9;&#x8C61;&#x6269;&#x5C55;&#x8BED;&#x6CD5;&#x521B;&#x5EFA;&#x65B0;&#x5BF9;&#x8C61;
var owner = this.state.owner;
this.setState({
  owner: {...owner, name: <span class="hljs-string">'Jason'</span>};
})
// &#x65B9;&#x6CD5;&#x4E8C;&#xFF1A;&#x4F7F;&#x7528;preState&#x3001;&#x5BF9;&#x8C61;&#x6269;&#x5C55;&#x8BED;&#x6CD5;&#x521B;&#x5EFA;&#x65B0;&#x5BF9;&#x8C61;
this.setState(preState => ({
  owner: {...preState.owner, name: <span class="hljs-string">'Jason'</span>};
}))
```

每次setState产生新的state会依次被存入一个队列，然后会根据isBathingUpdates变量判断是直接更新this.state还是放进dirtyComponent里回头再说。 isBatchingUpdates默认是false，也就表示setState会同步更新this.state。 但是，当React在调用事件处理函数之前就会调用batchedUpdates，这个函数会把isBatchingUpdates修改为true，造成的后果就是由React控制的事件处理过程setState不会同步更新this.state。
