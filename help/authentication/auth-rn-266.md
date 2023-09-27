---
title: Adobe Pass Authentication 2.66发行说明
description: Adobe Pass Authentication 2.66发行说明
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '148'
ht-degree: 0%

---

# Adobe Pass Authentication 2.66发行说明 {#authn-266-rn}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

本页介绍了此版本的新增功能、更改和已知问题：

## 服务器端和Web客户端 {#server-side-web-clients-266}

* [内部版本号](#build-number-266)
* [发行版概述](#release-overview-266)

### 内部版本号 {#build-number-266}

Adobe Pass身份验证： adobe-pass-**2.66.0.1**
发行日期： **07/11/2023 - 07/13/2023**

### 发行版概述 {#release-overview-266}

在此版本中，我们继续对新的REST API进行内部更新。

#### 错误修复 {#release-overview-bugfixes-266}

* 修复了基于SAML的MVPD的注销流程，其中注销请求中缺少RelayState参数。 我们将在发布后定位配置更新，以恢复受影响的MVPD的注销流程。
* 在SOAP授权端点的配置中添加了更新SSL证书的功能。
* 修复了某些ESM报表中的程序员字段中记录错误数据的极端情况。
