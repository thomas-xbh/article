---
link: null
title: 【前端工程化】使用dumi2搭建你的组件库和函数库
description: 【前端工程化】使用dumi2搭建你的组件库和函数库
keywords: 个人博客，简约博客，郭炯韦个人博客，郭炯韦,郭炯韦
author: 郭炯韦
date: null
publisher: 郭炯韦个人博客
stats: paragraph=230 sentences=207, words=1696
---
## 目录

1. dumi简介
2. 创建dumi项目
3. 开发基础组件
4. 基于antd二次开发
5. 开发工具函数
6. jest单元测试优化
7. 打包部署

**全文概览**

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/10366cd3709f4feb99b6d5358c1a82fb~tplv-k3u1fbpfcp-watermark.image?)

本文使用dumi2详细的一步一步带你搭建组件库和函数库，编写单元测试，部署文档站点和发布npm包，以及搭建过程中遇到的问题和解决方案。

## 一. dumi简介

直接引用[官网](https://d.umijs.org/)的介绍

### 1.1 什么是 dumi

dumi，中文发音 **嘟米**，是一款为组件开发场景而生的静态站点框架，与 [father](https://github.com/umijs/father) 一起为开发者提供一站式的组件开发体验， **father 负责组件源码构建，而 dumi 负责组件开发及组件文档生成**。

简单来说 **dumi**是一个开发组件库和函数库的框架，可以方便的开发组件和示例文档，最终把 **示例文档**打包为静态站点部署，并且把组件源码打包为最终发布的 **npm包**，我们可以专注于组件开发，节省了大量配置的时间。

### 1.2 特性

全新的 dumi 2.0 主要具备以下特性：

* 🚀 **更好的编译性能**：通过结合使用[Umi 4 MFSU](https://umijs.org/blog/mfsu-faster-than-vite)、esbuild、SWC、持久缓存等方案，带来比 dumi 1.x 更快的编译速度
* 🔍 **内置全文搜索**：不需要接入任何三方服务，标题、正文、demo 等内容均可被搜索，支持多关键词搜索，且不会带来产物体积的增加
* 🎨 **全新主题系统**：为主题包增加插件、国际化等能力的支持，且参考[Docusaurus](https://docusaurus.io/docs/swizzling) 为主题用户提供局部覆盖能力，更强更易用
* 🚥 **约定式路由增强**：通过拆分路由概念、简化路由配置等方式，让路由生成一改 dumi 1.x 的怪异、繁琐，更加符合直觉
* 💡 **资产元数据 2.0**：在 1.x 及 JSON Schema 的基础上对资产属性定义结构进行全新设计，为资产的流通提供更多可能
* 💎 **继续为组件研发而生**：提供与全新的 NPM 包研发工具[father 4](https://github.com/umijs/father) 集成的脚手架，为开发者提供一站式的研发体验

### 1.3 什么时候会用到

搭建自己的函数库和组件库通常是为了提高代码的复用性和开发效率，特别适合在以下情况下使用：

1. 多个项目共用的函数或组件。如果你有多个项目需要使用同样的函数或组件，可以将其封装成库来提高复用性和开发效率。这样做可以避免重复编写代码，同时也方便后续维护和更新。
2. 团队协作开发。如果你在一个团队中进行协作开发，搭建自己的函数库和组件库可以方便不同成员之间的代码共享和协作。这样做可以避免不同成员之间重复编写代码，提高开发效率和代码质量。
3. 提高代码可维护性。搭建自己的函数库和组件库可以方便后续的代码维护和更新。这样做可以避免代码中存在大量的冗余代码和重复代码，提高代码的可读性和可维护性。

## 二. 安装dumi

### 2.1 环境准备

确保正确安装 [Node.js](https://nodejs.org/en/) 且版本为 14+ 即可。

```
$ <span class="hljs-keyword">node</span> <span class="hljs-title">-v</span>
v14.<span class="hljs-number">19.1</span>
```

### 2.2 创建项目

先找个地方建个空目录

```
<span class="hljs-built_in">mkdir</span> dumi2-<span class="hljs-built_in">demo</span> && cd dumi2-<span class="hljs-built_in">demo</span>
```

目录创建好后通过官方工具创建项目，选择你需要的模板

```
<span class="hljs-attribute">npx create-dumi</span>
```

dumi2目前模版有三种

* Static Site：静态文档站点
* React Library: react组件库
* Theme Package: 主题包

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/48a3fd2d46c547dfb557d7128e8ad79b~tplv-k3u1fbpfcp-watermark.image?)

在进行模版类型的时候我们选择 **React Library**模版，可以开发组件和生成静态站点文档，其他的是选择npm管理工具，包名称，描述和邮箱，按自己情况来输入就好了，这里包名称输入的是 **dumi2-demo**。

等待安装完依赖后启动项目：

```
<span class="hljs-built_in">npm</span> start
```

项目就启动起来了，输入[http://localhost:8000](http://localhost:8000)，可以看到启动成功后的页面，这个界面也就是我们打包后的静态站点。

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/09c71105e6904a699b943361c9179a7e~tplv-k3u1fbpfcp-watermark.image?)

### 2.3 目录结构

一个普通的、使用 dumi 做研发的组件库目录结构大致如下：

```
.
&#x251C;&#x2500;&#x2500; docs
&#x2502;   &#x251C;&#x2500;&#x2500; <span class="hljs-keyword">index</span>.md
&#x2502;   &#x251C;&#x2500;&#x2500; guide
&#x2502;   &#x2502;   &#x251C;&#x2500;&#x2500; <span class="hljs-keyword">index</span>.md
&#x2502;   &#x2502;   &#x2514;&#x2500;&#x2500; help.md
&#x251C;&#x2500;&#x2500; src
&#x2502;   &#x251C;&#x2500;&#x2500; Button
&#x2502;   &#x2502;   &#x251C;&#x2500;&#x2500; <span class="hljs-keyword">index</span>.tsx
&#x2502;   &#x2502;   &#x251C;&#x2500;&#x2500; <span class="hljs-keyword">index</span>.less
&#x2502;   &#x2502;   &#x2514;&#x2500;&#x2500; <span class="hljs-keyword">index</span>.md
&#x2502;   &#x2514;&#x2500;&#x2500; <span class="hljs-keyword">index</span>.ts
&#x251C;&#x2500;&#x2500; .dumirc.ts
&#x2514;&#x2500;&#x2500; .fatherrc.ts
```

### 2.4 完善工作

在运行成功后的页面，可以看到有几个问题，一个是左上角项目名称换行，一个是要把Foo修改为组件库选项。

修改项目名称项目名称换行问题可以根据类名覆盖原有样式，在.dumirc.ts文件中添加注入的css配置:

```
<span class="hljs-keyword">import</span> { defineConfig } <span class="hljs-keyword">from</span> <span class="hljs-string">'dumi'</span>;
&#x200B;
<span class="hljs-keyword">export</span> <span class="hljs-keyword">default</span> defineConfig({

  <span class="hljs-attr">styles</span>: [
    <span class="hljs-string">`.dumi-default-header-left {
       width: 220px !important;
    }`</span>,
  ],
});
```

修改Foo选项为组件库，在.dumirc.ts文件中添加配置:

```
<span class="hljs-keyword">import</span> { defineConfig } from <span class="hljs-string">'dumi'</span>;
&#x200B;
export <span class="hljs-keyword">default</span> defineConfig({

<span class="hljs-symbol">  themeConfig:</span> {
<span class="hljs-symbol">    name:</span> <span class="hljs-string">'dumi2-demo'</span>,
<span class="hljs-symbol">    nav:</span> [
      { <span class="hljs-string">title:</span> <span class="hljs-string">'&#x4ECB;&#x7ECD;'</span>, <span class="hljs-string">link:</span> <span class="hljs-string">'/guide'</span> },
      { <span class="hljs-string">title:</span> <span class="hljs-string">'&#x7EC4;&#x4EF6;'</span>, <span class="hljs-string">link:</span> <span class="hljs-string">'/components/Foo'</span> },
    ],
  },

});
```

调整后的界面就变成了，此时就可以开始组件和函数的开发了。

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/43ca55d8ece64b78ad4b224709a39081~tplv-k3u1fbpfcp-watermark.image?)

## 三. 开发基础组件

> mac电脑可以使用自带的命令行操作，window系统建议使用git命令行操作。

### 3.1 **添加组件源代码**

这里先新增一个简单的Button组件，在src下面新增Button文件内写入index.tsx文件。

```
<span class="hljs-keyword">mkdir</span> src/<span class="hljs-keyword">Button</span> && touch src/<span class="hljs-keyword">Button</span>/index.tsx
```

在文件中新增一个简单的Button组件代码：

```
<span class="hljs-keyword">import</span> React, { memo } from <span class="hljs-string">'react'</span>;
<span class="hljs-keyword">import</span> <span class="hljs-string">'./styles/index.less'</span>
<span class="hljs-keyword">export</span> <span class="hljs-class"><span class="hljs-keyword">interface</span> <span class="hljs-title">ButtonProps</span> </span>{

  type?: <span class="hljs-string">'primary'</span> | <span class="hljs-string">'default'</span>;

  children?: React.ReactNode;
  onClick?: React.MouseEventHandler<htmlbuttonelement>
}
&#x200B;

<span class="hljs-keyword">const</span> Button: React.FC<buttonprops> = (props) => {
  <span class="hljs-keyword">const</span> { type = <span class="hljs-string">'default'</span>, children, onClick } = props
  <span class="hljs-keyword">return</span> (
    <button type="<span" class="hljs-string">'button'
      className={`dumi-btn ${type ? <span class="hljs-string">'dumi-btn-'</span> + type : <span class="hljs-string">''</span>}`}
      onClick={onClick}
    >
      {children}
    </button>
  );
};
&#x200B;
<span class="hljs-keyword">export</span> <span class="hljs-keyword">default</span> memo(Button);
</buttonprops></htmlbuttonelement>
```

### 3.2 **添加less样式和变量**

先定义less样式变量，方便在项目中使用的时候改变组件主题样式。

在src下创建variables.less文件

```
touch src/variables.<span class="hljs-keyword">less</span>
```

添加主题代码：

```

<span class="hljs-variable">@dumi-primary:</span> <span class="hljs-number">#4d90fe</span>;
```

然后在Button组件库下面新建styles文件夹，里面新建index.less文件

```
<span class="hljs-keyword">mkdir</span> src/<span class="hljs-keyword">Button</span>/styles && touch src/<span class="hljs-keyword">Button</span>/styles/index.less
```

添加样式文件

```

<span class="hljs-keyword">@import</span> <span class="hljs-string">'../../variables.less'</span>;
&#x200B;
<span class="hljs-selector-class">.dumi-btn</span> {
  <span class="hljs-attribute">font-size</span>: <span class="hljs-number">14px</span>;
  <span class="hljs-attribute">height</span>: <span class="hljs-number">32px</span>;
  <span class="hljs-attribute">padding</span>: <span class="hljs-number">4px</span> <span class="hljs-number">15px</span>;
  <span class="hljs-attribute">border-radius</span>: <span class="hljs-number">6px</span>;
  <span class="hljs-attribute">transition</span>: all .<span class="hljs-number">3s</span>;
  <span class="hljs-attribute">cursor</span>: pointer;
}
&#x200B;
<span class="hljs-selector-class">.dumi-btn-default</span> {
  <span class="hljs-attribute">background</span>: <span class="hljs-number">#fff</span>;
  <span class="hljs-attribute">color</span>: <span class="hljs-number">#333</span>;
  <span class="hljs-attribute">border</span>: <span class="hljs-number">1px</span> solid <span class="hljs-number">#d9d9d9</span>;
&#x200B;
  <span class="hljs-selector-tag">&</span><span class="hljs-selector-pseudo">:hover</span> {
    <span class="hljs-attribute">color</span>: <span class="hljs-variable">@dumi-primary</span>;
    <span class="hljs-attribute">border-color</span>: <span class="hljs-variable">@dumi-primary</span>;
  }
}
&#x200B;
<span class="hljs-selector-class">.dumi-btn-primary</span> {
  <span class="hljs-attribute">color</span>: <span class="hljs-number">#fff</span>;
  <span class="hljs-attribute">background</span>: <span class="hljs-variable">@dumi-primary</span>;
  <span class="hljs-attribute">border</span>: <span class="hljs-number">1px</span> solid <span class="hljs-variable">@dumi-primary</span>;
}
```

组件源代码添加好后，需要在src/index.ts中引入后暴露一下：

```

<span class="hljs-keyword">export</span> { <span class="hljs-keyword">default</span> <span class="hljs-keyword">as</span> Button } <span class="hljs-keyword">from</span> <span class="hljs-string">'./Button'</span>;
```

在这里引入并暴露出去以后，就可以在项目中通过 `import { Button } from 'dumi2-demo';`来引入了。

### 3.3 **添加demo示例**

每一个组件我们可以加一个demo示例，方便使用者能更方便的使用。

在Button目录下新建一个demo文件夹，内建一个基础演示base.tsx文件:

```
<span class="hljs-keyword">mkdir</span> src/<span class="hljs-keyword">Button</span>/demo && touch src/<span class="hljs-keyword">Button</span>/demo/base.tsx
```

然后添加组件的演示代码：

```

&#x200B;
<span class="hljs-keyword">import</span> React <span class="hljs-keyword">from</span> <span class="hljs-string">'react'</span>;
<span class="hljs-keyword">import</span> { Button } <span class="hljs-keyword">from</span> <span class="hljs-string">'dumi2-demo'</span>;
&#x200B;
<span class="hljs-keyword">export</span> <span class="hljs-keyword">default</span> () => {
&#x200B;
  <span class="hljs-keyword">return</span> (
    <span class="xml"><span class="hljs-tag"><></span>
      <span class="hljs-tag"><<span class="hljs-name">Button</span> <span class="hljs-attr">type</span>=<span class="hljs-string">"default"</span>></span>&#x9ED8;&#x8BA4;&#x6309;&#x94AE;<span class="hljs-tag">Button</span>></span>
      <span class="hljs-tag"><<span class="hljs-name">Button</span> <span class="hljs-attr">type</span>=<span class="hljs-string">"primary"</span>></span>&#x4E3B;&#x8981;&#x6309;&#x94AE;<span class="hljs-tag">Button</span>>
    <span class="hljs-tag"></></span>
  );
}
```

### 3.4 **添加组件文档**

再在该文件同目录新建一个index.md文件作为文档说明，这也是生成静态文档站点所需要的。

```
<span class="hljs-keyword">touch</span> src/Button/<span class="hljs-built_in">index</span>.md
```

添加文档内容，具体内容描述可以看官网[MakeDown配置项](https://d.umijs.org/config/markdown)，这里只在注释里面讲一下用到的配置。

```

<span class="hljs-attr">category:</span> <span class="hljs-string">Components</span>
<span class="hljs-attr">title:</span> <span class="hljs-string">Button</span> <span class="hljs-string">&#x6309;&#x94AE;</span>
<span class="hljs-attr">toc:</span> <span class="hljs-string">content</span>
<span class="hljs-attr">group:</span>
  <span class="hljs-attr">title:</span> <span class="hljs-string">&#x57FA;&#x7840;&#x7EC4;&#x4EF6;</span>
  <span class="hljs-attr">order:</span> <span class="hljs-number">1</span>

<span class="hljs-string">&#x200B;</span>

<span class="hljs-string">&#x200B;</span>

<span class="hljs-string">&#x200B;</span>
<span class="hljs-string">&#x57FA;&#x7840;&#x7684;&#x6309;&#x94AE;&#x7EC4;&#x4EF6;</span> <span class="hljs-string">Button&#x3002;</span>
<span class="hljs-string">&#x200B;</span>

<span class="hljs-string">&#x200B;</span>

<span class="hljs-string"><code< span> <span class="hljs-string">src="./demo/base.tsx">&#x57FA;&#x7840;&#x7528;&#x6CD5;</span></code<></span>
```

​

​

| 属性 | 类型                   | 默认值   | 必填 | 说明 |
| ---- | ---------------------- | -------- | ---- | ---- |
| type | 'primary' | 'default' | 'default |  false  | 按钮类型 |

全部配置好后，需要重启一下dumi2项目，重启后就可以在浏览器看到效果了。

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6c054950d954412b9bc9d31c263eb57b~tplv-k3u1fbpfcp-watermark.image?)

### 3.5 添加单元测试

**准备工作**

添加好基础组件后，通常需要加上给组件添加测试代码，来保障组件的健壮性。

测试框架采用react最常用的jest工具，再配合react配套的单元测试库，安装依赖：

```
npm i jest <span class="hljs-symbol">@testing</span>-library/react <span class="hljs-symbol">@types</span>/jest ts-jest jest-environment-jsdom jest-less-loader typescript<span class="hljs-symbol">@4</span> -D
```

* Jest: jest单元测试核心库
* @testing-library/react：React的测试工具库，在React应用中进行单元测试、集成测试和端到端测试。
* @types/jest：jest的类型。
* ts-jest：让jest支持ts语法的预设。
* jest-environment-jsdom: jest的测试环境，使用js-dom库模拟dom环境，默认是node环境。
* jest-less-loader: jest不认识less和css，使用该插件使jest支持less和css。
* tytpescript: 这是安装了4版本，最新的5版本会有警告。

装好依赖后在项目根目录新建jest的配置文件jest.config.js

```
<span class="hljs-selector-tag">touch</span> <span class="hljs-selector-tag">jest</span><span class="hljs-selector-class">.config</span><span class="hljs-selector-class">.js</span>
```

在jest.config.js添加以下配置：

```

<span class="hljs-keyword">module</span>.<span class="hljs-keyword">exports</span> = {
  preset: <span class="hljs-string">'ts-jest'</span>,
  testEnvironment: <span class="hljs-string">'jsdom'</span>,
  roots: [<span class="hljs-string">'./src'</span>],
  collectCoverage: <span class="hljs-keyword">true</span>,
  coverageDirectory: <span class="hljs-string">'coverage'</span>,
  transform: {
    <span class="hljs-string">'\.(less|css)$'</span>: <span class="hljs-string">'jest-less-loader'</span>
  },

  collectCoverageFrom: [<span class="hljs-string">'src/**/*.tsx'</span>, <span class="hljs-string">'src/**/*.ts'</span>, <span class="hljs-string">'!src/index.ts'</span>, <span class="hljs-string">'!src/**/demo/*'</span>],
};
```

**编写单元测试**

在Button目录下新建一个 `__tests__`文件夹放置单元测试代码，在里面新建index.test.tsx。

```
<span class="hljs-keyword">mkdir</span> src/<span class="hljs-keyword">Button</span>/__tests__ && touch src/<span class="hljs-keyword">Button</span>/__tests__/index.test.tsx
```

在index.test.tsx文件中编写我们的单元测试代码

```

&#x200B;
<span class="hljs-keyword">import</span> React <span class="hljs-keyword">from</span> <span class="hljs-string">'react'</span>;
<span class="hljs-keyword">import</span> { render, fireEvent } <span class="hljs-keyword">from</span> <span class="hljs-string">'@testing-library/react'</span>;
<span class="hljs-keyword">import</span> Button <span class="hljs-keyword">from</span> <span class="hljs-string">'..'</span>;
&#x200B;
describe(<span class="hljs-string">'Button&#x7EC4;&#x4EF6;'</span>, () => {
  it(<span class="hljs-string">'&#x80FD;&#x591F;&#x6B63;&#x786E;&#x6E32;&#x67D3;&#x6309;&#x94AE;&#x6587;&#x5B57;'</span>, () => {
    <span class="hljs-keyword">const</span> buttonText = <span class="hljs-string">'&#x6309;&#x94AE;&#x6587;&#x5B57;'</span>;
    <span class="hljs-keyword">const</span> { getByRole } = render(<span class="xml"><span class="hljs-tag"><<span class="hljs-name">Button</span>></span>{buttonText}<span class="hljs-tag">Button</span>></span>);
    <span class="hljs-keyword">const</span> buttonElement = getByRole(<span class="hljs-string">'button'</span>);
    expect(buttonElement.innerHTML).toBe(buttonText);
  });
&#x200B;
  it(<span class="hljs-string">'&#x80FD;&#x591F;&#x6B63;&#x786E;&#x6E32;&#x67D3;&#x9ED8;&#x8BA4;&#x6837;&#x5F0F;&#x7684;&#x6309;&#x94AE;'</span>, () => {
    <span class="hljs-keyword">const</span> { getByRole } = render(<span class="xml"><span class="hljs-tag"><<span class="hljs-name">Button</span> ></span>&#x9ED8;&#x8BA4;&#x6309;&#x94AE;<span class="hljs-tag">Button</span>></span>);
    <span class="hljs-keyword">const</span> buttonElement = getByRole(<span class="hljs-string">'button'</span>);
    expect(buttonElement.classList.contains(<span class="hljs-string">'dumi-btn'</span>)).toBe(<span class="hljs-literal">true</span>);
  });
&#x200B;
  it(<span class="hljs-string">'&#x80FD;&#x591F;&#x6B63;&#x786E;&#x6E32;&#x67D3;&#x4E3B;&#x8981;&#x6837;&#x5F0F;&#x7684;&#x6309;&#x94AE;'</span>, () => {
    <span class="hljs-keyword">const</span> { getByRole } = render(<span class="xml"><span class="hljs-tag"><<span class="hljs-name">Button</span> <span class="hljs-attr">type</span>=<span class="hljs-string">"primary"</span>></span>&#x4E3B;&#x8981;&#x6309;&#x94AE;<span class="hljs-tag">Button</span>></span>);
    <span class="hljs-keyword">const</span> buttonElement = getByRole(<span class="hljs-string">'button'</span>);
    expect(buttonElement.classList.contains(<span class="hljs-string">'dumi-btn-primary'</span>)).toBe(<span class="hljs-literal">true</span>);
  });
&#x200B;
  it(<span class="hljs-string">'&#x80FD;&#x591F;&#x89E6;&#x53D1;&#x70B9;&#x51FB;&#x4E8B;&#x4EF6;'</span>, () => {
    <span class="hljs-keyword">const</span> handleClick = jest.fn();
    <span class="hljs-keyword">const</span> { getByRole } = render(<span class="xml"><span class="hljs-tag"><<span class="hljs-name">Button</span> <span class="hljs-attr">type</span>=<span class="hljs-string">"primary"</span> <span class="hljs-attr">onClick</span>=<span class="hljs-string">{handleClick}</span>></span>&#x70B9;&#x51FB;&#x6309;&#x94AE;<span class="hljs-tag">Button</span>></span>);
    <span class="hljs-keyword">const</span> buttonElement = getByRole(<span class="hljs-string">'button'</span>);
    fireEvent.click(buttonElement);

    expect(handleClick).toHaveBeenCalledTimes(<span class="hljs-number">1</span>);
  });
});
```

单元测试代码分别测试了按钮基础渲染，渲染默认样式，渲染主要样式以及点击测试功能。

保存后在命令行执行npx jest执行一下单元测试:

```
<span class="hljs-attribute">npx jest</span>
```

> jest会自动寻找目录中 `__tests__`文件夹，去执行内部以.test.{js, jsx, ts, tsx}结尾的文件。

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6fb62832cbdf49f584513268ce096da3~tplv-k3u1fbpfcp-watermark.image?)

可以看到执行了Button组件的单元测试，并且最后生成了测试报告，标注了测试了哪些文件，每个文件的覆盖率，测试情况。

并且会在项目根目录下生成测试报告的静态站点 **coverage**文件夹，在浏览器打开coverage/lcov-report/index.html文件，即可看到测试报告，通过对每个文件的分析，可以看到有哪些单元测试没覆盖到的地方，可以帮助我们更好的写单元测试。

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2e4394d3a0bf463e90d5d978c09c64ea~tplv-k3u1fbpfcp-watermark.image?)

## 四. 基于antd二次开发

有时候除了从0封装基础组件之外，还会基于antd等组件库进行二次开发，方式和开发基础组件是一样的，只是要在打包package包时注意css的引入。

### 4.1 添加组件代码

先安装antd库，安装到开发依赖，后面会添加到peerDependencies依赖中。

```
<span class="hljs-built_in">npm</span> i antd -D
```

先做一个基础的例子，二次封装一下antd的按钮组件, 让它是primary风格的组件。

在src下新建一个PrimaryButton文件夹，内建index.tsx

```
mkdir src/PrimaryButton && <span class="hljs-keyword">touch</span> src/PrimaryButton/<span class="hljs-built_in">index</span>.tsx
```

在index.tsx里面编写代码：

```

&#x200B;
<span class="hljs-keyword">import</span> React, { memo } <span class="hljs-keyword">from</span> <span class="hljs-string">"react"</span>;
<span class="hljs-keyword">import</span> { Button, ButtonProps } <span class="hljs-keyword">from</span> <span class="hljs-string">"antd"</span>;
&#x200B;
type IPrimaryButtonProps = Omit<buttonprops, <span class="hljs-string">'type'>
&#x200B;
<span class="hljs-keyword">const</span> PrimaryButton: React.FC<iprimarybuttonprops> = <span class="hljs-function">(<span class="hljs-params">props</span>) =></span> {
&#x200B;
  <span class="hljs-keyword">const</span> { children, ...rest } = props
&#x200B;
  <span class="hljs-keyword">return</span> (
    <span class="xml"><span class="hljs-tag"><<span class="hljs-name">Button</span> {<span class="hljs-attr">...rest</span>} <span class="hljs-attr">type</span>=<span class="hljs-string">'primary'</span>></span>
      {children}
    <span class="hljs-tag">Button</span>></span>
  );
};
&#x200B;
<span class="hljs-keyword">export</span> <span class="hljs-keyword">default</span> memo(PrimaryButton);
</iprimarybuttonprops></buttonprops,>
```

组件源代码添加好后，需要在src/index.ts中引入后暴露一下：

```

<span class="hljs-keyword">export</span> { <span class="hljs-keyword">default</span> <span class="hljs-keyword">as</span> PrimaryButton } <span class="hljs-keyword">from</span> <span class="hljs-string">'./PrimaryButton'</span>;
```

在这里引入并暴露出去以后，dumi会帮我门放在包名称dumi2-demo里面，就可以在项目中通过

`import { PrimaryButton } from 'dumi2-demo';`来引入了。

### 4.2 **添加demo示例**

在PrimaryButton目录下新建一个demo文件夹，内建一个基础演示base.tsx文件。

```
<span class="hljs-built_in">mkdir</span> src/PrimaryButton/<span class="hljs-built_in">demo</span> && touch src/PrimaryButton/<span class="hljs-built_in">demo</span>/base.tsx
```

添加组件示例代码：

```

&#x200B;
<span class="hljs-keyword">import</span> React <span class="hljs-keyword">from</span> <span class="hljs-string">'react'</span>;
<span class="hljs-keyword">import</span> { PrimaryButton } <span class="hljs-keyword">from</span> <span class="hljs-string">'dumi2-demo'</span>;
&#x200B;
<span class="hljs-keyword">export</span> <span class="hljs-keyword">default</span> () => {
&#x200B;
  <span class="hljs-keyword">return</span> (
    <span class="xml"><span class="hljs-tag"><<span class="hljs-name">PrimaryButton</span>></span>&#x9ED8;&#x8BA4;&#x6309;&#x94AE;<span class="hljs-tag">PrimaryButton</span>></span>
  );
}
```

### 4.3 添加组件文档

再在该文件同目录新建一个index.md文件作为文档说明，这也是生成静态文档站点所需要的。

```
<span class="hljs-keyword">touch</span> src/PrimaryButton/<span class="hljs-built_in">index</span>.md
```

添加文档内容，具体内容描述可以看官网[MakeDown配置项](https://d.umijs.org/config/markdown)，这里只在注释里面讲一下用到的配置。

```

<span class="hljs-attr">category:</span> <span class="hljs-string">Components</span>
<span class="hljs-attr">title:</span> <span class="hljs-string">PrimaryButton</span>
<span class="hljs-attr">toc:</span> <span class="hljs-string">content</span>
<span class="hljs-attr">group:</span>
  <span class="hljs-attr">title:</span> <span class="hljs-string">&#x4E8C;&#x6B21;&#x5C01;&#x88C5;&#x7EC4;&#x4EF6;</span>
  <span class="hljs-attr">order:</span> <span class="hljs-number">2</span>

<span class="hljs-string">&#x200B;</span>

<span class="hljs-string">&#x200B;</span>

<span class="hljs-string">&#x200B;</span>
<span class="hljs-string">&#x57FA;&#x7840;&#x7684;&#x6309;&#x94AE;&#x7EC4;&#x4EF6;</span> <span class="hljs-string">PrimaryButton&#x3002;</span>
<span class="hljs-string">&#x200B;</span>

<span class="hljs-string">&#x200B;</span>

<span class="hljs-string"><code< span> <span class="hljs-string">src="./demo/base.tsx">&#x57FA;&#x7840;&#x7528;&#x6CD5;</span></code<></span>
```

​

​

| 属性 | 类型                   | 默认值   | 必填 | 说明 |
| ---- | ---------------------- | -------- | ---- | ---- |
| size | 'small' | 'midlle' | 'large |  false  | 按钮大小 |

然后重启一下dumi2项目，可以看到页面上已经有最新添加的组件了。

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ba59ceceaeb74540aa2994b299850f36~tplv-k3u1fbpfcp-watermark.image?)

### 4.4 添加单元测试

在PrimaryButton目录下新建一个 `__tests__`文件夹放置单元测试代码，在里面新建index.test.tsx。

```
mkdir src<span class="hljs-regexp">/PrimaryButton/</span>__tests__ && touch src<span class="hljs-regexp">/PrimaryButton/</span>__tests__<span class="hljs-regexp">/index.test.tsx</span>
```

在index.test.tsx文件中编写我们的单元测试代码

```

&#x200B;
<span class="hljs-keyword">import</span> { render } <span class="hljs-keyword">from</span> <span class="hljs-string">'@testing-library/react'</span>;
<span class="hljs-keyword">import</span> { ButtonProps } <span class="hljs-keyword">from</span> <span class="hljs-string">'antd'</span>;
<span class="hljs-keyword">import</span> React <span class="hljs-keyword">from</span> <span class="hljs-string">'react'</span>;
<span class="hljs-keyword">import</span> PrimaryButton <span class="hljs-keyword">from</span> <span class="hljs-string">'..'</span>;
&#x200B;
describe(<span class="hljs-string">'PrimaryButton&#x6309;&#x94AE;'</span>, () => {
  <span class="hljs-keyword">const</span> buttonProps: ButtonProps = {
    <span class="hljs-attr">loading</span>: <span class="hljs-literal">false</span>,
    <span class="hljs-attr">onClick</span>: jest.fn(),
  };
&#x200B;
  it(<span class="hljs-string">'&#x6B63;&#x786E;&#x6E32;&#x67D3;&#x6309;&#x94AE;'</span>, () => {
    <span class="hljs-keyword">const</span> buttonText = <span class="hljs-string">'&#x70B9;&#x51FB;&#x6309;&#x94AE;'</span>;
    <span class="hljs-keyword">const</span> { getByRole } = render(
      <span class="xml"><span class="hljs-tag"><<span class="hljs-name">PrimaryButton</span> {<span class="hljs-attr">...buttonProps</span>}></span>{buttonText}<span class="hljs-tag">PrimaryButton</span>></span>,
    );
    <span class="hljs-keyword">const</span> buttonElement = getByRole(<span class="hljs-string">'button'</span>);
    expect(buttonElement.textContent).toBe(buttonText);
  });
&#x200B;
  it(<span class="hljs-string">'&#x6B63;&#x786E;&#x6E32;&#x67D3;&#x6309;&#x94AE;&#x9ED8;&#x8BA4;type'</span>, () => {
    <span class="hljs-keyword">const</span> { getByRole } = render(
      <span class="xml"><span class="hljs-tag"><<span class="hljs-name">PrimaryButton</span> {<span class="hljs-attr">...buttonProps</span>}></span>&#x70B9;&#x51FB;&#x6309;&#x94AE;<span class="hljs-tag">PrimaryButton</span>></span>,
    );
    <span class="hljs-keyword">const</span> buttonElement = getByRole(<span class="hljs-string">'button'</span>);
    expect(buttonElement.classList.contains(<span class="hljs-string">'ant-btn-primary'</span>)).toBe(<span class="hljs-literal">true</span>);
  });
});
```

一个基于antd4二次开发简单组件就封装好了，由于在封装的时候没有引入antd原Button组件的样式，打包后会出现样式丢失问题，在最后打包章节会有处理方式。

## 五. 开发工具函数

dum2i除了开发组件库之外，也能开发函数库，开发函数库要比组件库简单很多，而且不限制前端框架，vue，react里面都能使用。

### 5.1 添加工具函数代码

写一个时间格式化的工具函数，在src下新建一个formatTime文件夹，新增index.ts。

```
<span class="hljs-built_in">mkdir</span> src/formatTime && touch src/formatTime/<span class="hljs-built_in">index</span>.<span class="hljs-keyword">ts</span>
```

在index.ts里面编写代码：

```

&#x200B;

<span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">formatTime</span>(<span class="hljs-params">timestamp: number, format='YYYY-MM-DD hh:mm:ss'</span>): <span class="hljs-title">string</span> </span>{
  <span class="hljs-keyword">const</span> <span class="hljs-built_in">date</span> = <span class="hljs-keyword">new</span> <span class="hljs-built_in">Date</span>(timestamp);
  <span class="hljs-keyword">const</span> year = <span class="hljs-built_in">date</span>.getFullYear();
  <span class="hljs-keyword">const</span> month = (<span class="hljs-string">'0'</span> + (<span class="hljs-built_in">date</span>.getMonth() + <span class="hljs-number">1</span>)).slice(<span class="hljs-number">-2</span>);
  <span class="hljs-keyword">const</span> day = (<span class="hljs-string">'0'</span> + <span class="hljs-built_in">date</span>.getDate()).slice(<span class="hljs-number">-2</span>);
  <span class="hljs-keyword">const</span> hours = (<span class="hljs-string">'0'</span> + <span class="hljs-built_in">date</span>.getHours()).slice(<span class="hljs-number">-2</span>);
  <span class="hljs-keyword">const</span> minutes = (<span class="hljs-string">'0'</span> + <span class="hljs-built_in">date</span>.getMinutes()).slice(<span class="hljs-number">-2</span>);
  <span class="hljs-keyword">const</span> seconds = (<span class="hljs-string">'0'</span> + <span class="hljs-built_in">date</span>.getSeconds()).slice(<span class="hljs-number">-2</span>);
  <span class="hljs-keyword">const</span> <span class="hljs-attribute">map</span>: { [<span class="hljs-attribute">key</span>: <span class="hljs-built_in">string</span>]: <span class="hljs-built_in">string</span> } = {
    <span class="hljs-attribute">YYYY</span>: <span class="hljs-built_in">String</span>(year),
    <span class="hljs-attribute">MM</span>: month,
    <span class="hljs-attribute">DD</span>: day,
    <span class="hljs-attribute">hh</span>: hours,
    <span class="hljs-attribute">mm</span>: minutes,
    <span class="hljs-attribute">ss</span>: seconds,
  };
  <span class="hljs-keyword">return</span> format.replace(<span class="hljs-regexp">/YYYY|MM|DD|hh|mm|ss/g</span>, (matched) => map[matched]);
}
&#x200B;
<span class="hljs-keyword">export</span> <span class="hljs-keyword">default</span> formatTime;
```

组件源代码添加好后，需要在src/index.ts中引入后暴露一下：

```

<span class="hljs-keyword">export</span> { <span class="hljs-keyword">default</span> <span class="hljs-keyword">as</span> formatTime } <span class="hljs-keyword">from</span> <span class="hljs-string">'./formatTime'</span>;
```

在这里引入并暴露出去以后，dumi会帮我门放在包名称dumi2-demo里面，就可以在项目中通过

`import { formatTime } from 'dumi2-demo';`来引入了。

### 5.2 **添加demo示例**

在formatTime目录下新建一个demo文件夹，内建一个基础演示base.tsx文件:

```

<span class="hljs-keyword">import</span> React, { useEffect, useState } <span class="hljs-keyword">from</span> <span class="hljs-string">'react'</span>;
<span class="hljs-keyword">import</span> { formatTime } <span class="hljs-keyword">from</span> <span class="hljs-string">'dumi2-demo'</span>;
&#x200B;
<span class="hljs-keyword">const</span> App: React.FC = <span class="hljs-function"><span class="hljs-params">()</span> =></span> {
  <span class="hljs-keyword">const</span> [currentDate, setCurrentDate] = useState(formatTime(<span class="hljs-built_in">Date</span>.now(), <span class="hljs-string">'YYYY&#x5E74;MM&#x6708;DD&#x65E5; hh:mm:ss'</span>));
  <span class="hljs-keyword">const</span> [siteDate, setSiteDate] = useState<string>();
&#x200B;
  useEffect(<span class="hljs-function"><span class="hljs-params">()</span> =></span> {

    <span class="hljs-keyword">const</span> timestamp=<span class="hljs-number">1673850986000</span>
    <span class="hljs-keyword">const</span> siteStr: string = formatTime(timestamp);
    setSiteDate(siteStr);
  }, []);
&#x200B;
  useEffect(<span class="hljs-function"><span class="hljs-params">()</span> =></span> {

    <span class="hljs-keyword">const</span> timer = setInterval(<span class="hljs-function"><span class="hljs-params">()</span> =></span> {
      <span class="hljs-keyword">const</span> date = <span class="hljs-built_in">Date</span>.now();
      <span class="hljs-keyword">const</span> dateStr = formatTime(date, <span class="hljs-string">'YYYY&#x5E74;MM&#x6708;DD&#x65E5; hh:mm:ss'</span>);
      setCurrentDate(dateStr);
    }, <span class="hljs-number">1000</span>);
    <span class="hljs-keyword">return</span> <span class="hljs-function"><span class="hljs-params">()</span> =></span> {
      clearInterval(timer);
    }
  }, []);
&#x200B;
  <span class="hljs-keyword">const</span> inputRef = React.createRef<htmlinputelement>();
  <span class="hljs-keyword">const</span> onFormatData = <span class="hljs-function"><span class="hljs-params">()</span> =></span> {
    <span class="hljs-keyword">const</span> value = inputRef.current?.value;
    <span class="hljs-keyword">if</span> (value) {
      <span class="hljs-keyword">const</span> dateStr = formatTime(<span class="hljs-built_in">Number</span>(value), <span class="hljs-string">'YYYY&#x5E74;MM&#x6708;DD&#x65E5; hh:mm:ss'</span>);
      setSiteDate(dateStr);
    }
  };
&#x200B;
  <span class="hljs-keyword">return</span> (
    <span class="xml"><span class="hljs-tag"><></span>
      &#x5F53;&#x524D;&#x65F6;&#x95F4;&#xFF1A;{currentDate}
      <span class="hljs-tag"><<span class="hljs-name">hr</span> /></span>
      &#x6307;&#x5B9A;&#x65F6;&#x95F4;&#x8F6C;&#x6362;&#xFF1A;
      <span class="hljs-tag"><<span class="hljs-name">input</span> <span class="hljs-attr">type</span>=<span class="hljs-string">"number"</span> <span class="hljs-attr">ref</span>=<span class="hljs-string">{inputRef}</span> <span class="hljs-attr">defaultValue</span>=<span class="hljs-string">{1673850986000}</span> /></span>
      <span class="hljs-tag"><<span class="hljs-name">button</span> <span class="hljs-attr">type</span>=<span class="hljs-string">'button'</span> <span class="hljs-attr">onClick</span>=<span class="hljs-string">{onFormatData}</span>></span>&#x8F6C;&#x6362;<span class="hljs-tag">button</span>></span>
      {siteDate}
    <span class="hljs-tag"></></span>
  );
};
&#x200B;
<span class="hljs-keyword">export</span> <span class="hljs-keyword">default</span> App;
</htmlinputelement></string>
```

### 5.3 添加工具文档

在formatTime目录下新建一个index.md文件:

```
---
category: Components
title: &#x65F6;&#x95F4;&#x683C;&#x5F0F;&#x5316;
toc: content
group:
  title: &#x5DE5;&#x5177;&#x51FD;&#x6570;
  order: 3
---
&#x200B;

&#x200B;
&#x5C06;&#x65F6;&#x95F4;&#x6233;&#x683C;&#x5F0F;&#x5316;&#x6210;&#x6307;&#x5B9A;&#x7684;&#x65E5;&#x671F;&#x65F6;&#x95F4;&#x683C;&#x5F0F;&#x3002;
&#x200B;

&#x200B;

&#x200B;
&#x57FA;&#x7840;&#x7528;&#x6CD5;
```

​

​
| 参数名    | 类型   | 是否必填 | 默认值                  | 说明                                                       |
| --------- | ------ | -------- | ----------------------- | ---------------------------------------------------------- |
| timestamp | number | 是       | -                       | 要格式化的时间戳，单位为毫秒                               |
| format    | string | 否       | `'YYYY-MM-DD hh:mm:ss'` | 要格式化成的日期时间格式，默认为 `'YYYY-MM-DD hh:mm:ss'`。 |
​

​
类型：string
格式化后的日期时间字符串。

然后重启一下dumi项目，可以看到页面上已经有最新添加的formatTime函数了。

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3193fd15a0c24320857c723868689e8a~tplv-k3u1fbpfcp-watermark.image?)

### 5.4 添加单元测试

在formatTime目录下新建一个 `__tests__`文件夹放置单元测试代码，在里面新建index.test.tsx。

在index.test.tsx文件中编写我们的单元测试代码

```

<span class="hljs-keyword">import</span> formatTime <span class="hljs-keyword">from</span> <span class="hljs-string">'..'</span>;
&#x200B;
describe(<span class="hljs-string">'formatTime'</span>, <span class="hljs-function"><span class="hljs-params">()</span> =></span> {
  it(<span class="hljs-string">'&#x6B63;&#x786E;&#x683C;&#x5F0F;&#x5316;&#x6307;&#x5B9A;&#x65F6;&#x95F4;'</span>, <span class="hljs-function"><span class="hljs-params">()</span> =></span> {
    <span class="hljs-keyword">const</span> timestamp = <span class="hljs-number">1681216363389</span>;
    <span class="hljs-keyword">const</span> formattedDate = formatTime(timestamp, <span class="hljs-string">'YYYY&#x5E74;MM&#x6708;DD&#x65E5; hh&#x65F6;mm&#x5206;ss&#x79D2;'</span>);
    expect(formattedDate).toEqual(<span class="hljs-string">'2023&#x5E74;04&#x6708;11&#x65E5; 20&#x65F6;32&#x5206;43&#x79D2;'</span>);
  });
&#x200B;
  it(<span class="hljs-string">'&#x9ED8;&#x8BA4;&#x683C;&#x5F0F;&#x5316;&#x6307;&#x5B9A;&#x65F6;&#x95F4;'</span>, <span class="hljs-function"><span class="hljs-params">()</span> =></span> {
    <span class="hljs-keyword">const</span> timestamp = <span class="hljs-number">1681216363389</span>;
    <span class="hljs-keyword">const</span> formattedDate = formatTime(timestamp);
    expect(formattedDate).toEqual(<span class="hljs-string">'2023-04-11 20:32:43'</span>);
  });
});
```

到这里工具函数库的示例也就添加成功了。

## 六. jest单元测试优化

刚才开发了两个组件和一个工具函数，并且都编写了jest单元测试，现在再来执行一下npx jest看一下效果:

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ef20ed8ec50748da9979a91b2ccc2c20~tplv-k3u1fbpfcp-watermark.image?)

