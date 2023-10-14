---
link: https://juejin.cn/post/6844904070021382158
title: Nodejs使用nodemailer发送邮件「nodemailer」Node 邮件发送模块JavaScript利用Nodemailer发送电子邮件Node.js使用Nodemailer发送邮件通知使用Nodemailer调用自己的邮箱发送邮件（简单）
description: 这里使用的是qq邮箱，因为qq邮箱的权限比较好设置一些。
keywords: Node.js
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2020-02-22T11:48:45.000Z
publisher: 稀土掘金
stats: paragraph=23 sentences=24, words=544
---
这里使用的是qq邮箱，因为qq邮箱的权限比较好设置一些。

## 安装模块

cnpm i nodemailer -S

## 创建-个SMTP客户端配置

```
 //&#x5F15;&#x5165;&#x6A21;&#x5757; nodemailer
 const nodemailer = require(<span class="hljs-string">'nodemailer'</span>)

 // &#x521B;&#x5EFA;&#x4E00;&#x4E2A;SMTP&#x5BA2;&#x6237;&#x7AEF;&#x914D;&#x7F6E;
 const config = {
     service: <span class="hljs-string">"QQ"</span>,
     auth: {
         // &#x53D1;&#x4EF6;&#x4EBA;&#x90AE;&#x7BB1;&#x8D26;&#x53F7;
         user: <span class="hljs-string">'xxxxxx@qq.com'</span>,
         //&#x53D1;&#x4EF6;&#x4EBA;&#x90AE;&#x7BB1;&#x7684;&#x6388;&#x6743;&#x7801; &#x8FD9;&#x91CC;&#x53EF;&#x4EE5;&#x901A;&#x8FC7;qq&#x90AE;&#x7BB1;&#x83B7;&#x53D6; &#x5E76;&#x4E14;&#x4E0D;&#x552F;&#x4E00;
         pass: <span class="hljs-string">'xxxxxxxxxxx'</span>
     }
 }
```

## 创建一个SMTP客户端配置对象

```
 const transporter = nodemailer.createTransport(config)
```

## 创建一个收件人对象

```
 // &#x9A8C;&#x8BC1;&#x7801;&#x968F;&#x673A;&#x6570;
 <span class="hljs-built_in">let</span> code = Math.random().toString().substr(2, 4)
 const mail = {
     // &#x53D1;&#x4EF6;&#x4EBA; &#x90AE;&#x7BB1;  <span class="hljs-string">'&#x6635;&#x79F0;<发件人邮箱>'</发件人邮箱></span>
     from: `<span class="hljs-string">"web"</span><xxxx@qq.com>`,
     // &#x4E3B;&#x9898;
     subject: <span class="hljs-string">'&#x6FC0;&#x6D3B;&#x9A8C;&#x8BC1;&#x7801;'</span>,
     // &#x6536;&#x4EF6;&#x4EBA; &#x7684;&#x90AE;&#x7BB1; &#x53EF;&#x4EE5;&#x662F;&#x5176;&#x4ED6;&#x90AE;&#x7BB1; &#x4E0D;&#x4E00;&#x5B9A;&#x662F;qq&#x90AE;&#x7BB1;
     to: <span class="hljs-string">''</span>,
     //&#x8FD9;&#x91CC;&#x53EF;&#x4EE5;&#x6DFB;&#x52A0;html&#x6807;&#x7B7E;
     html: `<b>&#x60A8;&#x7684;&#x6FC0;&#x6D3B;&#x9A8C;&#x8BC1;&#x7801;&#x4E3A;&#xFF1A;<span class="hljs-variable">${code}</span>, &#x8BF7;24&#x5C0F;&#x65F6;&#x5185;&#x6709;&#x6548;&#xFF0C;&#x8BF7;&#x8C28;&#x614E;&#x4FDD;&#x7BA1;&#x3002;</b>`
 }
</xxxx@qq.com>
```

## 发送邮件 调用transporter.sendMail(mail, callback)

```
 transporter.sendMail(mail, <span class="hljs-keyword">function</span>(error, info) {
         <span class="hljs-keyword">if</span> (error) {
             <span class="hljs-built_in">return</span> console.log(error);
         }
         transporter.close()
         console.log(<span class="hljs-string">'mail sent:'</span>, info.response)
     })
