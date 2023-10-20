---
link: https://zhuanlan.zhihu.com/p/376904826
title: Camunda SpringBoot与进阶内容
description: 在开始之前，首先确保你已经完成了 快速入门部分 （http://shaochenfeng.com/blog/camunda-快速入门/）本章我们将继续完成 Camunda 官网上的其他入门部分，你将学到： Spring Boot部分： 创建 Camunda Spring Boot…
keywords: Spring Boot,流程设计,流程
author: 晨峰说一只程序猿
date: 2021-06-01T01:35:00.000Z
publisher: 知乎专栏
stats: paragraph=77 sentences=38, words=347
---
本章我们将继续完成 Camunda 官网上的其他入门部分，你将学到：

Spring Boot部分：

DMN部分：

本教程对应官方教程中 Spring Boot 、DMN 、 JAVA Process Application 三章的内容，用一个例子串联起来，让我们开始吧！

## 创建 Camunda Spring Boot 项目

首先打开你喜欢的 IDE，新建一个空的Maven项目，本次的案例是"贷款审批"，所以项目名命名为 `loan-approval-spring-boot`，groupId为 `org.example`

也可以使用命令行创建

接下来添加 Maven 依赖，Maven依赖需要添加到项目根目录下的 `pom.xml` 文件中。我们需要将 Spring Boot 依赖添加到 "dependency management" 中，然后将 Camunda Spring Boot Starter 添加为依赖，这将提供 Camunda 流程引擎和自带的 WebApp ；为简单起见，数据库使用嵌入式H2数据库；最后添加 `spring-boot-maven-plugin` ，它会将Spring Boot项目打包在一起。最终完成后效果如下：

接下来，我们将为应用添加可以运行的主类，在 `src/main/java` 下新建一个类，类名为 `WebappExampleProcessApplication`，包名为 `org.example.loanapproval`

> 如果使用命名行创建项目，会存在一个自带的 `App.java`主类 ，这个不需要，删除即可

Spring Boot的主类需要添加 `@SpringBootApplication` 注解，最后完成的效果如下：

