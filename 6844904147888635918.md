---
link: https://juejin.cn/post/6844904147888635918
title: Flowable学习笔记（二、BPMN 2.0-基础 ）
description: 业务流程模型和标记法（BPMN, Business Process Model and Notation）是一套图形化表示法，用于以业务流程模型详细说明各种业务流程。 它最初由业务流程管理倡议组织（BPMI, Business Process Management Initia…
keywords: Java
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2020-05-05T02:46:32.000Z
publisher: 稀土掘金
stats: paragraph=80 sentences=34, words=202
---
业务流程模型和标记法（BPMN, Business Process Model and Notation）是一套图形化表示法，用于以业务流程模型详细说明各种业务流程。

它最初由业务流程管理倡议组织（BPMI, Business Process Management Initiative）开发，名称为"Business Process Modeling Notation"，即"业务流程建模标记法"。BPMI于2005年与对象管理组织（OMG, Object Management Group）合并。2011年1月OMG发布2.0版本，同时改为现在的名称。

BPMN包含四种要素：

**流对象（Flow Object）**:

* 事件（Events）
* 活动（Activities）
* 网关（Gateways）

**连接对象（Connecting Objects）:**

* 顺序流（Sequence Flow）
* 消息流（Message Flow）
* 关联（Association）

**泳道（Swimlanes）：**

* 池（Pool）
* 道（Lane）

**附加工件（Artifacts/Artefacts）:**

* 数据对象（Data Object）
* 组（Group）
* 注释（Annotation）

在这里需要重要关注4个基本对象，

* 事件（Event）：用来表明流程的生命周期中发生了什么。
* 活动（Activity）：活动（Activities）是业务流程定义的核心元素，中文称为"活动"、"节点"、"步骤"。一个活动可以是流程的基本处理单元（如人工任务、服务任务），也可以是一个组合单元（如外部子流程、嵌套子流程）。
* 网关（Gateway）：用来控制流程的流向。
* 流向/顺序流（Flow）：是连接两个流程节点的连线。

一个BPMN 2.0 XML流程的根是definitions元素。 在命名状态，子元素会包含真正的业务流程定义。 每个process子元素 可以拥有一个id（必填）和 name（可选）。下面是一个空的BPMN 2.0业务流程 。

```
<definitions id="<span" class="hljs-string">"myProcesses"
  xmlns:xsi=<span class="hljs-string">"http://www.w3.org/2001/XMLSchema-instance"</span>
  xsi:schemaLocation=<span class="hljs-string">"http://schema.omg.org/spec/BPMN/2.0 BPMN20.xsd"</span>
  xmlns=<span class="hljs-string">"http://schema.omg.org/spec/BPMN/2.0"</span>
  typeLanguage=<span class="hljs-string">"http://www.w3.org/2001/XMLSchema"</span>
  expressionLanguage=<span class="hljs-string">"http://www.w3.org/1999/XPath"</span>
  targetNamespace=<span class="hljs-string">"http://jbpm.org/example/bpmn2"</span>>

  <process id="<span" class="hljs-string">"My business processs" name=<span class="hljs-string">"myBusinessProcess"</span>>
</process></definitions>
```

在BPMN中，流对象是用于定义业务流程行为的主要图形元素。需要重点理解三个流对象。

## 4.1、事件

事件包含启动事件、结束事件、中间事件，还有一类边界事件，属于中间中间事件的一种。

**启动事件（start event）**（有的译为开始时间）是流程的起点。启动事件的类型（例如流程在消息到达时启动，在指定的时间间隔后启动，等等），定义了流程如何启动，并显示为启动事件中的小图标。在XML中，类型由子元素声明来定义。

启动事件随时捕获：启动事件（保持）等候，直到特定的触发器被触发。

* 描述：空"启动事件（none Start Event），指的是未指定启动流程实例触发器的启动事件。引擎将无法预知何时启动流程实例。空启动事件用于流程实例通过调用下列startProcessInstanceByXXX API方法启动的情况。

```
	ProcessInstance processInstance = runtimeService.startProcessInstanceByXXX();
```

* 图示：空启动事件用空心圆圈表示，中间没有图标（也就是说，没有触发器）:

* xml表示：

```
<startevent id="<span" class="hljs-string">"start" name=<span class="hljs-string">"my start event"</span> />
</startevent>
```

* 描述：定时器启动事件（timer start event）在指定时间创建流程实例。在流程只需要启动一次，或者流程需要在特定的时间间隔重复启动时，可以使用定时器启动事件。

请注意：子流程不能有定时器启动事件。 请注意：定时器启动事件，在流程部署的同时就开始计时。不需要调用startProcessInstanceByXXX就会在时间启动。调用startProcessInstanceByXXX时会在定时启动之外额外启动一个流程。 请注意：当部署带有定时器启动事件的流程的更新版本时，上一版本的定时器作业会被移除。这是因为通常并不希望旧版本的流程仍然自动启动新的流程实例。

* 图示：定时器启动事件，用其中有一个钟表图标的圆圈来表示。![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/5/5/171e2b9bfccfaf67~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.awebp)
* XML表示:定时器启动事件的XML表示格式，是普通的启动事件声明加上定时器定义子元素。

示例：流程会启动4次，间隔5分钟，从2011年3月11日，12:13开始

```
<startevent id="theStart">
  <timereventdefinition>
    <timecycle>R4/2011-03-11T12:13/PT5M</timecycle>
  </timereventdefinition>
</startevent>
```

示例：流程会在设定的时间启动一次

```
<startevent id="theStart">
  <timereventdefinition>
    <timedate>2011-03-11T12:13:14</timedate>
  </timereventdefinition>
</startevent>
```

* 描述： 消息启动事件（message start event）使用具名消息启动流程实例。消息名用于选择正确的启动事件。

当部署具有一个或多个消息启动事件的流程定义时，会做如下判断：

>> 给定流程定义中，消息启动事件的名字必须是唯一的。一个流程定义不得包含多个同名的消息启动事件。如果流程定义中有两个或多个消息启动事件引用同一个消息，或者两个或多个消息启动事件引用了具有相同消息名字的消息，则Flowable会在部署这个流程定义时抛出异常。

>> 在所有已部署的流程定义中，消息启动事件的名字必须是唯一的。如果在流程定义中，一个或多个消息启动事件引用了已经部署的另一流程定义中消息启动事件的消息名，则Flowable会在部署这个流程定义时抛出异常。

>> 流程版本：在部署流程定义的新版本时，会取消上一版本的消息订阅，即使新版本中并没有这个消息事件）。

* 图示：消息启动事件用其中有一个消息事件标志的圆圈表示。这个标志并未填充，用以表示捕获（接收）行为。![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/5/5/171e2b9c3344f145~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.awebp)
* XML表示： 消息启动事件的XML表示格式，为普通启动事件声明加上messageEventDefinition子元素：

```
<definitions id="definitions" xmlns="http://www.omg.org/spec/BPMN/20100524/MODEL" xmlns:flowable="http://flowable.org/bpmn" targetnamespace="Examples" xmlns:tns="Examples">

  <message id="newInvoice" name="newInvoiceMessage">

  <process id="invoiceProcess">

    <startevent id="messageStart">
    	<messageeventdefinition messageref="tns:newInvoice">
    </messageeventdefinition></startevent>
    ...

  </process>

</message></definitions>
```

* 描述： 信号启动事件（signal start event），使用具名信号启动流程实例。这个信号可以由流程实例中的信号抛出中间事件（intermediary signal throw event），或者API（runtimeService.signalEventReceivedXXX方法）触发。两种方式都会启动所有拥有相同名字信号启动事件的流程定义。

请注意可以选择异步还是同步启动流程实例。

需要为API传递的signalName，是由signal元素的name属性决定的名字。signal元素由signalEventDefinition的signalRef属性引用。

* 图示： 信号启动事件用其中有一个信号事件标志的圆圈表示。这个标志并未填充，用以表示捕获（接收）行为。

