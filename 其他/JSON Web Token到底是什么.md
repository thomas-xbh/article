---
link: null
title: JSON Web Token到底是什么-腾讯云开发者社区-腾讯云
description: JSON Web Token简称JWT，发音一般为”jot“，是一种标准，定义了在各方之间传输信息的URL安全方法。该标准遵循RFC-7519规范。
keywords: javascript
author: null
date: 2020-11-03T03:55:20.000Z
publisher: null
stats: paragraph=43 sentences=3, words=53
---
本文讲述JWT的组成及功能介绍。在本文中，我们将回答以下问题：

* **什么是JWT？**
* **JWT有哪些部分？**
* **JWT的主要作用是什么?*

下面是JWT的一个简单示例，为了便于阅读，插入了换行符和颜色：

实际上，JWT是由点分隔的三个字符串的串接。这些字符串中的每一个都是token的不同部分。

让我们看看如何获得token的最终版本。

JWT的三个部分是：

* **Header（头部）**
* **Payload（净荷）**
* **Signature（签名）*

其中，Signature（签名）是可选的。如果token不包含此部分，那么它是不安全的JWT。由于很少会使用不安全的JWT，因此在本文中，我将仅介绍包含signature的JWT。

除了signature，token还有另外两个部分：Header（头部）和payload（净荷）。这两部分最初都是用JSON编写的，因此被称为JSON Web Token。

为了实现上面展示的token最终版本，需要使用 **Base64Url**对header和payload进行单独编码。

Base64是一种编码算法，可以将任何字符编码成拉丁字母、数字、加号（+）和斜杠（/）。

Base64Url是Base64算法的修改版本。原来的Base64包含对文件名和URL无效的字符。相比之下，Base64Url修正了这一点，并允许JWT是URL安全的。

## **Header（头部）**

Header包含有关token本身的信息。它也被称为JOSE（框架）header。

关于header，以下是可用的header参数：

* **alg**：指定signature算法。该字段是必选的。对于不安全的JWT，该值应设置为none。
* **typ**：指明token的媒体类型。在本文示例中，应该是JWT。该字段是可选的。
* **cty**：描述内容类型。只有在payload包含嵌套token的情况下，才需要该属性。如果嵌套token是JWT，则这个参数应设置为JWT。该字段是可选的。

这是本文示例的header：

用Base64Url对header编码后，我们得到：

## **Payload（净荷）**

Payload是一组JWT声明（claims），即，提供有关方信息的名值对/键值对/属性值对。声明的名称为Claim Name，声明的值为Claim Value。

有三种JWT声明：

* **注册声明（registered claims）**
* **公有声明（public claims）**
* **私有声明（private claims）*

注册声明是指声明名称已在IANA JSON Web Token注册表中注册并在规范中定义的声明。 **这些声明都不是强制性的。**

注册声明名称包括：

iss（Issuer Claim）：JWT的签发人。

sub（Subject Claim）：JWT的主体。

aud（Audience Claim）：JWT的受众（接收对象）。

exp（Expiration Time Claim）：JWT的到期时间。该日期/时间是一个UNIX时间戳。规定的日期/时间必须在当前日期/时间之后。

nbf（Not Before Claim）：在此声明中定义的时间之前，JWT不接受处理。

iat（Issued At Claim）：JWT的签发时间。

jti（JWT ID Claim）：代表JWT唯一标识的字符串。

公有声明是指未在规范中定义但已在IANA JSON Web Token注册表中注册的声明，或者使用防冲突名称命名（例如，包含命名空间）的声明。

私有声明是指没有注册而是由JWT的消费者和生产者自定义的声明。正因为如此，存在名称冲突的可能性。

这是本文示例的payload：

用Base64Url对payload编码后，我们得到：

## **Signature（签名）**

Signature用于验证token。换句话说，它允许各方建立和验证token的真实性。然而，这并不能阻止其他方读取其内容。为了防止这种情况发生，需要对token进行加密。JSON Web Signature（JWS）遵循RFC-7515规范。

这是我们以伪代码获取signature的方式：

* alg是header中定义的算法。在本文示例中，它是HMAC + SHA256。
* secret 是 HMAC signature所需的共享秘钥。它通常由[服务器](https://cloud.tencent.com/product/cvm?from_column=20420&from=20420)持有，用于验证signature的真实性。在本文示例中，我们使用secret这个词。

我们signature的最终版本是这样的：

在此步骤之后，所有的token部分连接在一起并且由点（.）分隔，以形成我们token的最终版本。

JWT有几种作用，最常见的作用是认证。

以下是它的工作原理：
