---
link: https://www.51cto.com/article/707105.html
title: 一文彻底搞懂前端实现文件预览（word、excel、pdf、ppt、mp4、图片、文本）-前端实现图片预览
description: 主要介绍了w本文ord、excel、pdf文件实现预览的方式，前端实现预览最好的效果还是PDF，不会出现一些文字错乱和乱码的问题。
keywords: 前端,文件预览
author: null
date: 2022-04-21T06:29:00.000Z
publisher: null
stats: paragraph=50 sentences=60, words=313
---
![](https://s8.51cto.com/oss/202204/21/b7b8f2d859970049d8d530a3c6d2d557ea1951.jpg)

### 前言

因为业务需要，很多文件需要在前端实现预览，今天就来了解一下吧。

Demo地址：https://zhuye1993.github.io/file-view/dist/index.html

### 实现方案

找了网上的实现方案，效果看起来不错，放在下面的表格里，里面有一些是可以直接通过npm在vue中引入使用。

文档格式

老的开源组件

替代开源组件

word（docx）

mammoth

docx-preview(npm)

powerpoint（pptx）

pptxjs

pptxjs改造开发

excel（xlsx）

sheetjs、handsontable

exceljs(npm)、handsontable(npm)(npm)

pdf（pdf）

pdfjs

pdfjs(npm)

图片

jquery.verySimpleImageViewer

v-viewer(npm)

### docx文件实现前端预览

#### 代码实现

* 首先npm i docx-preview
* 引入renderAsync方法
* 将blob数据流传入方法中，渲染word文档

```
import { defaultOptions, renderAsync } from "docx-preview";renderAsync(buffer, document.getElementById("container"), null,options: {       className: string = "docx",        inWrapper: boolean = true,        ignoreWidth: boolean = false,        ignoreHeight: boolean = false,        ignoreFonts: boolean = false,        breakPages: boolean = true,        ignoreLastRenderedPageBreak: boolean = true,       experimental: boolean = false,        trimXmlDeclaration: boolean = true,        debug: boolean = false,    });
```

实现效果：

![](https://s4.51cto.com/oss/202204/21/995245900fb4aaf89446525854ee64ac3eac72.jpg)

### pdf实现前端预览

#### 代码实现

* 首先npm i pdfjs-dist
* 设置PDFJS.GlobalWorkerOptions.workerSrc的地址
* 通过PDFJS.getDocument处理pdf数据，返回一个对象pdfDoc
* 通过pdfDoc.getPage单独获取第1页的数据
* 创建一个dom元素，设置元素的画布属性
* 通过page.render方法，将数据渲染到画布上

```
import * as PDFJS from "pdfjs-dist/legacy/build/pdf";PDFJS.GlobalWorkerOptions.workerSrc = require("pdfjs-dist/legacy/build/pdf.worker.entry.js");PDFJS.getDocument(data).promise.then(pdfDoc=>{    const numPages = pdfDoc.numPages;         pdfDoc.getPage(1).then(page =>{          const canvas = document.getElementById("the_canvas");     const ctx = canvas.getContext("2d");     const dpr = window.devicePixelRatio || 1;     const bsr =       ctx.webkitBackingStorePixelRatio ||       ctx.mozBackingStorePixelRatio ||       ctx.msBackingStorePixelRatio ||       ctx.oBackingStorePixelRatio ||       ctx.backingStorePixelRatio ||       1;     const ratio = dpr / bsr;     const viewport = page.getViewport({ scale: 1 });     canvas.width = viewport.width * ratio;     canvas.height = viewport.height * ratio;     canvas.style.width = viewport.width + "px";     canvas.style.height = viewport.height + "px";     ctx.setTransform(ratio, 0, 0, ratio, 0, 0);     const renderContext = {       canvasContext: ctx,       viewport: viewport,     };          page.render(renderContext);    })})
```

实现效果：

![](https://s8.51cto.com/oss/202204/21/14328c20178df66d249021db949cafa9c1d557.jpg)

### excel实现前端预览

#### 代码实现

* 下载exceljs、handsontable的库
* 通过exceljs读取到文件的数据
* 通过workbook.getWorksheet方法获取到每一个工作表的数据，将数据处理成一个二维数组的数据
* 引入@handsontable/vue的组件HotTable
* 通过settings属性，将一些配置参数和二维数组数据传入组件，渲染成excel样式，实现预览

<pre><br><span>&#xFF08;new</span> <span>ExcelJS</span>.<span>Workbook</span>().<span>xlsx</span>.<span>load</span>(<span>buffer</span>)).<span>then</span>(<span>workbook</span><span>=&gt;</span>{<br>   <br>   <span>const</span> <span>ws</span> <span>=</span> <span>workbook</span>.<span>getWorksheet</span>(<span>1</span>);<br>   <br>   <span>const</span> <span>data</span> <span>=</span> <span>ws</span>.<span>getRows</span>(<span>1</span>, <span>ws</span>.<span>actualRowCount</span>);<br> })<br><br><span>import</span> { <span>HotTable</span> } <span>from</span> <span>&quot;@handsontable/vue&quot;</span>;<br><span>&lt;</span><span>hot</span><span>-</span><span>table</span>  :<span>settings</span><span>=</span><span>&quot;hotSettings&quot;</span><span>&gt;</span><span>&lt;</span><span>/hot-table&gt;</span><br><span>hotSettings</span> <span>=</span> {<br>   <span>language</span>: <span>&quot;zh-CN&quot;</span>,<br>   <span>readOnly</span>: <span>true</span>,<br>   <span>data</span>: <span>this</span>.<span>data</span>,<br>   <span>cell</span>: <span>this</span>.<span>cell</span>,<br>   <span>mergeCells</span>: <span>this</span>.<span>merge</span>,<br>   <span>colHeaders</span>: <span>true</span>,<br>   <span>rowHeaders</span>: <span>true</span>,<br>   <span>height</span>: <span>&quot;calc(100vh - 107px)&quot;</span>,<br>   <br>   <br>   <br>   <span>outsideClickDeselects</span>: <span>false</span>,<br>   <br>   <br>   <br>   <br>   <br>   <br>   <span>licenseKey</span>: <span>&quot;non-commercial-and-evaluation&quot;</span><br>}</pre>

#### 实现效果

![](https://s2.51cto.com/oss/202204/21/b76332805fb676871af402f8f27806970667f8.jpg)

### pptx的前端预览

主要是通过jszip库，加载二进制文件，再经过一些列处理处理转换实现预览效果，实现起来比较麻烦，就不贴代码了，感兴趣的可以下载代码查看。

#### 实现效果

![](https://s4.51cto.com/oss/202204/21/d19632d58d65f9ec852936c12c24473cd2ff7c.jpg)

### 总结

主要介绍了word、excel、pdf文件实现预览的方式，前端实现预览最好的效果还是PDF，不会出现一些文字错乱和乱码的问题，所以一般好的方案就是后端配合将不同格式的文件转换成pdf，再由前端实现预览效果，将会保留文件的一些样式的效果，对于图片、txt文件的实现，感兴趣的可以看下代码。

代码地址

​[​https://github.com/zhuye1993/file-view​](https://github.com/zhuye1993/file-view)​