* XML表示： 信号启动事件的XML表示格式，为普通启动事件声明，加上signalEventDefinition子元素：

```
<signal id="theSignal" name="The Signal">

<process id="processWithSignalStart1">
  <startevent id="theStart">
    <signaleventdefinition id="theSignalEventDefinition" signalref="theSignal">
  </signaleventdefinition></startevent>
  <sequenceflow id="flow1" sourceref="theStart" targetref="theTask">
  <usertask id="theTask" name="Task in process A">
  <sequenceflow id="flow2" sourceref="theTask" targetref="theEnd">
  <endevent id="theEnd">
</endevent></sequenceflow></usertask></sequenceflow></process>
</signal>
```

* 描述： 错误启动事件（error start event），可用于触发事件子流程（Event Sub-Process）。错误启动事件不能用于启动流程实例。 错误启动事件总是中断。
* 图示： 错误启动事件用其中有一个错误事件标志的圆圈表示。这个标志并未填充，用以表示捕获（接收）行为。

* XML表示： 错误启动事件的XML表示格式，为普通启动事件声明加上errorEventDefinition子元素：

```
<startevent id="messageStart">
	<erroreventdefinition errorref="someError">
</erroreventdefinition></startevent>
```

结束事件（end event）标志着流程或子流程中一个分支的结束。结束事件总是抛出（型）事件。这意味着当流程执行到达结束事件时，会抛出一个结果。结果的类型由事件内部的黑色图标表示。在XML表示中，类型由子元素声明给出。

* 描述： "空"结束事件(none end event)，意味着当到达这个事件时，没有特别指定抛出的结果。因此，引擎除了结束当前执行分支之外，不会多做任何事情。
* 图示： 空结束事件，用其中没有图标（没有结果类型）的粗圆圈表示。

* xml表示： 空事件的XML表示格式为普通结束事件声明，没有任何子元素（其它种类的结束事件都有子元素，用于声明其类型）。

```
<endevent id="<span" class="hljs-string">"end" name=<span class="hljs-string">"my end event"</span> />
</endevent>
```

* 描述： 当流程执行到达错误结束事件（error end event）时，结束执行的当前分支，并抛出错误。这个错误可以由匹配的错误边界中间事件捕获。如果找不到匹配的错误边界事件，将会抛出异常。
* 图示： 错误结束事件事件用内部有一个错误图标的标准结束事件（粗圆圈）表示。错误图标是全黑的，代表抛出的含义。

* XML表示： 错误结束事件表示为结束事件，加上errorEventDefinition子元素：

```
<endevent id="myErrorEndEvent">
  <erroreventdefinition errorref="myError">
</erroreventdefinition></endevent>
```

errorRef属性可以引用在流程外定义的error元素：

```
<error id="<span" class="hljs-string">"myError" errorCode=<span class="hljs-string">"123"</span> />
...

<process id="<span" class="hljs-string">"myProcess">
...

</process></error>
```

* 描述： 当到达终止结束事件（terminate end event）时，当前的流程实例或子流程会被终止。也就是说，当执行到达终止结束事件时，会判断第一个范围 scope（流程或子流程）并终止它。在BPMN 2.0中，子流程可以是嵌入式子流程，调用活动，事件子流程，或事务子流程。有一条通用规则：当存在多实例的调用过程或嵌入式子流程时，只会终止一个实例，其他的实例与流程实例不会受影响。 可以添加一个可选属性terminateAll。当其为true时，无论该终止结束事件在流程定义中的位置，也无论它是否在子流程（甚至是嵌套子流程）中，都会终止（根）流程实例。
* 图示： 终止结束事件用内部有一个全黑圆的标准结束事件（粗圆圈）表示。![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/5/5/171e2b9c6ee4c1f9~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.awebp)
* xml表示： 终止结束事件，表示为结束事件，加上terminateEventDefinition子元素。 terminateAll属性是可选的（默认为false）。

```
<endevent id="myEndEvent >
  <terminateEventDefinition flowable:terminateAll=" true">
</endevent>
```

* 描述： 取消结束事件（cancel end event）只能与BPMN事务子流程（BPMN transaction subprocess）一起使用。当到达取消结束事件时，会抛出取消事件，且必须由取消边界事件（cancel boundary event）捕获。取消边界事件将取消事务，并触发补偿（compensation）。
* 图示： 取消结束事件用内部有一个取消图标的标准结束事件（粗圆圈）表示。取消图标是全黑的，代表抛出的含义。![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/5/5/171e2b9c8337c1b4~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.awebp)
* xml表示： 取消结束事件，表示为结束事件，加上cancelEventDefinition子元素。

```
<endevent id="myCancelEndEvent">
  <canceleventdefinition>
</canceleventdefinition></endevent>
```

边界事件（boundary event）是捕获型事件，依附在活动（activity）上。边界事件永远不会抛出。这意味着当活动运行时，事件将监听特定类型的触发器。当捕获到事件时，会终止活动，并沿该事件的出口顺序流继续。

所有的边界事件都用相同的方式定义：

```
<boundaryevent id="myBoundaryEvent" attachedtoref="theActivity">
      <xxxeventdefinition>
</xxxeventdefinition></boundaryevent>
```

边界事件由下列元素定义：

* （流程范围内）唯一的标识符
* 由attachedToRef属性定义的，对该事件所依附的活动的引用。边界事件及其所依附的活动，应定义在相同级别（也就是说，边界事件并不包含在活动内）。
* 定义了边界事件的类型的，形如XXXEventDefinition的XML子元素（例如TimerEventDefinition，ErrorEventDefinition，等等）。查阅特定的边界事件类型，以了解更多细节。

* 描述: 定时器边界事件（timer boundary event）的行为像是跑表与闹钟。当执行到达边界事件所依附的活动时，将启动定时器。当定时器触发时（例如在特定时间间隔后），可以中断活动，并沿着边界事件的出口顺序流继续执行。
* 图示： 定时器边界事件用内部有一个定时器图标的标准边界事件（圆圈）表示。

* XML表示： 定时器边界事件与一般边界事件一样定义。其中类型子元素为timerEventDefinition元素。

```
<boundaryevent id="escalationTimer" cancelactivity="true" attachedtoref="firstLineSupport">
  <timereventdefinition>
    <timeduration>PT4H</timeduration>
  </timereventdefinition>
</boundaryevent>
```

* 描述 在活动边界上的错误捕获中间（事件），或简称错误边界事件（error boundary event），捕获其所依附的活动范围内抛出的错误。 在嵌入式子流程或者调用活动上定义错误边界事件最有意义，因为子流程的范围会包括其中的所有活动。错误可以由错误结束事件抛出。这样的错误会逐层向其上级父范围传播，直到在范围内找到一个匹配错误事件定义的错误边界事件。 当捕获错误事件时，会销毁边界事件定义所在的活动，同时销毁其中所有的当前执行（例如，并行活动，嵌套子流程，等等）。流程执行将沿着边界事件的出口顺序流继续。
* 图示： 错误边界事件用内部有一个错误图标的标准中间事件（两层圆圈）表示。错误图标是白色的，代表捕获的含义。![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/5/5/171e2b9ca19873b2~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.awebp)
* xml表示： 错误边界事件与标准边界事件一样定义：

```
<boundaryevent id="catchError" attachedtoref="mySubProcess">
  <erroreventdefinition errorref="myError">
</erroreventdefinition></boundaryevent>
```

* 描述: 依附在活动边界上的信号捕获中间（事件），或简称信号边界事件（signal boundary event），捕获与其信号定义具有相同名称的信号。

与其他事件例如错误边界事件不同的是，信号边界事件不只是捕获其所依附范围抛出的信号。信号边界事件为全局范围（广播）的，意味着信号可以从任何地方抛出，甚至可以是不同的流程实例。

* 图示： 信号边界事件，用内部有一个信号图标的标准中间事件（两层圆圈）表示。信号图标是白色的，代表捕获的含义。

* xml表示： 信号边界事件与标准边界事件一样定义：

