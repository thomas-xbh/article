---
link: https://juejin.cn/post/6844903843151478791
title: React精髓！一篇全概括(急速)
description: 一个人并不是生来要给打败的，你尽可以把他消灭掉，可就是打不败他。 JSX中，可以使用花括号{}嵌入任意的JavaScript合法表达式，如：2 + 2、user.firstName、formatName(user)都是合法的。示例如： JSX本身也是一种表达式，所以它可以像其他…
keywords: React.js
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2019-05-13T13:57:53.000Z
publisher: 稀土掘金
stats: paragraph=217 sentences=203, words=1590
---
一个人并不是生来要给打败的，你尽可以把他消灭掉，可就是打不败他。

学和使用react有一年多了，最近想在梳理一下react基础知识,夯实基础，激流勇进~ 关于reacr-router,redux,redux-saga后续都会慢慢输出，希望各位看官老爷持续关注~~要是能给个赞鼓励一下就更 **赞**了~

> 提醒一下： 看完之后抓紧时间趁热打铁，redux,react-redux,redux-saga

## react基础知识速览

## 1、什么是JSX？

一个JSX语法的示例，如下所示

```ini
const element = Hello, world!

复制代码
```

这种语法形式，既不是HTML，也不是字符串，而是称之为JSX，是React里用来描述UI和样式的语法，JSX最终会被编译为合法的JS语句调用（编译器在遇到 `{`时采用JS语法进行解析，遇到 `<`就采用HTML规则进行解析）

## 2、嵌入表达式

JSX中，可以使用花括号 `{}`嵌入任意的JavaScript合法表达式，如： `2 + 2`、 `user.firstName`、 `formatName(user)`都是合法的。示例如：

```js
const user = {
    firstName: 'Zhang',
    lastName : 'Busong'
};

const elem = (
    <h1>Hello, {formatName(user)}h1>
);

ReactDOM.render(
    element,
    document.getElementById('app')
)
复制代码
```

## 3、JSX也是一种表达式

JSX本身也是一种表达式，所以它可以像其他表达式一样，用于给一个变量赋值、作为函数实参、作为函数返回值，等等。如：

```js
function getGreeting(user) {
    if (user) {
        return <h1>Hello, {formatName(user)}h1>
    }
    return <h1>Hello, Guest!h1>;
}
复制代码
```

**注意：** 1、在JSX中，声明属性时不要使用引号，如果声明属性的时候使用引号，那么将被作为字符串解析，而不会被作为一个表达式解析，如：

```xml
<div firstName="{user.firstName}" lastName={user.lastName}>div>
复制代码
```

解析后，可以得到：

```ini
firstName="{user.firstName}" lastName="Lau">
复制代码
```

因此，当我们需要使用一个字符串字面量的时候，可以使用引号，但是如果要作为表达式解析的时候，则不应当使用引号 2、在JSX中，有些属性名称需要进行特殊处理。如 `class`应该用 `className`代替， `tabindex`则用 `tabIndex`代替。这是因为JSX本质上更接近于JavaScript，而 `class`是JavaScript中的保留字。同时，应该使用 `camelCase`来命名一个属性，而不是使用HTML的属性命名方式 3、JSX本身已经做了防注入处理，对于那些不是明确编写的HTML代码，是不会被解析为HTML DOM的，ReactDOM会将他们一律视为字符串，在渲染完成前就转化为字符串，所以可以防止XSS攻击 4、如果JSX标签是闭合的，那么结尾需要用 `/>`，另外，JSX标签是可以互相嵌套的，这和HTML里是一样的

## 4、JSX实质

JSX通过babel编译，而babel实际上把JSX编译给 `React.createElement()`调用。如下JSX代码：

```ini
const element = (
    className="greeting">
        Hello, world!

)
复制代码
```

是等同于以下的语句的：

```php
const elem = React.createElement(
    'h1',
    {className: 'greeting'},
    'Hello, world!'
);
复制代码
```

`React.createElement()`方法会首先进行一些避免BUG的检查，然后返回类似以下例子的对象：

```css
const element = {
    type: 'h1',
    props: {
        className: 'greeting',
        children: 'Hello, world'
    }
}
复制代码
```

