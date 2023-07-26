---
link: https://juejin.cn/post/7147952174536851469
title: 【项目部署】使用Jenkins一键打包部署SpringBoot应用
description: 我报名参加金石计划1期挑战——瓜分10万奖池，这是我的第4篇文章，点击查看活动详情 前言 嗨，大家好，我是希留，一个被迫致力于全栈开发的老菜鸟。 一般而言，一个项目部署的由：拉取代码->构建->测试-
keywords: 后端,运维
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2022-09-27T07:05:32.000Z
publisher: 稀土掘金
stats: paragraph=62 sentences=83, words=264
---
我报名参加金石计划1期挑战——瓜分10万奖池，这是我的第4篇文章，[点击查看活动详情](https://s.juejin.cn/ds/jooSN7t "https://s.juejin.cn/ds/jooSN7t")

# 前言

嗨，大家好，我是希留，一个被迫致力于全栈开发的老菜鸟。

一般而言，一个项目部署的由：拉取代码->构建->测试->打包->部署等过程组成，如果我们经常需要部署项目，特别是在微服务时代，服务特别多的情况下，不停的测试打包部署，那估计得有个人一整天专门做这事了，而这事又是繁琐的重复无意义的，所以就需要一套能够持续集成、持续交付、持续部署的自动化构建流程。

[Jenkins](https://link.juejin.cn?target=https%3A%2F%2Fwww.jenkins.io%2Fzh%2F "https://www.jenkins.io/zh/")是开源CI&CD软件领导者，提供超过1000个插件来支持构建、部署、自动化，满足任何项目的需要。我们可以用Jenkins来构建和部署我们的项目，比如说从我们的代码仓库获取代码，然后将我们的代码打包成可执行的文件，之后通过远程的ssh工具执行脚本来运行我们的项目。

# 一、准备工作

真正的生产环境上，可能是有多台服务器，但是我是用来练手，手头没有多余的服务器，只有一台。所以我就将 Jenkins 和我的 Spring Boot 项目都部署到一台服务器上。

由于本次是使用Docker安装Jenkins，所以需要服务器上提前安装好JDK，Maven，Doker三个必备的环境配置。这里就不过多赘述这三个环境的安装了，可以自行查询资料安装。

* 检查 JDK 环境

```powershell
java -version
复制代码
```

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/bc9f8df436b0489bb236bb480a870cc5~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image)

* 检查 Maven 环境

```powershell
mvn -v
复制代码
```

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/967134e6ca244ef4a0495595f5224459~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image)

* 检查 Docker 环境

```powershell
docker version
复制代码
```

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5381737f7cd54ff3a56c8d0ea716a6c5~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image)

# 二、Jenkins的安装及配置

## 1. Docker环境下的安装

* （1）下载Jenkins的Docker镜像：

```powershell
docker pull jenkins/jenkins:lts
复制代码
```

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e900c1dac3a543459b5bbcd154f185cf~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image)

* （2）在Docker容器中运行Jenkins：

```powershell
docker run -p 9080:8080 --name xiliu-jenkins \
-u root \
-v /mydata/jenkins_home:/var/jenkins_home \
-v /usr/local/jdk1.8.0_321:/usr/local/jdk1.8 \
-v /usr/local/apache-maven-3.8.6:/usr/local/apache-maven-3.8.6 \
-v $(which docker):/usr/bin/docker \
-v /var/run/docker.sock:/var/run/docker.sock \
-d jenkins/jenkins:lts
复制代码
```

* 参数说明

参数说明-p 9080:8080端口映射（将容器的8080端口【后面的8080】映射到服务器的9080端口【前面的9080】，云服务器需要开通9080端口供外网访问）--name容器名字-u root用户名-v /mydata/jenkins_home:/var/jenkins_home将配置文件夹挂在到主机（:前面的是主机目录，后面的是容器目录）-v /usr/local/jdk1.8.0_321:/usr/local/jdk1.8是把linux下的jdk和容器内的关联（配置Jenkins时使用，:前面的是主机目录，后面的是容器目录）-v /usr/local/apache-maven-3.8.6:/usr/local/apache-maven-3.8.6是把linux下的maven和容器内的关联（配置Jenkins时使用，:前面的是主机目录，后面的是容器目录）-v $(which docker):/usr/bin/docker是可以在Jenkins容器里使用我们Linux下的docker-v /var/run/docker.sock:/var/run/docker.sock是可以在Jenkins容器里使用我们Linux下的docker-d jenkins/jenkins:lts后台启动 Jenkins镜像（最新版）

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/82d4529b34f0474b8c0798eb2e1700b1~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image)

