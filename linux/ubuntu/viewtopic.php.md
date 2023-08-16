---
link: null
title: [已解决]通过clash设置好代理后，火狐可以访问但edge无法访问相关网站 / 网络相关 / Arch Linux 中文论坛
description: null
keywords: null
author: null
date: null
publisher: null
stats: paragraph=4 sentences=19, words=24
---
刚发现HTTP代理填错了，应该是127.0.0.1:7890，但是即便改正也没有在edge中起效

这个是我的代理设置，但是没有写入环境变量
curl访问youtube无回应
edge版本为 96.0.1054.62

更新：
export了如下内容：
HTTPS_PROXY=http://127.0.0.1:7890/
HTTP_PROXY=http://127.0.0.1:7890/
http_proxy=http://127.0.0.1:7890/
https_proxy=http://127.0.0.1:7890/
后也没有效果，删去http://等内容只保留地址和端口也是

_最近编辑记录 Yukiko (2022-02-05 13:31:10)_