可以看到每个单元测试执行情况和覆盖率。

### 6.1 全量单元测试

每次手动执行执行npx不太方便，可以把命令添加在package.json的脚本script标签里面。

```

&#x200B;
<span class="hljs-string">"scripts"</span>: {
  <span class="hljs-string">"test:all"</span>: <span class="hljs-string">"jest --coverage --bail"</span>
}
```

jest的命令参数:

* --bail: 遇到测试用例失败时立即停止测试，不再执行剩余的测试用例，可以节省时间。
* --coverage：生成单元测试覆盖率报告。
* --findRelatedTests: 可以指定要执行的单元测试文件。

再次执行npm run test:all就可以进行单元测试了。

### 6.2 按需单元测试

直接npx jest的方式是全量进行单元测试，但在正常开发过程中一般只需要测试我们修改了的方法，不需要每一次都进行全量测试，可以有效节省时间。

可以自己写一个脚本，通过git diff --staged --diff-filter=ACMR --name-only命令获取到本次修改的文件列表，然后进行分析需要执行哪些单元测试，通过--findRelatedTests参数去精准执行对应的单元测试文件。

按这个思路在项目根目录新建jest.staged.js，添加代码：

```
<span class="hljs-keyword">const</span> fs = <span class="hljs-built_in">require</span>(<span class="hljs-string">'fs'</span>).promises;
<span class="hljs-keyword">const</span> path = <span class="hljs-built_in">require</span>(<span class="hljs-string">'path'</span>);
<span class="hljs-keyword">const</span> { execSync } = <span class="hljs-built_in">require</span>(<span class="hljs-string">'child_process'</span>);
&#x200B;

<span class="hljs-keyword">async</span> <span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">start</span>() </span>{

  <span class="hljs-keyword">const</span> addFiles = execSync(<span class="hljs-string">`git diff --staged --diff-filter=ACMR --name-only`</span>)
    .toString()
    .split(<span class="hljs-string">'\n'</span>);

  <span class="hljs-keyword">const</span> diffFileList = addFiles
    .filter(<span class="hljs-built_in">Boolean</span>)
    .map(<span class="hljs-function">(<span class="hljs-params">item</span>) =></span> path.join(__dirname, item));
&#x200B;

  <span class="hljs-keyword">const</span> srcPath = path.join(__dirname, <span class="hljs-string">'./src'</span>);
&#x200B;

  <span class="hljs-keyword">const</span> diffFileMap = {};
  diffFileList.forEach(<span class="hljs-function">(<span class="hljs-params">filePath</span>) =></span> {
    <span class="hljs-keyword">if</span> (
      filePath.includes(srcPath) &&
      (filePath.endsWith(<span class="hljs-string">'.ts'</span>) || filePath.endsWith(<span class="hljs-string">'.tsx'</span>))
    ) {
      <span class="hljs-keyword">const</span> relativePath = path.relative(srcPath, filePath);
      <span class="hljs-keyword">if</span> (relativePath.includes(<span class="hljs-string">'/'</span>)) {
        diffFileMap[relativePath.split(<span class="hljs-string">'/'</span>)[<span class="hljs-number">0</span>]] = <span class="hljs-literal">true</span>;
      }
    }
  });
&#x200B;
  <span class="hljs-built_in">console</span>.log(<span class="hljs-string">'&#x672C;&#x6B21;&#x6539;&#x52A8;&#x7684;&#x65B9;&#x6CD5;'</span>, <span class="hljs-built_in">Object</span>.keys(diffFileMap));
&#x200B;

  <span class="hljs-keyword">const</span> list = (
    <span class="hljs-keyword">await</span> <span class="hljs-built_in">Promise</span>.all(
      <span class="hljs-built_in">Object</span>.keys(diffFileMap).map(<span class="hljs-keyword">async</span> (toolPath) => {
        <span class="hljs-keyword">const</span> testsDir = path.join(srcPath, toolPath, <span class="hljs-string">'__tests__'</span>);
        <span class="hljs-keyword">try</span> {
          <span class="hljs-keyword">const</span> files = <span class="hljs-keyword">await</span> fs.readdir(testsDir);
          <span class="hljs-keyword">return</span> files.map(<span class="hljs-function">(<span class="hljs-params">item</span>) =></span> path.join(testsDir, item));
        } <span class="hljs-keyword">catch</span> (error) {
          <span class="hljs-keyword">return</span> [];
        }
      }),
    )
  ).flat();
&#x200B;

  <span class="hljs-keyword">if</span> (list.length) {
    <span class="hljs-keyword">try</span> {
      execSync(<span class="hljs-string">`npx jest --bail --findRelatedTests <span class="hljs-subst">${list.join(<span class="hljs-regexp">/ /</span>)}</span>`</span>, {
        cwd: __dirname,
        stdio: <span class="hljs-string">'inherit'</span>,
      });
    } <span class="hljs-keyword">catch</span> (error) {
      process.exit(<span class="hljs-number">1</span>);
    }
  }
}
&#x200B;
start();
```