```
<boundaryevent id="boundary" attachedtoref="task" cancelactivity="true">
    <signaleventdefinition signalref="alertSignal">
</signaleventdefinition></boundaryevent>
```

* 描述： 在活动边界上的消息捕获中间（事件），或简称消息边界事件（message boundary event），捕获与其消息定义具有相同消息名的消息。
* 图示： 消息边界事件，用内部有一个消息图标的标准中间事件（两层圆圈）表示。信号图标是白色的，代表捕获的含义。![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/5/5/171e2b9caeeaf1f4~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.awebp) 息边界事件既可以是中断型的（右图），也可以是非中断型的（左图）。
* XML表示： 消息边界事件与标准边界事件一样定义：

```
<boundaryevent id="boundary" attachedtoref="task" cancelactivity="true">
    <messageeventdefinition messageref="newCustomerMessage">
</messageeventdefinition></boundaryevent>
```

* 描述 依附在事务子流程边界上的取消捕获中间事件，或简称取消边界事件（cancel boundary event），在事务取消时触发。当取消边界事件触发时，首先会中断当前范围的所有活动执行。接下来，启动事务范围内所有有效的的补偿边界事件（compensation boundary event）。补偿会同步执行，也就是说在离开事务前，边界事件会等待补偿完成。当补偿完成时，沿取消边界事件的任何出口顺序流离开事务子流程。

>>> 一个事务子流程只允许使用一个取消边界事件。 >>> 如果事务子流程中有嵌套的子流程，只会对成功完成的子流程触发补偿。 >>> 如果取消边界事件放置在具有多实例特性的事务子流程上，如果一个实例触发了取消，则边界事件将取消所有实例。

* 图示： 取消边界事件，用内部有一个取消图标的标准中间事件（两层圆圈）表示。取消图标是白色的（未填充），代表捕获的含义。

* xml表示： 取消边界事件与标准边界事件一样定义：

```
<boundaryevent id="boundary" attachedtoref="transaction">
    <canceleventdefinition>
</canceleventdefinition></boundaryevent>
```

* 描述： 依附在活动边界上的补偿捕获中间（事件），或简称补偿边界事件（compensation boundary event），可以为活动附加补偿处理器。 补偿边界事件必须使用直接关联的方式引用单个的补偿处理器。 补偿边界事件与其它边界事件的活动策略不同。其它边界事件，例如信号边界事件，在其依附的活动启动时激活；当该活动结束时会被解除，并取消相应的事件订阅。而补偿边界事件不是这样。补偿边界事件在其依附的活动成功完成时激活，同时创建补偿事件的相应订阅。当补偿事件被触发，或者相应的流程实例结束时，才会移除订阅。请考虑下列因素： >>> 当补偿被触发时，会调用补偿边界事件关联的补偿处理器。调用次数与其依附的活动成功完成的次数相同。 >>>如果补偿边界事件依附在具有多实例特性的活动上，则会为每一个实例创建补偿事件订阅。 >>> 如果补偿边界事件依附在位于循环内部的活动上，则每次该活动执行时，都会创建一个补偿事件订阅。 >>> 如果流程实例结束，则取消补偿事件的订阅。
* 图示： 补偿边界事件，用内部有一个补偿图标的标准中间事件（两层圆圈）表示。补偿图标是白色的（未填充），代表捕获的含义。另外，补偿边界事件使用单向连接关联补偿处理器，如下图所示：![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/5/5/171e2b9ce06f9494~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.awebp)
* xml表示： 补偿边界事件与标准边界事件一样定义：

```
<boundaryevent id="compensateBookHotelEvt" attachedtoref="bookHotel">
    <compensateeventdefinition>
</compensateeventdefinition></boundaryevent>

<association associationdirection="One" id="a1" sourceref="compensateBookHotelEvt" targetref="undoBookHotel">

<servicetask id="undoBookHotel" isforcompensation="true" flowable:class="...">
</servicetask></association>
```

在开始事件和结束事件之间发生的事件都称为中间事件。中间事件会影响流程的流转路线，但不会启动或直接终止流程的执行。

中间事件按照其特性可以分为两类：中间Catching(捕获)事件和中间Throwing(抛出)事件，当流程到达中间Catching事件时，它会一直在等待被触发，直接接收到的信息，才会被触发，而当流程到达中间Throwing事件时，该事件会自动被触发并抛出相应的结果或者信息。

所有的捕获中间事件（intermediate catching events）都使用相同方式定义：

```
<intermediatecatchevent id="myIntermediateCatchEvent">
    <xxxeventdefinition>
</xxxeventdefinition></intermediatecatchevent>
```

捕获中间事件由下列元素定义:

* （流程范围内）唯一的标识符
* 定义了捕获中间事件类型的，形如XXXEventDefinition的XML子元素（例如TimerEventDefinition等）。查阅特定中间捕获事件类型，以了解更多细节。

* 描述： 定时器捕获中间事件（timer intermediate catching event）的行为像是跑表。当执行到达捕获事件时，启动定时器；当定时器触发时（例如在一段时间间隔后），沿定时器中间事件的出口顺序流继续执行。
* 图示： 定时器中间事件用内部有定时器图标的中间捕获事件表示。![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/5/5/171e2b9cf2d82768~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.awebp)
* XML表示： 定时器中间事件与捕获中间事件一样定义。子元素为timerEventDefinition。

```
<intermediatecatchevent id="timer">
  <timereventdefinition>
    <timeduration>PT5M</timeduration>
  </timereventdefinition>
</intermediatecatchevent>
```

* 描述： 信号捕获中间事件（signal intermediate catching event），捕获与其引用的信号定义具有相同信号名称的信号。

>>> 与其他事件如错误事件不同，信号在被捕获后不会被消耗。如果有两个激活的信号中间事件，捕获相同的信号事件，则两个中间事件都会被触发，哪怕它们不在同一个流程实例里。

* 图示： 信号捕获中间事件用内部有信号图标的标准中间事件（两层圆圈）表示。信号图标是白色的（未填充），代表捕获的含义。

* xml表示： 信号中间事件与捕获中间事件一样定义。子元素为signalEventDefinition。

```
<intermediatecatchevent id="signal">
  <signaleventdefinition signalref="newCustomerSignal">
</signaleventdefinition></intermediatecatchevent>
```

* 描述： 消息捕获中间事件（message intermediate catching event），捕获特定名字的消息。
* 图示： 消息捕获中间事件用内部有消息图标的标准中间事件（两层圆圈）表示。消息图标是白色的（未填充），代表捕获的含义。

* xml表示： 消息中间事件与捕获中间事件一样定义。子元素为messageEventDefinition。

```
<intermediatecatchevent id="message">
  <messageeventdefinition signalref="newCustomerMessage">
</messageeventdefinition></intermediatecatchevent>
```

所有的抛出中间事件（intermediate throwing evnet）都使用相同方式定义：

```
<intermediatethrowevent id="myIntermediateThrowEvent">
      <xxxeventdefinition>
</xxxeventdefinition></intermediatethrowevent>
```

抛出中间事件由下列元素定义:

* （流程范围内）唯一的标识符
* 定义了抛出中间事件类型的，形如XXXEventDefinition的XML子元素（例如signalEventDefinition等）。查阅特定中间抛出事件类型，以了解更多细节。

下面的流程图展示了空抛出中间事件（intermediate throwing none event）的简单例子。其用于指示流程已经到达了某种状态。

添加一个执行监听器后，空中间事件就可以成为很好的监视某些KPI（Key Performance Indicators 关键绩效指标）的钩子。

```
<intermediatethrowevent id="noneEvent">
  <extensionelements>
    <flowable:executionlistener class="org.flowable.engine.test.bpmn.event.IntermediateNoneEventTest$MyExecutionListener" event="start">
  </flowable:executionlistener></extensionelements>
</intermediatethrowevent>
```

* 描述： 信号抛出中间事件（signal intermediate throwing event），抛出所定义信号的信号事件。

