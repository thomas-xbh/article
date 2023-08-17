---
link: https://juejin.cn/post/7166068828202336263
title: Vue3.2 + Element-Plus 二次封装 el-table（Pro版🚀🚀）
description: 前言 📖 一、在线预览 👀 Link：https://admin.spicyboy.cn 二、Git 仓库地址 (欢迎 Star⭐⭐⭐) Gitee：https://gitee.com/laramie
keywords: 前端,Vue.js
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2022-11-15T02:47:06.000Z
publisher: 稀土掘金
stats: paragraph=226 sentences=562, words=3539
---
## 前言 📖

> ProTable 组件目前已是 `2.0&#x7248;&#x672C;`🌈，在 [1.0版本](https://juejin.cn/post/7094890833064755208 "https://juejin.cn/post/7094890833064755208") 中大家提出的问题与功能优化，目前已经得到优化和解决。

> 😀 欢迎大家在使用过程中发现任何问题或更好的想法，都可以在下方评论区留言，或者我的开源项目 issues 中提出。如果你觉得还不错，请帮我点个小小的 **Star** 🧡

## 一、在线预览 👀

## 二、Git 仓库地址 (欢迎 Star⭐⭐⭐)

## 三、ProTable 功能 🚀🚀🚀

> **ProTable** 组件目前使用属性透传进行重构，支持 **el-table && el-table-column** 所有属性、事件、方法的调用，不会有任何心智负担。

* 表格内容自适应屏幕宽高，溢出内容表格内部滚动（flex 布局）
* 表格搜索、重置、分页查询 Hooks 封装 （页面使用不会存在任何搜索、重置、分页查询逻辑）
* 表格数据操作 Hooks 封装 （单条数据删除、批量删除、重置密码、状态切换等操作）
* 表格数据多选 Hooks 封装 （支持现跨页勾选数据）
* 表格数据导入组件、导出 Hooks 封装
* 表格搜索区域使用 Grid 布局重构，支持自定义响应式配置
* 表格分页组件封装（Pagination）
* 表格数据刷新、列显隐、列排序、搜索区域显隐设置
* 表格数据打印功能（可勾选行数据、隐藏列打印）
* 表格配置支持多级 prop（示例 ==> prop: user.detail.name）
* 单元格内容格式化、tag 标签显示（有字典 enum 会根据字典 enum 自动格式化）
* 支持多级表头、表头内容自定义渲染（支持作用域插槽、tsx 语法、h 函数）
* 支持单元格内容自定义渲染（支持作用域插槽、tsx 语法、h 函数）
* 配合 TreeFilter、SelectFilter 组件使用更佳（项目中有使用示例）

## 四、ProTable 功能需求分析 📑

### 首先我们来看效果图（总共可以分为五个模块）：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/da2c1b029cdf4d31afa6ba72bd4c7ed5~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

* 1、表格搜索区域
* 2、表格数据操作按钮区域
* 3、表格功能按钮区域
* 4、表格主体内容展示区域
* 5、表格分页区域

### 1、表格搜索区域需求分析：

> 可以看到搜索区域的字段都是存在于表格当中的，并且每个页面的搜索、重置方法都是一样的逻辑，只是不同的查询参数而已。我们完全可以在传表格配置项 **columns** 时，直接指定某个 **column** 的 **search** 配置，就能把该项变为搜索项，然后使用 **el** 字段可以指定搜索框的类型，最后把表格的搜索方法都封装成 **Hooks** 钩子函数。页面上完全就不会存在任何搜索、重置逻辑了。

> 在 **1.0** 版本中使用 **v-if** 判断太麻烦，为了更方便用户传递参数，搜索组件在 **2.0** 版本中通过 **component :is** 动态组件 && **v-bind** 属性透传实现，将用户传递的参数全部透传到组件上，所以大家可以直接根据 **element** 官方文档在 **props** 中传递参数了。以下代码还结合了自己逻辑上的一些处理：

```html
<template>
  <component
    :is="column.search?.render ?? `el-${column.search?.el}`"
    v-bind="{ ...handleSearchProps, ...placeholder, searchParam, clearable }"
    v-model.trim="searchParam[column.search?.key ?? handleProp(column.prop!)]"
    :data="column.search?.el === 'tree-select' ? columnEnum : []"
    :options="['cascader', 'select-v2'].includes(column.search?.el!) ? columnEnum : []"
  >
    <template #default="{ data }" v-if="column.search?.el === 'cascader'">
      <span>{{ data[fieldNames.label] }}span>
    template>
    <template v-if="column.search?.el === 'select'">
      <component
        :is="`el-option`"
        v-for="(col, index) in columnEnum"
        :key="index"
        :label="col[fieldNames.label]"
        :value="col[fieldNames.value]"
      >component>
    template>
    <slot v-else>slot>
  component>
template>

<script setup lang="ts" name="SearchFormItem">
import { computed, inject, ref } from "vue";
import { handleProp } from "@/utils";
import { ColumnProps } from "@/components/ProTable/interface";

interface SearchFormItem {
  column: ColumnProps;
  searchParam: { [key: string]: any };
}
const props = defineProps<SearchFormItem>();

const fieldNames = computed(() => {
  return {
    label: props.column.fieldNames?.label ?? "label",
    value: props.column.fieldNames?.value ?? "value",
    children: props.column.fieldNames?.children ?? "children"
  };
});

const enumMap = inject("enumMap", ref(new Map()));
const columnEnum = computed(() => {
  let enumData = enumMap.value.get(props.column.prop);
  if (!enumData) return [];
  if (props.column.search?.el === "select-v2" && props.column.fieldNames) {
    enumData = enumData.map((item: { [key: string]: any }) => {
      return { ...item, label: item[fieldNames.value.label], value: item[fieldNames.value.value] };
    });
  }
  return enumData;
});

const handleSearchProps = computed(() => {
  const label = fieldNames.value.label;
  const value = fieldNames.value.value;
  const children = fieldNames.value.children;
  const searchEl = props.column.search?.el;
  let searchProps = props.column.search?.props ?? {};
  if (searchEl === "tree-select") {
    searchProps = { ...searchProps, props: { ...searchProps.props, label, children }, nodeKey: value };
  }
  if (searchEl === "cascader") {
    searchProps = { ...searchProps, props: { ...searchProps.props, label, value, children } };
  }
  return searchProps;
});

const placeholder = computed(() => {
  const search = props.column.search;
  if (["datetimerange", "daterange", "monthrange"].includes(search?.props?.type) || search?.props?.isRange) {
    return { rangeSeparator: "至", startPlaceholder: "开始时间", endPlaceholder: "结束时间" };
  }
  const placeholder = search?.props?.placeholder ?? (search?.el?.includes("input") ? "请输入" : "请选择");
  return { placeholder };
});

const clearable = computed(() => {
  const search = props.column.search;
  return search?.props?.clearable ?? (search?.defaultValue == null || search?.defaultValue == undefined);
});
script>

```

> **表格搜索项可使用 tsx 组件自定义渲染**

```html
<script setup lang="tsx">
const columns: ColumnProps[] = [
   {
    prop: "user.detail.age",
    label: "年龄",
    search: {

      render: ({ searchParam }) => {
        return (
          <div class="flx-center">
            <el-input vModel_trim={searchParam.minAge} placeholder="最小年龄" style={{ width: "50%" }} />
            <span class="mr10 ml10">-span>
            <el-input vModel_trim={searchParam.maxAge} placeholder="最大年龄" style={{ width: "50%" }} />
          div>
        );
      }
    }
  },
];
script>
```

> 表格搜索组件在 **2.0** 版本中还支持了响应式配置，使用 **Grid** 方法进行整体重构 😋。

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c415a959108e4af3be10d90a5b85b92c~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

### 2、表格数据操作按钮区域需求分析：

> 表格数据操作按钮基本上每个页面都会不一样，所以我们直接使用 **作用域插槽** 来完成每个页面的数据操作按钮区域， **作用域插槽** 可以将表格多选数据信息从 **ProTable** 的 **Hooks** 多选钩子函数中传到页面上使用。

> **scope** 数据中包含： **selectedList（当前选择的数据）、selectedListIds（当前选择的数据id）、isSelected（当前是否选中的数据）**

```html

<slot name="tableHeader" :selectList="selectedList" :selectedListIds="selectedListIds" :isSelected="isSelected">slot>

<template #tableHeader="scope">
    <el-button type="primary" :icon="CirclePlus" @click="openDrawer('新增')">新增用户el-button>
    <el-button type="primary" :icon="Upload" plain @click="batchAdd">批量添加用户el-button>
    <el-button type="primary" :icon="Download" plain @click="downloadFile">导出用户数据el-button>
    <el-button type="danger" :icon="Delete" plain @click="batchDelete(scope.selectedListIds)" :disabled="!scope.isSelected">批量删除用户el-button>
template>

```

### 3、表格功能按钮区域分析：

> 这块区域没什么特殊功能，只有四个按钮，其功能分别为： **表格数据刷新（一直会携带当前查询和分页条件）、表格数据打印、表格列设置（列显隐、列排序）、表格搜索区域显隐（方便展示更多的数据信息）**。 可通过 **toolButton** 属性控制这块区域的显隐。

> 表格打印功能基于 **PrintJs** 实现，因 **PrintJs** 不支持多级表头打印，所以当页面存在多级表头时，只会打印最后一级表头。表格打印功能可根据显示的列和勾选的数据动态打印，默认打印当前显示的所有数据。

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5cbc2c00f03d444fbf1c205365f3deff~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

### 4、表格主体内容展示区域分析：

> 🍉 该区域是最重要的数据展示区域，对于使用最多的功能就是表头和单元格内容可以自定义渲染，在第 **1.0** 版本中，自定义表头只支持传入 `renderHeader`方法，自定义单元格内容只支持 `slot`插槽。

> 💥 目前 **2.0** 版本中，表头支持 `headerRender`方法（避免与 **el-table-column** 上的属性重名导致报错）、作用域插槽（ `column.prop + 'Header'`）两种方式自定义，单元格内容支持 `render`方法和作用域插槽（ `column &#x4E0A;&#x7684; prop &#x5C5E;&#x6027;`）两种方式自定义。

* 使用作用域插槽：

```html

<template #username="scope">
    {{ scope.row.username }}
template>

<template #usernameHeader="scope">
    <el-button type="primary" @click="ElMessage.success('我是通过作用域插槽渲染的表头')">
        {{ scope.column.label }}
    el-button>
template>
```

* 使用 tsx 语法：

```html
<script setup lang="tsx">
const columns: ColumnProps[] = [
 {
    prop: "username",
    label: "用户姓名",

    headerRender: scope => {
      return (
        <el-button
          type="primary"
          onClick={() => {
            ElMessage.success("我是通过 tsx 语法渲染的表头");
          }}
        >
          {scope.column.label}
        el-button>
      );
    }
  },
  {
    prop: "status",
    label: "用户状态",

    render: scope => {
      return (
          <el-switch
            model-value={scope.row.status}
            active-text={scope.row.status ? "启用" : "禁用"}
            active-value={1}
            inactive-value={0}
            onClick={() => changeStatus(scope.row)}
          />
        )
      );
    }
  },
];
script>
```

> 💢💢💢 **最强大的功能：如果你想使用 `el-table` 的任何属性、事件，目前通过属性透传都能支持。**
**如果你还不了解属性透传，请阅读 vue 官方文档：[cn.vuejs.org/guide/compo...](https://link.juejin.cn?target=https%3A%2F%2Fcn.vuejs.org%2Fguide%2Fcomponents%2Fattrs.html "https://cn.vuejs.org/guide/components/attrs.html")**

* **ProTable 组件上的绑定的所有属性和事件都会通过 `v-bind="$attrs"` 透传到 el-table 上。**
* **ProTable 组件内部暴露了 el-table DOM，可通过 `proTable.value.element.&#x65B9;&#x6CD5;&#x540D;` 调用其方法。*

```html
<template>
    <el-table
      ref="tableRef"
      v-bind="$attrs"
    >
    el-table>
template>

<script setup lang="ts" name="ProTable">
import { ref } from "vue";
import { ElTable } from "element-plus";

const tableRef = ref<InstanceType<typeof ElTable>>();

defineExpose({ element: tableRef });
script>
```

### 5、表格分页区域分析：

> 分页区域也没有什么特殊的功能，该支持的都支持了🤣（页面上使用 ProTable 组件完全不存在分页逻辑）

```html
<template>

  <el-pagination
    :background="true"
    :current-page="pageable.pageNum"
    :page-size="pageable.pageSize"
    :page-sizes="[10, 25, 50, 100]"
    :total="pageable.total"
    layout="total, sizes, prev, pager, next, jumper"
    @size-change="handleSizeChange"
    @current-change="handleCurrentChange"
  >el-pagination>
template>

<script setup lang="ts" name="Pagination">
interface Pageable {
  pageNum: number;
  pageSize: number;
  total: number;
}

interface PaginationProps {
  pageable: Pageable;
  handleSizeChange: (size: number) => void;
  handleCurrentChange: (currentPage: number) => void;
}

defineProps<PaginationProps>();
script>

```

## 五、ProTable 文档 📚

### 1、ProTable 属性（ProTableProps）：

> 使用 `v-bind="$attrs"` 通过属性透传将 **ProTable** 组件属性全部透传到 **el-table** 上，所以我们支持 **el-table** 的所有 **Props** 属性。在此基础上，还扩展了以下 **Props：**
[el-table 属性文档链接](https://link.juejin.cn?target=https%3A%2F%2Felement-plus.org%2Fzh-CN%2Fcomponent%2Ftable.html%23table-%25E5%25B1%259E%25E6%2580%25A7 "https://element-plus.org/zh-CN/component/table.html#table-%E5%B1%9E%E6%80%A7")

属性名类型是否必传默认值属性描述columnsColumnProps✅—ProTable 组件会根据此字段渲染搜索表单与表格列，详情见 ColumnPropsdataArray❌—静态 table data 数据，若存在则不会使用 requestApi 返回的 datarequestApiFunction❌—获取表格数据的请求 APIrequestAutoBoolean❌true表格初始化是否自动执行请求 APIrequestErrorFunction❌—表格 API 请求错误监听dataCallbackFunction❌—后台返回数据的回调函数，可对后台返回数据进行处理titleString❌—表格标题，目前只在打印的时候用到paginationBoolean❌true是否显示分页组件：pagination 为 false 后台返回数据应该没有分页信息 和 list 字段，data 就是 list 数据initParamObject❌{}表格请求的初始化参数，该值变化会自动请求表格数据toolButtonBoolean❌true是否显示表格功能按钮rowKeyString❌'id'当表格数据多选时，所指定的 idsearchColObject❌{ xs: 1, sm: 2, md: 2, lg: 3, xl: 4 }表格搜索项每列占比配置

### 2、Column 配置（ColumnProps）：

> 使用 `v-bind="column"` 通过属性透传将每一项 **column** 属性全部透传到 **el-table-column** 上，所以我们支持 **el-table-column** 的所有 **Props** 属性。在此基础上，还扩展了以下 **Props：**

属性名类型是否必传默认值属性描述tagBoolean❌false当前单元格值是否为标签展示，可通过 enum 数据中 tagType 字段指定 tag 类型isShowBoolean❌true当前列是否显示在表格内(只对 prop 列生效)searchSearchProps❌—搜索项配置，详情见 SearchPropsenumArray | Function❌—字典，可格式化单元格内容，还可以作为搜索框的下拉选项（字典可以为 API 请求函数，内部会自动执行）isFilterEnumBoolean❌true当前单元格值是否根据 enum 格式化（例如 enum 只作为搜索项数据，不参与内容格式化）fieldNamesObject❌—指定 label && value && children 的 key 值headerRenderFunction❌—自定义表头内容渲染（tsx 语法、h 语法）renderFunction❌—自定义单元格内容渲染（tsx 语法、h 语法）_childrenColumnProps❌—多级表头

### 3、搜索项 配置（SearchProps）：

> 使用 `v-bind="column.search.props&#x201C;` 通过属性透传将 **search.props** 属性全部透传到每一项搜索组件上，所以我们支持 **input、select、tree-select、date-packer、time-picker、time-select、switch** 大部分属性，并在其基础上还扩展了以下 **Props：**

属性名类型是否必传默认值属性描述elString❌—当前项搜索框的类型，支持：input、input-number、select、select-v2、tree-select、cascader、date-picker、time-picker、time-select、switch、sliderpropsObject❌—根据 element plus 官方文档来传递，该属性所有值会透传到组件defaultValueAny❌—搜索项默认值keyString❌—当搜索项 key 不为 prop 属性时，可通过 key 指定orderNumber❌—搜索项排序（从小到大）spanNumber❌1搜索项所占用的列数，默认为 1 列offsetNumber❌—搜索字段左侧偏移列数renderFunction❌自定义搜索内容渲染（tsx 语法、h 语法）

### 4、ProTable 事件：

> 根据 **ElementPlus Table** 文档在 **ProTable** 组件上绑定事件即可，组件会通过 **$attrs** 透传给 **el-table**。
[el-table 事件文档链接](https://link.juejin.cn?target=https%3A%2F%2Felement-plus.org%2Fzh-CN%2Fcomponent%2Ftable.html%23table-%25E4%25BA%258B%25E4%25BB%25B6 "https://element-plus.org/zh-CN/component/table.html#table-%E4%BA%8B%E4%BB%B6")

### 5、ProTable 方法：

> **ProTable** 组件暴露了 **el-table** 实例和一些组件内部的参数和方法：
[el-table 方法文档链接](https://link.juejin.cn?target=https%3A%2F%2Felement-plus.org%2Fzh-CN%2Fcomponent%2Ftable.html%23table-%25E6%2596%25B9%25E6%25B3%2595 "https://element-plus.org/zh-CN/component/table.html#table-%E6%96%B9%E6%B3%95")

方法名描述element `el-table`

实例，可以通过 `proTable.value.element.&#x65B9;&#x6CD5;&#x540D;`

来调用 `el-table`

的所有方法tableData当前页面所展示的数据searchParam所有的搜索参数，不包含分页searchInitParam所有的搜索初始化默认的参数pageable当前表格的分页数据getTableList获取、刷新表格数据的方法（携带所有参数）search表格查询方法，相当于点击搜索按钮reset重置表格查询参数，相当于点击重置按钮clearSelection清空表格所选择的数据，除此方法之外还可使用 `proTable.value.element.clearSelection()`

enumMap当前表格使用的所有字典数据（Map 数据结构）isSelected表格是否选中数据selectedList表格选中的数据列表selectedListIds表格选中的数据列表的 id

### 6、ProTable 插槽：

插槽名描述—默认插槽，支持直接在 ProTable 中写 el-table-column 标签tableHeader自定义表格头部左侧区域的插槽，一般情况该区域放操作按钮toolButton自定义表格头部左右侧侧功能区域的插槽append插入至表格最后一行之后的内容， 如果需要对表格的内容进行无限滚动操作，可能需要用到这个 slot。 若表格有合计行，该 slot 会位于合计行之上。empty当表格数据为空时自定义的内容pagination分页组件插槽 `column.prop`

单元格的作用域插槽 `column.prop`

+ "Header"表头的作用域插槽

## 六、代码实现 & 基础使用 💪（代码较多，详情请去项目里查看）

### 使用一段话总结下我的想法：📚📚

> 🤔 **前提：首先我们在封装 ProTable 组件的时候，在不影响 el-table 原有的属性、事件、方法的前提下，然后在其基础上做二次封装，否则做得再好，也不太完美。**

> 🧐 **思路：把一个表格页面所有重复的功能 （表格多选、查询、重置、刷新、分页、数据操作二次确认、文件下载、文件上传） 都封装成 Hooks 函数钩子或组件，然后在 ProTable 组件中使用这些函数钩子或组件。在页面中使用的时，只需传给 ProTable 当前表格数据的请求 API、表格配置项 columns 就行了，数据传输都使用 作用域插槽 或 tsx 语法从 ProTable 传递给父组件就能在页面上获取到了。**

### 1、常用 Hooks 函数

* **useTable：*

```ts
import { Table } from "./interface";
import { reactive, computed, toRefs } from "vue";

export const useTable = (
  api?: (params: any) => Promise<any>,
  initParam: object = {},
  isPageable: boolean = true,
  dataCallBack?: (data: any) => any,
  requestError?: (error: any) => void
) => {
  const state = reactive<Table.TableStateProps>({

    tableData: [],

    pageable: {

      pageNum: 1,

      pageSize: 10,

      total: 0
    },

    searchParam: {},

    searchInitParam: {},

    totalParam: {}
  });

  const pageParam = computed({
    get: () => {
      return {
        pageNum: state.pageable.pageNum,
        pageSize: state.pageable.pageSize
      };
    },
    set: (newVal: any) => {
      console.log("我是分页更新之后的值", newVal);
    }
  });

  const getTableList = async () => {
    if (!api) return;
    try {

      Object.assign(state.totalParam, initParam, isPageable ? pageParam.value : {});
      let { data } = await api({ ...state.searchInitParam, ...state.totalParam });
      dataCallBack && (data = dataCallBack(data));
      state.tableData = isPageable ? data.list : data;

      const { pageNum, pageSize, total } = data;
      isPageable && updatePageable({ pageNum, pageSize, total });
    } catch (error) {
      requestError && requestError(error);
    }
  };

  const updatedTotalParam = () => {
    state.totalParam = {};

    let nowSearchParam: { [key: string]: any } = {};

    for (let key in state.searchParam) {

      if (state.searchParam[key] || state.searchParam[key] === false || state.searchParam[key] === 0) {
        nowSearchParam[key] = state.searchParam[key];
      }
    }
    Object.assign(state.totalParam, nowSearchParam, isPageable ? pageParam.value : {});
  };

  const updatePageable = (resPageable: Table.Pageable) => {
    Object.assign(state.pageable, resPageable);
  };

  const search = () => {
    state.pageable.pageNum = 1;
    updatedTotalParam();
    getTableList();
  };

  const reset = () => {
    state.pageable.pageNum = 1;
    state.searchParam = {};

    Object.keys(state.searchInitParam).forEach(key => {
      state.searchParam[key] = state.searchInitParam[key];
    });
    updatedTotalParam();
    getTableList();
  };

  const handleSizeChange = (val: number) => {
    state.pageable.pageNum = 1;
    state.pageable.pageSize = val;
    getTableList();
  };

  const handleCurrentChange = (val: number) => {
    state.pageable.pageNum = val;
    getTableList();
  };

  return {
    ...toRefs(state),
    getTableList,
    search,
    reset,
    handleSizeChange,
    handleCurrentChange,
    updatedTotalParam
  };
};

```

* **useSelection：*

```ts
import { ref, computed } from "vue";

export const useSelection = (rowKey: string = "id") => {
  const isSelected = ref<boolean>(false);
  const selectedList = refkey: string]: any }[]>([]);

  const selectedListIds = computed((): string[] => {
    let ids: string[] = [];
    selectedList.value.forEach(item => ids.push(item[rowKey]));
    return ids;
  });

  const selectionChange = (rowArr: { [key: string]: any }[]) => {
    rowArr.length ? (isSelected.value = true) : (isSelected.value = false);
    selectedList.value = rowArr;
  };

  return {
    isSelected,
    selectedList,
    selectedListIds,
    selectionChange
  };
};

```

* **useDownload：*

```ts
import { ElNotification } from "element-plus";

export const useDownload = async (
  api: (param: any) => Promise<any>,
  tempName: string,
  params: any = {},
  isNotify: boolean = true,
  fileType: string = ".xlsx"
) => {
  if (isNotify) {
    ElNotification({
      title: "温馨提示",
      message: "如果数据庞大会导致下载缓慢哦，请您耐心等待！",
      type: "info",
      duration: 3000
    });
  }
  try {
    const res = await api(params);
    const blob = new Blob([res]);

    if ("msSaveOrOpenBlob" in navigator) return window.navigator.msSaveOrOpenBlob(blob, tempName + fileType);
    const blobUrl = window.URL.createObjectURL(blob);
    const exportFile = document.createElement("a");
    exportFile.style.display = "none";
    exportFile.download = `${tempName}${fileType}`;
    exportFile.href = blobUrl;
    document.body.appendChild(exportFile);
    exportFile.click();

    document.body.removeChild(exportFile);
    window.URL.revokeObjectURL(blobUrl);
  } catch (error) {
    console.log(error);
  }
};

```

* **useHandleData：*

```ts
import { ElMessageBox, ElMessage } from "element-plus";
import { HandleData } from "./interface";

export const useHandleData = (
  api: (params: any) => Promise<any>,
  params: any = {},
  message: string,
  confirmType: HandleData.MessageType = "warning"
) => {
  return new Promise((resolve, reject) => {
    ElMessageBox.confirm(`是否${message}?`, "温馨提示", {
      confirmButtonText: "确定",
      cancelButtonText: "取消",
      type: confirmType,
      draggable: true
    }).then(async () => {
      const res = await api(params);
      if (!res) return reject(false);
      ElMessage({
        type: "success",
        message: `${message}成功!`
      });
      resolve(true);
    });
  });
};

```

### 2、Protable 组件：

* **ProTable：*

```html

<template>

  <SearchForm
    :search="search"
    :reset="reset"
    :columns="searchColumns"
    :search-param="searchParam"
    :search-col="searchCol"
    v-show="isShowSearch"
  />

  <div class="card table-main">

    <div class="table-header">
      <div class="header-button-lf">
        <slot name="tableHeader" :selectedListIds="selectedListIds" :selectedList="selectedList" :isSelected="isSelected" />
      div>
      <div class="header-button-ri" v-if="toolButton">
        <slot name="toolButton">
          <el-button :icon="Refresh" circle @click="getTableList" />
          <el-button :icon="Printer" circle v-if="columns.length" @click="print" />
          <el-button :icon="Operation" circle v-if="columns.length" @click="openColSetting" />
          <el-button :icon="Search" circle v-if="searchColumns.length" @click="isShowSearch = !isShowSearch" />
        slot>
      div>
    div>

    <el-table
      ref="tableRef"
      v-bind="$attrs"
      :data="data ?? tableData"
      :border="border"
      :row-key="rowKey"
      @selection-change="selectionChange"
    >

      <slot>slot>
      <template v-for="item in tableColumns" :key="item">

        <el-table-column
          v-bind="item"
          :align="item.align ?? 'center'"
          :reserve-selection="item.type == 'selection'"
          v-if="item.type && ['selection', 'index', 'expand'].includes(item.type)"
        >
          <template #default="scope" v-if="item.type == 'expand'">
            <component :is="item.render" v-bind="scope" v-if="item.render"> component>
            <slot :name="item.type" v-bind="scope" v-else>slot>
          template>
        el-table-column>

        <TableColumn v-if="!item.type && item.prop && item.isShow" :column="item">
          <template v-for="slot in Object.keys($slots)" #[slot]="scope">
            <slot :name="slot" v-bind="scope">slot>
          template>
        TableColumn>
      template>

      <template #append>
        <slot name="append"> slot>
      template>

      <template #empty>
        <div class="table-empty">
          <slot name="empty">
            <img src="@/assets/images/notData.png" alt="notData" />
            <div>暂无数据div>
          slot>
        div>
      template>
    el-table>

    <slot name="pagination">
      <Pagination
        v-if="pagination"
        :pageable="pageable"
        :handle-size-change="handleSizeChange"
        :handle-current-change="handleCurrentChange"
      />
    slot>
  div>

  <ColSetting v-if="toolButton" ref="colRef" v-model:col-setting="colSetting" />
template>

<script setup lang="ts" name="ProTable">
import { ref, watch, computed, provide, onMounted } from "vue";
import { ElTable } from "element-plus";
import { useTable } from "@/hooks/useTable";
import { useSelection } from "@/hooks/useSelection";
import { BreakPoint } from "@/components/Grid/interface";
import { ColumnProps } from "@/components/ProTable/interface";
import { Refresh, Printer, Operation, Search } from "@element-plus/icons-vue";
import { filterEnum, formatValue, handleProp, handleRowAccordingToProp } from "@/utils";
import SearchForm from "@/components/SearchForm/index.vue";
import Pagination from "./components/Pagination.vue";
import ColSetting from "./components/ColSetting.vue";
import TableColumn from "./components/TableColumn.vue";
import printJS from "print-js";

export interface ProTableProps {
  columns: ColumnProps[];
  data?: any[];
  requestApi?: (params: any) => Promise;
  requestAuto?: boolean;
  requestError?: (params: any) => void;
  dataCallback?: (data: any) => any;
  title?: string;
  pagination?: boolean;
  initParam?: any;
  border?: boolean;
  toolButton?: boolean;
  rowKey?: string;
  searchCol?: number | Record<BreakPoint, number>;
}

const props = withDefaults(defineProps<ProTableProps>(), {
  columns: () => [],
  requestAuto: true,
  pagination: true,
  initParam: {},
  border: true,
  toolButton: true,
  rowKey: "id",
  searchCol: () => ({ xs: 1, sm: 2, md: 2, lg: 3, xl: 4 })
});

const isShowSearch = ref(true);

const tableRef = ref<InstanceType<typeof ElTable>>();

const { selectionChange, selectedList, selectedListIds, isSelected } = useSelection(props.rowKey);

const { tableData, pageable, searchParam, searchInitParam, getTableList, search, reset, handleSizeChange, handleCurrentChange } =
  useTable(props.requestApi, props.initParam, props.pagination, props.dataCallback, props.requestError);

const clearSelection = () => tableRef.value!.clearSelection();

onMounted(() => props.requestAuto && getTableList());

watch(() => props.initParam, getTableList, { deep: true });

const tableColumns = ref<ColumnProps[]>(props.columns);

const enumMap = ref(new Mapkey: string]: any }[]>());
provide("enumMap", enumMap);
const setEnumMap = async (col: ColumnProps) => {
  if (!col.enum) return;

  if (typeof col.enum !== "function") return enumMap.value.set(col.prop!, col.enum!);
  const { data } = await col.enum();
  enumMap.value.set(col.prop!, data);
};

const flatColumnsFunc = (columns: ColumnProps[], flatArr: ColumnProps[] = []) => {
  columns.forEach(async col => {
    if (col._children?.length) flatArr.push(...flatColumnsFunc(col._children));
    flatArr.push(col);

    col.isShow = col.isShow ?? true;
    col.isFilterEnum = col.isFilterEnum ?? true;

    setEnumMap(col);
  });
  return flatArr.filter(item => !item._children?.length);
};

const flatColumns = ref<ColumnProps[]>();
flatColumns.value = flatColumnsFunc(tableColumns.value);

const searchColumns = flatColumns.value.filter(item => item.search?.el || item.search?.render);

searchColumns.forEach((column, index) => {
  column.search!.order = column.search!.order ?? index + 2;
  if (column.search?.defaultValue !== undefined && column.search?.defaultValue !== null) {
    searchInitParam.value[column.search.key ?? handleProp(column.prop!)] = column.search?.defaultValue;
    searchParam.value[column.search.key ?? handleProp(column.prop!)] = column.search?.defaultValue;
  }
});

searchColumns.sort((a, b) => a.search!.order! - b.search!.order!);

const colRef = ref();
const colSetting = tableColumns.value!.filter(
  item => !["selection", "index", "expand"].includes(item.type!) && item.prop !== "operation" && item.isShow
);
const openColSetting = () => colRef.value.openColSetting();

const printData = computed(() => {
  const handleData = props.data ?? tableData.value;
  const printDataList = JSON.parse(JSON.stringify(selectedList.value.length ? selectedList.value : handleData));

  const needTransformCol = flatColumns.value!.filter(
    item => (item.enum || (item.prop && item.prop.split(".").length > 1)) && item.isFilterEnum
  );
  needTransformCol.forEach(colItem => {
    printDataList.forEach((tableItem: { [key: string]: any }) => {
      tableItem[handleProp(colItem.prop!)] =
        colItem.prop!.split(".").length > 1 && !colItem.enum
          ? formatValue(handleRowAccordingToProp(tableItem, colItem.prop!))
          : filterEnum(handleRowAccordingToProp(tableItem, colItem.prop!), enumMap.value.get(colItem.prop!), colItem.fieldNames);
      for (const key in tableItem) {
        if (tableItem[key] === null) tableItem[key] = formatValue(tableItem[key]);
      }
    });
  });
  return printDataList;
});

const print = () => {
  const header = `${props.title}`;
  const gridHeaderStyle = "border: 1px solid #ebeef5;height: 45px;color: #232425;text-align: center;background-color: #fafafa;";
  const gridStyle = "border: 1px solid #ebeef5;height: 40px;color: #494b4e;text-align: center";
  printJS({
    printable: printData.value,
    header: props.title && header,
    properties: flatColumns
      .value!.filter(item => !["selection", "index", "expand"].includes(item.type!) && item.isShow && item.prop !== "operation")
      .map((item: ColumnProps) => ({ field: handleProp(item.prop!), displayName: item.label })),
    type: "json",
    gridHeaderStyle,
    gridStyle
  });
};

defineExpose({
  element: tableRef,
  tableData,
  pageable,
  searchParam,
  searchInitParam,
  getTableList,
  search,
  reset,
  handleSizeChange,
  handleCurrentChange,
  clearSelection,
  enumMap,
  isSelected,
  selectedList,
  selectedListIds
});
script>

```

* **TableColumn：*

```html
<template>
  <RenderTableColumn v-bind="column" />
template>

<script setup lang="tsx" name="TableColumn">
import { inject, ref, useSlots } from "vue";
import { ColumnProps, RenderScope, HeaderRenderScope } from "@/components/ProTable/interface";
import { filterEnum, formatValue, handleProp, handleRowAccordingToProp } from "@/utils";

definePropscolumn: ColumnProps }>();

const slots = useSlots();

const enumMap = inject("enumMap", ref(new Map()));

const renderCellData = (item: ColumnProps, scope: RenderScope) => {
  return enumMap.value.get(item.prop) && item.isFilterEnum
    ? filterEnum(handleRowAccordingToProp(scope.row, item.prop!), enumMap.value.get(item.prop)!, item.fieldNames)
    : formatValue(handleRowAccordingToProp(scope.row, item.prop!));
};

const getTagType = (item: ColumnProps, scope: RenderScope) => {
  return filterEnum(handleRowAccordingToProp(scope.row, item.prop!), enumMap.value.get(item.prop), item.fieldNames, "tag");
};

const RenderTableColumn = (item: ColumnProps) => {
  return (
    <>
      {item.isShow && (

          {{
            default: (scope: RenderScope) => {
              if (item._children) return item._children.map(child => RenderTableColumn(child));
              if (item.render) return item.render(scope);
              if (slots[handleProp(item.prop!)]) return slots[handleProp(item.prop!)]!(scope);
              if (item.tag) return {renderCellData(item, scope)};
              return renderCellData(item, scope);
            },
            header: (scope: HeaderRenderScope) => {
              if (item.headerRender) return item.headerRender(scope);
              if (slots[`${handleProp(item.prop!)}Header`]) return slots[`${handleProp(item.prop!)}Header`]!(scope);
              return item.label;
            }
          }}

      )}
    </>
  );
};
script>

```

### 3、页面使用 ProTable 组件：

```html
<template>
  <div class="table-box">
    <ProTable
      ref="proTable"
      title="用户列表"
      :columns="columns"
      :request-api="getTableList"
      :init-param="initParam"
      :data-callback="dataCallback"
    >

      <template #tableHeader="scope">
        <el-button type="primary" :icon="CirclePlus" @click="openDrawer('新增')" v-auth="'add'">新增用户el-button>
        <el-button type="primary" :icon="Upload" plain @click="batchAdd" v-auth="'batchAdd'">批量添加用户el-button>
        <el-button type="primary" :icon="Download" plain @click="downloadFile" v-auth="'export'">导出用户数据el-button>
        <el-button type="primary" plain @click="toDetail">To 子集详情页面el-button>
        <el-button type="danger" :icon="Delete" plain @click="batchDelete(scope.selectedListIds)" :disabled="!scope.isSelected">
          批量删除用户
        el-button>
      template>

      <template #expand="scope">
        {{ scope.row }}
      template>

      <template #usernameHeader="scope">
        <el-button type="primary" @click="ElMessage.success('我是通过作用域插槽渲染的表头')">
          {{ scope.column.label }}
        el-button>
      template>

      <template #createTime="scope">
        <el-button type="primary" link @click="ElMessage.success('我是通过作用域插槽渲染的内容')">
          {{ scope.row.createTime }}
        el-button>
      template>

      <template #operation="scope">
        <el-button type="primary" link :icon="View" @click="openDrawer('查看', scope.row)">查看el-button>
        <el-button type="primary" link :icon="EditPen" @click="openDrawer('编辑', scope.row)">编辑el-button>
        <el-button type="primary" link :icon="Refresh" @click="resetPass(scope.row)">重置密码el-button>
        <el-button type="primary" link :icon="Delete" @click="deleteAccount(scope.row)">删除el-button>
      template>
    ProTable>
    <UserDrawer ref="drawerRef" />
    <ImportExcel ref="dialogRef" />
  div>
template>

<script setup lang="tsx" name="useProTable">
import { ref, reactive } from "vue";
import { useRouter } from "vue-router";
import { User } from "@/api/interface";
import { useHandleData } from "@/hooks/useHandleData";
import { useDownload } from "@/hooks/useDownload";
import { useAuthButtons } from "@/hooks/useAuthButtons";
import { ElMessage, ElMessageBox } from "element-plus";
import ProTable from "@/components/ProTable/index.vue";
import ImportExcel from "@/components/ImportExcel/index.vue";
import UserDrawer from "@/views/proTable/components/UserDrawer.vue";
import { ProTableInstance, ColumnProps, HeaderRenderScope } from "@/components/ProTable/interface";
import { CirclePlus, Delete, EditPen, Download, Upload, View, Refresh } from "@element-plus/icons-vue";
import {
  getUserList,
  deleteUser,
  editUser,
  addUser,
  changeUserStatus,
  resetUserPassWord,
  exportUserInfo,
  BatchAddUser,
  getUserStatus,
  getUserGender
} from "@/api/modules/user";

const router = useRouter();

const toDetail = () => {
  router.push(`/proTable/useProTable/detail/${Math.random().toFixed(3)}?params=detail-page`);
};

const proTable = ref<ProTableInstance>();

const initParam = reactive({ type: 1 });

const dataCallback = (data: any) => {
  return {
    list: data.list,
    total: data.total,
    pageNum: data.pageNum,
    pageSize: data.pageSize
  };
};

const getTableList = (params: any) => {
  let newParams = JSON.parse(JSON.stringify(params));
  newParams.createTime && (newParams.startTime = newParams.createTime[0]);
  newParams.createTime && (newParams.endTime = newParams.createTime[1]);
  delete newParams.createTime;
  return getUserList(newParams);
};

const { BUTTONS } = useAuthButtons();

const headerRender = (scope: HeaderRenderScope) => {
  return (
    <el-button type="primary" onClick={() => ElMessage.success("我是通过 tsx 语法渲染的表头")}>
      {scope.column.label}
    el-button>
  );
};

const columns: ColumnProps<User.ResUserList>[] = [
  { type: "selection", fixed: "left", width: 80 },
  { type: "index", label: "#", width: 80 },
  { type: "expand", label: "Expand", width: 100 },
  {
    prop: "username",
    label: "用户姓名",
    search: { el: "input" },
    render: scope => {
      return (
        <el-button type="primary" link onClick={() => ElMessage.success("我是通过 tsx 语法渲染的内容")}>
          {scope.row.username}
        el-button>
      );
    }
  },
  {
    prop: "gender",
    label: "性别",

    enum: getUserGender,

    search: { el: "select", props: { filterable: true } },
    fieldNames: { label: "genderLabel", value: "genderValue" }
  },
  {

    prop: "user.detail.age",
    label: "年龄",
    search: {

      render: ({ searchParam }) => {
        return (
          <div class="flx-center">
            <el-input vModel_trim={searchParam.minAge} placeholder="最小年龄" />
            <span class="mr10 ml10">-span>
            <el-input vModel_trim={searchParam.maxAge} placeholder="最大年龄" />
          div>
        );
      }
    }
  },
  { prop: "idCard", label: "身份证号", search: { el: "input" } },
  { prop: "email", label: "邮箱" },
  { prop: "address", label: "居住地址" },
  {
    prop: "status",
    label: "用户状态",
    enum: getUserStatus,
    search: { el: "tree-select", props: { filterable: true } },
    fieldNames: { label: "userLabel", value: "userStatus" },
    render: scope => {
      return (
        <>
          {BUTTONS.value.status ? (
            <el-switch
              model-value={scope.row.status}
              active-text={scope.row.status ? "启用" : "禁用"}
              active-value={1}
              inactive-value={0}
              onClick={() => changeStatus(scope.row)}
            />
          ) : (
            <el-tag type={scope.row.status ? "success" : "danger"}>{scope.row.status ? "启用" : "禁用"}el-tag>
          )}
        </>
      );
    }
  },
  {
    prop: "createTime",
    label: "创建时间",
    headerRender,
    width: 180,
    search: {
      el: "date-picker",
      span: 2,
      props: { type: "datetimerange", valueFormat: "YYYY-MM-DD HH:mm:ss" },
      defaultValue: ["2022-11-12 11:35:00", "2022-12-12 11:35:00"]
    }
  },
  { prop: "operation", label: "操作", fixed: "right", width: 330 }
];

const deleteAccount = async (params: User.ResUserList) => {
  await useHandleData(deleteUser, { id: [params.id] }, `删除【${params.username}】用户`);
  proTable.value?.getTableList();
};

const batchDelete = async (id: string[]) => {
  await useHandleData(deleteUser, { id }, "删除所选用户信息");
  proTable.value?.clearSelection();
  proTable.value?.getTableList();
};

const resetPass = async (params: User.ResUserList) => {
  await useHandleData(resetUserPassWord, { id: params.id }, `重置【${params.username}】用户密码`);
  proTable.value?.getTableList();
};

const changeStatus = async (row: User.ResUserList) => {
  await useHandleData(changeUserStatus, { id: row.id, status: row.status == 1 ? 0 : 1 }, `切换【${row.username}】用户状态`);
  proTable.value?.getTableList();
};

const downloadFile = async () => {
  ElMessageBox.confirm("确认导出用户数据?", "温馨提示", { type: "warning" }).then(() =>
    useDownload(exportUserInfo, "用户列表", proTable.value?.searchParam)
  );
};

const dialogRef = ref<InstanceType<typeof ImportExcel> | null>(null);
const batchAdd = () => {
  const params = {
    title: "用户",
    tempApi: exportUserInfo,
    importApi: BatchAddUser,
    getTableList: proTable.value?.getTableList
  };
  dialogRef.value?.acceptParams(params);
};

const drawerRef = ref<InstanceType<typeof UserDrawer> | null>(null);
const openDrawer = (title: string, row: Partial = {}) => {
  const params = {
    title,
    isView: title === "查看",
    row: { ...row },
    api: title === "新增" ? addUser : title === "编辑" ? editUser : undefined,
    getTableList: proTable.value?.getTableList
  };
  drawerRef.value?.acceptParams(params);
};
script>

```

## 七、贡献者 👨‍👦‍👦

* [HalseySpicy](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2FHalseySpicy "https://github.com/HalseySpicy")
* [denganjia](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fdenganjia "https://github.com/denganjia")