这样的对象，则称为 `React&#x5143;&#x7D20;`，代表所有呈现在屏幕上的东西。React正是通过读取这些对象来构建DOM，并且保持数据和UI同步的

## 5、元素渲染

元素（ `elements`）是构成React应用的最小单元，元素描述了想要在屏幕中看到的内容，如：

```ini
const element = Hello, world
复制代码
```

和DOM元素不同的是，React元素是纯对象，创建的代价低。并且React会进行优化处理，只把有必要的变化更新到DOM上。此外，元素和组件的概念，是不一样的，组件是由元素组成的。

## 6、将元素渲染进DOM

在React中，使用 `ReactDOM.render()`方法来将React元素渲染进一个DOM中。如：

```js
ReactDOM.render(
    element,
    document.getElementById('root')
)
复制代码
```

React元素是不可变的，所以一旦一个元素创建完成后，我们是无法改变其内容或者属性的。一个元素就像是动画里的一帧，它代表UI在某一时间点的样子。如果非要使用元素来构成可变化的UI界面，就需要使用 `setInterval`了，如：

```js
function tick() {
    const element = (
        <div>Now is {new Date().toLocaleTimeString()}div>
    );
    ReactDOM.render(
        element,
        document.getElementById('root')
    );
}
setInterval(tick, 1000);
复制代码
```

在实际开发中，大多数React应用只会调用一次 `ReactDOM.render()`，所以更好的方式是使用 `&#x6709;&#x72B6;&#x6001;&#x7EC4;&#x4EF6;`

## 7、组件和Props

组件（ `component`）能够将UI划分为独立的、可复用的部分，这样我们就只需专注于构建每一个单独的部件。 从概念上看，组件就像是函数：接受任意的输入（称为属性， `Props`），返回React元素。React中有两种定义组件的方式： `&#x51FD;&#x6570;&#x5B9A;&#x4E49;`和 `&#x7C7B;&#x5B9A;&#x4E49;`

这种方式是最简单的定义组件的方式，就像写一个JS函数一样，如：

```js
function Welcome (props) {
    return <h1>Hello, {props.name}h1>;;
}
复制代码
```

还可以使用ES6里的类来定义一个组件，如下所示：

```scala
class Welcome extends React.Component {
    render () {
        return Hello, {this.props.name};
    }
}
复制代码
```

这种方式比起 `&#x51FD;&#x6570;&#x5B9A;&#x4E49;`方式则更加灵活

先前，我们遇到的React元素只是呈现一个DOM标签，如：

```ini
const element =
复制代码
```

然而，React元素也可以是用户自定义的 `&#x7EC4;&#x4EF6;`，如：

```ini
const element = "Tom" />
复制代码
```

Welcome组件中声明了一个属性 `name="Tom"`，而这个属性，将以 `props.name`的方式传递给组件，如下方式：

```js
function Welcome (props) {
    return <h1>Hello, {props.name}h1>;
}
复制代码
```

此时，对于以下的代码：

```js
ReactDOM.render(
    <Welcome name="张不怂" />,
    document.getElementById('root')
)
复制代码
```

最终就会以 `<h1>Hello, &#x5F20;&#x4E0D;&#x6002;</h1>`的方式呈现。在这个过程中，发生了如下的事情：

* 对 `<welcome name="&#x5F20;&#x4E0D;&#x6002;"></welcome>`元素调用了 `ReactDOM.render()`丰富
* React将 `{ name: '&#x5F20;&#x4E0D;&#x6002;' }`作为props实参来调用Welcome组件
* Welcome完成渲染，返回 `<h1>Hello, &#x5F20;&#x4E0D;&#x6002;</h1>`元素
* ReactDOM计算最小更新代价，然后更新DOM

组件是可以组合的。即组件内部可以引用其他组件，如：

```js
function Welcome (props) {
    return <h1>Hello, {props.name}h1>;
}

function App () {
    return (
        <div>
            <Welcome name="Tom" />
            <Welcome name="Jack" />
            <Welcome name="Mike" />
        div>
    )
}

ReactDOM.render(
    <App />,
    document.getElementById('root')
)
复制代码
```

