---
title: Adobe Pass Authentication 3.0.3发行说明
description: Adobe Pass Authentication 3.0.3发行说明
exl-id: f54b7c4a-78c5-4536-bed7-3c5f15640dea
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '148'
ht-degree: 0%

---

# Adobe Pass Authentication 3.0.3发行说明 {#authn-303-rn}

>[!IMPORTANT]
>
> 确保随时了解汇总在[产品公告](/help/authentication/product-announcements.md)页中的最新Adobe Pass身份验证产品公告和停用时间表。

本页介绍了此版本的新增功能、更改和已知问题：

## 服务器端和Web客户端 {#server-side-web-clients-303}

* [内部版本号](#build-number-303)
* [发行版概述](#release-overview-303)

### 内部版本号 {#build-number-303}

Adobe Pass身份验证： adobe-pass-**3.0.3**

发行日期：**2024年10月29日 — 2024年10月31日**

### 发行版概述 {#release-overview-303}

#### REST API v2

##### 代码

* REST API V2增强功能(由于Adobe Pass 3.0主要版本提供了[REST API V2](../integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md))。
* 在`notBefore`中添加了`notAfter`和`/sessions/{code}`字段以返回有关身份验证代码有效性的信息。
* 改进了平台识别。

##### 文档

* 要开始使用新的REST API v2，请参阅[REST API v2概述](../integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)文档。

##### 工具

* 要尝试使用新的REST API v2，请参阅[Adobe Developer](https://developer.adobe.com/adobe-pass)网站上的新Adobe Pass身份验证页面。
