---
link: https://juejin.cn/post/7063795725296992270
title: 如何在vue中实现SSR服务端渲染？
description: SSR意为服务端渲染，指由服务侧完成页面的 HTML 结构拼接的页面处理技术，发送到浏览器，然后为其绑定状态与事件，成为完全可
keywords: Vue.js
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2022-02-12T12:19:22.000Z
publisher: 稀土掘金
stats: paragraph=119 sentences=105, words=917
---
## 一、SSR是什么

`Server-Side Rendering` 我们称其为 `SSR`，意为服务端渲染

指由服务侧完成页面的 `HTML` 结构拼接的页面处理技术，发送到浏览器，然后为其绑定状态与事件，成为完全可交互页面的过程

先来看看 `Web`3个阶段的发展史：

* 传统服务端渲染SSR
* 单页面应用SPA
* 服务端渲染SSR

### **传统web开发**

网页内容在服务端渲染完成，⼀次性传输到浏览器

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e336f179073e4ea69e7950de051ca0e4~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp?)

打开页面查看源码，浏览器拿到的是全部的 `dom`结构

### **单页应用SPA**

单页应用优秀的用户体验，使其逐渐成为主流，页面内容由 `JS`渲染出来，这种方式称为客户端渲染

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ff16bcf343334c329f5aaeaeba47c490~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp?)

打开页面查看源码，浏览器拿到的仅有宿主元素 `#app`，并没有内容

### 服务端渲染SSR

`SSR`解决方案，后端渲染出完整的首屏的 `dom`结构返回，前端拿到的内容包括首屏及完整 `spa`结构，应用激活后依然按照 `spa`方式运行

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1f9064444bda4da8b910d4e0b096e7a2~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp?)

看完前端发展，我们再看看 `Vue`官方对 `SSR`的解释：

> Vue.js 是构建客户端应用程序的框架。默认情况下，可以在浏览器中输出 Vue 组件，进行生成 DOM 和操作 DOM。然而，也可以将同一个组件渲染为服务器端的 HTML 字符串，将它们直接发送到浏览器，最后将这些静态标记"激活"为客户端上完全可交互的应用程序
服务器渲染的 Vue.js 应用程序也可以被认为是"同构"或"通用"，因为应用程序的大部分代码都可以在服务器和客户端上运行

我们从上门解释得到以下结论：

* `Vue SSR`是一个在 `SPA`上进行改良的服务端渲染
* 通过 `Vue SSR`渲染的页面，需要在客户端激活才能实现交互
* `Vue SSR`将包含两部分：服务端渲染的首屏，包含交互的 `SPA`

## 二、解决了什么

SSR主要解决了以下两种问题：

* seo：搜索引擎优先爬取页面 `HTML`结构，使用 `ssr`时，服务端已经生成了和业务想关联的 `HTML`，有利于 `seo`
* 首屏呈现渲染：用户无需等待页面所有 `js`加载完成就可以看到页面视图（压力来到了服务器，所以需要权衡哪些用服务端渲染，哪些交给客户端）

但是使用 `SSR`同样存在以下的缺点：

* 复杂度：整个项目的复杂度
* 库的支持性，代码兼容
* 性能问题
  - 每个请求都是 `n`个实例的创建，不然会污染，消耗会变得很大
  - 缓存 `node serve&#xA0;`、 `ngin`x判断当前用户有没有过期，如果没过期的话就缓存，用刚刚的结果。
  - 降级：监控 `cpu`、内存占用过多，就 `spa`，返回单个的壳
* 服务器负载变大，相对于前后端分离务器只需要提供静态资源来说，服务器负载更大，所以要慎重使用

所以在我们选择是否使用 `SSR`前，我们需要慎重问问自己这些问题：

1. 需要 `SEO`的页面是否只是少数几个，这些是否可以使用预渲染（Prerender SPA Plugin）实现
2. 首屏的请求响应逻辑是否复杂，数据返回是否大量且缓慢

## 三、如何实现

