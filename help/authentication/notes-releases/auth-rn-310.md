---
title: Adobe Pass Authentication 3.1.0发行说明
description: Adobe Pass Authentication 3.1.0发行说明
source-git-commit: 4ad5ea619f64a78a72f69228c9ae3c83a7b66f24
workflow-type: tm+mt
source-wordcount: '192'
ht-degree: 0%

---

# Adobe Pass Authentication 3.1.0发行说明 {#authn-310-rn}

>[!IMPORTANT]
>
> 确保随时了解汇总在[产品公告](/help/authentication/product-announcements.md)页中的最新Adobe Pass身份验证产品公告和停用时间表。

本页介绍了此版本的新增功能、更改和已知问题：

## 服务器端和Web客户端 {#server-side-web-clients-310}

* [内部版本号](#build-number-310)
* [发行版概述](#release-overview-310)

### 内部版本号 {#build-number-310}

Adobe Pass身份验证： adobe-pass-**3.1.0**

发行日期：**02/25/2025 - 02/27/2025**

### 发行版概述 {#release-overview-310}

#### REST API v2

* 新的`partner_logout`操作名称和`partner_interactive`操作类型用于区分REST API v2 [注销API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)响应中的常规注销和合作伙伴单点登录。
* 新`reason`字段，用于在REST API v2 [会话API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)和[会话SSO API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md)响应中提供对操作名称的更多分析。

#### 错误修复

* 修复了阻止频谱订阅者通过REST API v2 [身份验证API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)进行身份验证的问题。
* 修复了导致通过REST API V2 [身份验证API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)生成的事件无法在ESM中正确聚合的问题。
* 修复了导致在REST API v2 [配置文件API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)响应中错误计算用户配置文件`notBefore`时间戳的问题。

#### JavaScript SDK

* 删除了[AccessEnabler JavaScript SDK](authn-rn-javascript-471.md)的3.5.0版本。