写好脚本代码后，在package.json的脚本script标签里面。

```

&#x200B;
<span class="hljs-string">"scripts"</span>: {
  <span class="hljs-string">"test:staged"</span>: <span class="hljs-string">"node jest.staged.js"</span>
}
```

然后每次修改代码提交前都可以通过执行npm run test:staged来精确执行修改到的单元测试文件。

> 前提项目要git init，有.git文件

也可以把命令加到lint-staged里面每一次提交代码时自动执行按需执行jest单元测试。

lint-staged在dumi项目创建时已经默认安装依赖配置好了，我们只要修改一下package.json的lint-staged字段就可以了。

```
// package.json
&#x200B;
<span class="hljs-string">"lint-staged"</span>: {
  <span class="hljs-string">"src/**/*.{md,json}"</span>: [
    <span class="hljs-string">"prettier --write --no-error-on-unmatched-pattern"</span>
  ],
  <span class="hljs-string">"src/**/*.{css,less}"</span>: [
    <span class="hljs-string">"stylelint --fix"</span>,
    <span class="hljs-string">"prettier --write"</span>
  ],
  <span class="hljs-string">"src/**/*.{ts, tsx}"</span>: [
    <span class="hljs-string">"eslint --fix"</span>,
    <span class="hljs-string">"prettier --parser=typescript --write"</span>,
    <span class="hljs-string">"npm run test:staged"</span>
  ]
},
```