在Flowable中，信号会广播至所有的激活的处理器（也就是说，所有的信号捕获事件）。可以同步或异步地发布信号。

*
  - 在默认配置中，信号同步地传递。这意味着抛出信号的流程实例会等待，直到信号传递至所有的捕获信号的流程实例。所有的捕获流程实例也会在与抛出流程实例相同的事务中，也就是说如果收到通知的流程实例中，有一个实例产生了技术错误（抛出异常），则所有相关的实例都会失败。
*
  - 信号也可以异步地传递。这是由到达抛出信号事件时的发送处理器来决定的。对于每个激活的处理器，JobExecutor会为其存储并传递一个异步通知消息(asynchronous notification message),即作业（Job）。
* 图示： 消息抛出中间事件用内部有信号图标的标准中间事件（两层圆圈）表示。信号图标是黑色的（已填充），代表抛出的含义。![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/5/5/171e2b9d0a7ef1ad~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.awebp)
* xml表示： 信号中间事件与抛出中间事件一样定义。子元素为signalEventDefinition。

```
<intermediatethrowevent id="signal">
  <signaleventdefinition signalref="newCustomerSignal">
</signaleventdefinition></intermediatethrowevent>
```

异步信号事件这样定义：

```
<intermediatethrowevent id="signal">
  <signaleventdefinition signalref="newCustomerSignal" flowable:async="true">
</signaleventdefinition></intermediatethrowevent>
```

* 描述： 补偿抛出中间事件（compensation intermediate throwing event）用于触发补偿。

触发补偿：既可以为设计的活动触发补偿，也可以为补偿事件所在的范围触发补偿。补偿由活动所关联的补偿处理器执行。

*
  - 活动抛出补偿时，活动关联的补偿处理器将执行的次数，为活动成功完成的次数。
*
  - 抛出补偿时，当前范围中所有的活动，包括并行分支上的活动都会被补偿。
*
  - 补偿分层触发：如果将要被补偿的活动是一个子流程，则该子流程中所有的活动都会触发补偿。如果该子流程有嵌套的活动，则会递归地抛出补偿。然而，补偿不会传播至流程的上层：如果子流程中触发了补偿，该补偿不会传播至子流程范围外的活动。BPMN规范指出，对"与子流程在相同级别"的活动触发补偿。
*
  - 在Flowable中，补偿按照执行的相反顺序运行。这意味着最后完成的活动会第一个补偿。
*
  - 可以使用补偿抛出中间事件补偿已经成功完成的事务子流程。

>>> 如果抛出补偿的范围中有一个子流程，而该子流程包含有关联了补偿处理器的活动，则当抛出补偿时，只有该子流程成功完成时，补偿才会传播至该子流程。如果子流程内嵌套的部分活动已经完成，并附加了补偿处理器，但包含这些活动的子流程还没有完成，则这些补偿处理器仍不会执行。参考下面的例子：

在这个流程中，有两个并行的执行：一个执行嵌入子流程，另一个执行"charge credit card（信用卡付款）"活动。假定两个执行都已开始，且第一个执行正等待用户完成"review bookings（检查预定）"任务。第二个执行进行了"charge credit card（信用卡付款）"活动的操作，抛出了错误，导致"cancel reservations（取消预订）"事件触发补偿。这时并行子流程还未完成，意味着补偿不会传播至该子流程，因此不会执行"cancel hotel reservation（取消酒店预订）"补偿处理器。而如果"cancel reservations（取消预订）"运行前，这个用户任务（因此该嵌入式子流程也）已经完成，则补偿会传播至该嵌入式子流程。

流程变量：当补偿嵌入式子流程时，用于执行补偿处理器的执行，可以访问子流程的局部流程变量在子流程完成时的值。为此，会对范围执行（为执行子流程所创建的执行）所关联的流程变量进行快照。意味着：

*
  - 补偿执行器无法访问子流程范围内并行执行所添加的变量。
*
  - 上层执行所关联的流程变量（例如流程实例关联的流程变量）不在该快照中。因为补偿处理器可以直接访问这些流程变量在抛出补偿时的值。
*
  - 只会为嵌入式子流程进行变量快照。其他活动不会进行变量快照。

目前的限制：

*
  - 目前不支持waitForCompletion="false"。当补偿抛出中间事件触发补偿时，只有在补偿成功完成时，才会离开该事件。
*
  - 补偿由并行执行运行。并行执行会按照补偿活动完成的逆序启动。
*
  - 补偿不会传播至调用活动（call activity）生成的子流程。
* 图示： 补偿抛出中间事件用内部有补偿图标的标准中间事件（两层圆圈）表示。补偿图标是黑色的（已填充），代表抛出的含义。

* xml表示： 补偿中间事件与抛出中间事件一样定义。子元素为compensateEventDefinition。

```
<intermediatethrowevent id="throwCompensation">
    <compensateeventdefinition>
</compensateeventdefinition></intermediatethrowevent>
```

另外，activityRef可选项用于为指定的范围或活动触发补偿：

```
<intermediatethrowevent id="throwCompensation">
    <compensateeventdefinition activityref="bookHotel">
</compensateeventdefinition></intermediatethrowevent>
```

## 4.2、顺序流

* 描述： 顺序流（sequence flow）是流程中两个元素间的连接器。在流程执行过程中，一个元素被访问后，会沿着其所有出口顺序流继续执行。这意味着BPMN 2.0的默认是并行执行的：两个出口顺序流就会创建两个独立的、并行的执行路径。
* 图示： 顺序流，用从源元素指向目标元素的箭头表示。箭头总是指向目标元素。![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/5/5/171e2b9d48c1446d~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.awebp)
* xml表示： 顺序流需要有流程唯一的id，并引用存在的源与目标元素。

```
	<sequenceflow id="<span" class="hljs-string">"flow1" sourceRef=<span class="hljs-string">"theStart"</span> targetRef=<span class="hljs-string">"theTask"</span> />
</sequenceflow>
```

* 描述： 在顺序流上可以定义条件（conditional sequence flow）。当离开BPMN 2.0活动时，默认行为是计算其每个出口顺序流上的条件。当条件计算为true时，选择该出口顺序流。如果该方法选择了多条顺序流，则会生成多个执行，流程会以并行方式继续。

>>> 上面的介绍针对BPMN 2.0活动（与事件），但不适用于网关（gateway）。不同类型的网关，会用不同的方式处理带有条件的顺序流。

* 图示： 条件顺序流用起点带有小菱形的顺序流表示。在顺序流旁显示条件表达式。

* xml表示： 条件顺序流的XML表示格式为含有conditionExpression（条件表达式）子元素的普通顺序流。请注意目前只支持tFormalExpressions。可以省略xsi:type=""定义，默认为唯一支持的表达式类型。

```
<sequenceflow id="flow" sourceref="theStart" targetref="theTask">
  <conditionexpression xsi:type="tFormalExpression">
    <!--[CDATA[${order.price > 100 && order.price < 250}]]-->
  </conditionexpression>
</sequenceflow>
```

>>> 目前conditionalExpressions只能使用UEL。使用的表达式需要能解析为boolean值，否则当计算条件时会抛出异常。

* 下面的例子，通过典型的JavaBean的方式，使用getter引用流程变量的数据。

```
<conditionexpression xsi:type="tFormalExpression">
  <!--[CDATA[${order.price > 100 && order.price < 250}]]-->
</conditionexpression>
```

* 这个例子调用了一个解析为boolean值的方法。

```
<conditionexpression xsi:type="tFormalExpression">
  <!--[CDATA[${order.isStandardOrder()}]]-->
</conditionexpression>
```

Flowable发行版中包含了下列示例流程，用于展示值表达式与方法表达式的使用。

* 描述： 所有的BPMN 2.0任务与网关都可以使用默认顺序流（default sequence flow）。只有当没有其他顺序流可以选择时，才会选择默认顺序流作为活动的出口顺序流。流程会忽略默认顺序流上的条件。
* 图示： 默认顺序流用起点带有"斜线"标记的一般顺序流表示。

