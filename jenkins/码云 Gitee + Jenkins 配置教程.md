---
link: https://blog.csdn.net/liqing0013/article/details/98031485
title: 码云 Gitee + Jenkins 配置教程
description: 安装 Gitee Jenkins 插件环境说明插件安装插件配置添加 Gitee 链接配置新建构建任务任务全局配置源码管理配置触发器配置构建后步骤配置构建结果回评至码云构建成功自动合并PR新建码云仓库WebHook测试触发构建通过测试按钮触发构建测试推送触发构建测试PR触发构建环境说明系统版本：Jenkins 版本Jenkins ver. 2.176.2插件安装前往 ..._"+refs/heads/*:refs/remotes/origin/* +refs/pull/*/merge:refs/pull/*/merge\" re"
keywords: "+refs/heads/*:refs/remotes/origin/* +refs/pull/*/merge:refs/pull/*/merge\" re"
author: Andy_li_ Csdn认证博客专家 Csdn认证企业博客 码龄8年 暂无认证
date: 2023-06-22T08:43:51.000Z
publisher: null
stats: paragraph=30 sentences=23, words=370
---
### 安装 Gitee Jenkins 插件

* [环境说明](#-1)
* [插件安装](#-6)
* [插件配置](#-14)
*
  - [添加 Gitee 链接配置](#-gitee--15)
  - [新建构建任务](#-32)
  - [任务全局配置](#-35)
  - [源码管理配置](#-38)
  - [触发器配置](#-55)
  - [构建后步骤配置](#-71)
  -
    + [构建结果回评至码云](#-73)
    + [构建成功自动合并PR](#pr-80)
  - [新建码云仓库WebHook](#webhook-83)
  - [测试触发构建](#-94)
  -
    + [通过测试按钮触发构建](#-95)
    + [测试推送触发构建](#-98)
    + [测试PR触发构建](#pr-104)

# 环境说明

* 系统版本：
* Jenkins 版本
  - Jenkins ver. 2.176.2

# <a name="_6">;</a> 插件安装

* 前往 Manage Jenkins -> Manage Plugins -> Available，在 Filter 中搜索 Gitee:
* 下方可选列表中勾选 Gitee（如列表中不存在 Gitee，则点击 Check now 更新插件列表），然后点击"Download now and install after restart"。
  - 在安装页面勾选"Restart Jenkins when installation is complete and no jobs are running"
* 安装完之后，可以在 "installed" 页面看到 Gitee 插件

# 插件配置

## 添加 Gitee 链接配置

1. 前往 Jenkins -> Manage Jenkins -> Configure System -> Gitee 配置 -> Gitee 链接
2. 在 链接名 中输入 Gitee 或者你想要的名字
3. Gitee 域名 URL 中输入码云完整 URL地址：[https://gitee.com](https://gitee.com) （码云私有化客户输入部署的域名）
4. 证书令牌 中如还未配置码云 APIV5 私人令牌，点击 Add - > Jenkins
  1. Domain 选择 全局凭据
  2. Kind 选择 Gitee API 令牌
  3. Scope 选择你需要的范围
  4. Gitee API Token 输入你的码云私人令牌，获取地址：[https://gitee.com/profile/personal_access_tokens](https://gitee.com/profile/personal_access_tokens)
  5. ID, Descripiton 中输入你想要的 ID 和描述即可。
![](https://img-blog.csdnimg.cn/20190801102932768.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpcWluZzAwMTM=,size_16,color_FFFFFF,t_70)
5. Credentials 选择配置好的 Gitee APIV5 Token

![](https://img-blog.csdnimg.cn/20190801103023218.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpcWluZzAwMTM=,size_16,color_FFFFFF,t_70)
6. 点击 Advanced ，可配置是否忽略 SSL 错误（适您的Jenkins环境是否支持），并可设置链接测超时时间（适您的网络环境而定）
7. 点击 Test Connection 测试链接是否成功，如失败请检查以上 3，5，6 步骤。
![](https://img-blog.csdnimg.cn/20190801103404419.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpcWluZzAwMTM=,size_16,color_FFFFFF,t_70)

## <a name="_32">;</a> 新建构建任务

* 前往 Jenkins -> New Item , name 输入 'Gitee Test'，选择 Freestyle project , 再点击 OK 即可创建构建项目。

## 任务全局配置

* 任务全局配置中需要选择前一步中的Gitee 链接。如图：

## <a name="_38">;</a> 源码管理配置

选择 Source Code Management 选项卡：

1. 点击 Git
2. 输入你的仓库地址，例如[https://gitee.com/AndyWannaSing/hello-casstime-demo.git](https://gitee.com/AndyWannaSing/hello-casstime-demo.git)
  1. 点击 add-Jenkins，添加用户名和密码凭证（连接项目的时候需要用它做校验）
  2. 点击 Advanced 按钮, Name 字段中输入 origin， Refspec 字段输入 `+refs/heads/*:refs/remotes/origin/* +refs/pull/*/MERGE:refs/pull/*/MERGE`
![](https://img-blog.csdnimg.cn/20190801110735522.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpcWluZzAwMTM=,size_16,color_FFFFFF,t_70)
3. Branch Specifier 选项:
  1. 对于单仓库工作流输入: origin/${giteeSourceBranch}
  2. 对于 PR 工作流输入: pull/${giteePullRequestIid}/MERGE
4. Additional Behaviours 选项：
  1. 对于单仓库工作流，如果你希望推送的分支构建前合并默认分支（发布的分支），可以做以下操作：
    1. 点击 Add 下拉框
    2. 选择 Merge before build
    3. 设置 Name of repository 为 origin
    4. 设置 Branch to merge to 为 ${ReleaseBranch} 即您要合并的默认分支（发布分支）
  2. 对于 PR 工作流，码云服务端已经将 PR 的原分支和目标分支作了预合并，您可以直接构建，如果目标分支不是默认分支（发布分支），您也可以进行上诉构建前合并。

## <a name="_55">;</a> 触发器配置

选择 Build Triggers 选项卡：

1. Enabled Gitee triggers 勾选您所需要的构建触发规则，如 Push Event, Opened Merge Request Events，勾选的事件会接受WebHook，触发构建。目前支持触发事件有：
  1. Push Events ：推送代码事件
  2. Opened Merge Request Events ：提交 PR 事件
  3. Updated Merge Request Events ：更新 PR 事件
  4. Accepted Merge Request Events ：接受/合并 PR 事件
  5. Closed Merge Request Events ：关闭 PR 事件
  6. Approved Pull Requests ： 审查通过 PR 事件
  7. Tested Pull Requests ：测试通过 PR 事件
2. Enable [ci-skip] 该选项可以开启支持 [ci-skip] 指令，只要commit message 中包含 [ci-skip]，当前commit 即可跳过构建触发。
3. Ignore last commit has build 该选项可以跳过已经构建过的 Commit 版本。
4. Allowed branches 可以配置允许构建的分支，目前支持分支名和正则表达式的方式进行过滤。
5. Secret Token for Gitee WebHook 该选项可以配置 WebHook 的密码，该密码需要与码云 WebHook配置的密码一致方可触发构建。
6. 注意：若 PR 状态为不可自动合并，则不触发构建。
![](https://img-blog.csdnimg.cn/20190801134548851.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpcWluZzAwMTM=,size_16,color_FFFFFF,t_70)

## 构建后步骤配置

选择 Post-build Actions 选项卡：

### 构建结果回评至码云

1. 点击 Add post-build action 下拉框选择 "将构建状态评论到 Gitee pull request 中"
2. Advanced 中可以配置：
  1. 仅为构建失败回评到码云
  2. 自定义各状态的回评内容（内容可以引用 Jenkins 的环境变量，或者自定义的环境变量）
3. 若开启该功能，还可将不可自动合并的状态回评至码云
![](https://img-blog.csdnimg.cn/20190801114516321.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpcWluZzAwMTM=,size_16,color_FFFFFF,t_70)

### <a name="pr_80">;</a> 构建成功自动合并PR

1. 点击 Add post-build action 下拉框选择 "当构建成功自动合并 Gitee 的 Pull Request"
![](https://img-blog.csdnimg.cn/20190801115035743.png)

## 新建码云仓库WebHook

进入源码管理配置中设置的码云仓库中，进入 管理 -> WebHooks:

1. 添加 WebHook， URL 填写 触发器配置的地址。
2. 密码填写：触发器配置第 5 点中配置的 WebHook密码，不设密码可以不填
3. 勾选 PUSH， Pull Request
![](https://img-blog.csdnimg.cn/20190801140416814.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpcWluZzAwMTM=,size_16,color_FFFFFF,t_70)

![](https://img-blog.csdnimg.cn/20190801140218884.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpcWluZzAwMTM=,size_16,color_FFFFFF,t_70)
其中：[http://7e2be7e8.ngrok.io/project/Gitee Test](http://7e2be7e8.ngrok.io/project/Gitee%20Test) 是外网的IP。（[内网转外网域名的方法](https://blog.csdn.net/liqing0013/article/details/80731757)）
![](https://img-blog.csdnimg.cn/2019080113592660.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpcWluZzAwMTM=,size_16,color_FFFFFF,t_70)

## <a name="_94">;</a> 测试触发构建

### 通过测试按钮触发构建

### <a name="_98">;</a> 测试推送触发构建

1. 码云的 WebHook 管理中选择勾选了PUSH的 WebHook 点击测试，观察 Jenkins 任务的构建状态
2. 码云仓库页面编辑一个文件提交，观察 Jenkins 任务的构建状态

### 测试PR触发构建

1. 码云的 WebHook 管理中选择勾选了 Pull Request 的 WebHook 点击测试，观察 Jenkins 任务的构建状态
2. 在码云仓库中新建一个Pull Request，观察 Jenkins 任务的构建状态

![](https://img-blog.csdnimg.cn/20190801142351413.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpcWluZzAwMTM=,size_16,color_FFFFFF,t_70)
![](https://img-blog.csdnimg.cn/20190801142423211.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpcWluZzAwMTM=,size_16,color_FFFFFF,t_70)

![](https://img-blog.csdnimg.cn/20190801142436588.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpcWluZzAwMTM=,size_16,color_FFFFFF,t_70)
