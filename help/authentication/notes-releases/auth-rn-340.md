---
title: Adobe Pass Authentication 3.4.0发行说明
description: Adobe Pass Authentication 3.4.0发行说明
exl-id: ad572617-f607-419d-a085-70c025465080
source-git-commit: c9958a17ad9dfb518bab1d24087c85fdcb6fd057
workflow-type: tm+mt
source-wordcount: '185'
ht-degree: 0%

---

# Adobe Pass Authentication 3.4.0发行说明 {#authn-340-rn}

>[!IMPORTANT]
>
> 确保随时了解汇总在[产品公告](/help/authentication/product-announcements.md)页中的最新Adobe Pass身份验证产品公告和停用时间表。

本页介绍了此版本的新增功能、更改和已知问题：

## 服务器端和Web客户端 {#server-side-web-clients-340}

* [内部版本号](#build-number-340)
* [发行版概述](#release-overview-340)

### 内部版本号 {#build-number-340}

Adobe Pass身份验证： adobe-pass-**3.4.0**
发行日期： **09/16/2025 - 09/18/2025**

### 发行版概述 {#release-overview-340}

#### REST API v2

* 添加了对[Experience Cloud ID (ECID)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-visitor-identifier.md)的支持，以改进用户识别和跟踪功能。
* 已将REST API V2 [配置API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)的标头AP-Device-Identifier和X-Device-Info更改为可选标头。

#### 错误修复

* 修复了MVPD ID和代理MVPD ID在REST API V2 [决策](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)响应的令牌中反转的问题。
* 修复了REST API V2 [决策](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)响应的令牌中不存在请求者ID的问题。
* 修复了将过多资源传递到REST API V2 [预授权决策](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) API导致响应中的决策列表为空，而不是[增强型错误代码](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) **too_many_resources**&#x200B;的问题。

#### 杂项

* 修补了安全漏洞。