**注意：** 在React中，组件必须返回 `&#x5355;&#x4E00;`的根元素，这也是为什么App组件中需要用 标签包裹的原因。如以下的方式，是错误的（因为它有3个根元素）：

```ini
function App () {
    return (
        name="Tom" />
        name="Jack" />
        name="Mike" />
    )
}
复制代码
```

考虑以下这种情况：

```css
function sum (a, b) {
    return a + b;
}
复制代码
```

这种函数称为 `&#x7EAF;&#x51FD;&#x6570;`：它不改变自己的输入值，且总是对相同的输入返回相同的结果。 与之对立的，则是 `&#x975E;&#x7EAF;&#x51FD;&#x6570;`，如：

```ini
function withdraw (account, amount) {
    account.total -= amount
}
复制代码
```

`&#x975E;&#x7EAF;&#x51FD;&#x6570;`在函数内改变了输入的参数。在React中，无论是通过 `function`还是 `class`声明组件，我们都不应该修改它自身的属性（ `props`）。虽然React相当灵活，但是它也有一个严格的规定： `&#x6240;&#x6709;&#x7684;React&#x7EC4;&#x4EF6;&#x90FD;&#x5FC5;&#x987B;&#x50CF;&#x7EAF;&#x51FD;&#x6570;&#x90A3;&#x6837;&#x6765;&#x4F7F;&#x7528;&#x5B83;&#x4EEC;&#x7684;props`

## 8、State与生命周期

使用 `&#x7C7B;&#x5B9A;&#x4E49;&#x7EC4;&#x4EF6;`有一些额外的好处，如拥有 `&#x672C;&#x5730;&#x72B6;&#x6001;`这一特性。 以下是一个 `&#x7C7B;&#x5B9A;&#x4E49;&#x7EC4;&#x4EF6;`

```scala
class Clock extends React.Component {
    render () {
        return (

                Hello, world!

                Now is {this.props.date.toLocaleTimeString()}

        );
    }
}
复制代码
```

需要注意的有：

* `&#x7C7B;&#x540D;`即为 `&#x7EC4;&#x4EF6;&#x540D;`（无论是函数定义组件还是类定义组件，组件名称的首字母都必须大写，并且继承自 `React.Component`）
* 使用 `render()` 方法，用来返回需要呈现的内容

state是属于一个组件自身的。我们可以在类的构造函数 `constructor`中来初始化状态，如：

```js
constructor (props) {
    super(props)
    this.state = {
        date: new Date()
    }
}
复制代码
```

如此一来，我们就可以在 `render()`函数中使用 `this.state.xxx`来引用一个状态

在应用里，往往都会有许许多多的组件。在组件销毁后，回收和释放它们所占据的资源非常重要。 在 `&#x65F6;&#x949F;&#x5E94;&#x7528;`的例子里，我们需要在第一次渲染到DOM的时候设置一个定时器，并且需要在相应的DOM销毁后，清除这个定时器。那么，这种情况下，React为我们提供了生命周期的钩子函数，方便我们进行使用。在React中，生命周期分为： 1） `Mount` 已插入真实DOM 2） `Update` 正在重新渲染 3） `Unmount` 已移出真实DOM 而相应的，生命周期钩子函数有：

* `componentWillMount`
* `componentDidMount`
* `componentWillUpdate(newProps, nextState)`
* `componentDidUpdate(prevProps, prevState)`
* `componentWillUnmount()`

此外，还有两种特殊状态的处理函数：

* `componentWillReceiveProps(nextProps)` 已加载的组件收到新的参数时调动
* `shouldComponentUpdate(nextProps, nextState)` 组件判断是否重新渲染时调用

因此，基于生命周期钩子函数，我们可以实现一个时钟应用如下：

```scala
class Clock extends React.Component {
    constructor (props) {
        super(props);
        this.state = {
            date: new Date()
        }
    }
    tick () {
        this.setState({
            date: new Date()
        });
    }
    componentDidMount () {
        this.timerId = setInterval(() => {
            this.tick()
        }, 1000);
    }
    componentWillUnmount () {
        clearInterval(this.timerId);
    }
    render () {
        return (
            Now is {this.state.date.toLocaleTimeString()}
        );
    }
}
复制代码
```

