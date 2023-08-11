---
link: https://juejin.cn/post/7067345530497531917
title: Vite+Vue3服务端渲染改造指南
description: 服务端渲染（Server Side Render）简称SSR，是在浏览器请求页面URL的时候，服务端将我们需要的HTML文本组装好，并返回给浏览器，这个HTML文本被浏览器解析之后
keywords: 前端,Vite
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2022-02-22T01:50:49.000Z
publisher: 稀土掘金
stats: paragraph=183 sentences=256, words=1132
---
服务端渲染（Server Side Render）简称SSR，是在浏览器请求页面URL的时候，服务端将我们需要的HTML文本组装好，并返回给浏览器，这个HTML文本被浏览器解析之后，不需要经过 JavaScript 脚本的执行，即可直接构建出希望的DOM结构并展示到页面中。这个服务端组装HTML的过程，叫做服务端渲染。 其实早期的Web技术，例如PHP或者JSP这种直接访问并解析的页面也可以叫做服务端渲染，直到ajax技术的成熟带来的前后端分离，则导致了后端只负责数据接口的提供，前端负责页面逻辑的开发，这种叫做客户端渲染。 在Vue项目中大多数也是这种前后端分离的开发模式，Vue整体接管了前端的页面逻辑包括页面渲染，路由调整，组件交互等等，但是这也存在一定的弊端，例如不利于页面SEO，前端页面白屏时间过长等等。 所以，为了解决这些问题，Vue也同时提供了服务端渲染能力，这主要指利用Node.js在后端生成HTML，并提供给浏览器页面首屏的代码同构级别的渲染。

## 服务端渲染概述

### 客户端渲染

在讲解服务端渲染之前，我们先回顾一下主流浏览器页面的渲染流程，如下步骤：

* 浏览器通过请求得到一个HTML文本。
* 渲染进程解析HTML文本，生成DOM树。
* 解析HTML的同时，如果遇到内联样式或者样式脚本，则下载并生成CSS样式规则（Stytle Rules），若遇到JavaScript脚本，则会下载执行脚本。
* DOM树和CSS样式规则树构建完成之后，渲染进程将两者合并成渲染树（Render Tree）。
* 渲染进程开始对渲染树进行布局，生成布局树（Layout Tree）。
* 渲染进程对布局树进行绘制，显示到页面中。

