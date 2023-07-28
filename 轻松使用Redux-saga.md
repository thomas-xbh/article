---
link: https://zhuanlan.zhihu.com/p/114409848
title: 轻松使用Redux-saga
description: 背景近期在维护老版react项目（生命周期），看到油管对saga的评价不错，就动手把redux-thunk替换成了redux-saga。在使用了redux之后，我们对middleware也有了长足的了解。本文将针对以下几个点展开探讨。 1、redux…
keywords: React,前端开发,前端入门
author: 大虎​​网易 高级前端开发工程师
date: 2022-06-26T08:08:00.000Z
publisher: 知乎专栏
stats: paragraph=65 sentences=5, words=40
---
近期在维护老版react项目（生命周期），看到油管对saga的评价不错，就动手把redux-thunk替换成了redux-saga。在使用了redux之后，我们对middleware也有了长足的了解。本文将针对以下几个点展开探讨。

**1、redux-thunk与redux-saga的比较**

**2、saga的技术细节**

**3、saga的实现过程**

**扫码加微信前端二、三群（一群已满）**：BAT大厂资深大牛定期推送面经与源码分析，各平台大牛优秀文章推荐，更有内推跳槽咨询、视频资源共享、学习资料文章pdf面经网盘资源等等福利。加入我们一起进步。

## **redux-thunk与redux-saga的比较**

redux-thunk处理副作用的一些问题

redux中的数据流：

**_UI—————>action（plain）—————>reducer——————>state——————>UI_**

在数据流中，action是一个原始js对象（plain object）且reducer是一个纯函数，对于同步且没有副作用的操作，上述的数据流起到可以管理数据，从而控制视图层更新的目的。

**但是如果存在副作用，比如axios异步请求等等，那么应该怎么做？**

如果存在副作用函数，那么我们需要首先处理副作用函数，然后生成原始的js对象。如何处理副作用操作，在redux中选择在发出action，到reducer处理函数之间使用中间件处理副作用。

redux增加中间件处理副作用后的数据流大致如下：

**_UI——>action(side function)—>middleware—>action(plain)—>reducer—>state—>UI_**

副作用的action和原始的action之间增加中间件处理，中间件的作用就是：

**转换异步操作，生成原始的action，这样，reducer函数就能处理相应的action，从而改变state，更新UI。**

在redux中，thunk是redux作者给出的中间件，实现极为简单：

这几行代码做的事情也很简单，判别action的类型，如果action是函数，就调用这个函数，调用的步骤为：

发现实参为dispatch和getState，因此我们在定义action为thunk函数是，一般形参为dispatch和getState。

## **1、redux-thunk的缺点**

thunk的缺点也是很明显的，thunk仅仅做了执行这个函数，并不在乎函数主体内是什么，也就是说thunk使
得redux可以接受函数作为action，但是函数的内部可以多种多样。比如下面是一个获取商品列表的异步操作所对应的action：

从这个具有副作用的action中，我们可以看出，函数内部极为复杂。如果需要为每一个异步操作都如此定义一个action，显然action不易维护。

action不易维护的原因：

* action的形式不统一
* 就是异步操作太为分散，分散在了各个action中

## **2、redux-saga技术细节**

（1）声明Effect（api）

redux-saga中提供了一系列的api，且最大特点是提供了声明式的Effect，声明式的Effect使得redux-saga监听原始js对象形式的action，并且可以方便单元测试。

redux-saga中的api有take、put、all、select这些，在redux-saga中将这些api都定义为Effect。在Effect执行后，当函数resolved时返回一个描述对象，然后saga根据这个描述对象恢复执行generator中的函数。

**redux-thunk的大体过程：**

**_action1(side function)—>redux-thunk监听—>执行相应的有副作用的方法—>action2(plain object)_**

在action2中的是js对象形式的action，然后执行reducer就会更新store中的state。

**redux-saga的大体过程：**

**_action1(plain object)——>redux-saga监听—>执行相应的Effect方法——>返回描述对象—>恢复执行异步和副作用函数—>action2(plain object)_**

对比redux-thunk我们发现，redux-saga中监听到了原始js对象action，并不会马上执行副作用操作，会先通过Effect方法将其转化成一个描述对象，然后再将描述对象，作为标识，再恢复执行副作用函数。

通过使用Effect类函数，可以方便单元测试，我们不需要测试副作用函数的返回结果。只需要比较执行Effect方法后返回的描述对象，与我们所期望的描述对象是否相同即可。

举例来说，call方法是一个Effect类方法：

上述代码中，比如我们需要测试Api.fetch返回的结果是否符合预期，通过调用call方法，返回一个描述对象。这个描述对象包含了所需要调用的方法和执行方法时的实际参数，我们认为只要描述对象相同，也就是说只要调用的方法和执行该方法时的实际参数相同，就认为最后执行的结果肯定是满足预期的，这样可以方便的进行单元测试，不需要模拟Api.fetch函数的具体返回结果。

（2）Effect的各种方法

从低阶的API，比如take，call(apply)，fork，put，select等，到高阶API，比如takeEvery和takeLatest等，从而加深对redux-saga用法的认识。

**take**

take这个方法，是用来监听action，返回的是监听到的action对象。比如：

在UI Component中dispatch一个action:

在saga中使用：

可以监听到UI传递到中间件的Action,上述take方法的返回，就是dispatch的原始对象。一旦监听到login动作，返回的action为：

**call(apply)主要用于异步请求**

call和apply方法与js中的call和apply相似，我们以call方法为例：

call方法调用fn，参数为args，返回一个描述对象。不过这里call方法传入的函数fn可以是普通函数，也可以是generator。call方法应用很广泛，在redux-saga中使用异步请求等常用call方法来实现。


**put**

在前面提到，redux-saga做为中间件，工作流是这样的：

**_UI——>action1————>redux-saga中间件————>action2————>reducer.._**

从工作流中，我们发现redux-saga执行完副作用函数后，必须发出action，然后这个action被reducer监听，从而达到更新state的目的。相应的这里的put对应与redux中的dispatch，工作流程图如下：

从图中可以看出redux-saga执行副作用方法转化action时，put这个Effect方法跟redux原始的dispatch相似，都是可以发出action，且发出的action都会被reducer监听到。put的使用方法：

* **select*

put方法与redux中的dispatch相对应，同样的如果我们想在中间件中获取state，那么需要使用select。select方法对应的是redux中的getState，用户获取store中的state，使用方法：

* **fork*

fork方法相当于web work，fork方法不会阻塞主线程，在非阻塞调用中十分有用。

* **takeEvery和takeLatest*

takeEvery和takeLatest用于监听相应的动作并执行相应的方法，是构建在take和fork上面的高阶api，比如要监听login动作，好用takeEvery方法可以：

takeEvery监听到login的动作，就会执行loginFunc方法，除此之外，takeEvery可以同时监听到多个相同的action。

takeLatest方法跟takeEvery是相同方式调用：

与takeEvery不同的是，takeLatest是会监听执行最近的那个被触发的action。

## 3、redux-saga应用实例

在reducer中实现两个方法获取相应的数据，运用到了多种effect。

而我们在需要注入数据的组件中去dispatch相应的action即可

Happy Hacking~~
