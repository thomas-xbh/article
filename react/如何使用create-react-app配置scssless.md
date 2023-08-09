---
link: https://juejin.cn/post/6844904181942222862
title: 如何使用create-react-app配置scss/less
description: 我们发现，项目正常执行；如果没有安装node-sass与sass-loader，将css文件的后缀改为.scss后，项目会报错，提示安装node-sass。 答案：不可以。 注意：如果执行了eject后，那么node_modules是会被重置的，就不会再有react-scrip…
keywords: React.js
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2020-06-06T14:41:35.000Z
publisher: 稀土掘金
stats: paragraph=47 sentences=29, words=81
---
在我们使用create-react-app来快速搭建脚手架时，我们发现生成的项目中并没有明显的暴露出webpack相关配置，那么我们要使用scss/less该如何进行配置呢？

本文采用react版本号：^16.13.1

create-react-app版本号：3.4.1

## 一、配置scss

配置scss最简单，只需要安装node-sass和sass-loader即可（why?后面有说明）：

```css
npm install --save node-sass sass-loader复制代码
```

安装完成之后，将相应的css文件后缀改为.scss即可

![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/6/6/172890d949e021d8~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.image)

我们发现，项目正常执行；如果没有安装node-sass与sass-loader，将css文件的后缀改为.scss后，项目会报错，提示安装node-sass。

## 二、配置less

从配置sass来看，我们是不是只需要安装一下less-loader就可以了呢？

答案：不可以。

我们尝试发现将css文件后缀名改为.less，发现项目正常运行，页面样式异常，检查样式发现less文件中的样式未加载出来；

![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/6/6/172899dc536bd1a0~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.image)

### 步骤1，先寻找webpack配置文件（两种方法）

**方法一、通过执行如下命令将webpack配置文件暴露出来：**

```arduino
npm run eject复制代码
```

![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/6/6/1728a1109769b616~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.image)

执行成功后我们会发现，package.json内容（含npm run xxx的相应命令配置）发生了变化，依赖包进行了重置，目录中生成了config和scripts两个文件

![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/6/6/17289acb95a669a4~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.image)

_注意：如果没有修改本地任何文件及内容，__执行npm run eject__则正常（成功以后此命令被删除）；如果修改了本地文件及内容后执行__npm run eject，出现__报错_

即要在执行eject之前先执行如下命令：

```csharp
git init
git add .
git commit -m 'xxx'复制代码
```

展开config文件夹，我们可以找到webpack配置文件，如下图所示：

![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/6/6/17289bb35d3a81c6~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.image)

**方法二、我们不用通过npm run eject，直接在此目录node_modules/react-scripts/config下找到webpack配置文件**

![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/6/6/17289c3cc7936db8~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.image)

注意：如果执行了eject后，那么node_modules是会被重置的，就不会再有react-scripts这个依赖包（即react-scripts依赖包替换为config和scripts文件暴露出来）。

通过对照两种方式下的webpack.config.js文件内容是相同的；

### 步骤2，修改webpack配置

通过查看代码，我们发现webpack.config.js对style的处理是内置了(scss|sass)相应的处理，难怪在使用sass时，只需要下载node-sass和sass-loader即可；

![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/6/6/17289ba4a0e1a92f~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.image)

同理，我们加上相应的less配置即可，添加代码(3处)如下：

![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/6/6/17289d49a5bc7d1c~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.image)

![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/6/6/17289d8f0cf020c3~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.image)

![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/6/7/1728cff47f273afd~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.image)

其实就是复制一下sass相关配置代码，并将其修改为less相关配置即可；

接下来，只需要安装一下less-loader即可：

```css
npm install --save less-loader复制代码
```

安装完成之后，我们将css文件后缀名都改为.less，看看效果是否与css展示一样：

![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/6/6/17289e1ff9fd76e4~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.image)

![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/6/6/17289e37f67aeb87~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.image)

大功告成，less加载出来与css结果一样。

## 三、antd按需加载配置

方法一：npm run eject后在package.json中配置如下：

安装插件：**babel-plugin-import**后配置如下

```json
"babel": {    "presets": [      "react-app"    ],    "plugins": [      ["import", {        "libraryName": "antd",        "libraryDirectory": "es",        "style": "css"      }]    ]  }复制代码
```


