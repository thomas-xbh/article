---
link: https://zhuanlan.zhihu.com/p/506750413
title: 使用hexo+icarus快速搭建属于自己的博客网站
description: 准备环境安装nodejs✅安装git✅安装hexo✅ # 此为全局安装，可能需要sudo权限 npm install -g hexo-cli创建git仓库直接在github主页创建一个新的仓库，此处假设仓库名称为blog_tensorrt 使用hexo建初始博客首先初…
keywords: Hexo,网站搭建
author: 尔哟​
date: 2022-04-28T09:30:00.000Z
publisher: 知乎专栏
stats: paragraph=21 sentences=4, words=13
---
## **准备环境**

* 安装nodejs✅
* 安装git✅
* 安装hexo✅

## **创建git仓库**

直接在github主页创建一个新的仓库，此处假设仓库名称为blog_tensorrt

## **使用hexo建初始博客**

首先初始化一个博客项目，此处blog可以换成自己想要起的名称。该操作之后在当前目录下会出现一个叫做blog的新的文件夹

进入blog文件夹下

可以看到当前的文件夹下有一个themes的文件夹，此时看到里面没有文件，下载icarus主题代码到其中

之后修改_config.yml文件，将theme修改为icarus

之后在命令行进行构建

输入生成命令可能会报错，提示有没有安装的包，安装确实的包

接着生成

查看网页初始效果

## **自定义博客设计**

## **部署网站**

首先修改_config.yml文件

之后进行本地查看

之后接着修改_config.yml文件

安装部署需要的包

之后部署

在仓库里面setting里面修改github pages的none为master分支，点击save，等待一会之后就可以在访问自己刚刚部署到的网站了
