---
link: https://zhuanlan.zhihu.com/p/137861452
title: 如何在 Ubuntu 20.04 上安装 Visual Studio Code
description: 本文最先发布在： 如何在 Ubuntu 20.04 上安装 Visual Studio CodeVisual Studio Code 是一个由微软开发的强大的开源代码编辑器。它包含内建的调试支持，嵌入的 Git 版本控制，语法高亮，代码自动完成，集成终端，…
keywords: Ubuntu,Visual Studio Code,Microsoft Visual Studio 2019
author: 雪梦科技
date: 2020-05-02T16:21:00.000Z
publisher: 知乎专栏
stats: paragraph=32 sentences=6, words=137
---
本文最先发布在：

Visual Studio Code 是一个由微软开发的强大的开源代码编辑器。它包含内建的调试支持，嵌入的 Git 版本控制，语法高亮，代码自动完成，集成终端，代码重构以及代码片段功能。

Visual Studio Code 是跨平台的，在 Windows, Linux, 和 macOS 上可用。

这篇指南显示了两种在 Ubuntu 20.04 上安装 Visual Studio Code 的方式。 VS Code 可以通过 [Snapcraft 商店](https://link.zhihu.com/?target=https%3A//snapcraft.io/store)或者微软源仓库中的一个 deb 软件包来安装。

选择最适合你的环境的安装方式。

## 一、作为一个 Snap 软件包安装 Visual Studio Code

Visual Studio Code snap 软件包由微软来进行分发和维护。

Snaps 是一种自包含的软件包，它包含需要运行这个应用所有的依赖。 Snap 软件包容易升级，并且非常安全。和标准的 deb 软件包不同，snaps 需要占用更大的磁盘空间，和 更长的应用启动时间。

Snap 软件包可以通过命令行或者 Ubuntu 软件应用来安装。

想要安装 VS Code snap版，打开你的终端(`Ctrl+Alt+T`)并且运行下面的命令：

就这些。Visual Studio Code 已经在你的 Ubuntu 机器上安装好了，你可以开始使用它了。

如果你喜欢使用 GUI 图形界面，打开 Ubuntu 软件中心，并且搜索"Visual Studio Code",然后安装应用：

不管何时，当新版本发布时，Visual Studio Code 软件包都会在后台被自动升级。

## 二、使用 apt 安装 Visual Studio Code

Visual Studio Code 在官方的微软 Apt 源仓库中可用。想要安装它，按照下面的步骤来：

01.以 sudo 用户身份运行下面的命令，更新软件包索引，并且安装依赖软件：

02.使用 wget 命令插入 Microsoft GPG key ：

启用 Visual Studio Code 源仓库，输入：

03.一旦 apt 软件源被启用，安装 Visual Studio Code 软件包：

当一个新版本被发布时，你可以通过你的桌面标准软件工具，或者在你的终端运行命令，来升级 Visual Studio Code 软件包：

## 三、启动 Visual Studio Code

在 Activities 搜索栏输入 "Visual Studio Code"，并且点击图标，启动应用。

当你第一次启动 VS Code 时，一个类似下面的窗口应该会出现：

你可以开始安装插件，并且根据你的喜好配置 VS Code 了。

VS Code 也可以通过在终端命令行输入 `code`进行启动。

## 四、总结

我们将会讲解如何在 Ubuntu 20.04 上安装 VS Code。

如果你有任何疑问，请通过以下方式联系我们：

微信: sn0wdr1am86

微信群: 加上面的微信，备注微信群

QQ: 3217680847

QQ 群: 82695646
