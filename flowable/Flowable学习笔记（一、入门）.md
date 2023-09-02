---
link: https://juejin.cn/post/6844904158231789582
title: Flowable学习笔记（一、入门）
description: Flowable是一个使用Java编写的轻量级业务流程引擎。Flowable流程引擎可用于部署BPMN 2.0流程定义（用于定义流程的行业XML标准）， 创建这些流程定义的流程实例，进行查询，访问运行中或历史的流程实例与相关数据，等等。这个章节将用一个可以在你自己的开发环境中使…
keywords: Java
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2020-05-03T07:19:08.000Z
publisher: 稀土掘金
stats: paragraph=98 sentences=101, words=555
---
## 1、Flowable是什么

Flowable是一个使用Java编写的轻量级业务流程引擎。Flowable流程引擎可用于部署BPMN 2.0流程定义（用于定义流程的行业XML标准）， 创建这些流程定义的流程实例，进行查询，访问运行中或历史的流程实例与相关数据，等等。这个章节将用一个可以在你自己的开发环境中使用的例子，逐步介绍各种概念与API。

Flowable可以十分灵活地加入你的应用/服务/构架。可以将JAR形式发布的Flowable库加入应用或服务，来嵌入引擎。 以JAR形式发布使Flowable可以轻易加入任何Java环境：Java SE；Tomcat、Jetty或Spring之类的servlet容器；JBoss或WebSphere之类的Java EE服务器，等等。 另外，也可以使用Flowable REST API进行HTTP调用。也有许多Flowable应用（Flowable Modeler, Flowable Admin, Flowable IDM 与 Flowable Task），提供了直接可用的UI示例，可以使用流程与任务。

所有使用Flowable方法的共同点是核心引擎。核心引擎是一组服务的集合，并提供管理与执行业务流程的API。 下面的教程从设置与使用核心引擎的介绍开始。后续章节都建立在之前章节中获取的知识之上。

## 2、Flowable与Activiti

Flowable，2016年基于Activiti诞生。

## 1、构建命令行程序

我们将构建的例子是一个简单的请假(holiday request)流程：

* 雇员(employee)申请几天的假期
* 经理(manager)批准或驳回申请
* 我们会模拟将申请注册到某个外部系统，并给雇员发送结果邮件

## 1.1、创建流程引擎

创建一个名为holiday-request的maven项目，添加依赖：

```
        <!--Flowable流程引擎-->
        <dependency>
            <groupid>org.flowable</groupid>
            <artifactid>flowable-engine</artifactid>
            <version>6.3.0</version>
        </dependency>
        <!--MySQL驱动，这里采用MySQL数据库，如果采用其它数据库，需要引入对应的依赖。-->
        <dependency>
            <groupid>mysql</groupid>
            <artifactid>mysql-connector-java</artifactid>
            <version>8.0.15</version>
        </dependency>
```

* 创建一个数据库flowable_demo。
* 创建一个普通的Java类：HolidayRequest

```

<span class="hljs-keyword">public</span> <span class="hljs-class"><span class="hljs-keyword">class</span> <span class="hljs-title">HolidayRequest</span> </span>{
    <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">static</span> <span class="hljs-keyword">void</span> <span class="hljs-title">main</span><span class="hljs-params">(String[] args)</span> </span>{

        ProcessEngineConfiguration cfg=<span class="hljs-keyword">new</span> StandaloneProcessEngineConfiguration()

                .setJdbcUrl(<span class="hljs-string">"jdbc:mysql://localhost:3306/flowable_demo?useUnicode=true&characterEncoding=utf8&serverTimezone=GMT%2b8&nullCatalogMeansCurrent=true"</span>)
                .setJdbcUsername(<span class="hljs-string">"root"</span>)
                .setJdbcPassword(<span class="hljs-string">"root"</span>)
                .setJdbcDriver(<span class="hljs-string">"com.mysql.jdbc.Driver"</span>)
                .setDatabaseSchemaUpdate(ProcessEngineConfiguration.DB_SCHEMA_UPDATE_TRUE);

        ProcessEngine processEngine=cfg.buildProcessEngine();
    }
}
```

