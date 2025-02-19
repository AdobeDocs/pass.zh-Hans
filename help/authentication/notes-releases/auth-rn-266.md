---
title: Adobe Pass Authentication 2.66发行说明
description: Adobe Pass Authentication 2.66发行说明
exl-id: 7c3cd007-ed2b-455f-8f70-6ec5d0a6552a
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '150'
ht-degree: 0%

---

# Adobe Pass Authentication 2.66发行说明 {#authn-266-rn}

>[!IMPORTANT]
>
> 确保随时了解汇总在[产品公告](/help/authentication/product-announcements.md)页中的最新Adobe Pass身份验证产品公告和停用时间表。

本页介绍了此版本的新增功能、更改和已知问题：

## 服务器端和Web客户端 {#server-side-web-clients-266}

* [内部版本号](#build-number-266)
* [发行版概述](#release-overview-266)

### 内部版本号 {#build-number-266}

Adobe Pass身份验证： adobe-pass-**2.66.0.1**

发行日期：**07/11/2023 - 07/13/2023**

### 发行版概述 {#release-overview-266}

在此版本中，我们继续对新的REST API进行内部更新。

#### 错误修复

* 修复了基于SAML的MVPD的注销流程，其中注销请求中缺少RelayState参数。 我们将在发布后定位配置更新，以恢复受影响的MVPD的注销流程。
* 添加了在SOAP授权端点的配置中更新SSL证书的功能。
* 修复了某些ESM报表中的程序员字段中记录错误数据的极端情况。
