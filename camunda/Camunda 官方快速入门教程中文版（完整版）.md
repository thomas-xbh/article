---
link: https://zhuanlan.zhihu.com/p/375908620
title: Camunda 官方快速入门教程中文版（完整版）
description: 本文为Camunda官网快速入门部分的中文版本 原文地址： https://docs.camunda.org/get-started/quick-start/ （英文）0.介绍本教程将指导您使用Camunda BPM平台建模并实现您的第一个工作流程，其中将使用JAVA或Node…
keywords: 流程,流程设计,后端技术
author: 晨峰说一只程序猿
date: 2021-06-02T03:42:00.000Z
publisher: 知乎专栏
stats: paragraph=134 sentences=27, words=272
---
本文为Camunda官网快速入门部分的中文版本

## 0.介绍

本教程将指导您使用Camunda BPM平台建模并实现您的第一个工作流程，其中将使用JAVA或NodeJS作为外部客户端，以及使用DMN分离流程与决策，让我们开始吧！

首先使用git克隆示例代码

在教程开始之前，首先将代码签出到Start标签

在教程过程中可以随时通过Start标签恢复到初试状态，或使用Step-X（X表示步骤）标签，恢复到任意一步骤的状态

教程将分为六个步骤

1.下载和安装

> 在计算机上安装Camunda BPM平台和Camunda Modeler。

2.编辑流程

> 了解处理Camunda Modeler的基础知识，了解如何对完全可执行的流程进行建模和配置，以及如何集成自己的业务逻辑。

3.部署流程

> 将流程部署到Camunda并启动您的第一个流程实例。

4.人工任务

> 了解将人工任务集成到流程中的基础知识，以及如何使用Camunda构建表单。

5.动态性

> 了解如何通过向流程添加网关来使流程更具动态性。

6.决策自动化

> 了解如何在流程中集成DMN决策表。

## 1.下载和安装

首先，您需要安装 Camunda BPM平台和Camunda Modeler。

确保你具有 JAVA1.8以上的JRE或JDK，并可以在命令行使用

在命令行中运行以下命令，检查你的java版本

首先我们需要下载 Camunda Platform

下载ZIP压缩包，并解压到任意位置

windows运行 `start.bat`linux运行start.sh，脚本会启动服务

下载对应系统的版本，并解压到任意位置

执行 `camunda-modeler.exe` (Windows), `camunda-modeler.app` (Mac), or `camunda-modeler.sh` (Linux)，即可启动 `Camunda Modeler`

## 2.编辑流程

本章中，我们将会使用Camunda Modeler创建第一个BPMN 2.0流程，并执行一些自动任务

首先，打开Camunda Modeler

点击 _File > New File > BPMN Diagram_ ，创建一个新的流程设计文件

> 标签可以换行，需要使用Shift +回车

可以看到一个新的活动显示到画布上，双击将它命名为"刷卡付款"

服务类型有很多执行的方法，这次我们使用"external（外部）"任务模式

这里我们修改id为_payment-retrieval_，id是区分流程的标识

然后修改Name 为"付款流程"

最后确保 _Executable_ 是勾选的，只有_Executable_被勾选，流程才能执行

> 到此第一部分结束，如果想直接获取到现在为止的进度，可以使用如下命令
git checkout -f Step-1

## 3.用java或NodeJS实现外部任务

在完成上面流程的编辑后，现在实现"刷卡付款"的业务逻辑

Camunda 可以使多种语言实现业务逻辑，本文将演示java和nodejs两种方式，你可以任意选择喜欢的一种

需要JAVA1.8+，maven（有的IDE自带），IDE

使用喜欢的ide创建一个maven项目，添加maven依赖如下

编写主类，代码如下

使用ide上的运行编译运行按钮运行

如果正常运行，则可以进入下一章了

> 到此第二部分结束，如果想直接获取到现在为止的进度，可以使用如下命令
git checkout -f Step-2a

需要NodeJS >= v10

首先创建一个新的NodeJS项目

添加Camunda外部任务依赖

新建一个 JavaScript 文件，命名为 `worker.js`，代码如下

如果正常运行，则可以进入下一章了

> 到此第二部分结束，如果想直接获取到现在为止的进度，可以使用如下命令
git checkout -f Step-2b

## 4.部署流程

下面我们将部署流程到流程引擎，然后发起流程，检查流程是否发起成功

点击工具栏中的部署按钮可以将当前流程部署到流程引擎，点击部署按钮，输入 `Deployment Name` 为 "Payment" ，输入下方 `REST Endpoint` 为 `http://localhost:8080/engine-rest` ，然后点击右下角Deploy部署

如果收到成功提示，表示部署成功

在浏览器中打开 http://localhost:8080/camunda/app/cockpit/default/#/processes 使用账号 demo / demo 登录 ，可以看到刚部署的流程显示出来了

这里使用Rest API发起流程，所以需要一个接口测试工具（例如：Postman），或者也可以使用电脑自带的curl

在命令行中执行

如果能看到成功的返回结果，则流程发起成功

在url中输入 `http://localhost:8080/engine-rest/process-definition/key/payment-retrieval/start`

模式选择POST

点击Body，选择 `raw`，并在右面选择 `application/json`

然后输入Body内容：

编辑完成后，结果这样：

点击 Send 发送请求

如果能看到成功的返回结果，则流程发起成功

## 添加人工任务

在本章中，我们会将人工任务添加进流程中，让人参与到 BPMN 2.0 流程中来。

首先打开 Camunda Modeler ，在左侧选择任务节点（圆角矩形），将它拖入"付款请求"和"刷卡付款"之间，重命名为"批准付款"

