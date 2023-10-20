---
link: null
title: 搞清楚怎样基于 Webpack5 从 0 搭建 React开发环境-超详细-腾讯云开发者社区-腾讯云
description: 出处 @刘江 ，原文地址 https://juejin.cn/post/6929496472232656909
keywords: html,react,webpack,javascript,打包
author: null
date: 2021-04-21T08:38:37.000Z
publisher: null
stats: paragraph=29 sentences=31, words=101
---
出处 @刘江 ，原文地址 https://juejin.cn/post/6929496472232656909

**初始化**

创建文件夹并进入：

初始化 `package.json`

安装 `Webpack`

创建以下目录结构、文件和内容：

然后根目录终端输入： `npm run build`

在浏览器中打开 `dist` 目录下的 `index.html`，如果一切正常，你应该能看到以下文本： `'React'`

`index.html` 目前放在 `dist` 目录下，但它是手动创建的，下面会教你如何生成 `index.html` 而非手动编辑它。

## **Webpack 核心功能**

在根目录下添加 `.babelrc` 文件：

由于 `webpack` 使用的是 `^5.21.2` 版本，在使用该插件时，会提示 `clean-webpack-plugin: options.output.path not defined. Plugin disabled...`，暂时还未解决。

## **.jsx 文件**

在 `src` 目录下，新增 `App.jsx` 文件：

## **React Router**

## **MobX**

在 `src` 目录下新建 `store.js`

## **Ant Design**

## **TypeScript**

在根目录下，新增 `tsconfig.json` 文件：

更换文件后缀 `App.jsx` -> `App.tsx`

## **代码规范**

代码校验、代码格式化、 `Git` 提交前校验、 `Vscode`配置、编译校验

在根目录下，新增 `.eslintrc.js` 文件：

在根目录下，新增 `.eslintignore` 文件：

在根目录下新增 `.vscode &#x6587;&#x4EF6;&#x5939;`，然后新增 `.vscode/settings.json`

在根目录下，新增 `prettier.config.js` 文件：

在根目录下，新增 `stylelint.config.js` 文件：

搭建这个的过程，也是遇到了不少坑，收获也是蛮多的，希望这个教程能够帮助更多的同学，少采点坑，完整的 `React` 开发环境可以看这个tristana，求点赞，求关注！
