---
title: 使用Hexo和Github搭建博客
date: 2023-08-16 16:33:14
toc: true
categories:
- 其他
tags:
- Hexo
---

### 前言
Hexo是一款基于Node.js的博客框架,优点是易于安装使用,同时也提供许多主题供用户使用,可以将生成的静态网页放在Github托管,不需要服务器即可通过url访问博客,下面将从零开始搭建一个博客.lets start it
<!-- more -->
### 前置环境
> Node.js v16.16.0
> git version 2.41.0.windows.1
### 安装Hexo
全局安装hexo-cli
> npm install -g hexo-cli

安装完成后,初始化博客
> hexo init blog 

初始化完成后,打开blog目录可以看到

![image.png](https://s2.loli.net/2023/08/16/jNUHD4AQidYBkE1.png)

可以通过输入下面两条命令来检测
> hexo g //生成静态页面
> hexo s //启动本地服务

[打开loclhost](http://localhost:4000/)查看是否有下面界面
![image.png](https://s2.loli.net/2023/08/16/AqQFSOoCf7J4u8w.png)
到目前为止Hexo就安装成功啦
### Github配置
现在要将网站推送到Github上面
1. 创建Github仓库
> 注意:仓库名称为 "Github用户名.github.io"

2. 生成token
在Github的settings=>developer Settings=>personal access tokens设置token
![image.png](https://s2.loli.net/2023/08/16/dbnhFa4vjBHMmxN.png)
进行如下配置
![image.png](https://s2.loli.net/2023/08/16/fhT5G7Zo2mbOKzN.png)
然后创建即可
3. 将token放入Hexo中
打开blog目录下的_config.yml文件,进行如下配置
```yml
deploy:
  type: git
  repo: https://生成的token@仓库地址
  branch: 'main'
```
4. 部署
> hexo clean //清除缓存
> hexo g 
> hex d //部署页面
5. 验证
通过在浏览器输入仓库名来访问,验证是否部署成功;到这里基本的blog就完成了,下篇文章将会介绍如何使用icarus给blog添加更好看的主题