需要注意的是： 1） `render()`里用不到的 `state`，不应该声明在 `state`里 2）不能直接使用 `this.state.xxx = xxx`的方式来改变一个 `state`的值，应该使用 `this.setState()`。如：

```kotlin
setName () {
    this.setState({
        name: '张不怂'
    })
}
复制代码
```

`this.setState()`会自动覆盖 `this.state`里相应的属性，并触发 `render()`重新渲染。 3） **状态更新可能是异步的** React可以将多个 `setState()`调用合并成一个调用来提升性能。且由于 `this.props`和 `this.state`可能是异步更新的，所以不应该依靠它们的值来计算下一个状态。这种情况下，可以给 `setState`传入一个函数，如：

```js
this.setState((prevState, props) => ({
    counter: prevState.counter + props.increment
}));
复制代码
```

## 9、事件处理

React元素的事件与DOM元素类似，不过也有一些区别，如： 1）React事件使用 `camelCase`命名（ `onClick`），而不是全小写的形式（ `onclick`） 2）使用JSX，传入的是事件的句柄，而不是一个字符串 如以下的HTML：

```ini
onclick="increment()">ADD
复制代码
```

使用React的方式描述如：

```xml
<button onClick={increment}>ADDbutton>
复制代码
```

还有一个不同在于，在原生DOM中，我们可以通过返回 `false`来阻止默认行为，但是这在React中是行不通的，在React中需要明确使用 `preventDefault()`来阻止默认行为。如：

```js
function ActionLink () {
    function handleClick (e) {
        e.preventDefault();
        alert('Hello, world!');
    }

    return (
        <a href="#" onClick={handleClick}>Click Mea>
    );
}
复制代码
```

这里，事件回调函数里的 `event`是经过React特殊处理过的（遵循W3C标准），所以我们可以放心地使用它，而不用担心跨浏览器的兼容性问题。 **注意：** 在使用事件回调函数的时候，我们需要特别注意 `this`的指向问题，因为在React里， **除了构造函数和生命周期钩子函数里会自动绑定this为当前组件外，其他的都不会自动绑定 `this` 的指向为当前组件**，因此需要我们自己注意好this的绑定问题， 通常而言，在一个类方式声明的组件里使用事件回调，我们需 **要在组件的 `constructor` 里绑定回调方法的this指向**，如：

```kotlin
class Counter extends React.Component {
    constructor (props) {
        super(props);
        this.state = {
            counter: 0
        }

        this.increment = this.increment.bind(this);
    }
    increment () {
        this.setState({
            counter: this.state.counter + 1
        });
    }
    render () {
        return (

                The counter now is: {this.state.counter}
                this.increment}>+1

        );
    }
}
复制代码
```

当然，我们还有另外一种方法来使用 **箭头函数**绑定指向，就是使用 `&#x5B9E;&#x9A8C;&#x6027;`的属性初始化语法，如：

```scala
class Counter extends React.Component {
    increment: () => {
        this.setState({
            counter: this.state.counter + 1
        });
    }

}

复制代码
```

3）像事件处理程序传递参数 我们可以为事件处理程序传递额外的参数，方式有以下两种：

```js
(e) => this.deleteRow(id, e)}>Delete Row
<button onClick={this.deleteRow.bind(this, id)}>Delete Rowbutton>

复制代码
```

需要注意的是，使用箭头函数的情况下，参数 `e`要显式传递，而使用bind的情况下，则无需显式传递（参数 `e`会作为最后一个参数传递给事件处理程序）

## 10、条件渲染

在React里，我们可以创建不同的组件来封装我们需要的功能。我们也可以根据组件的状态，只渲染组件中的一部分内容，而条件渲染就是为此而准备的。在React中，我们可以像在JavaScript中写条件语句一样地写条件渲染语句，如：

