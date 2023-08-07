---
link: https://juejin.cn/post/6891649726656020493
title: webpack 中如何自定义loader
description: webpack是一个打包模块化JavaScript的工具，它会从入口模块出发，识别出源码中的模块化导入语句，递归地找出入口文件的所有依赖，将入口和其所有的依赖打包到一个单独的文件中。什么是loader
keywords: Webpack
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2020-11-05T14:44:09.000Z
publisher: 稀土掘金
stats: paragraph=71 sentences=50, words=469
---
webpack 是一个打包模块化 JavaScript 的工具，它会从入口模块出发，识别出源码中的模块化导入语句，递归地找出入口文件的所有依赖，将入口和其所有的依赖打包到一个单独的文件中。

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/40b4e7e0708d4439a897d2992c1a1df4~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

## 什么是loader

webpack 是基于 node 的模块化打包工具，它默认只知道如何处理 JS 和 JSON 模块，对于其他格式的模块如 CSS、图片等，就不知道如何处理了。这时候我们就需要定义相应的 loader ，告诉webpack如何处理。loader 就相当于是一个翻译机，将源文件经过传化后输出能被webpack处理的新内容，并且一个文件还可以链式的经过多个翻译机翻译。

以处理 LESS 文件为例：

```java
module.exports = {
  module: {
    rules: [
      {

        test: /\.less/,

        use: [
          'style-loader',
          {
            loader:'css-loader',

            options:{
              minimize:true,
            }
          },
          'less-loader'],
      },
    ]
  },
};
```

1. less 源代码会先交给 less-loader 把 less 转换成 CSS
2. 然后把 less-loader 输出的 CSS 交给 css-loader 处理，找出 CSS 中依赖的资源、压缩 CSS 等
3. 最后将 css-loader 输出的内容交给 style-loader 处理，转换成 style 将 CSS 插入到HTML页面中

可以看出 less 的处理过程是有顺序的链式执行，先 less-loader ，然后是 css-loader，最后才是 style-loader。从上面的处理过程我们可以知道，loader 的执行顺序和书写顺序是相反的，即：最后一个 loader 最先执行，然后从右往左按顺序执行，最后才是第一个 loader 执行。

## loader 的职责

从 less 的转换过程我们可以看出，一个loader应该遵循如下的规则：

* 职责单一：每个loader 只做一件事
* 链式调用：第一个loader接收到的是源文件的内容，后续loader都是接收到的是上一个loader 返回的处理结果，webpack 会按顺序链式调用每个 loader
* 统一原则：遵循 Webpack 制定的设计规则和结构，输入与输出均为字符串，各个 Loader 完全独立，即插即用
* 模块化：保证 loader 是模块化的。loader 生成模块需要遵循和普通模块一样的设计原则
* 无状态：在多次模块的转化之间，我们不应该在 loader 中保留状态。每个 loader 运行时应该确保与其他编译好的模块保持独立，同样也应该与前几个 loader 对相同模块的编译结果保持独立

## 实现loader

一个loader就是一个Node.js 模块，这个模块需要导出一个函数，这个导出的函数的工作就是获得处理前的源内容，对源内容进行处理后，返回处理后的内容。

### loader "模板"

我们来看一个最简单的 loader：

```js
module.exports = function(source) {

    return source
}
```

注意：loader 中导出的函数不能是箭头函数，原因是 webpack 为 loader 提供的 API 都绑定在了 this 对象上

### loader 返回值

我们知道loader的原理就是将输入的源内容进行处理后返回，loader 有两种方式返回处理后的内容：

**方式一 return source**

这种方式返回的是源内容转换后的内容

```ini
module.exports = function (source) {
    // 处理 source ...

    const content = source.replace("hello", "哈哈")
    return content
}
```

**方式二 this.callback()**

这种方式可以返回除了处理内容以外的其它信息，this.callback 是 webpack 给 loader 注入的 API，方便 loader 和 webpack之间通信。

```js
module.exports = function (source) {

  const content = source.replace("hello", "哈哈");

  this.callback(null, content);

  return
};
```

this.callback 的详细用法如下：

```typescript
this.callback(

    err: Error | null,

    content: string | Buffer,

    sourceMap?: SourceMap,

    abstractSyntaxTree?: AST
);
```

### loader 接受参数

webpack 为 loader 提供了 `this.query` API 来获取在 webpack.config.js 中配置的options 对象。如下面的代码，在 loader 配置中配置了 options ，this.query 获取到的就是这个 options

```css
{
  test: /\.js$/,
  use: [
    "replace-loader",
    {
      loader: "replace-loader-async",
      options: {
        name: "loaderName",
      },
    },
  ],
},
```

**获取配置中的 options**

```js
module.exports = function (source) {

  return source.replace("hello", this.query.name);
};
```

### 同步与异步 loader

loader 有同步异步之分，上面介绍的 loader 都是同步的 loader ，因为它们的转换流程都是同步的，转换完成后再返回结果。但在某些场景下转换内容需要异步才能完成，例如需要通过网络请求才能得到结果，如果使用同步的方式，网络请求就会阻塞整个构建过程，导致构建变得十分缓慢。

当转换内容需要异步才能完成时，我们可以使用 webpack 为 loader 提供的 `this.async()` 将这个 loader 变成是一个异步 loader：

```js
module.exports = function (source) {

  const callback = this.async();

  setTimeout(() => {
    const content = source.replace("hello", "哈哈");

    callback(null, content);
  }, 3000);
};
```

### loader 的路径问题

当我们定义好 loader 之后，我们在使用 loader 时需要在配置中加上路径，以便webpack可以找到我们自定义的loader

```css
{
    test: /\.js$/,
  use: path.resolve(__dirname, "./myLoaders/replace-loader")
}
```

如上面的代码，我们每使用一个自定义的loader，都必须使用 path 模块来解析自定义loader的路径问题，这就会导致代码变得难以维护。那可不可以像引用第三方的loader一样，只写loader 名呢？我们可以使用 resolveLoader 来解决这个问题。

**ResolveLoader** 用于配置 webpack 如何寻找 loader，默认情况下只会去 node_modules 目录下寻找，为了让 webpack 去加载自定义的 loader，我们需要修改 resolveLoader.modules

比如我们自定义的loader 放在 ./myLoaders 目录下，则需要如下配置：

```java
module.exports = {
    resolveLoader: {

    modules: ["node_modules", "./myLoaders"],
  },
}
```

## loader 实战

下面，我们分别来实现mini版的 style-loader、css-loader、less-loader

### mini版 style-loader

style-loader 做的事情很简单，就是把序列化后的 css 内容放入 style 标签中，然后将 style 标签插入到 HTML 页面的 head 标签中。

```ini
module.exports = function(source) {
    return `const styleTag = document.createElement('style')
        styleTag.innerHTML = ${source}
        document.head.appendChild(styleTag)
    `
}
```

### mini版 css-loader

css-loader 做的事情也十分的简单，将 less-loader 转换后的 css 内容进行序列化

```js
module.exports = function(source) {
    return JSON.stringify(source);
}
```

### mini版 less-loader

less-loader 做的事情就是使用 less 模块，将 less 转换成 css

```js

const less = require('less');
module.exports = function(source) {
    less.render(source, (error, output) => {
    let { css } = output;
    this.callback(error, css)
  })
}
```