再改一下.husky/pre-commit文件把里面的npx lint-staged改为npx lint-staged -p true -v，这样才能在执行lint-staged的时候在控制台看到打印的信息。

```

. <span class="hljs-string">"<span class="hljs-variable">$(dirname -- "$0")</span>/_/husky.sh"</span>
&#x200B;
npx lint-staged -v
```

现在每次git commit提交代码如果组件或函数代码有改动，会通过lint-staged触发npm run test:staged命令，然后再内部使用git获取到修改到的文件，对文件进行分析，去执行对应组件或工具函数下面的jest单元测试。

## 七. 打包部署

在组件或者工具函数开发完成后，就需要进行部署操作了，部署分为两部分部署：

一是打包文档静态站点文档，让用户可以通过域名访问到文档站点，方便其使用。

二是打包组件库源码，部署到npm仓库上面，让其他人可以通过npm安装使用。

### 7.1 打包静态站点

打包静态站点dumi2在创建项目时已经配置好了命令，只需要在控制台执行

```
npm <span class="hljs-keyword">run</span><span class="bash"> docs:build</span>
```

打包完成后会在项目中生成docs-dist文件夹，该文件夹就是部署静态文档站点的静态资源。

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ef8944e639bf4a5fbf32184a7e3a0669~tplv-k3u1fbpfcp-watermark.image?)

