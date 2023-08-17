---
link: https://juejin.cn/post/7094890833064755208
title: Vue3.2 + Element-Plus 二次封装 el-table（TypeScript版🔥🔥）
description: 前言 📖 在公司经常接触到后台管理系统的开发，基本上 90% 以上都是 table 页面，业务逻辑也基本上一样。刚开始也想的是找一个基于 element 二次封装的 el-table
keywords: 前端
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2022-05-07T07:21:43.000Z
publisher: 稀土掘金
stats: paragraph=135 sentences=233, words=1963
---
## 前言 📖

> 在公司经常接触到后台管理系统的开发，基本上 **90%** 以上都是 **table** 页面，业务逻辑也基本上一样。刚开始也想的是找一个基于 **element** 二次封装的 **el-table**，在搜寻过程中也接触了很多优秀的项目，其中包括：（vxe-table、avue......）但是这些项目终归没有自己开发灵活，所有就诞生了我的 **Pro-Table** 组件🎉🎉🎉 目前能节省我 **60%** 的工作量。（仅代表个人实践）

> 💢 请注意：以下内容只代表我个人封装思想，如果你觉得还不错，请帮我点个小小的 **star**，如果你有更好的想法，请在评论区留言，蟹蟹 😆😆

## 一、在线预览 👀

## 二、Git 仓库地址 (欢迎 Star⭐⭐⭐)

## 三、Pro-Table 功能 🔨🔨🔨

* 表格内容自适应屏幕宽高，溢出内容表格内部滚动。
* 表格数据操作 Hooks （单条数据删除、批量删除、重置密码、状态切换......）
* 表格数据多选 Hooks （支持现跨页勾选数据）
* 表格序号、每行可自定义展开信息、表格头部自定义渲染（使用 tsx 语法）
* 表格列排序、单元格内容格式化（有字典会根据字典自动格式化）
* 树形表格展示（后期会增加懒加载）
* 表格数据导入组件、导出钩子函数
* 表格查询（可携带初始参数）、重置功能的封装
* 表格分页模块封装（Pagination）
* 表格数据刷新、列显隐、搜索显隐设置

## 四、需求分析 📑

### 首先我们来看效果图（总共可以分为五个区域）：

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/142e8b74c1b747c0bb384770e1993cf4~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.image?)

* 1、表格搜索区域
* 2、表格数据操作按钮区域
* 3、表格功能按钮区域
* 4、表格主体内容展示区域
* 5、表格分页区域

### 1、搜索区域分析：

> 可以看到搜索区域的字段都是存在于表格当中的，并且每个页面的搜索、重置方法都是一样的逻辑，只是不同的查询参数而已。我们完全可以在传表格配置项 **columns** 时，直接指定某个字段的 **search:true** 就能把该项变为搜索项，然后使用 **SearchType** 字段可以指定搜索框的类型，最后把表格的搜索方法都封装成 **Hooks** 钩子函数。页面上完全就不会存在搜索逻辑了。

### 2、表格数据操作按钮区域分析：

> 表格数据操作按钮基本上每个页面都会不一样，所以我们直接使用 **作用域插槽** 来完成每个页面的数据操作按钮区域， **作用域插槽** 可以将表格多选数据信息从 **Pro-Table** 的 **Hooks** 多选钩子函数中传到页面上使用。

### 3、表格功能按钮区域分析：

> 这块区域没什么特殊功能，只有三个按钮，其功能分别为： **表格数据刷新（一直会携带当前查询和分页条件）、表格列显隐设置、表格搜索区域显隐（方便展示更多的数据信息）**。 可通过 **toolButton** 属性控制这块区域的显隐。

### 4、表格主体内容展示区域分析：

> 这块区域主要就是数据展示，配置 **columns** 项传到 **Pro-Table** 组件中就行了。使用作用域插槽可以自定义每一列的显示自己需要的内容，还支持表格数据多选（内部已封装了多选 Hooks 钩子函数）。

