---
link: https://blog.csdn.net/duansamve/article/details/122645027
title: process.env环境变量配置方式（配置环境变量区分开发环境和生产环境）
description: 一、process.env 为何物？言归正传。 process.env 是 Node.js 中的一个环境变量。其中保存着系统的环境的变量信息。可使用 Node.js 命令行工具直接进行查看：1.安装nodejs	2.通过终端（cmd），输入node，进入编辑模式	3.输入process+回车, 显示进程	4.输入process.env+回车，显示 当前环境（environment）	5.输入process.env.NODE_ENV+回车，显示'undefined'说明 process.en_process.env
keywords: process.env
author: Duansamve Csdn认证博客专家 Csdn认证企业博客 码龄5年 暂无认证
date: 2022-05-26T14:31:57.000Z
publisher: null
stats: paragraph=84 sentences=135, words=491
---
言归正传。 process.env 是 Node.js 中的一个环境变量。其中保存着系统的环境的变量信息。可使用 Node.js 命令行工具直接进行查看：

* 1.安装nodejs
* 2.通过终端（cmd），输入node，进入编辑模式
* 3.输入process+回车, 显示进程
* 4.输入process.env+回车，显示 当前环境（environment）
* 5.输入process.env.NODE_ENV+回车，显示'undefined'

说明 process.env.NODE_ENV 不是系统默认选项，是人为后续加入的一个自定义项

这个变量主要用于标识当前的环境（生产环境，开发环境）。默认是没有这个环境变量的，需要自己手动配置。不同系统有不同的环境变量配置方式。

开始设置：

1、npm init -y 先初始化一个默认的包配置

2、在生成的package.json中，对scripts添加新指令

windows和posix命令如何使用环境变量存在差异

```js
...

scripts:{
  "dev" : "set NODE_ENV=production webpack"
}
...

```

```js
...

scripts:{
  "dev" : "NODE_ENV=production webpack"
}
...

```

所以需要借助于第三方包，来解决这个问题

3、cross-env (解决不同系统之前的命令兼容问题)

cross-env makes it so you can have a single command without worrying about setting or using the environment variable properly for the platform.

Just set it like you would if it's running on a POSIX system,
and cross-env will take care of setting it properly.

安装：

```js
npm install cross-env -D
```

使用：

```js
...

{
  "scripts": {
    "build": "cross-env NODE_ENV=production webpack"
  }
}
...

```

4、完成设置与验证

安装完 cross-env 在package.json中，定义2个指令，一个开发，一个生产，将NODE_ENV完成设置

```js
"scripts": {
  "dev": "cross-env NODE_ENV=development webpack --progress --colors",
  "build": "cross-env NODE_ENV=production webpack"
}
```

在webpack.config.js（没有就新建一个官当文档配置）中，设置log验证一下

```js
let env = process.env.NODE_ENV;
console.log(env);
```

终端输入npm run dev，log输出信息 "development"
终端输入npm run build，log输出信息 "production"

验证完毕，process.env.NODE_ENV设置好了

5、将process.env.NODE_ENV全局化

如果在业务代码中，需要根据开发环境，动态改变数据请求地址，会更加自动化，所以不仅仅在打包配置中我们需要使用 process.env.NODE_ENV，在其他模块中，仍要使用它，则需要将其全局化。

在webpack.config.js中

```js
...

const webpack = require('webpack');
...

module.exports = {
  configureWebpack: {
    plugins: [
      new webpack.DefinePlugin({
        "process.argv": JSON.stringify(process.argv)
      })
    ]
  }
}
...

```

6、总结

安装 cross-env 指令系统兼容包

```js
npm install cross-env -D
```

在package.json中配置指令

```js
...

"scripts": {
  "dev": "cross-env NODE_ENV=development webpack --progress --colors",
  "build": "cross-env NODE_ENV=production webpack"
}
...

```

在webpack.config.js中，全局化process.env.NODE_ENV

```js
...

const webpack = require('webpack');
...

module.exports = {
  configureWebpack: {
    plugins: [
      new webpack.DefinePlugin({
        "process.argv": JSON.stringify(process.argv)
      })
    ]
  }
}
...

```

根据开发环境，DIY自己业务逻辑

