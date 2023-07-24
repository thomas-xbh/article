---
link: https://juejin.cn/post/7067874686226399269
title: centos安装docker
description: 较旧的Docker版本称为docker或docker-engine。如果已安装这些程序，请卸载它们以及相关的依赖项。 一般如果没安装过都是返回没有匹配到相关安装
keywords: 后端
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2022-02-23T12:04:00.000Z
publisher: 稀土掘金
stats: paragraph=50 sentences=9, words=126
---
## 卸载旧版本

较旧的Docker版本称为docker或docker-engine。如果已安装这些程序，请卸载它们以及相关的依赖项。

```arduino
 sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
复制代码
```

一般如果没安装过都是返回没有匹配到相关安装

## 设置存储库

第一次安装docker都需要安装yum-utils软件包（提供yum-config-manager 实用程序）并设置稳定的存储库。

```
 sudo yum install -y yum-utils
<span class="copy-code-btn">&#x590D;&#x5236;&#x4EE3;&#x7801;</span>
```

```arduino
 sudo yum-config-manager \
    --add-repo \
    https:
复制代码
```

## 安装DOCKER引擎

安装最新版本的Docker Engine和容器，或转到下一步以安装特定版本：

```lua
 sudo yum install docker-ce docker-ce-cli containerd.io
复制代码
```

# 常用方法

## docker管理

1、查看docker 版本

```
docker -V
<span class="copy-code-btn">&#x590D;&#x5236;&#x4EE3;&#x7801;</span>
```

2、启动docker

```sql
systemctl start docker
复制代码
```

3、停止docker

```arduino
systemct1 stop docker
复制代码
```

4、设置开机启动docker

```bash
systeactl enable docker
复制代码
```

## docker 镜像管理

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/30b1f2a570f34e8a85aa682c49b4b2c1~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image)

## 镜像使用

1.搜索镜像

```sql
 docker search tomcat
复制代码
```

2、拉取镜像

```
docker pull tomcat
<span class="copy-code-btn">&#x590D;&#x5236;&#x4EE3;&#x7801;</span>
```

3、根据镜像启动容器

```arduino
docker run --name mytomcat -d tomcat :latest
复制代码
```

4、查看运行中的容器

```
docker ps
<span class="copy-code-btn">&#x590D;&#x5236;&#x4EE3;&#x7801;</span>
```

5、停止运行中的容器

```arduino
docker stop [容器的id]
复制代码
```

6、查看所有的容器

```css
docker ps -a
复制代码
```

7、启动容器

```bash
docker start [容器id]
复制代码
```

8、删除一个容器或镜像

```bash
docker rm [容器id]
复制代码
```

```arduino
docker image  rm [镜像 名]
复制代码
```

9、启动一个做了端口映射的tomcat

```diff
docker run -d -p 8888:8080 tomcat
-d:后台运行
-p:将主机的端口映射到容器的一一个端口主机端口:容器内部的端口
复制代码
```

1. 为了演示简单关闭了linux的防火墙

```arduino
service firewalld status ; 查看防火墙状态
service firewalld stop :关闭防火墙
复制代码
```

11、查看容器的日志

```bash
docker logs container- name/container-id
复制代码
```

12、进入容器操作配置

```bash
docker exec -it nginx bash
复制代码
```

## 更多命令参看

[docs.docker.com/engine/refe...](https://link.juejin.cn?target=https%3A%2F%2Fdocs.docker.com%2Fengine%2Freference%2Fcommandline%2Fdocker%2F "https://docs.docker.com/engine/reference/commandline/docker/") 可以参考每一个镜像的文档
