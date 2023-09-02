### 前言

我们在工作中使用git的时候一般都是手写commit信息的,但是当项目人多时,会造成提交信息混乱,毕竟每个人习惯不一样,那有没有提供一个规范呢

### 开始

规范肯定是有的,我第一次了解这个,还是在看vben的文档,发现存在这个规范

![image.png](https://s2.loli.net/2023/08/20/tq3DmTnMXe76WBU.png)

当然,现在提供了很多工具来进行约束

### Commitizen

使用commitizen/cz-cli可以快速生成提交说明

> npm install -g commitizen

我们可以使用官方提供的模板

> commitizen init cz-conventional-changelog --save --save-exact

然后在package.json中新增config.commitizen文件,配置适配器的路径

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

完成之后使用`git cz`来代替git commit -m ''

![image.png](https://s2.loli.net/2023/08/20/HuON3GZit7qE6Dj.png)

### 自定义规范

如果想自定义规范可以使用cz-customizable

> npm install cz-customizable --save-dev

将之前的package.json修改如下

```js
"config": {
  "commitizen": {
    "path": "node_modules/cz-customizable"
  }
}
```

在根目录下创建.cz-config.js

```js
'use strict';

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

  // it needs to match the value for field type. Eg.: 'fix'
  /*
  scopeOverrides: {
    fix: [
      {name: 'merge'},
      {name: 'style'},
      {name: 'e2eTest'},
      {name: 'unitTest'}
    ]
  },
  */
  // override the messages, defaults are as follows
  messages: {
    type: '选择一种你的提交类型:',
    scope: '选择一个scope (可选):',
    // used if allowCustomScopes is true
    customScope: 'Denote the SCOPE of this change:',
    subject: '短说明:\n',
    body: '长说明，使用"|"换行(可选)：\n',
    breaking: '非兼容性说明 (可选):\n',
    footer: '关联关闭的issue，例如：#31, #34(可选):\n',
    confirmCommit: '确定提交说明?'
  },

  allowCustomScopes: true,
  allowBreakingChanges: ['特性', '修复'],

  // limit subject length
  subjectLimit: 100

};
```

到目前为止,就完成了提交信息的规范了,但是这只是规范,可能会有人不遵守这个规范,直接使用git commit -m ''来提交...

这时候就要用到一个校验工具了

## Commitizen校验

安装校验工具

> npm install --save-dev @commitlint/cli

安装符合Angular风格的校验规则

> npm install --save-dev @commitlint/config-conventional 

在根目录新建commitlint.config.js

```js
module.exports = {
  extends: ['@commitlint/config-conventional']
};
```

安装huksy

> npm install husky --save-dev

在package.json中配置

```js
"husky": {
  "hooks": {
    "commit-msg": "commitlint -E HUSKY_GIT_PARAMS"
  }  
}
```