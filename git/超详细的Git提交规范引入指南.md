---
link: https://juejin.cn/post/6844903793033756680
title: 超详细的Git提交规范引入指南
description: 规范Commit message不仅能解决上述问题，而且基本没有副作用和学习成本，应该尽早加上。 为了实现规范，我们使用commitlint和husky 来进行提交检查，当执行git commit时会在对应的git钩子上做校验，只有符合格式的Commit message才能提交…
keywords: Git
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2019-03-08T15:33:34.000Z
publisher: 稀土掘金
stats: paragraph=48 sentences=10, words=449
---
> 最近公司的前端团队分了组，我根据兴趣加入了基础设施建设组，负责做一些方便和规范开发的东西。第一个产出是增加了Git的提交规范，之前参与开源项目时接触到的，感觉很有意思，也很实际，用得到。

在开发过程中，Git每次提交代码，都需要写Commit message（提交说明），规范的Commit message有很多好处：

* 方便快速浏览查找，回溯之前的工作内容
* 可以直接从commit 生成Change log(发布时用于说明版本差异)

目前我们并没有对commit message进行规范，造成以下麻烦：

* 每个人风格不同，格式凌乱，查看很不方便
* 部分commit没有填写message，事后难以得知对应修改的作用

**规范Commit message不仅能解决上述问题，而且基本没有副作用和学习成本，应该尽早加上。**