运行该类，会发现在数据库flowable_demo中创建了34个表：

**flowable命名规则：**

* ACT_RE_* ：' RE '表示repository（存储）。RepositoryService接口操作的表。带此前缀的表包含的是静态信息，如，流程定义，流程的资源（图片，规则等）。
* ACT_RU_* ：' RU '表示runtime。这是运行时的表存储着流程变量，用户任务，变量，职责（job）等运行时的数据。flowable只存储实例执行期间的运行时数据，当流程实例结束时，将删除这些记录。这就保证了这些运行时的表小且快。
* ACT_ID_* : ' ID '表示identity(组织机构)。这些表包含标识的信息，如用户，用户组，等等。
* ACT_HI_* : ' HI '表示history。就是这些表包含着历史的相关数据，如结束的流程实例，变量，任务，等等。
* ACT_GE_* : 普通数据，各种情况都使用的数据。

**34张表说明：**

表分类 表名 表说明 一般数据(2) ACT_GE_BYTEARRAY 通用的流程定义和流程资源 ACT_GE_PROPERTY 系统相关属性 流程历史记录(8) ACT_HI_ACTINST 历史的流程实例 ACT_HI_ATTACHMENT 历史的流程附件 ACT_HI_COMMENT 历史的说明性信息 ACT_HI_DETAIL 历史的流程运行中的细节信息 ACT_HI_IDENTITYLINK 历史的流程运行过程中用户关系 ACT_HI_PROCINST 历史的流程实例 ACT_HI_TASKINST 历史的任务实例 ACT_HI_VARINST 历史的流程运行中的变量信息 用户用户组表(9) ACT_ID_BYTEARRAY 二进制数据表 ACT_ID_GROUP 用户组信息表 ACT_ID_INFO 用户信息详情表 ACT_ID_MEMBERSHIP 人与组关系表 ACT_ID_PRIV 权限表 ACT_ID_PRIV_MAPPING 用户或组权限关系表 ACT_ID_PROPERTY 属性表 ACT_ID_TOKEN 系统登录日志表 ACT_ID_USER 用户表 流程定义表(3) ACT_RE_DEPLOYMENT 部署单元信息 ACT_RE_MODEL 模型信息 ACT_RE_PROCDEF 已部署的流程定义 运行实例表(10) ACT_RU_DEADLETTER_JOB 正在运行的任务表 ACT_RU_EVENT_SUBSCR 运行时事件 ACT_RU_EXECUTION 运行时流程执行实例 ACT_RU_HISTORY_JOB 历史作业表 ACT_RU_IDENTITYLINK 运行时用户关系信息 ACT_RU_JOB 运行时作业表 ACT_RU_SUSPENDED_JOB 暂停作业表 ACT_RU_TASK 运行时任务表 ACT_RU_TIMER_JOB 定时作业表 ACT_RU_VARIABLE 运行时变量表 其他表(2) ACT_EVT_LOG 事件日志表 ACT_PROCDEF_INFO 流程定义信息

在上面的运行中，同时可以看到，控制台有报错的信息，这是日志没有正确地配置：

Flowable使用SLF4J作为内部日志框架。我们使用log4j作为SLF4J的实现。因此在pom.xml文件中添加下列依赖：

```
<dependency>
  <groupid>org.slf4j</groupid>
  <artifactid>slf4j-api</artifactid>
  <version>1.7.21</version>
</dependency>
<dependency>
  <groupid>org.slf4j</groupid>
  <artifactid>slf4j-log4j12</artifactid>
  <version>1.7.21</version>
</dependency>
```

在 src/resouce目录下新建log4j的配置文件log4j.properties:

```
log4j.rootLogger=DEBUG, CA

log4j.appender.CA=org.apache.log4j.ConsoleAppender
log4j.appender.CA.layout=org.apache.log4j.PatternLayout
log4j.appender.CA.layout.ConversionPattern= %d{hh:mm:ss,SSS} [%t] %-<span class="hljs-number">5</span>p %c %x - %m%n
```

再次运行，可以看到关于引擎启动与创建数据库表结构的提示日志：

## 1.2、部署流程定义