点击刚创建的"批准付款"节点，然后点击节点右侧出现的扳手，将节点更改为人工任务（User Task）

下面我们将为人工任务配置表单：

点击"批准付款节点"，在右侧的属性面板中点击Forms（表单）选项卡，点击下面的加号，添加3个属性：

属性1：

* ID: amount
* Type: long
* Label: 金额

属性2：

* ID: item
* Type: string
* Label: 项目

属性3：

* ID: approved
* Type: boolean
* Label: 是否同意?

和上一章一样，使用 Camunda Modeler 工具栏上的上传按钮将流程上传到流程引擎中

打开任务列表（http://localhost:8080/camunda/app/tasklist/），使用 demo / demo 登录

点击右上角的 `Start process` ，在弹出的对话框中选择"付款流程"

这时会弹出编辑流程变量的对话框，可以通过点击 Add a variable 按钮添加变量，这次我们先不添加，直接点击右下角 Start 启动流程

这时，在任务列表应该就能看到刚创建的人工任务了，如果没有可以手动刷新一下

> 部分用户这里看不到，可能是 All Tasks 过滤器没有自动添加，左侧显示为：


这时点击 Add a simple filter 即可

选择这个任务项，在右侧可以看到表单

点击 Diagram 选项卡，可以看到高亮的节点就是当前进行中的节点

接下来，我们将为流程带来一些变化，仅在金额足够大的时候进行人工审核，下一章：添加网关

> 到此第三部分结束，如果想直接获取到现在为止的进度，可以使用如下命令
git checkout -f Step-3

## 添加网关

本章中，我们将使用**排他网关**(_Exclusive Gateways_)，为流程添加分支，仅在金额足够大的时候进行人工审核

首先打开 Camunda Modeler ，在左侧的工具架中找到网关（菱形），将它拖动到"付款请求"和"刷卡付款"之间，将"批准付款"向下移动再添加一个网关，调整流程，最后看起来类似这样：

现在为新元素命名

接下来，我们选择"

同样的，对另一条线也进行配置，表达式为 `${amount>=1000}`

yes的表达式是 `${approved}`

no的表达式是 `${!approved}`

和上一章一样，使用 Camunda Modeler 工具栏上的上传按钮将流程上传到流程引擎中

打开任务列表（http://localhost:8080/camunda/app/tasklist/），使用 demo / demo 登录

点击右上角的 `Start process` ，在弹出的对话框中选择"付款流程"

在上一章中，我们直接点击 Start，但这次我们要增加几个变量来测试动态的流程

试着更改 `amount` 的值，查看对流程执行顺序的影响

> 到此第四部分结束，如果想直接获取到现在为止的进度，可以使用如下命令
git checkout -f Step-4

## 决策自动化

在本章中，我们将使用DMN为流程添加一个业务规则

打开 Camunda Modeler，点击 "批准付款"，在右面的扳手菜单中，将类型改为"**Business Rule Task** "（业务规则任务）

下一步，将属性面板中的 `Implementation` 选择为 `DMN`

输入 `Decision Ref` 为 `approve-payment`

为了将决策的结果存入流程变量，我们还需要编辑结果变量 `Result Variable` 为 `approved`

结果类型 `Map Decision Result` 选择为 `singleEntry` ，最后结果如下：

最后保存并部署到流程引擎

点击 _File > New File > DMN Diagram_ 创建一个新的DMN

现在画布上已经存在一个决策元素了，选择它，然后在右侧属性面板中更改Id和Name，这里的Id需要和流程中的 `Decision Ref`属性一致，这次我们输入Id为 `approve-payment`

接下来，点击决策元素左上角的表格按钮，创建新的DMN表

首先编辑输入参数，在本例中，为了简单，我们依据项目名进行判断，规则可以使用 _FEEL 表达式_、_JUEL_ 或者 _Script_ 书写，详情可以阅读[DMN引擎中的表达式](https://link.zhihu.com/?target=https%3A//docs.camunda.org/manual/latest/user-guide/dmn-engine/expressions-and-scripts/)

双击表格中的_Input_，编辑第一个输入参数

Label : 项目

Expression : item

下面，设置输入参数，双击_Output_编辑

Label : 是否同意

Name: approved

Type : boolean

下面，我们点击左侧的蓝色加号，添加一些规则，最后类似这样：

点击顶部的部署按钮，将DMN部署到流程引擎中

现在打开 http://localhost:8080/camunda/app/cockpit/ ，使用demo/demo登录，可以看到决策定义增加了一个，点击进去可以看到刚才编辑的DMN

接下来，打开 http://localhost:8080/camunda/app/tasklist/ ，使用demo/demo登录，点击 Start process 按钮，使用以下参数启动：

再点击一次 Start process 按钮，使用如下参数启动：

这里，我们改变了输入的 item，下面我们打开 http://localhost:8080/camunda/app/cockpit/ 点击 _Decision Definitions_ ，可以看到刚才创建的两个流程实例，点击查看"批准付款"的决策情况

> 到此第五部分结束，如果想直接获取到现在为止的进度，可以使用如下命令
git checkout -f Step-5

恭喜：你已经完成了全部的快速入门部分，想要继续学习，请前往[Camunda 文档](https://link.zhihu.com/?target=https%3A//docs.camunda.org/manual/latest/).

也可以关注我的博客[http://www.shaochenfeng.com ;](https://link.zhihu.com/?target=http%3A//www.shaochenfeng.com)，后续也会继续推出更多Camunda文档翻译或教程
