---
link: https://blog.csdn.net/huzhenv5/article/details/105232187
title: package.json中的type字段含义—— node官方翻译
description: package.json的“type”字段如果最近的package.json文件包含一个顶级字段“type”，其值为“module”，则以.js结尾或没有任何扩展名的文件将作为ES模块进行加载。最近的package.json被定义为第一个在当前文件夹、该文件夹的父文件夹等中搜索时发现的package.json，直到到达卷的根目录。// package.json{  "type": "mo..._package.json type:module
keywords: package.json type:module
author: Huzhenv5 Csdn认证博客专家 Csdn认证企业博客 码龄7年 暂无认证
date: 2020-03-31T13:41:14.000Z
publisher: null
stats: paragraph=12 sentences=21, words=30
---
如果最近的package.json文件包含一个顶级字段"type"，其值为"module"，则以.js结尾或没有任何扩展名的文件将作为ES模块进行加载。

最近的package.json被定义为第一个在当前文件夹、该文件夹的父文件夹等中搜索时发现的package.json，直到到达卷的根目录。

```json

{
  "type": "module"
}
```

> 在上述package.json所在的文件夹
node --experimental-modules my-app.js #my-app.js将作为ES模块运行

如果最近的package.json缺少"type"字段，或者包含"type":"commonjs"，则无扩展名的文件和.js结尾文件将被视为commonjs。如果一直到卷根，还是没找到package.json, Node.js则按默认规则运行，就像package.json中没有"type"字段。"无扩展"指的是不包含扩展名的文件路径，而不是在说明符中选择性地删除文件扩展名。

如果最近的父package.json包含"type":"module"，则.js结尾的文件和和无扩展文件的导入语句将被视为ES模块。

```js

import './startup.js';
```

包的作者应该在package.json中指明"type"字段，即使在所有源都是CommonJS的包中也是如此。如果Node.js的默认类型发生了变化，那么明确包的类型将使包在将来不会受到影响，而且它还使构建工具和加载程序更容易确定包中的文件应该如何解释。

不管"type"字段的值是多少，.mjs文件总是被当作ES模块，而.cjs文件总是被当作CommonJS。

总的来说，关于type，知道下面4点就行了：