完整流程如图所示。 ![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/28382747a0244cf19c70a5dacd1d7bcb~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

浏览器请求到这个HTML文件中加载了很多渲染页面需要的JavaScript脚本和CSS样式表，浏览器拿到HTML文件后开始加载脚本和样式表，并且执行脚本，这个时候脚本请求后端服务提供的API，获取数据，获取完成后将数据通过JavaScript脚本动态的将数据渲染到页面中，完成页面显示。这就是客户端渲染主要流程，如图所示。

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3778bf3189cd4921b588c42f3fb78ff4~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

前端团队接管了所有页面渲染的事，后端团队只负责提供所有数据查询与处理的API。

### 客户端渲染VS服务端渲染

服务端渲染的大体流程与客户端渲染有些相似，前端采用Node.js部署了前端服务器。首先是浏览器请求URL，前端服务器接收到URL请求之后，根据不同的URL，前端服务器向后端服务器请求数据，请求完成后，前端服务器会组装一个携带了具体数据的HTML文本，并且返回给浏览器，浏览器得到HTML之后开始渲染页面，同时，浏览器加载并执行JavaScript脚本，给页面上的元素绑定事件，让页面变得可交互，当用户与浏览器页面进行交互，如跳转到下一个页面时，浏览器会执行JavaScript 脚本，向后端服务器请求数据，获取完数据之后再次执行JavaScript代码动态渲染页面，流程如图所示。

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/bf62d7aa6d724487a4bee92c7684b269~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

这样用户在看到页面首屏主要内容时，只和服务器有一个HTTP请求交互，就是获取HTML页面内容，这个内容就是完整的页面内容。当然后续的页面用户交互还是在前端进行的。 这样看下来服务端渲染要比客户端好很多，尤其是首屏的用户体验，服务端渲染和客户端渲染这两种方式主要有以下几个方面的优劣对比：

* SEO支持：服务端渲染可以有效的进行SEO，当爬虫工具请求你的页面地址时，可以拿到完整的HTML内容，便于对网站内容的收录，而客户端渲染爬虫工具拿到的只是一个空的HTML壳子，无法对网站内容进行完整收录。
* 白屏时间：相对于客户端渲染，服务端渲染在浏览器请求URL之后已经得到了一个带有数据的HTML文本，浏览器只需要解析HTML，直接构建DOM树就可以。而客户端渲染，需要先得到一个空的HTML页面，这个时候页面已经进入白屏，之后还需要经过加载并执行 JavaScript、请求后端服务器获取数据、JavaScript 渲染页面几个过程才可以看到最后的页面。特别是在复杂应用中，由于需要加载 JavaScript 脚本，越是复杂的应用，需要加载的 JavaScript 脚本就越多、越大，这会导致应用的首屏加载时间非常长，进而降低了用户体验。
* 服务器运维：除了前端静态资源服务和后端接口服务外，服务端渲染需要额外搭建一套Node.js服务，主要用来请求后端服务的数据和HTML组装，这在一定程度上提升了项目复杂度，同时需要更多的关注服务器的负载均衡及相关运维问题，同时由于代码需要可以在服务端运行，也可以在浏览器端运行，需要兼顾两端代码，提示了代码复杂度。 所以在使用服务端渲染之前，需要开发者考虑投入产出比，比如大部分应用系统都不需要SEO，而且首屏时间并没有非常的慢，如果使用反而小题大做了。

## Vite服务端渲染改造

Vue作为前端框架，除了支持单页面应用的普通的客户端渲染外，还提供了服务端渲染能力，这需要借助Node.js来实现。并且Vue的服务端渲染是代码同构的，即服务端运行的代码和客户端运行的代码可以使用一套，极大提升了服务端渲染的可维护性。

### 同构

我们知道，服务端渲染只负责首屏内容，首屏之后的用户交互还是需要在客户端进行的，那么这就涉及到是否需要单独为首屏写一套代码，而且这套代码是否和不用服务端渲染的代码兼容，这就涉及到代码同构问题。 所谓同构就是采用一套代码，构建双端客户端和服务端逻辑，最大限度的重用代码，不用维护两套代码，如图所示。

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/bbecb04186fa45cdb0577e074c32bf30~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

可以设想一个场景，在使用了服务端渲染的项目中，当需要在首屏内容中添加一张图片时，我们需要在服务端渲染逻辑中将这个图片相关的代码添加上，但是并不是所有情况下都能使用服务端渲染的，因为我们必须为服务端渲染失败时预留容错逻辑，即客户端渲染首屏的这部分逻辑还要保留，所以还需要在客户端渲染逻辑中添加上图片相关的代码，这就造成了需要维护两套代码。而Vue给我们提供的服务端渲染则会避免这种情况发生。 注意，同构并不是一模一样，如果有需要判断平台端的逻辑且有不同的业务表现时，还是需要有这部分代码的。

### 基于Vite服务端渲染概述

如果用一句话来总结Vue.js服务端渲染：基于正常的客户端渲染逻辑编写好代码，然后通过构建来生成客户端渲染使用的文件和服务端渲染使用的文件，并结合Node.js提供服务。这里的构建可以通过Vue Cli也可以通过Vite，本章主要介绍基于Vite下的服务端渲染配置。 服务端渲染的主要步骤概括如下：

* 使用Vite创建正常的客户端渲染项目脚手架。
* 基于服务端渲染逻辑和客户端渲染逻辑改造main.js。
* 跑通正常的客户端渲染开发和生产构建流程。
* 创建Node.js服务端server.js逻辑，结合Vite跑通基于服务端渲染的开发流程。
* 改造Node.js服务端server.js逻辑，跑通服务端渲染生产构建流程。
* 配置package.json里定义的命令，完成改造。

其主要流程如图所示。 ![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/caf236a34a4040a083e75feebb0b2ad0~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

下面，就这些步骤进行逐一讲解。

### 创建Vite项目

其实不需要把服务端渲染想象成一个很复杂的东西，它其实就是对一个正常的客户端Vite项目进行改造集成而已。 首先利用Vite创建一个项目，这里我们可以延用上一章节的myapp项目，其目录结构如图所示。 ![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3618206614b6468abeb424bd3a6d6793~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

其中index.html是单页面项目访问的入口文件， `main.js`本身是用来创建项目Vue的根实例，而改造客户端渲染和服务端渲染逻辑可以从这个文件作为出入口区分。

### 改造main.js

在和 `main.js`同级创建两个文件， `entry-client.js`和 `entry-server.js`分别作为客户端渲染逻辑入口文件和服务端渲染逻辑入口文件，然后改造 `main.js`，使其作为统一的根实例出口，其内容如下代码所示：

```js

import App from './App.vue'
import { createSSRApp } from 'vue'
import { createRouter } from './router'

export function createApp() {

  const app = createSSRApp(App)

  const router = createRouter()
  app.use(router)

  return { app, router }
}
复制代码
```

修改entry-client.js内容，和正常客户端渲染逻辑一样，调用mount方法，将应用挂载到一个DOM元素上，其内容如下代码所示：

```js

import { createApp } from './main'

const { app, router } = createApp()

router.isReady().then(() => {
  app.mount('#app')
})
复制代码
```

上面代码中，唯一的区别就是针对如果所有路由都是异步懒加载的情况，需要等到路由解析完才能进行挂载。然后修改index.html，将引入的 `main.js`改为 `entry-client.js`，如下代码所示：

```html
// index.html
<body>
  <div id="app">div>
  <script type="module" src="/src/entry-client.js">script>
body>
复制代码
```

注意， `entry-client.js`也包括了后续除了首屏之外的前端用户交互逻辑，所以必须引入，vite.config.js暂不需要修改。为了区分服务端渲染和客户端渲染的构建命令，为其添加上client后缀，将package.json的dev命令和build命令改造如下：

```json

"scripts": {
  "dev:client": "vite",
  "build:client": "vite build --outDir dist/client --ssrManifest"
},
复制代码
```

对于 `build:client`命令，--outDir参数为其指定了构建后所产生的文件存放的目录地址，--ssrManifest表示在进行客户端生产构建后，会生成一个ssr-manifest.json文件，这个文件标识了静态资源的映射信息，这样在服务端渲染时，它就可以自动推断并向渲染出来的HTML中注入需要preload/prefetch的资源，并且包括了懒加载的组件所对应的资源。 preload/prefetch两者是以 `<link rel="preload">` 和 `<link rel="prefetch">`作为引入方式，其主要作用和区别如下：

* preload：基本的用法是提前加载资源，告诉浏览器预先请求当前页需要的资源，从而提高这些资源的请求优先级，加载但是不运行，占用浏览器对同一个域名的并发数。
* prefetch：基本用法是浏览器会在空闲的时候，下载资源并缓存起来。当有页面使用的时候，直接从缓存中读取。其实就是把决定是否和什么时间加载这个资源的决定权交给浏览器。

Vite主要利用 `preload`（对于E6 Modules时改为modulepreload），其实是一种优化，当访问首屏时，会提前加载其他页面所需要的资源，这样当打开其他页面时，就会减少等待时间，提升用户体验。正常的客户端渲染出来的HTML默认情况下都会带有这个优化，服务端渲染的HTML则需要上面的ssr-manifest.json才能又对应的优化，对应HTML的这部分内容如图所示。 ![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/03974a59cdcc433da60c61085e253dc2~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

至此，客户端渲染的逻辑都已基本完成，可以正常使用，而且预留了服务端渲染所需要的文件和配置。

### 创建Node.js服务server.js

下面进入服务端渲染的改造过程，服务端渲染核心能力是利用Node.js提供渲染首屏HTML的服务，所以可以利用Express框架来开启一个 `Node.js`服务，同时为了在开发模式下也能使用，将Vite利用中间件的形式集成到Express里，在 `index.html`同级创建 `server.js`，其内容如下代码所示：

```js

const fs = require('fs')
const path = require('path')
const express = require('express')
const { createServer: createViteServer } = require('vite')

async function createServer() {
  const app = express()

  const vite = await createViteServer({
    server: { middlewareMode: 'ssr' }
  })

  app.use(vite.middlewares)

  app.use('*', async (req, res) => {

  })

  app.listen(8887)
}

createServer()
复制代码
```

通过Express提供了一个在端口8887上的Node.js服务，通过浏览器访问 `http://localhost:8887`即可得到首屏的HTML代码，这里vite是ViteDevServer 的一个实例。vite.middlewares是一个Connect实例，它可以在任何一个兼容connect的Node.js框架中被用作一个中间件。下一步是实现 * 处理程序供给服务端渲染的HTML：

```js
app.use('*', async (req, res) => {
  const url = req.originalUrl

  try {

    let template = fs.readFileSync(
      path.resolve(__dirname, 'index.html'),
      'utf-8'
    )

    template = await vite.transformIndexHtml(url, template)

    const { render } = await vite.ssrLoadModule('/src/entry-server.js')

    const appHtml = await render(url)

    const html = template.replace(``, appHtml)

    res.status(200).set({ 'Content-Type': 'text/html' }).end(html)
  } catch (e) {

    vite.ssrFixStacktrace(e)
    console.error(e)
    res.status(500).end(e.message)
  }
})
复制代码
```

对于服务端渲染来说，其核心就是产出首屏HTML，上面的代码就是对浏览器请求就行拦截，然后对HTML进行处理和加工，主要包括了：

* 获取index.html内容，作为初始的HTML模板。
* 在模板基础上添加Vite开发模式的支持带啊吗，主要是热更新HMR相关的JavaScript文件。
* 调用entry-server.js中的方法，得到首屏的HTML字符串。
* 将字符串和HTML模板进行合并替换，构造出完整的HTML内容。
* 通过Express提供的接口返回给浏览器。

上面步骤中，核心点在于首屏的HTML字符串是动态的，还记得之前我们创建的 `entry-server.js`文件吗，他就是产生首屏HTML的主要逻辑文件，其内容如下代码所示：

```js

import { createApp } from "./main"
import { renderToString } from "@vue/server-renderer"

export async function render(){
  const { app, router } = createApp()

  router.push(url)
  await router.isReady()

  const ctx = {};

  const html = await renderToString(app, ctx);

  return { html }
}
复制代码
```

通过 `vue/server-renderer`这个库提供的 `renderToString`方法，将当前状态下的app根实例转换成了对应的HTML代码，这一步很关键，就相当于让浏览器帮忙运行了一下，产生了HTML代码。最后，将得到的HTML字符串替换到之前index.html模板中的 `<!--app-html-->`位置上，就得到了最终的HTML。 通过执行node server.js命令，同时可以把这个命令配置在package.json中，如下代码所示：

```json

"scripts": {
  "dev:ssr": "node ./server.js"
},
复制代码
```

这样Node.js服务就跑起来了，通过浏览器访问 `http://localhost:8887`即可得到首屏的HTML代码，这就完成了开发模式下的服务端渲染。

### 生产模式服务端渲染

生产模式和开发模式的服务端渲染主要区别是去除了Vite相关的配置，直接采用Node.js服务解析 `entry-server.js`并产生首屏HTML代码返回给浏览器即可，同时，添加上了一些资源的preload逻辑，首先需要构造出生产模式的 `entry-server.js`，主要和之前开发模式的 `entry-server.js`代码逻辑一样，只需要添加上需要preload资源的逻辑，其代码如下：

```js

...

const html = await renderToString(app, ctx)

const preloadLinks = renderPreloadLinks(ctx.modules, manifest)
...

function renderPreloadLinks(modules, manifest) {

  let links = ''
  const seen = new Set()
  modules.forEach((id) => {
    const files = manifest[id]
    if (files) {
      files.forEach((file) => {
        if (!seen.has(file)) {
          seen.add(file)
          links += renderPreloadLink(file)
        }
      })
    }
  })

  return links
}

function renderPreloadLink(file) {
  if (file.endsWith('.js')) {
    return `${file}">`
  } else if (file.endsWith('.css')) {
    return `${file}">`
  } else if (file.endsWith('.woff')) {
    return ` ${file}" as="font" type="font/woff" crossorigin>`
  } else if (file.endsWith('.woff2')) {
    return ` ${file}" as="font" type="font/woff2" crossorigin>`
  } else if (file.endsWith('.gif')) {
    return ` ${file}" as="image" type="image/gif">`
  } else if (file.endsWith('.jpg') || file.endsWith('.jpeg')) {
    return ` ${file}" as="image" type="image/jpeg">`
  } else if (file.endsWith('.png')) {
    return ` ${file}" as="image" type="image/png">`
  } else {

    return ''
  }
}
复制代码
```

打印出预加载资源字符串，如图所示。 ![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/73becb4aec674f2585da4d4f008cddc9~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

修改后的 `entry-server.js`后还需要设置构建生产环境的 `entry-server.js`，在package.json中添加命令，其代码如下：

```php

"scripts": {
"build:ssr": "vite build --outDir dist/server --ssr src/entry-server.js"
}
``
注意使用 --ssr 标志表明这将会是一个服务端构建，同时需要指定对应文件的入口。构建完成后，`entry-server.js`会在server.js中被调用，同时传入对应的参数，来获得首屏的HTML。修改`server.js`，添加生产模式相关逻辑，其部分代码如下：

```javascript

...

isProd = process.env.NODE_ENV === 'production'
...

const indexProd = isProd ? fs.readFileSync(resolve('dist/client/index.html'), 'utf-8') : ''

const manifest = require('./dist/client/ssr-manifest.json')
...

if (isProd) {

  app.use(require('compression')())

  app.use(
    require('serve-static')(resolve('dist/client'), {
      index: false
    })
  )
}

if (isProd) {
  render = require('./dist/server/entry-server.js').render

  const [appHtml, preloadLinks] = await render(url, manifest)
const html = template
  .replace(``, preloadLinks)
  .replace(``, appHtml)
}
...

复制代码
```

总结下来，生产模式服务端构建主要对 `server.js`做了以下事情：

* 将Vite开发服务器的创建和所有使用都移到开发模式条件分支后面，然后添加Express静态文件服务中间件来服务dist/client中的文件。
* 使用dist/client/index.html作为模板，而不是根目录的 index.html，因为前者包含了到客户端构建的正确资源链接。
* 使用require('./dist/server/entry-server.js')，而不是vite.ssrLoadModule('/src/entry-server.js')（前者是 SSR 构建后的最终结果）。
* 将preload对应的字符串替换到index.html中位置上。

当执行 `npm run build:ssr`时，就可以在生产模式下将服务跑起来，生产模式下并不会启动端口服务，只是将生产用的资源打包好，当全部准备就绪时，我们就可以访问完整的生产模式下的服务端渲染。

### 优化package.json命令，完成改造

修改package.json，并结合之前添加的命令，其完整代码如下：

```json

"scripts": {

  "dev:client": "vite",

  "build:client": "vite build --outDir dist/client --ssrManifest",

  "dev:ssr":"node ./server.js",

  "build:ssr": "vite build --ssr src/entry-server.js --outDir dist/server",

  "build": "npm run build:client && npm run build:ssr",

  "serve": "cross-env NODE_ENV=production node server",
}
复制代码
```

执行 `npm run serve`，然后浏览器访问 `http://localhost:8887`即可访问到生产模式下的服务端渲染出来的页面，如果需要部署生产服务器，将这整个项目部署到服务器即可。 至此，整个服务端渲染完成改造，项目目录结构如图所示。

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fbc5e3f15a4b4bcd944ead09aafbfb21~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

## 编写通用的代码

尽管代码同构可以避免维护两个平台的代码，但是我们在编写含有服务端渲染的项目代码时，也需要注意以及遵循一些原则，从而避免bug的产生。

### 服务端的数据响应性

在只有客户端的应用中，每个用户都在各自的浏览器中使用一个干净的应用实例。对于服务端渲染来说我们也希望如此：每个请求应该拥有一个干净的相互隔离的应用实例，以避免跨请求的状态污染。 服务端渲染只负责提供首屏的静态HTML，这可能会从服务器"预获取"一些数据，这意味着应用状态在我们开始渲染之前已经被解析好了。所以数据响应式相关的特性在服务端是不必要的，因此它默认是不开启的，禁用数据响应性也避免了将数据转换为响应式对象的性能损耗。如下代码所示：

```js

<span class="hljs-keyword">const</span> count = <span class="hljs-title function_">ref</span>(<span class="hljs-number">0</span>)

<span class="hljs-built_in">setTimeout</span>(<span class="hljs-function">()=></span>{
  count.<span class="hljs-property">value</span> = <span class="hljs-number">2</span>
},<span class="hljs-number">1000</span>)

复制代码
```

上面代码中，count的响应式在服务端渲染时将不会生效。

### 组件生命周期钩子

因为服务端渲染没有响应式以及动态更新，所以针对组件来说，唯一会在服务端渲染过程中被调用的生命周期钩子是 `beforeCreate`和 `created`。这意味着其它生命周期钩子例如 `beforeMount`或 `mounted`只会在客户端被执行。 所以，如果首屏页面需要一些数据是从后端服务获取的，那么这部分逻辑应该放在 `created`中，如下代码所示：

```js

<span class="hljs-keyword">import</span> axios <span class="hljs-keyword">from</span> <span class="hljs-string">'axios'</span>

<span class="hljs-keyword">export</span> <span class="hljs-keyword">default</span> {
  <span class="hljs-keyword">async</span> <span class="hljs-title function_">created</span>(<span class="hljs-params"></span>){

<span class="hljs-keyword">const</span> data = <span class="hljs-keyword">await</span> axios.<span class="hljs-title function_">get</span>(<span class="hljs-string">`/api/foo/1`</span>)
  }
}

复制代码
```

注意， `beforeCreate`和 `created`相关逻辑会在服务端渲染时执行，当页面被浏览器打开时，客户端也会执行这里的逻辑，由于此刻时间内数据变化的可能性非常小，所以就是客户端又渲染了一遍这里的逻辑，但是页面基本不会改变。当然，我们也可以通过环境标志位 `import.meta.env.SSR`来规定一些逻辑只在服务端渲染时执行，如下代码所示：

```js
export default {
  async created(){

    if (import.meta.env.SSR) {
      const data = await axios.get(`/api/post/1`)
    }
  }
}
复制代码
```

另外，在服务端渲染时，注意避免代码在 `beforeCreate`或 `created`中产生全局的副作用，例如通过 `setInterval`设置定时器。在只有客户端的代码中我们可以设置定时器然后在 `beforeUnmount`或 `unmounted`时销毁。然而，因为销毁相关的钩子在服务端渲染中不会被调用，这些定时器就会永久地保留下来。为了避免这件事，把有副作用的代码移至 `beforeMount`或 `mounted`里面。 如果项目中使用了Vuex来获取数据，那么服务端渲染中提前设置Store的内容，在客户端渲染时就可以拿到，关于Vuex的服务端渲染这部分内容将会在后面的实战项目章节中深入讲解。

### 访问特定平台的API

当代码可能会在浏览器和Node.js服务端都运行时，就需要考虑特有平台API的使用，因此如果代码直接使用只存在于浏览器的全局变量例如 `window`或 `document`，它们会在Node.js里执行的时候抛出错误，反之亦然。 基于此，在编码时就要做好平台的逻辑判断，而对于一些可以共享平台的API，例如axios，既可以在服务端使用也可以在浏览器端使用类似的库，则更加推荐使用。 举一个例子，对于服务端渲染来说，由于采用的Node.js环境，所以需要对于 `window`对象做兼容处理，这里推荐使用 `jsdom`库：

```css
npm install jsdom --save
复制代码
```

然后可以将jsdom相关逻辑添加到 `server.js`里面，如下代码所示：

```js
const jsdom = require('jsdom')
const { JSDOM } = jsdom

const resourceLoader = new jsdom.ResourceLoader({
  userAgent: "Mozilla/5.0 (iPhone; CPU iPhone OS 13_2_3 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/13.0.3 Mobile/15E148 Safari/604.1",
});
const dom = new JSDOM('', {
  url:'https://app.nihaoshijie.com.cn/index.html',
  resources: resourceLoader
});

global.window = dom.window
global.document = window.document
global.navigator = window.navigator
window.nodeis = true
复制代码
```

这样，就可以在Node.js里使用模拟的window对象了，不仅扩展了一些功能，同时可以防止真正需要在浏览器使用的window代码，在Node.js端使用而报错的场景出现。

## 预渲染

预渲染（Server Side General）简称SSG，如果服务端渲染的数据是完全静态的，即不依赖于不同用户访问看到的内容不一样，那么可以采用预渲染来实现服务端渲染的优势，即直接通过前端构建来生成首屏的静态页面资源，不依赖于后端Node.js服务，用户通过浏览器访问时，直接打开预先生成好的HTML页面，这也可以达到利于SEO，首屏提速等优化，并且不需要Node.js服务，减少后端运维成本。 在server.js同级目录新增prerender.js，其内容如下：

```js

const fs = require('fs')
const path = require('path')

const toAbsolute = (p) => path.resolve(__dirname, p)

const manifest = require('./dist/static/ssr-manifest.json')

const template = fs.readFileSync(toAbsolute('dist/static/index.html'), 'utf-8')

const { render } = require('./dist/server/entry-server.js')

;(async () => {

  let url = '/'

  const [appHtml, preloadLinks] = await render(url, manifest)

  const html = template
      .replace(``, preloadLinks)
      .replace(``, appHtml)

  const filePath = `dist/static${url === '/' ? '/index' : url}.html`
  fs.writeFileSync(toAbsolute(filePath), html)

  fs.unlinkSync(toAbsolute('dist/static/ssr-manifest.json'))
})()
复制代码
```

执行node `prerender.js`即可在 `dist/static`目录下生成预渲染的首屏 `index.html`文件，这个文件不是单独的空壳子，而是含有首屏内容的静态HTML页面，如下代码所示。

```xml
...

<body>
  <div id="app"><h1 data-v-485b7ba6>首屏prerender内容h1>div>
body>
...

复制代码
```

修改package.json，完善命令：

```json
"scripts": {

  "prerender": "vite build --ssrManifest --outDir dist/static && npm run build:ssr && node prerender"
},
复制代码
```

预渲染不适用经常变化的数据，比如说股票代码网站，天气预报网站。因为此时的数据是动态的，而预渲染时必须要求事先生成好页面内容，这就无法保证这些数据的实时性。

## Nuxt.js介绍

Nuxt.js 是一个基于 Vue.js 的通用应用框架，一个用于Vue.js 开发SSR应用的一站式解决方案。它的优点是将原来几个配置文件要完成的内容，都整合在了一个nuxt.config.js，封装与扩展性完美的契合。 Nuxt.js默认集成了Vue Router，Vuex，SSR，构建采用Webpack处理代码的自动化构建工作（如打包、代码分层、压缩等等）。和Express框架相比，两者都是采用Node.js服务提供传统的服务端渲染功能，但是Nuxt.js主要和Vue.js结合，更加适合熟悉Vue API的前端开发使用，你可以把Nuxt.js想象成在服务端使用的Vue.js。 Nuxt.js应用一个完整的服务器请求到渲染的过程如图所示。 ![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ff96efffa58246af80428eb2750a7f6b~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

Nuxt.js应用的大部分页面都是采用服务端渲染（而非Vue.js服务端渲染更加专注首屏内容），并且这些渲染功能都服务端进行处理，同时也有一部分组件生命周期在服务端和客户端运行（主要是beforeCreate和created），当一个请求进入Nuxt.js时，进行容器的初始化，然后执行一些中间件逻辑包括组件到DOM的生成和布局等等，然后经过一些校验，最后进入asyncData方法，这个方法是Nuxt.js在Vue组件上扩展的用来获取异步数据的方法，使得可以在设置组件的数据之前能够异步进行处理，最终将组件和DOM和数据结合生成HTML返回给浏览器进行渲染。当客户端浏览器渲染时，也会执行Nuxt.js的beforeCreated和created方法。 总结下来，如果项目完全依赖服务端渲染，并且相对复杂，再加上比较熟悉Node.js后端相关的技术，采取Nuxt.js来开发项目是一个不错的选择。

## 本章小结

在本章中，讲解了Vue.js服务端渲染相关知识，主要内容包括：服务端渲染概述、Vite服务端渲染改造、如何编写通用代码以及预渲染相关知识，最后简单介绍的基于Vue的服务端渲染框架Nuxt.js。 服务端渲染在一定程度上优化了页面性能，提升了用户体验，但是也提升了项目的整体复杂度，但笔者认为了解服务端渲染有助于提升前端工程师的综合能力，因为除了了解Vue在客户端浏览器运行的机制，也可以学习Vue与Node.js结合的后端知识，同时还能掌握一些服务器运维知识，从而得到整体的锻炼和提升。

附改造案例源码：[github.com/lvming68160...](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Flvming6816077%2Fdoubanmovie "https://github.com/lvming6816077/doubanmovie")