* XML表示： 活动的默认顺序流由该活动的default属性定义。下面的XML片段展示了一个排他网关（exclusive gateway），带有默认顺序流flow 2。只有当conditionA与conditionB都计算为false时，才会选择默认顺序流作为网关的出口顺序流。

```
<exclusivegateway id="exclusiveGw" name="Exclusive Gateway" default="flow2">

<sequenceflow id="flow1" sourceref="exclusiveGw" targetref="task1">
    <conditionexpression xsi:type="tFormalExpression">${conditionA}</conditionexpression>
</sequenceflow>

<sequenceflow id="flow2" sourceref="exclusiveGw" targetref="task2">

<sequenceflow id="flow3" sourceref="exclusiveGw" targetref="task3">
    <conditionexpression xsi:type="tFormalExpression">${conditionB}</conditionexpression>
</sequenceflow>
</sequenceflow></exclusivegateway>
```

对应下面的图示：

## 4.3、网关

网关（gateway）用于控制执行的流向（或者按BPMN 2.0的用词：执行的"标志（token）"）。网关可以消费（consuming）与生成（generating）标志。

网关用其中带有图标的菱形表示。该图标显示了网关的类型。

**这里出口顺序流的含义与BPMN 2.0中的一般情况不一样。一般情况下，会选择所有条件计算为true的顺序流，并行执行。而使用排他网关时，只会选择一条顺序流。当多条顺序流的条件都计算为true时，会且仅会选择在XML中最先定义的顺序流继续流程。如果没有可选的顺序流，会抛出异常。**

* 描述： 排他网关（exclusive gateway）（也叫异或网关 XOR gateway，或者更专业的，基于数据的排他网关 exclusive data-based gateway），用于对流程中的决策建模。当执行到达这个网关时，会按照所有出口顺序流定义的顺序对它们进行计算。选择第一个条件计算为true的顺序流（当没有设置条件时，认为顺序流为true）继续流程。
* 图示： 排他网关用内部带有'X'图标的标准网关（菱形）表示，'X'图标代表异或的含义。请注意内部没有图标的网关默认为排他网关。BPMN 2.0规范不允许在同一个流程中混合使用有及没有X的菱形标志。![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/5/5/171e2b9daad7f43b~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.awebp)
* xml表示： 排他网关的XML表示格式很简洁：一行定义网关的XML。条件表达式定义在其出口顺序流上。

以下面的模型为例：

其xml表示如下：

```
<exclusivegateway id="exclusiveGw" name="Exclusive Gateway">

<sequenceflow id="flow2" sourceref="exclusiveGw" targetref="theTask1">
  <conditionexpression xsi:type="tFormalExpression">${input == 1}</conditionexpression>
</sequenceflow>

<sequenceflow id="flow3" sourceref="exclusiveGw" targetref="theTask2">
  <conditionexpression xsi:type="tFormalExpression">${input == 2}</conditionexpression>
</sequenceflow>

<sequenceflow id="flow4" sourceref="exclusiveGw" targetref="theTask3">
  <conditionexpression xsi:type="tFormalExpression">${input == 3}</conditionexpression>
</sequenceflow>
</exclusivegateway>
```

* 描述： 网关也可以建模流程中的并行执行。在流程模型中引入并行的最简单的网关，就是并行网关（parallel gateway）。它可以将执行分支（fork）为多条路径，也可以合并（join）多条入口路径的执行。

并行网关的功能取决于其入口与出口顺序流：

*
  - 分支：所有的出口顺序流都并行执行，为每一条顺序流创建一个并行执行。
*
  - 合并：所有到达并行网关的并行执行都会在网关处等待，直到每一条入口顺序流都到达了有个执行。然后流程经过该合并网关继续。

>>> 如果并行网关同时具有多条入口与出口顺序流，可以同时具有分支与合并的行为。在这种情况下，网关首先合并所有入口顺序流，然后分裂为多条并行执行路径。

**与其他网关类型有一个重要区别：并行网关不计算条件。如果连接到并行网关的顺序流上定义了条件，会直接忽略该条件。**

* 图示： 并行网关，用内部带有'加号'图标的网关（菱形）表示，代表与（AND）的含义。

* xml表示： 定义并行网关只需要一行XML：

```
	<parallelgateway id="<span" class="hljs-string">"myParallelGateway" />
</parallelgateway>
```

实际行为（分支，合并或两者皆有），由连接到该并行网关的顺序流定义。

例如，上面的模型表示为下面的XML：

```
<startevent id="<span" class="hljs-string">"theStart" />
<sequenceflow id="<span" class="hljs-string">"flow1" sourceRef=<span class="hljs-string">"theStart"</span> targetRef=<span class="hljs-string">"fork"</span> />

<parallelgateway id="<span" class="hljs-string">"fork" />
<sequenceflow sourceref="<span" class="hljs-string">"fork" targetRef=<span class="hljs-string">"receivePayment"</span> />
<sequenceflow sourceref="<span" class="hljs-string">"fork" targetRef=<span class="hljs-string">"shipOrder"</span> />

<usertask id="<span" class="hljs-string">"receivePayment" name=<span class="hljs-string">"Receive Payment"</span> />
<sequenceflow sourceref="<span" class="hljs-string">"receivePayment" targetRef=<span class="hljs-string">"join"</span> />

<usertask id="<span" class="hljs-string">"shipOrder" name=<span class="hljs-string">"Ship Order"</span> />
<sequenceflow sourceref="<span" class="hljs-string">"shipOrder" targetRef=<span class="hljs-string">"join"</span> />

<parallelgateway id="<span" class="hljs-string">"join" />
<sequenceflow sourceref="<span" class="hljs-string">"join" targetRef=<span class="hljs-string">"archiveOrder"</span> />

<usertask id="<span" class="hljs-string">"archiveOrder" name=<span class="hljs-string">"Archive Order"</span> />
<sequenceflow sourceref="<span" class="hljs-string">"archiveOrder" targetRef=<span class="hljs-string">"theEnd"</span> />

<endevent id="<span" class="hljs-string">"theEnd" />
</endevent></sequenceflow></usertask></sequenceflow></parallelgateway></sequenceflow></usertask></sequenceflow></usertask></sequenceflow></sequenceflow></parallelgateway></sequenceflow></startevent>
```

在上面的例子中，当流程启动后会创建两个任务：

```
ProcessInstance pi = runtimeService.startProcessInstanceByKey(<span class="hljs-string">"forkJoin"</span>);
TaskQuery query = taskService.createTaskQuery()
    .processInstanceId(pi.getId())
    .orderByTaskName()
    .asc();

List<task> tasks = query.list();
assertEquals(<span class="hljs-number">2</span>, tasks.size());

Task task1 = tasks.get(<span class="hljs-number">0</span>);
assertEquals(<span class="hljs-string">"Receive Payment"</span>, task1.getName());
Task task2 = tasks.get(<span class="hljs-number">1</span>);
assertEquals(<span class="hljs-string">"Ship Order"</span>, task2.getName());
</task>
```

当这两个任务完成后，第二个并行网关会合并这两个执行。由于它只有一条出口顺序流，因此就不会再创建并行执行路径，而只是激活Archive Order(存档订单)任务。

并行网关不需要"平衡"（也就是说，前后对应的两个并行网关，其入口/出口顺序流的数量不需要一致）。每个并行网关都会简单地等待所有入口顺序流，并为每一条出口顺序流创建并行执行，而不受流程模型中的其他结构影响。因此，下面的流程在BPMN 2.0中是合法的：

* 描述： 可以把包容网关（inclusive gateway）看做排他网关与并行网关的组合。与排他网关一样，可以在包容网关的出口顺序流上定义条件，包容网关会计算条件。然而主要的区别是，包容网关与并行网关一样，可以同时选择多于一条出口顺序流。

包容网关的功能取决于其入口与出口顺序流：

*
  - 分支：流程会计算所有出口顺序流的条件。对于每一条计算为true的顺序流，流程都会创建一个并行执行。
