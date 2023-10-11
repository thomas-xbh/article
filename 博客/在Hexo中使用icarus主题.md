---
title: 在Hexo中使用icarus主题
date: 2023-08-16 17:21:09
toc: true
categories:
- 其他
tags:
- Hexo
---
### 前言
接下来主要介绍icarus基础使用,和常见错误，完成以下操作步骤将初步完成 Hexo 博客 + icarus 主题的配置。后续可以对 Hexo 博客进行功能扩展。来到这里相信你已经正确安装 Hexo 博客的基础运行环境了,如果不会可以看前一篇文件.
<!-- more -->
### 前置环境
>Node
>Git
>Hexo
### 开始
在博客目录下clone icarus 到themes/icarus中
>  git clone git@github.com:ppoffice/hexo-theme-icarus.git /themes/icarus
然后修改blog中_config.yml里面的theme配置
> theme: icarus

然后进行测试
> hexo g
> hexo s

如果允许报错,需要安装一些需要的包
> npm i bulma-stylus@0.8.0 hexo-component-inferno@^1.1.0 hexo-pagination@^2.0.0 hexo-renderer-inferno@^0.1.3 inferno@^7.3.3 inferno-create-element@^7.3.3

访问本地localhost:4000,是否出现如下界面
![image.png](https://s2.loli.net/2023/08/16/8YclMZEeBXiC34I.png)

关于icarus更详细的配置,可以去阅读官方文档

### 易错点
##### 修改Hexo样式
个人觉得icarus三栏两边的间距太多了,想修改一下,打开控制台发现只需要修改边距就行了
![image.png](https://s2.loli.net/2023/08/16/TMD9oQNFLX1qAO8.png)
发现对应的样式文件在publi/css/default.css中,随即就修改了;运行部署发现没有问题,但是在Hexo中有一个`hexo clean`命令,使用该命令会清空publi下的文件,使用`hexo g`会重新生成初始的public目录,这....,不就相当与我白修改的么.经过网上查阅,发现只要把修改后的css文件放到source文件中,下次重新生成就直接从source中复制过去,完美解决问题
过来几天我又发现一个问题,那就是在hexo中有个搜索功能,这是通过索引source中的内容输出的,但是我们不是把css文件也放进去了么,这就导致在搜索页面有三个很丑的文件
![image.png](https://s2.loli.net/2023/08/16/hFOVI63b8NyQ7Bc.png)
又经过百度,发现一个折中办法,还得配置_config.yml,设为false即可
```js
search:
    type: insight
    # 可选项。设置为false时从搜索结果中排除所有page
    index_pages: false
```
这样一个基本的博客框架就出来啦,可以多阅读文档,把界面改成自己喜欢的样子吧,加油!