### 5、表格分页区域分析：

> 分页也没有什么特殊的功能，该支持的都支持了。 🤣🤣

## 五、Pro-Table 文档

### 1、Pro-Table 属性配置：

字段字段类型是否必传默认值字段描述columnsColumnProps✅—Pro-Table 会根据此字段渲染搜索表单与表格列requestApiFunction✅—获取表格数据的请求 APIdataCallbackFunction❌—返回数据的回调函数，可以对数据进行处理paginationBoolean❌true是否显示分页组件initParamObject❌{}是否携带表格请求的初始化参数borderBoolean❌true是否带有纵向边框stripeBoolean❌false是否为斑马纹 tabletoolButtonBoolean❌true是否显示表格工具按钮区域childrenNameString❌children当表格为树形表格时，指定 children 字段名

### 2、ColumnProps 属性配置（都是可选参数）：

字段字段类型默认值可选值字段描述typeString—index | selection | expand特殊类型（序号、多选、展开）propString——字段名称对应列内容的字段名labelString——表头标题widthNumber | String——单元格宽度minWidthNumber | String——单元格最小列宽isShowBooleantrue—是否显示在表格内sortableBooleanfalse—是否静态排序fixedString—left | right固定在表格左、右tagBooleanfalse—是否是标签展示searchBooleanfalse—是否为搜索项searchTypeStringtexttext | select | multipleSelect | treeSelect | mutipleTreeSelect | date | timerange | datetimerange搜索项类型searchPropsObject——搜索项参数，根据 element 文档来，标签自带属性 > props 属性searchInitParamString | Number | Boolean | Any[]——搜索项是否带初始化参数enumObject——字典，可格式化单元格，还可以作为搜索框的下拉选项renderHeaderFunction——自定义表头

## 六、代码实现💪（详情去项目里查看，这里只贴了一部分代码）

### 使用一段话总结下我的想法：📚📚

> 把一个表格页面所有重复的功能 **（表格多选、查询、重置、刷新、分页器、数据操作二次确认、文件下载、文件上传）** 都封装成 **Hooks** 函数钩子，然后在 **Pro-Table** 组件中使用这些函数钩子。在页面中使用的时，只需传给 **Pro-Table** 当前表格数据的请求 **API**，表格配置项 **columns** 就行了，数据传输都使用作用域插槽从 **Pro-Table** 传给父组件就能在页面上获取到了。

### 1、常用 Hooks 函数

* **useTable：*

```js
import { Table } from "./interface";
import { reactive, computed, onMounted, toRefs } from "vue";

export const useTable = (
  api: (params: any) => Promise,
  initParam: object = {},
  isPageable: boolean = true,
  dataCallBack?: (data: any) => any
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

  onMounted(() => {
    reset();
  });

  const getTableList = async () => {
    try {

      Object.assign(state.totalParam, initParam, isPageable ? pageParam.value : {});
      let { data } = await api(state.totalParam);
      dataCallBack && (data = dataCallBack(data));
      state.tableData = isPageable ? data.datalist : data;

      const { pageNum, pageSize, total } = data;
      isPageable && updatePageable({ pageNum, pageSize, total });
    } catch (error) {
      console.log(error);
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
    handleCurrentChange
  };
};

```

* **useSelection：*

```js
import { ref, computed } from "vue";

export const useSelection = () => {

  const isSelected = ref(false);

  const selectedList = ref([]);

  const selectedListIds = computed((): string[] => {
    let ids: string[] = [];
    selectedList.value.forEach(item => {
      ids.push(item["id"]);
    });
    return ids;
  });

  const getRowKeys = (row: { id: string }) => {
    return row.id;
  };

  const selectionChange = (rowArr: any) => {
    rowArr.length === 0 ? (isSelected.value = false) : (isSelected.value = true);
    selectedList.value = rowArr;
  };

  return {
    isSelected,
    selectedList,
    selectedListIds,
    selectionChange,
    getRowKeys
  };
};

```

* **useDownload：*

