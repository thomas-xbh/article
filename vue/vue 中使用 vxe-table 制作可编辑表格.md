---
link: https://juejin.cn/post/7154999068672917535
title: vue 中使用 vxe-table 制作可编辑表格
description: 持续创作，加速成长！这是我参与「掘金日新计划 · 10 月更文挑战」的第19天，点击查看活动详情 vue 中使用 vxe-table 制作可编辑表格的使用过程 项目上有一个表格需要实现在线编辑，开始用
keywords: 掘金·日新计划
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2022-10-16T06:54:43.000Z
publisher: 稀土掘金
stats: paragraph=45 sentences=28, words=248
---
持续创作，加速成长！这是我参与「掘金日新计划 · 10 月更文挑战」的第19天，[点击查看活动详情](https://juejin.cn/post/7147654075599978532 "https://juejin.cn/post/7147654075599978532")

# vue 中使用 vxe-table 制作可编辑表格的使用过程

项目上有一个表格需要实现在线编辑，开始用了 element 的el-table 实现，单元格内基础情况就是监听了单击单元格切换一个span标签与input标签，复杂点的单元格使用了大量的条件判断来实现对应的编辑操作，比如下拉选，popover弹框编辑。整个表格几十列，十几条数据就已经出现了明显的卡顿，在做了诸多操作（比如el-input使用原生input替换、减少判断、减少频繁的数据切换等）之后，速度虽然有所提升，但是还是肉眼可见的卡顿，基本不可用。然后便转战vxe-table，重写了一遍这个表格。 （别问我为什么不直接用更好的vxe-table，正经人写代码谁会首先想到重构而不是试试优化呢。。。）

下面记录一下使用过程。

1. 全局安装

```js
npm install xe-utils@3 vxe-table@3
```

main.js 中引入

```js
import 'xe-utils';
import VXETable from 'vxe-table';
import 'vxe-table/lib/style.css';

Vue.use(VXETable);
```

其实它可以按需加载来减少项目体积，但是有点麻烦

1. 基础用法

```js

    <vxe-table :align="allAlign" :data="tableData">
        <vxe-table-column type="seq" width="60">vxe-table-column>
        <vxe-table-column field="name" title="名称">vxe-table-column>
        <vxe-table-column field="desc" title="描述">vxe-table-column>
        <vxe-table-column field="link" title="链接">vxe-table-column>
    vxe-table>

<script>
    export default {
        data () {
            return {
                allAlign: null,
                tableData: [
                    {
                        name: "html",
                        desc: '超文本标记语言',
                        link: 'https://www.runoob.com/html/html-tutorial.html'
                    },
                    {
                        name: "css",
                        desc: '层叠样式表',
                        link: 'https://www.runoob.com/css/css-intro.html'
                    },
                    {
                        name: "js",
                        desc: 'JavaScript',
                        link: 'https://www.runoob.com/js/js-tutorial.html'
                    }
                ]
            }
        }
    }
script>
```

以上，即可实现一个基础表格，但是现在仅仅是表格展示，实现编辑还需要另外的配置。

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/06149cd81ab748e88fcd1ca004415576~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

1. 实现编辑

```js

    <vxe-table border :data="tableData" :edit-config="{trigger: 'click', mode: 'cell'}">

        <vxe-table-column title="描述" width="180" fixed="left" field="desc"
                          :edit-render="{name: 'input', attrs: {type: 'text'}}">
        vxe-table-column>
    vxe-table>

```

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/bcbec6b932f24a66b3e1de33721f7ca2~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

具体配置可以查看 [api](https://link.juejin.cn?target=https%3A%2F%2Fxuliangzhan_admin.gitee.io%2Fvxe-table%2F "https://xuliangzhan_admin.gitee.io/vxe-table/")

1. 实现下拉选择

```js

    <vxe-table border :data="tableData" :edit-config="{trigger: 'click', mode: 'cell'}">

        <vxe-table-column title="是否展示" width="180" field="isShow"
                          :edit-render="{name: 'select', options: selection, optionProps:                                         {value: 'status', label: 'label'}}">
        vxe-table-column>
    vxe-table>

<script>
    export default {
        data () {
            return {
                allAlign: null,
                tableData: [
                    {
                        name: "html",
                        desc: '超文本标记语言',
                        link: 'https://www.runoob.com/html/html-tutorial.html',
                        isShow: 1
                    }

                ],
                selection: [
                    {status: 1, label: '是'},
                    {status: 0, label: '否'}
                ]
            }
        }
    }
script>
```

1. 自定义模板

vxe-table自定义模板是使用插槽实现的，可以使用 `<template #插槽名></template>`实现，比如：

```js
"name" width="120" title="名称"
                  :edit-render="{name: 'input', attrs: {type: 'text'}}">

    <template #header>
        <span>名称span>
        <span style="font-size: 12px; color: #ccc">技术span>
    template>

    <template #default="{row}">
        <span>技术名称span>
        <span>{{row.name}}span>
    template>

    <template #edit="{row}">
        <p>技术名称p>
        <input type="text" v-model="row.name" class="vxe-default-input">
    template>
vxe-table-column>
```

需要演示，所以把名称列做成了可编辑列，使用#header、#default、#edit分别自定义了列头、默认显示内容、编辑显示内容，如下图：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/53519d09739d465d8a7c91a910f981a2~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

1. 实时保存功能

使用vxe-table的edit-closed方法监听编辑框关闭，调用更新接口即可实现。

```js

    <vxe-table border :data="tableData" :edit-config="{trigger: 'click', mode: 'cell'}"
               @edit-closed="updateData">
        <vxe-table-column title="是否展示" width="180" field="isShow"
                          :edit-render="{name: 'select', options: selection, optionProps:                                         {value: 'status', label: 'label'}}">
        vxe-table-column>
    vxe-table>

<script>
    export default {
        data () {

        },
        methods: {
            updateData ({ row, column }) {

                console.log(row);
            }
        }
    }
script>
```

其实官方方法还实现了检查当前单元格内容是否更改，不过我们这里数据结构略复杂，源码里的方法不太适用。 这里贴出来一下

```js
editClosedEvent ({ row, column }) {
    const $table = this.$refs.xTable
    const field = column.property
    const cellValue = row[field]

    if ($table.isUpdateByRow(row, field)) {
        setTimeout(() => {
            this.$XModal.message({
                content: `局部保存成功！ ${field}=${cellValue}`,
                status: 'success'
            })

            $table.reloadRow(row, null, field)
        }, 300)
    }
}
```

以上即为实现可编辑表格的基本写法，容我再研究一下如何检测数据很深的情况下如何检测数据是否发生改动。

总结一下，vxe-table的可编辑表格直接内置了可编辑的功能，配置即可用，避免了el-table的各种判断切换，可以更优雅的实现编辑功能，另外它还支持虚拟滚动，在大量数据加载时可以有更好的性能。缺点就是在UI图确定的情况下需要重写表格样式，挺费时间的。

建议各位如果遇到复杂表格的话，就不要自己想着优化性能了，直接使用vxe-table一步到位，后期徒增重构成本，血的教训啊