对于同构开发，我们依然使用 `webpack`打包，我们要解决两个问题：服务端首屏渲染和客户端激活

这里需要生成一个服务器 `bundle`文件用于服务端首屏渲染和一个客户端 `bundle`文件用于客户端激活

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0f946172c7ee4fb3bcb660414b640fcb~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp?)

代码结构 除了两个不同入口之外，其他结构和之前 `vue`应用完全相同

```js
src
├── router
├────── index.js # 路由声明
├── store
├────── index.js # 全局状态
├── main.js # ⽤于创建vue实例
├── entry-client.js # 客户端⼊⼝，⽤于静态内容"激活"
└── entry-server.js # 服务端⼊⼝，⽤于⾸屏内容渲染
复制代码
```

路由配置

```js
import Vue from "vue"; import Router from "vue-router"; Vue.use(Router);

export function createRouter(){
    return new Router({
    mode: 'history',
    routes: [

        { path: "/", component: { render: h => h('div', 'index page') } },
        { path: "/detail", component: { render: h => h('div', 'detail page') }}
    ]
  });
 }
复制代码
```

主文件main.js

跟之前不同，主文件是负责创建 `vue`实例的工厂，每次请求均会有独立的 `vue`实例创建

```js
import Vue from "vue";
import App from "./App.vue";
import { createRouter } from "./router";

export function createApp(context) {
    const router = createRouter();
    const app = new Vue({ router, context, render: h => h(App) });
    return { app, router };
  }
复制代码
```

编写服务端入口 `src/entry-server.js`

它的任务是创建 `Vue`实例并根据传入 `url`指定首屏

```js
import { createApp } from "./main";

export default context => {

    return new Promise((resolve, reject) => {
        const { app, router } = createApp(context);

        router.push(context.url);

        router.onReady(() => {
        resolve(app);
        }, reject);
    });
};
复制代码
```

编写客户端入口 `entry-client.js`

客户端入口只需创建 `vue`实例并执行挂载，这⼀步称为激活

```js
import { createApp } from "./main";

const { app, router } = createApp();

router.onReady(() => { app.$mount("#app"); });
复制代码
```

对 `webpack`进行配置

安装依赖

`npm install webpack-node-externals lodash.merge -D`

对 `vue.config.js`进行配置

```js

const VueSSRServerPlugin = require("vue-server-renderer/server-plugin");
const VueSSRClientPlugin = require("vue-server-renderer/client-plugin");
const nodeExternals = require("webpack-node-externals");
const merge = require("lodash.merge");

const TARGET_NODE = process.env.WEBPACK_TARGET === "node";
const target = TARGET_NODE ? "server" : "client";
module.exports = {
    css: {
        extract: false
    },
    outputDir: './dist/'+target,
    configureWebpack: () => ({

        entry: `./src/entry-${target}.js`,

        devtool: 'source-map',

        target: TARGET_NODE ? "node" : "web",

        node: TARGET_NODE ? undefined : false,
        output: {

            libraryTarget: TARGET_NODE ? "commonjs2" : undefined
        },

        externals: TARGET_NODE
        ? nodeExternals({

            whitelist: [/\.css$/]
        })
        : undefined,
        optimization: {
            splitChunks: undefined
        },

        plugins: [TARGET_NODE ? new VueSSRServerPlugin() : new
                  VueSSRClientPlugin()]
    }),
    chainWebpack: config => {

        if (TARGET_NODE) {
            config.optimization.delete('splitChunks')
        }

        config.module
            .rule("vue")
            .use("vue-loader")
            .tap(options => {
            merge(options, {
                optimizeSSR: false
            });
        });
    }
};
复制代码
```

对脚本进行配置，安装依赖

`npm i cross-env -D`

```js

"scripts": {
 "build:client": "vue-cli-service build",
 "build:server": "cross-env WEBPACK_TARGET=node vue-cli-service build",
 "build": "npm run build:server && npm run build:client"
}
复制代码
```

执行打包： `npm run build`

最后修改宿主文件 `/public/index.html`