要构建的流程是一个非常简单的请假流程。Flowable引擎需要流程定义为BPMN 2.0格式，这是一个业界广泛接受的XML标准。

在Flowable术语中，我们将其称为一个流程定义(process definition)。一个流程定义可以启动多个流程实例(process instance)。流程定义可以看做是重复执行流程的蓝图。 在这个例子中，流程定义定义了请假的各个步骤，而一个流程实例对应某个雇员提出的一个请假申请。

BPMN 2.0存储为XML，并包含可视化的部分：使用标准方式定义了每个步骤类型（人工任务，自动服务调用，等等）如何呈现，以及如何互相连接。这样BPMN 2.0标准使技术人员与业务人员能用双方都能理解的方式交流业务流程。

我们要使用的流程定义为：

流程定义的说明：

* 我们假定启动流程需要提供一些信息，例如雇员名字、请假时长以及说明。当然，这些可以单独建模为流程中的第一步。 但是如果将它们作为流程的"输入信息"，就能保证只有在实际请求时才会建立一个流程实例。否则（将提交作为流程的第一步），用户可能在提交之前改变主意并取消，但流程实例已经创建了。 在某些场景中，就可能影响重要的指标（例如启动了多少申请，但还未完成），取决于业务目标。
* 左侧的圆圈叫做**启动事件(start event)**。这是一个流程实例的起点。
* 第一个矩形是一个**用户任务(user task)**。这是流程中人类用户操作的步骤。在这个例子中，经理需要批准或驳回申请。
* 取决于经理的决定，**排他网关(exclusive gateway)**(带叉的菱形)会将流程实例路由至批准或驳回路径。
* 如果批准，则需要将申请注册至某个外部系统，并跟着另一个用户任务，将经理的决定通知给申请人。当然也可以改为发送邮件。
* 如果驳回，则为雇员发送一封邮件通知他。

这里我们直接撰写XML，以熟悉BPMN 2.0及其概念。

以下是与上面展示的流程图对应的BPMN 2.0 XML。这里只包含了"流程部分"。如果使用图形化建模工具，实际的XML文件还将包含"可视化部分"，用于描述图形信息，如流程定义中各个元素的坐标（所有的图形化信息包含在XML的BPMNDiagram标签中，作为definitions标签的子元素）。

在src/main/resources文件夹下创建为holiday-request.bpmn20.xml文件：

```
<?xml version="1.0" encoding="utf-8" ?>

<definitions xmlns="http://www.omg.org/spec/BPMN/20100524/MODEL" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:bpmndi="http://www.omg.org/spec/BPMN/20100524/DI" xmlns:omgdc="http://www.omg.org/spec/DD/20100524/DC" xmlns:omgdi="http://www.omg.org/spec/DD/20100524/DI" xmlns:flowable="http://flowable.org/bpmn" typelanguage="http://www.w3.org/2001/XMLSchema" expressionlanguage="http://www.w3.org/1999/XPath" targetnamespace="http://www.flowable.org/processdef">

    <process id="holiday-request" name="Holiday Request" isexecutable="true">
        <!--开始事件：流程实例的起点-->
        <startevent id="startEvent">
        <!--顺序流：执行时会从一个活动流向另一个活动-->
        <sequenceflow sourceref="startEvent" targetref="approveTask">

        <!--用户任务：需要人工来进行操作-->
        <usertask id="approveTask" name="Approve or reject request">
        <sequenceflow sourceref="approveTask" targetref="decision">

        <!--排他网关-->
        <exclusivegateway id="decision">
        <sequenceflow sourceref="decision" targetref="externalSystemCall">
            <!--顺序流条件：以表达式(expression)的形式定义了条件(condition) -->
            <conditionexpression xsi:type="tFormalExpression">
                <!--条件表达式：是${approved == true}的简写-->
                <!--[CDATA[
                  ${approved}
                ]]-->
            </conditionexpression>
        </sequenceflow>
        <sequenceflow sourceref="decision" targetref="sendRejectionMail">
            <conditionexpression xsi:type="tFormalExpression">
                <!--[CDATA[
                  ${!approved}
                ]]-->
            </conditionexpression>
        </sequenceflow>

        <!--服务任务，一个自动活动，它会调用一些服务-->
        <servicetask id="externalSystemCall" name="Enter holidays in external system" flowable:class="edu.hpu.process.CallExternalSystemDelegate">

        <usertask id="holidayApprovedTask" name="Holiday Approve!">
        <sequenceflow sourceref="holidayApprovedTask" targetref="approveEnd">

        <servicetask id="sendRejectionMail" name="Send out rejection email" flowable:class="edu.hpu.process.SendRejectionMail">
        <sequenceflow sourceref="sendRejectionMail" targetref="rejectEnd">

        <!--结束事件-->
        <endevent id="approveEnd">
        <endevent id="rejectEnd">
    </endevent></endevent></sequenceflow></servicetask></sequenceflow></usertask></servicetask></exclusivegateway></sequenceflow></usertask></sequenceflow></startevent></process>

</definitions>

```

