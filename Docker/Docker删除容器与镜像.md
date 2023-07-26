---
link: https://blog.csdn.net/qq_32447301/article/details/79387649
title: Docker删除容器与镜像
description: 1.停止所有的container，这样才能够删除其中的images：docker stop $(docker ps -a -q)如果想要删除所有container的话再加一个指令：docker rm $(docker ps -a -q)2.查看当前有些什么imagesdocker images3.删除images，通过image的id来指定删除谁docker rmi &amp;amp;amp;amp;..._docker卸载容器
keywords: docker卸载容器
author: 灬点点 Csdn认证博客专家 Csdn认证企业博客 码龄8年 暂无认证
date: 2018-02-27T06:11:56.000Z
publisher: null
stats: paragraph=36 sentences=4, words=337
---
列出所有容器ID

```
docker ps -aq
```

查看所有运行或者不运行容器

```
docker ps -a
```

停止所有的container（容器），这样才能够删除其中的images：

```
docker stop $(docker ps -a -q) &#x6216;&#x8005; docker stop $(docker ps -aq)
```

如果想要删除所有container（容器）的话再加一个指令：

```
docker rm $(docker ps -a -q) &#x6216;&#x8005; docker rm $(docker ps -aq)
```

查看当前有些什么images

```
docker images
```

删除images（镜像），通过image的id来指定删除谁

```
docker rmi <image id>
</image>
```

想要删除untagged images，也就是那些id为的image的话可以用

```
docker rmi $(docker images | grep "^<none>" | awk "{print $3}")
</none>
```

要删除全部image（镜像）的话

```
docker rmi $(docker images -q)
```

强制删除全部image的话

```
docker rmi -f $(docker images -q)
```

从容器到宿主机复制

```
 docker cp tomcat&#xFF1A;/webapps/js/text.js /home/admin
 docker  cp &#x5BB9;&#x5668;&#x540D;:  &#x5BB9;&#x5668;&#x8DEF;&#x5F84;       &#x5BBF;&#x4E3B;&#x673A;&#x8DEF;&#x5F84;
```

从宿主机到容器复制

```
 docker cp /home/admin/text.js tomcat&#xFF1A;/webapps/js
 docker cp &#x5BBF;&#x4E3B;&#x8DEF;&#x5F84;&#x4E2D;&#x6587;&#x4EF6;      &#x5BB9;&#x5668;&#x540D;  &#x5BB9;&#x5668;&#x8DEF;&#x5F84;
```

删除所有停止的容器

```
docker container prune
```

删除所有不使用的镜像

```
docker image prune --force --all&#x6216;&#x8005;docker image prune -f -a
```

停止、启动、杀死、重启一个容器

```
docker stop Name&#x6216;&#x8005;ID
docker start Name&#x6216;&#x8005;ID
docker kill Name&#x6216;&#x8005;ID
docker restart name&#x6216;&#x8005;ID
```

docker进入容器，查看配置文件

```
docker exec &#xFF1A;&#x5728;&#x8FD0;&#x884C;&#x7684;&#x5BB9;&#x5668;&#x4E2D;&#x6267;&#x884C;&#x547D;&#x4EE4;
        -d :&#x5206;&#x79BB;&#x6A21;&#x5F0F;: &#x5728;&#x540E;&#x53F0;&#x8FD0;&#x884C;
        -i :&#x5373;&#x4F7F;&#x6CA1;&#x6709;&#x9644;&#x52A0;&#x4E5F;&#x4FDD;&#x6301;STDIN&#xFF08;&#x6807;&#x51C6;&#x8F93;&#x5165;&#xFF09; &#x6253;&#x5F00;,&#x4EE5;&#x4EA4;&#x4E92;&#x6A21;&#x5F0F;&#x8FD0;&#x884C;&#x5BB9;&#x5668;&#xFF0C;&#x901A;&#x5E38;&#x4E0E; -t &#x540C;&#x65F6;&#x4F7F;&#x7528;&#xFF1B;
        -t: &#x4E3A;&#x5BB9;&#x5668;&#x91CD;&#x65B0;&#x5206;&#x914D;&#x4E00;&#x4E2A;&#x4F2A;&#x8F93;&#x5165;&#x7EC8;&#x7AEF;&#xFF0C;&#x901A;&#x5E38;&#x4E0E; -i &#x540C;&#x65F6;&#x4F7F;&#x7528;&#xFF1B;
docker exec -it  f94d2c317477 /bin/bash
```

出现root@f94d2c317477:/usr/share/elasticsearch/config# vi elasticsearch.yml
bash: vi: command not found

```
apt-get update && apt-get install vim -y
```

修改配置、退出容器

```
1&#x3001;&#x5982;&#x679C;&#x8981;&#x6B63;&#x5E38;&#x9000;&#x51FA;&#x4E0D;&#x5173;&#x95ED;&#x5BB9;&#x5668;&#xFF0C;&#x8BF7;&#x6309;Ctrl+P+Q&#x8FDB;&#x884C;&#x9000;&#x51FA;&#x5BB9;&#x5668;
2&#x3001;&#x5982;&#x679C;&#x4F7F;&#x7528;exit&#x9000;&#x51FA;&#xFF0C;&#x90A3;&#x4E48;&#x5728;&#x9000;&#x51FA;&#x4E4B;&#x540E;&#x4F1A;&#x5173;&#x95ED;&#x5BB9;&#x5668;&#xFF0C;&#x53EF;&#x4EE5;&#x4F7F;&#x7528;&#x4E0B;&#x9762;&#x7684;&#x6D41;&#x7A0B;&#x8FDB;&#x884C;&#x6062;&#x590D;
&#x4F7F;&#x7528;docker restart&#x547D;&#x4EE4;&#x91CD;&#x542F;&#x5BB9;&#x5668;
&#x4F7F;&#x7528;docker attach&#x547D;&#x4EE4;&#x8FDB;&#x5165;&#x5BB9;&#x5668;
```