```html

<html lang="en">
    <head>
        <meta charset="utf-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width,initial-scale=1.0">
        <title>Documenttitle>
    head>
    <body>

    body>
html>
复制代码
```

> 是服务端渲染入口位置，注意不能为了好看而在前后加空格

安装 `vuex`

`npm install -S vuex`

创建 `vuex`工厂函数

```js
import Vue from 'vue'
import Vuex from 'vuex'
Vue.use(Vuex)
export function createStore () {
    return new Vuex.Store({
        state: {
            count:108
        },
        mutations: {
            add(state){
                state.count += 1;
            }
        }
    })
}
复制代码
```

在 `main.js`文件中挂载 `store`

```js
import { createStore } from './store'
export function createApp (context) {

    const store = createStore()
    const app = new Vue({
        store,
        render: h => h(App)
    })
    return { app, router, store }
}
复制代码
```

服务器端渲染的是应用程序的"快照"，如果应用依赖于⼀些异步数据，那么在开始渲染之前，需要先预取和解析好这些数据

在 `store`进行一步数据获取

```js
export function createStore() {
    return new Vuex.Store({
        mutations: {

            init(state, count) {
                state.count = count;
            },
        },
        actions: {

            getCount({ commit }) {
                return new Promise(resolve => {
                    setTimeout(() => {
                        commit("init", Math.random() * 100);
                        resolve();
                    }, 1000);
                });
            },
        },
    });
}
复制代码
```

组件中的数据预取逻辑

```js
export default {
    asyncData({ store, route }) {

        return store.dispatch("getCount");
    }
};
复制代码
```

服务端数据预取， `entry-server.js`

```js
import { createApp } from "./app";
export default context => {
    return new Promise((resolve, reject) => {

        const { app, router, store } = createApp(context);
        router.push(context.url);
        router.onReady(() => {

            const matchedComponents = router.getMatchedComponents();

            if (!matchedComponents.length) {
                return reject({ code: 404 });
            }

            Promise.all(
                matchedComponents.map(Component => {
                    if (Component.asyncData) {
                        return Component.asyncData({
                            store,
                            route: router.currentRoute,
                        });
                    }
                }),
            )
                .then(() => {

                context.state = store.state;

                resolve(app);
            })
                .catch(reject);
        }, reject);
    });
};
复制代码
```

客户端在挂载到应用程序之前， `store` 就应该获取到状态， `entry-client.js`

```js

const { app, router, store } = createApp();

if (window.__INITIAL_STATE__) {
    store.replaceState(window.__INITIAL_STATE__);
}
复制代码
```

客户端数据预取处理， `main.js`

```js
Vue.mixin({
    beforeMount() {
        const { asyncData } = this.$options;
        if (asyncData) {

            this.dataPromise = asyncData({
                store: this.$store,
                route: this.$route,
            });
        }
    },
});
复制代码
```

修改服务器启动文件

```js

const resolve = dir => require('path').resolve(__dirname, dir)

app.use(express.static(resolve('../dist/client'), {index: false}))

const { createBundleRenderer } = require("vue-server-renderer");

const bundle = resolve("../dist/server/vue-ssr-server-bundle.json");

const renderer = createBundleRenderer(bundle, {
    runInNewContext: false,
    template: require('fs').readFileSync(resolve("../public/index.html"), "utf8"),
    clientManifest: require(resolve("../dist/client/vue-ssr-clientmanifest.json"))
});
app.get('*', async (req,res)=>{

    const context = {
        title:'ssr test',
        url:req.url
    }
    const html = await renderer.renderToString(context);
    res.send(html)
})
复制代码
```

### 小结

* 使用 `ssr`不存在单例模式，每次用户请求都会创建一个新的 `vue`实例
* 实现 `ssr`需要实现服务端首屏渲染和客户端激活
* 服务端异步获取数据 `asyncData`可以分为首屏异步获取和切换组件获取
  - 首屏异步获取数据，在服务端预渲染的时候就应该已经完成
  - 切换组件通过 `mixin`混入，在 `beforeMount`钩子完成数据获取
