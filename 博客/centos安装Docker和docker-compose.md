### 前言
最近把博客从wordpress迁移到halo，根据halo官网选择使用docker-compose来部署，这里就记录一下centos安装docker和docker-compose的步骤吧
### Docker安装
为了安装 Docker，我们需要首先安装yum-utils，以便添加 Docker 的源。
> yum install -y yum-utils
> 
> yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
#### 直接使用yum命令即可安装 Docker
> yum install docker-ce docker-ce-cli containerd.io
#### Docker 安装完之后不会自动运行，需要手动运行
>systemctl start docker
#### 可使用docker ps命令查看 Docker 运行状态，如看到输出正在运行的窗口列表，则启动成功
> CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES
#### 为了让 Docker 在每次重启系统的时候能自动运行，还需要设置自启动
> systemctl enable docker
#### 至此就大功告成了。可以使用hello-world镜像验证一下
> docker run hello-world
看到以下输出即为成功：
```js
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
93288797bd35: Pull complete
Digest: sha256:37a0b92b08d4919615c3ee023f7ddb068d12b8387475d64c622ac30f45c29c51
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

 ### docker-compose安装
 这里有好几种安装方式
#### 手动安装：

从[github](https://github.com/docker/compose/releases)仓库下载，上传到服务器

#### 选择版本下载：

 1. 选择版本下载（以v2.16.0为例）

 > sudo curl -L https://get.daocloud.io/docker/compose/releases/download/v2.16.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
 2. 授权
 > chmod +x /usr/local/bin/docker-compose
 3. 检查版本

 >  docker-compose version

到这里docker和docker-compose就安装完成啦

如果你想通过docker-compose部署halo,可以参考[halo官网](https://docs.halo.run/getting-started/install/docker-compose),配置yaml文件即可

