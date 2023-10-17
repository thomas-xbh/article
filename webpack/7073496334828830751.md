---
link: https://juejin.cn/post/7073496334828830751
title: React(Webpack5)配置LESS、SASS
description: React项目配置LESS、SASS。 Create React App ，Supported Less And Sass
keywords: React.js
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2022-03-10T15:46:43.000Z
publisher: 稀土掘金
stats: paragraph=27 sentences=25, words=151
---
# React(Webpack5)配置LESS、SASS

## 创建react-app

> [Getting Started | Create React App (create-react-app.dev)](https://link.juejin.cn?target=https%3A%2F%2Fcreate-react-app.dev%2Fdocs%2Fgetting-started%2F "https://create-react-app.dev/docs/getting-started/")

```shell
yarn create react-app my-app
cd my-app
ls
README.md  node_modules/  package.json  public/  src/  yarn.lock
yarn start
```

## 暴露webpack5

* 最新安装的是 `webpack5`

> [webpack 中文文档 (docschina.org)](https://link.juejin.cn?target=https%3A%2F%2Fwebpack.docschina.org%2Fconcepts%2F "https://webpack.docschina.org/concepts/")

```shell
yarn eject
ls
README.md  config/  node_modules/  package.json  public/  scripts/  src/  yarn.lock
```

## 配置LESS、SASS

* 安装依赖

```shell
// 安装处理css文件的loader
yarn add style-loader css-loader
// 安装处理less文件的loader
yarn add less-loader less
// 安装处理scss文件的loader
yarn add sass-loader node-sass
```

* 配置config文件下的 `webpack.config.js`

> `module > rules > oneOf` 下新增规则

```json
{

    test: /\.css$/,

    use: ['style-loader', 'css-loader'],
},
{
    test: /\.less$/,
    use: ['style-loader', 'css-loader', 'less-loader'],
},
{
    test: /\.scss$/,
    use: ['style-loader', 'css-loader', 'sass-loader'],
}
```

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/34bf8bbbc028449aa8284107181766ef~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

## 测试

* `src/test.scss`

```css
.less {
  font-size: 32px;
  h1 {
    color: #f40;
  }
}
```

* `src/test.scss`

```css
.scss {
  font-size: 32px;
  h1 {
    color: #ff0;
  }
}

```

* `src/App.js`

```js
import './test.less'
import './test.scss'
function App() {
  return (
    <div className="App">
      <div className='less'>
        <h1>LESSh1>
      div>
      <div className='scss'>
        <h1>SCSSh1>
      div>
    div>
  );
}

export default App;
```

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ba1f5184864347448ac35c76c7d7333e~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)
