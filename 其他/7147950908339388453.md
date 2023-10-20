---
link: https://juejin.cn/post/7147950908339388453
title: base64转换为file文件类型（Demo）base64与文件对象相互转换深入浅出Base64 、Blob、File之间的相互转换java将base64图片转为file上传到服务器文件转base64字符串（ Jquery）
description: 项目中需要实现把图片的base64编码转成file文件的功能，然后再上传至服务器。起初是直接通过new File()的方式进行转换，在各个主流的浏览器基本上都支持
keywords: 前端
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2022-09-27T07:00:18.000Z
publisher: 稀土掘金
stats: paragraph=8 sentences=18, words=154
---
项目中需要实现把图片的base64编码转成file文件的功能，然后再上传至服务器。起初是直接通过new File()的方式进行转换，在各个主流的浏览器基本上都支持，Android也没问题，但是在ios系统埋了个坑，ios11.4以下的系统上传失败。定位bug发现是new File()这个方法不兼容ios系统，只能另辟蹊径，最后找到一个方法就是： **将base64转成blob ——> blob转成file**

1.通过new File()将base64转换成file文件，此方式需考虑浏览器兼容问题。

```ini
//将base64转换为文件
dataURLtoFile(dataurl, filename) {
    var arr = dataurl.split(','),
    mime = arr[0].match(/:(.*?)
    bstr = atob(arr[1]),
    n = bstr.length,
    u8arr = new Uint8Array(n)
    while (n--) {
        u8arr[n] = bstr.charCodeAt(n)
    }
    return new File([u8arr], filename, { type: mime })
}

//调用
var file = dataURLtoFile(base64Data, imgName)
复制代码
```

2.先将base64转换成blob，再将blob转换成file文件，此方法不存在浏览器不兼容问题。

```ini
//将base64转换为blob
dataURLtoBlob(dataurl) {
    var arr = dataurl.split(','),
    mime = arr[0].match(/:(.*?)
    bstr = atob(arr[1]),
    n = bstr.length,
    u8arr = new Uint8Array(n)
    while (n--) {
        u8arr[n] = bstr.charCodeAt(n)
    }
    return new Blob([u8arr], { type: mime })
}

//将blob转换为file
blobToFile(theBlob, fileName){
   theBlob.lastModifiedDate = new Date()
   theBlob.name = fileName
   return theBlob
},
//调用
var blob = dataURLtoBlob(base64Data)
var file = blobToFile(blob, imgName)

```
