---
link: https://juejin.cn/post/7037321595311882277
title: vue3 服务端渲染（SSR）爬坑记、保姆级示例
description: 前言 vue2 SSR 已经被不少大佬讲烂了，vue3 SSR 除了官网 几乎没搜到其他文章，而且官网上也没讲 vuex 方面，那么我就来凑个数吧，相信大家都知道 SSR 是干嘛的，前世今生也不多说了
keywords: Vue.js
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2021-12-03T04:02:50.000Z
publisher: 稀土掘金
stats: paragraph=110 sentences=292, words=1517
---
# 前言

vue2 SSR 已经被不少大佬讲烂了，vue3 SSR 除了[官网](https://link.juejin.cn?target=https%3A%2F%2Fv3.cn.vuejs.org%2Fguide%2Fssr%2Fintroduction.html "https://v3.cn.vuejs.org/guide/ssr/introduction.html") 几乎没搜到其他文章，而且官网上也没讲 vuex 方面，那么我就来凑个数吧，相信大家都知道 SSR 是干嘛的，前世今生也不多说了，这儿主要结合官网把 vue3 SSR 的配置和使用过程及遇到的坑给大家排除下。

# 依赖

首先执行以下命令把依赖装好

```java
npm install /server-renderer
npm install webpack-manifest-plugin webpack-node-externals webpack lru-cache -D
```

```arduino
npm install express
```

```kotlin
npm install cross-env -D
"build:server": "set SSR=1 && vue-cli-service build --dest dist/server",
但是这个 set SSR=1 在 linux 下不生效，所以还是得用 cross-env

官网 package.json 打包脚本如下
"build:client": "vue-cli-service build --dest dist/client",
"build:server": "cross-env SSR=1 vue-cli-service build --dest dist/server",
"build:ssr": "npm run build:client && npm run build:server",
```

# 路由

每个请求都需要有一个干净的路由实例，所以得提供一个路由的工厂函数。 例如下列代码

```js
import { createRouter, RouteRecordRaw } from 'vue-router';
import Home from '../views/Home/index.vue';

export const routes: Array<RouteRecordRaw> = [
  {
    path: '/',
    name: 'Home',
    component: Home,
    meta: {

      depth: 1
    }
  },
  {
    path: '/popular-now/:link',
    name: 'PopularNow',
    component: () => import( '../views/PopularNow/index.vue'),
    props: true,
    meta: {
      title: '我一直不明白为什么地球要',

      depth: 1
    }
  },
  {
    path: '/subscription',
    name: 'Subscription',
    component: () => import( '../views/Subscription/index.vue'),
    meta: {
      title: '绕着太阳转',

      depth: 1
    }
  },
  {
    path: '/mediaLibrary',
    name: 'MediaLibrary',
    component: () => import( '../views/MediaLibrary/index.vue'),
    meta: {
      title: '直到遇见你',
      depth: 1
    }
  },
  {
    path: '/history',
    name: 'History',
    component: () => import( '../views/History/index.vue'),
    meta: {
      title: '我才蓦然明白',
      depth: 1
    }
  },
  {
    path: '/search',
    name: 'Search',
    component: () => import( '../views/Search/index.vue'),
    props: route => ({ keywords: route.query.q }),
    meta: {
      title: '也许是因为有了你',

      depth: 1
    }
  },
  {
    path: '/watch/:mvid',
    name: 'Watch',
    component: () => import( '../views/Watch/index.vue'),
    props: true,
    meta: {
      title: '这世界才有了',
      depth: 2
    }
  },
  {
    path: '/:pathMatch(.*)*',
    name: 'NotFound',
    component: () => import( '../views/Notfound/index.vue'),
    meta: {
      title: '春夏秋冬',
      depth: 2
    }
  }
]

export default function (history: any) {
  return createRouter({
    history,
    routes
  })
}

```

```ini
// keepAlive: true
keepAlive 是注释掉的，SSR 并不支持  组件缓存，如果你原本有如下这样的代码
v-slot="{ Component }">

      class="[{ 'router-paper-full': !isShowSidebar }, 'router-paper']"
        :is="Component"
        v-if="$route.meta.keepAlive"
        :key="$route.name"
      />

    class="[{ 'router-paper-full': !isShowSidebar }, 'router-paper']"
      :is="Component"
      v-if="!$route.meta.keepAlive"
    />

要支持 SSR 请将  代码块干掉，不然用户在切换页面时会报如下错误，意思就是新节点插入位置错误
```

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/88ef5e7a490a4ca7998226ff1af41dcd~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?) 对啦，SSR 也不支持指令，所以你有用到指令的话，请将指令拆掉，改为 util 方式实现，例如：

