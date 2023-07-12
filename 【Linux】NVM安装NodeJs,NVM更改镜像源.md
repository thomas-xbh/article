---
link: https://blog.csdn.net/qq_38238995/article/details/126280600
title: 【Linux】NVM安装NodeJs,NVM更改镜像源
description: NVM安装及镜像源更改_nvm_nodejs_org_mirror
keywords: nvm_nodejs_org_mirror
author: ·Timidity Csdn认证博客专家 Csdn认证企业博客 码龄6年 暂无认证
date: 2022-08-11T02:45:11.000Z
publisher: null
stats: paragraph=10 sentences=10, words=42
---
# 安装NVM

```bash
curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash
```

> 无法访问时可以先下载再上传至服务器运行

## 更改NVM镜像

```bash

sudo vi ~/.bashrc

export NVM_NODEJS_ORG_MIRROR=https://npm.taobao.org/mirrors/node/
export NVM_IOJS_ORG_MIRROR=http://npm.taobao.org/mirrors/iojs

source ~/.profile
```

## NVM常用命令

```bash
nvm install stable
nvm install <version>
nvm uninstall <version>
nvm use <version>
nvm ls
nvm ls-remote
nvm current
nvm alias <name> <version>
nvm unalias <name>
nvm reinstall-packages <version>
nvm alias default [node版本号]
```
