---
link: https://juejin.cn/post/6898147400456601608
title: React 传送门 - 实现Dialog弹窗组件
description: protal不仅可以实现一些弹窗，对话框组件，在使用到地图Api,图表的业务场景下也是很实用的，一般而言，地图的层级一般是比较高的，对于地图封装的一些方法例如滚动，禁止滚动等场景下，地图的滚动事件和浏览的的滚动事件可能会发生冲突，此时可以通过将地图挂载其他的节点中，已避免事件冲…
keywords: React.js
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2020-11-23T02:55:04.000Z
publisher: 稀土掘金
stats: paragraph=23 sentences=31, words=208
---
通常情况下我们使用React封装的弹窗组件是直接挂载某个div节点上，有时候弹窗不能直接覆盖整个屏幕，不能满足我们的需求，在antd等ui组件中实现的Dialog是直接挂载在body节点下的，那是怎么实现的呢？

## 让我们来了解一下react中传送门的概念吧？

1. react v16之后，react就为开发者们提供了Protals, `&#x7528;&#x4E8E;&#x5B9E;&#x73B0;&#x5185;&#x5BB9;&#x4F20;&#x9001;&#x7684;&#x529F;&#x80FD;`，这是一个ReactDOM对象下的方法，接收两个参数 ReactDOM.createProtal(children, container);

* Children: 任何可渲染的React元素；
* Container: 需要被挂载的DOM元素；

1. Dialog对话框组件设计思路

* 创建Dialog组件实例时将组件挂载到body下，卸载组件时从body上移除组件实例

1. 具体实现

* 使用Dialog组件

```!javascript
import React, { useState } from 'react';
import Dialog from '../../components/Dialog/index';
import { Button } from 'antd';
import './index.less';

const UseDialog = () => {
    const [isShowDialog, setIsShowDialog] = useState(false);
    const toggleDialog = () => {
        setIsShowDialog(prev => !prev.isShowDialog);
    }
    const closeDialog = () => {
        setIsShowDialog(false);
    }
    const onSure = () => {
        console.log('确定...');
        setTimeout(() => {
            setIsShowDialog(false);
        }, 2000);
    }
    return (

            使用弹窗类组件
            {
                isShowDialog
                &&
                    具体内容请写在这里...

                }

    );
}

export default UseDialog;
```

* Dialog 实现

```!javascript
import React, { useEffect } from 'react';
import { createPortal } from 'react-dom';
import './index.less';
import { Divider, Button } from 'antd';

const Dialog = (props) => {
    const node = document.createElement('div');
    document.body.appendChild(node);
    useEffect(() => {
        return () => {
            document.body.removeChild(node);
        };
    }, []);
    return createPortal(

                {props.title}

                {props.children}

                    {props.cancelText || '取消'}
                    {props.sureText || '确定'}

        ,
        node
    )
}

export default Dialog;
```

* 最终效果预览

![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e16a6b4d0f3f4933a237f39c0b66f1cb~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

### 拓展

protal不仅可以实现一些弹窗，对话框组件，在使用到地图Api,图表的业务场景下也是很实用的，一般而言，地图的层级一般是比较高的，对于地图封装的一些方法例如滚动，禁止滚动等场景下，地图的滚动事件和浏览的的滚动事件可能会发生冲突，此时可以通过将地图挂载其他的节点中，已避免事件冲突。