```typescript

const loadScrollEvent = function (el: HTMLElement, binding: any) {
    window.addEventListener(
        "scroll",
        function () {
            const temp: any = el.lastElementChild?.previousElementSibling;
            if (temp) {
                const a = temp.offsetTop + temp.offsetHeight;

                const b =
                    document.documentElement.clientHeight +
                    (el.parentElement?.parentElement?.scrollTop || 0);

                if (binding.value.commandAutoLoading.isCommand && a - 100 value.autoLoading();
                }
            }
        },
        true
    );
}
要使用的地方把 dom 放进去就行啦
 loadScrollEvent(this.$refs.commentList as any, {
   value: {
      commandAutoLoading: this.commentListAutoLoading,
      autoLoading: this.commentListLoading,
   },
 });
```

# vuex

依旧要提供一个工厂函数，例如：

```typescript
import { createStore } from 'vuex';
import HistoryMv from '../store/modules/HistoryMv';
import Subscription from '../store/modules/Subscription';
import LikedMv from '../store/modules/LikedMv';
import TopProgressBar from '../store/modules/TopProgressBar';
import HomeMv from '../store/modules/HomeMv';
import PopularNowMv from '../store/modules/PopularNowMv';
import CloudSearchMv from '../store/modules/CloudSearchMv';
import WatchMv from '../store/modules/WatchMv';

export default function () {

  HistoryMv.state.historyMvList = [];
  Subscription.state.artistList = [];
  LikedMv.state.likedMvList = [];
  TopProgressBar.state.start = false;
  HomeMv.state.mvList = [];
  PopularNowMv.state.mvList = [];
  CloudSearchMv.state.mvList = [];
  WatchMv.state = {
    simiMvList: [],
    artistMvList: [],
    likedCount: 0,
    mv: null,
    artistDetail: null,
    commentMv: {
      hotComments: [],
      comments: [],
      total: 0,
      more: false,
    },
    mvUrl: '',
    mvUrlErrorMsg: ''
  }

  const store = createStore({
    modules: {
      HistoryMv,
      Subscription,
      LikedMv,
      TopProgressBar,
      HomeMv,
      PopularNowMv,
      CloudSearchMv,
      WatchMv
    }
  })

  try {
    if (window && (window as any).__INITIAL_STATE__) {
      store.replaceState((window as any).__INITIAL_STATE__);
    }
  } catch (error) {
    console.log('有一个错', error.message);
  }

  return store;

}

```

**注意啦~~~**

如果用到 vuex 的命名空间，一定要重置各项数据，否者会共享这些命名空间下的 state ！

并且在这个工厂函数中去获取了 node 服务器传过来的 (window as any). **INITIAL_STATE** 里面的 store 数据。

# entry-client.js

客户端入口，该干啥就干啥啦，例如：

```js
import { createSSRApp } from 'vue'
import { createWebHistory } from 'vue-router'
import App from './App.vue'
import createRouter from './router-ssr'
import createStore from './store-ssr'
import { filters } from './share/filters';
import './assets/fonts/iconfont/iconfont.css';
import './assets/fonts/iconfont/iconfont.js';

const app = createSSRApp(App)

const router = createRouter(createWebHistory('loutube'))
const store = createStore()

app.config.globalProperties.$filters = filters;

app.use(router).use(store)

router.isReady().then(() => {
    app.mount('#app')
})
```

# entry-server.js

服务端入口，例如：

```js
import { createSSRApp } from 'vue'
import { createMemoryHistory } from 'vue-router'
import App from './App.vue'
import createRouter from './router-ssr'
import createStore from './store-ssr'
import { filters } from './share/filters';
import './assets/fonts/iconfont/iconfont.css';

导入的话会报错 window 或 document 找不到之类的错误。

export default function () {
    const app = createSSRApp(App)

    const router = createRouter(createMemoryHistory('loutube'))
    const store = createStore()

    app.config.globalProperties.$filters = filters;

    app.use(router).use(store)

    return {
      app,
      router,
      store
    }
}
```

# vue.config.js

打包配置文件，例如：

```js
const { WebpackManifestPlugin } = require('webpack-manifest-plugin')
const nodeExternals = require('webpack-node-externals')
const webpack = require('webpack')

const path = require('path')

if (process.env.COMMON) {
  return console.log('退出 vue.config.js -----');
}

module.exports = {
  chainWebpack: webpackConfig => {

    webpackConfig.module.rule('vue').uses.delete('cache-loader')
    webpackConfig.module.rule('js').uses.delete('cache-loader')
    webpackConfig.module.rule('ts').uses.delete('cache-loader')
    webpackConfig.module.rule('tsx').uses.delete('cache-loader')

    if (!process.env.SSR) {

      webpackConfig
        .entry('app')
        .clear()
        .add('./src/entry-client.js')
      return
    }

    webpackConfig
      .entry('app')
      .clear()
      .add('./src/entry-server.js')

    webpackConfig.target('node')

    webpackConfig.output.libraryTarget('commonjs2')

    webpackConfig
      .plugin('manifest')
      .use(new WebpackManifestPlugin({ fileName: 'ssr-manifest.json' }))

    webpackConfig.externals(nodeExternals({ allowlist: /\.(css|vue)$/ }))

    webpackConfig.optimization.splitChunks(false).minimize(false)

    webpackConfig.plugins.delete('preload')
    webpackConfig.plugins.delete('prefetch')
    webpackConfig.plugins.delete('progress')
    webpackConfig.plugins.delete('friendly-errors')

    webpackConfig.plugin('limit').use(
      new webpack.optimize.LimitChunkCountPlugin({
        maxChunks: 1
      })
    )

  }
}
```