在本地可以借助serve起一个服务托管静态进行测试一下，全局安装serve:

```
<span class="hljs-built_in">npm</span> i serve -g
```

安装完成后在项目根目录执行命令托管文档静态站点：

```
<span class="hljs-attribute">serve -s docs-dist</span>
```

打开提示的服务访问链接[http://localhost:3000/](http://localhost:3000/)，就可以在浏览器访问到打包后的静态站点，实际情况下需要部署到服务器上面，可以借助nginx来部署。

### 7.2优化静态站点打包

在静态站点默认配置下，会把每一个组件或者函数单独打包一份静态文件在components文件夹下，在上图我们也可以看到，但实际上一般是不需要再单独生成一份的，可以修改打包配置，取消打包单个静态资源。

修改.dumirc.ts文件

```
<span class="hljs-keyword">import</span> { defineConfig } <span class="hljs-keyword">from</span> <span class="hljs-string">'dumi'</span>;
&#x200B;
<span class="hljs-keyword">export</span> <span class="hljs-keyword">default</span> defineConfig({

  <span class="hljs-attr">exportStatic</span>: <span class="hljs-literal">false</span>
});
```

再次打包就不会生成单个组件的文档站点了，可以有效节省打包时间和打包体积。

### 7.3 打包npm源码包

静态站点打包好后，就需要打包组件和函数库最终的npm包了，dumi2也在创建项目时就提供了npm包打包的命令，直接执行:

```
npm <span class="hljs-keyword">run</span><span class="bash"> build</span>
```

打包完成后会在项目中生成dist文件夹，该dist文件夹就是最终要发布到npm仓库上的源码。

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e428861a86fd4fa18d7c5b262e32c786~tplv-k3u1fbpfcp-watermark.image?)

