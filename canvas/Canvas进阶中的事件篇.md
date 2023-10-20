---
link: https://juejin.cn/post/7171484788602175495
title: PixiJs学前篇（四）：Canvas进阶【动画篇】🔥🔥前端图形开发之canvas入门篇【原创】JavaScript canvas画表格+画文字(或画图形)，出现文字和线条模糊的问题canvas也能实现事件系统？？？？PixiJS基础教程
description: 面对网页性能要求越来越高的今天，项目性能优化已经成了项目开发中必不可少的一个环节。尤其是现在越来越火🔥的在线教育、在线编辑、直播、游戏等类型的项目中Canvas和WebGL的运用越来越多。
keywords: 前端,Canvas
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2022-11-29T17:05:17.000Z
publisher: 稀土掘金
stats: paragraph=199 sentences=296, words=1862
---
本文为稀土掘金技术社区首发签约文章，14天内禁止转载，14天后未获授权禁止转载，侵权必究！

大家好我是ndz，很高兴也很荣幸成为了一名稀土掘金技术社区签约作者，在这里真的很感谢平台给予的肯定和各位读者的支持，感谢 🙏 🙏 🙏。

本文为稀土掘金技术社区签约作者专栏 - 《从Canvas到PixiJs》 的第六篇文章，喜欢的小伙伴记得点赞加关注，以防需要用时回来不迷路 😂


# 前言

在前面的文章中我们已经介绍了 Canvas 基础的内容：

