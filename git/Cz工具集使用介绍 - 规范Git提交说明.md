---
link: https://juejin.cn/post/6844903831893966856
title: Cz工具集使用介绍 - 规范Git提交说明
description: 在多人协作的项目中，如果Git的提交说明精准，在后期协作以及Bug处理时会变得有据可查，项目的开发可以根据规范的提交说明快速生成开发日志，从而方便开发者或用户追踪项目的开发信息和功能特性。 这里提供演示项目地址：cz-example。 如果没有规范的提交说明，很难阐述当前代码的…
keywords: JavaScript,前端
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2019-04-27T14:39:47.000Z
publisher: 稀土掘金
stats: paragraph=140 sentences=64, words=871
---
在多人协作的项目中，如果Git的 **提交说明**精准，在后期协作以及Bug处理时会变得有据可查，项目的开发可以根据规范的 **提交说明**快速生成开发日志，从而方便开发者或用户追踪项目的开发信息和功能特性。

本文主要内容：

* 介绍符合[Angular规范](https://link.juejin.cn?target=)（需要翻墙）的 **提交说明**
* 介绍 **提交说明**工具集[cz](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fcommitizen%2Fcz-cli "https://github.com/commitizen/cz-cli")（适配器、校验以及日志）的使用方法
* 介绍供开发者快速生成项目 **cz**工具集的Vue CLI 3插件[@ziyi2/ui-cz](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fziyi2%2Fvue-cli3-lerna-ui%2Ftree%2Fmaster%2Fplugins%2Fvue-cli-plugin-ui-cz "https://github.com/ziyi2/vue-cli3-lerna-ui/tree/master/plugins/vue-cli-plugin-ui-cz")

这里提供演示项目地址：[cz-example](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fziyi2%2Fcz-example "https://github.com/ziyi2/cz-example")。

## 广告

子弈在掘金平台的课程里发布了一本小册，可以看下阅读指南看看是否值得购买哈，如有打扰请多多包涵。

* [《深入浅出微前端》](https://juejin.cn/book/7258893482318626868 "https://juejin.cn/book/7258893482318626868")
* [《深入浅出微前端阅读指南》](https://juejin.cn/book/7258893482318626868/section/7259668174382547004 "https://juejin.cn/book/7258893482318626868/section/7259668174382547004")

## Git的提交说明

Git每次提交代码的时候都需要手写 **提交说明**（Commit message）：

```js
git commit -m "hello world"
```

书写多行可以使用以下命令：

```js
git commit
```

此时会跳出一个文本编辑器，可以在文本编辑器中书写多行 **提交说明**：

```js
# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.

#
# On branch master
# Your branch is up to date with 'origin/master'.

#
# Changes to be committed:
#       new file:   package.json
#
G:/git-lab/cz/.git/COMMIT_EDITMSG [unix] (19:49 24/01/2019)
```

如果没有规范的 **提交说明**，很难阐述当前代码的提交性质（修复Bug、代码性能优化、新增功能或者发布版本等）。查看Vue项目的Git **提交说明**（ `fix`表明修复问题、 `feat`表明新增功能...），它完全符合Angular规范：

![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/4/27/16a5f3b8c96cf228~tplv-t2oaga2asx-zoom-in-crop-mark:1512:0:0:0.awebp)

手写符合规范的 **提交说明**很难避免错误，可以借助工具来实现规范的 **提交说明**。

## 规范的Git提交说明

* 提供更多的历史信息，方便快速浏览
* 可以过滤某些 `commit`，便于筛选代码 `review`
* 可以追踪 `commit`生成更新日志
* 可以关联 **issues*

### `Git` 提交说明结构

`Git` **提交说明**可分为三个部分： `Header`、 `Body`和 `Footer`。

```js
<Header> <Body> <Footer>
```

### `Header`

`Header`部分包括三个字段 `type`（必需）、 `scope`（可选）和 `subject`（必需）。

```js
():
```

> Vue源码的 **提交说明**省略了 `scope`。

#### `type`

`type`用于说明 `commit` 的提交性质。

值描述feat新增一个功能fix修复一个Bugdocs文档变更style代码格式（不影响功能，例如空格、分号等格式修正）refactor代码重构perf改善性能test测试build变更项目构建或外部依赖（例如scopes: webpack、gulp、npm等）ci更改持续集成软件的配置文件和package中的scripts命令，例如scopes: Travis, Circle等chore变更构建流程或辅助工具revert代码回退

#### `scope`

`scope`说明 `commit`影响的范围。 `scope`依据项目而定，例如在业务项目中可以依据菜单或者功能模块划分，如果是组件库开发，则可以依据组件划分。

> 提示： `scope`可以省略。

#### `subject`

`subject`是 `commit`的简短描述。

### `Body`

`commit`的详细描述，说明代码提交的详细说明。

### `Footer`

如果代码的提交是 **不兼容变更**或 **关闭缺陷**，则 `Footer`必需，否则可以省略。

#### 不兼容变更

当前代码与上一个版本不兼容，则 `Footer`以 **BREAKING CHANGE**开头，后面是对变动的描述、以及变动的理由和迁移方法。

#### 关闭缺陷

如果当前提交是针对特定的issue，那么可以在 `Footer`部分填写需要关闭的单个 issue 或一系列issues。

## Commitizen

[commitizen/cz-cli](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fcommitizen%2Fcz-cli "https://github.com/commitizen/cz-cli")是一个可以实现规范的 **提交说明**的工具：

**When you commit with Commitizen, you'll be prompted to fill out any required commit fields at commit time. No more waiting until later for a git commit hook to run and reject your commit (though that can still be helpful). No more digging through CONTRIBUTING.md to find what the preferred format is. Get instant feedback on your commit message formatting and be prompted for required fields.**

提供选择的提交信息类别，快速生成 **提交说明**。安装cz工具:

```js
npm install -g commitizen
```

## Commitizen适配器

### cz-conventional-changelog

如果需要在项目中使用 **commitizen**生成符合AngularJS规范的 **提交说明**，初始化 **cz-conventional-changelog**适配器：

```js
commitizen init cz-conventional-changelog --save --save-exact
```

如果当前已经有其他适配器被使用，则会报以下错误，此时可以加上 `--force`选项进行再次初始化

```js
Error: A previous adapter is already configured. Use --force to override
```

初始化命令主要进行了3件事情

1. 在项目中安装 **cz-conventional-changelog** 适配器依赖
2. 将适配器依赖保存到 `package.json`的 `devDependencies`字段信息
3. 在 `package.json`中新增 `config.commitizen`字段信息，主要用于配置cz工具的适配器路径：

```js
"devDependencies": {
 "cz-conventional-changelog": "^2.1.0"
},
"config": {
  "commitizen": {
    "path": "./node_modules/cz-conventional-changelog"
  }
}
```

接下来可以使用cz的命令 `git cz`代替 `git commit`进行 **提交说明**：

![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/4/27/16a5f3b8c9c6a783~tplv-t2oaga2asx-zoom-in-crop-mark:1512:0:0:0.awebp)

代码提交到远程的Github后，可以在相应的项目中进行查看，例如（这里使用 `feat`不是很合适，只是一个示例）：

![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/4/27/16a5f3b8ca1ece00~tplv-t2oaga2asx-zoom-in-crop-mark:1512:0:0:0.awebp)

### cz-customizable

如果想定制项目的 **提交说明**，可以使用[cz-customizable](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fleonardoanalista%2Fcz-customizable "https://github.com/leonardoanalista/cz-customizable")适配器：

Suitable for large teams working with multiple projects with their own commit scopes. When you specify the scopes in your .cz-config.js, cz-customizable allows you to select the pre-defined scopes. No more spelling mistakes embarrassing you when generating the changelog file.

安装适配器

`npm install cz-customizable --save-dev`

将之前符合Angular规范的 **cz-conventional-changelog**适配器路径改成 **cz-customizable**适配器路径：

```js
"devDependencies": {
  "cz-customizable": "^5.3.0"
},
"config": {
  "commitizen": {
    "path": "node_modules/cz-customizable"
  }
}
```

cz-customizable will first look for a file called .cz-config.js，alternatively add a config block in your package.json。

官方提供了一个 `.cz-config.js`示例文件[cz-config-EXAMPLE.js](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fleonardoanalista%2Fcz-customizable%2Fblob%2Fmaster%2Fcz-config-EXAMPLE.js "https://github.com/leonardoanalista/cz-customizable/blob/master/cz-config-EXAMPLE.js")，如下所示：

```js
;

module.exports = {

  types: [
    {value: 'feat',     name: 'feat:     A new feature'},
    {value: 'fix',      name: 'fix:      A bug fix'},
    {value: 'docs',     name: 'docs:     Documentation only changes'},
    {value: 'style',    name: 'style:    Changes that do not affect the meaning of the code\n            (white-space, formatting, missing semi-colons, etc)'},
    {value: 'refactor', name: 'refactor: A code change that neither fixes a bug nor adds a feature'},
    {value: 'perf',     name: 'perf:     A code change that improves performance'},
    {value: 'test',     name: 'test:     Adding missing tests'},
    {value: 'chore',    name: 'chore:    Changes to the build process or auxiliary tools\n            and libraries such as documentation generation'},
    {value: 'revert',   name: 'revert:   Revert to a commit'},
    {value: 'WIP',      name: 'WIP:      Work in progress'}
  ],

  scopes: [
    {name: 'accounts'},
    {name: 'admin'},
    {name: 'exampleScope'},
    {name: 'changeMe'}
  ],

  messages: {
    type: 'Select the type of change that you\'re committing:',
    scope: '\nDenote the SCOPE of this change (optional):',

    customScope: 'Denote the SCOPE of this change:',
    subject: 'Write a SHORT, IMPERATIVE tense description of the change:\n',
    body: 'Provide a LONGER description of the change (optional). Use "|" to break new line:\n',
    breaking: 'List any BREAKING CHANGES (optional):\n',
    footer: 'List any ISSUES CLOSED by this change (optional). E.g.: #31, #34:\n',
    confirmCommit: 'Are you sure you want to proceed with the commit above?'
  },

  allowCustomScopes: true,
  allowBreakingChanges: ['feat', 'fix'],

  subjectLimit: 100

};
```

这里对其进行汉化处理（只是为了说明定制说明的一个示例）：

```js
;

module.exports = {

  types: [
    {value: '特性',     name: '特性:    一个新的特性'},
    {value: '修复',      name: '修复:    修复一个Bug'},
    {value: '文档',     name: '文档:    变更的只有文档'},
    {value: '格式',    name: '格式:    空格, 分号等格式修复'},
    {value: '重构', name: '重构:    代码重构，注意和特性、修复区分开'},
    {value: '性能',     name: '性能:    提升性能'},
    {value: '测试',     name: '测试:    添加一个测试'},
    {value: '工具',    name: '工具:    开发工具变动(构建、脚手架工具等)'},
    {value: '回滚',   name: '回滚:    代码回退'}
  ],

  scopes: [
    {name: '模块1'},
    {name: '模块2'},
    {name: '模块3'},
    {name: '模块4'}
  ],

  messages: {
    type: '选择一种你的提交类型:',
    scope: '选择一个scope (可选):',

    customScope: 'Denote the SCOPE of this change:',
    subject: '短说明:\n',
    body: '长说明，使用"|"换行(可选)：\n',
    breaking: '非兼容性说明 (可选):\n',
    footer: '关联关闭的issue，例如：#31, #34(可选):\n',
    confirmCommit: '确定提交说明?'
  },

  allowCustomScopes: true,
  allowBreakingChanges: ['特性', '修复'],

  subjectLimit: 100

};
```

再次使用 `git cz`命令进行 **提交说明**：

![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/4/27/16a5f3b8ca41a50b~tplv-t2oaga2asx-zoom-in-crop-mark:1512:0:0:0.awebp)

从上图可以看出此时的 **提交说明**选项已经汉化，继续填写 **提交说明**：

![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/4/27/16a5f3b8d00feed2~tplv-t2oaga2asx-zoom-in-crop-mark:1512:0:0:0.awebp)

把代码提交到远程看看效果：

![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/4/27/16a5f3b8d09ec06b~tplv-t2oaga2asx-zoom-in-crop-mark:1512:0:0:0.awebp)

## Commitizen校验

### commitlint

校验提交说明是否符合规范，安装校验工具[commitlint](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fmarionebl%2Fcommitlint "https://github.com/marionebl/commitlint")：

```js
npm install --save-dev @commitlint/cli
```

### @commitlint/config-conventional

安装符合Angular风格的校验规则

```js
npm install --save-dev @commitlint/config-conventional
```

在项目中新建 `commitlint.config.js`文件并设置校验规则：

```js
module.exports = {
  extends: ['@commitlint/config-conventional']
};
```

安装huksy（git钩子工具）

```js
npm install husky --save-dev
```

在package.json中配置 `git commit`提交时的校验钩子：

```js
"husky": {
  "hooks": {
    "commit-msg": "commitlint -E HUSKY_GIT_PARAMS"
  }
}
```

需要注意，使用该校验规则不能对 `.cz-config.js`进行不符合Angular规范的定制处理，例如之前的汉化，此时需要将 `.cz-config.js`的文件按照官方示例文件[cz-config-EXAMPLE.js](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fleonardoanalista%2Fcz-customizable%2Fblob%2Fmaster%2Fcz-config-EXAMPLE.js "https://github.com/leonardoanalista/cz-customizable/blob/master/cz-config-EXAMPLE.js")进行符合Angular风格的改动。

执行错误的提交说明：

![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/4/27/16a5f3ba28a5c09a~tplv-t2oaga2asx-zoom-in-crop-mark:1512:0:0:0.awebp)

执行符合Angular规范的提交说明：

![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/4/27/16a5f3ba9cf74c0a~tplv-t2oaga2asx-zoom-in-crop-mark:1512:0:0:0.awebp)

> commitlint需要配置一份校验规则，[@commitlint/config-conventional](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fmarionebl%2Fcommitlint%2Ftree%2Fmaster%2F%40commitlint%2Fconfig-conventional "https://github.com/marionebl/commitlint/tree/master/@commitlint/config-conventional")就是符合Angular规范的一份校验规则。

### commitlint-config-cz

如果是使用 **cz-customizable**适配器做了破坏Angular风格的提交说明配置，那么不能使用**@commitlint/config-conventional**规则进行提交说明校验，可以使用[commitlint-config-cz](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fwhizark%2Fcommitlint-config-cz "https://github.com/whizark/commitlint-config-cz")对定制化提交说明进行校验。

安装校验规则：

```js
npm install commitlint-config-cz --save-dev
```

然后加入commitlint校验规则配置：

```js
module.exports = {
  extends: [
    'cz'
  ]
};
```

> 这里推荐使用**@commitlint/config-conventional**校验规则，如果想使用cz-customizable适配器，那么定制化的配置不要破坏Angular规范即可。

### validate-commit-msg

除了使用 **commitlint**校验工具，也可以使用[validate-commit-msg](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2FFrikki%2Fvalidate-commit-message "https://github.com/Frikki/validate-commit-message")校验工具对cz提交说明是否符合Angular规范进行校验。

## Commitizen日志

如果使用了[cz](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fcommitizen%2Fcz-cli "https://github.com/commitizen/cz-cli")工具集，配套[conventional-changelog](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fconventional-changelog%2Fconventional-changelog%2Ftree%2Fmaster%2Fpackages%2Fconventional-changelog "https://github.com/conventional-changelog/conventional-changelog/tree/master/packages/conventional-changelog")可以快速生成开发日志：

```js
npm install conventional-changelog -D
```

在 `pacage.json`中加入生成日志命令：

```js
"version": "conventional-changelog -p angular -i CHANGELOG.md -s -r 0 && git add CHANGELOG.md"
```

You could follow the following workflow

* Make changes
* Commit those changes
* Pull all the tags
* Run the npm version [patch|minor|major] command
* Push

执行 `npm run version`后可查看生产的日志[CHANGELOG.md](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fziyi2%2Fcz-example%2Fblob%2Fmaster%2FCHANGELOG.md "https://github.com/ziyi2/cz-example/blob/master/CHANGELOG.md")。

> 注意要使用正确的 `Header`的 `type`，否则生成的日志会不准确，这里只是一个示例，生成的日志不是很严格。

## Vue CLI 3 插件

如果对于上述所说的配置感到繁琐，这里提供一个Vue CLI 3的插件，如果开发的项目由Vue CLI 3系统生成，可以使用插件 `@ziyi2/ui-cz`一键生成：

```js
vue add @ziyi2/ui-cz
```

该插件采用了 **cz-customizable**定制化 **提交说明**的适配器、 **@commitlint/config-conventional**校验规则以及 **conventional-changelog**日志生成器。

## 总结

呼吁大家书写规范的提交说明，代码说明不规范，项目成员泪两行。演示项目地址：[cz-example](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fziyi2%2Fcz-example "https://github.com/ziyi2/cz-example")。

## 链接

* [Angular规范](https://link.juejin.cn?target=https%3A%2F%2Fdocs.google.com%2Fdocument%2Fd%2F1QrDFcIiPjSLDn3EL15IJygNPiHORgU1_OOAqWjiDU5Y%2Fedit%23heading%3Dh.greljkmo14y0 "https://docs.google.com/document/d/1QrDFcIiPjSLDn3EL15IJygNPiHORgU1_OOAqWjiDU5Y/edit#heading=h.greljkmo14y0")
* [cz-cli](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fcommitizen%2Fcz-cli "https://github.com/commitizen/cz-cli") - cz工具
* [cz-customizable](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fleonardoanalista%2Fcz-customizable "https://github.com/leonardoanalista/cz-customizable") - cz适配器
* [@commitlint/config-conventional](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fmarionebl%2Fcommitlint%2Ftree%2Fmaster%2F%40commitlint%2Fconfig-conventional "https://github.com/marionebl/commitlint/tree/master/@commitlint/config-conventional") - cz适配器
* [commitlint](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fmarionebl%2Fcommitlint "https://github.com/marionebl/commitlint") - cz校验工具
* [commitlint-config-cz](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fwhizark%2Fcommitlint-config-cz "https://github.com/whizark/commitlint-config-cz") - cz校验工具的校验规则
* [validate-commit-msg](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2FFrikki%2Fvalidate-commit-message "https://github.com/Frikki/validate-commit-message") - cz校验工具
* [conventional-changelog](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fconventional-changelog%2Fconventional-changelog%2Ftree%2Fmaster%2Fpackages%2Fconventional-changelog "https://github.com/conventional-changelog/conventional-changelog/tree/master/packages/conventional-changelog") - cz日志生成器
* [commit_msg](https://link.juejin.cn?target=https%3A%2F%2Fgit-scm.com%2Fdocs%2Fgithooks%23_commit_msg "https://git-scm.com/docs/githooks#_commit_msg") - git钩子文档
