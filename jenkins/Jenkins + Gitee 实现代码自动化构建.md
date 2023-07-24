---
link: null
title: Jenkins + Gitee 实现代码自动化构建 （超级详细）-腾讯云开发者社区-腾讯云
description: 1： 搭建jenkins线上服务， 参考Linux环境下安装Jenkins
 2：注册gitee账号，并创建一个项目，这里我的项目名是 lvsige-demo(下面简称demo)
keywords: jenkins,git,shell,vue.js
author: null
date: 2022-06-15T13:56:32.000Z
publisher: null
stats: paragraph=24 sentences=3, words=91
---
1： 搭建jenkins线上服务， 参考[Linux环境下安装Jenkins](/developer/tools/blog-entry?target=http://101.35.120.154/archives/12) 2：注册gitee账号，并创建一个项目，这里我的项目名是 lvsige-demo(下面简称demo)

2：检查jenkins配置配置文件，将执行用户改成root,不然后面可能出现执行shell没有权限

如果重启服务报错， 可以 ps -ef|grep jenkins 查看jenkins进程 然后 kill -9 xxx(进程)

然后重启 jenkins 命令：service jenkins start

3： 安装Gitee插件(系统管理->插件管理->可选插件->筛选Gitee->选中直接安装，安装成功之后重启jenkins服务)

4：添加Gitee(码云)链接配置(系统管理->系统配置->Gitee配置)

然后点击 "添加" 即可。添加之后， 点击测试链接， 显示成功ok。 如果爆红提示没有权限，检查上一步，你的帐密输错了没。

5: 创建一个自由风格的任务，命名test，按照图片配置选项

6: Gitee(码云)配置

这里就开始构建了。

> 至此， 你已经成功了。 下面是我遇到的问题

报错是这样的， 说明没有用户名密码，解决办法

> 解决方案 执行 git config --global credential.helper store 命令 然后git push origin master // master就是分支的名字 会让你输入用户名和密码，这时你输入就好了， 然后下次再git push 或者/pull 的时候就不用密码了

完结撒花！

> 这个时候你在本地修改后 执行 git push. jenkins就会开始构建，构建的时候执行shell里脚本， 进入你的项目文件夹，git pull。 然后刷新，就已经是最新代码了。

我觉得这个文章已经很详细了，因为我在这个jenkins自动化部署上已经消磨了一整天了。 希望大家可以避坑！

今天shell里执行的是一个简单的html文件。 回头需要自动化部署vue， 应该会有点麻烦， 我还没有看， 后续继续更新！

时隔一夜，我来更新了， 自动化部署vue项目的步骤。 其实思路是先用命令执行一遍，能走的通，直接把命令粘贴在shell里就行了

首先说一下我自己的目录，这个根据自己的情况而定。

我的项目是在 /www/wwwroot 下。 nginx访问文件是在 /www/wwwroot/test下。

我用的是8084端口， root /www/wwwroot/test/dist; 这个dist就是项目打包后的静态文件。

部署vue项目和H5唯一不同的点就是，执行shell的命令不一样，下面是我shell执行的命令， 可以参照一下， 路径一定根据自己的情况变化。

这个时候， 只要你执行 git push 命令，jenkins就会开始构建部署。

大功告成！
