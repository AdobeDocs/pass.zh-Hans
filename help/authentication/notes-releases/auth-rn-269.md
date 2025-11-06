---
title: Adobe Pass Authentication 2.69发行说明
description: Adobe Pass Authentication 2.69发行说明
exl-id: d031c4c5-dbd5-4a77-b298-a53b992cc4c5
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 0%

---

# Adobe Pass Authentication 2.69发行说明 {#authn-269-rn}

>[!IMPORTANT]
>
> 确保随时了解汇总在[产品公告](/help/authentication/product-announcements.md)页中的最新Adobe Pass身份验证产品公告和停用时间表。

本页介绍了此版本的新增功能、更改和已知问题：

## 服务器端和Web客户端 {#server-side-web-clients-269}

* [内部版本号](#build-number-269)
* [发行版概述](#release-overview-269)

### 内部版本号 {#build-number-269}

Adobe Pass身份验证： adobe-pass-**2.69**

发行日期：**02/27/2024 - 02/29/2024**

### 发行版概述 {#release-overview-269}

#### 杂项

* 修补了安全漏洞。
* 增强了Dynamic Client Registration (DCR)重置Temp Pass安全层的功能。
   * 您可以在此处找到更多详细信息： [临时传递功能](../integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md)
* 对Platform Identification报表的增强。

#### REST API

* 正在开发新的REST API。
   * 即将发布的专用版本将引入新的端点和流程，这些端点和流程将在单独的通知中公告。
   * 更新文档以使用这些新API正在进行中。

#### 动态仪表板

* 正在开发新的TVE功能板。
   * 即将发布的专用版本将引入新的TVE功能板，该功能板将在单独的通知中宣布。
   * 正在更新文档以使用此新TVE功能板。

#### JavaScript SDK 4.7.0

* 由于安全漏洞，删除了已弃用的Access Enabler JavaScript SDK版本2.0.1。
   * 有关更多详细信息，请参阅链接： [Adobe Pass Authentication JavaScript 4.7.0发行说明](authn-rn-javascript-470.md)