> 到此第一部分结束，如果想直接获取到现在为止的进度，可以下载[zip包](https://link.zhihu.com/?target=https%3A//gitee.com/zoollcar/camunda-get-started-spring-boot-and-dmn/repository/archive/Step-1.zip)，或者使用如下命令<br> `sh git clone https://gitee.com/zoollcar/camunda-get-started-spring-boot-and-dmn.git git checkout -f Step-1`

让我们在 `src/main/resources` 下创建 `application.yaml`文件，输入如下内容：

以上配置将使用 demo 作为管理员用户名密码，在 `Tasklist`中添加 `All tasks` 过滤器

IDE通常有工具可以直接编译运行，使用工具运行即可

也可以使用命令行运行：

运行后打开浏览器访问：[http://localhost:8080/](https://link.zhihu.com/?target=http%3A//localhost%3A8080/) ，会自动打开登录界面，可以使用 demo/demo 登录，然后打开 Tasklist，可以看到 "All tasks"也被创建出来了。

> 到此第二部分结束，如果想直接获取到现在为止的进度，可以下载[zip包](https://link.zhihu.com/?target=https%3A//gitee.com/zoollcar/camunda-get-started-spring-boot-and-dmn/repository/archive/Step-2.zip)，或者使用如下命令<br> `sh git clone https://gitee.com/zoollcar/camunda-get-started-spring-boot-and-dmn.git git checkout -f Step-2`

## 部署BPMN2.0流程

在本章中，我们将要为刚才的Spring Boot项目部署流程，并使用监听器，在流程部署时发起流程

下载并打开 Camunda Modeler，编辑只包含一个人工节点的简单流程

点击空白处，编辑流程id为 `loan-approval` ，Name 为 `Loan approval`

流程编辑完成后，保存流程模型到项目的 `src/main/resources` 文件夹中，命名为 `loan-approval.bpmn`，然后重新编译运行项目。

> 如果遇到报错：
src-resolve: 无法将名称 'XXXX' 解析为 'element declaration' 组件。
请检查项目路径是否有中文，这里要求路径不能包含中文。

现在我们在 Camunda Spring Boot 项目中添加 `@EnableProcessApplication` 注解，这会提供更多可配置项，以及启用更多流程相关注解。

添加完注解后，需要新建 Camunda 配置文件——`src/main/resources/META-INF/processes.xml`， `META-INF`文件夹可能不存在，需要新建后，在其中增加 `processes.xml`，这个文件暂时留空，但对于 Camunda 来说却是必要的。

下一步是一项测试，我们希望在流程部署完成后发起一次流程，需要用到事件监听注解 `@EventListener`，监听的类型为 `PostDeployEvent` （流程部署），修改后的 `WebappExampleProcessApplication` 如下：

> 到此第三部分结束，如果想直接获取到现在为止的进度，可以下载[zip包](https://link.zhihu.com/?target=https%3A//gitee.com/zoollcar/camunda-get-started-spring-boot-and-dmn/repository/archive/Step-3.zip)，或者使用如下命令<br> `sh git clone https://gitee.com/zoollcar/camunda-get-started-spring-boot-and-dmn.git git checkout -f Step-3`

## 添加自定义表单

我们需要将自定义表单放入Spring Boot静态资源文件夹，在 `resources` 中新建目录 `static/forms`，然后新建申请表单，内容如下，命名为 `loan-approval.html`，保存到 `resources/static/forms` 目录

打开 Camunda Modeler ，点击 "发起贷款申请" 节点，在右侧属性菜单选择Forms，写入 `embedded:app:forms/loan-approval.html` 这意味着表单将以嵌入式的方式添加进 Tasklist 中

同样的，我们新建 `loan-auth.html` 到 `resources/static/forms` 目录，内容如下：

打开 Camunda Modeler ，点击 "检查申请单" 节点，在右侧属性菜单选择Forms，写入 `embedded:app:forms/loan-auth.html`

重新编译并运行项目，浏览器中打开 [http://localhost:8080/](https://link.zhihu.com/?target=http%3A//localhost%3A8080/) ，点击 Tasklist 进入，点击右上角 `Start Process`，可以看到表单已经嵌入到 Tasklist 中了，发起流程后，任务列表中可以看到审核界面的表单

> 到此第四部分结束，如果想直接获取到现在为止的进度，可以下载[zip包](https://link.zhihu.com/?target=https%3A//gitee.com/zoollcar/camunda-get-started-spring-boot-and-dmn/repository/archive/Step-4.zip)，或者使用如下命令<br> `sh git clone https://gitee.com/zoollcar/camunda-get-started-spring-boot-and-dmn.git git checkout -f Step-4`

## 在流程中调用 JAVA Class

在本章中我们将学到如何使用流程的 `Service task&#xFF08;&#x670D;&#x52A1;&#x4EFB;&#x52A1;&#xFF09;` 中调用JAVA类。

打开 Camunda Modeler ，在左侧工具架中选择 Task（矩形），拖动到"检查申请单"后面，双击命名为"发放贷款"

点击新创建的节点，点击右侧弹出的扳手按钮，将类型修改为 Service Task。

现在我们需要添加 Service Task 可以调用的实现类，将类添加到 `org.example.loanapproval` 包下，命名为 `OfferLoan`，这里我们希望打印贷款人信息到日志，具体代码如下：

回到 Camunda Modeler ，选择"发放贷款"节点，在右侧的属性面板中，选择 `Implementation`为 `Java Class` ，Java Class 为我们刚才创建的类 `org.example.loanapproval.OfferLoan`

保存流程

刷新页面，完成人工任务

检查 Spring Boot 程序控制台输出，可以看到贷款人信息已经被打印出来了

> 到此第五部分结束，如果想直接获取到现在为止的进度，可以下载[zip包](https://link.zhihu.com/?target=https%3A//gitee.com/zoollcar/camunda-get-started-spring-boot-and-dmn/repository/archive/Step-5.zip)，或者使用如下命令<br> `sh git clone https://gitee.com/zoollcar/camunda-get-started-spring-boot-and-dmn.git git checkout -f Step-5`

## 在 JAVA 中调用 DMN 决策

在本章中，我们将会学到创建一个DMN决策并部署到流程引擎中，以及在JAVA里调用测试我们的DMN。

我们使用BMI健康状况作为案例，BMI是身高体重的比值，通常用于判断人的胖瘦程度。

启动 Camunda Modeler，点击工具栏最左侧的新建按钮，新建一个 DMN

点击画布上默认的DMN决策表，修改在右侧的属性面板中的 `id`为 `bmi-classification`， `Name`为 `BMI Classification`

下一步，点击左上角的表格按钮，进入决策编辑界面

双击 input 区域，_input_修改为 `BMI`，_Expression_修改为 `bmi`，_Type_选择 `douoble`

同样的，双击output区域，_output_修改为 `result`，_Output Name_修改为 `result`，_Type_选择 `string`

点击表格左侧的蓝色加号，添加BMI分类规则，最终效果如下：

> 默认的规则就是"唯一"，这条规则可以自动匹配，不需要更改。

编辑完成后，点击工具栏"save"按钮，将流程保存到项目的 `src/main/resources`目录下。

> 到此第六部分结束，如果想直接获取到现在为止的进度，可以下载[zip包](https://link.zhihu.com/?target=https%3A//gitee.com/zoollcar/camunda-get-started-spring-boot-and-dmn/repository/archive/Step-6.zip)，或者使用如下命令<br> `sh git clone https://gitee.com/zoollcar/camunda-get-started-spring-boot-and-dmn.git git checkout -f Step-6`

我们将在决策表部署完成后调用测试，因此如下修改主类：

> 到此第七部分结束，如果想直接获取到现在为止的进度，可以下载[zip包](https://link.zhihu.com/?target=https%3A//gitee.com/zoollcar/camunda-get-started-spring-boot-and-dmn/repository/archive/Step-7zip)，或者使用如下命令<br> `sh git clone https://gitee.com/zoollcar/camunda-get-started-spring-boot-and-dmn.git git checkout -f Step-7`

编译项目并重新运行，查看 Spring Boot 输出，如果看到：

表示执行成功了

## 从DMN决策表与DRG决策图

这一章，我们将创建新的DMN决策表，并将两个DMN决策表组成一张DRG决策图，最后通过JAVA调用，获得两个决策表协同得到的结果。

> DMN与DRG：
DMN本质上就是一组输入输出的对应关系，体现的是一个简单决策，不应该过于复杂，对于需要多次决策才能得到答案的复杂决策，可以将多个简单的DMN连接起来，组成DRG决策图。
DRG与DRD
DRD是使用可视化的方式显示和编辑DRG的方式，因为Camunda Modeler是可视化的，所以在Camunda Modeler中显示未DRD

点击左上角的 Edit DRD 切换到 DRD编辑界面

就像编辑流程一样，点击空白处，修改DRD的id为 `bmi-health` ，Name为 `BMI Health`

现在可以创建一个新的决策了，我们希望在评估完BMI类型后给出建议，则拖动左侧正方形到画布，双击修改名称为 `BMI Suggestion`，切换类型为 Decision Table，最后修改id为 `bmi-suggestion`

点击 "BMI Classification" 节点，选择弹出的连线，连接到新的"BMI Suggestion"节点，这表示，新节点[依赖于](https://link.zhihu.com/?target=https%3A//docs.camunda.org/manual/latest/reference/dmn11/drg/%23required-decisions)"BMI Classification"

点击 "BMI Suggestion" 节点左上角的表格按钮，进入编辑界面，输入内容如下：

"Input"为 `result` ，"Expression"为 `result`

"Output"为 `suggestion` ，"Expression"为 `suggestion`

执行DRG决策图的方法就是执行它的最后一个节点，因为DRG的节点之间具有依赖关系，在执行最后一个节点前会执行它所以来的决策

所以新增的代码与执行DMN一样，修改后的主类如下：

重新编译并运行，可以看到 Spring Boot 输出：

表示执行成功

> 到此第八部分结束，如果想直接获取到现在为止的进度，可以下载[zip包](https://link.zhihu.com/?target=https%3A//gitee.com/zoollcar/camunda-get-started-spring-boot-and-dmn/repository/archive/Step-8.zip)，或者使用如下命令<br> `sh git clone https://gitee.com/zoollcar/camunda-get-started-spring-boot-and-dmn.git git checkout -f Step-8`
恭喜：你已经完成了快速入门的全部内容，想要继续学习，请前往[Camunda 文档](https://link.zhihu.com/?target=https%3A//docs.camunda.org/manual/latest/).

也可以关注我的博客[http://shaochenfeng.com](https://link.zhihu.com/?target=http%3A//shaochenfeng.com/)，后续也会继续推出更多Camunda文档翻译或教程