发布的时候除了dist文件之外，还需要在package.json里面做配置:

把antd添加到，表示使用该组件库，必须要先安装antd对应版本。

```
<span class="hljs-string">"peerDependencies"</span>: {
  <span class="hljs-string">"react"</span>: <span class="hljs-string">">=16.9.0"</span>,
  <span class="hljs-string">"react-dom"</span>: <span class="hljs-string">">=16.9.0"</span>,
  <span class="hljs-string">"antd"</span>: <span class="hljs-string">">=5.4.2"</span>
},
```

package.json的name字段对应npm包的版本号，第一次发可以0.0.1，后面再发就需要修改版本号。

其他的字段files和module，types在项目初始化的时候dumi就帮我们设置好了。

然后去npm官网注册账号，在命令行通过npm login进行登录，最后回到项目目录，打开命令行，输入

```
<span class="hljs-built_in">npm</span> publish
```

就可以发布到npm仓库了。

### 7.4 优化npm源码打包

在上面打包源码包图中，我们可以看到在函数源码下面依然有demo文件夹，但实际使用过程中是不会用到的，可以通过配置在打包npm源码包的时候把demo文件夹过滤掉。

因为打包npm源码是用father来打包的，所以我们要修改.fatherrc.ts配置文件:

```
<span class="hljs-keyword">import</span> { defineConfig } <span class="hljs-keyword">from</span> <span class="hljs-string">'father'</span>;
&#x200B;
<span class="hljs-keyword">export</span> <span class="hljs-keyword">default</span> defineConfig({
  <span class="hljs-attr">esm</span>: {

    <span class="hljs-attr">ignores</span>: [
      <span class="hljs-string">'src/**/demo/**'</span>,
    ],
  },

});
&#x200B;
```

