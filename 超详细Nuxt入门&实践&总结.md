---
link: https://juejin.cn/post/7205047834793295931
title: 超详细Nuxt入门&实践&总结
description: Nuxt入门&实践总结 一、nuxt简介及安装 Nuxt.js 官方介绍： 1、nuxt简介 1)、那服务器端渲染的益处 nuxt.js简单的说是Vue.js的通用框架，最常用的就是用来作SSR（服务
keywords: 前端,Vue.js,Nuxt.js
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2023-02-28T06:20:41.000Z
publisher: 稀土掘金
stats: paragraph=191 sentences=371, words=2285
---
## 一、nuxt简介及安装

> Nuxt.js 是一个基于 Vue.js 的通用应用框架。通过对客户端/服务端基础架构的抽象组织，Nuxt.js 主要关注的是应用的 UI 渲染。我们的目标是创建一个灵活的应用框架，你可以基于它初始化新项目的基础结构代码，或者在已有 Node.js 项目中使用 Nuxt.js，Nuxt.js 预设了利用 Vue.js 开发服务端渲染的应用所需要的各种配置。作为框架，Nuxt.js 为 客户端/服务端 这种典型的应用架构模式提供了许多有用的特性，例如异步数据加载、中间件支持、布局支持等。

`nuxt.js`简单的说是 `Vue.js`的通用框架，最常用的就是用来作 `SSR&#xFF08;&#x670D;&#x52A1;&#x5668;&#x7AEF;&#x6E32;&#x67D3;&#xFF09;`。 `Vue.js`是开发 `SPA&#xFF08;&#x5355;&#x9875;&#x5E94;&#x7528;&#xFF09;`的， `Nuxt.js`这个框架，用 `Vue`开发多页应用，并在服务端完成渲染，可以直接用命令把我们制作的 `vue`项目生成为静态 `html`。

主要的原因时 `SPA&#xFF08;&#x5355;&#x9875;&#x5E94;&#x7528;&#xFF09;`不利于搜索引擎的 `SEO`操作， `Nuxt.js`适合作新闻、博客、电影、咨询这样的需要搜索引擎提供流量的项目。如果你要作移动端的项目，就没必要使用这个框架了。

* `SSR` 就是 服务器渲染，什么是 服务器渲染？
* 由 服务器 组装好 `DOM` 元素，生成 `HTML` 字符串给到浏览器，也就是在浏览器里面是可以看到整个页面的 `DOM` 源码的。

* `SEO`：搜索引擎的优先爬取级别是页面的 `HTML` 结构，当我们使用 `SSR` 的时候，服务端已经生成了与业务相关联的 `HTML`，这样的信息对于 `SEO` 是很友好的。
* 内容呈现：客户端无需等待所有的 `JS` 文件加载完成即可看见渲染的业务相关视图（压力来到了服务端这边，这也是需要做权衡的地方，需要区分哪些由服务端渲染，哪些可以交给客户端）。

* 代码兼容：对于开发人员来讲，需要去兼容代码在不同环境的运行 `Vue SSR` 所需要的服务端环境是 `Node`，有一些客户端的对象，比如 `dom`、 `windows` 之类的则无法使用。
* 服务器负载：相对于前后端分离模式下服务器只需要提供静态资源来说， `SSR` 需要的服务器负载更大，所以在项目中使用 `SSR` 模式要慎重，比如一整套图表页面，相对于服务端渲染，可能用户不会在乎初始加载的前几秒，可以交由客户端使用类似于骨架屏，或者懒加载之类的提升用户体验。

* 在网页上右键查看源码： `Vue SSR`与 原生 `HTML`是可以看到源码标签的
* 在认识 `SSR`之前，首先对 `CSR`与 `SSR`之间做个对比。
  - 首先看一下传统的 `web`开发，传统的 `web`开发是，客户端向服务端发送请求，服务端查询数据库，拼接 `HTML`字符串（模板），通过一系列的数据处理之后，把整理好的 `HTML`返回给客户端，浏览器相当于打开了一个页面。这种比如我们经常听说过的 `jsp`， `PHP`， `aspx`也就是传统的 `MVC`的开发。
  - `SPA`应用，到了 `Vue`、 `React`，单页面应用优秀的用户体验，逐渐成为了主流，页面整体是 `javaScript`渲染出来的，称之为客户端渲染 `CSR`。 `SPA`渲染过程。由客户端访问 `URL`发送请求到服务端，返回 `HTML`结构（但是 `SPA`的返回的 `HTML`结构是非常的小的，只有一个基本的结构，如第一段代码所示）。客户端接收到返回结果之后，在客户端开始渲染 `HTML`，渲染时执行对应 `javaScript`，最后渲染 `template`，渲染完成之后，再次向服务端发送数据请求，注意这里时数据请求，服务端返回 `json`格式数据。客户端接收数据，然后完成最终渲染。（请求两次，百度搜索引擎不能抓取 `SPA`页面的数据）
  - `SPA`虽然给服务器减轻了压力，但是也是有缺点的：
    + 首屏渲染时间比较长：必须等待 `JavaScript`加载完毕，并且执行完毕，才能渲染出首屏。
    + `SEO`不友好：爬虫只能拿到一个 `div`元素，认为页面是空的，不利于 `SEO`。 为了解决如上两个问题，出现了 `SSR`解决方案，后端渲染出首屏的 `DOM`结构返回，前端拿到内容带上首屏，后续的页面操作，再用单页面路由和渲染，称之为服务端渲染(`SSR`)。
* `SSR`渲染流程是这样的，客户端发送 `URL`请求到服务端，服务端读取对应的 `url`的模板信息，在服务端做出 `html`和数据的渲染，渲染完成之后返回 `html`结构，客户端这时拿到的之后首屏页面的 `html`结构。所以用户在浏览首屏的时候速度会很快，因为客户端不需要再次发送 `ajax`请求。并不是做了 `SSR`我们的页面就不属于 `SPA`应用了，它仍然是一个独立的 `spa`应用。
* `SSR`是处于 `CSR`与 `SPA`应用之间的一个折中的方案，在渲染首屏的时候在服务端做出了渲染，注意仅仅是首屏，其他页面还是需要在客户端渲染的，在服务端接收到请求之后并且渲染出首屏页面，会携带着剩余的路由信息预留给客户端去渲染其他路由的页面。

* 基于`Vue``
* 自动代码分层
* 服务端渲染
* 强大的路由功能，支持异步数据
* 静态文件服务
* `EcmaScript6`和 `EcmaScript7`的语法支持
* 打包和压缩 `JavaScript`和 `CSS`
* `HTML`头部标签管理
* 本地开发支持热加载
* 集成 `ESLint`
* 支持各种样式预编译器 `SASS`、 `LESS`等等
* 支持 `HTTP/2`推送

```less

npm install npx -g

npx create-nuxt-app 项目名

Project name
Project description
Use a custom server framework
Choose features to install
Use a custom UI framework
Use a custom test framework
Choose rendering mode
Universal
Single Page App
复制代码
```

当一个客户端请求进入的时候，服务端有通过 `nuxtServerInit`这个命令执行在 `Store`的 `action`中，在这里接收到客户端请求的时候，可以将一些客户端信息存储到 `Store`中，也就是说可以把在服务端存储的一些客户端的一些登录信息存储到 `Store`中。之后使用了中间件机制，中间件其实就是一个函数，会在每个路由执行之前去执行，在这里可以做很多事情，或者说可以理解为是路由器的拦截器的作用。然后再 `validate`执行的时候对客户端携带的参数进行校验，在 `asyncData`与 `fetch`进入正式的渲染周期， `asyncData`向服务端获取数据，把请求到的数据合并到 `Vue`中的 `data`中

```java
└─my-nuxt-demo
  ├─.nuxt
  ├─assets
  ├─components
  ├─layouts
  ├─middleware
  ├─node_modules
  ├─pages
  ├─plugins
  ├─static
  └─store
  ├─.editorconfig
  ├─.eslintrc.js
  ├─.gitignore
  ├─nuxt.config.js
  ├─package-lock.json
  ├─package.json
  ├─README.md
