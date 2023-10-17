---
link: https://juejin.cn/post/7146512333609631757
title: ESlint与Prettier配置指南VSCode合理配置ESLint+Prettier如何在 React 项目中整合 Eslint 和 Prettier使用ESLint+Prettier来统一前端代码风格ESLint 之与 Prettier 配合使用
description: 格式化工具介绍 Prettier  作为 代码格式化 工具 ESLint  代码质量 方面的语法检查 EditorConfig 负责统一各种编辑器的配置，所有和编辑器相关的配置 (行尾、缩进样式、缩进
keywords: 前端
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2022-09-23T09:58:30.000Z
publisher: 稀土掘金
stats: paragraph=46 sentences=37, words=278
---
* **Prettier** 作为 `&#x4EE3;&#x7801;&#x683C;&#x5F0F;&#x5316;` 工具
* **ESLint** `&#x4EE3;&#x7801;&#x8D28;&#x91CF;` 方面的语法检查
* **EditorConfig** 负责统一各种编辑器的配置，所有和编辑器相关的配置 (行尾、缩进样式、缩进距离...) 都交给它

## ESLint规则

ESLint中的规则有3种设置：off、warn和error。

> "off"可以替换成0，代表关闭该规则
"warn"可以替换成1，代表打开规则，提示警告，但不会报错
"error"可以替换成2，代表打开规则，直接报错

在.eslintrc文件中，新增一个rules属性，为JSON对象类型。

## 安装ESLint

* eslint只有开发阶段需要，因此添加到开发阶段的依赖中即可

```css
npm install eslint --save-dev
```

* 在VS Code中安装eslint插件，以在开发中自动进行eslint校验

## 配置ESLint

* eslint-config-airbnb

为了规范团队成员代码格式，以及保持统一的代码风格，项目采用当前业界最火的Airbnb规范

* eslint-plugin-import：此插件主要为了校验 `import/export` 语法，防止错误拼写文件路径以及导出名称的问题
* eslint-plugin-jsx-a11y：提供 `jsx` 元素可访问性校验
* eslint-plugin-react：校验 React
* eslint-plugin-react-hooks：根据 Hooks API 校验 Hooks 的使用
* eslint-config-airbnb: airbnb的eslint 规范 （可选）

执行以下命令

```arduino
npm i -D eslint eslint-config-airbnb eslint-plugin-import eslint-plugin-jsx-a11y eslint-plugin-react eslint-plugin-react-hooks
```

下面，让我们需要根据 Eslint 的文档来配置这些插件。在控制台运行下面命令：

```bash
./node_modules/.bin/eslint --init
```

我们可以根据项目的需求，来选则相应的配置。执行完毕之后可以看到项目的根目录多了一个 `.eslintrc.js` 文件。

Eslint 支持多种格式的配置文件，优先级如下：

* 1、 .eslintrc.js
* 2、 .eslintrc.yaml
* 3、 .eslintrc.yml
* 4、 .eslintrc.json
* 5、 .eslintrc
* 6、 package.json

我们使用官方推荐的 `.eslintrc.js` 格式就好。

接下来，让我们使用喜欢的编辑器来打开这个文件，为了便于理解，我增加了一些注释：

```java
module.exports = {

    "env": {
        "browser": true,
        "es6": true
    },

    "extends": [
        "airbnb"
    ],

    "globals": {
        "Atomics": "readonly",
        "SharedArrayBuffer": "readonly",
        "_": true,
        "$": true,
    },

    "parserOptions": {
        "ecmaFeatures": {
            "jsx": true
        },
        "ecmaVersion": 2018,
        "sourceType": "module"
    },

    "plugins": [
        "react",

    ],

    "rules": {
        semi: 0,
        'no-unused-vars': [
            1,
            {
                vars: 'all',
                args: 'after-used',
                ignoreRestSiblings: true,
                varsIgnorePattern: '^_',
                argsIgnorePattern: '^_|^err|^ev'
            }
        ],
        'no-useless-escape': 2,
    }
};
```

## Prettier

我们可以借助 Eslint 来提高我们编码的质量，但是却无法保证统一代码风格。一个统一的代码风格对于团队来说是很有价值的，所以为了达到目的，我们可以选择使用 `Prettier` 在保存和提交代码的时候，将代码修改成统一的风格。这样做同时也降低了 Code Review 的成本。它不会代替 Eslint，所以需要和 Eslint 搭配使用。

安装以下依赖包

* prettier
* eslint-plugin-prettier
* eslint-config-prettier 解决eslint与prettier冲突问题

1、 安装依赖

```arduino
npm i -D prettier eslint-config-prettier eslint-plugin-prettier
```

2、 修改 Exlint 配置，打开 `.eslintrc.js` 文件，在扩展中增加 "plugin:prettier/recommended" ：

```json
    "extends": [
        "airbnb",
        "plugin:prettier/recommended"
    ]
```

3、 增加 prettier 配置文件，在根目录创建 `.prettierrc.js` ：

```java
module.exports = {
  "printWidth": 120,
  "tabWidth": 2,
}
```

1、 在 VS Code 商店中寻找并安装插件 ESlint，Prettier

2、 编辑 `settings.json`，通过下面路径，可以找到相应的配置文件：

* **Windows** `%APPDATA%\Code\User\settings.json`
* **macOS** `$HOME/Library/Application Support/Code/User/settings.json`
* **Linux** `$HOME/.config/Code/User/settings.json`

然后增加如下参数：

```json
  "files.autoSave": "onFocusChange",
  "editor.formatOnSave": true,
  "editor.formatOnType": true,
  "eslint.autoFixOnSave": true,
  "eslint.enable": true,
```

这样当我们在保存文件的时候，就会自动优化文件格式了。 后续更新 **EditorConfig**
