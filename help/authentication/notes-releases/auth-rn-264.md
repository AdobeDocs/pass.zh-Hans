---
title: Adobe Pass Authentication 2.64发行说明
description: Adobe Pass Authentication 2.64发行说明
exl-id: 4db21026-a0c2-4e33-b01f-4ccae824a110
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 0%

---

# Adobe Pass Authentication 2.64发行说明 {#authn-264-rn}

>[!IMPORTANT]
>
> 确保随时了解汇总在[产品公告](/help/authentication/product-announcements.md)页中的最新Adobe Pass身份验证产品公告和停用时间表。

本页介绍了此版本的新增功能、更改和已知问题：

## 服务器端和Web客户端 {#server-side-web-clients-264}

* [内部版本号](#build-number-264)
* [发行版概述](#release-overview-264)

### 内部版本号 {#build-number-264}

Adobe Pass身份验证： adobe-pass-**2.64**

发行日期：**2022年11月8日 — 2022年11月10日**

### 发行版概述 {#release-overview-264}

* 基础架构更新，旨在缩短服务器响应时间，提高系统整体性能。
* 改进了新的平台识别机制。
* 能够阻止来自MVPD的未经请求的身份验证响应，其中SAML断言中缺少“in_response_to”参数。

#### 错误修复

* 修复了某些旧版TempPass令牌的格式不正确的问题。
* 解决了第二个屏幕身份验证流程中的轻微问题。