复制代码
```

注意：

* `export default`在一个模块中只能有一个，当然也可以没有。 `export`在一个模块中可以有多个。
* `export default`的对象、变量、函数、类，可以没有名字。 `export`的必须有名字。
* `export default`对应的 `import`和 `export`有所区别
* `module`变量代表当前模块。这个变量是一个对象， `module`对象会创建一个叫 `exports`的属性，这个属性的默认值是一个空的对象

`Node`为每个模块提供一个 `exports`变量，指向 `module.exports`，两个是相等的关系，但又不是绝对相当的关系， `module.exports`可以直接导出一个匿名函数或者一个值，但是 `export`的必须有名字，故不行， `export default`或 `export`名字可以

```ruby
const pkg = require('./package')
module.exports = {
  mode: 'universal',    // 当前渲染使用模式，分为universal和spa，既然是nuxt开发，那就是universal
  // 全局页头配置 (https://go.nuxtjs.dev/config-head)
  head: {       // 页面head配置信息
    title: pkg.name,        // title
    meta: [         // meat
      { charset: 'utf-8' },
      { name: 'viewport', content: 'width=device-width, initial-scale=1' },
      { hid: 'description', name: 'description', content: pkg.description }
      // 这里可以添加网站验证码信息
      // { name: 'google-site-verification', content: 'xxx' },
      // 实测百度无法通过验证，此问题还没解决
      // { name: 'baidu-site-verification', content: 'code-xxx' },
    ],
    link: [  // favicon，若引用css不会进行打包处理
      { rel: 'icon', type: 'image/x-icon', href: '/favicon.ico' }
    ]
  },
  // nuxt 加载进度条配置 (https://zh.nuxtjs.org/api/configuration-loading)
  loading: { color: '
  // 全局css (https://go.nuxtjs.dev/config-css)
  css: [    // 全局css（会进行webpack打包处理）
    'element-ui/lib/theme-chalk/index.css'
  ],
   // 配置后，会在页面渲染之前加载 (https://go.nuxtjs.dev/config-plugins)
  plugins: [        // 插件
    '@/plugins/element-ui'
  ],
  // 工具module (https://go.nuxtjs.dev/config-modules)
  modules: [        // 模块
    '@nuxtjs/axios',
  ],
 // 如果添加了@nuxt/axios则会需要此配置来覆盖默认的一些配置 (https://go.nuxtjs.dev/config-axios)
  axios: {
    https: true,
    progress: true, // 是否显示加载进度条
    credentials: true, // 请求携带cookie
    baseURL: 'https://www.abeille.top/api',
    proxy: true // 请求代理，开发中跨域问题解决方法
  },
  // 打包配置 (https://go.nuxtjs.dev/config-build)
  build: {      // 打包
    transpile: [/^element-ui/],
    extend(config, ctx) {       // webpack自定义配置
    }
  }
}
复制代码
```

```json
{
  "scripts": {

    "dev": "cross-env NODE_ENV=development nodemon server/index.js --watch server",

    "build": "nuxt build",

    "start": "cross-env NODE_ENV=production node server/index.js",

    "generate": "nuxt generate"
  }
}
复制代码
```

## 二、nuxt常用配置

第一种 `nuxt.config.js` :

```yaml
module.exports = {
    server: {
        port: 8000,
        host: '127.0.0.1'
    }
}
复制代码
```

第二种 `package.json` :

```json
"config": {
    "nuxt": {
        "port": "8000",
        "host": "127.0.0.1"
    }
}
复制代码
```

在开发多页项目时，都会定义一个全局的 `CSS`来初始化我们的页面渲染，比如把 `padding`和 `margin`设置成 `0`，网上也有非常出名的开源 `css`文件 `normailze.css`。要定义这些配置，需要在 `nuxt.config.js`里进行操作。

比如现在我们要把页面字体设置为红色，就可以在 `assets/css/common.css`文件，然后把字体设置为红色。

`/assets/css/common.css`

```css
html{
    color:red;
}
body {
    margin:0;
    padding:0;
}
复制代码
```

`/nuxt.config.js`

```vbnet
css:[
复制代码
```

设置好后，在终端输入 `pm run dev`。然后你会发现字体已经变成了红色。

## 三、Nuxt的路由配置和参数传递

`Nuxt.js`的路由并不复杂，它给我们进行了封装，让我们节省了很多配置环节。

`Nuxt`会自动生成路由，故而只需使用 `<nuxt-link :to> </nuxt-link>`，而非 `<router-link :to> </router-link>`

页面跳转方式：

* 不要写成 `a`标签，因为是重新获取一个新的页面，并不是 `SPA`
* `this.$router.push('/users')`

动态路由：

* 在 `Nuxt.js` 里面定义带参数的动态路由，需要创建对应的以下划线作为前缀的 `Vue` 文件 或 目录。
* 获取动态参数 `{{$route.params.id}}`

`this.$route.query.key`的方式参数显示在地址栏上, 但是并不是我们想要的， ``:id?id=``?``所以建议还是尽量使用 `router-link`来实现跳转来解决地址栏的变化,更方便网站的优化

```ruby
方式一
传参：
:to="{path:'/about',query:{index:id}}" target="_blank" >
地址栏显示：
loaclhost:3000/about/id
接收地址栏参数：
this.$route.query.index

方式二
传参：
"_blank" :to="{name: 'log-id', params:{id: n.id,key:value}}">
地址栏显示：
loaclhost:3000/about/id
接收：
async asyncData ({ params }) { //  params.id 就是我们传进来的值}// 或者 created () { this.$route.params.xxx}`
复制代码
```

```bash
方式一
跳转：
getDescribe(id) {
    // 直接调用$router.push 实现携带参数的跳转
    this.$router.push({
      path: `/describe/${id}`,
    })
接收：
$route.params.id

方式二
注意：
页面之间的跳转使用query 不然的话刷新页面后会找不到参数
跳转：
 this.$router.push({
          path: '/describe',
          query: {
            id: id
          }
})
接收：
$route.query.id
复制代码
```

`Nuxt.js` 可以让你在动态路由对应的页面组件中配置一个 `validate`方法用于校验动态路由参数的有效性。该函数有一个布尔类型的返回值，如果返回 `true`则表示校验通过，如果返回 `false`则表示校验未通过。

```js
export default {

  validate(obj) {

    return /^\d+$/.test(obj.params.id)
  }
}
复制代码
```

**注意：**

在 `nuxt`框架中，在 `nuxt.config.js`中 `components: true`已开启了组件自动导入，故而不需要在 `vue`文件中通过 `import`和 `components`来导入组件，只需在 `template`中写入相应组件的组件名即可

* 添加一个 `Vue`文件，作为父组件
* 添加一个与父组件同名的文件夹来存放子视图组件
* 在父文件中，添加组件，用于展示匹配到的子视图

路由的动画效果，也叫作页面的更换效果。Nuxt.js提供两种方法为路由提供动画效果，一种是全局的，一种是针对单独页面制作。

全局动画默认使用page来进行设置，例如现在我们为每个页面都设置一个进入和退出时的渐隐渐现的效果。我们可以先在根目录的 `assets/css`下建立一个 `normailze.css`文件。

**（1）添加样式文件**

`/assets/css/normailze.css`(没有请自行建立)

```css
.page-enter-active, .page-leave-active {
    transition: opacity 2s;
}

.page-enter, .page-leave-active {
    opacity: 0;
}
复制代码
```

**（2）文件配置**

然后在 `nuxt.config.js`里加入一个全局的 `css`文件就可以了。

```vbnet
css:[
复制代码
```

```ruby
:to="{name:'news-id',params:{id:123}}">News-1</nuxt-link>>
复制代码
```

想给一个页面单独设置特殊的效果时，我们只要在css里改变默认的page，然后在页面组件的配置中加入transition字段即可。例如，我们想给about页面加入一个字体放大然后缩小的效果，其他页面没有这个效果。

**（1）在全局样式 `assets/main.css` 中添加以下内容**

```css
.test-enter-active, .test-leave-active {
    transition: all 2s;
    font-size:12px;
}
.test-enter, .test-leave-active {
    opacity: 0;
    font-size:40px;
}
复制代码
```

**（2）然后在about/index.vue组件中设置**

```arduino
export default {
  transition:'test'
}
复制代码
```

这时候就有了页面的切换独特动效了。

总结：在需要使用的页面导入即可。

## 四、Nuxt的默认模版和默认布局

在开发应用时，经常会用到一些公用的元素，比如网页的标题是一样的，每个页面都是一模一样的标题。这时候我们有两种方法，第一种方法是作一个公用的组件出来，第二种方法是修改默认模版。这两种方法各有利弊，比如公用组件更加灵活，但是每次都需要自己手动引入；模版比较方便，但是只能每个页面都引入。

`Nuxt`为我们提供了超简单的默认模版订制方法，只要在根目录下创建一个 `app.html`就可以实现了。现在我们希望每个页面的最上边都加入"学习nuxt.js" 这几个字，我们就可以使用默认模版来完成。

app.html中：

```xml

<html lang="en">
<head>
   {{ HEAD }}
head>
<body>
    <p>学习nuxt.jsp>
    {{ APP }}
body>
html>

复制代码
```

这里的 `{{ HEAD }}`读取的是 `nuxt.config.js`里的信息， `{{APP}} `就是我们写的 `pages`文件夹下的主体页面了。需要注意的是 `HEAD`和 `APP`都需要大写，如果小写会报错的。

注意：如果你建立了默认模板后，记得要重启服务器，否则显示不会成功；但是默认布局是不用重启服务器的。

2.默认布局 默认模板类似的功能还有默认布局，但是从名字上你就可以看出来，默认布局主要针对于页面的统一布局使用。它在位置根目录下的 `layouts/default.vue`。需要注意的是在默认布局里不要加入头部信息，只是关于 `<template></template>`标签下的内容统一订制。

需求：我们在每个页面的最顶部放入"学习nuxt.js" 这几个字，看一下在默认布局里的实现。

```xml
<template>
  <div>
    <p>学习nuxt.jsp>
    <nuxt/>
  div>
template>
复制代码
```

这里的 `<nuxt></nuxt>`就相当于我们每个页面的内容，你也可以把一些通用样式放入这个默认布局里，但会增加页面的复杂程度。

总结：要区分默认模版和默认布局的区别，模版可以订制很多头部信息，包括 `IE`版本的判断；模版只能定制 `<template></template>`里的内容，跟布局有关系。在工作中修改时要看情况来编写代码。

## 五、Nuxt插件的使用

(1)、下载 `npm i element-ui -S`

(2)、在 `plugins`文件夹下面，创建 `ElementUI.js`文件

(3)、在 `nuxt.config.js`中添加配置

```css
css: [
  'element-ui/lib/theme-chalk/index.css'
],
plugins: [
  {src: '~/plugins/ElementUI', ssr: true }
],
build: {
  vendor: ['element-ui']
}
复制代码
```

```scss
# 先下载element-ui

npm install element-ui --save

# 如果使用按需引入，必须安装babel-plugin-component(官网有需要下载说明，此插件根据官网规则不同，安装插件不同)

npm install babel-plugin-component --save-dev
复制代码
```

安装好以后，按照 `nuxt.js`中的规则，你需要在 `plugins/` 目录下创建相应的插件文件

在 **文件根目录**创建(或已经存在) `plugins/`目录，创建名为： `element-ui.js`的文件，内容如下：

```js
import Vue from 'vue'

import { Button } from 'element-ui'

export default ()=>{
    Vue.use(Button)
}
复制代码
```

在 `nuxt.config.js`中，添加配置为： `plugins`

```vbnet
css:[

],
plugins:[

]
复制代码
```

默认为：开启 `SSR`，采用服务端渲染，也可以 **手动配置关闭SSR**，配置为：

```css
css:[
    'element-ui/lib/theme-chalk/index.css'
],
plugins:[
    {
        src:'~/plugins/element-ui',
        ssr:false    //关闭ssr
    }
]
复制代码
```

在 `nuxt.config.js`中，配置在 `build`选项中，规则为官网规则：

```js
build: {
      babel:{
          "plugins":[
              [
                  "component",
                  {
                      "libraryName":"element-ui",
                      "styleLibraryName":"theme-chalk"
                  }
              ]
          ]
      },

    extend (config, ctx) {
      if (ctx.isClient) {
        config.module.rules.push({
           enforce: 'pre',
           test: /\.(js|vue)$/,
           loader: 'eslint-loader',
           exclude: /(node_modules)/
        })
      }
    }
 }

复制代码
```

(1)、安装 `npm install --save axios`

(2)、使用

```js
import axios from 'axios'

asyncData(context, callback) {
  axios.get('http://localhost:3301/in_theaters')
    .then(res => {
      console.log(res);
      callback(null, {list: res.data})
    })
}
复制代码
```

(3)、为防止重复打包，在 `nuxt.config.js`中配置

```css
module.exports = {
  build: {
    vendor: ['axios']
  }
}
复制代码
```

`Nuxt.js` 内置引用了 `vuex` 模块，所以不需要额外安装。

`Nuxt.js` 会找到应用根目录下的 `store` 目录，如果该目录存在，它将做以下的事情：

* 引用 vuex 模块
* 将 vuex 模块 加到 vendors 构建配置中去
* 设置 Vue 根实例的 store 配置项

`Nuxt.js` 支持两种使用 `store` 的方式，你可以择一使用：

* 模块方式： store 目录下的每个 .js 文件会被转换成为状态树指定命名的子模块 （当然，index 是根模块）
* 普通方式： store/index.js 返回一个 Vuex.Store 实例（官方不推荐）

模块方式

状态树还可以拆分成为模块， `store` 目录下的每个 `.js` 文件会被转换成为状态树指定命名的子模块

使用状态树模块化的方式， `store/index.js` 不需要返回 `Vuex.Store` 实例，而应该直接将 `state&#x3001;mutations &#x548C; actions`暴露出来：

`index.js`

```js
export const state = () => ({
    articleTitle: [],
    labelList: []
})

export const mutations = {

    updateArticleTitle(state, action) {
        state.articleTitle = action
    },

    updateLabel(state, action){
        state.labelList = action
    }
}

export const actions = {

    fetchArticleTitle({ commit }) {
        return this.$axios
            .$get('http://localhost:3000/article/title')
            .then(response => {
                commit('updateArticleTitle', response.data)
            })
    },

    fetchLabel({ commit }) {
        return this.$axios
            .$get('http://localhost:3000/label/list')
            .then(response => {
                commit('updateLabel', response.data)
            })
    }
}
复制代码
```

`index.vue`<br> `fetch`方法中触发异步提交状态事件

```xml
<script>
import ArticleList from '~/components/archive/list'
import Banner from '~/components/archive/banner'

export default {
  components: {
    ArticleList,
    Banner,
  },
  async asyncData({ app }){

    let article = await app.$axios.get(`http://localhost:3000/article/list?pageNum=1&pageSize=5`)
    return {articleList: article.data.data}
  },
  async fetch({ store }) {
      return Promise.all([
          store.dispatch('common/fetchArticleTitle'),
          store.dispatch('common/fetchLabel')
      ])
  },
  computed: {

  },
  methods: {

  }
}
script>
复制代码
```

对应组件中通过 `computed` 方法获取状态数据

```js
computed: {
      labelList(){
            return this.$store.state.common.labelList
      }
}
复制代码
```

## 六、Nuxt的错误页面和个性meta设置

当用户输入路由错误的时候，我们需要给他一个明确的指引，所以说在应用程序开发中404页面是必不可少的。 `Nuxt.js`支持直接在默认布局文件夹里建立错误页面。

在根目录下的 `layouts`文件夹下建立一个 `error.vue`文件，它相当于一个显示应用错误的组件。

```xml
<template>
  <div>
      <h2 v-if="error.statusCode==404">404页面不存在h2>
      <h2 v-else>500服务器错误h2>
      <ul>
          <li><nuxt-link to="/">HOMEnuxt-link>li>
      ul>
  div>
template>

<script>
export default {
  props:['error']
}
script>
复制代码
```

代码用 `v-if`进行判断错误类型，需要注意的是这个错误是你需要在 ``<script></code>里进行声明的，如果不声明程序是找不到<code>error.statusCode</code>的。</p><p>这里我也用了一个<code><nuxt-link></code>的简单写法直接跟上路径就可以了。</p><p>页面的<code>Meta</code>对于<code>SEO</code>的设置非常重要，比如你现在要作个新闻页面，那为了搜索引擎对新闻的收录，需要每个页面对新闻都有不同的<code>title</code>和<code>meta</code>设置。直接使用<code>head</code>方法来设置当前页面的头部信息就可以了。我们现在要把<code>New-1</code>这个页面设置成个性的<code>meta</code>和<code>title</code>。</p><p><code>/pages/news/index.vue</code></p><pre><code class="hljs language-ruby copyable"><li><nuxt-link <span class="hljs-symbol">:to=<span class="hljs-string">"{name:'news-id',params:{id:123,title:'nuxt.com'}}"</span>>News-</span><span class="hljs-number">1</span><<span class="hljs-regexp">/nuxt-link></li</span>>
<span class="copy-code-btn">复制代码</span></code></pre><p>(2)、第一步完成后，我们修改<code>/pages/news/_id.vue</code>，让它根据传递值变成独特的<code>meta</code>和<code>title</code>标签。</p><pre><code class="hljs language-xml copyable"><span class="hljs-tag"><<span class="hljs-name">template</span>></span>
  <span class="hljs-tag"><<span class="hljs-name">div</span>></span>
      <span class="hljs-tag"><<span class="hljs-name">h2</span>></span>News-Content [{{$route.params.id}}]<span class="hljs-tag"></<span class="hljs-name">h2</span>></span>
      <span class="hljs-tag"><<span class="hljs-name">ul</span>></span>
        <span class="hljs-tag"><<span class="hljs-name">li</span>></span><span class="hljs-tag"><<span class="hljs-name">a</span> <span class="hljs-attr">href</span>=<span class="hljs-string">"/"</span>></span>Home<span class="hljs-tag"></<span class="hljs-name">a</span>></span><span class="hljs-tag"></<span class="hljs-name">li</span>></span>
      <span class="hljs-tag"></<span class="hljs-name">ul</span>></span>
  <span class="hljs-tag"></<span class="hljs-name">div</span>></span>
<span class="hljs-tag"></<span class="hljs-name">template</span>></span>

<span class="hljs-tag"><<span class="hljs-name">script</span>></span><span class="javascript">
<span class="hljs-keyword">export</span> <span class="hljs-keyword">default</span> {
  validate ({ params }) {

    <span class="hljs-keyword">return</span> <span class="hljs-regexp">/^\d+$/</span>.<span class="hljs-title function_">test</span>(params.<span class="hljs-property">id</span>)
  },
  <span class="hljs-title function_">data</span>(<span class="hljs-params"></span>){
    <span class="hljs-keyword">return</span>{
      <span class="hljs-attr">title</span>:<span class="hljs-variable language_">this</span>.<span class="hljs-property">$route</span>.<span class="hljs-property">params</span>.<span class="hljs-property">title</span>,
    }
  },

  <span class="hljs-title function_">head</span>(<span class="hljs-params"></span>){
      <span class="hljs-keyword">return</span>{
        <span class="hljs-attr">title</span>:<span class="hljs-variable language_">this</span>.<span class="hljs-property">title</span>,
        <span class="hljs-attr">meta</span>:[
          {<span class="hljs-attr">hid</span>:<span class="hljs-string">'description'</span>,<span class="hljs-attr">name</span>:<span class="hljs-string">'news'</span>,<span class="hljs-attr">content</span>:<span class="hljs-string">'This is news page'</span>}
        ]
      }
    }
}
</span><span class="hljs-tag"></<span class="hljs-name">script</span>></span>

<span class="copy-code-btn">复制代码</span></code></pre><p>注意：为了避免子组件中的<code>meta</code>标签不能正确覆盖父组件中相同的标签而产生重复的现象，建议利用 <code>hid</code> 键为<code>meta</code>标签配一个唯一的标识编号。</p><h2>七、asyncData方法获取数据</h2><p><code>Nuxt.js</code>贴心的为我们扩展了<code>Vue.js</code>的方法，增加了<code>anyncData</code>，异步请求数据。</p><p><code>Nuxt.js</code> 提供了几种不同的方法来使用 <code>asyncData</code> 方法，你可以选择自己熟悉的一种来用：</p><ul><li>返回一个 <code>Promise</code>, nuxt.js会等待该<code>Promise</code>被解析之后才会设置组件的数据，从而渲染组件</li><li>使用 <code>async</code> 或 <code>await</code></li><li>使用回调函数</li></ul><p><strong>返回Promise</strong></p><pre><code class="hljs language-javascript copyable"><span class="hljs-keyword">export</span> <span class="hljs-keyword">default</span> {
  asyncData ({ params }) {
    <span class="hljs-keyword">return</span> axios.<span class="hljs-title function_">get</span>(<span class="hljs-string">`https://my-api/posts/<span class="hljs-subst">${params.id}</span>`</span>)
      .<span class="hljs-title function_">then</span>(<span class="hljs-function">(<span class="hljs-params">res</span>) =></span> {
        <span class="hljs-keyword">return</span> { <span class="hljs-attr">title</span>: res.<span class="hljs-property">data</span>.<span class="hljs-property">title</span> }
      })
  }
}
<span class="copy-code-btn">复制代码</span></code></pre><p><strong>使用async或await</strong></p><pre><code class="hljs language-csharp copyable">export <span class="hljs-literal">default</span> {
  <span class="hljs-function"><span class="hljs-keyword">async</span> <span class="hljs-title">asyncData</span> (<span class="hljs-params">{ <span class="hljs-keyword">params</span> }</span>)</span> {
    <span class="hljs-keyword">const</span> { data } = <span class="hljs-keyword">await</span> axios.<span class="hljs-keyword">get</span>(`https:
    <span class="hljs-keyword">return</span> { title: data.title }
  }
}
<span class="copy-code-btn">复制代码</span></code></pre><p><strong>使用回调函数</strong></p><pre><code class="hljs language-javascript copyable"><span class="hljs-keyword">export</span> <span class="hljs-keyword">default</span> {
  asyncData ({ params }, callback) {
    axios.<span class="hljs-title function_">get</span>(<span class="hljs-string">`https://my-api/posts/<span class="hljs-subst">${params.id}</span>`</span>)
      .<span class="hljs-title function_">then</span>(<span class="hljs-function">(<span class="hljs-params">res</span>) =></span> {
        <span class="hljs-title function_">callback</span>(<span class="hljs-literal">null</span>, { <span class="hljs-attr">title</span>: res.<span class="hljs-property">data</span>.<span class="hljs-property">title</span> })
      })
  }
}
<span class="copy-code-btn">复制代码</span></code></pre><p><strong>安装Axios</strong> <code>Vue.js</code>官方推荐使用的远程数据获取方式就<code>Axios</code>，所以我们安装官方推荐，来使用<code>Axios</code>。这里我们使用<code>npm </code>来安装 <code>axios</code>。 直接在终端中输入下面的命令： <code>npm install axios --save</code></p><p>我们在<code>pages</code>下面新建一个文件，叫做<code>ansyData.vue</code>。然后写入下面的代码：</p><pre><code class="hljs language-xml copyable"><span class="hljs-tag"><<span class="hljs-name">script</span>></span><span class="javascript">
<span class="hljs-keyword">import</span> axios <span class="hljs-keyword">from</span> <span class="hljs-string">'axios'</span>
<span class="hljs-keyword">export</span> <span class="hljs-keyword">default</span> {
  <span class="hljs-title function_">data</span>(<span class="hljs-params"></span>){
     <span class="hljs-keyword">return</span> {
         <span class="hljs-attr">name</span>:<span class="hljs-string">'hello World'</span>,
     }
  },
  <span class="hljs-title function_">asyncData</span>(<span class="hljs-params"></span>){
      <span class="hljs-keyword">return</span> axios.<span class="hljs-title function_">get</span>(<span class="hljs-string">'接口'</span>)
      .<span class="hljs-title function_">then</span>(<span class="hljs-function">(<span class="hljs-params">res</span>)=></span>{
          <span class="hljs-variable language_">console</span>.<span class="hljs-title function_">log</span>(res)
          <span class="hljs-keyword">return</span> {<span class="hljs-attr">info</span>:res.<span class="hljs-property">data</span>}
      })
  }
}
</span><span class="hljs-tag"></<span class="hljs-name">script</span>></span>
<span class="copy-code-btn">复制代码</span></code></pre><p>asyncData的方法会把值返回到data中。是组件创建（页面渲染）之前的动作，所以不能使用this.info</p><p><strong>return的重要性</strong></p><p>一定要return出去获取到的对象，这样就可以在组件中使用，这里返回的数据会和组件中的data合并。这个函数不光在服务端会执行，在客户端同样也会执行。</p><pre><code class="hljs language-less copyable"><span class="hljs-selector-tag">async</span> <span class="hljs-selector-tag">asyncData</span>(context) {
  <span class="hljs-selector-tag">let</span> <span class="hljs-selector-attr">[newDetailRes, hotInformationRes, correlationRes]</span> = <span class="hljs-selector-tag">await</span> <span class="hljs-selector-tag">Promise</span><span class="hljs-selector-class">.all</span>([
    axios.<span class="hljs-built_in">post</span>(<span class="hljs-string">'http://www.huanjingwuyou.com/eia/news/detail'</span>, {
      <span class="hljs-attribute">newsCode</span>: newsCode
    }),
    axios.<span class="hljs-built_in">post</span>(<span class="hljs-string">'http://www.huanjingwuyou.com/eia/news/select'</span>, {
      <span class="hljs-attribute">newsType</span>: newsType,
      <span class="hljs-attribute">start</span>: <span class="hljs-number">0</span>,
      <span class="hljs-attribute">pageSize</span>: <span class="hljs-number">10</span>,
      <span class="hljs-attribute">newsRecommend</span>: true
    }),
    axios.<span class="hljs-built_in">post</span>(<span class="hljs-string">'http://www.huanjingwuyou.com/eia/news/select'</span>, {
      <span class="hljs-attribute">newsType</span>: newsType,
      <span class="hljs-attribute">start</span>: <span class="hljs-number">0</span>,
      <span class="hljs-attribute">pageSize</span>: <span class="hljs-number">3</span>,
      <span class="hljs-attribute">newsRecommend</span>: false
    })
  ])
  <span class="hljs-selector-tag">return</span> {
    newDetailList: newDetailRes.data.result,
    hotNewList: hotInformationRes.data.result.data,
    newsList: correlationRes.data.result.data,
    newsCode: newsCode,
    newsType: newsType
  }
}
<span class="copy-code-btn">复制代码</span></code></pre><p>当然上面的方法稍显过时，现在都在用<code>ansyc...await</code>来解决异步,改写上面的代码。</p><pre><code class="hljs language-xml copyable"><span class="hljs-tag"><<span class="hljs-name">script</span>></span><span class="javascript">
<span class="hljs-keyword">import</span> axios <span class="hljs-keyword">from</span> <span class="hljs-string">'axios'</span>
<span class="hljs-keyword">export</span> <span class="hljs-keyword">default</span> {
  <span class="hljs-title function_">data</span>(<span class="hljs-params"></span>){
     <span class="hljs-keyword">return</span> {
         <span class="hljs-attr">name</span>:<span class="hljs-string">'hello World'</span>,
     }
  },
  <span class="hljs-keyword">async</span> <span class="hljs-title function_">asyncData</span>(<span class="hljs-params"></span>){
      <span class="hljs-keyword">let</span> {data}=<span class="hljs-keyword">await</span> axios.<span class="hljs-title function_">get</span>(<span class="hljs-string">'接口'</span>)
      <span class="hljs-keyword">return</span> {<span class="hljs-attr">info</span>: data}
  }
}
</span><span class="hljs-tag"></<span class="hljs-name">script</span>></span>
<span class="copy-code-btn">复制代码</span></code></pre><ul><li><code>asyncData</code> 方法会在组件(限于页面组件)每次加载之前被调用</li><li><code>asyncData</code> 可以在服务端或路由更新之前被调用</li><li>第一个参数被设定为当前页面的上下文对象</li><li><code>Nuxt</code>会将 <code>asyncData</code> 返回的数据融合到组件的<code>data</code>方法返回的数据一并返回给组件使用</li><li>对于 <code>asyncData</code> 方式是在组件初始化前被调用的，所以在方法内饰没办法通过<code>this</code>来引用组件的实例对象</li></ul><h2>八、静态资源和打包</h2><p><strong>（1）直接引入图片</strong></p><p>在网上任意下载一个图片，放到项目中的static文件夹下面，然后可以使用下面的引入方法进行引用</p><pre><code class="hljs language-css copyable"><<span class="hljs-selector-tag">div</span>><<span class="hljs-selector-tag">img</span> <span class="hljs-attribute">src</span>="~static/logo<span class="hljs-selector-class">.png</span>" /></<span class="hljs-selector-tag">div</span>>
<span class="copy-code-btn">复制代码</span></code></pre><p>"~"就相当于定位到了项目根目录，这时候图片路径就不会出现错误，就算打包也是正常的。</p><p><strong>（2）CSS引入图片</strong></p><p>如果在<code>CSS</code>中引入图片，方法和<code>html</code>中直接引入是一样的，也是用"~"符号引入。</p><pre><code class="hljs language-xml copyable"><span class="hljs-tag"><<span class="hljs-name">style</span>></span><span class="css">
  <span class="hljs-selector-class">.diss</span>{
    <span class="hljs-attribute">width</span>: <span class="hljs-number">300px</span>;
    <span class="hljs-attribute">height</span>: <span class="hljs-number">100px</span>;
    <span class="hljs-attribute">background-image</span>: <span class="hljs-built_in">url</span>(<span class="hljs-string">'~static/logo.png'</span>)
  }
</span><span class="hljs-tag"></<span class="hljs-name">style</span>></span>
<span class="copy-code-btn">复制代码</span></code></pre><p>这时候在<code>npm run dev</code> 下是完全正常的。</p><p>用<code>Nuxt.js</code>制作完成后，你可以打包成静态文件并放在服务器上，进行运行。</p><p>在终端中输入：<code>npm run generate</code></p><p>然后在<code>dist</code>文件夹下输入<code>live-server</code>就可以了。</p><p>总结：<code>Nuxt.js</code>框架非常简单，因为大部分的事情他都为我们做好了，我们只要安装它的规则来编写代码。</p><h2>九、nuxt的跨域解决+拦截器</h2><p><code>nuxt.js</code> 在创建项目的时候可以选择安装 <code>axios</code>。 <code>axios</code> 与 <code>@nuxtjs/axios</code> 可以共用 <code>nuxt.config.js</code> 中代理配置。</p><p>使用的时候需要注意 <code>asyncData()</code> 中需要请求全链接或者服务器有配代理的接口，也就是在服务器渲染的时候需要拿到组装的数据，等到了浏览器本地之后，需要走代理请求，否则会出现跨域，支持加载更多跟其他接口请求操作，更换数据也是没问题的，但是到浏览器之后必须走代理请求，在服务器渲染的时候必须走全链接请求或者走服务器配置了代理的请求，没配置代理就走全链接请求，在服务器不存在跨域</p><p>在创建项目的时候，就可以选择导入 <code>@nuxtjs/axios</code>，它是对 <code>axios</code> 包装，更好在 <code>nuxt.js</code> 中使用，可以通过 <code>this.$axios.get(url).then()</code> 进行全局使用。</p><p>检查 <code>package.json</code> 文件中 <code>dependencies</code> 有没有存在 <code>@nuxtjs/axios</code>，没有命令行安装(建议创建项目的时候就通过脚手架安装上), <code>@nuxtjs/proxy</code>代理</p><pre><code class="hljs language-scss copyable">npm install <span class="hljs-attr">--save</span> <span class="hljs-keyword">@nuxtjs</span>/axios <span class="hljs-keyword">@nuxtjs</span>/proxy
<span class="copy-code-btn">复制代码</span></code></pre><p><code>nuxt.config.js</code> 配置，代理配置</p><pre><code class="hljs language-arduino copyable"><span class="hljs-keyword">export</span> <span class="hljs-keyword">default</span> {
  head: { ... },
  css: [],

  modules: [
    <span class="hljs-string">'@nuxtjs/axios'</span>,
    <span class="hljs-string">'@nuxtjs/proxy'</span>
  ],

  axios: {

    proxy: <span class="hljs-literal">true</span>,

    prefix: <span class="hljs-string">'/api'</span>,

    credentials: <span class="hljs-literal">true</span>
  },

  proxy: {

    <span class="hljs-string">'/api'</span>: {

      target: <span class="hljs-string">'http://test.dzm.com'</span>,

      changeOrigin: <span class="hljs-literal">true</span>,
      pathRewrite: {

        <span class="hljs-string">'^/api'</span>: <span class="hljs-string">'/'</span>
      }
    }
  },

  build: {

    vendor: [<span class="hljs-string">'axios'</span>]
  }
}
<span class="copy-code-btn">复制代码</span></code></pre><p>在组件中使用，这样就可以在服务器渲染到页面之后，通过请求进行更换数据，因为到页面之后需要走代理的方式才能获取到数据，否则会报错跨域。</p><pre><code class="hljs language-javascript copyable"><span class="hljs-title function_">mounted</span>(<span class="hljs-params"></span>) {
    <span class="hljs-variable language_">this</span>.<span class="hljs-property">$axios</span>.<span class="hljs-title function_">get</span>(<span class="hljs-string">"/index"</span>).<span class="hljs-title function_">then</span>(<span class="hljs-function"><span class="hljs-params">res</span>=></span>{
        <span class="hljs-variable language_">console</span>.<span class="hljs-title function_">log</span>(res)
    })

}
<span class="copy-code-btn">复制代码</span></code></pre><p>安装命令，默认一般 <code>nuxt.js</code> 自带 <code>axios</code>，是不需要手动安装的，在 <code>package.json</code> 文件中 <code>dependencies</code> 中可能并不体现出来，可以通过 <code>node_modules</code> 文件夹找到 <code>axios</code>。</p><p>安装<code>axios</code></p><pre><code class="hljs language-sql copyable">npm install <span class="hljs-variable">@nuxtjs</span><span class="hljs-operator">/</span>axios
<span class="copy-code-btn">复制代码</span></code></pre><p>如果找到了，就不要去安装了，直接使用即可，<strong>axios 与 @nuxtjs/axios 可以共用 nuxt.config.js 中代理配置。</strong> 可直接在<code>vue</code>组件中使用即可</p><pre><code class="hljs language-xml copyable"><span class="hljs-tag"><<span class="hljs-name">script</span>></span><span class="javascript">
<span class="hljs-keyword">import</span> axios <span class="hljs-keyword">from</span> <span class="hljs-string">'axios'</span>
<span class="hljs-keyword">export</span> <span class="hljs-keyword">default</span> {
    <span class="hljs-title function_">mounted</span>(<span class="hljs-params"></span>) {
        axios.<span class="hljs-title function_">get</span>(<span class="hljs-string">"/api/index"</span>).<span class="hljs-title function_">then</span>(<span class="hljs-function"><span class="hljs-params">res</span>=></span>{
            <span class="hljs-variable language_">console</span>.<span class="hljs-title function_">log</span>(res)
        })

    }
}
</span><span class="hljs-tag"></<span class="hljs-name">script</span>></span>
<span class="copy-code-btn">复制代码</span></code></pre><p><strong><code>axios</code> 可以进行封装使用，跟 <code>vue</code> 中一样</strong></p><p><code>axios.js</code></p><pre><code class="hljs language-javascript copyable"><span class="hljs-keyword">import</span> axios <span class="hljs-keyword">from</span> <span class="hljs-string">'axios'</span>

<span class="hljs-keyword">const</span> service = axios.<span class="hljs-title function_">create</span>({

  <span class="hljs-attr">baseURL</span>: <span class="hljs-string">''</span>,

  <span class="hljs-attr">timeout</span>: <span class="hljs-number">90000</span>
})

<span class="hljs-keyword">export</span> {
  service <span class="hljs-keyword">as</span> axios
}
<span class="copy-code-btn">复制代码</span></code></pre><p><code>request.js</code></p><pre><code class="hljs language-php copyable">import { axios } <span class="hljs-keyword">from</span> <span class="hljs-string">'./axios'</span>

<span class="hljs-keyword">const</span> <span class="hljs-variable constant_">BASE_URL</span> = process.env.NODE_ENV === <span class="hljs-string">'production'</span> ? <span class="hljs-string">'http://dzm.com'</span> : <span class="hljs-string">'http://test.dzm.com'</span>

export <span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">getxxx</span> (<span class="hljs-params">parameter</span>) </span>{
  <span class="hljs-keyword">return</span> <span class="hljs-title function_ invoke__">axios</span>({
    <span class="hljs-attr">url</span>: BASE_URL + `/<span class="hljs-keyword">list</span>`,
    <span class="hljs-attr">method</span>: <span class="hljs-string">'get'</span>,
    <span class="hljs-attr">params</span>: parameter
  })
}

export <span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">postxxx</span> (<span class="hljs-params">parameter</span>) </span>{
  <span class="hljs-keyword">return</span> <span class="hljs-title function_ invoke__">axios</span>({
    <span class="hljs-attr">url</span>: BASE_URL + `/reload`,
    <span class="hljs-attr">method</span>: <span class="hljs-string">'post'</span>,
    <span class="hljs-attr">data</span>: parameter
  })
}

