---
link: https://juejin.cn/post/7103352367197716516
title: Centos7安装Git
description: 持续创作，加速成长！这是我参与「掘金日新计划 · 6 月更文挑战」的第4天，点击查看活动详情 前言 版本控制是企业级开发过程中必不可少的东西，也是我们每天都需要频繁接触的东西，从入职第一天起做的第一件
keywords: 后端
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2022-05-30T02:35:33.000Z
publisher: 稀土掘金
stats: paragraph=44 sentences=30, words=89
---
持续创作，加速成长！这是我参与「掘金日新计划 · 6 月更文挑战」的第4天，[点击查看活动详情](https://juejin.cn/post/7099702781094674468 "https://juejin.cn/post/7099702781094674468")

# 前言

版本控制是企业级开发过程中必不可少的东西，也是我们每天都需要频繁接触的东西，从入职第一天起做的第一件和工作有关的是就是需要和版本控制打交道。这也说明版本控制在开发工作中的重要性。

版本控制其实就是为了做一个数据的备份，管理我们的版本，记录每一次提交、每一次改动，可以进行版本回退的一种管理工具。除了用来管理项目的代码，同样也可以用来管理项目的文档。

常用的版本控制系统有SVN、GIT。目前企业中这两种工具都在使用，历史悠久的项目（比如SSM）依然在用SVN，新一点的项目（比如微服务）使用的是GIT。但是SVN和GIT的选择并不是因为使用什么项目而决定的，只是项目的一个技术选择而已，不论是单体项目还是微服务项目都可以选择SVN或者GIT作为版本控制工具。但是GIT比SVN还是有很多有点的，比如整个团队协作上，版本分支上的管理是优于SVN的，具体的差异对比可参考其他网络相关文档。

今天主要是介绍如何在电脑上安装GIT客户端，GIT服务端一般由公司的运维团队或者环境组搭建，开发者或者使用者只需要在本地安装客户端即可。常用的服务端比如有个人开发者使用的GitHub、Gitee。企业中有使用Gitlab、bitbuckt等。以上的服务端都可以通过GIT客户端进行访问。

# 环境说明

Centos7、JDK1.8、Maven3.6.3、 `Git2.9.5`

# 下载地址

[git-scm.com/downloads](https://link.juejin.cn?target=https%3A%2F%2Fgit-scm.com%2Fdownloads "https://git-scm.com/downloads")

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7eb1dd1234d2415bb70e0039c9b73a7b~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp?)

选择对应的环境下载即可。下载完成后我们可以通过xftp等工具将下载好的安装包上传到Centos7系统中，然后进行安装。再多介绍一种在Centos中下载安装包的方式。首先复制下载链接，然后通过Centos系统的wget命令下载安装包。

```ruby
wget https://mirrors.edge.kernel.org/pub/software/scm/git/git-2.9.5.tar.gz
复制代码
```

如果提示没有wget命令，请通过以下命令进行安装即可，然后再执行以上的下载命令。

```
yum install wget
<span class="copy-code-btn">&#x590D;&#x5236;&#x4EE3;&#x7801;</span>
```

# 开始安装

## 安装依赖

git的运行环境需要用到第三方库，需要先安装第三方依赖库。

```shell
yum -y install curl-devel expat-devel gettext-devel openssl-devel zlib-devel

yum -y install gcc perl-ExtUtils-MakeMaker
复制代码
```

> Windows环境不需要先安装第三方依赖库，下载完成后，双击安装，一直下一步即可

## 移除历史版本

```shell
yum remove git
复制代码
```

## 解压压缩包

```shell
首先通过cd命令进入到安装包所在的目录,然后执行
cd /opt
tar -zxvf git-2.9.5.tar.gz
复制代码
```

## 进入解压目录

```shell
cd git-2.9.5/
复制代码
```

## 预编译并指定安装路径

```shell
./configure --prefix=/usr/local/git_2.9.5
复制代码
```

## 编译并安装

```shell
make && make install
复制代码
```

## 创建软链接

```shell
ln -s /usr/local/git_2.9.5/bin/* /usr/bin/
复制代码
```

## 配置环境变量

```shell
vim /etc/profile
git_2.9.5 environment

export GIT_HOME=/usr/local/git_2.9.5

export PATH=$PATH:$GIT_HOME/bin

source /etc/profile
复制代码
```

> 添加软链接和配置环境变量都是为了便于执行脚本，这样执行git命令的时候不用到git的安装目录，在路径执行都可以。添加软链接和配置环境变量任选一个操作即可，同时都进行配置也没问题

## 刷新环境变量

```shell
source /etc/profile
复制代码
```

## 验证

```shell
git -version
复制代码
```

输出git的版本号，即代表安装成功。