在webpack.config.js中，根据process.env.NODE_ENV自行配置引用依赖逻辑，在子模块中根据process.env.NODE_ENV调整数据请求连接，使得项目更加自动化。

NODE_ENV 变量只能在系统中配置吗？其实不然。在 Vue 项目中， Vue 提供了自己的配置方式。这就要涉及到 Vue CLI 中模式的概念了。 Vue CLI 文档说明了这个问题。

> **模式**是 Vue CLI 项目中一个重要的概念。默认情况下，一个 Vue CLI 项目有三个模式：

* `development` 模式用于 `vue-cli-service serve`
* `test` 模式用于 `vue-cli-service test:unit`
* `production` 模式用于 `vue-cli-service build` 和 `vue-cli-service test:e2e`

你可以通过传递 `--mode` 选项参数为命令行覆写默认的模式。例如，如果你想要在构建命令中使用开发环境变量：

```
vue-cli-service build --mode development
```

也就是说，在 Vue 中， NODE_ENV 可以通过 .env 文件或者 .env.[mode] 文件配置。配置过后，运行 Vue CLI 指令（ npm run dev(serve) ，npm run build ）时，就会将该模式下的NODE_ENV载入其中了。而这些命令，都有自己的默认模式：

* npm run dev(serve) ，其实是运行了 vue-cli service serve ，默认模式为 development 。可以在 .env.development 文件下修改该模式的 NODE_ENV 。
* npm run build ，其实运行了 vue-cli service build ，默认模式为 production 。可以在 .env.production 文件下修改该模式的 NODE_ENV 。

修改方式如下，以键值对的方式：

.env.staging

```js
NODE_ENV = production
```

除了以上的修改方式外，也可以在命令后直接使用 --mode 参数手动指定模式。当然，每个模式配置的变量也不只有 NODE_ENV , 也可以通过配置其他的变量简化工作流程。

模式的应用

有了模式的概念，就可以根据不同的环境配置模式，就不用每次打包时都去更改 vue.config.js 文件了。比如在测试环境和生产环境， publicPath参数 （部署应用包时的基本 URL） 可能不同。遇到这种情况就可以在 vue.config.js 文件中，将 publicPath 参数设置为：

```js
publicPath: process.env.BASE_URL
```

设置之后，再在各个 .env.[mode] 文件下对 BASE_URL 进行配置就行了，这样就避免了每次修改配置文件的尴尬。其他的配置也是同理。

> Tips: 即使不是生产环境，也可以将模式设置为 production ，这样可以获得 webpack 默认的打包优化。

**示例：**

1、在根目录新增五个文件(根据自身情况增减)

.env 和 .env.dev和 .env.pre和 .env.prod和 .env.sit和 .env.uat，分别为默认配置、本地开发配置、灰度配置、生产配置、测试配置1、测试配置2，(ps: vue_APP是统一标志，后面的拓展名可以任取)

.env

```js
VUE_APP_TITLE='dev'
```

.env.dev

```js
NODE_ENV = 'development'
VUE_APP_TITLE = 'development'/*请求接口地址*/
VUE_APP_INTERFACE_URL="https://xxx"/*首页地址*/
VUE_APP_URL="http://xxx"/*proxy代理地址*/
VUE_APP_PROXYURL='http://xxx'
```

.env.prod

```js
NODE_ENV = production
VUE_APP_TITLE = 'prod'/*请求接口地址*/
VUE_APP_INTERFACE_URL="https://xxx"/*首页地址*/
VUE_APP_URL="http://xxx"
```

2、设置项目启动时默认的环境

只需要在项目启动命令后面修改需要的环境就行，例如我这是npm run dev，把--mode dev改成--mode uat就行了

package.json

```js
"scripts": {
    "dev": "vue-cli-service serve --mode dev",
    "build": "vue-cli-service build --mode dev",
    "lint": "vue-cli-service lint",
    "build-sit": "vue-cli-service build --mode sit",
    "build-uat": "vue-cli-service build --mode uat",
    "build-pre": "vue-cli-service build --mode pre",
    "build-prod": "vue-cli-service build --mode prod"
}
```

3、查看环境是否配置成功

在main.js里打印当前环境，输出就成功了

```js
console.log(process.env.NODE_NEV);
console.log(process.env.VUE_APP_URL);
```