```

## qq权限的设置

## 最后就可以愉快的可以发送邮件啦

## 完整代码演示

```
 //&#x5F15;&#x5165;&#x6A21;&#x5757; nodemailer
 const nodemailer = require(<span class="hljs-string">'nodemailer'</span>)

 // &#x9A8C;&#x8BC1;&#x7801;&#x968F;&#x673A;&#x4E66;
 <span class="hljs-built_in">let</span> code = Math.random().toString().substr(2, 4)

 // &#x521B;&#x5EFA;&#x4E00;&#x4E2A;SMTP&#x5BA2;&#x6237;&#x7AEF;&#x914D;&#x7F6E;
 const config = {
     service: <span class="hljs-string">"QQ"</span>,
     auth: {
         // &#x53D1;&#x4EF6;&#x4EBA;&#x90AE;&#x7BB1;&#x8D26;&#x53F7;
         user: <span class="hljs-string">'xxxxxxxxx@qq.com'</span>,
         //&#x53D1;&#x4EF6;&#x4EBA;&#x90AE;&#x7BB1;&#x7684;&#x6388;&#x6743;&#x7801; &#x8FD9;&#x91CC;&#x53EF;&#x4EE5;&#x901A;&#x8FC7;qq&#x90AE;&#x7BB1;&#x83B7;&#x53D6; &#x5E76;&#x4E14;&#x4E0D;&#x552F;&#x4E00;
         pass: <span class="hljs-string">'xxxxxxxxxxxxxxxxxxxxxx'</span>   //&#x6388;&#x6743;&#x7801;&#x751F;&#x6210;&#x4E4B;&#x540E;&#xFF0C;&#x8981;&#x7B49;&#x4E00;&#x4F1A;&#x624D;&#x80FD;&#x4F7F;&#x7528;&#xFF0C;&#x5426;&#x5219;&#x9A8C;&#x8BC1;&#x7684;&#x65F6;&#x5019;&#x4F1A;&#x62A5;&#x9519;&#xFF0C;&#x4F46;&#x662F;&#x4E0D;&#x8981;&#x614C;&#x5F20;&#x54E6;
     }
 }

 //&#x521B;&#x5EFA;&#x4E00;&#x4E2A;SMTP&#x5BA2;&#x6237;&#x7AEF;&#x914D;&#x7F6E;&#x5BF9;&#x8C61;
 const transporter = nodemailer.createTransport(config)

 //&#x521B;&#x5EFA;&#x4E00;&#x4E2A;&#x6536;&#x4EF6;&#x4EBA;&#x5BF9;&#x8C61;
 const mail = {
     // &#x53D1;&#x4EF6;&#x4EBA; &#x90AE;&#x7BB1;  <span class="hljs-string">'&#x6635;&#x79F0;<发件人邮箱>'</发件人邮箱></span>
     from: `<span class="hljs-string">"web"</span><xxxxxxxxxx@qq.com>`,
     // &#x4E3B;&#x9898;
     subject: <span class="hljs-string">'&#x6FC0;&#x6D3B;&#x9A8C;&#x8BC1;&#x7801;'</span>,
     // &#x6536;&#x4EF6;&#x4EBA; &#x7684;&#x90AE;&#x7BB1; &#x53EF;&#x4EE5;&#x662F;&#x5176;&#x4ED6;&#x90AE;&#x7BB1; &#x4E0D;&#x4E00;&#x5B9A;&#x662F;qq&#x90AE;&#x7BB1;
     to: <span class="hljs-string">'xxxxxxx@163.com'</span>,
     //&#x8FD9;&#x91CC;&#x53EF;&#x4EE5;&#x6DFB;&#x52A0;html&#x6807;&#x7B7E;
     html: `<b>&#x60A8;&#x7684;&#x6FC0;&#x6D3B;&#x9A8C;&#x8BC1;&#x7801;&#x4E3A;&#xFF1A;<span class="hljs-variable">${code}</span>, &#x8BF7;24&#x5C0F;&#x65F6;&#x5185;&#x6709;&#x6548;&#xFF0C;&#x8BF7;&#x8C28;&#x614E;&#x4FDD;&#x7BA1;&#x3002;</b>`
 }

 //  &#x53D1;&#x9001;&#x90AE;&#x4EF6; &#x8C03;&#x7528;transporter.sendMail(mail, callback)
 transporter.sendMail(mail, <span class="hljs-keyword">function</span>(error, info) {
         <span class="hljs-keyword">if</span> (error) {
             <span class="hljs-built_in">return</span> console.log(error);
         }
         transporter.close()
         console.log(<span class="hljs-string">'mail sent:'</span>, info.response)
     })

</xxxxxxxxxx@qq.com>
```
