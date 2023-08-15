---
link: https://juejin.cn/post/7144961058191441933
title: 我的Ubuntu使用体验心得
description: Ubuntu和Windows在日常使用方面有什么区别呢，Ubuntu究竟适不适合作为日常使用操作系统呢？
keywords: Linux
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2022-09-19T05:38:55.000Z
publisher: 稀土掘金
stats: paragraph=32 sentences=8, words=164
---
> 此文章用来记录一下自己使用 `Ubuntu` 操作系统日常办公开发的一些使用心得。适合想了解 `Ubuntu` 适不适合作为日常开发办公和想从 `Windows` 转为 `Linux` 操作系统却有些犹豫不决的读者阅读。

## 2023-01-06日更新

> 补充一下相关软件

* [QQ Linux](https://link.juejin.cn?target=https%3A%2F%2Fim.qq.com%2Flinuxqq%2Findex.shtml "https://im.qq.com/linuxqq/index.shtml") 最新版本已经发布，使用了几天下来体验还不错。
* [有道云笔记 Linux](https://link.juejin.cn?target=https%3A%2F%2Fnote.youdao.com%2Fnote-download%2F "https://note.youdao.com/note-download/")
* 经过网友提醒，钉钉，飞书，腾讯会议，TODESK，向日葵这些办公套件都有了 `Linux` 版本，所以大家可以放心的加入 `Linux` 阵营了

## 为什么要使用 Ubuntu ？

`Windows` 难道不香吗？香！

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6c764df34851473db2e4f749b801f1c5~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp?)

图片来源网络，侵删

如果是日常办公和游戏，不涉及开发工作的话， `Windows` 绝对是不二之选。

但是如果要做一些开发工作，使用 `Linux` 就有了以下优势

* 本地开发环境要更加贴近服务器的环境（ `windows server` 除外），开发上更加方便，对于一些编程语言，数据库软件等的支持要更好。
* 可以让自己更加熟悉 `Linux` 操作系统（现在的招聘要求很多都有要熟练 `Linux` 操作系统[!doge]），包括软件的安装， `shell` 终端的使用，服务器环境的配置等等。

当然现在 `Windows`内置 `WSL2`和 `Docker` 容器技术的加持下， `Windows` 下的开发体验已经很完善了。这里没有哪个操作系统更好的观点，以免引起不必要的争论🤣

## Ubuntu 使用体验如何？

我是在 18 年开始使用 [Ubuntu 桌面版](https://link.juejin.cn?target=https%3A%2F%2Fubuntu.com%2Fdownload%2Fdesktop "https://ubuntu.com/download/desktop")，那时候是 `16.LTS` 版本。现在已经是 `22.LTS`。但是考虑到升级系统的不确定性，我目前一直使用的是 `18.LTS`。

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0cf0308419fc4daaa62fd34709898ed9~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp?)

`Ubuntu` 是 `Linux` 操作系统的一个发行版，类似的发行版还有很多，有国产的优秀代表 [`Deepin`;](https://link.juejin.cn?target=https%3A%2F%2Fwww.deepin.org%2Findex%2Fzh "https://www.deepin.org/index/zh")等。这里只说明我使用过的发行版 `Ubuntu` 介绍使用体验

* 本身有着广泛的社区支持，较完善的软件库，友好的界面，基本可视化界面已经很完善了，即使是小白，不管是安装还是使用都比较简单。

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4e8421a755304d068ee1aecf0f2561db~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp?)

* 在使用过程中基本未出现过死机，崩溃等情况，即使是突然断电，也经受住考验，未出现系统损坏（当然不建议如此操作，还是会存在损坏系统的概率）
* 在笔者电脑硬件下系统响应流畅，平时不关机，只在系统有更新时候重启。平时多开 `VsCode`、 `GoLand`、 `Chrome`、 `Edge` 的情况下长时间使用后内存使用率较高，但是很少出现卡顿，无响应状态（偶尔软件本身的原因会导致反应迟钝）。
* 工作沟通使用网页版 钉钉和微信，钉钉网页版足够满足日常使用。微信网页版则只支持简单的聊天，发送文件，复杂功能不支持
* 中文输入法目前有 `&#x641C;&#x72D7;` 输入法和 `&#x767E;&#x5EA6;` 输入法，安装时需要折腾一下

## 常用开发套件

* 宇宙级编辑器 `VsCode` 和 `JetBrain` 家的全家桶都是跨平台的，完全不用担心使用问题
* 开发常用浏览器 `Chrome`, `FireFox`和 `Edge`都完全支持，推荐 `Edge`浏览器
* 各种编程语言支持，内置 `python3`、 `php` 等，开箱即用
* `Vim` 大法开箱即用，做为开发怎么能不会 `Vim` 呢，在终端下编辑文件，少不了 `Vim` 编辑器，逼格瞬间提升
* `Docker`完全支持 ，使用命令行操作 `Docker`更加快速便捷
* 抓包工具 `Charles` 和 `Fiddler Everywhere` 都有 `Linux` 版

## 常用软件的适配情况

* 聊天工具钉钉微信截止目前只有网页版， `QQ`有 `Linux` 版本，但是还停留在远古时代，基本只可聊天
* 中文输入法日常使用没有问题，偶尔会出现小问题，但是不影响基本使用
* 娱乐软件QQ音乐和网易云音乐都有 `Linux` 版本
* 百度云，坚果云，有道云笔记都有 `Linux` 版本，印象笔记只有网页版
* 办公套件有 `Wps Linux` 版，使用体验一样
* 截图软件有 `flameshot`等，使用体验不如 `Windows`上截图工具，但是基本使用没问题

## 不适合使用的情况

如果是以下软件的重度使用者，不推荐使用

* 微信等小程序开发者， `Linux` 上虽然有别人贡献的开源的微信开发者工具，但是目前基本处于停止维护状态，可以正常使用，但是无法升级，一些功能也不完善，容易崩溃
* 微信桌面端， 微信目前并未对 `Ubuntu` 的适配微信软件，当然可以通过 `Wine` 来解决，但是毕竟不是原生，容易有 `bug`，所以极度依赖微信的不建议
* 依赖 `IE`, 一堆国产浏览器工作的时候不适合
* 必须使用 微软Office 办公用户

## 总结

上面说的软件安装方式基本都是官网下载，双击安装，不用担心不会安装的问题。

对于需要终端操作安装的也没有太多难度，同时还可以提升自己的Shell脚本的熟练度。

所以如果要想熟练使用 `Linux`，想进一步提升自己相关的技能，完全推荐使用，更改自己的习惯总是难的，但是适应以后就特别的舒服，会越来越得心应手。

对于选择哪个发行版问题，我当然推荐自己使用的 `Ubuntu`，当然入门使用 `Deepin`也不错，在聊天工具封装这块做的很好，基本上属于傻瓜式使用了。

谢谢大家的支持～
