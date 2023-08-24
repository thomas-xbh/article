---
link: https://juejin.cn/post/7101596844181962788
title: 【前端工程化】配置React+ts企业级代码规范及样式格式和git提交规范
description: 使用prettier+eslint+stylelint+lint-staged+husky+commitlint统一代码和git提交规范
keywords: 前端,React.js,代码规范
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2022-05-25T09:04:12.000Z
publisher: 稀土掘金
stats: paragraph=261 sentences=283, words=1705
---
## 目录

1. 前言
2. 代码规范技术栈
3. 创建 **react18**+ **vite2**+ **ts**项目
4. **editorconfig**统一编辑器配置
5. **prettier**自动格式化代码
6. **eslint**+ **lint-staged**检测代码
7. 使用 **tsc**检测类型和报错
8. 代码提交时使用 **husky**检测代码语法规范
9. 代码提交时使用 **husky**检测 **commit**备注规范
10. 配置 **commitizen**方便添加 **commit**辅助备注信息
11. **stylelint**规范样式和保存自动修复
12. 总结

## 一. 前言

在前端项目工程日益复杂的今天，一套完善的开发环境配置可以极大的提升开发效率，提高代码质量，方便多人合作，以及后期的项目迭代和维护，项目规范分项目 **目录结构规范**， **代码格式规范**和 **git提交规范**，本文主要讲后两种, 最终代码已上传到[react18-vite2-ts](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fguojiongwei%2Freact18-vite2-ts "https://github.com/guojiongwei/react18-vite2-ts")

## 二. 代码规范技术栈

### 2.1 代码格式规范和语法检测