再一次打包就会发现demo文件夹不会出现在最终的npm包dist文件夹里面了。

### 7.5 解决antd打包npm后没有样式

上面虽然基于antd封装好了按钮，在文档预览没有问题，但是在把npm包引入使用的时候会出现样式丢失的问题，有三个常见的解决方案。

1. 直接全局引入antd的样式，但这样虽然可以解决问题，但是会引入很多不必要的css资源，增加项目体积，所以不推荐。
2. 在二次封装的组件内手段引入对应antd组件的样式，这样虽然可以解决样式丢失问题，并且支持按需引入，但需要手动加比较麻烦。
3. 可以在npm包打包配置.fatherrc.ts添加antd按需引入css的配置，安装按需引入依赖：

```
<span class="hljs-built_in">npm</span> i babel-plugin-<span class="hljs-keyword">import</span> -D
```

然后在.fatherrc.ts添加extraBabelPlugins配置

```
<span class="hljs-keyword">import</span> { defineConfig } <span class="hljs-keyword">from</span> <span class="hljs-string">'father'</span>
&#x200B;
<span class="hljs-keyword">export</span> <span class="hljs-keyword">default</span> defineConfig({

  <span class="hljs-attr">extraBabelPlugins</span>: [
    [
      <span class="hljs-string">'babel-plugin-import'</span>,
      {
        <span class="hljs-attr">libraryName</span>: <span class="hljs-string">'antd'</span>,
        <span class="hljs-attr">libraryDirectory</span>: <span class="hljs-string">'es'</span>,
        <span class="hljs-attr">style</span>: <span class="hljs-literal">true</span>,
      },
    ],
  ],
})
```

