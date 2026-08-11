---
title: Adobe Pass Authentication 3.8.0发行说明
description: Adobe Pass Authentication 3.8.0发行说明
source-git-commit: 7d3f430ccfa158c3da32512e6c6d3b6f189ee63c
workflow-type: tm+mt
source-wordcount: '153'
ht-degree: 0%

---

# Adobe Pass Authentication 3.8.0发行说明 {#authn-380-rn}

>[!IMPORTANT]
>
> 确保随时了解汇总在[产品公告](/help/authentication/product-announcements.md)页中的最新Adobe Pass身份验证产品公告和停用时间表。

本页介绍了此版本的新增功能、更改和已知问题：

## 服务器端和Web客户端 {#server-side-web-clients-380}

* [内部版本号](#build-number-380)
* [发行版概述](#release-overview-380)

### 内部版本号 {#build-number-380}

Adobe Pass身份验证： adobe-pass-**3.8.0**\
发行日期：**08/11/2026 - 08/13/2026**

### 发行版概述 {#release-overview-380}

此版本重点介绍Adobe Pass身份验证服务的稳定性、增强功能和安全更新。

#### 错误修复

* 修复了由于deviceId中的某些无效字符而导致V2 API出现HTTP 500错误的问题。

#### 增强功能

* 改进了刷新令牌处理以支持滚动令牌续订。
* 增强了分析辅助设备上的visitorId识别功能。
* 增强了URL参数验证，以加强安全控制并改善整体系统完整性。
* TVE功能板版本1.5.2，对UI进行了细微改进。