1. [vscode](https://link.juejin.cn?target=http%3A%2F%2Fvscode.bianjiqi.net%2F "http://vscode.bianjiqi.net/")：统一前端编辑器。
2. [editorconfig](https://link.juejin.cn?target=https%3A%2F%2Feditorconfig.org%2F "https://editorconfig.org/"): 统一团队vscode编辑器默认配置。
3. [prettier](https://link.juejin.cn?target=https%3A%2F%2Fwww.prettier.cn%2F "https://www.prettier.cn/"): 保存文件自动格式化代码。
4. [eslint](https://link.juejin.cn?target=https%3A%2F%2Feslint.bootcss.com%2F "https://eslint.bootcss.com/"): 检测代码语法规范和错误。
5. [lint-staged](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fokonet%2Flint-staged "https://github.com/okonet/lint-staged"): 只检测暂存区文件代码，优化eslint检测速度。

### 2.2 代码git提交规范

1. [husky](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Ftypicode%2Fhusky "https://github.com/typicode/husky"):可以监听[githooks](https://link.juejin.cn?target=https%3A%2F%2Fgit-scm.com%2Fbook%2Fzh%2Fv2%2F%25E8%2587%25AA%25E5%25AE%259A%25E4%25B9%2589-Git-Git-%25E9%2592%25A9%25E5%25AD%2590 "https://git-scm.com/book/zh/v2/%E8%87%AA%E5%AE%9A%E4%B9%89-Git-Git-%E9%92%A9%E5%AD%90")执行，在对应hook执行阶段做一些处理的操作。
2. [pre-commit](https://link.juejin.cn?target=https%3A%2F%2Fgit-scm.com%2Fbook%2Fzh%2Fv2%2F%25E8%2587%25AA%25E5%25AE%259A%25E4%25B9%2589-Git-Git-%25E9%2592%25A9%25E5%25AD%2590 "https://git-scm.com/book/zh/v2/%E8%87%AA%E5%AE%9A%E4%B9%89-Git-Git-%E9%92%A9%E5%AD%90")：githooks之一， 在commit提交前使用tsc和eslint对语法进行检测。
3. [commit-msg](https://link.juejin.cn?target=https%3A%2F%2Fgit-scm.com%2Fbook%2Fzh%2Fv2%2F%25E8%2587%25AA%25E5%25AE%259A%25E4%25B9%2589-Git-Git-%25E9%2592%25A9%25E5%25AD%2590 "https://git-scm.com/book/zh/v2/%E8%87%AA%E5%AE%9A%E4%B9%89-Git-Git-%E9%92%A9%E5%AD%90")：githooks之一，在commit提交前对commit备注信息进行检测。
4. [commitlint](https://link.juejin.cn?target=https%3A%2F%2Fcommitlint.js.org%2F%23%2F "https://commitlint.js.org/#/")：在githooks的pre-commit阶段对commit备注信息进行检测。
5. [commitizen](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fcommitizen%2Fcz-cli "https://github.com/commitizen/cz-cli")：git的规范化提交工具，辅助填写commit信息。

## 三. 创建 **react18** + **vite2** + **ts** 项目

创建项目使用 **vite**官网[搭建第一个vite项目](https://link.juejin.cn?target=https%3A%2F%2Fvitejs.cn%2Fguide%2F%23scaffolding-your-first-vite-project "https://vitejs.cn/guide/#scaffolding-your-first-vite-project")，因为要创建的是 **react+ts**项目，执行命令

```sh

pnpm create vite my-react-app -- --template react-ts
```

执行完成后的会在目录下创建 **my-react-app**项目，提示依次执行命令，来启动项目。

```sh
cd my-react-app
pnpm i
pnpm run dev
```

项目默认是运行在[http://localhost:3000](https://link.juejin.cn?target=http%3A%2F%2Flocalhost%3A3000 "http://localhost:3000") 地址，启动成功后在浏览器打开。

## 四. editorconfig统一编辑器配置

由于每个人的 **vsocde**编辑器默认配置可能不一样，比如有的默认缩进是 **4**个空格，有的是 **2**个空格，如果自己编辑器和项目代码缩进不一样，会给开发和项目代码规范带来一定影响，所以需要在项目中为编辑器配置下格式。

### 4.1 安装vscode插件EditorConfig

打开 **vsocde**插件商店，搜索 **EditorConfig for VS Code**，然后进行安装。

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9b0825f69f974600a567cb43ff0fc595~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

### 4.2 添加.editorconfig配置文件

安装插件后，在根目录新增 **.editorconfig**配置文件:

```sh
root = true
​
[**]
indent_style = space
indent_size = 2
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = true
end_of_line = lf
​
[**.md]
trim_trailing_whitespace = false
```

上面的配置可以规范本项目中文件的缩进风格，和缩进空格数等，会覆盖 **vscode**的配置，来达到不同编辑器中代码默认行为一致的作用。

## 五. prettier自动格式化代码

每个人写代码的风格习惯不一样，比如代码换行，结尾是否带分号，单双引号，缩进等，而且不能只靠口头规范来约束，项目紧急的时候可能会不太注意代码格式，这时候需要有工具来帮我们自动格式化代码，而 **prettier**就是帮我们做这件事的。

### 5.1 安装vscode插件Prettier

打开 **vsocde**插件商店，搜索 **Prettier - Code formatter**，然后进行安装。

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e3fbc9f8edae4043a902c38580405afb~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

### 5.2 添加.prettierrc.js配置文件

安装插件后，在根目录新增 **.prettierrc.js**配置文件，配置内容如下：

```js
module.exports = {
  printWidth: 100,
  tabWidth: 2,
  useTabs: false,
  semi: false,
  singleQuote: true,
  trailingComma: 'none',
  jsxSingleQuote: true,
  bracketSpacing: true,
  arrowParens: "avoid",
}
```

各个字段配置就是后面注释中的说明

### 5.3 添加.vscode/settings.json

配置前两步后，虽然已经配置 **prettier**格式化规则，但还需要让 **vscode**来支持保存后触发格式化

在项目根目录新建 **.vscode**文件夹，内部新建 **settings.json**文件配置文件，内容如下：

```json
{
  "search.exclude": {
    "/node_modules": true,
    "dist": true,
    "pnpm-lock.sh": true
  },
  "editor.formatOnSave": true,
  "[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[javascriptreact]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[typescript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[typescriptreact]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[json]": {
    "editor.defaultFormatter": "vscode.json-language-features"
  },
  "[html]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[markdown]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[css]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[less]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[scss]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "liveServer.settings.port": 5502
}
```

先配置了忽略哪些文件不进行格式化，又添加了保存代码后触发格式化代码配置，以及各类型文件格式化使用的格式。

这一步配置完成后，修改项目代码，把格式打乱，点击保存时就会看到编辑器自动把代码格式规范化了。

## 六. eslint+lint-staged检测代码

### 6.1 安装vscode插件ESLint

打开 **vsocde**插件商店，搜索 **ESLint**，然后进行安装。

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ab70fe4c4dc441189ba6708aa3252d29~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

### 6.2 安装eslint依赖

```js
pnpm i eslint -D
```

### 6.3 配置.eslintrc.js文件

安装 **eslint**后，执行 **pnpm init @eslint/config**，选择自己需要的配置

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f6e563a56a7a4948a2897d9beec93e93~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

这里我们选择了

* 使用 **eslint**检测并问题
* 项目使用的模块规范是 **es module**
* 使用的框架是 **react**
* 使用了 **typescript**
* 代码选择运行在浏览器端
* **eslint**配置文件使用 **js**格式
* 是否现在安装相关依赖，选择是
* 使用 **pnpm**包管理器安装依赖

选择完成后会在根目录下生成 **.eslint.js**文件，默认配置如下

```js
module.exports = {
    env: {
        browser: true,
        es2021: true
    },
    extends: [
        "eslint:recommended",
        "plugin:react/recommended",
        "plugin:@typescript-eslint/recommended"
    ],
    parser: "@typescript-eslint/parser",
    parserOptions: {
        ecmaFeatures: {
            jsx: true
        },
        ecmaVersion: "latest",
        sourceType: "module"
    },
    plugins: ["react", "@typescript-eslint"],
    rules: {
    }
};
```

### 6.4 解决当前项目eslint语法错误

此时 **eslint**基础配置就已经配置好了，此时要解决出现的几个小问题：

1. 看 **App.tsx**页面会发现 **jsx**部分有红色报红，提示 **'React' must be in scope when using JSX**

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a1ec998a85bc459c8a6acd3f8588abfa~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

这是因为 **React18**版本中使用 **jsx**语法不需要再引入 **React**了，根据[eslint-react-plugin](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fjsx-eslint%2Feslint-plugin-react%2Fblob%2Fmaster%2Fdocs%2Frules%2Freact-in-jsx-scope.md "https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/react-in-jsx-scope.md")中的说明，如果使用了 **react17**版本以上，不需要在使用 **jsx**页面引入 **React**时，在 **eslint**配置文件 **.eslint.js**的 **extends**字段添加插件 **plugin:react/jsx-runtime**。

```js
 extends: [
   'eslint:recommended',
   'plugin:react/recommended',
   'plugin:@typescript-eslint/recommended',
   'plugin:react/jsx-runtime'
 ],
```

此时 **App.tsx**和 **main.tsx**就不会报错了。

1. 看到 **main.tsx**文件带有警告颜色，看警告提示是 **Forbidden non-null assertion**。

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f54f46ee579f495fa1b560566058380b~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

这个提示是不允许使用非空操作符!，但实际在项目中经常会用到，所以可以把该项校验给关闭掉。

在 **eslint**配置文件 **.eslint.js**的 **rules**字段添加插件 **'@typescript-eslint/no-non-null-assertion': 'off'**。

```js
 rules: {
    '@typescript-eslint/no-non-null-assertion': 'off'
  }
```

然后就不会报警告了，如果为了避免代码出现异常，不想关闭该校验，可以提前做判断

```jsx
import React from 'react'
import ReactDOM from 'react-dom/client'
import App from './App'
import './index.css'
​
const root = document.getElementById('root')

if (root) {
  ReactDOM.createRoot(root).render(
    <React.StrictMode>
      <App />
    React.StrictMode>
  )
}
```

1. **.eslintec.js**文件也有红色报错，报错是 **'module' is not defined**.

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c17be49ede3d45d085d13404739ee763~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

这个是因为上面选择的浏览器环境，配置文件的 **module.exports**需要 **node**环境，需要在 **eslint**的 **env**环境配置中添加支持 **node**环境

```js
  env: {
    browser: true,
    es2021: true,
    node: true
  },
```

到这一步项目目前就没有报错了，在页面里面如果有定义了但未使用的变量，会有 **eslint**的标黄效果提示，如果有语法问题也会警告或者报错提示，等后面出现了其他问题再按照情况进行修复或者调整 **eslint**配置。

### 6.5 添加eslint语法检测脚本

前面的 **eslint**报错和警告都是我们用眼睛看到的，有时候需要通过脚本执行能检测出来，在 **package.json**的 **scripts**中新增

```bash
"eslint": "eslint src/**/*.{ts,tsx}"
```

代表检测 **src**目录下以**.ts**, **.tsx**为后缀的文件，然后在 **main.tsx**里面定义一个未使用的变量 **a**

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d59038557f044b739319c59ed7e23c34~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

执行 **npm run eslint**

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4158d6db7b9b447a8bacf7de1f94ea12~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

可以看到黄色代码部分会检测到有变量 **a**没有用到，会报一个警告。

除此之外再解决一个问题就是 **eslint**报的警告 **React version not specified in eslint-plugin-react settings**,需要告诉 **eslint**使用的 **react**版本，在 **.eslintrc.js**和 **rules**平级添加 **settings**配置，让 **eslint**自己检测 **react**版本,对应[issuse](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fyannickcr%2Feslint-plugin-react%2Fissues%2F2157 "https://github.com/yannickcr/eslint-plugin-react/issues/2157")

```js
 settings: {
    "react": {
      "version": "detect"
    }
  }
```

再执行 **npm run eslint**就不会报这个未设置 **react**版本的警告了。

### 6.6 使用lint-staged优化eslint检测速度

在上面配置的 **eslin**t会检测 **src**文件下所有的 **.ts**, **.tsx**文件，虽然功能可以实现，但是当项目文件多的时候，检测的文件会很多，需要的时间也会越来越长，但其实只需要检测提交到暂存区，就是 **git add**添加的文件，不在暂存区的文件不用再次检测，而 **lint-staged**就是来帮我们做这件事情的。

安装依赖（ **lint-staged**最新13版本需要 **node**大于 **14.13.1**版本,此处暂时用 **12**版本，配置是一样的）

```sh
pnpm i lint-staged@12.5.0 -D
```

修改 **package.json**脚本 **eslint**的配置

```js
"eslint": "eslint"
```

在 **package.json**添加 **lint-staged**配置

```js
"lint-staged": {
  "src/**/*.{ts,tsx}": [
    "npm run eslint"
  ]
},
```

因为要检测 **git**暂存区代码，所以需要执行 **git init**初始化一下 **git**

```sh
git init
```

初始化 **git**完成后就可以进行测试了，先提交一下没有语法问题的 **App.tsx**

```sh
git add src/App.tsx
```

把 **src/App.tsx**提交到暂存区后，执行 **npx lint-staged**

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/da6c0a950e364e80ae87bfe53f6d875b~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

可以看到检测通过了，没有检测到语法问题，再把有语法问题的 **src/main.tsx**文件提交暂存区再检测一下看看

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/00791d4b2bb64c3796fc4692f8abffed~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

发现虽然 **main.tsx**虽然有 **eslint**语法警告，但依然验证成功了，是因为 **lint-staged**只会检测到语法报错才会有提示只是警告不会，如果需要出现警告也阻止代码提交，需要给eslint检测配置参数 **--max-warnings=0**

```js
 "eslint": "eslint --max-warnings=0"
```

代表允许最多 **0**个警告，就是只要出现警告就会报错，修改完成后再次执行 **npx lint-staged**

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e08002c7a41f4f2a8f0dd4c817f6c1e1~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

就会看到最终执行的命令是

```sh
eslint --max-warnings=0 "E:/wuyou/react/my-react-app/src/App.tsx" "E:/wuyou/react/my-react-app/src/main.tsx"
```

**eslint**只检测了git暂存区的 **App.tsx**和 **main.tsx**两个文件，做到了只检测 **git**暂存区中文件的功能，避免每次都全量检测文件。

而添加了 **--max-warnings=0** 参数后，警告也可以检测出来并报错中断命令行了。

## 七. 使用tsc检测类型和报错

在项目中使用了 **ts**,但一些类型问题，现在配置的 **eslint**是检测不出来的，需要使用 **ts**提供的 **tsc**工具进行检测，如下示例

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d24ac6886dfa4ead852b92b730dbafbc~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

在 **main.tsx**定义了函数 **say**,参数 **name**是 **string**类型，当调用传 **number**类型参数时，页面有了明显的 **ts**报错，但此时提交 **main.tsx**文件到暂存区后执行 **npx lint-staged**

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3e89226b95534648a8a8ecb610937936~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

发现没有检测到报错，所以需要配置下 **tsc**来检测类型，在 **package.json**添加脚本命令

```sh
"pre-check": "tsc && npx lint-staged"
```

执行 **npm run pre-check**，发现已经可以检测出类型报错了。

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/24d0d3be3b4142deab84d199a2df7ca8~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

## 八. git提交时检测语法规范

为了避免把不规范的代码提交到远程仓库，一般会在 **git**提交代码时对代码语法进行检测，只有检测通过时才能被提交， **git**提供了一系列的[githooks](https://link.juejin.cn?target=https%3A%2F%2Fgit-scm.com%2Fwomdocs%2Fgithooks "https://git-scm.com/womdocs/githooks") ，而我们需要其中的 **pre-commit**钩子，它会在 **git commit**把代码提交到本地仓库之前执行，可以在这个阶段检测代码，如果检测不通过就退出命令行进程停止 **commit**。

### 8.1 代码提交前husky检测语法

而 **husky**就是可以监听 **githooks**的工具，可以借助它来完成这件事情。

### 8.2 安装husky

```sh
pnpm i husky -D
```

### 8.3 配置husky的pre-commit钩子

生成 **.husky**配置文件夹（如果项目中没有初始化 **git**,需要先执行 **git init**）

```sh
npx husky install
```

会在项目根目录生成 **.husky**文件夹，生成文件成功后，需要让 **husky**支持监听 **pre-commit**钩子，监听到后执行上面定义的 **npm run pre-check**语法检测。

```sh
npx husky add .husky/pre-commit 'npm run pre-check'
```

会在 **.husky**目录下生成 **pre-commit**文件，里面可以看到我们设置的 **npm run pre-check**命令。

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ac4936441a044b7ea61942d8052d6f3e~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

然后提交代码进行测试

```sh
git add .
git commit -m "feat: 测试提交验证"
```

会发现监听 **pre-commit**钩子执行了 **npm rim pre-check**, 使用 **eslint**检测了 **git**暂存区的两个文件，并且发现了 **main.tsx**的警告，退出了命令行，没有执行 **git commit**把暂存区代码提交到本地仓库。

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/eb5627665e024a9f8e35a6eea5419ce0~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

到这里代码提交语法验证就配置完成了，可以有效的保障仓库的代码质量。

### 8.4 安装依赖后自动husky install

husky生效需要手动执行husky install，可以借助package.json里面的postintall钩子实现这个功能，该钩子会在依赖安装完成后执行，在package.json里面添加

```json
"scripts": {
    "postinstall": "husky install"
}
```

这样每一次安装依赖时就都会执行husky install了，不过.husky文件不能被git忽略，需要提交到远程仓库上，不然得重新配置pre-commit钩子。

## 九. 代码提交时检测commit备注规范

在提交代码时，良好的提交备注会方便多人开发时其他人理解本次提交修改的大致内容，也方便后面维护迭代，但每个人习惯都不一样，需要用工具来做下限制，在 **git**提供的一系列的[githooks](https://link.juejin.cn?target=https%3A%2F%2Fgit-scm.com%2Fwomdocs%2Fgithooks "https://git-scm.com/womdocs/githooks") 中， **commit-msg**会在 **git commit**之前执行，并获取到 **git commit**的备注，可以通过这个钩子来验证备注是否合理，而验证是否合理肯定需要先定义一套规范，而[commitlint](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fconventional-changelog%2Fcommitlint "https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fconventional-changelog%2Fcommitlint")就是用来做这件事情的，它会预先定义一套规范，然后验证 **git commit**的备注是否符合定义的规范。

### 9.1 安装和配置commitlint

安装 **commitlint**

```sh
pnpm i @commitlint/config-conventional @commitlint/cli -D
```

在根目录创建 **commitlint.config.js**文件,添加配置如下

```sh
module.exports = {
  // 继承的规则
  extends: ['@commitlint/config-conventional'],
  // 定义规则类型
  rules: {
    // type 类型定义，表示 git 提交的 type 必须在以下类型范围内
    'type-enum': [
      2,
      'always',
      [
        'feat', // 新功能 feature
        'fix', // 修复 bug
        'docs', // 文档注释
        'style', // 代码格式(不影响代码运行的变动)
        'refactor', // 重构(既不增加新功能，也不是修复bug)
        'perf', // 性能优化
        'test', // 增加测试
        'chore', // 构建过程或辅助工具的变动
        'revert', // 回退
        'build' // 打包
      ]
    ],
    // subject 大小写不做校验
    'subject-case': [0]
  }
}
```

### 9.2 配置husky监听commit-msg

上面已经安装了 **husky**,现在需要再配置下 **husky**，让 **husky**支持监听 **commit-msg**钩子，在钩子函数中使用 **commitlint**来验证

```sh
npx husky add .husky/commit-msg 'npx --no-install commitlint --edit "$1"'
```

会在 **.husky**目录下生成 **commit-msg**文件，并且配置 **commitlint**命令对备注进行验证配置，配置完后就可以进行测试了（要把 **main.tsx**中的语法报错代码去掉并添加到暂存区）

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/bda9c8c1b8084577a2954fc461bda449~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

再次执行 **git commit -m "修改xx功能"**

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/bf155351381645429dc4c3121e285296~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

再次提交后可以看到 **commit-msg**验证没有通过，输入的备注信息为 **修改xx功能**,下面提示项目描述信息和类型不能为空，代表验证起到作用。

使用正确格式的备注再次提交,类型和描述之间需要用冒号加空格隔开

```sh
git commit -m 'feat: 修改xx功能'
```

就可以看到提交成功了。

## 十. 添加git commit辅助备注信息

在上面虽然定义了很多提交类型，但都是英文前缀，不容易记忆，可以添加辅助信息在我们提交代码的时候做选择，会方便很多，可以借助** [commitizen](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fcommitizen%2Fcz-cli "https://github.com/commitizen/cz-cli")**来实现这个功能。

### 10.1 安装commitizen

全局安装 **commitizen**，否则无法执行插件的 **git cz**命令：

```sh
pnpm i commitizen -g
```

在项目内安装 **cz-customizable**：

```sh
pnpm i cz-customizable -D
```

### 10.2 配置commitizen自定义提示规则

添加以下配置到 **package.json** 中：

```js
"config": {
    "commitizen": {
        "path": "./node_modules/cz-customizable"
    }
}
```

在根目录创建 **.cz-config.js** 自定义提示文件：

```js
module.exports = {

  types: [
    { value: 'feat', name: 'feat: 新功能' },
    { value: 'fix', name: 'fix: 修复' },
    { value: 'docs', name: 'docs: 文档变更' },
    { value: 'style', name: 'style: 代码格式(不影响代码运行的变动)' },
    { value: 'refactor', name: 'refactor: 重构(既不是增加feature，也不是修复bug)' },
    { value: 'perf', name: 'perf: 性能优化' },
    { value: 'test', name: 'test: 增加测试' },
    { value: 'chore', name: 'chore: 构建过程或辅助工具的变动' },
    { value: 'revert', name: 'revert: 回退' },
    { value: 'build', name: 'build: 打包' }
  ],

  messages: {
    type: '请选择提交类型:',

    subject: '请简要描述提交(必填):',

    footer: '请输入要关闭的issue(可选):',
    confirmCommit: '确认使用以上信息提交？(y/n)'
  },

  skipQuestions: ['scope', 'customScope', 'body', 'footer'],

  subjectLimit: 72
}
```

### 10.3 测试commitizen辅助提交

使用 **git add**添加文件到暂存区，然后使用 **git cz**替代 **git commit**命令提交代码：

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/de48ed7deccd4dbf81674ca590b3edd5~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

会出现选择提交类型和填写提交描述信息，选择 **yes**后，会触发 **git**提交语法验证，最终提交成功了，提交的备注是 **feat: 添加commit辅助信息**

## 十一. stylelint规范样式和保存自动修复

随便现在设备还有网络，浏览器性能越来越好，在前端代码性能方面关注的更多的是js层面的，但 **css**层面也能做很多性能优化， **css**属性的书写顺序，选择器使用，都会对整体应用性能产生影响。所以配置一套完善的 **css**代码书写规范可以有效提升应用的性能，而[stylelint](https://link.juejin.cn?target=https%3A%2F%2Fstylelint.io%2F "https://stylelint.io/")就是现在比较流行的 **csslint**库。

### 6.1 安装vscode插件Stylelint

打开 **vsocde**插件商店，搜索 **Stylelint**，然后进行安装

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0e458b3fc4d647c6b89da4ac5c8eefdc~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

### 6.2 安装Stylelint相关依赖

支持检测css文件需要安装 **stylelint**相关依赖

* **stylelint**: stylelint核心依赖
* **stylelint-config-standard**: stylelint拓展，支持配置文件拓展一些检测规则
* **stylelint-order**: 检测css属性书写顺序的规则集合,比如display:flex要写在align-items之前
* **stylelint-config-recess-order**: stylelint-order 插件的第三方配置

```bash
pnpm i stylelint stylelint-config-standard stylelint-order stylelint-config-recess-order -D
```

### 6.3 配置.stylelintrc.js文件

在根目录新增 **.stylelintrc.js**文件，添加配置

```js
module.exports = {
  extends: [
    'stylelint-config-standard',
    'stylelint-config-prettier',
    'stylelint-config-recess-order'
  ]
}
```

配置完成后打开 **src/App.css**，可以看到很多红色报错

**第一个报错：**

**vite**脚手架创建的 **react**项目，类名 **App**用的大驼峰，默认的 **stylelint**命名规则是小驼峰，这里就会有红色波浪线报错。

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ecb76127d2f04f0d919ae1c343bf234a~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

这种必须手动改，没有办法自动修复，或者修改 **styleint**默认规则，这里改为不限制类名命名，具体配置可看[selector-class-pattern](https://link.juejin.cn?target=https%3A%2F%2Fstylelint.io%2Fuser-guide%2Frules%2Flist%2Fselector-class-pattern%2F "https://stylelint.io/user-guide/rules/list/selector-class-pattern/")。

```js

module.exports = {

  rules: {
    'selector-class-pattern': null,
  }
}
```

**第二个报错：**

这个报错就是 **css**属性顺序的报错，一般 **display**应该放在设置高度之前更合理，这类问题可以通过保存后自动修复。

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1a83258e667f4da0a5c6a74c50c80fbb~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

**第三个报错：**

和第一个报错类似，是 **@keyframes App-logo-spin**,动画命名不允许大驼峰，这个可以手动修改为小驼峰命名。

### 6.4 设置保存自动修复

一般希望在保存文件 **css**文件后自动修复 **css**中的不合理的地方，在安装 **vscode**插件 **stylelint**后，需要修改一下 **.vscode/settins.json**文件

因为要使用 **stylelint**的规则格式化代码，不使用 **perttier**来格式化 **css**, **less**, **scss**文件了，删除掉 **.vscode/settins.json**中配置的使用 **perttier**格式化 **css**, **less**, **scss**的配置。

```json

  "[css]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[less]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[scss]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
```

在 **.vscode/settins.json**新增 **styleint**保存文件格式化样式文件配置

```json
{

  "editor.codeActionsOnSave": {
    "source.fixAll.stylelint": true
  },

  "css.validate": false,
  "less.validate": false,
  "scss.validate": false,

  "stylelint.validate": ["css", "less", "postcss", "scss", "sass", "vue"],

}
```

在 **.vscode/settings.json**文件中添加上面 **styleint**保存后自动修复配置代码后，来到 **src/App.css**文件，按 **ctrl+s**保存一下代码，会发现 **display**和 **min-height**的位置调整了，变成了

```css
.App-header {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  font-size: calc(10px + 2vmin);
  color: white;
  background-color: #282c34;
}
```

就代表自动修复配置成功了。

### 6.5 支持scss和less

**支持scss**

要支持 **scss**需要先安装 **scss**的 **stylelint**配置插件[stylelint-config-standard-scss](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fstylelint-scss%2Fstylelint-config-standard-scss "https://github.com/stylelint-scss/stylelint-config-standard-scss"),内部定义了 **scss**的语法规则，让 **stylelint**可以识别 **scss**的语法:

```bash
pnpm i stylelint-config-standard-scss -D
```

把插件添加到 **.stylelintrc.js**的 **extends**中

```js

module.exports = {

  extends: {

    'stylelint-config-standard-scss'
  }
}
```

新建 **src/App.scss**文件,代码如下，测试了变量命名， **mixin**混入和嵌套功能，同时把 **height**高度写在了 **display**之前。

```scss
$app-height: 200px;
​
@mixin rounded-corners {
  border-radius: 5px;
}
​
#root {
  .App {
    height: $app-height;
    display: block;
​
    @include rounded-corners;
  }
}
```

编辑好 **App.scss**文件后，可以看到里面对应 **scss**本身的语法没有报错，可以识别，同时检测到了 **height**不能放在 **display**前面的问题, 此时按 **crtl+s**，会自动修复格式调整 **height**和 **display**的位置。

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/22faf301ca2e4d37b15ebcb6ec0d2efd~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

**scss**文件经过测试上面配置的 **stylelint**已经支持 **scss**文件，并且可以进行语法检测和保存自动格式化。

**支持less**

要支持 **less**，要安装 **stylelint-less**插件来支持

```bash
pnpm i stylelint-less -D
```

修改 **.stylelintrc.js**, 新增以下配置

```js
module.exports = {

  plugins: ['stylelint-less'],
  rules: {

    'at-rule-no-unknown': null,
    'scss/at-rule-no-unknown': null,
    'color-no-invalid-hex': true,
    'less/color-no-invalid-hex': true
  }
}
```

新建 **src/App.less**文件

```less
@height: 200px;
​
.rounded-corners {
  border-radius: 5px;
}
​
#root {
  .App {
    display: block;
    height: @height;
  }
}
```

编辑好 **App.less**文件后，可以里面看到有报错， **height**不能放在 **display**前面, 此时按 **crtl+s**，会自动修复格式。

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cef92cb1a8f843458c54624ecfcf0349~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

### 6.7 通过命令行命令检测和修复。

自动修复只能修复一部分可通过改变属性顺序或者缩进换行来修复的问题，但像类名命名规则不符合规定命名的，自动修复就无能为力，需要手动调整，为了避免把不符合规范的样式代码提交到git远程仓库，需要在提交代码时对本次更改的样式文件进行语法检测。

上面已经配置了 **husky**钩子， **git commit**时会执行 **pre-check**脚本，只需要修改 **package.json**中的 **scripts**

```json
"scripts": {

  "stylelint": "stylelint"
},
"lint-staged": {

  "src/**/*.{css,less,scss}": [
    "npm run stylelint"
  ]
}
```

**测试一下**

注释掉 **.stylelintrc.js**文件中取消类名命名验证的配置

```js
module.exports = {

  plugins: ['stylelint-less'],
  rules: {

  }
}
```

然后提交 **src/App.scss**文件

```bash
git add src/App.scss
git commit -m "feat: 添加stylelint检测和修复样式问题"
```

结果如下图，可以看到检测到 **App.scss**文件中有类名的命名没有准守小驼峰加连接符的规则，所以报错中断后续命令了。

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/099569f654ae4ebb991b90b92a333678~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

**处理原有的样式文件**

如果原有的样式文件也想做修复处理，一种方式是挨个打开文件去执行 **crtl+s**保存，来达到修复效果，但这样效率很低，可以通过命令一键修复项目内样式文件

```bash
npx stylelint "src/**/*.{css,less,scss}" --fix
```

具体细节的stylelint配置可以查看[stylelint官网](https://link.juejin.cn?target=https%3A%2F%2Fstylelint.io%2Fuser-guide%2Frules%2Flist "https://stylelint.io/user-guide/rules/list")

## 十二. 总结

到现在一个 **react18+vite2+ts**项目从创建，到规范编辑器默认配置，代码自动格式化，代码提交时使用 **eslint**, **stylelint**和 **tsc**检测代码，验证 **git**提交备注信息，辅助选择 **git**提交备注信息已经后面的 **stylelint**配置 **css**和 **less**验证和语法自动修复就都已经配置好了。

这只是一个最常用基础的配置，比如 **eslint**还有很多可以深度定制配置的，比如针对 **react-hook**的语法检测配置，还有代码自动修复，但感觉自动修复有时候会比较坑，就没有配置。

**package.json**完整代码，其他配置文件代码上面都有,也可以看代码仓库[react18-vite2-ts](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fguojiongwei%2Freact18-vite2-ts "https://github.com/guojiongwei/react18-vite2-ts")

```json
{
  "name": "my-react-app",
  "private": true,
  "version": "0.0.0",
  "scripts": {
    "dev": "vite",
    "build": "tsc && vite build",
    "preview": "vite preview",
    "eslint": "eslint --max-warnings=0",
    "pre-check": "tsc && npx lint-staged",
    "stylelint": "stylelint"
  },
  "lint-staged": {
    "src/**/*.{ts,tsx}": [
      "npm run eslint"
    ],
    "src/**/*.{css,less,scss}": [
      "npm run stylelint"
    ]
  },
  "config": {
    "commitizen": {
      "path": "./node_modules/cz-customizable"
    }
  },
  "dependencies": {
    "react": "^18.0.0",
    "react-dom": "^18.0.0"
  },
  "devDependencies": {
    "@commitlint/cli": "^17.0.1",
    "@commitlint/config-conventional": "^17.0.0",
    "@types/react": "^18.0.0",
    "@types/react-dom": "^18.0.0",
    "@typescript-eslint/eslint-plugin": "^5.27.0",
    "@typescript-eslint/parser": "^5.27.0",
    "@vitejs/plugin-react": "^1.3.0",
    "cz-customizable": "^6.3.0",
    "eslint": "^8.16.0",
    "eslint-plugin-react": "^7.30.0",
    "eslint-plugin-react-hooks": "^4.5.0",
    "husky": "^8.0.1",
    "lint-staged": "12.5.0",
    "postcss": "^8.4.14",
    "stylelint": "^14.9.0",
    "stylelint-config-recess-order": "^3.0.0",
    "stylelint-config-standard": "^26.0.0",
    "stylelint-config-standard-scss": "^4.0.0",
    "stylelint-less": "^1.0.5",
    "stylelint-order": "^5.0.0",
    "typescript": "^4.6.3",
    "vite": "^2.9.9"
  }
}
```
