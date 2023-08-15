---
link: https://www.myfreax.com/how-to-install-sublime-text-3-on-ubuntu-20-04/
title: 如何在Ubuntu 20.04上安装Sublime Text 3 | myfreax
description: Sublime Text 是用于Web和软件开发的流行文本和源代码编辑器。本文介绍了如何在Ubuntu 20.04上安装Sublime Text 3
keywords: linux,ubuntu,sublime text
author: Myfreax 微信公众号 支付宝打赏
date: 2021-01-11T14:19:06.000Z
publisher: myfreax
stats: paragraph=11 sentences=9, words=63
---
[Sublime Text](https://www.sublimetext.com/)是用于Web和软件开发的流行文本与源代码编辑器。 它非常快，并且具有许多强大的功能。 可以通过安装插件和创建自定义设置来增强Sublime Text。

本教程介绍了如何在Ubuntu 20.04上安装Sublime Text 3。 在Ubuntu上安装Sublime非常简单。 我们将启用Sublime存储库，导入存储库GPG密钥，然后安装编辑器。 相同的说明应适用于基于Debian的Linux发行版。

Sublime Text是专有应用程序。 可以通过下载评估版本。 但是，如果您连续使用，则需要购买许可证。 评估版本没有强制执行的时限。

## 在Ubuntu 20.04上安装Sublime Text 3

```bash
sudo apt update
sudo apt install dirmngr gnupg apt-transport-https ca-certificates software-properties-common
```

```bash
curl -fsSL https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -
sudo add-apt-repository "deb https://download.sublimetext.com/ apt/stable/"
```

启用存储库后，通过键入以下命令安装Sublime Text 3：

```bash
sudo apt update
sudo apt install sublime-text
```

您已经在Ubuntu 20.04桌面上安装了Sublime Text 3。当发布新版本时，您可以通过桌面的软件中心来更新Sublime软件包。

## 启动Sublime

您可以从终端通过输入 `subl`或单击Sublime图标（ `Activities -> Sublime`）启动Sublime Text编辑器：