export <span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">getxxx</span> (<span class="hljs-params">parameter</span>) </span>{
  <span class="hljs-keyword">return</span> <span class="hljs-title function_ invoke__">axios</span>({
    <span class="hljs-attr">url</span>: <span class="hljs-string">'/api'</span> + `/<span class="hljs-keyword">list</span>`,
    <span class="hljs-attr">method</span>: <span class="hljs-string">'get'</span>,
    <span class="hljs-attr">params</span>: parameter
  })
}

export <span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">postxxx</span> (<span class="hljs-params">parameter</span>) </span>{
  <span class="hljs-keyword">return</span> <span class="hljs-title function_ invoke__">axios</span>({
    <span class="hljs-attr">url</span>: <span class="hljs-string">'/api'</span> + `/reload`,
    <span class="hljs-attr">method</span>: <span class="hljs-string">'post'</span>,
    <span class="hljs-attr">data</span>: parameter
  })
}
<span class="copy-code-btn">复制代码</span></code></pre><p>安装: <code>npm install @nuxtjs/axios @nuxtjs/proxy --save</code></p><p><code>nuxt.config.js</code></p><pre><code class="hljs language-css copyable">module<span class="hljs-selector-class">.expores</span>{
  plugins: [
    {
      <span class="hljs-attribute">src</span>: <span class="hljs-string">"~/plugins/axios"</span>,
      ssr: false
    },
  ],
  modules: [
    // Doc: https://axios.nuxtjs.org/usage
    <span class="hljs-string">'@nuxtjs/axios'</span>,
  ]
}
<span class="copy-code-btn">复制代码</span></code></pre><p><code>plugins/axios.js</code></p><pre><code class="hljs language-javascript copyable"><span class="hljs-keyword">export</span> <span class="hljs-keyword">default</span> ({ $axios, redirect }) => {
  $axios.<span class="hljs-title function_">onRequest</span>(<span class="hljs-function"><span class="hljs-params">config</span> =></span> {
    <span class="hljs-variable language_">console</span>.<span class="hljs-title function_">log</span>(<span class="hljs-string">'Making request to '</span> + config.<span class="hljs-property">url</span>)
  })

  $axios.<span class="hljs-title function_">onError</span>(<span class="hljs-function"><span class="hljs-params">error</span> =></span> {
    <span class="hljs-keyword">const</span> code = <span class="hljs-built_in">parseInt</span>(error.<span class="hljs-property">response</span> && error.<span class="hljs-property">response</span>.<span class="hljs-property">status</span>)
    <span class="hljs-keyword">if</span> (code === <span class="hljs-number">400</span>) {
      <span class="hljs-title function_">redirect</span>(<span class="hljs-string">'/400'</span>)
    }
  })
}