```js
import { ElNotification } from "element-plus";

export const useDownload = async (
  api: (param: any) => Promise,
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

```js
import { ElMessageBox, ElMessage } from "element-plus";
import { HandleData } from "./interface";

export const useHandleData = (
  api: (params: any) => Promise,
  params: Parameters<typeof api>[0],
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

### 2、Pro-table 组件：

* **Template**：

```html

<template>
  <div class="table-box">

    <SearchForm :search="search" :reset="reset" :searchParam="searchParam" :columns="searchColumns" v-show="isShowSearch" />

    <div class="table-header">
      <div class="header-button-lf">
        <slot name="tableHeader" :ids="selectedListIds" :isSelected="isSelected">slot>
      div>
      <div class="header-button-ri" v-if="toolButton">
        <el-button :icon="Refresh" circle @click="getTableList"> el-button>
        <el-button :icon="Operation" circle @click="openColSetting"> el-button>
        <el-button :icon="Search" circle v-if="searchColumns.length" @click="isShowSearch = !isShowSearch"> el-button>
      div>
    div>

    <el-table
      height="575"
      ref="tableRef"
      :data="tableData"
      :border="border"
      @selection-change="selectionChange"
      :row-key="getRowKeys"
      :stripe="stripe"
      :tree-props="{ children: childrenName }"
    >
      <template v-for="item in tableColumns" :key="item">

        <el-table-column
          v-if="item.type == 'selection' || item.type == 'index'"
          :type="item.type"
          :reserve-selection="item.type == 'selection'"
          :label="item.label"
          :width="item.width"
          :min-width="item.minWidth"
          :fixed="item.fixed"
        >
        el-table-column>

        <el-table-column
          v-if="item.type == 'expand'"
          :type="item.type"
          :label="item.label"
          :width="item.width"
          :min-width="item.minWidth"
          :fixed="item.fixed"
          v-slot="scope"
        >
          <slot :name="item.type" :row="scope.row">slot>
        el-table-column>

        <el-table-column
          v-if="!item.type && item.prop && item.isShow"
          :prop="item.prop"
          :label="item.label"
          :width="item.width"
          :min-width="item.minWidth"
          :sortable="item.sortable"
          :show-overflow-tooltip="item.prop !== 'operation'"
          :resizable="true"
          :fixed="item.fixed"
        >

          <template #header v-if="item.renderHeader">
            <component :is="item.renderHeader" :row="item"> component>
          template>

          <template #default="scope">
            <slot :name="item.prop" :row="scope.row">

              <el-tag v-if="item.tag" :type="filterEnum(scope.row[item.prop!], item.enum!, item.searchProps,'tag')">
                {{
		    item.enum?.length ? filterEnum(scope.row[item.prop!], item.enum!, item.searchProps) : formatValue(scope.row[item.prop!])
                }}
              el-tag>

              <span v-else>
                {{
		    item.enum?.length ? filterEnum(scope.row[item.prop!], item.enum!, item.searchProps) : formatValue(scope.row[item.prop!])
                }}
              span>
            slot>
          template>
        el-table-column>
      template>
      <template #empty>
        <div class="table-empty">
          <img src="@/assets/images/notData.png" alt="notData" />
          <div>暂无数据div>
        div>
      template>
    el-table>

    <Pagination
      v-if="pagination"
      :pageable="pageable"
      :handleSizeChange="handleSizeChange"
      :handleCurrentChange="handleCurrentChange"
    />

    <ColSetting v-if="toolButton" ref="colRef" :tableRef="tableRef" :colSetting="colSetting" />
  div>
template>
```

* **Script**：

```html
<script setup lang="ts" name="proTable">
import { ref, watch } from "vue";
import { useTable } from "@/hooks/useTable";
import { useSelection } from "@/hooks/useSelection";
import { Refresh, Operation, Search } from "@element-plus/icons-vue";
import { ColumnProps } from "@/components/ProTable/interface";
import { filterEnum, formatValue } from "@/utils/util";
import SearchForm from "@/components/SearchForm/index.vue";
import Pagination from "./components/Pagination.vue";
import ColSetting from "./components/ColSetting.vue";

const tableRef = ref();

const isShowSearch = ref(true);

interface ProTableProps {
  columns: Partial<ColumnProps>[];
  requestApi: (params: any) => Promise;
  dataCallback?: (data: any) => any;
  pagination?: boolean;
  initParam?: any;
  border?: boolean;
  stripe?: boolean;
  toolButton?: boolean;
  childrenName?: string;
}

const props = withDefaults(defineProps<ProTableProps>(), {
  columns: () => [],
  pagination: true,
  initParam: {},
  border: true,
  stripe: false,
  toolButton: true,
  childrenName: "children"
});

const { selectionChange, getRowKeys, selectedListIds, isSelected } = useSelection();

const { tableData, pageable, searchParam, searchInitParam, getTableList, search, reset, handleSizeChange, handleCurrentChange } =
  useTable(props.requestApi, props.initParam, props.pagination, props.dataCallback);

watch(
  () => props.initParam,
  () => {
    getTableList();
  },
  { deep: true }
);

const tableColumns = ref<Partial<ColumnProps>[]>();
tableColumns.value = props.columns.map(item => {
  return {
    ...item,
    isShow: item.isShow ?? true
  };
});

tableColumns.value.forEach(async item => {
  if (item.enum && typeof item.enum === "function") {
    const { data } = await item.enum();
    item.enum = data;
  }
});

const searchColumns = tableColumns.value.filter(item => item.search);

searchColumns.forEach(column => {
  if (column.searchInitParam !== undefined && column.searchInitParam !== null) {
    searchInitParam.value[column.prop!] = column.searchInitParam;
  }
});

const colRef = ref();

const colSetting = tableColumns.value.filter((item: Partial) => {
  return (
    item.type !== "selection" &&
    item.type !== "index" &&
    item.type !== "expand" &&
    item.prop !== "operation" &&
    item.isShow !== false
  );
});
const openColSetting = () => {
  colRef.value.openColSetting();
};

defineExpose({ searchParam, refresh: getTableList });
script>

```

### 3、页面使用：

```html
<template>
  <div class="table-box">
    <ProTable ref="proTable" :columns="columns" :requestApi="getUserList" :initParam="initParam" :dataCallback="dataCallback">

      <template #tableHeader="scope">
        <el-button type="primary" :icon="CirclePlus" @click="openDrawer('新增')" v-if="BUTTONS.add">新增用户el-button>
        <el-button type="primary" :icon="Upload" plain @click="batchAdd" v-if="BUTTONS.batchAdd">批量添加用户el-button>
        <el-button type="primary" :icon="Download" plain @click="downloadFile" v-if="BUTTONS.export">导出用户数据el-button>
        <el-button
          type="danger"
          :icon="Delete"
          plain
          :disabled="!scope.isSelected"
          @click="batchDelete(scope.ids)"
          v-if="BUTTONS.batchDelete"
        >
          批量删除用户
        el-button>
      template>

      <template #expand="scope">
        {{ scope.row }}
      template>

      <template #status="scope">

        <div @click="changeStatus(scope.row)" v-if="BUTTONS.status">
          <el-switch
            :value="scope.row.status"
            :active-text="scope.row.status === 1 ? '启用' : '禁用'"
            :active-value="1"
            :inactive-value="0"
          />
        div>
        <el-tag :type="scope.row.status === 1 ? 'success' : 'danger'" v-else>
          {{ scope.row.status === 1 ? "启用" : "禁用" }}
        el-tag>
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

<script setup lang="tsx" name="useComponent">
import { ref, reactive } from "vue";
import { ElMessage } from "element-plus";
import { User } from "@/api/interface";
import { ColumnProps } from "@/components/ProTable/interface";
import { useHandleData } from "@/hooks/useHandleData";
import { useDownload } from "@/hooks/useDownload";
import { useAuthButtons } from "@/hooks/useAuthButtons";
import ProTable from "@/components/ProTable/index.vue";
import ImportExcel from "@/components/ImportExcel/index.vue";
import UserDrawer from "@/views/proTable/components/UserDrawer.vue";
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

const proTable = ref();

const initParam = reactive({
  type: 1
});

const dataCallback = (data: any) => {
  return {
    datalist: data.datalist,
    total: data.total,
    pageNum: data.pageNum,
    pageSize: data.pageSize
  };
};

const { BUTTONS } = useAuthButtons();

const renderHeader = (scope: any) => {
  return (
    <el-button
      type="primary"
      onClick={() => {
        ElMessage.success("我是自定义表头");
      }}
    >
      {scope.row.label}
    el-button>
  );
};

const columns: Partial<ColumnProps>[] = [
  { type: "selection", width: 80, fixed: "left" },
  { type: "index", label: "#", width: 80 },
  { type: "expand", label: "Expand", width: 100 },
  { prop: "username", label: "用户姓名", width: 130, search: true, searchProps: { disabled: true }, renderHeader },

  {
    prop: "gender",
    label: "性别",
    width: 120,
    sortable: true,
    search: true,
    searchType: "select",
    enum: getUserGender,
    searchProps: { label: "genderLabel", value: "genderValue" }
  },
  { prop: "idCard", label: "身份证号", search: true },
  { prop: "email", label: "邮箱", search: true },
  { prop: "address", label: "居住地址", search: true },
  {
    prop: "status",
    label: "用户状态",
    sortable: true,
    search: true,
    searchType: "select",
    enum: getUserStatus,
    searchProps: { label: "userLabel", value: "userStatus" }
  },
  {
    prop: "createTime",
    label: "创建时间",
    width: 200,
    sortable: true,
    search: true,
    searchType: "datetimerange",
    searchProps: {
      disabledDate: (time: Date) => time.getTime() < Date.now() - 8.64e7
    },
    searchInitParam: ["2022-08-30 00:00:00", "2022-08-20 23:59:59"]
  },
  { prop: "operation", label: "操作", width: 330, fixed: "right", renderHeader }
];

const deleteAccount = async (params: User.ResUserList) => {
  await useHandleData(deleteUser, { id: [params.id] }, `删除【${params.username}】用户`);
  proTable.value.refresh();
};

const batchDelete = async (id: string[]) => {
  await useHandleData(deleteUser, { id }, "删除所选用户信息");
  proTable.value.refresh();
};

const resetPass = async (params: User.ResUserList) => {
  await useHandleData(resetUserPassWord, { id: params.id }, `重置【${params.username}】用户密码`);
  proTable.value.refresh();
};

const changeStatus = async (row: User.ResUserList) => {
  await useHandleData(changeUserStatus, { id: row.id, status: row.status == 1 ? 0 : 1 }, `切换【${row.username}】用户状态`);
  proTable.value.refresh();
};

const downloadFile = async () => {
  useDownload(exportUserInfo, "用户列表", proTable.value.searchParam);
};

interface DialogExpose {
  acceptParams: (params: any) => void;
}
const dialogRef = ref<DialogExpose>();
const batchAdd = () => {
  let params = {
    title: "用户",
    tempApi: exportUserInfo,
    importApi: BatchAddUser,
    getTableList: proTable.value.refresh
  };
  dialogRef.value!.acceptParams(params);
};

interface DrawerExpose {
  acceptParams: (params: any) => void;
}
const drawerRef = ref<DrawerExpose>();
const openDrawer = (title: string, rowData: Partial = { avatar: "" }) => {
  let params = {
    title,
    rowData: { ...rowData },
    isView: title === "查看",
    apiUrl: title === "新增" ? addUser : title === "编辑" ? editUser : "",
    getTableList: proTable.value.refresh
  };
  drawerRef.value!.acceptParams(params);
};
script>
```
