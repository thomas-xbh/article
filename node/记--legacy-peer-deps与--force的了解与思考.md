---
link: https://juejin.cn/post/7168127182579957790
title: 记--legacy-peer-deps与--force的了解与思考
description: 讲在前面 先介绍一下两者分别的作用： --legacy-peer-deps 当install时忽略所有对等依赖（peerDependencies），以npm版本4到6的样式安装。 ignore all
keywords: 前端
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2022-11-20T15:54:59.000Z
publisher: 稀土掘金
stats: paragraph=32 sentences=13, words=130
---
## 讲在前面

先介绍一下两者分别的作用：

### --legacy-peer-deps

当install时忽略所有对等依赖（peerDependencies），以npm版本4到6的样式安装。

ignore all peerDependencies when installing, in the style of npm version 4 through version 6.

### --force或-f

将强制npm获取远程资源即使在磁盘上有个本地的副本。

will force npm to fetch remote resources even if a local copy exists on disk.

### --strict-peer-deps（了解）

遇到任何冲突的对等依赖项时，失败并中止安装进程。默认情况下，npm只会因根目录的直接依赖关系引起的peerDependency冲突而崩溃

fail and abort the install process for any conflicting _peerDependencies_ when encountered. By default, npm will only crash for _peerDependencies_ conflicts caused by the direct dependencies of the root project.

## 问题与解决

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/131709c76c4d4dee9b816ded604df74e~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp?)

当遇到这种依赖冲突时，可在运行命令时增加这些flags（--force,or --legacy-peer-deps）以解决这些冲突。

### 为什么可以解决？

#### --legacy-peer-deps

其实从上文其作用来看也比较清楚了，忽略所有对等依赖，以npm版本4到6的样式安装。 但是，从npm v7开始如果在安装过程中发现没有安装相关依赖就会默认安装peerDendencies；所以说遇到对等依赖冲突也是在npm v7开始的，所以说以npm v4 through v6安装的话不会安装peerDependenices.

In the new version of npm (v7), by default, `npm install` will fail when it encounters conflicting _peerDependencies_.

npm install 使用 npm v6, 依赖项中不会存在peerDependencies

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2ced92665d104cd7a08d80bce08f77f6~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp?)

使用v7以上版本，则默认安装peerDependencies

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/62ef03d35c284f119efc3162404e2bf3~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp?)

#### --force

无视冲突，强制从远程仓库中获取资源并覆盖掉本地资源； (理解：有点像强制执行用户某个指令的意思)

### 通过网上找的资料，另外再比较一下同版本的npm使用--legacy-peer-deps与--force安装某个依赖时的区别：

--legacy-peer-deps

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d1ce1001793a493c99305b22ac3bf165~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp?)

--force

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7b450410240a4aba9ac03469df4663fd~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp?)

可以看出来，--force绑定的相关依赖更为可能严格准确一些。

As you see, `npm install --force` still pins many dependency versions which is stricter.

#### 关于这两个flags探究目前暂时这么多了，感觉npm极其复杂，后续有了解再回来补充，文章如有错误以及大神们有任何指导，十分欢迎留言！！