[PixiJs学前篇（一）：Canvas基础【绘制篇】🔥🔥](https://juejin.cn/post/7161696893695688740 "https://juejin.cn/post/7161696893695688740")

[PixiJs学前篇（二）：Canvas基础【图形篇】🔥🔥](https://juejin.cn/post/7168040195365797902 "https://juejin.cn/post/7168040195365797902")

[PixiJs学前篇（三）：Canvas基础【文字篇】🔥🔥](https://juejin.cn/post/7168122048437288996 "https://juejin.cn/post/7168122048437288996")

[PixiJs学前篇（四）：Canvas进阶【动画篇】🔥🔥](https://juejin.cn/post/7170675847991394335 "https://juejin.cn/post/7170675847991394335")

在之前的文章中我们学习了Canvas的基础内容和进阶内容中的动画，接下来我们将进入Canvas进阶中的事件篇。

# 事件

在我们选择用Canvas的时候，往往不仅仅是用来做一些自执行的动画，很多时候我们都会伴随着用户的交互，比如现在比较流行的一些微信小游戏，在线编辑等都是有交互的，那么既然有交互也就意味着需要交互事件。

下面我们就来看一下Canvas中的事件。

## 鼠标事件

首先咱们还是先回顾一下鼠标的常用事件：

* click（点击）
* dblclick（双击）
* mouseover（鼠标移入）
* mouseout（鼠标移出）
* mouseenter（鼠标移入）
* mouseleave（鼠标移出）
* mousedown（鼠标按下）
* mouseup（鼠标抬起）
* mousemove（鼠标移动）
* mousewheel（鼠标滚轮）

以上就是咱们常用的鼠标事件，我们发现其中的 `mouseover`和 `mouseenter`还有 `mouseout`和 `mouseleave`好像是一样的事件，但凭我们的经验虽然看似一样，其实肯定不一样，或者说有区别。

那么他们有什么区别呢？

`mouseover`和 `mouseenter`都是鼠标移入时触发，但区别是 `mouseover`支持事件冒泡，而 `mouseenter`不支持事件冒泡。简单说就是 `mouseover`事件在鼠标指针移入被选元素或者是被选元素的任何子元素，都会触发，而 `mouseenter`事件只有在鼠标指针移入被选元素时，才会触发，移入被选元素的子元素不会触发。

`mouseout`和 `mouseleave`都是鼠标移入时触发，但区别是 `mouseout`支持事件冒泡，而 `mouseleave`不支持事件冒泡。简单说就是 `mouseout`事件在鼠标指针离开被选元素或者是被选元素的任何子元素，都会触发，而 `mouseleave`事件只有在鼠标指针离开被选元素时，才会触发，离开被选元素的子元素不会触发。

## 键盘事件

键盘事件相对于鼠标事件就比较简单一些，就三个：

* keydown（键盘按下）
* keyup（键盘抬起）
* keypress（键盘按下）

这里我们常见的其实就是 `keydown`和 `keyup`两个事件，至于 `keypress`事件其实相对于 `keydown`和 `keyup`事件复杂一些。虽然 `keypress`事件和 `keydown`事件都是按下时触发，但也有区别， `keypress`事件返回的是输入的字符的 **ASCII**码,也就是 **baiKeyAscii**，而 `keydown`事件返回的是键盘码。并且 `keypress`事件虽然也是用户按下键盘上的字符键时触发，但如果按住不让的话，会重复触发此事件。

## 事件的添加和移除

给Canvas中的元素添加事件我们用的是： `addEventListener()`方法，移除事件用的是 `removeEventListener()`方法，

下面我们尝试看看如何给Canvas元素中的元素添加鼠标事件和键盘事件。

首先看一下添加鼠标事件，代码如下：

```html

<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>canvas - 鼠标事件title>
head>
<body>
  <canvas
    id="canvas"
    width="550"
    height="500"
    style="box-shadow: 0px 0px 5px #ccc; border-radius: 8px; margin: 0; padding: 0;">
    当前浏览器不支持canvas元素，请升级或更换浏览器！
  canvas>
  <script>

    const canvas = document.getElementById('canvas');

    const ctx = canvas.getContext('2d');

    canvas.addEventListener("mousemove", mouseMoving, false);
    function mouseMoving(e) {
      console.log(`当前鼠标在Canvas中的位置: x: ${e.clientX}  y: ${e.clientY}`);
    }
  script>
body>
html>

```

效果如下：

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8231906139394c4d98ebd81dd808d508~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

如图就是给Canvas添加好鼠标移动事件的效果。

Canvas支持所有的鼠标事件，但是并不支持键盘事件，那么想要让Canvas支持键盘事件我们就需要自己处理，这边有两种方法可以实现键盘事件。

方法一：让Canvas自动获取焦点，从而支持键盘事件。

具体代码如下：

```html

<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>canvas - 键盘事件title>
  <style>
    * { margin: 0; padding: 0; }
  style>
head>
<body>
  <canvas
    id="canvas"
    width="550"
    height="500"
    tabindex="0"
    style="box-shadow: 0px 0px 5px #ccc; border-radius: 8px;">
    当前浏览器不支持canvas元素，请升级或更换浏览器！
  canvas>
  <script>

    const canvas = document.getElementById('canvas');

    const ctx = canvas.getContext('2d');

    canvas.addEventListener("keydown", doKeydown, false);
    canvas.focus();
    function doKeydown(e) {
      switch(e.keyCode) {
        case 37:
          console.log(`按下左键`)
          break;
        case 38:
        console.log(`按下上键`)
          break;
        case 39:
        console.log(`按下右键`)
          break;
        case 40:
        console.log(`按下下键`)
          break;
      }
    }
  script>
body>
html>

```

效果如下：

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9f91c99e848a4a55930f5312e252f005~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

如上所示，当我们自动让Canvas获取焦点以后，这样我们可以实现为Canvas添加键盘事件，但需要注意以下几点：

1. 首先需要为 `<canvas></canvas>`标签添加 `tabindex="0"`属性
2. 获取 `canvas`元素以后，需要调用 `focus()`方法让canvas自动获取焦点
3. 需要注意，当鼠标点击别的元素的时候， `canvas`元素会失去焦点，从而失去键盘事件

方法二：通过为 `windows`对象添加键盘事件，从而控制 `canvas`元素。

这种方法也是我们开发时常用的方法。

下面举例看一下：

```html

<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>canvas - 键盘事件title>
  <style>
    * { margin: 0; padding: 0; }
  style>
head>
<body>
  <canvas
    id="canvas"
    width="500"
    height="500"
    tabindex="0"
    style="box-shadow: 0px 0px 5px #ccc; border-radius: 8px;">
    当前浏览器不支持canvas元素，请升级或更换浏览器！
  canvas>
  <script>

    const canvas = document.getElementById('canvas');

    const ctx = canvas.getContext('2d');

    ctx.fillStyle="orange";

    let x = canvas.width / 2 - 50;
    let y = canvas.height / 2 - 50;

    ctx.fillRect(x, y, 100, 50);

    window.addEventListener("keydown", doKeydown, false);

    function doKeydown(e) {
      ctx.clearRect(0, 0, 500, 500)
      var keyID = e.keyCode ? e.keyCode :e.which;
      switch(e.keyCode) {
        case 37:
          console.log(`按下左键`)
          x = x - 10;
          ctx.fillRect(x, y, 100, 50);
          break;
        case 38:
          console.log(`按下上键`)
          y = y - 10;
          ctx.fillRect(x, y, 100, 50);
          break;
        case 39:
          console.log(`按下右键`)
          x = x + 10;
          ctx.fillRect(x, y, 100, 50);
          break;
        case 40:
          console.log(`按下下键`)
          y = y + 10;
          ctx.fillRect(x, y, 100, 50);
          break;
      }
    }
  script>
body>
html>
```

效果如下：

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/312491f7e3ce4eb5aeb26a932f752359~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

如上所示，我们可以通过 `windows`对象控制 `canvas`元素，这也是我们常用的方法。

## 内部元素添加事件

上面的所有事件其实都是添加到 `canvas`元素上的，但往往在平常的需求中我们需要针对 `canvas`元素内部的子元素做单独的事件交互，那么我们就需要考虑如何给 `canvas`元素的内部元素添加事件。

`canvas`元素本身并没有提供给内部元素添加事件的Api，正常开发中我们其实也很少会直接使用原生的方式和 `canvas`元素的内部元素进行交互，因为正常开发我们往往会使用 `canvas`的一些成熟的框架或者库（比如Pixi.js）来实现这样的需求，而这样的库中肯定已经封装了为单独元素添加交互的Api。但这里咱们既然学习的是 `canvas`本身，那么咱们就看看如何实现和 `canvas`元素的内部元素进行交互。

这里咱们就以拖拽为例，假如 `canvas`元素内部有多个子元素，那么想拖拽其中一个子元素，我们首先得知道，在鼠标按下的时候是否按在 `canvas`元素的子元素上，只有按在 `canvas`元素的子元素上我们才能对它进行拖拽。

下面咱们就以这个拖拽的案例来说一下。

#### 1、布局

这里我们准备了几个图片，我们先把他绘制到 `canvas`元素中。

代码如下：

```html

<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>canvas - 事件title>
  <style>
    * { margin: 0; padding: 0; }
  style>
head>
<body>
  <canvas
    id="canvas"
    width="1000"
    height="500"
    tabindex="0"
    style="box-shadow: 0px 0px 5px #ccc; border-radius: 8px;">
    当前浏览器不支持canvas元素，请升级或更换浏览器！
  canvas>
  <script>

    const canvas = document.getElementById('canvas');
    const width = canvas.width;
    const height = canvas.height;

    const ctx = canvas.getContext('2d');
    const images = [
      {
        name: "白月魁",
        url: "../images/bailaoban.jpg"
      },
      {
        name: "鸣人",
        url: "../images/mingren.jpg",
      },
      {
        name: "路飞",
        url: "../images/lufei.jpg",
      },
      {
        name: "哪吒",
        url: "../images/nazha.jpg",
      },
      {
        name: "千寻",
        url: "../images/qianxun.jpg",
      },
    ];

    images.forEach((item)=>{

      const image = new Image()
      image.src = item.url;
      const name = item.name;
      image.onload = () => {

        const w = 200;

        const h = 200 / image.width * image.height;
        const x = Math.random() * (width - w) ;
        const y = Math.random() * (height - h);
        const imageObj = { image, name, x, y, w, h }
        draw(imageObj)
      }
    })

    function draw(imageObj) {
      ctx.drawImage(imageObj.image, imageObj.x, imageObj.y, imageObj.w, imageObj.h);
    }
  script>
body>
html>

```

效果如下：

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/abf5e7c11a794ea9b22fa68de96619c5~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

如上所示，把图片随机的绘制到 `canvas`元素中。

#### 2、添加事件

把图片绘制到 `canvas`元素中以后，我们接下来就是为 `canvas`元素添加 `&#x9F20;&#x6807;&#x6309;&#x4E0B;`、 `&#x9F20;&#x6807;&#x79FB;&#x52A8;`和 `&#x9F20;&#x6807;&#x62AC;&#x8D77;`是哪个事件了。

代码如下：

```js

canvas.addEventListener("mousedown", mousedownFn, false)

function mousedownFn(e) {

  canvas.addEventListener("mousemove", mousemoveFn, false)
  canvas.addEventListener("mouseup", mouseupFn, false)
}

function mousemoveFn() {}

function mouseupFn() {}

```

因为只有鼠标按下才能拖拽，所以我们把鼠标移动和鼠标抬起事件的添加放在鼠标按下的事件中。

#### 3、判断选中元素

定义完事件以后，我们就需要判断每次点击的元素是其中的哪一个，这样我们才能针对这个元素做交互。

判断每次点击的元素是其中的哪一个元素，有两种方法：

##### 方法一：

一种是通过计算，如上面布局的代码，每个图片绘制的x、y、width和height我们都是知道的，那么当我们每次点击下去的时候就可以遍历图片的数据，看我们是否点击到元素上。

代码如下：

```html

<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>canvas - 事件title>
  <style>
    * { margin: 0; padding: 0; }
  style>
head>
<body>
  <canvas
    id="canvas"
    width="1000"
    height="500"
    tabindex="0"
    style="box-shadow: 0px 0px 5px #ccc; border-radius: 8px;">
    当前浏览器不支持canvas元素，请升级或更换浏览器！
  canvas>
  <script>

    const canvas = document.getElementById('canvas');
    const width = canvas.width;
    const height = canvas.height;

    const ctx = canvas.getContext('2d');
    const images = [
      {
        name: "白月魁",
        url: "../images/bailaoban.jpg"
      },
      {
        name: "鸣人",
        url: "../images/mingren.jpg",
      },
      {
        name: "路飞",
        url: "../images/lufei.jpg",
      },
      {
        name: "哪吒",
        url: "../images/nazha.jpg",
      },
      {
        name: "千寻",
        url: "../images/qianxun.jpg",
      },
    ];
    let imagesData = []
    let clickCoordinate = { x: 0, y: 0 }
    let target;
    images.forEach((item)=>{

      const image = new Image()
			image.src = item.url;
      const name = item.name;
      image.onload = () => {

        const w = 200;

        const h = 200 / image.width * image.height;
        const x = Math.random() * (width - w) ;
        const y = Math.random() * (height - h);
        const imageObj = { image, name, x, y, w, h }
        imagesData.push(imageObj)
        draw(imageObj)
      }
    })

    function draw(imageObj) {
      ctx.drawImage(imageObj.image, imageObj.x, imageObj.y, imageObj.w, imageObj.h);
    }

    canvas.addEventListener("mousedown", mousedownFn, false)

    function mousedownFn(e) {

      clickCoordinate.x = e.pageX - canvas.offsetLeft;
      clickCoordinate.y = e.pageY - canvas.offsetTop;

      checkElement()

      canvas.addEventListener("mousemove", mousemoveFn, false)
      canvas.addEventListener("mouseup", mouseupFn, false)
    }

    function mousemoveFn() {}

    function mouseupFn() {}

    function checkElement() {
      imagesData.forEach((item, index)=>{
        const minX = item.x
        const maxX = item.x + item.w
        const minY = item.y
        const maxY = item.y + item.h
        if(minX x && clickCoordinate.x y && clickCoordinate.y console.log("点击的元素是：", item.name)
        }
      })

    }

  script>
body>
html>
```

效果如下：

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/af94ba6fd4af40a78783e869fe97afc2~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

除了计算其实我们还可以利用 `canvas`元素自身提供的方法来确定咱们选中的元素是哪一个。

##### 方法二：

这里利用的是： `isPointInPath()`方法，此方法可以把坐标传入，然后判断是否在路径之内。

语法： `isPointInPath(x, y)` x为监测点的x坐标，y为监测点的y坐标。

这里需要注意的是，在我们的案例中，我们是通过 `drawImage()`方法把图片绘制到 `canvas`元素上，而 `drawImage()`方法不支持 `isPointInPath()`方法的路径检测，这里我们就需使用绘制路径的方法，因此在绘制图片的时候，我们就需要同时绘制一个一样大小的路径。

绘制方法就需要修改为：

```js

function draw(imageObj) {
  ctx.drawImage(imageObj.image, imageObj.x, imageObj.y, imageObj.w, imageObj.h);
  ctx.beginPath();
  ctx.strokeStyle = "#fff";
  ctx.rect(imageObj.x, imageObj.y, imageObj.w, imageObj.h);
  ctx.stroke();
}

```

检测选中元素的方法修改为：

```html

<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>canvas - 事件title>
  <style>
    * { margin: 0; padding: 0; }
  style>
head>
<body>
  <canvas
    id="canvas"
    width="1000"
    height="500"
    tabindex="0"
    style="box-shadow: 0px 0px 5px #ccc; border-radius: 8px;">
    当前浏览器不支持canvas元素，请升级或更换浏览器！
  canvas>
  <script>

    const canvas = document.getElementById('canvas');
    const width = canvas.width;
    const height = canvas.height;

    const ctx = canvas.getContext('2d');
    const images = [
      {
        name: "白月魁",
        url: "../images/bailaoban.jpg"
      },
      {
        name: "鸣人",
        url: "../images/mingren.jpg",
      },
      {
        name: "路飞",
        url: "../images/lufei.jpg",
      },
      {
        name: "哪吒",
        url: "../images/nazha.jpg",
      },
      {
        name: "千寻",
        url: "../images/qianxun.jpg",
      },
    ];
    let imagesData = []
    let clickCoordinate = { x: 0, y: 0 }
    let target;
    images.forEach((item)=>{

      const image = new Image()
			image.src = item.url;
      const name = item.name;
      image.onload = () => {

        const w = 200;

        const h = 200 / image.width * image.height;
        const x = Math.random() * (width - w) ;
        const y = Math.random() * (height - h);
        const imageObj = { image, name, x, y, w, h }
        imagesData.push(imageObj)
        draw(imageObj)
      }
    })

    function draw(imageObj) {
      ctx.drawImage(imageObj.image, imageObj.x, imageObj.y, imageObj.w, imageObj.h);
      ctx.beginPath();
      ctx.strokeStyle = "#fff";
      ctx.rect(imageObj.x, imageObj.y, imageObj.w, imageObj.h);
      ctx.stroke();
    }

    canvas.addEventListener("mousedown", mousedownFn, false)

    function mousedownFn(e) {

      clickCoordinate.x = e.pageX - canvas.offsetLeft;
      clickCoordinate.y = e.pageY - canvas.offsetTop;

      checkElement()

      canvas.addEventListener("mousemove", mousemoveFn, false)
      canvas.addEventListener("mouseup", mouseupFn, false)
    }

    function mousemoveFn() {}

    function mouseupFn() {}

    function checkElement() {
      imagesData.forEach((item, index)=>{
        draw(item)
        if(ctx.isPointInPath(clickCoordinate.x, clickCoordinate.y)) {
          target = index
          console.log("点击的元素是：", item.name)
        }
      })
    }

  script>
body>
html>
```

效果如下：

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/df864242b0e7480f92369a1261f07a71~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

#### 4、编写移动的方法

知道选中的元素以后，我们就需要在移动的时候把移动的坐标赋值给选中的元素，让选中的元素跟着鼠标移动。

移动的方法如下：

```js

function mousemoveFn(e) {
  const moveX = e.pageX
  const moveY = e.pageY

  imagesData[target].x = imagesData[target].x + ( moveX - clickCoordinate.x );
  imagesData[target].y = imagesData[target].y + ( moveY - clickCoordinate.y );

                    ctx.clearRect(0, 0, width, height);

  imagesData.forEach((i) => draw(i))

  clickCoordinate.x = moveX;
  clickCoordinate.y = moveY;
}

function mouseupFn() {

  canvas.removeEventListener("mousemove", mousemoveFn, false)
  canvas.removeEventListener("mouseup", mouseupFn, false)

  target = undefined
}
```

到此咱们的相册拖拽案例就算完成了，看一下整体代码：

```html

<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>canvas - 事件title>
  <style>
    * { margin: 0; padding: 0; }
  style>
head>
<body>
  <canvas
    id="canvas"
    width="1000"
    height="500"
    tabindex="0"
    style="box-shadow: 0px 0px 5px #ccc; border-radius: 8px;">
    当前浏览器不支持canvas元素，请升级或更换浏览器！
  canvas>
  <script>

    const canvas = document.getElementById('canvas');
    const width = canvas.width;
    const height = canvas.height;

    const ctx = canvas.getContext('2d');
    const images = [
      {
        name: "白月魁",
        url: "../images/bailaoban.jpg"
      },
      {
        name: "鸣人",
        url: "../images/mingren.jpg",
      },
      {
        name: "路飞",
        url: "../images/lufei.jpg",
      },
      {
        name: "哪吒",
        url: "../images/nazha.jpg",
      },
      {
        name: "千寻",
        url: "../images/qianxun.jpg",
      },
    ];
    let imagesData = []
    let clickCoordinate = { x: 0, y: 0 }
    let target;
    images.forEach((item)=>{

      const image = new Image()
			image.src = item.url;
      const name = item.name;
      image.onload = () => {

        const w = 200;

        const h = 200 / image.width * image.height;
        const x = Math.random() * (width - w) ;
        const y = Math.random() * (height - h);
        const imageObj = { image, name, x, y, w, h }
        imagesData.push(imageObj)
        draw(imageObj)
      }
    })

    function draw(imageObj) {
      ctx.drawImage(imageObj.image, imageObj.x, imageObj.y, imageObj.w, imageObj.h);
      ctx.beginPath();
      ctx.strokeStyle = "#fff";
      ctx.rect(imageObj.x, imageObj.y, imageObj.w, imageObj.h);
      ctx.stroke();
    }

    canvas.addEventListener("mousedown", mousedownFn, false)

    function mousedownFn(e) {

      clickCoordinate.x = e.pageX - canvas.offsetLeft;
      clickCoordinate.y = e.pageY - canvas.offsetTop;

      checkElement()

      if(target !== undefined) return;

      canvas.addEventListener("mousemove", mousemoveFn, false)
      canvas.addEventListener("mouseup", mouseupFn, false)
    }

    function mousemoveFn(e) {
      const moveX = e.pageX
      const moveY = e.pageY

      imagesData[target].x = imagesData[target].x + ( moveX - clickCoordinate.x );
      imagesData[target].y = imagesData[target].y + ( moveY - clickCoordinate.y );

      ctx.clearRect(0, 0, width, height);

      imagesData.forEach((i) => draw(i))

      clickCoordinate.x = moveX;
      clickCoordinate.y = moveY;
    }

    function mouseupFn() {

      canvas.removeEventListener("mousemove", mousemoveFn, false)
      canvas.removeEventListener("mouseup", mouseupFn, false)

      target = undefined
    }

    function checkElement() {
      imagesData.forEach((item, index)=>{
        draw(item)
        if(ctx.isPointInPath(clickCoordinate.x, clickCoordinate.y)) {
          target = index
          console.log("点击的元素是：", item.name)
        }
      })
    }

  script>
body>
html>
```

咱们看一下具体的效果：

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/00671f0ee4f3421a977e6ec22bd06ca1~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

到此咱们的 `Canvas`事件篇就算完成了，在日常咱们的开发中其实主要要是用 `Pixi`等框架来实现，直接用 `Canvas`元素来实现的话开发的效率肯定比较低。

下一篇文章咱们就来唠唠 `Canvas`的的应用吧，介绍几个我们常用的案例。
