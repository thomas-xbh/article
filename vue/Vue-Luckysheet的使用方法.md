---
link: https://blog.csdn.net/Lyy1991Wdl/article/details/124175868
title: Vue-Luckysheet的使用方法
description: Luckysheet ，一款纯前端类似excel的在线表格，功能强大、配置简单、完全开源。配置文档 · API · 教程：快速上手 | Luckysheet文档现将分享我在使用该组件过程中遇到的问题及解决方法。Vue中定义Luckysheet组件：组件系统是 Vue 的另一个重要概念，因为它是一种抽象，允许我们使用小型、独立和通常可复用的组件构建大型应用，因此，先定义好可重复使用的Luckysheet组件。代码如下：<template>  <div class="she_luckysheet vue
keywords: luckysheet vue
author: Liuyanyun-15 Csdn认证博客专家 Csdn认证企业博客 码龄8年 暂无认证
date: 2022-08-25T06:45:14.000Z
publisher: null
stats: paragraph=56 sentences=59, words=1312
---
Luckysheet ，一款纯前端类似excel的在线表格，功能强大、配置简单、完全开源。

配置文档 · API · 教程：[快速上手 | Luckysheet文档](https://mengshukeji.gitee.io/LuckysheetDocs/zh/guide/#%E5%9F%BA%E6%9C%AC%E4%BB%8B%E7%BB%8D "快速上手 | Luckysheet文档")

现将分享我在使用该组件过程中遇到的问题及解决方法。

第一步： **定义Luckysheet组件**：组件系统是 Vue 的另一个重要概念，因为它是一种抽象，允许我们使用小型、独立和通常可复用的组件构建大型应用，因此，先定义好可重复使用的Luckysheet组件。代码如下：

>

```html

export default {
  name: "HelloWorld",
  data() {
    return {};
  },
  mounted() {
    // In some cases, you need to use $nextTick
    // this.$nextTick(() => {
    $(function () {
      luckysheet.create({
        container: "luckysheet", // 设定DOM容器的id
        title: "Luckysheet Demo", // 设定表格名称
        lang: "zh", // 设定表格语言
        plugins: ["chart"],
        data: [
          {
            name: "", //工作表名称
            color: "#eee333", //工作表(工作表名称底部边框线)颜色
            index: 0, //工作表索引(新增一个工作表时该值是一个随机字符串)
            status: 1, //激活状态
            order: 0, //工作表的下标
            hide: 0, //是否隐藏
            row: 36, //行数
            column: 10, //列数
            defaultRowHeight: 28, //自定义行高,单位px
            defaultColWidth: 100, //自定义列宽,单位px
            celldata: [], //初始化使用的单元格数据,r代表行，c代表列，v代表该单元格的值，最后展示的是value1，value2
            config: {
              merge: {}, //合并单元格
              rowlen: {}, //表格行高
              columnlen: {}, //表格列宽
              rowhidden: {}, //隐藏行
              colhidden: {}, //隐藏列
              borderInfo: {}, //边框
              authority: {}, //工作表保护
            },

            scrollLeft: 0, //左右滚动条位置
            scrollTop: 0, //上下滚动条位置
            luckysheet_select_save: [], //选中的区域
            calcChain: [], //公式链
            isPivotTable: false, //是否数据透视表
            pivotTable: {}, //数据透视表设置
            filter_select: {}, //筛选范围
            filter: null, //筛选配置
            luckysheet_alternateformat_save: [], //交替颜色
            luckysheet_alternateformat_save_modelCustom: [], //自定义交替颜色
            luckysheet_conditionformat_save: {}, //条件格式
            frozen: {}, //冻结行列配置
            chart: [], //图表配置
            zoomRatio: 1, // 缩放比例
            image: [], //图片
            showGridLines: 1, //是否显示网格线
            dataVerification: {}, //数据验证配置
          },
        ],
        showtoolbar: false,
        showtoolbarConfig: {
          undoRedo: false, //撤销重做，注意撤消重做是两个按钮，由这一个配置决定显示还是隐藏
          paintFormat: false, //格式刷
          currencyFormat: false, //货币格式
          percentageFormat: false, //百分比格式
          numberDecrease: false, // '减少小数位数'
          numberIncrease: false, // '增加小数位数
          moreFormats: false, // '更多格式'
          font: false, // '字体'
          fontSize: false, // '字号大小'
          bold: false, // '粗体 (Ctrl+B)'
          italic: false, // '斜体 (Ctrl+I)'
          strikethrough: false, // '删除线 (Alt+Shift+5)'
          underline: false, // '下划线 (Alt+Shift+6)'
          textColor: false, // '文本颜色'
          fillColor: false, // '单元格颜色'
          border: false, // '边框'
          mergeCell: false, // '合并单元格'
          horizontalAlignMode: false, // '水平对齐方式'
          verticalAlignMode: false, // '垂直对齐方式'
          textWrapMode: false, // '换行方式'
          textRotateMode: false, // '文本旋转方式'
          image: false, // '插入图片'
          link: false, // '插入链接'
          chart: false, // '图表'（图标隐藏，但是如果配置了chart插件，右击仍然可以新建图表）
          postil: false, //'批注'
          pivotTable: false, //'数据透视表'
          function: false, // '公式'
          frozenMode: false, // '冻结方式'
          sortAndFilter: false, // '排序和筛选'
          conditionalFormat: false, // '条件格式'
          dataVerification: false, // '数据验证'
          splitColumn: false, // '分列'
          screenshot: false, // '截图'
          findAndReplace: false, // '查找替换'
          protection: false, // '工作表保护'
          print: false, // '打印'
        },
        showsheetbar: false, //是否显示底部sheet页按钮
        showsheetbarConfig: {
          add: false, //新增sheet
          menu: false, //sheet管理菜单
          sheet: false, //sheet页显示
        },
        showinfobar: false, //是否显示顶部信息栏
        showstatisticBar: false, //是否显示底部计数栏
        showstatisticBarConfig: {
          count: false, // 计数栏
          view: false, // 打印视图
          zoom: false, // 缩放
        },
        sheetFormulaBar: false, //是否显示公式栏
        allowCopy: false, //是否允许拷贝
        enableAddRow: true, //允许添加行
      });
    });

    // });
  },
};

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

```

页面样式效果是这样子的，可实现复制粘贴，excel的单元格下拉等表格操作，重点在于创建luckysheet实例时对实例属性的配置。![](https://img-blog.csdnimg.cn/e7e08c7d9c6b4aa39e219d7427503b35.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATGl1eWFueXVuLTE1,size_20,color_FFFFFF,t_70,g_se,x_16)

第二步： **引用组件**，在父组件中注册并引用组件。先import 组件，再components 中注册，最后在以标签的形式引用组件。代码如下：

```html

import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
  }
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

```

第三步： **父组件将接口返回的数据传递给子组件，子组件获取数据渲染数据**。代码如下：

父组件中获取数据并绑定参数传值。

```html

import HelloWorld from "./components/HelloWorld.vue";

export default {
  name: "App",
  components: {
    HelloWorld,
  },
  data() {
    return {
      sheetParams: {
        excelHeader: ["姓名", "年龄", "性别"],
        excelData: {
          姓名: ["张三", "赵兰", "李四"],
          年龄: ["18", "17", "20"],
          性别: ["男", "女", "男"],
        },
      },
    };
  },
};

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

```

子组件获取父组件传递的数据，渲染数据。

```html

export default {
  name: "HelloWorld",
  props: {
    sheetParams: {},
  },
  data() {
    return {
      luckysheetOption: {
          container: "luckysheet", // 设定DOM容器的id
          title: "Luckysheet Demo", // 设定表格名称
          lang: "zh", // 设定表格语言
          plugins: ["chart"],
          data: [
            {
              name: "", //工作表名称
              color: "#eee333", //工作表(工作表名称底部边框线)颜色
              index: 0, //工作表索引(新增一个工作表时该值是一个随机字符串)
              status: 1, //激活状态
              order: 0, //工作表的下标
              hide: 0, //是否隐藏
              row: 36, //行数
              column: 10, //列数
              defaultRowHeight: 28, //自定义行高,单位px
              defaultColWidth: 100, //自定义列宽,单位px
              celldata: [], //初始化使用的单元格数据,r代表行，c代表列，v代表该单元格的值，最后展示的是value1，value2
              config: {
                merge: {}, //合并单元格
                rowlen: {}, //表格行高
                columnlen: {}, //表格列宽
                rowhidden: {}, //隐藏行
                colhidden: {}, //隐藏列
                borderInfo: {}, //边框
                authority: {}, //工作表保护
              },

              scrollLeft: 0, //左右滚动条位置
              scrollTop: 0, //上下滚动条位置
              luckysheet_select_save: [], //选中的区域
              calcChain: [], //公式链
              isPivotTable: false, //是否数据透视表
              pivotTable: {}, //数据透视表设置
              filter_select: {}, //筛选范围
              filter: null, //筛选配置
              luckysheet_alternateformat_save: [], //交替颜色
              luckysheet_alternateformat_save_modelCustom: [], //自定义交替颜色
              luckysheet_conditionformat_save: {}, //条件格式
              frozen: {}, //冻结行列配置
              chart: [], //图表配置
              zoomRatio: 1, // 缩放比例
              image: [], //图片
              showGridLines: 1, //是否显示网格线
              dataVerification: {}, //数据验证配置
            },
          ],
          showtoolbar: false,
          showtoolbarConfig: {
            undoRedo: false, //撤销重做，注意撤消重做是两个按钮，由这一个配置决定显示还是隐藏
            paintFormat: false, //格式刷
            currencyFormat: false, //货币格式
            percentageFormat: false, //百分比格式
            numberDecrease: false, // '减少小数位数'
            numberIncrease: false, // '增加小数位数
            moreFormats: false, // '更多格式'
            font: false, // '字体'
            fontSize: false, // '字号大小'
            bold: false, // '粗体 (Ctrl+B)'
            italic: false, // '斜体 (Ctrl+I)'
            strikethrough: false, // '删除线 (Alt+Shift+5)'
            underline: false, // '下划线 (Alt+Shift+6)'
            textColor: false, // '文本颜色'
            fillColor: false, // '单元格颜色'
            border: false, // '边框'
            mergeCell: false, // '合并单元格'
            horizontalAlignMode: false, // '水平对齐方式'
            verticalAlignMode: false, // '垂直对齐方式'
            textWrapMode: false, // '换行方式'
            textRotateMode: false, // '文本旋转方式'
            image: false, // '插入图片'
            link: false, // '插入链接'
            chart: false, // '图表'（图标隐藏，但是如果配置了chart插件，右击仍然可以新建图表）
            postil: false, //'批注'
            pivotTable: false, //'数据透视表'
            function: false, // '公式'
            frozenMode: false, // '冻结方式'
            sortAndFilter: false, // '排序和筛选'
            conditionalFormat: false, // '条件格式'
            dataVerification: false, // '数据验证'
            splitColumn: false, // '分列'
            screenshot: false, // '截图'
            findAndReplace: false, // '查找替换'
            protection: false, // '工作表保护'
            print: false, // '打印'
          },
          showsheetbar: false, //是否显示底部sheet页按钮
          showsheetbarConfig: {
            add: false, //新增sheet
            menu: false, //sheet管理菜单
            sheet: false, //sheet页显示
          },
          showinfobar: false, //是否显示顶部信息栏
          showstatisticBar: false, //是否显示底部计数栏
          showstatisticBarConfig: {
            count: false, // 计数栏
            view: false, // 打印视图
            zoom: false, // 缩放
          },
          sheetFormulaBar: false, //是否显示公式栏
          allowCopy: false, //是否允许拷贝
          enableAddRow: true, //允许添加行
        }
    };
  },
  created() {},
  mounted() {
    this.initLuckysheet(this.sheetParams);
  },
  methods: {
    initLuckysheet(data) {
      var _this = this;//注意这里要重新指定下this对象。

      // In some cases, you need to use $nextTick
      // this.$nextTick(() => {
      $(function () {
        if (data.excelHeader.length != 0 && JSON.stringify(data.excelData) != "{}") {
          _this.luckysheetOption.hook = {
            workbookCreateAfter: function () {
              _this.dataRendSheet(data.excelHeader, data.excelData);
            },
          };
        }
        luckysheet.create(_this.luckysheetOption);
      });

      // });
    },
    /**接口数据回显 */
    dataRendSheet(excelHeader, excelData) {
      //回显表格表头，第一行
      if (excelHeader.length > 0) {
        excelHeader.forEach((item1, index1) => {
          luckysheet.setCellValue(0, index1, item1);
          //普通回显数据
          if (JSON.stringify(excelData) != "{}") {
            excelData[item1].forEach((item2, index2) => {
              var row = index2 + 1;
              luckysheet.setCellValue(row, index1, item2);
            });
          }
        });
      }
    },
  },
};

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

```

表格回显数据如图：

![](https://img-blog.csdnimg.cn/fb6ea8f6329d4d18bf66ae9062626220.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATGl1eWFueXVuLTE1,size_20,color_FFFFFF,t_70,g_se,x_16)

注意两点，第一数据格式，就是对象数组的转换，没什么难度；第二用到了Luckysheet的钩子函数，钩子函数的用法是在配置对象加一个hook对象，并给hook对象添加一个workbookCreateAfter方法，在工作簿创建好之后就执行这个函数的渲染数据的逻辑。

第四步： **父组件获取子组件修改后的数据。**父组件的布局和样式有调整，以对话框的形式来引用组件，点击按钮，获取数据。代码如下：

```html

          取 消
          确 定

import HelloWorld from "./components/HelloWorld.vue";

export default {
  name: "App",
  components: {
    HelloWorld,
  },
  data() {
    return {
      centerDialogVisible: true,
      closeOnClickModal: false,
      closeOnPressEscape: false,
      fullscreen: false,
      lockScroll: false,
      luckysheetName: "luckysheet数据编辑",

      sheetParams: {
        excelHeader: ["姓名", "年龄", "性别"],
        excelData: {
          姓名: ["张三", "赵兰", "李四"],
          年龄: ["18", "17", "20"],
          性别: ["男", "女", "男"],
        },
      },
      luckySheetData: {
        excelHeader: [],
        excelData: {},
      },
    };
  },
  methods: {
    saveSheetData() {
      var _this = this;
      _this.$refs.luckysheetRef.getSheetData(); //调用子组件获取sheet数据
      console.log(JSON.stringify(_this.luckySheetData));
      // document.getElementById("luckysheet-input-box").style.zIndex = "-1";
      // document.getElementsByClassName("luckysheet-cell-input").innerHTML = "";
      // _this.dialogFormVisible = false; //关闭对话框
    },
    //luckySheet数据接收
    receive: function (sheetTitle, commonData) {
      var _this = this;
      _this.luckySheetData.excelHeader = sheetTitle;
      _this.luckySheetData.excelData = commonData;
    },
    handleToClose() {
      this.centerDialogVisible = false;
    }
  },
};

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

```

页面效果如图：

![](https://img-blog.csdnimg.cn/955dc31ca42244759251974862956e27.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBATGl1eWFueXVuLTE1,size_20,color_FFFFFF,t_70,g_se,x_16)

点击确定按钮，获取的数据格式为：

{"excelHeader":["姓名","年龄","性别"],"excelData":[{"姓名":"张三","年龄":18,"性别":"男"},{"姓名":"赵兰","年龄":17,"性别":"女"},{"姓名":"李四","年龄":20,"性别":"男"}]}

注意一：如果想每次打开对话框都重新加载数据和渲染画布，则在对话框加上两行代码： **:visible.sync="centerDialogVisible"；v-if="centerDialogVisible"**

注意二：文件的引用放在public目录下的index.html文件中，luckysheet文件的引用路径一定要对，

```html

      We're sorry but  doesn't work properly without JavaScript enabled. Please enable it to continue.

```

如图：

![](https://img-blog.csdnimg.cn/47964691b1754c5cada28fba740c7a36.png)

好了，今天分享的关于Luckysheet组件在vue中使用的方法，就到这里了，希望能够帮助到大家。