* 每一个步骤（在BPMN 2.0术语中称作**活动(activity)**）都有一个id属性，为其提供一个在XML文件中唯一的标识符。所有的活动都可以设置一个名字，以提高流程图的可读性。
* 活动之间通过**顺序流(sequence flow)**连接，在流程图中是一个有向箭头。在执行流程实例时，执行(execution)会从启动事件沿着顺序流流向下一个活动。
* 离开**排他网关(带有X的菱形)**的顺序流很特别：都以表达式(expression)的形式定义了条件(condition) 。当流程实例的执行到达这个网关时，会计算条件，并使用第一个计算为true的顺序流。这就是排他的含义：**只选择一个。**当然如果需要不同的路由策略，可以使用其他类型的网关。
* 这里用作条件的表达式为${approved}，这是${approved == true}的简写。变量'approved'被称作流程变量(process variable)。流程变量是持久化的数据，与流程实例存储在一起，并可以在流程实例的生命周期中使用。在这个例子里，我们需要在特定的地方（当经理用户任务提交时，或者以Flowable的术语来说，完成(complete)时）设置这个流程变量，因为这不是流程实例启动时就能获取的数据。

现在我们已经有了流程BPMN 2.0 XML文件，下来需要将它部署(deploy)到引擎中。部署一个流程定义意味着：

* 流程引擎会将XML文件存储在数据库中，这样可以在需要的时候获取它。
* 流程定义转换为内部的、可执行的对象模型，这样使用它就可以启动流程实例。

将流程定义部署至Flowable引擎，需要使用RepositoryService，其可以从ProcessEngine对象获取。使用RepositoryService，可以通过XML文件的路径创建一个新的部署(Deployment)，并调用deploy()方法实际执行：

```

        RepositoryService repositoryService=processEngine.getRepositoryService();

        Deployment deployment=repositoryService.createDeployment()
                .addClasspathResource(<span class="hljs-string">"holiday-request.bpmn20.xml"</span>)
                .deploy();
```

我们现在可以通过API查询验证已经部署在引擎中的流程定义。通过RepositoryService创建的ProcessDefinitionQuery对象实现。

```

        ProcessDefinition processDefinition=repositoryService.createProcessDefinitionQuery()
                .deploymentId(deployment.getId())
                .singleResult();
        System.out.println(<span class="hljs-string">"Found process definition : "</span>+processDefinition.getName());
```

运行:

xml文件已经存储进了数据库：

## 1.3、启动流程实例

现在已经在流程引擎中部署了流程定义，因此可以使用这个流程定义作为"蓝图"启动流程实例。

* 要启动流程实例，需要提供一些初始化流程变量。一般来说，可以通过呈现给用户的表单，或者在流程由其他系统自动触发时通过REST API，来获取这些变量。在这个例子里，我们简化为使用java.util.Scanner类在命令行输入一些数据：

```

        Scanner scanner = <span class="hljs-keyword">new</span> Scanner(System.in);

        System.out.println(<span class="hljs-string">"Who are you?"</span>);
        String employee = scanner.nextLine();

        System.out.println(<span class="hljs-string">"How many holidays do you want to request?"</span>);
        Integer nrOfHolidays = Integer.valueOf(scanner.nextLine());

        System.out.println(<span class="hljs-string">"Why do you need them?"</span>);
        String description = scanner.nextLine();
```

