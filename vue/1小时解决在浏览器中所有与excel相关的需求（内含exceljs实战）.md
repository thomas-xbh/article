---
link: https://juejin.cn/post/7232524757525659708
title: 1小时解决在浏览器中所有与excel相关的需求（内含exceljs实战）在线编辑excel功能一次完整体验历程，以及可以避免的坑SpreadJS：一款中国研发的类Excel开发工具，功能涵盖Excel的 95% 以上开源界最强类Excel控件——Luckysheethandsontable结合js-xlsx实现可编辑xlsx导入导出功能(参考)
description: Luckyexcel官网: 一个适配 Luckysheet 的excel导入导出库，只支持.xlsx格式文件（不支持.xls） 1. 在HTML中预览xlsx 1.1 预览效果 1.2 代码 2. V
keywords: 前端,Vue.js,Excel
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2023-05-13T07:49:38.000Z
publisher: 稀土掘金
stats: paragraph=39 sentences=116, words=1032
---
[Luckyexcel官网: 一个适配 Luckysheet 的excel导入导出库，只支持.xlsx格式文件（不支持.xls）](https://link.juejin.cn?target=https%3A%2F%2Fgitee.com%2Fmengshukeji%2FLuckyexcel "https://gitee.com/mengshukeji/Luckyexcel")

## 1. 在HTML中预览xlsx

```xml

<script src="https://cdn.jsdelivr.net/npm/luckyexcel/dist/luckyexcel.umd.js">script>
<script>

    LuckyExcel.transformExcelToLucky(
        file,
        function(exportJson, luckysheetfile){

            luckysheet.create({
                container: 'luckysheet',
                data:exportJson.sheets,
                title:exportJson.info.name,
                userInfo:exportJson.info.name.creator
            });
        },
        function(err){
            logger.error('Import failed. Is your fail a valid xlsx?');
        }
    );
script>
```

> UMD 版本可以通过 `<script></code> 标签直接用在浏览器中</p></blockquote><h3>1.1 预览效果</h3><p><img src="https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5573e2616a404010b1838d3a379266f9~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?"></p><h3>1.2 代码</h3><pre><code class="hljs language-xml">
<span class="hljs-tag"><<span class="hljs-name">html</span>></span>
  <span class="hljs-tag"><<span class="hljs-name">head</span>></span>
    <span class="hljs-tag"><<span class="hljs-name">meta</span> <span class="hljs-attr">charset</span>=<span class="hljs-string">"UTF-8"</span> /></span>
    <span class="hljs-tag"><<span class="hljs-name">title</span>></span>Hello xlsx!<span class="hljs-tag"></<span class="hljs-name">title</span>></span>

    <span class="hljs-tag"><<span class="hljs-name">link</span>
      <span class="hljs-attr">rel</span>=<span class="hljs-string">"stylesheet"</span>
      <span class="hljs-attr">href</span>=<span class="hljs-string">"https://cdn.jsdelivr.net/npm/luckysheet/dist/plugins/css/pluginsCss.css"</span>
    /></span>
    <span class="hljs-tag"><<span class="hljs-name">link</span>
      <span class="hljs-attr">rel</span>=<span class="hljs-string">"stylesheet"</span>
      <span class="hljs-attr">href</span>=<span class="hljs-string">"https://cdn.jsdelivr.net/npm/luckysheet/dist/plugins/plugins.css"</span>
    /></span>
    <span class="hljs-tag"><<span class="hljs-name">link</span>
      <span class="hljs-attr">rel</span>=<span class="hljs-string">"stylesheet"</span>
      <span class="hljs-attr">href</span>=<span class="hljs-string">"https://cdn.jsdelivr.net/npm/luckysheet/dist/css/luckysheet.css"</span>
    /></span>
    <span class="hljs-tag"><<span class="hljs-name">link</span>
      <span class="hljs-attr">rel</span>=<span class="hljs-string">"stylesheet"</span>
      <span class="hljs-attr">href</span>=<span class="hljs-string">"https://cdn.jsdelivr.net/npm/luckysheet/dist/assets/iconfont/iconfont.css"</span>
    /></span>
    <span class="hljs-tag"><<span class="hljs-name">script</span> <span class="hljs-attr">src</span>=<span class="hljs-string">"https://cdn.jsdelivr.net/npm/luckysheet/dist/plugins/js/plugin.js"</span>></span><span class="hljs-tag"></<span class="hljs-name">script</span>></span>

    <span class="hljs-tag"><<span class="hljs-name">script</span> <span class="hljs-attr">src</span>=<span class="hljs-string">"https://cdn.jsdelivr.net/npm/luckysheet/dist/luckysheet.umd.js"</span>></span><span class="hljs-tag"></<span class="hljs-name">script</span>></span>

    <span class="hljs-tag"><<span class="hljs-name">script</span>></span><span class="javascript">
      $(<span class="hljs-keyword">function</span> (<span class="hljs-params"></span>) {

        <span class="hljs-keyword">var</span> options = {
          <span class="hljs-attr">container</span>: <span class="hljs-string">'luckysheet'</span>,
          <span class="hljs-attr">showinfobar</span>: <span class="hljs-literal">false</span>
        }
        luckysheet.<span class="hljs-title function_">create</span>(options)
      })
    </span><span class="hljs-tag"></<span class="hljs-name">script</span>></span>
  <span class="hljs-tag"></<span class="hljs-name">head</span>></span>
  <span class="hljs-tag"><<span class="hljs-name">body</span>></span>
    <span class="hljs-tag"><<span class="hljs-name">div</span>
      <span class="hljs-attr">id</span>=<span class="hljs-string">"lucky-mask-demo"</span>
      <span class="hljs-attr">style</span>=<span class="hljs-string">"
        position: absolute;
        z-index: 1000000;
        left: 0px;
        top: 0px;
        bottom: 0px;
        right: 0px;
        background: rgba(255, 255, 255, 0.8);
        text-align: center;
        font-size: 40px;
        align-items: center;
        justify-content: center;
        display: none;
      "</span>
    ></span>
      Downloading
    <span class="hljs-tag"></<span class="hljs-name">div</span>></span>
    <span class="hljs-tag"><<span class="hljs-name">p</span> <span class="hljs-attr">style</span>=<span class="hljs-string">"text-align: center"</span>></span>
      <span class="hljs-tag"><<span class="hljs-name">input</span>
        <span class="hljs-attr">style</span>=<span class="hljs-string">"font-size: 16px"</span>
        <span class="hljs-attr">type</span>=<span class="hljs-string">"file"</span>
        <span class="hljs-attr">id</span>=<span class="hljs-string">"Luckyexcel-demo-file"</span>
        <span class="hljs-attr">name</span>=<span class="hljs-string">"Luckyexcel-demo-file"</span>
        <span class="hljs-attr">change</span>=<span class="hljs-string">"demoHandler"</span>
      /></span>
      Or Load remote xlsx file:
      <span class="hljs-tag"><<span class="hljs-name">select</span>
        <span class="hljs-attr">style</span>=<span class="hljs-string">"height: 27px; top: -2px; position: relative"</span>
        <span class="hljs-attr">id</span>=<span class="hljs-string">"Luckyexcel-select-demo"</span>
      ></span>
        <span class="hljs-tag"><<span class="hljs-name">option</span> <span class="hljs-attr">value</span>=<span class="hljs-string">""</span>></span>select a demo<span class="hljs-tag"></<span class="hljs-name">option</span>></span>
        <span class="hljs-tag"><<span class="hljs-name">option</span>
          <span class="hljs-attr">value</span>=<span class="hljs-string">"https://minio.cnbabylon.com/public/luckysheet/money-manager-2.xlsx"</span>
        ></span>
          Money Manager.xlsx
        <span class="hljs-tag"></<span class="hljs-name">option</span>></span>
        <span class="hljs-tag"><<span class="hljs-name">option</span>
          <span class="hljs-attr">value</span>=<span class="hljs-string">"https://minio.cnbabylon.com/public/luckysheet/Activity%20costs%20tracker.xlsx"</span>
        ></span>
          Activity costs tracker.xlsx
        <span class="hljs-tag"></<span class="hljs-name">option</span>></span>
        <span class="hljs-tag"><<span class="hljs-name">option</span>
          <span class="hljs-attr">value</span>=<span class="hljs-string">"https://minio.cnbabylon.com/public/luckysheet/House%20cleaning%20checklist.xlsx"</span>
        ></span>
          House cleaning checklist.xlsx
        <span class="hljs-tag"></<span class="hljs-name">option</span>></span>
        <span class="hljs-tag"><<span class="hljs-name">option</span>
          <span class="hljs-attr">value</span>=<span class="hljs-string">"https://minio.cnbabylon.com/public/luckysheet/Student%20assignment%20planner.xlsx"</span>
        ></span>
          Student assignment planner.xlsx
        <span class="hljs-tag"></<span class="hljs-name">option</span>></span>
        <span class="hljs-tag"><<span class="hljs-name">option</span>
          <span class="hljs-attr">value</span>=<span class="hljs-string">"https://minio.cnbabylon.com/public/luckysheet/Credit%20card%20tracker.xlsx"</span>
        ></span>
          Credit card tracker.xlsx
        <span class="hljs-tag"></<span class="hljs-name">option</span>></span>
        <span class="hljs-tag"><<span class="hljs-name">option</span>
          <span class="hljs-attr">value</span>=<span class="hljs-string">"https://minio.cnbabylon.com/public/luckysheet/Blue%20timesheet.xlsx"</span>
        ></span>
          Blue timesheet.xlsx
        <span class="hljs-tag"></<span class="hljs-name">option</span>></span>
        <span class="hljs-tag"><<span class="hljs-name">option</span>
          <span class="hljs-attr">value</span>=<span class="hljs-string">"https://minio.cnbabylon.com/public/luckysheet/Student%20calendar%20%28Mon%29.xlsx"</span>
        ></span>
          Student calendar (Mon).xlsx
        <span class="hljs-tag"></<span class="hljs-name">option</span>></span>
        <span class="hljs-tag"><<span class="hljs-name">option</span>
          <span class="hljs-attr">value</span>=<span class="hljs-string">"https://minio.cnbabylon.com/public/luckysheet/Blue%20mileage%20and%20expense%20report.xlsx"</span>
        ></span>
          Blue mileage and expense report.xlsx
        <span class="hljs-tag"></<span class="hljs-name">option</span>></span>
      <span class="hljs-tag"></<span class="hljs-name">select</span>></span>
      <span class="hljs-tag"><<span class="hljs-name">a</span> <span class="hljs-attr">href</span>=<span class="hljs-string">"javascript:void(0)"</span> <span class="hljs-attr">id</span>=<span class="hljs-string">"Luckyexcel-downlod-file"</span>
        ></span>Download source xlsx file</a
      >
    <span class="hljs-tag"></<span class="hljs-name">p</span>></span>
    <span class="hljs-tag"><<span class="hljs-name">div</span>
      <span class="hljs-attr">id</span>=<span class="hljs-string">"luckysheet"</span>
      <span class="hljs-attr">style</span>=<span class="hljs-string">"
        margin: 0px;
        padding: 0px;
        position: absolute;
        width: 100%;
        left: 0px;
        top: 50px;
        bottom: 0px;
        outline: none;
      "</span>
    ></span><span class="hljs-tag"></<span class="hljs-name">div</span>></span>

    <span class="hljs-tag"><<span class="hljs-name">script</span> <span class="hljs-attr">src</span>=<span class="hljs-string">"https://cdn.jsdelivr.net/npm/luckyexcel/dist/luckyexcel.umd.js"</span>></span><span class="hljs-tag"></<span class="hljs-name">script</span>></span>

    <span class="hljs-tag"><<span class="hljs-name">script</span>></span><span class="javascript">
      <span class="hljs-keyword">function</span> <span class="hljs-title function_">demoHandler</span>(<span class="hljs-params"></span>) {
        <span class="hljs-keyword">let</span> upload = <span class="hljs-variable language_">document</span>.<span class="hljs-title function_">getElementById</span>(<span class="hljs-string">'Luckyexcel-demo-file'</span>)
        <span class="hljs-keyword">let</span> selectADemo = <span class="hljs-variable language_">document</span>.<span class="hljs-title function_">getElementById</span>(<span class="hljs-string">'Luckyexcel-select-demo'</span>)
        <span class="hljs-keyword">let</span> downlodDemo = <span class="hljs-variable language_">document</span>.<span class="hljs-title function_">getElementById</span>(<span class="hljs-string">'Luckyexcel-downlod-file'</span>)
        <span class="hljs-keyword">let</span> mask = <span class="hljs-variable language_">document</span>.<span class="hljs-title function_">getElementById</span>(<span class="hljs-string">'lucky-mask-demo'</span>)
        <span class="hljs-keyword">if</span> (upload) {
          <span class="hljs-variable language_">window</span>.<span class="hljs-property">onload</span> = <span class="hljs-function">() =></span> {
            upload.<span class="hljs-title function_">addEventListener</span>(<span class="hljs-string">'change'</span>, <span class="hljs-keyword">function</span> (<span class="hljs-params">evt</span>) {
              <span class="hljs-keyword">var</span> files = evt.<span class="hljs-property">target</span>.<span class="hljs-property">files</span>
              <span class="hljs-keyword">if</span> (files == <span class="hljs-literal">null</span> || files.<span class="hljs-property">length</span> == <span class="hljs-number">0</span>) {
                <span class="hljs-title function_">alert</span>(<span class="hljs-string">'No files wait for import'</span>)
                <span class="hljs-keyword">return</span>
              }

              <span class="hljs-keyword">let</span> name = files[<span class="hljs-number">0</span>].<span class="hljs-property">name</span>
              <span class="hljs-keyword">let</span> suffixArr = name.<span class="hljs-title function_">split</span>(<span class="hljs-string">'.'</span>),
                suffix = suffixArr[suffixArr.<span class="hljs-property">length</span> - <span class="hljs-number">1</span>]
              <span class="hljs-keyword">if</span> (suffix != <span class="hljs-string">'xlsx'</span>) {
                <span class="hljs-title function_">alert</span>(<span class="hljs-string">'Currently only supports the import of xlsx files'</span>)
                <span class="hljs-keyword">return</span>
              }
              <span class="hljs-title class_">LuckyExcel</span>.<span class="hljs-title function_">transformExcelToLucky</span>(
                files[<span class="hljs-number">0</span>],
                <span class="hljs-keyword">function</span> (<span class="hljs-params">exportJson, luckysheetfile</span>) {
                  <span class="hljs-keyword">if</span> (
                    exportJson.<span class="hljs-property">sheets</span> == <span class="hljs-literal">null</span> ||
                    exportJson.<span class="hljs-property">sheets</span>.<span class="hljs-property">length</span> == <span class="hljs-number">0</span>
                  ) {
                    <span class="hljs-title function_">alert</span>(
                      <span class="hljs-string">'Failed to read the content of the excel file, currently does not support xls files!'</span>
                    )
                    <span class="hljs-keyword">return</span>
                  }
                  <span class="hljs-variable language_">window</span>.<span class="hljs-property">luckysheet</span>.<span class="hljs-title function_">destroy</span>()

                  <span class="hljs-variable language_">window</span>.<span class="hljs-property">luckysheet</span>.<span class="hljs-title function_">create</span>({
                    <span class="hljs-attr">container</span>: <span class="hljs-string">'luckysheet'</span>,
                    <span class="hljs-attr">showinfobar</span>: <span class="hljs-literal">false</span>,
                    <span class="hljs-attr">data</span>: exportJson.<span class="hljs-property">sheets</span>,
                    <span class="hljs-attr">title</span>: exportJson.<span class="hljs-property">info</span>.<span class="hljs-property">name</span>,
                    <span class="hljs-attr">userInfo</span>: exportJson.<span class="hljs-property">info</span>.<span class="hljs-property">name</span>.<span class="hljs-property">creator</span>
                  })
                }
              )
            })

            selectADemo.<span class="hljs-title function_">addEventListener</span>(<span class="hljs-string">'change'</span>, <span class="hljs-keyword">function</span> (<span class="hljs-params">evt</span>) {
              <span class="hljs-keyword">var</span> obj = selectADemo
              <span class="hljs-keyword">var</span> index = obj.<span class="hljs-property">selectedIndex</span>
              <span class="hljs-keyword">var</span> value = obj.<span class="hljs-property">options</span>[index].<span class="hljs-property">value</span>
              <span class="hljs-keyword">var</span> name = obj.<span class="hljs-property">options</span>[index].<span class="hljs-property">innerHTML</span>
              <span class="hljs-keyword">if</span> (value == <span class="hljs-string">''</span>) {
                <span class="hljs-keyword">return</span>
              }
              mask.<span class="hljs-property">style</span>.<span class="hljs-property">display</span> = <span class="hljs-string">'flex'</span>
              <span class="hljs-title class_">LuckyExcel</span>.<span class="hljs-title function_">transformExcelToLuckyByUrl</span>(
                value,
                name,
                <span class="hljs-keyword">function</span> (<span class="hljs-params">exportJson, luckysheetfile</span>) {
                  <span class="hljs-keyword">if</span> (
                    exportJson.<span class="hljs-property">sheets</span> == <span class="hljs-literal">null</span> ||
                    exportJson.<span class="hljs-property">sheets</span>.<span class="hljs-property">length</span> == <span class="hljs-number">0</span>
                  ) {
                    <span class="hljs-title function_">alert</span>(
                      <span class="hljs-string">'Failed to read the content of the excel file, currently does not support xls files!'</span>
                    )
                    <span class="hljs-keyword">return</span>
                  }
                  <span class="hljs-variable language_">console</span>.<span class="hljs-title function_">log</span>(exportJson, luckysheetfile)
                  mask.<span class="hljs-property">style</span>.<span class="hljs-property">display</span> = <span class="hljs-string">'none'</span>
                  <span class="hljs-variable language_">window</span>.<span class="hljs-property">luckysheet</span>.<span class="hljs-title function_">destroy</span>()

                  <span class="hljs-variable language_">window</span>.<span class="hljs-property">luckysheet</span>.<span class="hljs-title function_">create</span>({
                    <span class="hljs-attr">container</span>: <span class="hljs-string">'luckysheet'</span>,
                    <span class="hljs-attr">showinfobar</span>: <span class="hljs-literal">false</span>,
                    <span class="hljs-attr">data</span>: exportJson.<span class="hljs-property">sheets</span>,
                    <span class="hljs-attr">title</span>: exportJson.<span class="hljs-property">info</span>.<span class="hljs-property">name</span>,
                    <span class="hljs-attr">userInfo</span>: exportJson.<span class="hljs-property">info</span>.<span class="hljs-property">name</span>.<span class="hljs-property">creator</span>
                  })
                }
              )
            })

            downlodDemo.<span class="hljs-title function_">addEventListener</span>(<span class="hljs-string">'click'</span>, <span class="hljs-keyword">function</span> (<span class="hljs-params">evt</span>) {
              <span class="hljs-keyword">var</span> obj = selectADemo
              <span class="hljs-keyword">var</span> index = obj.<span class="hljs-property">selectedIndex</span>
              <span class="hljs-keyword">var</span> value = obj.<span class="hljs-property">options</span>[index].<span class="hljs-property">value</span>

              <span class="hljs-keyword">if</span> (value.<span class="hljs-property">length</span> == <span class="hljs-number">0</span>) {
                <span class="hljs-title function_">alert</span>(<span class="hljs-string">'Please select a demo file'</span>)
                <span class="hljs-keyword">return</span>
              }

              <span class="hljs-keyword">var</span> elemIF = <span class="hljs-variable language_">document</span>.<span class="hljs-title function_">getElementById</span>(<span class="hljs-string">'Lucky-download-frame'</span>)
              <span class="hljs-keyword">if</span> (elemIF == <span class="hljs-literal">null</span>) {
                elemIF = <span class="hljs-variable language_">document</span>.<span class="hljs-title function_">createElement</span>(<span class="hljs-string">'iframe'</span>)
                elemIF.<span class="hljs-property">style</span>.<span class="hljs-property">display</span> = <span class="hljs-string">'none'</span>
                elemIF.<span class="hljs-property">id</span> = <span class="hljs-string">'Lucky-download-frame'</span>
                <span class="hljs-variable language_">document</span>.<span class="hljs-property">body</span>.<span class="hljs-title function_">appendChild</span>(elemIF)
              }
              elemIF.<span class="hljs-property">src</span> = value
            })
          }
        }
      }
      <span class="hljs-title function_">demoHandler</span>()
    </span><span class="hljs-tag"></<span class="hljs-name">script</span>></span>
  <span class="hljs-tag"></<span class="hljs-name">body</span>></span>
<span class="hljs-tag"></<span class="hljs-name">html</span>></span>

</code></pre><h2>2. Vue项目 引入LuckySheet</h2><ul><li>1、先从官网下载：<a href="https://link.juejin.cn?target=https%3A%2F%2Fgitee.com%2Fmengshukeji%2FLuckysheet%23https%3A%2F%2Fmengshukeji.github.io%2FLuckysheetDemo" title="https://gitee.com/mengshukeji/Luckysheet#https://mengshukeji.github.io/LuckysheetDemo">LuckySheet源码</a>，npm i 依赖安装</li><li>2、再使用 npm run build，将LuckySheet源码进行打包，打包完之后会出现dist文件</li><li>3、打包好的 <strong>dist 文件夹下的文件</strong>移动到Vue项目的public 文件下（也就是跟index.html文件同目录下）</li></ul><p><img src="https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d3d62cff37cc433e9e111231383927b7~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?"></p><h3>2.1. 在public/index.html文件中引入js</h3><pre><code class="hljs language-ini"><link <span class="hljs-attr">rel</span>=<span class="hljs-string">'stylesheet'</span> href=<span class="hljs-string">'./plugins/css/pluginsCss.css'</span> />
<link <span class="hljs-attr">rel</span>=<span class="hljs-string">'stylesheet'</span> href=<span class="hljs-string">'./plugins/plugins.css'</span> />
<link <span class="hljs-attr">rel</span>=<span class="hljs-string">'stylesheet'</span> href=<span class="hljs-string">'./css/luckysheet.css'</span> />
<link <span class="hljs-attr">rel</span>=<span class="hljs-string">'stylesheet'</span> href=<span class="hljs-string">'./assets/iconfont/iconfont.css'</span> />
<script <span class="hljs-attr">src</span>=<span class="hljs-string">"./plugins/js/plugin.js"</span>></script>
<script <span class="hljs-attr">src</span>=<span class="hljs-string">"./luckysheet.umd.js"</span>></script>
`

```xml

<html lang="">
  <head>
    <meta charset="utf-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width,initial-scale=1.0" />
    <link rel="icon" href="favicon.ico" />
    <title>title>

    <link rel="stylesheet" href="./luckysheet/plugins/css/pluginsCss.css" />
    <link rel="stylesheet" href="./luckysheet/plugins/plugins.css" />
    <link rel="stylesheet" href="./luckysheet/css/luckysheet.css" />
    <link rel="stylesheet" href="./luckysheet/assets/iconfont/iconfont.css" />
    <script src="./luckysheet/plugins/js/plugin.js">script>
    <script src="./luckysheet/luckysheet.umd.js">script>
  head>
  <body>
    <noscript>
      <strong
        >We're sorry but  doesn't work
        properly without JavaScript enabled. Please enable it to
        continue.

    noscript>
    <div id="app">div>

  body>
html>
```

### 2.2 在Vue组件中使用

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/aadc493d63d944a9be223462b344b32a~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

```xml
<template>
  <div class="hello">
    <el-upload class="upload-demo" action="" :before-upload="beforeUpload">
      <i class="el-icon-upload">i><em>点击上传em>
      <div class="el-upload__tip" slot="tip">只能上传xlsx文件，div>
    el-upload>
    <div id="luckysheet" class="luckysheet-content">div>
  div>
template>

<script>
import LuckyExcel from 'luckyexcel'
export default {
  name: 'HelloWorld',
  data() {
    return {}
  },
  mounted() {},
  methods: {
    beforeUpload(file) {
      this.init(file)
    },
    init(file) {
      LuckyExcel.transformExcelToLucky(
        file,
        function (exportJson, luckysheetfile) {

          console.log(exportJson)
          luckysheet.create({
            container: 'luckysheet',
            data: exportJson.sheets,
            title: exportJson.info.name,
            userInfo: exportJson.info.name.creator,
            lang: 'zh',
            showinfobar: false,
            showtoolbar: false,
            sheetFormulaBar: false
          })

        },
        function (err) {
          logger.error('Import failed. Is your fail a valid xlsx?')
        }
      )
    },

    createExcel() {
      LuckyExcel.create({
        container: 'luckysheet',
        title: 'Luckysheet Demo',
        lang: 'zh',
        plugins: ['chart'],
        data: [
          {
            name: '表1',
            color: '#eee333',
            index: 0,
            status: 1,
            order: 0,
            hide: 0,
            row: 36,
            column: 10,
            defaultRowHeight: 28,
            defaultColWidth: 100,
            celldata: [],
            config: {
              merge: {},
              rowlen: {},
              columnlen: {},
              rowhidden: {},
              colhidden: {},
              borderInfo: {},
              authority: {}
            },

            scrollLeft: 0,
            scrollTop: 0,
            luckysheet_select_save: [],
            calcChain: [],
            isPivotTable: false,
            pivotTable: {},
            filter_select: {},
            filter: null,
            luckysheet_alternateformat_save: [],
            luckysheet_alternateformat_save_modelCustom: [],
            luckysheet_conditionformat_save: {},
            frozen: {},
            chart: [],
            zoomRatio: 1,
            image: [],
            showGridLines: 1,
            dataVerification: {}
          }
        ],
        showtoolbar: false,
        showtoolbarConfig: {
          undoRedo: false,
          paintFormat: false,
          currencyFormat: false,
          percentageFormat: false,
          numberDecrease: false,
          numberIncrease: false,
          moreFormats: false,
          font: false,
          fontSize: false,
          bold: false,
          italic: false,
          strikethrough: false,
          underline: false,
          textColor: false,
          fillColor: false,
          border: false,
          mergeCell: false,
          horizontalAlignMode: false,
          verticalAlignMode: false,
          textWrapMode: false,
          textRotateMode: false,
          image: false,
          link: false,
          chart: false,
          postil: false,
          pivotTable: false,
          function: false,
          frozenMode: false,
          sortAndFilter: false,
          conditionalFormat: false,
          dataVerification: false,
          splitColumn: false,
          screenshot: false,
          findAndReplace: false,
          protection: false,
          print: false
        },
        showsheetbar: false,
        showsheetbarConfig: {
          add: false,
          menu: false,
          sheet: false
        },
        showinfobar: false,
        showstatisticBar: false,
        showstatisticBarConfig: {
          count: false,
          view: false,
          zoom: false
        },
        sheetFormulaBar: false,
        allowCopy: false,
        enableAddRow: true
      })
    }
  }
}
script>
<style lang="css" scoped>
.luckysheet-content {
  margin: 0px;
  padding: 0px;
  position: absolute;
  width: 100%;
  height: 500px;
  left: 0px;
  top: 40px;
  bottom: 0px;
}
style>

```
