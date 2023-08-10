---
link: https://blog.csdn.net/sunyctf/article/details/124616674
title: Express框架中res.write、res.end及res.send 、res.json方法之间的区别？
description: Express框架中res.write、res.end及res.send 、res.json方法之间的区别？泣血整理。Express响应中常用的四种API：res.write() | res.end() | res.send() | res.json()，这4个API方法，都可以发送HTTP响应，返回浏览器的请求数据，不过用法之间各有区别_res.send
keywords: res.send
author: 儒雅的烤地瓜 Csdn认证博客专家 Csdn认证企业博客 码龄4年 暂无认证
date: 2022-05-07T06:42:01.000Z
publisher: null
stats: paragraph=115 sentences=173, words=632
---
**目录**

[写在前面：](#%e5%86%99%e5%9c%a8%e5%89%8d%e9%9d%a2%ef%bc%9a)

[好的，我们开始 👇 👇 👇](#%e5%a5%bd%e7%9a%84%ef%bc%8c%e6%88%91%e4%bb%ac%e5%bc%80%e5%a7%8b%20%f0%9f%91%87%c2%a0%20%c2%a0%20%c2%a0%20%c2%a0%20%c2%a0%20%c2%a0%20%c2%a0%20%c2%a0%20%c2%a0%20%c2%a0%20%c2%a0%20%c2%a0%20%c2%a0%20%f0%9f%91%87%c2%a0%20%c2%a0%20%c2%a0%20%c2%a0%20%c2%a0%20%c2%a0%20%c2%a0%20%c2%a0%20%c2%a0%20%c2%a0%20%c2%a0%20%c2%a0%20%c2%a0%20%f0%9f%91%87)

[🏝️ 一. res.write()方法](#%f0%9f%8f%9d%ef%b8%8f%20%c2%a0%e4%b8%80%20reswrite%28%29%e6%96%b9%e6%b3%95%c2%a0)

[🏝️ 二. res.end方法](#%c2%a0%f0%9f%8f%9d%ef%b8%8f%c2%a0%c2%a0%e4%ba%8c%20resend%e6%96%b9%e6%b3%95)

[🏝️ 三. res.send()方法](#%f0%9f%8f%9d%ef%b8%8f%c2%a0%c2%a0%e4%b8%89%20ressend%28%29%e6%96%b9%e6%b3%95%c2%a0%20%c2%a0)

[🏝️ 四. res.json()方法](#%f0%9f%8f%9d%ef%b8%8f%c2%a0%20%e5%9b%9b%20resjson%28%29%e6%96%b9%e6%b3%95)

[♻️ 4种API的简单总结](#%e2%99%bb%ef%b8%8f%c2%a0%20%e6%80%bb%20%e7%bb%93)

# 写在前面：

😇本文为综合资料查询及自己作为小白的粗浅理解整理而成，如有错误敬请评论斧正😇

# 好的，我们开始 👇 👇 👇

开局举例：在已下载好express包（[如何下载](https://blog.csdn.net/sunyctf/article/details/124485263 "如何下载")）的Demo文件夹里新建服务器文件app.js 、路由器文件product.js（咱们直接在路由器里写路由（[为何推荐在路由器模块中集中写该模块下的所有路由](https://blog.csdn.net/sunyctf/article/details/124575101 "为何推荐在路由器模块中集中写该模块下的所有路由")），当然你也可以在[服务器里文件里写路由](https://blog.csdn.net/sunyctf/article/details/124575101 "服务器里文件里写路由")以图方便，不过不推荐）

> ◼️ **如何下载各种nodejs包：**
npm install 包的名称 回车 如：npm install mysql
会将下载的包放入到node_modules中，如果node_modules 目录不存在会自动创建，同时会生成文件package-lock.json，用于记录包的版本号

> ◼️ **为何要用路由器：**
项目开发中，可能出现不同模块下相同的URL，为了团队协作，独立出每个功能模块，使用路由器将当前模块下所有的路由放到一起，并给URL添加前缀，最后挂载到web服务器下

![](https://img-blog.csdnimg.cn/485bf3e708e74857a4a1eae0b0603ab7.png)

*服务器文件app.js

```js
//引入express包
const express=require('express');
//引入路由器模块
const productRouter=require('./product.js');
//console.log(productRouter);
//创建web服务器
const app=express();
//设置端口
app.listen(8080);
//在web服务器下挂载，同时添加前缀 /product
app.use('/product',productRouter);
```

首先，Express响应中常用的四种API：res.write() | res.end() | res.send() | res.json()，这4个API方法，都可以 **发送HTTP响应**，返回浏览器的请求数据

## 🏝️ **一. res.write()方法**

*路由器文件product.js

```js
//引入express包
const express = require('express');
//创建路由器对象
const r = express.Router();
//往路由器中添加路由
//商品列表路由：get  /list
r.get('/list', (req, res) => {
  // 在响应头信息里设置响应返回的内容类型为html，编码为utf-8(在浏览器页面正常显示中文)
  // 设置内容解析的编码为utf-8，正确地告诉浏览器，服务器响应的内容是什么编码的，你浏览器应该按照我服务器设定的编码格式来解析给你的内容
  // res.writeHead(200, {
  //   'Content-Type': 'text/html;charset=utf-8'
  // });
  let show = "888";
  res.write(show);
  res.write('商品列表');
  res.end();
  //res.end('该方法用于结束响应的浏览器请求');
});

//导出路由器对象
module.exports = r;
```

👉启动服务器：

在Demo文件夹里打开命令行窗口，运行命令：node app.js

👉浏览器向服务器发送请求：

地址栏输入127.0.0.1:8080/product/list

![](https://img-blog.csdnimg.cn/073c4769e6d946ffa8cd46701b893ad4.png)

在路由器文件product.js中，手动添加设置context-type返回类型后，浏览器显示的页面结果，如下图所示：

![](https://img-blog.csdnimg.cn/7b5a868525954c39a75bdbf89a24215d.png)

**小结：**res.write() 发送字符串对象或buffer对象的响应。

**1. res.write()响应的数据"所见即所得"**

res.write()的返回数据是没有经过处理的，原封不动的返回原数据，所见即所得

**2. res.write()与res.end()总是且必须成对出现**

如果要使用res.write()最后必须要有res.end，两者是成对出现的，缺一不可，也就是说使用 **res.write方法**向前端返回数据， **必须调用res.end方法结束请求**。否则浏览器会一直处于处于请求状态

**3. res.write()方法在结束浏览器响应请求之前，允许多次调用**

如果想要输出多条语句，使用的是res.write()，也就是说在res.end() 之前，res.write() 可以被执行多次），且返回的数据会被拼接到一起。

**4.res.write()是可以结合HTML标签显示的**

res.write()输出内容可以结合HTML标签进行使用。

**5. res.write()只支持输出字符串类型或是Buffer对象两种内容类型的数据**
如果此时我们输出一个数字就会报错，查看报错信息，提醒我们不能输出number类型
res.write(123);
TypeError [ERR_INVALID_ARG_TYPE]: The "chunk" argument must be of type string or an instance of Buffer or Uint8Array. Received type number (123)

![](https://img-blog.csdnimg.cn/a93407b3b40d4db79815f3eba60a4b18.png)

## 🏝️ **二. res.end方法**

*路由器文件product.js

```js
//引入express包
const express = require('express');
//创建路由器对象
const r = express.Router();
//往路由器中添加路由
//商品列表路由：get  /list
r.get('/list', (req, res) => {
  // 在响应头信息里设置响应返回的内容类型为html，编码为utf-8(在浏览器页面正常显示中文)
  // 设置内容解析的编码为utf-8，正确地告诉浏览器，服务器响应的内容是什么编码的，你浏览器应该按照我服务器设定的编码格式来解析给你的内容
  res.writeHead(200, {
     'Content-Type': 'text/html;charset=utf-8'
  });
  res.end('该方法用于结束响应的浏览器请求');
  //下面语句将不会输出
  res.end("Hello world");
});

//导出路由器对象
module.exports = r;
```

![](https://img-blog.csdnimg.cn/5dec94aad31f4622856ec26c5963364f.png)

**小结：**res.end() 终结响应处理流程

res.end()函数用于结束响应过程。该方法用于快速结束响应，而无需任何数据。也就是说用于在没有任何数据的情况下快速结束响应。如果有响应数据，就不能用 res.end，会报错，请使用res.send()和res.json()等方法。

**1. res.end()响应的数据"所见即所得"**

res.end()的返回数据同res.write()一样，也是没有经过处理的，原封不动的返回原数据，所见即所得

**2. res.end()是不允许输出多行的**

不同于res.write()方法，res.end()作为结束浏览器请求的方法，仅能调用一次

> res.end方法跟res.write方法一样，也可以用来向前端返回数据，因为end是结束的方法，作为结束语句只能结束一次，所以不能输出多条语句，res.end的作用主要还是配合res.write方法结束浏览器的请求

**3.** **res.end()是可以结合HTML标签显示的**

res.end()同res.write()一样，输出的内容可以是带HTML标签的内容

res.end('' 该方法用于结束响应的浏览器请求'');

**4. res.end()只支持输出字符串类型或是Buffer对象两种内容类型的数据**

res.end()同res.write一样，不能输入除字符串类型或是Buffer对象类型外的其他内容类型的数据

## 🏝️ **三. res.send()方法**

*路由器文件product.js

```js
//引入express包
const express = require('express');
//创建路由器对象
const r = express.Router();
//往路由器中添加路由
//商品列表路由：get  /list
r.get('/list', (req, res) => {
  let show = "888";
  //res.send只能被调用一次，等同于res.write+res.end()
  res.send(show);
  res.send("商品列表");
  //当参数是Array或Object，Express以JSON表示响应：
  res.send({ user: 'tobi' });
  res.send([1,2,3]);
});

//导出路由器对象
module.exports = r;
```

![](https://img-blog.csdnimg.cn/ec121e0f6291445a94fb33d8d4addb6f.png)

**小结：**res.send() 发送各种类型的响应

**1. res.send()响应的数据是经过处理的**

res.send()会自动发送更多的响应报文头，其中就有Content-Type：text/html;charset=utf-8，所以没有乱码。

即res.send返回的数据是被处理过的，打开浏览器控制台，在响应头中被自动添加了context-type，也就是说，res.send()方法响应返回给页面数据时，在响应头信息里会被自动添加设置返回数据类型的context-type属性

**2. res.send()只能被调用一次，因为它等同于res.write+res.end()**

多个send输出只执行第一个send语句，后续send语句将不被执行

**3.res.send()同res.write()、res.end()一样，可以结合HTML标签数据显示**

**4. res.send ()支 持多种内容格式的输出**

res.send()方法可以支持多种参数，比如可以传String、Array、Buffer对象、对象、json对象

当参数是Array或Object、json对象，Express以 **JSON表示响应**：

res.send({ user: 'tobi' });

res.send([1,2,3]);

![](https://img-blog.csdnimg.cn/4aed5ef5c8ac44d39e815114192e28e3.png)

**🌱 扩展：res.send([body])**

**发送HTTP响应**，该body参数可以是一个Buffer对象，一个String对象或一个Array

```js
res.send(new Buffer('whoop'));
res.send({ some: 'json' });
res.send('some html');
res.status(404).send('Sorry, we cannot find that!');
res.status(500).send({ error: 'something blew up' });
```

此方法为简单的非流式响应执行许多有用的任务：例如，它能够自动分配Content-Length HTTP响应头字段（除非先前已定义）并自动提供HEAD和HTTP缓存支持

**三种不同参数 express 响应时不同行为:**


**◼️ 当参数是Buffer对象**

该方法将Content-Type响应头字段设置为" application/octet-stream "，除非之前定义如下:

```js
res.set('Content-Type', 'text/html')
res.send(Buffer.from('some html'))
```

**◼️ 当参数是String**

该方法设置header 中Content-Type为"text/html"：

```java
res.send('some html')
```

**◼️ 当参数是Array 或 Object**

Express以JSON表示响应，该方法设置header 中Content-Type为"text / html"

```js
res.send({ user: 'tobi' })
res.send([1, 2, 3])
```

## 🏝️ **四. res.json()方法**

```js
//引入express包
const express = require('express');
//创建路由器对象
const r = express.Router();
//往路由器中添加路由
//商品列表路由：get  /list
r.get('/list', (req, res) => {
  let show = {
    id: 12,
    name: "gao",
    age: 30,
    gender: "男",
    interests: ["篮球","爬山","旅游"]
  };
  res.json(show);
  res.json('商品列表');
});

//导出路由器对象
module.exports = r;
```

res.json()方法的输出语句，浏览器都将以json格式展示在页面上：

![](https://img-blog.csdnimg.cn/460cb85f452445369be911a0e354c36c.png)

**小结：**res.json() 发送各种类型的响应

**1. res.json()响应的数据是经过处理的**

调用res.json()方法会被自动添加Content-Type:application/json; charset=utf-8，所以没有乱码。该方法告诉浏览器返回数据类型是json。

**2. res.json()只能被调用一次，因为它等同于res.json()+res.end()**

多个json输出只执行第一个json语句，后续json语句将不被执行

**4. res.json ()支 持多种内容格式的输出**

res.json()方法发送一个JSON响应。此方法发送一个响应(具有正确的内容类型)，该响应是使用JSON.stringify()转换为JSON字符串的参数

res.json()方法可以支持多种参数，该参数可以是任何JSON类型，比如可以传String、Array、Buffer对象、对象、json对象、布尔值、数字或null，还可以使用它将其他值转换为JSON

_**无论res.json()方法的参数是何种类型，Express都将以JSON表示响应**_

## **♻️ 4种API的简单总结**

1. 参数类型的区别:

**◼️** res.send() 参数为: a Buffer object / a String / an object / an Array

**◼️** res.json() 参数为:任何JSON类型

**◼️** res.end() 参数为: a Buffer object / a String

2. 场景使用的区别:

**◼️** 用于快速结束没有任何数据的响应，使用res.end()。

> 简单说就是如果服务端没有数据传回客户端就可以直接用red.end返回，如果有数据，就不能用 res.end，会报错，可以使用res.send，red.json，此时可以不写res.end了，因为在前面两个方法中默认会返回。

**◼️** 响应中要发送JSON响应，使用res.json()。

**◼️** 响应中要发送数据,使用res.send() ，但要注意header 'content-type'参数。

**◼️** 如果使用res.end()返回数据非常影响性能。

了解更多Express响应API，请参考Express官方文档：[Express 中文文档](https://www.expressjs.com.cn/4x/api.html#res "Express 中文文档")

![](https://img-blog.csdnimg.cn/48817b26878346488298eebe33f65f9f.gif)​

如果这篇【 **文章**】有帮助到你，希望可以给【 ** [青春木鱼](https://blog.csdn.net/sunyctf "青春木鱼")**】点个 **赞**👍，创作不易，相比官方的陈述，我更喜欢用【 **通俗易懂】**的文笔去讲解每一个知识点，如果有对【 **前端技术**】感兴趣的小可爱，也欢迎关注❤️❤️❤️ **【 [青春木鱼](https://blog.csdn.net/sunyctf "青春木鱼") 】**❤️❤️❤️，我将会给你带来巨大的【 **收获与惊喜】**💕💕！