*
  - 合并：所有到达包容网关的并行执行，都会在网关处等待。直到每一条具有流程标志（process token）的入口顺序流，都有一个执行到达。这是与并行网关的重要区别。换句话说，**包容网关只会等待可以被执行的入口顺序流。**在合并后，流程穿过合并并行网关继续。

>>> 如果包容网关同时具有多条入口与出口顺序流，可以同时具有分支与合并的行为。在这种情况下，网关首先合并所有具有流程标志的入口顺序流，然后为每一个条件计算为true的出口顺序流分裂出并行执行路径。

包容网关的汇聚行为比并行网关更复杂。所有到达包容网关的并行执行，都会在网关等待，直到所有"可以到达"包容网关的执行都"到达"包容网关。 判断方法为：计算当前流程实例中的所有执行，检查从其位置是否有一条到达包容网关的路径（忽略顺序流上的任何条件）。如果存在这样的执行（可到达但尚未到达），则不会触发包容网关的汇聚行为。

* xml表示： 定义包容网关需要一行XML：

```
<inclusivegateway id="<span" class="hljs-string">"myInclusiveGateway" />
</inclusivegateway>
```

实际行为（分支，合并或两者皆有），由连接到该包容网关的顺序流定义。

例如，上面的模型表现为下面的XML：

```
<startevent id="theStart">
<sequenceflow id="flow1" sourceref="theStart" targetref="fork">

<inclusivegateway id="fork">
<sequenceflow sourceref="fork" targetref="receivePayment">
  <conditionexpression xsi:type="tFormalExpression">${paymentReceived == false}</conditionexpression>
</sequenceflow>
<sequenceflow sourceref="fork" targetref="shipOrder">
  <conditionexpression xsi:type="tFormalExpression">${shipOrder == true}</conditionexpression>
</sequenceflow>

<usertask id="receivePayment" name="Receive Payment">
<sequenceflow sourceref="receivePayment" targetref="join">

<usertask id="shipOrder" name="Ship Order">
<sequenceflow sourceref="shipOrder" targetref="join">

<inclusivegateway id="join">
<sequenceflow sourceref="join" targetref="archiveOrder">

<usertask id="archiveOrder" name="Archive Order">
<sequenceflow sourceref="archiveOrder" targetref="theEnd">

<endevent id="theEnd">
</endevent></sequenceflow></usertask></sequenceflow></inclusivegateway></sequenceflow></usertask></sequenceflow></usertask></inclusivegateway></sequenceflow></startevent>
```

在上面的例子中，当流程启动后，如果流程变量paymentReceived == false且shipOrder == true，会创建两个任务。如果只有一个流程变量等于true，则只会创建一个任务。如果没有条件计算为true，会抛出异常（可通过指定默出口顺序流避免）。在下面的例子中，只会创建ship order（传递订单）一个任务：

```
HashMap<string, object> variableMap = <span class="hljs-keyword">new</span> HashMap<string, object>();
variableMap.put(<span class="hljs-string">"receivedPayment"</span>, <span class="hljs-keyword">true</span>);
variableMap.put(<span class="hljs-string">"shipOrder"</span>, <span class="hljs-keyword">true</span>);

ProcessInstance pi = runtimeService.startProcessInstanceByKey(<span class="hljs-string">"forkJoin"</span>);

TaskQuery query = taskService.createTaskQuery()
    .processInstanceId(pi.getId())
    .orderByTaskName()
    .asc();

List<task> tasks = query.list();
assertEquals(<span class="hljs-number">1</span>, tasks.size());

Task task = tasks.get(<span class="hljs-number">0</span>);
assertEquals(<span class="hljs-string">"Ship Order"</span>, task.getName());
</task></string,></string,>
```

当这个任务完成后，第二个包容网关会合并这两个执行。并且由于它只有一条出口顺序流，所有不会再创建并行执行路径，而只会激活Archive Order(存档订单)任务。

>>> 包容网关不需要"平衡"（也就是说，对应的包容网关，其入口/出口顺序流的数量不需要匹配）。包容网关会简单地等待所有入口顺序流，并为每一条出口顺序流创建并行执行，不受流程模型中的其他结构影响。

>>> 包容网关不需要"平衡"（也就是说，前后对应的两个包容网关，其入口/出口顺序流的数量不需要一致）。每个包容网关都会简单地等待所有入口顺序流，并为每一条出口顺序流创建并行执行，不受流程模型中的其他结构影响。

* 描述： 基于事件的网关（event-based gateway）提供了根据事件做选择的方式。网关的每一条出口顺序流都需要连接至一个捕获中间事件。当流程执行到达基于事件的网关时，与等待状态类似，网关会暂停执行，并且为每一条出口顺序流创建一个事件订阅。

>>> 基于事件的网关的出口顺序流与一般的顺序流不同。这些顺序流从不实际执行。相反，它们用于告知流程引擎：当执行到达一个基于事件的网关时，需要订阅什么事件。有以下限制：

*
  - 一个基于事件的网关，必须有两条或更多的出口顺序流。
*
  - 基于事件的网关，只能连接至intermediateCatchEvent（捕获中间事件）类型的元素（Flowable不支持在基于事件的网关之后连接"接收任务 Receive Task"）。
*
  - 连接至基于事件的网关的intermediateCatchEvent，必须只有一个入口顺序流。
* 图示： 基于事件的网关，用内部带有特殊图标的网关（菱形）表示。

* xml表示： 用于定义基于事件的网关的XML元素为eventBasedGateway。
* 示例： 下面是一个带有基于事件的网关的示例流程。当执行到达基于事件的网关时，流程执行暂停。流程实例订阅alert信号事件，并创建一个10分钟后触发的定时器。流程引擎会等待10分钟，并同时等待信号事件。如果信号在10分钟内触发，则会取消定时器，流程沿着信号继续执行，激活Handle alert用户任务。如果10分钟内没有触发信号，则会继续执行，并取消信号订阅。

```
<definitions id="definitions" xmlns="http://www.omg.org/spec/BPMN/20100524/MODEL" xmlns:flowable="http://flowable.org/bpmn" targetnamespace="Examples">

    <signal id="alertSignal" name="alert">

    <process id="catchSignal">

        <startevent id="start">

        <sequenceflow sourceref="start" targetref="gw1">

        <eventbasedgateway id="gw1">

        <sequenceflow sourceref="gw1" targetref="signalEvent">
        <sequenceflow sourceref="gw1" targetref="timerEvent">

        <intermediatecatchevent id="signalEvent" name="Alert">
            <signaleventdefinition signalref="alertSignal">
        </signaleventdefinition></intermediatecatchevent>

        <intermediatecatchevent id="timerEvent" name="Alert">
            <timereventdefinition>
                <timeduration>PT10M</timeduration>
            </timereventdefinition>
        </intermediatecatchevent>

        <sequenceflow sourceref="timerEvent" targetref="exGw1">
        <sequenceflow sourceref="signalEvent" targetref="task">

        <usertask id="task" name="Handle alert">

        <exclusivegateway id="exGw1">

        <sequenceflow sourceref="task" targetref="exGw1">
        <sequenceflow sourceref="exGw1" targetref="end">

        <endevent id="end">
    </endevent></sequenceflow></sequenceflow></exclusivegateway></usertask></sequenceflow></sequenceflow></sequenceflow></sequenceflow></eventbasedgateway></sequenceflow></startevent></process>
</signal></definitions>
```

## 4.4、任务

一个任务表示工作需要被外部实体完成， 比如人工或自动服务。

任务被描绘成一个圆角矩形，一般内部包含文字。 任务的类型（用户任务，服务任务，脚本任务，等等）显示在矩形的左上角，用小图标区别。 根据任务的类型， 引擎会执行不同的功能。

* 描述： "用户任务（user task）"，也叫人工任务，见名知意，是用于对需要人工执行的任务进行建模。当流程执行到达用户任务时，会为指派至该任务的用户或组的任务列表创建一个新任务。
* 图示： 用户任务用左上角有一个小用户图标的标准任务（圆角矩形）表示。![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/5/5/171e2b9ded6afa07~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.awebp)
* xml表示： 用户任务在XML中如下定义。其中id是必须属性，name是可选属性。