* 接下来，我们使用RuntimeService启动一个流程实例。收集的数据作为一个java.util.Map实例传递，其中的键就是之后用于获取变量的标识符。这个流程实例使用key启动(还有其它方式)。这个key就是BPMN 2.0 XML文件中设置的id属性，在这个例子里是holiday-request。

```
<process id="<span" class="hljs-string">"holiday-request" name=<span class="hljs-string">"Holiday Request"</span> isExecutable=<span class="hljs-string">"true"</span>>
</process>
```

```

        RuntimeService runtimeService=processEngine.getRuntimeService();
        Map<string, object> variables = <span class="hljs-keyword">new</span> HashMap<string, object>();
        variables.put(<span class="hljs-string">"employee"</span>, employee);
        variables.put(<span class="hljs-string">"nrOfHolidays"</span>, nrOfHolidays);
        variables.put(<span class="hljs-string">"description"</span>, description);
        ProcessInstance processInstance=runtimeService.startProcessInstanceByKey(<span class="hljs-string">"holiday-request"</span>,variables);
</string,></string,>
```

在流程实例启动后，会创建一个**执行(execution)**，并将其放在启动事件上。从这里开始，这个执行会沿着顺序流移动到经理审批的用户任务，并执行用户任务行为。这个行为将在数据库中创建一个任务，该任务可以之后使用查询找到。用户任务是一个等待状态(wait state)，引擎会停止执行，返回API调用处。

输入流程初始化变量：

将数据插入数据库中

向数据库中插入了数据：

## 1.4、Flowable中的事务

在Flowable中，数据库事务扮演了关键角色，用于保证数据一致性，并解决并发问题。当调用Flowable API时，默认情况下，所有操作都是同步的，并处于同一个事务下。这意味着，当方法调用返回时，会启动并提交一个事务。

流程启动后，会有一个数据库事务从流程实例启动时持续到下一个等待状态。在这个例子里，指的是第一个用户任务。当引擎到达这个用户任务时，状态会持久化至数据库，提交事务，并返回API调用处。

在Flowable中，当一个流程实例运行时，总会有一个数据库事务从前一个等待状态持续到下一个等待状态。数据持久化之后，可能在数据库中保存很长时间，甚至几年，直到某个API调用使流程实例继续执行。请注意当流程处在等待状态时，不会消耗任何计算或内存资源，直到下一次APi调用。

在这个例子中，当第一个用户任务完成时，会启动一个数据库事务，从用户任务开始，经过排他网关（自动逻辑），直到第二个用户任务。或通过另一条路径直接到达结束。

## 1.5、查询与完成任务

在更实际的应用中，会为雇员及经理提供用户界面，让他们可以登录并查看任务列表。其中可以看到作为流程变量存储的流程实例数据，并决定如何操作任务。在这个例子中，我们通过执行API调用来模拟任务列表，通常这些API都是由UI驱动的服务在后台调用的。

我们还没有为用户任务配置办理人。

* 将第一个任务指派给"经理(managers)"组，而第二个用户任务指派给请假申请的提交人。

```
        <!--将任务指派给经理组-->
        <usertask id="<span" class="hljs-string">"approveTask" name=<span class="hljs-string">"Approve or reject request"</span> flowable:candidateGroups=<span class="hljs-string">"managers"</span>/>
</usertask>
```

```
       <!--指派给请假审批的审批人，${employee}使用流程变量动态指派，在流程实例启动时传递-->
        <usertask id="<span" class="hljs-string">"holidayApprovedTask" name=<span class="hljs-string">"Holiday Approve!"</span> flowable:assignee=<span class="hljs-string">"${employee}"</span>/>
</usertask>
```

* 要获得实际的任务列表，需要通过TaskService创建一个TaskQuery。这个查询配置为只返回'managers'组的任务：

```

        TaskService taskService=processEngine.getTaskService();
        List<task> tasks=taskService.createTaskQuery().taskCandidateGroup(<span class="hljs-string">"managers"</span>).list();
        System.out.println(<span class="hljs-string">"You have "</span> + tasks.size() + <span class="hljs-string">" tasks:"</span>);
        <span class="hljs-keyword">for</span> (<span class="hljs-keyword">int</span> i=<span class="hljs-number">0</span>; i<tasks.size(); i++) { system.out.println((i+<span class="hljs-number">1) + <span class="hljs-string">") "</span> + tasks.get(i).getName());
        }
</tasks.size();></task>
```