```js
function Greet(props) {
    const isLogined = props.isLogined;
    if (isLogined) {
        return <div>Hello !div>;
    }
    return <div>Please sign indiv>;
}

ReactDOM.render(
    <Greet isLogined={true} />,
    document.getElementById('root')
);

复制代码
```

这将渲染出：

```css
<div>Hello !div>

复制代码
```

我们也可以使用变量来存储元素，如：

```js
function LogBtn(props) {
    var button;
    const isLogined = props.isLogined;
    if (isLogined) {
        button = <button>退出button>
    } else {
        button = <button>登陆button>
    }
    return <div>You can {button}div>;
}

ReactDOM.render(
    <LogBtn isLogined={false} />,
    document.getElementById('root')
);

复制代码
```

由于JavaScript语法对待 `&&`运算符的性质，我们也可以使用&&运算符来完成条件渲染，如：

```js
function LogBtn(props) {
    var button;
    const isLogined = props.isLogined;
    return (
        <div>Hello
        {!isLogined && (
            <button>请登陆button>
        )}
        div>
    )
}

复制代码
```

当 `props.isLogined`为false的时候，就会渲染出：

```css
<div>Hello <button>请登录button>div>

复制代码
```

我们可能已经发现了，其实JSX可以像一个表达式那样子灵活使用，所以，我们自然也可以使用三目运算符进行渲染，如：

```js
function LogBtn (props) {
    const isLogined = props.isLogined;
    return (
        <div>You can
            <button>{isLogined ? '退出' : '登陆'}button>
        div>
    )
}

复制代码
```

有时候，我们希望是整个组件都不渲染，而不仅仅是局部不渲染，那么这种情况下，我们就可以在 `render()`函数里返回一个 `null`，来实现我们想要的效果，如：

```js
function LogBtn (props) {
    const isLogined = props.isLogined;
    const isShow = props.isShow;
    if (isShow) {
        return (
            <div>You can
                <button>{isLogined ? '退出' : '登陆'}button>
            div>
        )
    }
    return null;
}

复制代码
```

**注意：** 组件里返回 `null`不会影响组件生命周期的触发，如 `componentWillUpdate`和 `componentDidUpdate`仍然会被调用

## 11、列表渲染与keys

在JavaScript中，我们可以使用 `map()`函数来对一个数组列表进行操作，如：

```ini
const numbers = [1, 2, 3, 4, 5]
const doubled = numbers.map(number => number*2)
console.log(doubled)

复制代码
```

同样的，在React里，我们也可以使用 `map()`来进行列表渲染，如：

```js
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map(number => {
    return (
        <li>{number}li>
    )
});

ReactDOM.render(
    <ul>{listItems}ul>,
    document.getElementById('root')
)

复制代码
```

这将得到：

```css
<ul><li>1li>
    <li>2li>
    <li>3li>
    <li>4li>
    <li>5li>
ul>

复制代码
```

当然，我们还可以进行更好的封装，如：

```js
function NumberList (props) {
    const numbers = props.numbers;
    const listItems = numbers.map(number => {
        return (
            <li>{number}li>
        )
    });

    return <ul>{listItems}ul>
}

复制代码
```

当我们运行以上的代码的时候，会发现控制台提示： `Each child in an array or iterator should have a unique "key" prop`，因此，我们需要为列表项的每一个项分配一个 `key`，来解决这个问题，通常而言，我们可以使用以下几种方式来提供 `key`：

* 使用数据项自身的ID，如
* 使用索引下标（ `index`），如：

```typescript
const listItems = numbers.map((number, index) => {
    <li key={index}>{number}li>
});

复制代码
```

但是React不推荐在需要重新排序的列表里使用索引下标，因为会导致变得很慢。

**注意：** 只有在一个项的同胞里区分彼此的时候，才需要使用到key，key不需要全局唯一，只需要在一个数组内部区分彼此时唯一便可。key的作用是给React一个提示，而不会传递给组件。如果我们在组件内需要同样的一个值，可以换个名字传递，如：

```js
const content = posts.map(post => (
    <Post key={post.id} id={post.id} title={post.title} />
));

复制代码
```

## 12、表单

表单和其他的React中的DOM元素有所不同，因为表单元素生来就是为了保存一些内部状态。在React中，表单和HTML中的表单略有不同