```
	<usertask id="<span" class="hljs-string">"theTask" name=<span class="hljs-string">"Important task"</span> />
</usertask>
```

也可以为用户任务添加描述（description）。事实上任何BPMN 2.0元素都可以有描述。描述由documentation元素定义。

```
<usertask id="theTask" name="Schedule meeting">
  <documentation>
      Schedule an engineering meeting for next week with the new hire.

  </documentation>
</usertask>
```

* 到期日期 每个任务都可以使用一个字段标志该任务的到期日期（due date）。可以使用查询API，查询在给定日期前或后到期的任务。 可以在任务定义中使用扩展指定表达式，以在任务创建时设定到期日期。该表达式必须解析为java.util.Date，java.util.String (ISO8601格式)，ISO8601时间长度（例如PT50M），或者null。例如，可以使用在流程里前一个表单中输入的日期，或者由前一个服务任务计算出的日期。如果使用的是时间长度，则到期日期基于当前时间加上给定长度计算。例如当dueDate使用"PT30M"时，任务在从现在起30分钟后到期。

```
<usertask id="<span" class="hljs-string">"theTask" name=<span class="hljs-string">"Important task"</span> flowable:dueDate=<span class="hljs-string">"${dateVariable}"</span>/>
</usertask>
```

任务的到期日期也可以使用TaskService，或者在TaskListener中使用传递的DelegateTask修改。

* 用户指派： 用户任务可以直接指派（assign）给用户。可以定义humanPerformer子元素来实现。humanPerformer需要resourceAssignmentExpression来实际定义用户。目前，只支持formalExpressions。

```
<process>

  ...

  <usertask id="theTask" name="important task">
    <humanperformer>
      <resourceassignmentexpression>
        <formalexpression>kermit</formalexpression>
      </resourceassignmentexpression>
    </humanperformer>
  </usertask>
</process>
```

只能指定一个用户作为任务的humanPerformer。在Flowable术语中，这个用户被称作办理人（assignee）。拥有办理人的任务，在其他人的任务列表中不可见，而只能在该办理人的个人任务列表中看到。

可以通过TaskService获取特定用户办理的任务：

```
	List<task> tasks = taskService.createTaskQuery().taskAssignee(<span class="hljs-string">"kermit"</span>).list();
</task>
```

任务也可以放在用户的候选任务列表中。在这个情况下，需要使用potentialOwner（潜在用户）结构。用法与humanPerformer结构类似。请注意需要指定表达式中的每一个元素为用户还是组（引擎无法自行判断）。

```
<process>

  ...

  <usertask id="theTask" name="important task">
    <potentialowner>
      <resourceassignmentexpression>
        <formalexpression>user(kermit), group(management)</formalexpression>
      </resourceassignmentexpression>
    </potentialowner>
  </usertask>
</process>
```

可用如下方法获取定义了potentialOwner结构的任务：

```
 List<task> tasks = taskService.createTaskQuery().taskCandidateUser(<span class="hljs-string">"kermit"</span>);
</task>
```

将获取所有kermit作为候选用户的任务，也就是说，表达式含有user(kermit)的任务。同时也将获取所有指派给kermit为其成员的组的任务（例如，kermit时management组的成员，且任务指派给management组）。组在运行时解析，并可通过身份服务管理。

如果并未指定给定字符串是用户还是组，引擎默认其为组。下列代码与声明group(accountancy)效果一样。

```
<formalexpression>accountancy</formalexpression>
```

* 用于任务指派的Flowable扩展 很明显，当指派关系不复杂时，这种用户与组的指派方式十分笨重。为避免这种复杂性，可以在用户任务上使用自定义扩展。
*
  - assignee（办理人）属性：这个自定义扩展用于直接将用户指派至用户任务。

```
<usertask id="<span" class="hljs-string">"theTask" name=<span class="hljs-string">"my task"</span> flowable:assignee=<span class="hljs-string">"kermit"</span> />
</usertask>
```

与上面定义的humanPerformer结构效果完全相同。

*
  - candidateUsers（候选用户）属性：这个自定义扩展用于为任务指定候选用户。

```
<usertask id="<span" class="hljs-string">"theTask" name=<span class="hljs-string">"my task"</span> flowable:candidateUsers=<span class="hljs-string">"kermit, gonzo"</span> />
</usertask>
```

与使用上面定义的potentialOwner结构效果完全相同。请注意不需要像在potentialOwner中一样，使用user(kermit)的声明，因为这个属性只能用于用户。

*
  - candidateGroups（候选组）attribute：这个自定义扩展用于为任务指定候选组。

```
	<usertask id="<span" class="hljs-string">"theTask" name=<span class="hljs-string">"my task"</span> flowable:candidateGroups=<span class="hljs-string">"management, accountancy"</span> />
</usertask>
```

与使用上面定义的potentialOwner结构效果完全相同。请注意不需要像在potentialOwner中一样，使用group(management)的声明，因为这个属性只能用于组。

*
  - 可以定义在一个用户任务上同时定义candidateUsers与candidateGroups。

* 描述： 脚本任务（Script Task）是一个自动化任务。当流程到达脚本任务时，自动执行编写的脚本，完毕后继续执行后继路线。
* 图示： 脚本任务用左上角有一个小"脚本"图标的标准BPMN 2.0任务（圆角矩形）表示。![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/5/5/171e2b9e00c82801~tplv-t2oaga2asx-jj-mark:3024:0:0:0:q75.awebp)
* xml表示： 脚本任务使用script与scriptFormat元素定义。

```
<scripttask id="theScriptTask" name="Execute script" scriptformat="groovy">
  <script>
    sum = 0
    for ( i in inputArray ) {
      sum += i
    }
  </script>
</scripttask>
```

scriptFormat属性的值，必须是兼容JSR-223（Java平台脚本）的名字。默认情况下，JavaScript包含在每一个JDK中，因此不需要添加任何JAR文件。如果想使用其它（兼容JSR-223的）脚本引擎，则需要在classpath中添加相应的jar，并使用适当的名字。例如，Flowable单元测试经常使用Groovy，因为其语法与Java十分相似。

请注意Groovy脚本引擎与groovy-all JAR捆绑在一起。在Groovy 2.0版本以前，脚本引擎是Groovy JAR的一部分。因此，必须添加如下依赖：

```
<dependency>
    <groupid>org.codehaus.groovy</groupid>
    <artifactid>groovy-all</artifactid>
    <version>2.x.x<version>
</version></version></dependency>
```

* 脚本中的变量： 到达脚本引擎的执行中，所有的流程变量都可以在脚本中使用。在这个例子里，脚本变量'inputArray'实际上就是一个流程变量（一个integer的数组）。

```
<script>
    sum = 0
    for ( i in inputArray ) {
      sum += i
    }
</script>
```

也可以简单地调用execution.setVariable("variableName", variableValue)，在脚本中设置流程变量。默认情况下，变量不会自动储存（请注意，在一些早期版本中是会储存的！）。可以将scriptTask的autoStoreVariables参数设置为true，以自动保存任何在脚本中定义的变量（例如上例中的sum）。

```
	<scripttask id="<span" class="hljs-string">"script" scriptFormat=<span class="hljs-string">"JavaScript"</span> flowable:autoStoreVariables=<span class="hljs-string">"false"</span>>
</scripttask>
```

这个参数的默认值为false。也就是说如果在脚本任务定义中忽略这个参数，则脚本声明的所有变量将只在脚本执行期间有效。

在脚本中设置变量的例子：

```
<script>
    def scriptVar = "test123"
    execution.setVariable("myVar", scriptVar)
</script>
```

>>> 下列名字是保留字，不能用于变量名：out，out:print，lang:import，context，elcontext。