**注意啦~~~**

如果打包过程中报错却未显示错误信息，可以使用 empty-webpack-build-detail-plugin 插件记录错误日志，比如你在 SSR 项目中坚持使用指令，那么它会狠狠的不给你错误信息，根本就不给反馈好吧。。。╮(╯▽╰)╭

# 同构

```php

const path = require('path')
const express = require('express')
const fs = require('fs')
const { renderToString } = require('@vue/server-renderer')
const manifest = require('../dist/server/ssr-manifest.json')

const server = express()

const appPath = path.join(__dirname, '../dist', 'server', manifest['app.js'])
const createApp = require(appPath).default

server.use('/img', express.static(path.join(__dirname, '../dist/client', 'img')))
server.use('/js', express.static(path.join(__dirname, '../dist/client', 'js')))
server.use('/css', express.static(path.join(__dirname, '../dist/client', 'css')))
server.use(
  '/favicon.ico',
  express.static(path.join(__dirname, '../dist/client', 'favicon.ico'))
)

const LRU = require('lru-cache')
const microCache = new LRU({
  max: 100,
  maxAge: 1000 * 60 * 60
})

server.get('*', async (req, res) => {
  const hit = microCache.get(req.url)
  if (hit) {
    return res.send(hit)
  }

  const { app, router, store } = createApp()

  await router.push(req.url.replace('/loutube', ''))
  await router.isReady()

  const matchedComponents = router.currentRoute.value.matched;

  Promise.all(matchedComponents.map(Component => {
    if (Component.components.default.methods.asyncData) {
      return Component.components.default.methods.asyncData(
        store,
        router.currentRoute
      );
    }
  })).then(async () => {
    const appContent = await renderToString(app);

    fs.readFile(path.join(__dirname, '../dist/client/index.html'), (err, html) => {
      if (err) {
        throw err
      }

      html = html
        .toString()
        .replace('', `"app">${appContent}`)
        .replace(
          '',
          `"application/javascript"</span>>window.__INITIAL_STATE__=${JSON.<span class="hljs-title function_ invoke__">stringify</span>(store.state)}`
        )
      res.setHeader('Content-Type', 'text/html')
      res.send(html)
      microCache.set(req.url, html)
    })
  }).catch(error => {
    console.error(error);
    res.sendStatus(500)
  });
})

server.listen(8080)
```

**注意啦~~~**

这里要自己主动将 store 放进 html 里

```xml
.replace(
      'script>',
      `script><script type="application/javascript">window.__INITIAL_STATE__=${JSON.stringify(store.state)}script>`
)
```

## 组件 asyncData()

```typescript

asyncData(store: any, route: any) {
    return store.dispatch("HomeMv/getMvListRequest", {
      limit: 24,
      loadMoreCount: 0,
    });
}

asyncData(store: any, route: any) {
    return Promise.all([
      this.getMvUrl(store, route),
      this.getMvLikedCount(store, route),
      this.getMvDetail(store, route).then(async () => {
        await this.getArtistDetail(store, route);
        await this.getArtistMvList(store, route);
        await store.dispatch("HistoryMv/addHistoryMv", store.state.WatchMv.mv);
      }),
      this.getCommentMvList(store, route),
    ]).then();
}

async getMvUrl(store?: any, route?: any) {
    store = store || this.$store;

    const mvid = this.mvUrlState
      ? this.mvUrlState.queryParams.mvid
      : route.value.params.mvid;

    await store.dispatch("WatchMv/getMvUrlRequest", {
      id: mvid,
    });
}
```

promise 这一块需要你们自己去研究哈，这还不掌握，内卷的资格都没有。。。╮(╯▽╰)╭

# 最后

也许你的各个文件路径跟我展示的示例代码不一样而报错，请你仔细排查下。。。 对啦，在列下我的脚本

```swift
"scripts": {
    "serve": "cross-env COMMON=1 vue-cli-service serve",
    "build": "cross-env COMMON=1 vue-cli-service build",
    "lint": "vue-cli-service lint",
    "build:client": "vue-cli-service build --dest dist/client",
    "build:server": "cross-env SSR=1 vue-cli-service build --dest dist/server",
    "build:ssr": "npm run build:client && npm run build:server",
    "dev:serve": "cross-env WEBPACK_TARGET=node node ./server/server.js",
    "dev": "concurrently \"npm run serve\" \"npm run dev:serve\" "
},
```

那么我们就使用命令打包吧

```arduino
npm run build:ssr
```

通过 node 运行打包好的文件

```vbscript
node server.js
```
