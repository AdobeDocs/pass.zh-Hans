---
title: Adobe Pass Authentication 3.0发行说明
description: Adobe Pass Authentication 3.0发行说明
source-git-commit: 82fd63a0e208a90753235b81fa52757283c9d314
workflow-type: tm+mt
source-wordcount: '199'
ht-degree: 0%

---

# Adobe Pass Authentication 3.0发行说明 {#pt-authn-300-rn}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

本页介绍了此版本的新增功能、更改和已知问题：

## 服务器端和Web客户端 {#server-side-web-clients-300}

* [内部版本号](#build-number-300)
* [发行版概述](#release-overview-300)

### 内部版本号 {#build-number-2651}

Adobe Pass身份验证： adobe-pass-**3.0**
发行日期： **09/10/2024 - 09/12/2024**

### 新增功能 {#new-features-300}

#### REST API v2 {#rest-apis}

##### 代码

* 从adobe-pass-**3.0**&#x200B;版本开始，新的和现有的客户端应用程序可以集成或迁移到新的REST API v2，以从最新的Adobe Pass功能中受益。

##### 文档

* 要开始使用新的REST API v2，请参阅以下文档：
   * [REST API v2 - API — 概述](./rest-api-v2/apis/rest-api-v2-apis-overview.md)
   * [REST API v2 — 流 — 概述](./rest-api-v2/flows/rest-api-v2-flows-overview.md)
* REST API v1公共文档的URL已更改，请参阅以下文档：
   * [REST API v1 - API — 概述](./rest-api-overview.md)
   * [REST API v1 - API — 参考](./rest-api-reference.md)

##### 工具

* 要尝试使用新的REST API v2，请参阅[Adobe Developer](https://developer.adobe.com/adobe-pass)网站上的新Adobe Pass身份验证页面。

### 错误修复 {#bug-fixes-300}

* 修复了注销请求中存在时未使用重定向URL参数的问题。