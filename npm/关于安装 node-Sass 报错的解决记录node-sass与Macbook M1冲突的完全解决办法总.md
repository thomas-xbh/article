---
link: https://juejin.cn/post/6988698858754686983
title: 关于安装 node-Sass 报错的解决记录node-sass与Macbook M1冲突的完全解决办法总结安装node-sass遇到的问题node.js和node-sass版本不兼容问题NPM | Yarn 安装 node-sass 失败
description: 使用npm安装node-sass时，或者安装需要python2的依赖时，会报出以下错误。 解决方案为: 1. 安装python2 可以用npm命令安装 也可以自行下载安装 Python 2.7 2.
keywords: Node.js
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2021-07-25T03:21:59.000Z
publisher: 稀土掘金
stats: paragraph=19 sentences=55, words=283
---
## 使用npm安装node-sass时，或者安装需要python2的依赖时，会报出以下错误。

```sh
gyp verb check python checking for Python executable "python2" in the PATH
gyp verb `which` failed Error: not found: python2
gyp verb `which` failed     at getNotFoundError (E:\codes\proviet\client-nuxt\node_modules\which\which.js:13:12)
gyp verb `which` failed     at F (E:\codes\proviet\client-nuxt\node_modules\which\which.js:68:19)
gyp verb `which` failed     at E (E:\codes\proviet\client-nuxt\node_modules\which\which.js:80:29)
gyp verb `which` failed     at E:\codes\proviet\client-nuxt\node_modules\which\which.js:89:16
gyp verb `which` failed     at E:\codes\proviet\client-nuxt\node_modules\isexe\index.js:42:5
gyp verb `which` failed     at E:\codes\proviet\client-nuxt\node_modules\isexe\windows.js:36:5
gyp verb `which` failed     at FSReqCallback.oncomplete (fs.js:183:21)
gyp verb `which` failed  python2 Error: not found: python2
gyp verb `which` failed     at getNotFoundError (E:\codes\proviet\client-nuxt\node_modules\which\which.js:13:12)
gyp verb `which` failed     at E:\codes\proviet\client-nuxt\node_modules\isexe\index.js:42:5
gyp verb `which` failed     at E:\codes\proviet\client-nuxt\node_modules\isexe\windows.js:36:5
gyp verb `which` failed     at FSReqCallback.oncomplete (fs.js:183:21) {
gyp verb `which` failed   code: 'ENOENT'
gyp verb `which` failed }
gyp verb check python checking for Python executable "python" in the PATH
gyp verb `which` succeeded python C:\Program Files\python\python.EXE
gyp ERR! configure error
gyp ERR! stack Error: Command failed: C:\Program Files\python\python.EXE -c import sys; print "%s.%s.%s" % sys.version_info[:3];
gyp ERR! stack   File "", line 1
gyp ERR! stack     import sys; print "%s.%s.%s" % sys.version_info[:3];
gyp ERR! stack                       ^
gyp ERR! stack SyntaxError: invalid syntax
gyp ERR! stack
gyp ERR! stack     at ChildProcess.exithandler (child_process.js:308:12)
gyp ERR! stack     at ChildProcess.emit (events.js:315:20)
gyp ERR! stack     at maybeClose (internal/child_process.js:1048:16)
gyp ERR! stack     at Process.ChildProcess._handle.onexit (internal/child_process.js:288:5)
gyp ERR! System Windows_NT 10.0.19042
gyp ERR! command "C:\\Program Files\\nodejs\\node.exe" "E:\\codes\\proviet\\client-nuxt\\node_modules\\node-gyp\\bin\\node-gyp.js" "rebuild" "--verbose" "--libsass_ext=" "--libsass_cflags=" "--libsass_ldflags=" "--libsass_library="
gyp ERR! cwd E:\codes\proviet\client-nuxt\node_modules\node-sass
gyp ERR! node -v v14.16.0

```

## 解决方案一:

### 1. 安装python2

* 可以用npm命令安装

```sh
npm install -g node-gyp
```

* 也可以自行下载安装[Python 2.7](https://link.juejin.cn?target=https%3A%2F%2Fwww.python.org%2Fdownloads%2F "https://www.python.org/downloads/")

### 2. 安装完毕后配置环境变量

```sh
npm config set python python2.7
```

### 3.再配置一下版本

```sh
npm config set msvs_version 2017
```

## 修改完毕，之后就可以正常安装node-sass了，如果还不行就使用淘宝镜像。

## 解决方案二(推荐)

`node-sass`实在太坑了，之前遇到安装失败使用方法一完美解决。最近又一次遇到了，但是方法一又无效了。于是我又在网上找到另一个方法，就是用 `dart-sass`来替换 `node-sass`。

正常的替换也会出问题，还要改配置。使用以下方法便可以解决 yarn安装的:

```sh
yarn add node-sass@yarn:dart-sass -D
```

npm安装的:

```sh
npm install node-sass@npm:dart-sass -D
```