## 2. Jenkins的配置

* （1）运行成功后访问该地址登录Jenkins，第一次登录需要输入管理员密码：[http://你的ip:8080/](https://link.juejin.cn?target=http%3A%2F%2F%25E4%25BD%25A0%25E7%259A%2584ip%3A8080%2F "http://%E4%BD%A0%E7%9A%84ip:8080/") ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ca56556ec81242358f9e4f20338689f7~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image)
* （2）使用管理员密码进行登录，可以使用以下命令从容器启动日志中获取管理密码：

```powershell
docker logs xiliu-jenkins
复制代码
```

* 从日志中获取管理员密码： ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/86bfec0a39804d1f83b46b3f8f1a6198~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image)
* （3）输入管理员密码后，就进入安装界面，选择安装插件方式，这里我们直接安装推荐的插件： ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9408fb87293a4be7b830b158b34fd4cf~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image)
* （4）进入插件安装界面，联网等待插件安装： ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9698e172ea8342dc9ed9f59feb399acc~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image)
* （5）安装完成后，创建管理员账号： ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/045d873693b64d3895e9d1acc3c6748f~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image)
* （6）进行实例配置 ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b41fb5e6a286432294349a37c7e32df5~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image)
* （7）点击保存并完成，Jenkins就安装已完成 ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/01682f0cd0ca4dc4864702d43d6224f0~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image)
* （8）进入Jenkins，点击系统管理->插件管理，进行一些自定义的插件安装： ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f82629f95b7948d5a79a267022a52d22~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image) 修改插件的站点，把原站点：[updates.jenkins.io/update-cent...](https://link.juejin.cn?target=https%3A%2F%2Fupdates.jenkins.io%2Fupdate-center.json "https://updates.jenkins.io/update-center.json") 改为：[mirrors.tuna.tsinghua.edu.cn/jenkins/upd...](https://link.juejin.cn?target=https%3A%2F%2Fmirrors.tuna.tsinghua.edu.cn%2Fjenkins%2Fupdates%2Fupdate-center.json%25EF%25BC%258C%25E5%258F%25AF%25E4%25BB%25A5%25E6%259B%25B4%25E5%25BF%25AB%25E7%259A%2584%25E4%25B8%258B%25E8%25BD%25BD%25E6%258F%2592%25E4%25BB%25B6%25E3%2580%2582 "https://mirrors.tuna.tsinghua.edu.cn/jenkins/updates/update-center.json%EF%BC%8C%E5%8F%AF%E4%BB%A5%E6%9B%B4%E5%BF%AB%E7%9A%84%E4%B8%8B%E8%BD%BD%E6%8F%92%E4%BB%B6%E3%80%82") ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/048eb6daec7a41118d5546c7a9966143~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image)

确保以下插件被正确安装，未安装的在可选插件中搜索安装即可： （1）根据角色管理权限的插件：Role-based Authorization Strategy （2）把 Jenkins 打包好的 jar 上传到应用服务器上：Publish Over SSH ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d749786cfe6b4f3e8daadb7a2d36dd16~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image) ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f9c26db29b584f6c939f6b813aac0430~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image) 安装完成后需要重启一下jenkins，在连接后面加restart就可以重启jenkins了，或者使用命令：docker restart 容器名字 ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/64b0d07d3db14308b3faa232b631040f~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image)

