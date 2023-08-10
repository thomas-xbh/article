---
link: https://juejin.cn/post/7076268542437359652
title: wireshark抓包新手使用教程
description: wireshark抓包新手使用教程    Wireshark是非常流行的网络封包分析软件，可以截取各种网络数据包，并显示数据包详细信息。常用于开发测试过程各种问题定位。本文主要内容包括：   1、Wi
keywords: 前端
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2022-03-18T02:56:37.000Z
publisher: 稀土掘金
stats: paragraph=73 sentences=65, words=162
---
Wireshark是非常流行的网络封包分析软件，可以截取各种网络数据包，并显示数据包详细信息。常用于开发测试过程各种问题定位。本文主要内容包括：

1、Wireshark软件下载和安装以及Wireshark主界面介绍。

2、WireShark简单抓包示例。通过该例子学会怎么抓包以及如何简单查看分析数据包内容。

3、Wireshark过滤器使用。通过过滤器可以筛选出想要分析的内容。包括按照协议过滤、端口和主机名过滤、数据包内容过滤。

# Wireshark软件安装

软件下载路径：[wireshark官网](https://link.juejin.cn?target=https%3A%2F%2Fwww.wireshark.org%2F "https://www.wireshark.org/")。按照系统版本选择下载，下载完成后，按照软件提示一路Next安装。

如果你是Win10系统，安装完成后，选择抓包但是不显示网卡，下载win10pcap兼容性安装包。下载路径：[win10pcap兼容性安装包](https://link.juejin.cn?target=http%3A%2F%2Fwww.win10pcap.org%2Fdownload%2F "http://www.win10pcap.org/download/")

# **Wireshark 开始抓包示例**

先介绍一个使用wireshark工具抓取ping命令操作的示例，让读者可以先上手操作感受一下抓包的具体过程。

1、打开wireshark 2.6.5，主界面如下：

2、选择菜单栏上Capture -> Option，勾选WLAN网卡（这里需要根据各自电脑网卡使用情况选择，简单的办法可以看使用的IP对应的网卡）。点击Start。启动抓包。

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/37debbfce82045409dde318a1b378f7f~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

3、wireshark启动后，wireshark处于抓包状态中。

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/dfafd7c90eb3436191ed9a97c88bbf4c~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

4、执行需要抓包的操作，如ping [www.baidu.com。](https://link.juejin.cn?target=http%3A%2F%2Fwww.baidu.com%25E3%2580%2582 "http://www.baidu.com%E3%80%82")

5、操作完成后相关数据包就抓取到了。为避免其他无用的数据包影响分析，可以通过在过滤栏设置过滤条件进行数据包列表过滤，获取结果如下。说明：ip.addr == 119.75.217.26 and icmp 表示只显示ICPM协议且源主机IP或者目的主机IP为119.75.217.26的数据包。

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f74b87d484484d719bb12fbe1382a087~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

5、wireshark抓包完成，就这么简单。关于wireshark过滤条件和如何查看数据包中的详细内容在后面介绍。

# Wireshakr抓包界面

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c213ab5e2fba4fcdb0e5471f20dcafa2~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

说明：数据包列表区中不同的协议使用了不同的颜色区分。协议颜色标识定位在菜单栏View --> Coloring Rules。如下所示

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/222b4c7b994b467d9b2fd43ca71dc405~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

**WireShark 主要分为这几个界面**

1. Display Filter(显示过滤器)， 用于设置过滤条件进行数据包列表过滤。菜单路径：Analyze --> Display Filters。\

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d8ba739371c44ec38d0c6b12918dec87~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

2. Packet List Pane(数据包列表)， 显示捕获到的数据包，每个数据包包含编号，时间戳，源地址，目标地址，协议，长度，以及数据包信息。 不同协议的数据包使用了不同的颜色区分显示。\

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7e525c08a8984c13b1243c2f16314bbb~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

3. Packet Details Pane(数据包详细信息), 在数据包列表中选择指定数据包，在数据包详细信息中会显示数据包的所有详细信息内容。数据包详细信息面板是最重要的，用来查看协议中的每一个字段。各行信息分别为

（1）Frame: 物理层的数据帧概况

（2）Ethernet II: 数据链路层以太网帧头部信息

（3）Internet Protocol Version 4: 互联网层IP包头部信息

（4）Transmission Control Protocol: 传输层T的数据段头部信息，此处是TCP

（5）Hypertext Transfer Protocol: 应用层的信息，此处是HTTP协议

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/49815057609940acad05aa6fe84524b2~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

**TCP包的具体内容**

从下图可以看到wireshark捕获到的TCP包中的每个字段。

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a86f6f726abe4cecbfa01f695f546a6e~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

4. Dissector Pane(数据包字节区)。

# Wireshark过滤器设置

初学者使用wireshark时，将会得到大量的冗余数据包列表，以至于很难找到自己自己抓取的数据包部分。wireshar工具中自带了两种类型的过滤器，学会使用这两种过滤器会帮助我们在大量的数据中迅速找到我们需要的信息。

（1）抓包过滤器

捕获过滤器的菜单栏路径为Capture --> Capture Filters。用于 **在抓取数据包前设置。**

如何使用？可以在抓取数据包前设置如下。

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/52a5e832f1cf4c71a47bfdd835ddb8f2~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

ip host 60.207.246.216 and icmp表示只捕获主机IP为60.207.246.216的ICMP数据包。获取结果如下：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c96f8e738573450592324bc92162ee1d~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

（2）显示过滤器

显示过滤器是用于在抓取数据包后设置过滤条件进行过滤数据包。通常是在抓取数据包时设置条件相对宽泛，抓取的数据包内容较多时使用显示过滤器设置条件顾虑以方便分析。同样上述场景，在捕获时未设置捕获规则直接通过网卡进行抓取所有数据包，如下

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e0d9325298414f5bbb195bf8f261047b~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

执行ping [www.huawei.com获取的数据包列表如下](https://link.juejin.cn?target=http%3A%2F%2Fwww.huawei.com%25E8%258E%25B7%25E5%258F%2596%25E7%259A%2584%25E6%2595%25B0%25E6%258D%25AE%25E5%258C%2585%25E5%2588%2597%25E8%25A1%25A8%25E5%25A6%2582%25E4%25B8%258B "http://www.huawei.com%E8%8E%B7%E5%8F%96%E7%9A%84%E6%95%B0%E6%8D%AE%E5%8C%85%E5%88%97%E8%A1%A8%E5%A6%82%E4%B8%8B")

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5046d4a61a3d4f8d988a2f18d55337cc~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

观察上述获取的数据包列表，含有大量的无效数据。这时可以通过设置显示器过滤条件进行提取分析信息。ip.addr == 211.162.2.183 and icmp。并进行过滤。

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/23bf1db9b01e44699998193e6dfb2b13~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.image)

****上述介绍了抓包过滤器和显示过滤器的基本使用方法。 **在组网不复杂或者流量不大情况下，使用显示器过滤器进行抓包后处理就可以满足我们使用。** 下面介绍一下两者间的语法以及它们的区别。

**wireshark过滤器表达式的规则**

1、抓包过滤器语法和实例\

抓包过滤器类型Type（host、net、port）、方向Dir（src、dst）、协议Proto（ether、ip、tcp、udp、http、icmp、ftp等）、逻辑运算符（&& 与、|| 或、！非）

（1）协议过滤\

比较简单，直接在抓包过滤框中直接输入协议名即可。

TCP，只显示TCP协议的数据包列表

HTTP，只查看HTTP协议的数据包列表

ICMP，只显示ICMP协议的数据包列表

（2）IP过滤

host 192.168.1.104

src host 192.168.1.104

dst host 192.168.1.104

（3）端口过滤

port 80

src port 80

dst port 80

（4）逻辑运算符&& 与、|| 或、！非

src host 192.168.1.104 && dst port 80 抓取主机地址为192.168.1.80、目的端口为80的数据包

host 192.168.1.104 || host 192.168.1.102 抓取主机为192.168.1.104或者192.168.1.102的数据包

！broadcast 不抓取广播数据包

2、显示过滤器语法和实例

（1）比较操作符

比较操作符有== 等于、！= 不等于、> 大于、< 小于、>= 大于等于、