添加配置后打包npm包就会添加上antd的样式了。

### 7.6 解决组件不能按需引入

在发布到npm上面后本地安装使用时发现组件库没有tree-shaking效果。

```
<span class="hljs-built_in">npm</span> i dumi2-demo -S
&#x200B;
<span class="hljs-keyword">import</span> { PrimaryButton } <span class="hljs-keyword">from</span> <span class="hljs-string">'dumi2-demo'</span>
```

问题原因是由样式less文件引起的，构建工具认为样式有副作用，所以没有进行tree-shaking操作，解决方案只需要在dmi2组件代码的package.json里面加上sideEffet，告诉构建工具这个npm包没有副作用可以进行tree-shaking。

修改组件库的package.json，添加：

```
{

  <span class="hljs-attr">"slideEffects"</span>: <span class="hljs-literal">false</span>,
}
```

修改后使用组件就会按需加载，有tree-shking效果了。

## 八. 总结

到目前为止，一个基础的组件库和函数库demo就开发好了，在开发过程中也遇到了一些问题，还有待优化的地方，比如现在打包的是es模块，有时候也会需要支持common模块和在浏览器上面直接使用的umd，以及打包时把less编译为css，还有开发移动端的组件。

本文是总结自己在工作中使用dumi2搭建组件库和函数库中使用到的配置, 也很多没有做好的地方，后续有好的使用技巧和配置也会继续更新记录，附上本文demo相关的地址：

demo静态文档站点地址：[https://dumi2.guojiongwei.top](https://dumi2.guojiongwei.top)

demo源码github地址：[https://github.com/guojiongwei/dumi2-demo](https://github.com/guojiongwei/dumi2-demo)

demo npm包地址：[https://www.npmjs.com/package/dumi2-demo](https://www.npmjs.com/package/dumi2-demo)

往期前端工程化系列文章:

[【前端工程化】webpack5从零搭建完整的react18+ts开发和打包环境](https://juejin.cn/post/7111922283681153038)

[【前端工程化】配置React+ts项目完整的代码及样式格式和git提交规范](https://juejin.cn/post/7101596844181962788)

[【前端工程化】使用verdaccio搭建公司npm私有库完整流程和踩坑记录](https://juejin.cn/post/7096701542408912933)

[【前端工程化】vue2+webpack3老项目迁移vite2流程和踩坑总结](https://juejin.cn/post/7094130826471964708)