HTML中， `<input>`、 `<textarea></textarea>`、 `<select></select>`这类表单元素会维持自身状态，并根据用户输入进行更新。不过React中，可变的状态通常保存在组件的 `this.state`中，且只能用 `setState()`方法进行更新，如：

```kotlin
class NameForm extends React.Component {
    constructor (props) {
        super(props);
        this.state = {
            value: ''
        }
        this.handleChange = this.handleChange.bind(this);
        this.handleSubmit = this.handleSubmit.bind(this);
    }
    handleChange (event) {
        this.setState({
            value: event.target.value
        });
    }
    handleSubmit (event) {
        alert('Your name is '+this.state.value);
        event.preventDefault();
    }
    render () {
        return (
            this.handleSubmit}>
            Name: "text" value={this.state.value} onChange={this.handleChange} />
            "submit" value="Submit" />

        )
    }
}

复制代码
```

和HTML中不同的是，React中的 `textarea`并不需要写成 `<textarea></textarea>`的形式，而是写成 `<textarea value ...></textarea>`的形式便可。而对于HTML中的 `select`标签，通常做法是：

```vbnet
<select>
    <option value="A">Aoption>
    <option value="B" selected>Boption>
    <option value="C">Coption>
select>

复制代码
```

但是React中，不需要在需要选中的 `option`处加入 `selected`，而只需要传入一个value，就会自动根据value来选中相应的选项，如：

```ini
value="C">
    value="A">A
    value="B">B
    value="C">C

复制代码
```

那么如上述例子，C所在的这个 `option`就会被选中

通常一个表单都有多个输入，如果我们为每一个输入添加处理事件，那么将会非常繁琐。好的一个解决办法是，使用name，然后根据 `event.target.name`来选择做什么。如：

```kotlin
class Form extends React.Component {
    constructor (props) {
        super(props);
        this.state = {
            name: '',
            gender: '男',
            attend: false,
            profile: ''
        };
        this.handleInputChange = this.handleInputChange.bind(this);
        this.handleSubmit = this.handleSubmit.bind(this);
    }
    handleInputChange (event) {
        const target = event.target;
        const value = target.type==='checkbox' ? target.checked : target.value;
        const name = target.name;
        this.setState({
            [name]: value
        });
    }
    handleSubmit (event) {
        this.setState({
            profile: `姓名：${this.state.name}，${this.state.gender}，${this.state.attend ? '参加' : '不参加'}活动`
        });
        event.preventDefault();
    }
    render () {
        return (

            姓名："name" value={this.state.name} onChange={this.handleInputChange} />
            性别：
                "gender" value={this.state.gender} onChange={this.handleInputChange}>
                    "男">男
                    "女">女

            是否参加："attend" type="checkbox" onChange={this.handleInputChange} checked={this.state.attend} />
            "submit" value="Submit" onClick={this.handleSubmit} />
            您的报名信息：{this.state.profile}

        )
    }
}

复制代码
```

大多数情况下，使用 `&#x53D7;&#x63A7;&#x7EC4;&#x4EF6;`实现表单是首选，在受控组件中，表单数据是交由React组件处理的。如果想要让表单数据由DOM处理（即数据不保存在React的状态里，而是保存在DOM中），那么可以使用 `&#x975E;&#x53D7;&#x63A7;&#x7EC4;&#x4EF6;`，使用 `&#x975E;&#x53D7;&#x63A7;&#x7EC4;&#x4EF6;`，可以无需为每个状态更新编写事件处理程序，使用 `ref`即可实现，如：

```js
class NameForm extends React.Component {
    constrcutor(props) {
        super(props)
    }
    handleSubmit(event) {
        console.log('A name was submitted: ', this.input.value)
        event.preventDefault()
    }
    render() {
        return (
            <form onSubmit={this.handleSubmit}>
                <label>
                Name: <input type="text" ref={input => this.input = input} />
                label>
                <input type="submit" value="submit" />
            form>
        )
    }
}

复制代码
```

对于 `&#x975E;&#x53D7;&#x63A7;&#x7EC4;&#x4EF6;`，如果要指定默认值，那么可以使用 `defaultValue`，如：