为了实现规范，我们使用[commitlint](https://link.juejin.cn?target=https%3A%2F%2Fmarionebl.github.io%2Fcommitlint%2F%23%2F "https://marionebl.github.io/commitlint/#/")和[husky](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Ftypicode%2Fhusky "https://github.com/typicode/husky") 来进行提交检查，当执行 `git commit`时会在对应的git钩子上做校验，只有符合格式的Commit message才能提交成功。

有兴趣的小伙伴可以查阅相关文档。

为了方便使用，我们避免了过于复杂的规定，格式较为简单且不限制中英文：

```
<type>(<scope>): <subject>
// &#x6CE8;&#x610F;&#x5192;&#x53F7; : &#x540E;&#x6709;&#x7A7A;&#x683C;
// &#x5982; feat(miniprogram): &#x589E;&#x52A0;&#x4E86;&#x5C0F;&#x7A0B;&#x5E8F;&#x6A21;&#x677F;&#x6D88;&#x606F;&#x76F8;&#x5173;&#x529F;&#x80FD;
</subject></scope></type>
```

**scope选填**表示commit的作用范围，如数据层、视图层，也可以是目录名称 **subject必填**用于对commit进行简短的描述 **type必填**表示提交类型，值有以下几种：

* feat - 新功能 feature
* fix - 修复 bug
* docs - 文档注释
* style - 代码格式(不影响代码运行的变动)
* refactor - 重构、优化(既不增加新功能，也不是修复bug)
* perf - 性能优化
* test - 增加测试
* chore - 构建过程或辅助工具的变动
* revert - 回退
* build - 打包

**如何加入项目**

```

npm i commitlint --save-dev
npm i @commitlint/config-conventional --save-dev

<span class="hljs-built_in">module</span>.exports = {
  <span class="hljs-attr">extends</span>: [<span class="hljs-string">'@commitlint/config-conventional'</span>],
  <span class="hljs-attr">rules</span>: {

    <span class="hljs-string">'type-enum'</span>: [<span class="hljs-number">2</span>, <span class="hljs-string">'always'</span>, [
      <span class="hljs-string">"feat"</span>,
      <span class="hljs-string">"fix"</span>,
      <span class="hljs-string">"docs"</span>,
      <span class="hljs-string">"style"</span>,
      <span class="hljs-string">"refactor"</span>,
      <span class="hljs-string">"perf"</span>,
      <span class="hljs-string">"test"</span>,
      <span class="hljs-string">"chore"</span>,
      <span class="hljs-string">"revert"</span>,
      <span class="hljs-string">"build"</span>
    ]],

    <span class="hljs-string">'subject-case'</span>: [<span class="hljs-number">0</span>]
  }
};

npm i husky --save-dev

<span class="hljs-string">"husky"</span>: {
  <span class="hljs-string">"hooks"</span>: {
    <span class="hljs-string">"commit-msg"</span>: <span class="hljs-string">"commitlint -E HUSKY_GIT_PARAMS"</span>
  }
}

npm install commitizen -g

npm install commitizen --save-dev
commitizen init cz-customizable --save --save-exact

<span class="hljs-string">"config"</span>: {
  <span class="hljs-string">"commitizen"</span>: {
    <span class="hljs-string">"path"</span>: <span class="hljs-string">"./node_modules/cz-customizable"</span>
  }
}

;

<span class="hljs-built_in">module</span>.exports = {
  <span class="hljs-attr">types</span>: [
    {<span class="hljs-attr">value</span>: <span class="hljs-string">'feat'</span>,     <span class="hljs-attr">name</span>: <span class="hljs-string">'feat:     &#x65B0;&#x529F;&#x80FD;'</span>},
    {<span class="hljs-attr">value</span>: <span class="hljs-string">'fix'</span>,      <span class="hljs-attr">name</span>: <span class="hljs-string">'fix:      &#x4FEE;&#x590D;'</span>},
    {<span class="hljs-attr">value</span>: <span class="hljs-string">'docs'</span>,     <span class="hljs-attr">name</span>: <span class="hljs-string">'docs:     &#x6587;&#x6863;&#x53D8;&#x66F4;'</span>},
    {<span class="hljs-attr">value</span>: <span class="hljs-string">'style'</span>,    <span class="hljs-attr">name</span>: <span class="hljs-string">'style:    &#x4EE3;&#x7801;&#x683C;&#x5F0F;(&#x4E0D;&#x5F71;&#x54CD;&#x4EE3;&#x7801;&#x8FD0;&#x884C;&#x7684;&#x53D8;&#x52A8;)'</span>},
    {<span class="hljs-attr">value</span>: <span class="hljs-string">'refactor'</span>, <span class="hljs-attr">name</span>: <span class="hljs-string">'refactor: &#x91CD;&#x6784;(&#x65E2;&#x4E0D;&#x662F;&#x589E;&#x52A0;feature&#xFF0C;&#x4E5F;&#x4E0D;&#x662F;&#x4FEE;&#x590D;bug)'</span>},
    {<span class="hljs-attr">value</span>: <span class="hljs-string">'perf'</span>,     <span class="hljs-attr">name</span>: <span class="hljs-string">'perf:     &#x6027;&#x80FD;&#x4F18;&#x5316;'</span>},
    {<span class="hljs-attr">value</span>: <span class="hljs-string">'test'</span>,     <span class="hljs-attr">name</span>: <span class="hljs-string">'test:     &#x589E;&#x52A0;&#x6D4B;&#x8BD5;'</span>},
    {<span class="hljs-attr">value</span>: <span class="hljs-string">'chore'</span>,    <span class="hljs-attr">name</span>: <span class="hljs-string">'chore:    &#x6784;&#x5EFA;&#x8FC7;&#x7A0B;&#x6216;&#x8F85;&#x52A9;&#x5DE5;&#x5177;&#x7684;&#x53D8;&#x52A8;'</span>},
    {<span class="hljs-attr">value</span>: <span class="hljs-string">'revert'</span>,   <span class="hljs-attr">name</span>: <span class="hljs-string">'revert:   &#x56DE;&#x9000;'</span>},
    {<span class="hljs-attr">value</span>: <span class="hljs-string">'build'</span>,    <span class="hljs-attr">name</span>: <span class="hljs-string">'build:    &#x6253;&#x5305;'</span>}
  ],

  messages: {
    <span class="hljs-attr">type</span>: <span class="hljs-string">'&#x8BF7;&#x9009;&#x62E9;&#x63D0;&#x4EA4;&#x7C7B;&#x578B;:'</span>,

    customScope: <span class="hljs-string">'&#x8BF7;&#x8F93;&#x5165;&#x4FEE;&#x6539;&#x8303;&#x56F4;(&#x53EF;&#x9009;):'</span>,
    <span class="hljs-attr">subject</span>: <span class="hljs-string">'&#x8BF7;&#x7B80;&#x8981;&#x63CF;&#x8FF0;&#x63D0;&#x4EA4;(&#x5FC5;&#x586B;):'</span>,
    <span class="hljs-attr">body</span>: <span class="hljs-string">'&#x8BF7;&#x8F93;&#x5165;&#x8BE6;&#x7EC6;&#x63CF;&#x8FF0;(&#x53EF;&#x9009;&#xFF0C;&#x5F85;&#x4F18;&#x5316;&#x53BB;&#x9664;&#xFF0C;&#x8DF3;&#x8FC7;&#x5373;&#x53EF;):'</span>,

    footer: <span class="hljs-string">'&#x8BF7;&#x8F93;&#x5165;&#x8981;&#x5173;&#x95ED;&#x7684;issue(&#x5F85;&#x4F18;&#x5316;&#x53BB;&#x9664;&#xFF0C;&#x8DF3;&#x8FC7;&#x5373;&#x53EF;):'</span>,
    <span class="hljs-attr">confirmCommit</span>: <span class="hljs-string">'&#x786E;&#x8BA4;&#x4F7F;&#x7528;&#x4EE5;&#x4E0A;&#x4FE1;&#x606F;&#x63D0;&#x4EA4;&#xFF1F;(y/n/e/h)'</span>
  },
  <span class="hljs-attr">allowCustomScopes</span>: <span class="hljs-literal">true</span>,

  skipQuestions: [<span class="hljs-string">'body'</span>, <span class="hljs-string">'footer'</span>],

  subjectLimit: <span class="hljs-number">72</span>
};

```

使用 `npm run changelog`自动生成CHANGELOG.md文件。

* New features
* Bug fixes
* Breaking changes(不向上兼容的部分，我们的规范不要求footer，所以这一项不会出现)

每个部分都会罗列相关的 commit ，并且有指向这些 commit 的链接。当然，生成的文档允许手动修改，所以发布前，你还可以添加其他内容。

**如何加入项目**

```

npm i conventional-changelog-cli --save-dev

<span class="hljs-string">"scripts"</span>: {
  <span class="hljs-string">"changelog"</span>: <span class="hljs-string">"conventional-changelog -p angular -i CHANGELOG.md -s"</span>
}

npm run changelog
```

答：目前不行。调研之后发现，之所以会出现这个问题，是因为这个插件的理想状况是贡献者通过pr来提交代码，并且要求pr前rebase，合并出唯一的commit，以免污染主分支的git log，在这种情况下，从主分支生成的changelog自然就是非常干净不重复的。

目前我们是每个人都fork了相关项目，但对主仓库都有修改权限，也没有作要求，所以现在并不是通过pr到主仓库的，而是直接push upstream 到主仓库分支然后合并到st。也没有做过git rebase，这就导致了以下几个问题：

1）开发时同一功能的多个commit因为没有经历 `git rebase`所以仍然是多个，生成changelog时就会重复 2）目前changelog的commit链接会指向我们自己fork的仓库而不是我们实际跟踪到的upstream 3）目前changelog的commit链接是https而我们的gitlab是http

目前的解决方案：

大概看了下这个库的可配置项比较有限，也没有类似的功能。所以只能： 1.尝试给该库提pr，但这个原因是咱们自己不太规范所以pr可能不容易通过 2.fork了魔改一下作为私有库或者自己造个新轮子

距离最终解决还需要点时间。

答：应要求增加了[commitizen](https://link.juejin.cn?target=https%3A%2F%2Fwww.jianshu.com%2Fp%2F36d970a2b4da "https://www.jianshu.com/p/36d970a2b4da")支持，能够使用 `git cz`来提交，但cz-customizable这个包的配置范围有限，部分冗余功能无法去除。后在github上提了[PR](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fleonardoanalista%2Fcz-customizable%2Fpull%2F65 "https://github.com/leonardoanalista/cz-customizable/pull/65")，已通过。将cz-customizable升级至v5.4.0并配置skipQuestions即可。另外，commitizen实际上只起提示作用，如果使用时不规范也并不会做校验，做不到强制要求。所以仍需要配合commitlint和husky来使用。大家可以根据自己的情况选择使用 `git commit`或 `git cz`。

答：可以。
