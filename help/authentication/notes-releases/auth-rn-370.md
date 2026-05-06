---
title: Adobe Pass Authentication 3.7.0发行说明
description: Adobe Pass Authentication 3.7.0发行说明
hold: true
source-git-commit: 170d49b06e4ac8b31a840ee1bc5fac114bb3aa0b
workflow-type: tm+mt
source-wordcount: '142'
ht-degree: 0%

---

# Adobe Pass Authentication 3.7.0发行说明 {#authn-370-rn}

>[!IMPORTANT]
>
> 确保随时了解汇总在[产品公告](/help/authentication/product-announcements.md)页中的最新Adobe Pass身份验证产品公告和停用时间表。

本页介绍了此版本的新增功能、更改和已知问题：

## 服务器端和Web客户端 {#server-side-web-clients-370}

* [内部版本号](#build-number-370)
* [发行版概述](#release-overview-370)

### 内部版本号 {#build-number-370}

Adobe Pass身份验证： adobe-pass-**3.7.0.2**\
发行日期：**05/12/2026 - 05/14/2026**

### 发行版概述 {#release-overview-370}

此版本重点介绍MVPD集成升级、错误修复和TVE功能板改进。

#### MVPD集成

* 添加了对使用OAuth2和PKCE的Bell MVPD的支持。

#### 增强功能

* 发布了TVE功能板版本1.5.1。

#### 错误修复

* 修复了导致Apple SSO忽略某些MVPD配置不匹配的问题。
* 修复了由于响应标头中的无效字符而导致授权拒绝时出现HTTP 500错误的问题。