* 使用集合下标获取特定流程实例的变量,在控制台输出

```

        System.out.println(<span class="hljs-string">"Which task would you like to complete?"</span>);
        <span class="hljs-keyword">int</span> taskIndex = Integer.valueOf(scanner.nextLine());
        Task task=tasks.get(taskIndex-<span class="hljs-number">1</span>);
        Map<string,object> processVariables=taskService.getVariables(task.getId());
        System.out.println(processVariables.get(<span class="hljs-string">"employee"</span>) + <span class="hljs-string">" wants "</span> +
                processVariables.get(<span class="hljs-string">"nrOfHolidays"</span>) + <span class="hljs-string">" of holidays. Do you approve this?"</span>);
</string,object>
```

* 经理现在就可以完成任务了。在实际开发中，通常由用户提交一个表单。表单中的数据作为流程变量传递。在这里，我们在完成任务时传递带有'approved'变量（这个名字很重要，因为之后会在顺序流的条件中使用）的map来模拟：

```

        <span class="hljs-keyword">boolean</span> approved=scanner.nextLine().toLowerCase().equals(<span class="hljs-string">"y"</span>);
        variables = <span class="hljs-keyword">new</span> HashMap<string, object>();
        variables.put(<span class="hljs-string">"approved"</span>, approved);

        taskService.complete(task.getId(),variables);
</string,>
```

现在还缺最后一点，服务任务调用的服务没有实现：

```
       <!--服务任务，一个自动活动，它会调用一些服务-->
        <servicetask id="<span" class="hljs-string">"externalSystemCall" name=<span class="hljs-string">"Enter holidays in external system"</span> flowable:<span class="hljs-class"><span class="hljs-keyword">class</span></span>=<span class="hljs-string">"edu.hpu.process.CallExternalSystemDelegate"</span>/>
</servicetask>
```

* 创建一个类，实现JavaDelegate接口，实现execute方法，这个方法可以写很多业务逻辑，这里我们只是是在控制台打印输出一些内容：

```

<span class="hljs-keyword">public</span> <span class="hljs-class"><span class="hljs-keyword">class</span> <span class="hljs-title">CallExternalSystemDelegate</span> <span class="hljs-keyword">implements</span> <span class="hljs-title">JavaDelegate</span> </span>{
    <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">void</span> <span class="hljs-title">execute</span><span class="hljs-params">(DelegateExecution delegateExecution)</span> </span>{
        System.out.println(<span class="hljs-string">"Calling the external system for employee "</span>
                + delegateExecution.getVariable(<span class="hljs-string">"employee"</span>));
    }
}
```

运行：

启动流程：

* 查看任务![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/5/3/171d9668830bd3ff~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.png)
* 完成任务![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/5/3/171d9668a4cebd69~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.png)
* 执行自动逻辑![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/5/3/171d96689cb57eac~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.png)

至此，一个模拟请假流程就完成了。

## 1.6、使用历史数据

选择使用Flowable这样的流程引擎的原因之一，是它可以自动存储所有流程实例的审计数据或历史数据。这些数据可以用于创建报告，深入展现组织运行的情况，瓶颈在哪里，等等。

从ProcessEngine获取HistoryService，并创建历史活动(historical activities)的查询。

```

        HistoryService historyService=processEngine.getHistoryService();

        List<historicactivityinstance> activities =
                historyService.createHistoricActivityInstanceQuery()

                        .processInstanceId(processInstance.getId())

                        .finished()

                        .orderByHistoricActivityInstanceEndTime().asc()
                        .list();

        <span class="hljs-keyword">for</span> (HistoricActivityInstance activity : activities) {
            System.out.println(activity.getActivityId() + <span class="hljs-string">" took "</span>
                    + activity.getDurationInMillis() + <span class="hljs-string">" milliseconds"</span>);
        }
</historicactivityinstance>
```

**参考：**