<span class="hljs-keyword">export</span> <span class="hljs-keyword">default</span> <span class="hljs-keyword">function</span> (<span class="hljs-params">app</span>) {
  <span class="hljs-keyword">let</span> axios = app.<span class="hljs-property">$axios</span>;

  axios.<span class="hljs-property">defaults</span>.<span class="hljs-property">timeout</span> = <span class="hljs-number">10000</span>
  axios.<span class="hljs-property">defaults</span>.<span class="hljs-property">headers</span>.<span class="hljs-property">post</span>[<span class="hljs-string">'Content-Type'</span>] = <span class="hljs-string">'application/x-www-form-urlencoded'</span>

  axios.<span class="hljs-title function_">onRequest</span>(<span class="hljs-function"><span class="hljs-params">config</span> =></span> {})

  axios.<span class="hljs-title function_">onResponse</span>(<span class="hljs-function"><span class="hljs-params">res</span> =></span> {})

  axios.<span class="hljs-title function_">onError</span>(<span class="hljs-function"><span class="hljs-params">error</span> =></span> {})
}
<span class="copy-code-btn">复制代码</span></code></pre><h2>十、@nuxtjs/toast模块</h2><p><code>nuxt</code>中<code>toast</code>组件可参考<code>element</code>中的<code>message</code>，共四个状态</p><p><code>toast</code>可以说是很常用的功能，一般的<code>UI</code>框架都会有这个功能。但如果你的站点没有使用<code>UI</code>框架，而<code>alert</code>又太丑，不妨引入该模块：<code>npm install @nuxtjs/toast</code></p><p>然后在<code>nuxt.config.js</code>中引入</p><pre><code class="hljs language-css copyable">module<span class="hljs-selector-class">.exports</span> = {
    modules: [
    <span class="hljs-string">'@nuxtjs/toast'</span>,
    [<span class="hljs-string">'@nuxtjs/dotenv'</span>, { filename: <span class="hljs-string">'.env.prod'</span> }] // 指定打包时使用的dotenv
  ],
  toast: {// toast模块的配置
    <span class="hljs-attribute">position</span>: <span class="hljs-string">'top-center'</span>,
    duration: <span class="hljs-number">2000</span>
  }
}
<span class="copy-code-btn">复制代码</span></code></pre><p>这样，<code>nuxt</code>就会在全局注册<code>$toast</code>方法供你使用，非常方便：</p><pre><code class="hljs language-kotlin copyable"><span class="hljs-keyword">this</span>.$toast.error(<span class="hljs-string">'服务器开小差啦~~'</span>)
<span class="hljs-keyword">this</span>.$toast.error(<span class="hljs-string">'请求成功~~'</span>)
<span class="copy-code-btn">复制代码</span></code></pre><h2>十一、生命周期延展</h2><p><code>Nuxt</code>扩展了<code>Vue</code>的生命周期，大概如下：</p><pre><code class="hljs language-scss copyable">export default {
  middleware () {},
  validate () {},
  asyncData () {},
  fetch () {},
  beforeCreate () {
  created () {
  beforeMount () {},
  mounted () {}
}
<span class="copy-code-btn">复制代码</span></code></pre><p>该方法是<code>Nuxt</code>最大的一个卖点，服务端渲染的能力就在这里，首次渲染时务必使用该方法。<br><code>asyncData</code>会传进一个<code>context</code>参数，通过该参数可以获得一些信息，如：</p><pre><code class="hljs language-arduino copyable"><span class="hljs-keyword">export</span> <span class="hljs-keyword">default</span> {
  <span class="hljs-built_in">asyncData</span> (ctx) {
    ctx.app
    ctx.route
    ctx.params
    ctx.query
    ctx.error
  }
}
<span class="copy-code-btn">复制代码</span></code></pre><p>使用<code>asyncData</code>钩子时可能会由于服务器错误或<code>api</code>错误导致无法渲染，此时页面还未渲染出来，需要针对这种情况做一些处理，当遇到<code>asyncData</code>错误时，跳转到错误页面，<code>nuxt</code>提供了<code>context.error</code>方法用于错误处理，在<code>asyncData</code>中调用该方法即可跳转到错误页面。</p><pre><code class="hljs language-vbnet copyable">export <span class="hljs-keyword">default</span> {
    <span class="hljs-keyword">async</span> asyncData (ctx) {
        // 尽量使用<span class="hljs-keyword">try</span> <span class="hljs-keyword">catch</span>的写法，将所有异常都捕捉到
        <span class="hljs-keyword">try</span> {
            <span class="hljs-keyword">throw</span> <span class="hljs-built_in">new</span> <span class="hljs-keyword">Error</span>()
        } <span class="hljs-keyword">catch</span> {
            ctx.<span class="hljs-keyword">error</span>({statusCode: <span class="hljs-number">500</span>, message:
        }
    }
}
<span class="copy-code-btn">复制代码</span></code></pre><p>这样，当出现异常时会跳转到默认的错误页，错误页面可以通过<code>/layout/error.vue</code>自定义。</p><p>这里会遇到一个问题，<code>context.error</code>的参数必须是类似<code>{ statusCode: 500, message: '服务器开小差了~' }</code>，<code>statusCode</code>必须是<code>http</code>状态码， 而我们服务端返回的错误往往有一些其他的自定义代码，如<code>{resultCode: 10005, resultInfo: '服务器内部错误' }</code>，此时需要对返回的api错误进行转换一下。</p><p>为了方便，我引入了<code>/plugins/ctx-inject.js</code>为<code>context</code>注册一个全局的错误处理方法： <code>context.$errorHandler(err)</code>。注入方法可以参考：注入 <code>$root</code> 和 <code>context，ctx-inject.js</code>:</p><pre><code class="hljs language-php copyable">
export <span class="hljs-keyword">default</span> (ctx, inject) => {
  ctx.<span class="hljs-variable">$errorHandler</span> = err => {
    <span class="hljs-keyword">try</span> {
      <span class="hljs-keyword">const</span> <span class="hljs-variable constant_">res</span> = err.data
      <span class="hljs-keyword">if</span> (res) {

        ctx.<span class="hljs-title function_ invoke__">error</span>({ <span class="hljs-attr">statusCode</span>: <span class="hljs-number">500</span>, <span class="hljs-attr">message</span>: res.resultInfo })
      } <span class="hljs-keyword">else</span> {
        ctx.<span class="hljs-title function_ invoke__">error</span>({ <span class="hljs-attr">statusCode</span>: <span class="hljs-number">500</span>, <span class="hljs-attr">message</span>: <span class="hljs-string">'服务器开小差了~'</span> })
      }
    } <span class="hljs-keyword">catch</span> {
      ctx.<span class="hljs-title function_ invoke__">error</span>({ <span class="hljs-attr">statusCode</span>: <span class="hljs-number">500</span>, <span class="hljs-attr">message</span>: <span class="hljs-string">'服务器开小差了~'</span> })
    }
  }
}
<span class="copy-code-btn">复制代码</span></code></pre><p>然后在<code>nuxt.config.js</code>使用该插件：</p><pre><code class="hljs language-arduino copyable"><span class="hljs-keyword">export</span> <span class="hljs-keyword">default</span> {
  plugins: [
    <span class="hljs-string">'~/plugins/ctx-inject.js'</span>
  ]
}
<span class="copy-code-btn">复制代码</span></code></pre><p>注入完毕，我们就可以在<code>asyncData</code>介个样子使用了：</p><pre><code class="hljs language-javascript copyable"><span class="hljs-keyword">export</span> <span class="hljs-keyword">default</span> {
    <span class="hljs-keyword">async</span> asyncData (ctx) {

        <span class="hljs-keyword">try</span> {
            <span class="hljs-keyword">throw</span> <span class="hljs-keyword">new</span> <span class="hljs-title class_">Error</span>()
        } <span class="hljs-keyword">catch</span>(err) {
            ctx.$errorHandler(err)
        }
    }
}
<span class="copy-code-btn">复制代码</span></code></pre><p>对于<code>ajax</code>的异常，此时页面已经渲染，出现错误时不必跳转到错误页，可以通过<code>this.$toast.error(res.message)</code> <code>toast</code>出来即可。</p><p>nuxt内置了页面顶部<a href="https://link.juejin.cn?target=https%3A%2F%2Fnuxtjs.org%2Fdocs%2Fconfiguration-glossary%2Fconfiguration-loading%2F%23loading-%25E5%25B1%259E%25E6%2580%25A7%25E9%2585%258D%25E7%25BD%25AE" title="https://nuxtjs.org/docs/configuration-glossary/configuration-loading/#loading-%E5%B1%9E%E6%80%A7%E9%85%8D%E7%BD%AE">loading进度条的样式</a><br>推荐使用，提供页面跳转体验。<br>打开： <code>this.$nuxt.$loading.start()</code><br>完成： <code>this.$nuxt.$loading.finish()</code></p><h2>十二、注意事项</h2><blockquote><p>背景: 在<code><head></code>标签中，以inline的形式引入<code>flexible.js</code>文件。本项目主要为移动端项目，引入<code>flexible.js</code> 实现移动端适配问题。</p></blockquote><p>Nuxt.js 通过 <code>vue-meta</code> 实现头部标签管理，通过查看文档发现，可以按照如下方式配置：</p><pre><code class="hljs language-css copyable">head: {
  script: [{ innerHTML: <span class="hljs-built_in">require</span>(<span class="hljs-string">'./assets/js/flexible'</span>), type: <span class="hljs-string">'text/javascript'</span>, charset: <span class="hljs-string">'utf-8'</span>}],
  __dangerouslyDisableSanitizers: [<span class="hljs-string">'script'</span>]
}
<span class="copy-code-btn">复制代码</span></code></pre><blockquote><p>背景：在组件中的<code><template></code>， <code><script></code> 或 <code><style></code> 上使用各种预处理器，加上处理器后，控制台报错。</p></blockquote><p><code>npm install --save-dev node-sass sass-loader</code></p><p>但是解决过程并不是很顺利的，在阅读中文文档时，忽略版本号，按照上面的提示进行操作，发现不能成功，后来各种<code>debug</code>，最后发现了该解决方案。后知后觉的发现了中文文档的版本号过低，如果需要查看文档，一定要看最新版本的英文文档！</p><blockquote><p>背景：在css中，写入px，通过<code>px2rem loader</code>，将px转换成rem</p></blockquote><p>想到了一个其他方案，可以使用 <code>postcss</code> 处理。可以在 <code>nuxt.config.js</code> 文件中添加配置，也可以在<code>postcss.conf.js</code>文件中添加。</p><pre><code class="hljs language-css copyable">build: {
  postcss: [
    <span class="hljs-built_in">require</span>(<span class="hljs-string">'postcss-px2rem'</span>)({
      remUnit: <span class="hljs-number">75</span> // 转换基本单位
    })
  ]
}
<span class="copy-code-btn">复制代码</span></code></pre><blockquote><p>背景：给 utils 目录添加别名</p></blockquote><p>刚刚说到，<code>Nuxt.js</code>内置了 <code>webpack</code> 配置，如果想要拓展配置，可以在 <code>nuxt.config.js</code> 文件中添加。同时也可以在该文件中，将配置信息打印出来。</p><pre><code class="hljs language-arduino copyable"><span class="hljs-built_in">extend</span> (config, ctx) {
  console.<span class="hljs-built_in">log</span>(<span class="hljs-string">'webpack config:'</span>, config)
  <span class="hljs-keyword">if</span> (ctx.isClient) {

    Object.<span class="hljs-built_in">assign</span>(config.resolve.alias, {
      <span class="hljs-string">'utils'</span>: path.<span class="hljs-built_in">resolve</span>(__dirname, <span class="hljs-string">'utils'</span>)
    })
  }
}
<span class="copy-code-btn">复制代码</span></code></pre><blockquote><p>背景：自己封装了一个 toast vue plugin，由于 vue 实例化的过程没有暴露出来，不知道在哪个时机注入进去。</p></blockquote><p>可以在 <code>nuxt.config.js</code> 中添加 <code>plugins</code> 配置，这样插件就会在 <code>Nuxt.js</code> 应用初始化之前被加载导入。</p><pre><code class="hljs language-ini copyable"><span class="hljs-attr">module.exports</span> = {
  plugins: <span class="hljs-section">['~plugins/toast']</span>
}
<span class="copy-code-btn">复制代码</span></code></pre><p><code>~plugins/toast.js</code> 文件：</p><pre><code class="hljs language-javascript copyable"><span class="hljs-keyword">import</span> <span class="hljs-title class_">Vue</span> <span class="hljs-keyword">from</span> <span class="hljs-string">'vue'</span>
<span class="hljs-keyword">import</span> toast <span class="hljs-keyword">from</span> <span class="hljs-string">'../utils/toast'</span>
<span class="hljs-keyword">import</span> <span class="hljs-string">'../assets/css/toast.css'</span>

<span class="hljs-title class_">Vue</span>.<span class="hljs-title function_">use</span>(toast)
<span class="copy-code-btn">复制代码</span></code></pre></script>``
