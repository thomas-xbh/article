---
link: https://juejin.cn/post/6858508463274885134
title: React-Redux、Recoil、useContext 做个简单的对比setState是同步还是异步React 组件性能优化最佳实践useRef与forwardRef的使用React项目问题总结(持续总结更新ing...)
description: 很多使用现代 React 的前端开发者，时常遇到组件渲染两次的情况。这害得他们都快把自己薅秃了。 另外一些人注意到了这个行为，但是他们觉得这是 React 运行的原理。又有些人会在 React 官方 repository 上发起工单，把这当做一个 bug 上报。 所以开发者社区…
keywords: 掘金翻译计划
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2020-08-08T07:18:44.000Z
publisher: 稀土掘金
stats: paragraph=60 sentences=37, words=257
---
>

* 原文地址：[My React components render twice and drive me crazy](https://link.juejin.cn?target=https%3A%2F%2Fmariosfakiolas.com%2Fblog%2Fmy-react-components-render-twice-and-drive-me-crazy%2F "https://mariosfakiolas.com/blog/my-react-components-render-twice-and-drive-me-crazy/")
* 原文作者：[Marios Fakiolas](https://link.juejin.cn?target=https%3A%2F%2Fmariosfakiolas.com%2F "https://mariosfakiolas.com/")
* 译文出自：[掘金翻译计划](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fxitu%2Fgold-miner "https://github.com/xitu/gold-miner")
* 本文永久链接：[github.com/xitu/gold-m...](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fxitu%2Fgold-miner%2Fblob%2Fmaster%2Farticle%2F2020%2Fmy-react-components-render-twice-and-drive-me-crazy.md "https://github.com/xitu/gold-miner/blob/master/article/2020/my-react-components-render-twice-and-drive-me-crazy.md")
* 译者：[tanglie1993](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Ftanglie1993 "https://github.com/tanglie1993")
* 校对者：[racheldev](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fracheldev "https://github.com/racheldev"),[lhd951220](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Flhd951220 "https://github.com/lhd951220")

# 我的 React 组件会渲染两次，我快疯了

很多使用现代 React 的前端开发者，时常遇到组件渲染两次的情况。这害得他们都快把自己薅秃了。

另外一些人注意到了这个行为，但是他们觉得这是 `React` 运行的原理。又有些人会在 `React` 官方 repository 上发起工单，把这当做一个 bug 上报。

所以开发者社区中肯定对此存在着一些困惑。😬

这些事情发生的原因是 `React.StrictMode`。

我们来看一些真实的例子，复现一下这种情况，然后研究它为什么会发生吧。

## 函数组件的例子

我们可以从运行一个新的 `CRA` 安装命令开始：

```bash
npx create-react-app my-app && cd my-app
```

我们稍稍改动 `App.js` ，增加一个超级简单的 `console.log` 语句：

```jsx
function App() {
  console.log('I render 😁');

  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
      header>
    div>
  );
}
```

现在我们可以通过 `yarn start` 启动我们的应用，并且在浏览器中打开 `http://localhost:3000`:

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1e83802c0684464aabbfd12bc5594b38~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

嗯， `I render &#x1F601;` 语句只输出了一次，所以我们不能通过一个超级简单的函数组件实现渲染两次。

## 带状态的函数组件例子

如果我们使用一个 React hook，然后在函数组件中加入一些状态语句会发生什么？

```jsx
function App() {
  const [clicks, setClicks] = React.useState(0);

  console.log('I render 😁');

  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />

        <button onClick={() => setClicks(clicks => clicks + 1)}>
            Clicks: {clicks}
        button>
      header>
    div>
  );
}
```

我们再看一下浏览器：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7b4eae8e42b0421cae38249db0a96f1a~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

实现了！它首先渲染了两次，然后每当我们点击加入的按钮时，都渲染两次。

显然， `React.useState` 影响了组件在重复渲染方面的行为。

## 生产环境中带状态的函数组件例子

生产环境 bundle 又如何呢？为了检查这一点，我们需要首先构建自己的应用，然后在 3000 端口中通过 `serve` 之类的包使用它：

```bash
yarn build && npx serve build -l 3000
```

在浏览器中再次打开 `http://localhost:3000`：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/63524d579ad04b73b5d8d4d7f5c33d9c~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

哎呦！调试语句在开始时打印了一次，并且在我们每次点击按钮时打印一次。

如我们所见，渲染两次的行为在生产环境中肯定不能复现，尽管我们对于 `React.useState` 的用法是完全一致的。

## 为什么会发生这种事？

如上所述，原因是 `React.StrictMode`。如果我们在应用中检查我们之前使用 `CRA` 运行的文件，我们会发现，我们的 `<app></app>` 组件被它包裹：

```jsx
ReactDOM.render(
  <React.StrictMode>
    <App />
  React.StrictMode>,
  document.getElementById('root')
);
```

显然，重新渲染并不是一个 bug，或者和库的渲染机制有关的东西。正相反，它是 `React` 提供的一种调试机制 🤗。

## [#](/blog/my-react-components-render-twice-and-drive-me-crazy#what-is-reactstrictmode "/blog/my-react-components-render-twice-and-drive-me-crazy#what-is-reactstrictmode") 什么是 React.StrictMode?

`React.StrictMode` 是在 2018 年的 [16.3.0](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Ffacebook%2Freact%2Freleases%2Ftag%2Fv16.3.0 "https://github.com/facebook/react/releases/tag/v16.3.0") 版本中引入的组件。一开始，它只用在类组件中，而在 [16.8.0](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Ffacebook%2Freact%2Freleases%2Ftag%2Fv16.8.0 "https://github.com/facebook/react/releases/tag/v16.8.0") 中，它对 hook 同样适用。

就像在版本说明中提及的一样：

> React.StrictMode 是帮助应用适应异步渲染的组件

所以它应该用来帮助工程师避免常见的错误，并使他们的 `React` 应用抛弃过时的 API，从而逐步升级。

这些提示对于更好地调试是有帮助的，因为这个库正在向异步渲染时代迈进，所以大的改动时时发生。

很有用，对吧？

## 为什么会渲染两次呢?

我们从使用 `React.StrictMode` 中获得的好处之一是，它帮助我们检测到渲染期生命周期的预期之外的副作用。

这些生命周期有：

* `constructor`
* `componentWillMount` (或者 UNSAFE_componentWillMount)
* `componentWillReceiveProps` (或者 UNSAFE_componentWillReceiveProps)
* `componentWillUpdate` (或者 UNSAFE_componentWillUpdate)
* `getDerivedStateFromProps`
* `shouldComponentUpdate`
* `render`
* `setState` 更新函数 (第一个参数)

所有这些方法都被调用不止一次，所以避免副作用是十分重要的。如果我们无视这个原则，就有可能造成状态不一致问题或者内存泄漏。

`React.StrictMode` 不能马上检测到副作用，但是它可以通过故意调用一些关键函数两次，来帮助我们发现副作用。

这些函数有:

* 类组件 `constructor`、 `render` 以及 `shouldComponentUpdate` 方法
* 类组件静态 `getDerivedStateFromProps` 方法
* 方法组件的方法体
* 状态更新函数 (`setState` 的第一个参数)
* 传给 `useState`、 `useMemo`、或 `useReducer` 的函数

这个行为肯定对性能有一些影响，但我们不应该担心，因为它只在开发而不是生产环境中发生。

这就是我们只有在开发环境下使用带 React.useState 的组件函数，才可以成功复现渲染两次的原因。Cheers!!

如果你需要继续深入研究 React.StrictMode，你可以阅读 [官方文档](https://link.juejin.cn?target=https%3A%2F%2Freactjs.org%2Fdocs%2Fstrict-mode.html "https://reactjs.org/docs/strict-mode.html")。

> 如果发现译文存在错误或其他需要改进的地方，欢迎到 [掘金翻译计划](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fxitu%2Fgold-miner "https://github.com/xitu/gold-miner") 对译文进行修改并 PR，也可获得相应奖励积分。文章开头的 **本文永久链接** 即为本文在 GitHub 上的 MarkDown 链接。

> [掘金翻译计划](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fxitu%2Fgold-miner "https://github.com/xitu/gold-miner") 是一个翻译优质互联网技术文章的社区，文章来源为 [掘金](https://juejin.cn "https://juejin.cn") 上的英文分享文章。内容覆盖 [Android](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fxitu%2Fgold-miner%23android "https://github.com/xitu/gold-miner#android")、[iOS](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fxitu%2Fgold-miner%23ios "https://github.com/xitu/gold-miner#ios")、[前端](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fxitu%2Fgold-miner%23%25E5%2589%258D%25E7%25AB%25AF "https://github.com/xitu/gold-miner#%E5%89%8D%E7%AB%AF")、[后端](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fxitu%2Fgold-miner%23%25E5%2590%258E%25E7%25AB%25AF "https://github.com/xitu/gold-miner#%E5%90%8E%E7%AB%AF")、[区块链](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fxitu%2Fgold-miner%23%25E5%258C%25BA%25E5%259D%2597%25E9%2593%25BE "https://github.com/xitu/gold-miner#%E5%8C%BA%E5%9D%97%E9%93%BE")、[产品](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fxitu%2Fgold-miner%23%25E4%25BA%25A7%25E5%2593%2581 "https://github.com/xitu/gold-miner#%E4%BA%A7%E5%93%81")、[设计](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fxitu%2Fgold-miner%23%25E8%25AE%25BE%25E8%25AE%25A1 "https://github.com/xitu/gold-miner#%E8%AE%BE%E8%AE%A1")、[人工智能](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fxitu%2Fgold-miner%23%25E4%25BA%25BA%25E5%25B7%25A5%25E6%2599%25BA%25E8%2583%25BD "https://github.com/xitu/gold-miner#%E4%BA%BA%E5%B7%A5%E6%99%BA%E8%83%BD")等领域，想要查看更多优质译文请持续关注 [掘金翻译计划](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fxitu%2Fgold-miner "https://github.com/xitu/gold-miner")、[官方微博](https://link.juejin.cn?target=http%3A%2F%2Fweibo.com%2Fjuejinfanyi "http://weibo.com/juejinfanyi")、[知乎专栏](https://link.juejin.cn?target=https%3A%2F%2Fzhuanlan.zhihu.com%2Fjuejinfanyi "https://zhuanlan.zhihu.com/juejinfanyi")。

### 阿里巴巴本地生活招聘

团队需要多名前端开发工程师，校招/社招均可，base杭州或上海。 感兴趣的可以发送简历到 [tanglie.tl@koubei.com](https://link.juejin.cn?target=mailto%3Atanglie.tl%40koubei.com "mailto:tanglie.tl@koubei.com") 期待你的加入。
