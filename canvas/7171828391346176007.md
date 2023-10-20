---
link: https://juejin.cn/post/7171828391346176007
title: PixiJs学前篇（六）：Canvas进阶【应用篇】🔥🔥
description: 面对网页性能要求越来越高的今天，项目性能优化已经成了项目开发中必不可少的一个环节。尤其是现在越来越火🔥的在线教育、在线编辑、直播、游戏等类型的项目中Canvas和WebGL的运用越来越多。
keywords: 前端,Canvas
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2022-11-30T15:17:06.000Z
publisher: 稀土掘金
stats: paragraph=213 sentences=320, words=3070
---
本文为稀土掘金技术社区首发签约文章，14天内禁止转载，14天后未获授权禁止转载，侵权必究！

大家好我是ndz，很高兴也很荣幸成为了一名稀土掘金技术社区签约作者，在这里真的很感谢平台给予的肯定和各位读者的支持，感谢 🙏 🙏 🙏。

本文为稀土掘金技术社区签约作者专栏 - 《从Canvas到PixiJs》 的第七篇文章，喜欢的小伙伴记得点赞加关注，以防需要用时回来不迷路 😂


# 前言

在前面的文章中我们已经介绍了 Canvas 基础的内容：

[PixiJs学前篇（一）：Canvas基础【绘制篇】🔥🔥](https://juejin.cn/post/7161696893695688740 "https://juejin.cn/post/7161696893695688740")

[PixiJs学前篇（二）：Canvas基础【图形篇】🔥🔥](https://juejin.cn/post/7168040195365797902 "https://juejin.cn/post/7168040195365797902")

[PixiJs学前篇（三）：Canvas基础【文字篇】🔥🔥](https://juejin.cn/post/7168122048437288996 "https://juejin.cn/post/7168122048437288996")

[PixiJs学前篇（四）：Canvas进阶【动画篇】🔥🔥](https://juejin.cn/post/7170675847991394335 "https://juejin.cn/post/7170675847991394335")

[PixiJs学前篇（五）：Canvas进阶【事件篇】🔥🔥](https://juejin.cn/post/7171484788602175495 "https://juejin.cn/post/7171484788602175495")

在之前的文章中我们学习了Canvas的基础内容和进阶内容中的动画和事件，接下来我们将进入Canvas最后的应用篇。

本文主要要以日常开发中我们常见的案例为主，说一下canvas在我们前端开发中必不可少的一些应用。人狠话不多，直接进入主题吧。

# 应用一：图片保存

在我们的日常的摸鱼中，我们会看到一些H5小游戏，或者类似支付宝年度账单之类的小应用，其中就会有保存图片的按钮，或者说长按保存图片之类的功能，下面我们就看看这些功能如何实现。

## 所需方法

首先我们需要知道在保存图片的案例中，我们需要用到哪些方法？

### toDataURL

`toDataURL()`方法可以返回一个包含图片的 `Data URL`。

`Data URL`也就是前缀为 `data:` 协议的URL，其允许内容创建者向文档中嵌入小文件。

语法：<br> `toDataURL(type, encoderOptions)`

参数：

* type： `type`为图片格式，默认为 `image/png`，也可指定为： `image/jpeg`、 `image/webp`等格式
* encoderOptions： `encoderOptions`为图片的质量，默认值 `0.92`。在指定图片格式为 `image/jpeg` 或 `image/webp` 的情况下，可以从 `0` 到 `1` 的区间内选择图片的质量。如果不在这个范围内，则使用默认值 `0.92`。

下面咱们以上篇文章 [PixiJs学前篇（五）：Canvas进阶【事件篇】🔥🔥](https://juejin.cn/post/7171484788602175495 "https://juejin.cn/post/7171484788602175495") 中的相册拖拽为例，把每次拖拽好的相册截屏保存起来。

```js

function clickFn(){

  let url = canvas.toDataURL("image/png");

  Img.src = url;

  let arr = url.split(",")
  let mime = arr[0].match(/:(.*?);/)[1]
  let bstr = atob(arr[1])
  let n = bstr.length
  let u8arr = new Uint8Array(n);
  while (n--) {
    u8arr[n] = bstr.charCodeAt(n);
  }

  let file = new File([u8arr], "filename", { type: mime });

  let aDom = document.createElement("a");
  aDom.download = file.name;
  let href = URL.createObjectURL(file);
  aDom.href = href;
  document.body.appendChild(aDom);
  aDom.click();
  document.body.removeChild(aDom);
  URL.revokeObjectURL(href);
};
```

整个保存为图片并下载的方法如上，其中的canvas就为之前的相册，整体代码如下：

```html

<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>canvas - 保存并下载title>
  <style>
    * { margin: 0; padding: 0; }
    canvas {
      box-shadow: 0px 0px 5px #ccc;
      border-radius: 8px;
      float: left;
    }
    img {
      width: 800px;
      height: 500px;
      float: right;
    }
    button {
      position: absolute;
      top: 550px;
      left: 50%;
      margin-left: -40px;
    }
  style>
head>
<body>
  <canvas id="canvas" width="800"  height="500">
    当前浏览器不支持canvas元素，请升级或更换浏览器！
  canvas>
  <img id="img" src="" />
  <button id="btn">转化为图片且下载button>
  <script>

    const canvas = document.getElementById('canvas');
    var Img = document.getElementById('img');
    var Btn = document.getElementById('btn');
    const width = canvas.width;
    const height = canvas.height;

    const ctx = canvas.getContext('2d');
    const images = [
      {
        name: "白月魁",
        url: "https://img1.baidu.com/it/u=4141276181,3458238270&fm=253&fmt=auto&app=138&f=JPEG?w=281&h=500"
      },
      {
        name: "鸣人",
        url: "https://img2.baidu.com/it/u=1548765981,166433699&fm=253&fmt=auto&app=138&f=JPEG?w=500&h=889",
      },
      {
        name: "路飞",
        url: "https://img2.baidu.com/it/u=1700240772,3511789617&fm=253&fmt=auto&app=138&f=JPEG?w=500&h=500",
      },
      {
        name: "哪吒",
        url: "https://img2.baidu.com/it/u=4044887937,3129736188&fm=253&fmt=auto&app=138&f=JPEG?w=640&h=392",
      },
      {
        name: "千寻",
        url: "https://img1.baidu.com/it/u=3907076642,679964949&fm=253&fmt=auto&app=138&f=JPEG?w=500&h=293",
      },
    ];
    let imagesData = []
    let clickCoordinate = { x: 0, y: 0 }
    let target;
    images.forEach((item)=>{

      const image = new Image()
      image.crossOrigin = "Anonymous";
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

    Btn.addEventListener("click", clickFn, false)

    function mousedownFn(e) {

      clickCoordinate.x = e.pageX - canvas.offsetLeft;
      clickCoordinate.y = e.pageY - canvas.offsetTop;

      checkElement()

      if(target === undefined) return;

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

    function clickFn(){

      let url = canvas.toDataURL("image/png");

      Img.src = url;

      let arr = url.split(",")
      let mime = arr[0].match(/:(.*?);/)[1]
      let bstr = atob(arr[1])
      let n = bstr.length
      let u8arr = new Uint8Array(n);
      while (n--) {
        u8arr[n] = bstr.charCodeAt(n);
      }

      let file = new File([u8arr], "filename", { type: mime });

      let aDom = document.createElement("a");
      aDom.download = file.name;
      let href = URL.createObjectURL(file);
      aDom.href = href;
      document.body.appendChild(aDom);
      aDom.click();
      document.body.removeChild(aDom);
      URL.revokeObjectURL(href);
    };

  script>
body>
html>

```

看一下具体效果：

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/022bd1fc0552454dad05620283702e68~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

# 应用二：主题（滤镜）

滤镜大家应该比较熟悉，尤其女生应该会更加熟悉，比如每次拍照完成以后 P图 不！应该说修图的时候，我们都会换各种主题（滤镜），比如说暖色、冷色、复古等等。而这些主题就可以用滤镜来实现。下面我们来实现个简单的例子看一下。

实现滤镜的方式有很多种方式，这里既然咱们介绍的是canvas的应用，那么就用canvas的方式来实现看看。

具体实现我们可以遍历所有像素然后改变他们的数值，再将被修改的像素数组通过 `putImageData()` 方法放回到画布中去，以达到 `&#x53CD;&#x76F8;&#x989C;&#x8272;`。

## 所需方法：

首先我们需要知道在制作主题的案例中，我们需要用到哪些方法？

### getImageData()

`getImageData()`方法可以返回一个 `ImageData`对象。

`ImageData`对象用来描述 `canvas`区域隐含的像素数据，此区域通过矩形表示，起始点为 `(sx, sy)`、宽为 `sw`、高为 `sh`

语法：<br> `getImageData(sx, sy, sw, sh)`

参数：

* sx：将要被提取的图像数据矩形区域的左上角 x 坐标。
* sy：将要被提取的图像数据矩形区域的左上角 y 坐标。
* sw：将要被提取的图像数据矩形区域的宽度。
* sh：将要被提取的图像数据矩形区域的高度。

### putImageData()

`putImageData()`方法和 `getImageData()`方法正好相反，可以将数据从已有的 `ImageData`对象绘制为位图。如果提供了一个绘制过的矩形，则只绘制该矩形的像素。

语法：<br> `putImageData(imagedata, dx, dy, dirtyX, dirtyY, dirtyWidth, dirtyHeight)`

参数：

* ImageData：包含像素值的数组对象。
* dx：源图像数据在目标画布中 x 轴方向的偏移量。
* dy：源图像数据在目标画布中 y 轴方向的偏移量。
* dirtyX：可选参数，在源图像数据中，矩形区域左上角的位置。默认是整个图像数据的左上角（x 坐标）。
* dirtyY：可选参数，在源图像数据中，矩形区域左上角的位置。默认是整个图像数据的左上角（y 坐标）。
* dirtyWidth：可选参数，在源图像数据中，矩形区域的宽度。默认是图像数据的宽度。
* dirtyHeight：可选参数，在源图像数据中，矩形区域的高度。默认是图像数据的高度。

知道了这两个方法以后，下面我们简单编写两个方法来做两个主题（滤镜）效果。

#### 黑白主题

黑白主题咱们用一个： `blackWhite`函数来实现，具体是减掉颜色的最大色值255来实现

代码如下：

```js
const blackWhite = function() {
    ctx.drawImage(img, 0, 0, 450, 800);
    const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
    const data = imageData.data;
    for (var i = 0; i < data.length; i += 4) {
      var avg = (data[i] + data[i + 1] + data[i + 2]) / 3;
      data[i]     = avg;
      data[i + 1] = avg;
      data[i + 2] = avg;
    }
    ctx.putImageData(imageData, 0, 0);
};
```

#### 曝光主题

曝光主题咱们用一个： `exposure`函数来实现，具体是用红绿和蓝的平均值来实现

代码如下：

```js
const exposure = function() {
    ctx.drawImage(img, 0, 0, 450, 800);
    const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
    const data = imageData.data;
    for (var i = 0; i < data.length; i += 4) {
      data[i]     = 255 - data[i];
      data[i + 1] = 255 - data[i + 1];
      data[i + 2] = 255 - data[i + 2];
    }
    ctx.putImageData(imageData, 0, 0);
};
```

直接上例子吧：

```html

<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>canvas - 主题title>
  <style>
    canvas {
      box-shadow: 0px 0px 5px #ccc;
      border-radius: 8px;
    }
  style>
head>
<body>
  <canvas id="canvas" width="450" height="800">
    当前浏览器不支持canvas元素，请升级或更换浏览器！
  canvas>
  <div class="btnBox">
     <button id="original">还原button>
     <button id="blackWhite">黑白主题button>
     <button id="exposure">曝光主题button>
  div>
  <script>

    var canvas = document.getElementById('canvas');
    var originalEl = document.getElementById('original');
    var blackWhiteEl = document.getElementById('blackWhite');
    var exposureEl = document.getElementById('exposure');
    var sepiaEl = document.getElementById('sepia');

    if(canvas.getContext) {

      var ctx = canvas.getContext('2d');
      var img = new Image();
      img.crossOrigin = 'anonymous';
      img.src = 'https://img1.baidu.com/it/u=4141276181,3458238270&fm=253&fmt=auto&app=138&f=JPEG';
      img.onload = function() {
        ctx.drawImage(img, 0, 0, 450, 800);
      };
      var original = function() {
        ctx.drawImage(img, 0, 0, 450, 800);
      };
      var exposure = function() {
        ctx.drawImage(img, 0, 0, 450, 800);
        const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
        const data = imageData.data;
        for (var i = 0; i < data.length; i += 4) {
          data[i]     = 255 - data[i];
          data[i + 1] = 255 - data[i + 1];
          data[i + 2] = 255 - data[i + 2];
        }
        ctx.putImageData(imageData, 0, 0);
      };

      var blackWhite = function() {
        ctx.drawImage(img, 0, 0, 450, 800);
        const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
        const data = imageData.data;
        for (var i = 0; i < data.length; i += 4) {
          var avg = (data[i] + data[i + 1] + data[i + 2]) / 3;
          data[i]     = avg;
          data[i + 1] = avg;
          data[i + 2] = avg;
        }
        ctx.putImageData(imageData, 0, 0);
      };
      originalEl.addEventListener("click", function(evt) {
        original()
      })
      blackWhiteEl.addEventListener("click", function(evt) {
        blackWhite()
      })
      exposureEl.addEventListener("click", function(evt) {
        exposure()
      })
    }
  script>
body>
html>
```

我们看一下具体效果：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ba2a6350043d4b25b86789c7933183ad~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

# 应用三：拾色器

拾色器也是比较常见的一个案例，尤其在现在很多在线编辑的项目中很常见。

## 所需方法：

拾色器案例用的方法还是上面我们介绍过的 `getImageData()`方法。这里就不再赘述，记不得了可以返回去看看😄。

需要补充的是： `getImageData()`方法会返回一个 `ImageData`对象，它是画布区域的数据，画布的四个角分别表示为 (left, top)、(left + width, top)、(left, top + height)和(left + width, top + height) 四个点。这四个坐标点被设定为画布坐标空间元素。

接下来我们再用 `getImageData()`方法做一个拾色器

```html

<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>canvas - 拾色器title>
  <style>

    canvas {
      box-shadow: 0px 0px 5px #ccc;
      border-radius: 8px;
    }
    div {
      width: 430px;
      height: 30px;
      color: #fff;
      box-shadow: 0px 0px 5px #ccc;
      border-radius: 8px;
      line-height: 30px;
      padding: 10px;
    }
  style>
head>
<body>
  <canvas id="canvas" width="450" height="800">
    当前浏览器不支持canvas元素，请升级或更换浏览器！
  canvas>
  <div id="hovered">div>
  <div id="selected">div>
  <script>

    var canvas = document.getElementById('canvas');

    if(canvas.getContext) {

      var ctx = canvas.getContext('2d');
      var img = new Image();
      img.crossOrigin = 'anonymous';
      img.src = 'https://img1.baidu.com/it/u=4141276181,3458238270&fm=253&fmt=auto&app=138&f=JPEG';
      img.onload = function() {
        ctx.drawImage(img, 0, 0, 450, 800);
        img.style.display = 'none';
      };
      var hoveredColor = document.getElementById('hovered');
      var selectedColor = document.getElementById('selected');

      canvas.addEventListener('mousemove', function(event) {
        pickColor('move', event, hoveredColor);
      });
      canvas.addEventListener('click', function(event) {
        pickColor('click', event, selectedColor);
      });

      function pickColor(type, event, destination) {
        var x = event.layerX;
        var y = event.layerY;
        var pixel = ctx.getImageData(x, y, 1, 1);
        var data = pixel.data;
        const rgba = `rgba(${data[0]}, ${data[1]}, ${data[2]}, ${data[3] / 255})`;
        destination.style.background = rgba;
        if(type === 'move') {
          destination.textContent = "划过的颜色为：" + rgba;
        } else {
          destination.textContent = "选中的颜色为：" + rgba;
        }
        return rgba;
      }
    }
  script>
body>
html>

```

效果如下：

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c5c6c99834804fa4aeafff4b9ff5fb55~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

# 应用四：签名

签名也是比较常见的案例，在各大银行的app上基本都有，还有一些桌面应用上也是比较常见的，但这个案例的实现难度确比较低，具体有多简单咱们直接看代码吧。

具体代码如下：

```html

<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>canvas - 签名title>
  <style>

    canvas {
        box-shadow: 0px 0px 5px #ccc;
        border-radius: 8px;
    }
    div {
        width: 450px;
        box-shadow: 0px 0px 5px #ccc;
        border-radius: 8px;
        text-align: center;
    }
  style>
head>
<body>
  <canvas id="canvas" width="450" height="300">
      当前浏览器不支持canvas元素，请升级或更换浏览器！
  canvas>
  <div id="clear">清空画布div>
  <script>

    var canvas = document.getElementById('canvas');
    var clear = document.getElementById('clear');
    const ctx = canvas.getContext("2d");
    canvas.addEventListener('mouseenter', () => {
        canvas.addEventListener('mousedown', (e) => {
            ctx.beginPath()
            ctx.moveTo(e.offsetX, e.offsetY)
            canvas.addEventListener('mousemove', draw)
        })
        canvas.addEventListener('mouseup', () => {
            canvas.removeEventListener('mousemove', draw)
        })
    })
    function draw(e) {
        ctx.lineTo(e.offsetX, e.offsetY)
        ctx.stroke()
    }
    clear.addEventListener('click', () => {
        ctx.clearRect(0, 0, canvas.width, canvas.height);
    })

script>
body>
html>
```

效果如下：

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e7340d9b2d744b8095b969fdb87bf3f4~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

# 应用五：刮刮奖

刮刮奖这个案例其实不是太常见，但确是一个经典的案例，比如我们要说的下一个案例：擦玻璃，就是这个案例的一个扩展。

## 所需方法：

不管是刮刮奖还是擦玻璃，我们主要应用到的方法是 `globalCompositeOperation`，该属性用于设置在绘制新形状时应用的合成操作的类型。

该属性有很多方法，下面咱们看一下都有哪些？

语法： `globalCompositeOperation = type`，属性值 type 表示是要使用的合成或混合模式操作的类型。

type属性为不同值时，绘制显示将会不同，具体的类型值我们列一下：

属性类型表现形式source-over默认。在目标图像上显示源图像source-atop在目标图像顶部显示源图像。源图像位于目标图像之外的部分是不可见的。source-in在目标图像中显示源图像。只有目标图像内的源图像部分会显示，目标图像是透明的。source-out在目标图像之外显示源图像。只会显示目标图像之外源图像部分，目标图像是透明的。destination-over在源图像上方显示目标图像。destination-atop在源图像顶部显示目标图像。源图像之外的目标图像部分不会被显示。destination-in在源图像中显示目标图像。只有源图像内的目标图像部分会被显示，源图像是透明的。destination-out在源图像外显示目标图像。只有源图像外的目标图像部分会被显示，源图像是透明的。lighter显示源图像 + 目标图像。copy显示源图像。忽略目标图像。xor使用异或操作对源图像与目标图像进行组合。

具体的表现如下图：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cca11bcea17a47cd9300581432a5d3ba~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

上面是我在网上找的一个不同属性绘制成不同效果的图。

了解了 `globalCompositeOperation` 属性的各种类型的效果，下面咱们开始写案例。

具体代码如下：

```html

<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>canvas - 刮刮奖title>
  <style>

    canvas {
      background-color: #ccc;
      box-shadow: 0px 0px 5px #ccc;
      border-radius: 8px;
      float: left;
    }
  style>
head>
<body>
  <canvas id="canvas" width="1000" height="500">
    当前浏览器不支持canvas元素，请升级或更换浏览器！
  canvas>
  <script>

    var canvas = document.getElementById('canvas');

    if(canvas.getContext) {

      var ctx = canvas.getContext('2d');
      const imageUrl = "https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fimg.mp.itc.cn%2Fupload%2F20160909%2Feca561d1ecce4fcab4f600a74f15b221_th.jpeg&refer=http%3A%2F%2Fimg.mp.itc.cn&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=auto?sec=1672410563&t=65c34c7d49a899c2f2a3c0f99827312f";

      ctx.lineCap = 'round'
      ctx.lineJoin = 'round'
      ctx.lineWidth = 50

      canvas.addEventListener("mousedown", mousedownFn, false)
      let downX,downY

      function mousedownFn(e) {
        e.preventDefault()
        downX = e.pageX
        downY = e.pageY
        drawLine({startX: downX, startY: downY})

        canvas.addEventListener("mousemove", mousemoveFn, false)
        canvas.addEventListener("mouseup", mouseupFn, false)
      }

      function mousemoveFn(e) {
        e.preventDefault()
        const moveX = e.pageX
        const moveY = e.pageY
        drawLine({endX: moveX, endY: moveY})
        downX = moveX
        downY = moveY
      }

      function mouseupFn() {

        canvas.removeEventListener("mousemove", mousemoveFn, false)
        canvas.removeEventListener("mouseup", mouseupFn, false)
      }

      function drawLine(position) {
        const { startX, startY, endX, endY } = position
        ctx.beginPath()
        ctx.moveTo(startX, startY)
        ctx.lineTo(endX || startX, endY || startY)
        ctx.stroke()
      }

      drawImage(imageUrl)
      function drawImage(src) {
        const img = new Image()
        img.crossOrigin = ''
        img.src = src
        img.onload = () => {
          const imageAspectRatio = img.width / img.height
          const canvasAspectRatio = canvas.width / canvas.height
          ctx.drawImage( img, 0, 0, canvas.width, canvas.height )
          ctx.globalCompositeOperation = 'destination-out'
        }
      }
    }
  script>
body>
html>
```

效果如下：

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a6453764c248446ea4d6be54e8093bec~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

# 应用六：擦玻璃

擦玻璃和刮刮奖是差不多的实现，不同的是，刮刮奖的下面是中奖的文字，上面是一张图，而擦玻璃的实现下面是一张图，上面也是一张图，并且上下来那张图是同一张图，只是上面这张图需要做成模糊的效果。

这里我们需要考虑一个问题：如何让图片模糊？？

其实这个也比较简单， `&#x9AD8;&#x65AF;&#x6A21;&#x7CCA;`想来大家都知道，至于如何 `&#x9AD8;&#x65AF;&#x6A21;&#x7CCA;`我这边在网上找了一个方法，可以直接使用，具体代码为：

```js
  function gaussBlur(imgData) {
      const pixes = imgData.data;
      const width = imgData.width;
      const height = imgData.height;
      const gaussMatrix = [];
      let gaussSum = 0;
      let x;
      let y;
      let r;
      let g;
      let b;
      let a;
      let i;
      let j;
      let k;
      let len;
      const radius = 10;
      const sigma = 5;
      a = 1 / (Math.sqrt(2 * Math.PI) * sigma);
      b = -1 / (2 * sigma * sigma);

      for (i = 0, x = -radius; x Math.exp(b * x * x);
        gaussMatrix[i] = g;
        gaussSum += g;
      }

      for (i = 0, len = gaussMatrix.length; i < len; i++) {
        gaussMatrix[i] /= gaussSum;
      }

      for (y = 0; y < height; y++) {
        for (x = 0; x < width; x++) {
          r = g = b = a = 0;
          gaussSum = 0;
          for (j = -radius; j if (k >= 0 && k < width) {

              i = (y * width + k) * 4;
              r += pixes[i] * gaussMatrix[j + radius];
              g += pixes[i + 1] * gaussMatrix[j + radius];
              b += pixes[i + 2] * gaussMatrix[j + radius];

              gaussSum += gaussMatrix[j + radius];
            }
          }
          i = (y * width + x) * 4;

          pixes[i] = r / gaussSum;
          pixes[i + 1] = g / gaussSum;
          pixes[i + 2] = b / gaussSum;

        }
      }

      for (x = 0; x < width; x++) {
        for (y = 0; y < height; y++) {
          r = g = b = a = 0;
          gaussSum = 0;
          for (j = -radius; j if (k >= 0 && k < height) {

              i = (k * width + x) * 4;
              r += pixes[i] * gaussMatrix[j + radius];
              g += pixes[i + 1] * gaussMatrix[j + radius];
              b += pixes[i + 2] * gaussMatrix[j + radius];

              gaussSum += gaussMatrix[j + radius];
            }
          }
          i = (y * width + x) * 4;
          pixes[i] = r / gaussSum;
          pixes[i + 1] = g / gaussSum;
          pixes[i + 2] = b / gaussSum;
        }
      }
      return imgData
  }

```

此方法就是把像素数据传进去，然后经过模糊处理以后再返回出来，整体的代码如下：

```html

<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>canvas - 刮刮奖title>
  <style>

    canvas {
      background-image: url("https://gimg2.baidu.com/image_search/src=http%3A%2F%2Ffile.moyublog.com%2Fd%2Ffile%2F2021-05-29%2Ff8b2a20556774afebed8fd91ccbe0497.jpg&refer=http%3A%2F%2Ffile.moyublog.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=auto?sec=1672406341&t=a0b71fded87dd696982c1632cc015397");
      background-size: cover;
      background-position: center;
      box-shadow: 0px 0px 5px #ccc;
      border-radius: 8px;
      float: left;
    }
  style>
head>
<body>
  <canvas id="canvas" width="1000" height="500">
    当前浏览器不支持canvas元素，请升级或更换浏览器！
  canvas>
  <script>

    var canvas = document.getElementById('canvas');

    if(canvas.getContext) {

      var ctx = canvas.getContext('2d');
      const imageUrl = "https://gimg2.baidu.com/image_search/src=http%3A%2F%2Ffile.moyublog.com%2Fd%2Ffile%2F2021-05-29%2Ff8b2a20556774afebed8fd91ccbe0497.jpg&refer=http%3A%2F%2Ffile.moyublog.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=auto?sec=1672406341&t=a0b71fded87dd696982c1632cc015397";

      ctx.lineCap = 'round'
      ctx.lineJoin = 'round'
      ctx.lineWidth = 50

      canvas.addEventListener("mousedown", mousedownFn, false)
      let downX,downY

      function mousedownFn(e) {
        e.preventDefault()
        downX = e.pageX
        downY = e.pageY
        drawLine({startX: downX, startY: downY})

        canvas.addEventListener("mousemove", mousemoveFn, false)
        canvas.addEventListener("mouseup", mouseupFn, false)
      }

      function mousemoveFn(e) {
        e.preventDefault()
        const moveX = e.pageX
        const moveY = e.pageY
        drawLine({endX: moveX, endY: moveY})
        downX = moveX
        downY = moveY
      }

      function mouseupFn() {

        canvas.removeEventListener("mousemove", mousemoveFn, false)
        canvas.removeEventListener("mouseup", mouseupFn, false)
      }

      function drawLine(position) {
        const { startX, startY, endX, endY } = position
        ctx.beginPath()
        ctx.moveTo(startX, startY)
        ctx.lineTo(endX || startX, endY || startY)
        ctx.stroke()
      }

      drawImage(imageUrl)
      function drawImage(src) {
        const img = new Image()
        img.crossOrigin = ''
        img.src = src
        img.onload = () => {
          const imageAspectRatio = img.width / img.height
          const canvasAspectRatio = canvas.width / canvas.height
          ctx.drawImage( img, 0, 0, canvas.width, canvas.height )

          var canvasData = ctx.getImageData(0, 0, canvas.width, canvas.height);
          var tempData = gaussBlur(canvasData, 20);
          ctx.putImageData(tempData,0,0);

          ctx.globalCompositeOperation = 'destination-out'
        }
      }

      function gaussBlur(imgData) {
        const pixes = imgData.data;
        const width = imgData.width;
        const height = imgData.height;
        const gaussMatrix = [];
        let gaussSum = 0;
        let x;
        let y;
        let r;
        let g;
        let b;
        let a;
        let i;
        let j;
        let k;
        let len;
        const radius = 10;
        const sigma = 20;
        a = 1 / (Math.sqrt(2 * Math.PI) * sigma);
        b = -1 / (2 * sigma * sigma);

        for (i = 0, x = -radius; x Math.exp(b * x * x);
          gaussMatrix[i] = g;
          gaussSum += g;
        }

        for (i = 0, len = gaussMatrix.length; i < len; i++) {
          gaussMatrix[i] /= gaussSum;
        }

        for (y = 0; y < height; y++) {
          for (x = 0; x < width; x++) {
            r = g = b = a = 0;
            gaussSum = 0;
            for (j = -radius; j if (k >= 0 && k < width) {

                i = (y * width + k) * 4;
                r += pixes[i] * gaussMatrix[j + radius];
                g += pixes[i + 1] * gaussMatrix[j + radius];
                b += pixes[i + 2] * gaussMatrix[j + radius];

                gaussSum += gaussMatrix[j + radius];
              }
            }
            i = (y * width + x) * 4;

            pixes[i] = r / gaussSum;
            pixes[i + 1] = g / gaussSum;
            pixes[i + 2] = b / gaussSum;

          }
        }

        for (x = 0; x < width; x++) {
          for (y = 0; y < height; y++) {
            r = g = b = a = 0;
            gaussSum = 0;
            for (j = -radius; j if (k >= 0 && k < height) {

                i = (k * width + x) * 4;
                r += pixes[i] * gaussMatrix[j + radius];
                g += pixes[i + 1] * gaussMatrix[j + radius];
                b += pixes[i + 2] * gaussMatrix[j + radius];

                gaussSum += gaussMatrix[j + radius];
              }
            }
            i = (y * width + x) * 4;
            pixes[i] = r / gaussSum;
            pixes[i + 1] = g / gaussSum;
            pixes[i + 2] = b / gaussSum;
          }
        }
        return imgData
      }
    }
  script>
body>
html>
```

具体效果如下：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/39225be083ed48f09fe599003d39bfba~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

`canvas`的应用其实还很多，这里就不做过多的介绍了，希望已有的案例能让大家有所了解，有所收获。

到此咱们的 `canvas`应用也就结束了。

下回见。
