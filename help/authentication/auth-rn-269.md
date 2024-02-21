---
title: Adobe Pass Authentication 2.69发行说明
description: Adobe Pass Authentication 2.69发行说明
source-git-commit: 4417c5c50873c73c615f9845cadd4a42c26f3068
workflow-type: tm+mt
source-wordcount: '215'
ht-degree: 0%

---

# Adobe Pass Authentication 2.69发行说明 {#pt-authn-269-rn}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

本页介绍了此版本的新增功能、更改和已知问题：

## 服务器端和Web客户端 {#server-side-web-clients-269}

* [内部版本号](#build-number-269)
* [新增功能](#new-features-269)

### 内部版本号 {#build-number-269}

Adobe Pass身份验证： adobe-pass-**2.69**
发行日期： **02/27/2024 - 02/29/2024**

### 新增功能 {#new-features-269}

#### 杂项 {#misc}

* 修补了安全漏洞。
* 增强了Dynamic Client Registration (DCR)重置Temp Pass安全层的功能。
   * 您可以在此处找到更多详细信息： [重置临时传递](reset-temp-pass.md)
* 对Platform Identification报表的增强。

#### REST API {#rest-apis}

* 正在开发新的REST API。
   * 即将发布的专用版本将引入新的端点和流程，这些端点和流程将在单独的通知中公告。
   * 更新文档以使用这些新API正在进行中。

#### 动态仪表板 {#tve-dashboard}

* 正在开发新的TVE功能板。
   * 即将发布的专用版本将引入新的TVE功能板，该功能板将在单独的通知中宣布。
   * 正在更新文档以使用此新TVE功能板。

#### JavaScript SDK 4.7.0 {#js-sdk}

* 由于安全漏洞，删除了已弃用的Access Enabler JavaScript SDK版本2.0.1。
   * 有关更多详细信息，请访问链接： [Adobe Pass Authentication JavaScript 4.7.0发行说明](authn-rn-javascript-470.md)