* （9）配置jdk和maven，通过系统管理->全局工具配置来进行全局工具的配置，路径都是jenkins里面的路径 ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e62f6dd84fe74f06878c2cd78d9e4e87~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image) ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8099b76b775e4c0ea88d16d9142e8709~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image)
* （9）在系统管理->系统配置中，往下拉找到 Publish Over SSH，配置好要连接的应用服务器（在后面的操作中需要使用jenkins远程ssh连接到应用服务器，进行构建后的应用部署运行。用户名密码会导致 jar 包上传失败，要在应用服务器上生成 ssh 密钥对。不会生成服务器SSH密钥的可以自行查询资料生成一下。） ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8b5e57e6cea04fdcb8d3c1158fc1a136~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image) ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a49e98ec502a4c64b91c3d9706080e42~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image) 这样Jenkins使用ssh就可以执行远程的linux脚本了：
* （10）进入系统管理->凭据中，添加Gitee登录账号凭据。接下来配置 Gitee 的凭证，要根据这些凭证，才能从 Gitee 上拉取代码下来： ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/09d3c67053d244e1a40cd74275b1c272~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image) ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/24bf97ac7b9c42c09d8c505bded6d519~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image) ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cef4591c7dce40be8c69e6a6e2018bd8~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image) ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2c3ec83924e7407f9a8d55aa27108704~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image) 所有配置工作都做完了，接下来我们就可以开始构建一个项目了。

# 三、打包部署SpringBoot应用

因为我的源码是在Gitee上，所以这里以Gitee示例：

## 2.1 在Jenkins中创建执行任务

* （1）首先我们需要新建一个任务：![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7aece92f35b4494c8f12f942e6233194~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image)
* （2）设置任务名称后选择构建一个自由风格的软件项目：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e5c4c1e61f46437c8114666826eb6037~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image)

## 2.2 添加git凭据

添加Gitee登录账号凭据。接下来配置 Gitee 的凭证，要根据这些凭证，才能从 Gitee 上拉取代码下来。（如果前面添加了凭证，这里直接选择就行）![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b6f41ee6f4414129bc35d23d416cdb95~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image)

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4c34c150e7ef4ffaa6842a5bf64c4026~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image)

* （3）填写完成后选择该凭据，就可以正常连接git仓库了；

## 2.3 添加maven构建

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f9b1ed620e524b94936d5e2d5d4a8acd~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image)

* 选择我们的maven版本，然后设置maven命令和指定pom文件位置：![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fee638a0e4264a85bdec20c579547d94~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image)

## 2.4 增加构建后操作步骤

因为前面运行jenkins的时候已经做了目录的映射，所以jenkins打完包后其实是会把包同步到服务映射的目录里的。所以这里不需要上传jar包，直接执行xshell命令，进入jar包的目录，执行jar包即可。 ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7a00d95d9fc24b6093061f795186cca3~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image) ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/93f780457562404098b2ec630787baf5~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image) ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b9ea8465c49041628724e74611f9ade3~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image)

* shell脚本：

```powershell
#第一步是进入到服务器中生成好的jar包的目录下
cd /mydata/jenkins_home/workspace/xiliu-admin/xiliu-ucenter/target/

#第二步是根据jar包的名字获取运行的pid，并且将该进程杀死
ps -ef | grep xiliu-ucenter-0.0.1-SNAPSHOT.jar   |   grep -v   grep   |   awk '{printf $2}'  |  xargs kill -9

#执行 前加载一下环境变量，否则不会执行java -jar 命令
source /etc/profile

#最后一步就是将这个jar后台启动了，并且将日志输出到warpper.log中。
nohup java -jar xiliu-ucenter-0.0.1-SNAPSHOT.jar >warpper.log &2>1 &

#睡眠1秒
sleep 1

#输出内容，可不加
echo "启动完成"
复制代码
```

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6c78150daea54ab0b90248934155d7ab~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image)

## 2.5 立即构建

配置完成后，点击立即构建，可以看到控制台输出成功。 ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7ae1557904044f748c2bcb82f4d1224a~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image) ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/15905a636b3c457abfeab9ef7d38c673~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image)

## 2.6 测试

访问项目地址，能够正常访问。大功告成 ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8525e0a6e78a481f89717ce3014fb580~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image)

# 总结

以上就是本文的全部内容了，感谢大家的阅读。

如果觉得文章对你有帮助，还不忘帮忙点赞、收藏、关注、评论哟，您的支持就是我创作最大的动力！
