---
title: Adobe Pass Authentication 2.70发行说明
description: Adobe Pass Authentication 2.70发行说明
exl-id: 81713f8e-bc51-4057-9b00-6a2d6c83cd02
source-git-commit: 6d46beb688299aeef2a7dbb013b3eca7b4509593
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 0%

---

# Adobe Pass Authentication 2.70发行说明 {#pt-authn-270-rn}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

本页介绍了此版本的新增功能、更改和已知问题：

## 服务器端和Web客户端 {#server-side-web-clients-270}

* [内部版本号](#build-number-270)
* [新增功能](#new-features-270)

### 内部版本号 {#build-number-270}

Adobe Pass身份验证： adobe-pass-**2.70**
发行日期： **04/23/2024 - 04/25/2024**

### 新增功能 {#new-features-270}

#### 杂项 {#misc}

* 修补了安全漏洞。
* 对降级API服务的增强。
   * 使用DCR作为降级API的安全机制。
   * 您可以在此处找到更多详细信息：[降级API概述](degradation-api-overview.md)

#### REST API {#rest-apis}

* 正在开发新的REST API。
   * 即将发布的专用版本将引入新的端点和流程，这些端点和流程将在单独的通知中公告。
   * 更新文档以使用这些新API正在进行中。