```lua
<input type="text" defaultValue="Hello" ref={input => this.input = input} />

复制代码
```

相应的， `type="checkbox"`和 `type="radio"`，则使用 `defaultChecked`

## 13、状态提升

当需要几个组件共用状态数据的时候，可以使用状态提升技术。核心思想在于：把数据抽离到最近的共同父组件，父组件管理状态（state），然后通过属性（props）传递给子组件。如实现一个货币转换的组件，可以如下：

```lua
function USD2RMB (amount) {
    return amount * 6.7925;
}

function RMB2USD (amount) {
    return amount * 0.1472;
}

function convert (amount, typeFn) {
    return typeFn(amount);
}

复制代码
```

我们希望在RMB的输入表单上上输入的时候，USD的输入表单上的数值也同步更新，这种情况下，如果RMB组件自己管理自己的状态，是很难以实现的，因此，我们需要让这个状态提升自父组件进行管理。如下：

```scala
class CurrencyInput extends React.Component {
    constructor (props) {
        super(props)
        this.handleChange = this.handleChange.bind(this)
    }
    handleChange (event) {
        this.props.onInputChange(event.target.value)
    }
    render () {
        const value = this.props.value
        const type = this.props.type
        return (
            {type}: type="text" value={value} onChange={this.handleChange} />
        );
    }
}

复制代码
```

最后定义一个共同的父组件，如下：

```kotlin
class CurrencyConvert extends Component {
    constructor (props) {
        super(props);
        this.state = {
            type: 'RMB',
            amount: 0
        }
        this.handleRMBChange = this.handleRMBChange.bind(this);
        this.handleUSDChange = this.handleUSDChange.bind(this);
    }
    handleRMBChange (amount) {
        this.setState({
            type: 'RMB',
            amount
        });
    }
    handleUSDChange (amount) {
        this.setState({
            type: 'USD',
            amount
        });
    }
    render () {
        const type = this.state.type;
        const amount = this.state.amount;
        const RMB = type==='RMB' ? amount : convert(amount, USB2RMB);
        const USD = type==='USD' ? amount : convert(amount, RMB2USB);
        return (

                Please Input:
                "RMB" value={RMB} onInputChange={this.handleRMBChange} />
                "USD" value={USD} onInputChange={this.handleUSDChange} />

        );
    }
}

复制代码
```

## 14、组合vs继承

React推崇更多的是使用组合，而非使用继承。对于一些使用场景，React给出的建议如下：

当父组件不知道子组件可能的内容是什么的时候，可以使用 `props.children`，如：

```js
function Article (props) {
    return (
        <section>
            <aside>侧边栏aside>
            <article>{props.children}article>
        section>
    );
}

function App () {
    return (
        <Article>这是一篇文章Article>
    );
}

复制代码
```

这将渲染得到：

```css
<section>
    <aside>侧边栏aside>
    <article>这是一篇文章article>
section>

复制代码
```

我们还可以自定义名称，因为JSX实际上会被转化为合法的JS表达式，所以，还可以有：

```js
function Article (props) {
    return (
        <section>
            <aside>{props.aside}aside>
            <article>{props.children}article>
        section>
    );
}

function App () {
    return (
        <Article aside={
            <h1>这是一个侧栏h1>
        }>这是一篇文章Article>
    );
}

复制代码
```

这将渲染得到：

```css
<section>
    <aside><h1>这是一个侧栏h1>aside>
    <article>这是一篇文章article>
section>

复制代码
```

在Facebook的网站上，使用了数以千计的组件，但是实践证明还没有发现需要使用继承才能解决的情况。 属性和组合为我们提供了清晰的、安全的方式来自定义组件的样式和行为，组件可以接受任意元素，包括：基本数据类型、React元素、函数。 如果要在组件之间复用UI无关的功能，那么应该将其提取到单独的JavaScript模块中，这样子可以在不对组件进行扩展的前提下导入并使用函数、对象、类

觉得对你有帮助，不妨点个

不妨再点个关注，不迷路，下一篇关于redux的明天就发！！~
