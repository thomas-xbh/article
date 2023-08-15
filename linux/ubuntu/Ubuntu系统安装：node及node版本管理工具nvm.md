---
link: https://blog.csdn.net/weixin_44786530/article/details/128197994
title: Ubuntu系统安装：node及node版本管理工具nvm
description: 2、安装nvm进入nvm目录内执行安装命令根据执行安装命令后的提示继续执行提示内容： 通过nvm --version可以看到安装成功。4.nvm常用命令_nvm ubuntu
keywords: nvm ubuntu
author: 1024小神 Csdn认证博客专家 Csdn认证企业博客 码龄4年 全栈领域优质创作者
date: 2022-12-06T02:13:09.000Z
publisher: null
stats: paragraph=36 sentences=100, words=623
---
### 1、把nvm远程镜像克隆到指定目录

```
git clone https://gitee.com/mirrors/nvm
```

```
drc@iZwz91oq31508figapkas0Z:~/qiang/tools$ git clone https://gitee.com/mirrors/nvm
fatal: destination path 'nvm' already exists and is not an empty directory.

drc@iZwz91oq31508figapkas0Z:~/qiang/tools$ ls
nvm
drc@iZwz91oq31508figapkas0Z:~/qiang/tools$ cd nvm
drc@iZwz91oq31508figapkas0Z:~/qiang/tools/nvm$ ls
bash_completion     CONTRIBUTING.md  GOVERNANCE.md  LICENSE.md  nvm-exec  package.json        README.md       ROADMAP.md  update_test_mocks.sh
CODE_OF_CONDUCT.md  Dockerfile       install.sh     Makefile    nvm.sh    PROJECT_CHARTER.md  rename_test.sh  test
drc@iZwz91oq31508figapkas0Z:~/qiang/tools/nvm$
```

### 2、安装nvm

进入nvm目录内执行安装命令

```
bash install.sh
```

根据执行安装命令后的提示继续执行提示内容：

```
drc@iZwz91oq31508figapkas0Z:~/qiang/tools/nvm$ sudo bash install.sh
=> nvm is already installed in /root/.nvm, trying to update using git
=> => Compressing and cleaning up git repository

=> nvm source string already in /root/.bashrc
=> bash_completion source string already in /root/.bashrc
=> You currently have modules installed globally with `npm`. These will no
=> longer be linked to the active version of Node when you install a new node
=> with `nvm`; and they may (depending on how you construct your `$PATH`)
=> override the binaries of modules installed with `nvm`:

/usr/local/lib
&#x251C;&#x2500;&#x2500; corepack@0.10.0
=> If you wish to uninstall them at a later point (or re-install them under your
=> `nvm` Nodes), you can remove them from the system Node as follows:

     $ nvm use system
     $ npm uninstall -g a_module

=> Close and reopen your terminal to start using nvm or run the following to use it now:

export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
drc@iZwz91oq31508figapkas0Z:~/qiang/tools/nvm$ export NVM_DIR="$HOME/.nvm"
drc@iZwz91oq31508figapkas0Z:~/qiang/tools/nvm$ [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
drc@iZwz91oq31508figapkas0Z:~/qiang/tools/nvm$ [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
drc@iZwz91oq31508figapkas0Z:~/qiang/tools/nvm$ nvm --version
0.39.1
drc@iZwz91oq31508figapkas0Z:~/qiang/tools/nvm$
```

通过nvm --version可以看到安装成功。

### 3、使用nvm安装node16.13.1并使用该版本：