* 脚本任务的结果: 脚本任务的返回值，可以通过为脚本任务定义的'flowable:resultVariable'属性设置为流程变量。可以是已经存在的，或者新的流程变量。如果指定为已存在的流程变量，则流程变量的值会被脚本执行的结果值覆盖。如果不指定结果变量名，则脚本结果值将被忽略。

```
<scripttask id="theScriptTask" name="Execute script" scriptformat="juel" flowable:resultvariable="myVar">
  <script>#{echo}</script>
</scripttask>
```

在上面的例子中，脚本执行的结果（解析表达式'#{echo}'的值），将在脚本完成后，设置为名为'myVar'的流程变量。

服务任务（Service Task）是一个自动化任务。当流程到达系统任务时，它会调用一些服务（例如web service，java service等等），完毕后继续执行后继路线。

* 描述： Java服务任务（Java service task）用于调用Java类。
* 图示： 服务任务用左上角有一个小齿轮图标的圆角矩形表示。

* xml表示：

有四种方法声明如何调用Java逻辑：

*
  - 指定实现了JavaDelegate或ActivityBehavior的类
*
  - 调用解析为委托对象（delegation object）的表达式
*
  - 调用方法表达式（method expression）
*
  - 对值表达式（value expression）求值

使用flowable:class属性提供全限定类名（fully qualified classname），指定流程执行时调用的类。

```
<servicetask id="<span" class="hljs-string">"javaService"
             name=<span class="hljs-string">"My Java Service Task"</span>
             flowable:<span class="hljs-class"><span class="hljs-keyword">class</span></span>=<span class="hljs-string">"org.flowable.MyJavaDelegate"</span> />
</servicetask>
```

也可以使用解析为对象的表达式。该对象必须遵循的规则，与使用flowable:class创建的对象规则相同。

```
<servicetask id="<span" class="hljs-string">"serviceTask" flowable:delegateExpression=<span class="hljs-string">"${delegateExpressionBean}"</span> />
</servicetask>
```

delegateExpressionBean是一个实现了JavaDelegate接口的bean，定义在Spring容器中。

使用flowable:expression属性指定需要计算的UEL方法表达式。

```
<servicetask id="<span" class="hljs-string">"javaService"
             name=<span class="hljs-string">"My Java Service Task"</span>
             flowable:expression=<span class="hljs-string">"#{printer.printMessage()}"</span> />
</servicetask>
```

将在名为printer的对象上调用printMessage方法（不带参数）。

也可以为表达式中使用的方法传递变量。

```
<servicetask id="<span" class="hljs-string">"javaService"
             name=<span class="hljs-string">"My Java Service Task"</span>
             flowable:expression=<span class="hljs-string">"#{printer.printMessage(execution, myVar)}"</span> />
</servicetask>
```

将在名为printer的对象上调用printMessage方法。传递的第一个参数为DelegateExecution，名为execution，在表达式上下文中默认可用。传递的第二个参数，是当前执行中，名为myVar变量的值。

可以使用flowable:expression属性指定需要计算的UEL值表达式。

```
<servicetask id="<span" class="hljs-string">"javaService"
             name=<span class="hljs-string">"My Java Service Task"</span>
             flowable:expression=<span class="hljs-string">"#{split.ready}"</span> />
</servicetask>
```

会调用名为split的bean的ready参数的getter方法，getReady（不带参数）。该对象会被解析为执行的流程变量或（如果可用的话）Spring上下文中的bean。

* 实现：

要实现可以在流程执行中调用的类，需要实现org.flowable.engine.delegate.JavaDelegate接口，并在execute方法中提供所需逻辑。当流程执行到达该活动时，会执行方法中定义的逻辑，并按照BPMN 2.0的默认方法离开活动。

下面是一个Java类的示例，用于将流程变量String改为大写。这个类需要实现org.flowable.engine.delegate.JavaDelegate接口，因此需要实现execute(DelegateExecution)方法。这个方法就是引擎将调用的方法，需要实现业务逻辑。可以通过DelegateExecution接口（点击链接获取该接口操作的详细Javadoc）访问流程实例信息，如流程变量等。

```
<span class="hljs-keyword">public</span> <span class="hljs-class"><span class="hljs-keyword">class</span> <span class="hljs-title">ToUppercase</span> <span class="hljs-keyword">implements</span> <span class="hljs-title">JavaDelegate</span> </span>{

  <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">void</span> <span class="hljs-title">execute</span><span class="hljs-params">(DelegateExecution execution)</span> </span>{
    String var = (String) execution.getVariable(<span class="hljs-string">"input"</span>);
    var = var.toUpperCase();
    execution.setVariable(<span class="hljs-string">"input"</span>, var);
  }

}
```

* 服务任务的结果： 服务执行的返回值（仅对使用表达式的服务任务），可以通过为服务任务定义的'flowable:resultVariable'属性设置为流程变量。可以是已经存在的，或者新的流程变量。 如果指定为已存在的流程变量，则流程变量的值会被服务执行的结果值覆盖。 如果使用'flowable:useLocalScopeForResultVariable'，则会将结果值设置为局部变量。 如果不指定结果变量名，则服务任务的结果值将被忽略。

```
<servicetask id="<span" class="hljs-string">"aMethodExpressionServiceTask"
    flowable:expression=<span class="hljs-string">"#{myService.doSomething()}"</span>
    flowable:resultVariable=<span class="hljs-string">"myVar"</span> />
</servicetask>
```

* 描述： Web服务任务（Web service task）用于同步地调用外部的Web服务。
* 图示： Web服务任务与Java服务任务图标一样。

* xml表示： 使用Web服务之前，需要导入其操作及复杂的类型。可以使用导入标签（import tag）指向Web服务的WSDL，自动处理：

```
<<span class="hljs-keyword">import</span> importType=<span class="hljs-string">"http://schemas.xmlsoap.org/wsdl/"</span>
	location=<span class="hljs-string">"http://localhost:63081/counter?wsdl"</span>
	namespace=<span class="hljs-string">"http://webservice.flowable.org/"</span> />
```

按照上面的声明，Flowable会导入定义，但不会创建条目定义（item definition）与消息。如果需要调用一个名为'prettyPrint'的方法，则需要先为请求及回复消息创建对应的消息与条目定义：

```
<message id="<span" class="hljs-string">"prettyPrintCountRequestMessage" itemRef=<span class="hljs-string">"tns:prettyPrintCountRequestItem"</span> />
<message id="<span" class="hljs-string">"prettyPrintCountResponseMessage" itemRef=<span class="hljs-string">"tns:prettyPrintCountResponseItem"</span> />

<itemdefinition id="<span" class="hljs-string">"prettyPrintCountRequestItem" structureRef=<span class="hljs-string">"counter:prettyPrintCount"</span> />
<itemdefinition id="<span" class="hljs-string">"prettyPrintCountResponseItem" structureRef=<span class="hljs-string">"counter:prettyPrintCountResponse"</span> />
</itemdefinition></itemdefinition></message></message>
```

在声明服务任务前，需要定义实际引用Web服务的BPMN接口与操作。基本上，是定义"接口"与所需的"操作"。对每一个操作都可以重复使用之前定义的"传入"与"传出"消息。例如，下面的声明定义了"counter"接口及"prettyPrintCountOperation"操作：

```
<interface name="Counter Interface" implementationref="counter:Counter">
	<operation id="prettyPrintCountOperation" name="prettyPrintCount Operation" implementationref="counter:prettyPrintCount">
		<inmessageref>tns:prettyPrintCountRequestMessage</inmessageref>
		<outmessageref>tns:prettyPrintCountResponseMessage</outmessageref>
	</operation>
</interface>
```

这样就可以使用##WebService实现，声明Web服务任务，并引用Web服务操作。

```
<servicetask id="<span" class="hljs-string">"webService"
	name=<span class="hljs-string">"Web service invocation"</span>
	implementation=<span class="hljs-string">"##WebService"</span>
	operationRef=<span class="hljs-string">"tns:prettyPrintCountOperation"</span>>
</servicetask>
```

**参考：**