```
drc@iZwz91oq31508figapkas0Z:~/qiang/tools/nvm$ nvm ls

->       system
iojs -> N/A (default)
node -> stable (-> N/A) (default)
unstable -> N/A (default)
drc@iZwz91oq31508figapkas0Z:~/qiang/tools/nvm$ nvm install 16.13.1
Downloading and installing node v16.13.1...

Downloading https://nodejs.org/dist/v16.13.1/node-v16.13.1-linux-x64.tar.xz...

################################################################################################################################################################################################################ 100.0%
Computing checksum with sha256sum
Checksums matched!

Now using node v16.13.1 (npm v8.1.2)
Creating default alias: default -> 16.13.1 (-> v16.13.1)
drc@iZwz91oq31508figapkas0Z:~/qiang/tools/nvm$ nvm ls
->     v16.13.1
         system
default -> 16.13.1 (-> v16.13.1)
iojs -> N/A (default)
unstable -> N/A (default)
node -> stable (-> v16.13.1) (default)
stable -> 16.13 (-> v16.13.1) (default)
lts/* -> lts/gallium (-> N/A)
lts/argon -> v4.9.1 (-> N/A)
lts/boron -> v6.17.1 (-> N/A)
lts/carbon -> v8.17.0 (-> N/A)
lts/dubnium -> v10.24.1 (-> N/A)
lts/erbium -> v12.22.12 (-> N/A)
lts/fermium -> v14.19.2 (-> N/A)
lts/gallium -> v16.15.0 (-> N/A)
drc@iZwz91oq31508figapkas0Z:~/qiang/tools/nvm$ nvm use 16.13.1
Now using node v16.13.1 (npm v8.1.2)
drc@iZwz91oq31508figapkas0Z:~/qiang/tools/nvm$ node -v
v16.13.1
drc@iZwz91oq31508figapkas0Z:~/qiang/tools/nvm$
```

### 4.nvm常用命令

```
nvm ls &#xFF1A;&#x5217;&#x51FA;&#x6240;&#x6709;&#x5DF2;&#x5B89;&#x88C5;&#x7684; node &#x7248;&#x672C;

nvm ls-remote &#xFF1A;&#x5217;&#x51FA;&#x6240;&#x6709;&#x8FDC;&#x7A0B;&#x670D;&#x52A1;&#x5668;&#x7684;&#x7248;&#x672C;&#xFF08;&#x5B98;&#x65B9;node version list&#xFF09;

nvm list &#xFF1A;&#x5217;&#x51FA;&#x6240;&#x6709;&#x5DF2;&#x5B89;&#x88C5;&#x7684; node &#x7248;&#x672C;

nvm list available &#xFF1A;&#x663E;&#x793A;&#x6240;&#x6709;&#x53EF;&#x4E0B;&#x8F7D;&#x7684;&#x7248;&#x672C;

nvm install stable &#xFF1A;&#x5B89;&#x88C5;&#x6700;&#x65B0;&#x7248; node

nvm install [node&#x7248;&#x672C;&#x53F7;] &#xFF1A;&#x5B89;&#x88C5;&#x6307;&#x5B9A;&#x7248;&#x672C; node

nvm uninstall [node&#x7248;&#x672C;&#x53F7;] &#xFF1A;&#x5220;&#x9664;&#x5DF2;&#x5B89;&#x88C5;&#x7684;&#x6307;&#x5B9A;&#x7248;&#x672C;

nvm use [node&#x7248;&#x672C;&#x53F7;] &#xFF1A;&#x5207;&#x6362;&#x5230;&#x6307;&#x5B9A;&#x7248;&#x672C; node

nvm current &#xFF1A;&#x5F53;&#x524D; node &#x7248;&#x672C;

nvm alias [&#x522B;&#x540D;] [node&#x7248;&#x672C;&#x53F7;] &#xFF1A;&#x7ED9;&#x4E0D;&#x540C;&#x7684;&#x7248;&#x672C;&#x53F7;&#x6DFB;&#x52A0;&#x522B;&#x540D;

nvm unalias [&#x522B;&#x540D;] &#xFF1A;&#x5220;&#x9664;&#x5DF2;&#x5B9A;&#x4E49;&#x7684;&#x522B;&#x540D;

nvm alias default [node&#x7248;&#x672C;&#x53F7;] &#xFF1A;&#x8BBE;&#x7F6E;&#x9ED8;&#x8BA4;&#x7248;&#x672C;
```

成功后的结果：

![](https://img-blog.csdnimg.cn/a8518880eb5e481cabbaf42df058491e.png)
