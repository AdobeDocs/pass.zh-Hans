---
title: 产品公告
description: 产品公告
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '670'
ht-degree: 1%

---

# 产品公告 {#product-announcements}

## 生命周期结束(EOL) {#eol}

本节概述了某些计划停用的Adobe Pass身份验证功能和产品的支持终止日期和生命周期终止日期。

确保随时了解停用时间表，并计划采取下面列出的相应措施。

| 公告 | 公告日期 | 支持终止日期 | 生命周期结束日期 | 描述 |
|-----------------------------------------------------------------|-------------------|---------------------|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Adobe Pass Authentication AccessEnabler JavaScript SDK v3.5生命周期结束 | 12/11/2024 | 不适用 | 01/08/2025 | 我们计划于&#x200B;**2025年1月8日**&#x200B;结束Adobe Pass Authentication AccessEnabler JavaScript SDK v3.5的生命周期。 在此日期之后，AccessEnabler JavaScript SDK v3.5提供的功能和服务将无法再正常使用，包括用户身份验证和授权。 |
| Adobe Pass身份验证非DCR安全机制EOL | 12/11/2024 | 不适用 | 01/20/2025 | 我们计划于&#x200B;**2025年1月20日**&#x200B;终止Adobe Pass身份验证非DCR安全机制的生命周期。 此机制用于保护对以下Adobe Pass身份验证API的访问：<ul><li><a href="/help/authentication/integration-guide-programmers/features-premium/temporary-access/reset-temp-pass.md">重置临时传递API</a></li><li><a href="/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-api-overview.md">降级API</a></li><li><a href="/help/authentication/integration-guide-mvpds/proxy-mvpd-webserv.md">代理MVPD API</a></li><li><a href="/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-api.md">授权服务监控API</a></li></ul>在此日期之后，通过此安全机制访问的上述API提供的功能和服务将不再起作用。</br></br>为确保继续提供服务，您需要迁移所有应用程序以使用[动态客户端注册](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)机制。 |
| Adobe Pass身份验证REST API v1生命周期结束 | 12/11/2024 | 12/31/2025 | 2026年底（待定） | 我们计划于&#x200B;**2025年12月31日**&#x200B;停止对Adobe Pass身份验证REST API v1的支持。 在此日期之后，将不再提供更新或修复。</br></br>为确保继续获得支持，您需要将所有应用程序迁移到<a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md">REST API v2</a>。</br></br>我们计划在2026年底&#x200B;**之前**&#x200B;结束Adobe Pass身份验证REST API v1的生命周期。 在此日期之后，REST API v1提供的功能和服务将无法再正常使用，包括用户身份验证和授权。</br></br>为确保继续提供服务，您需要将所有应用程序迁移到<a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md">REST API v2</a>。 |
| Adobe Pass Authentication AccessEnabler SDK生命周期结束 | 12/11/2024 | 05/31/2026 | 2026年底（待定） | 我们计划于2026年5月31日&#x200B;**停止支持Adobe Pass Authentication AccessEnabler SDK**。 在此日期之后，将不再提供更新或修复。</br></br>为确保继续获得支持，您需要将所有应用程序迁移到<a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md">REST API v2</a>。</br></br>我们计划在2026年底&#x200B;**之前**&#x200B;结束Adobe Pass Authentication AccessEnabler SDK的生命周期。 在此日期之后，AccessEnabler SDK提供的功能和服务（包括用户身份验证和授权）将无法再正常使用。</br></br>为确保继续提供服务，您需要将所有应用程序迁移到<a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md">REST API v2</a>。 |

## 产品版本 {#product-releases}

此部分编译对Adobe Pass身份验证的发行历史记录和相应发行说明的引用。

### 2024 {#product-releases-2024}

| 发行说明 | 日期 |
|-----------------------------------------------------------------------------------------------------------|-------------------------|
| [Adobe Pass Authentication 3.0.3发行说明](notes-releases/auth-rn-303.md) | 10/29/2024 - 10/31/2024 |
| [Adobe Pass Authentication 3.0发行说明](notes-releases/auth-rn-300.md) | 09/10/2024 - 09/12/2024 |
| [Adobe Pass Authentication 2.70发行说明](notes-releases/auth-rn-270.md) | 04/23/2024 - 04/25/2024 |
| [Adobe Pass Authentication 2.69发行说明](notes-releases/auth-rn-269.md) | 02/27/2024 - 02/29/2024 |
| [Adobe Pass Authentication JavaScript SDK 4.7.0发行说明](notes-releases/authn-rn-javascript-470.md) | 02/27/2024 - 02/29/2024 |
| [Adobe Pass Authentication iOS / tvOS SDK 3.9.2发行说明](notes-releases/authn-rn-ios-tvos-392.md) | 03/26/2024 |
| [Adobe Pass Authentication iOS / tvOS SDK 3.8.4发行说明](notes-releases/authn-rn-ios-tvos-384.md) | 01/26/2024 |

### 2023 {#product-releases-2023}

| 发行说明 | 日期 |
|---------------------------------------------------------------------------------------------------------|-------------------------|
| [Adobe Pass Authentication 2.68发行说明](notes-releases/auth-rn-268.md) | 12/05/2023 - 12/07/2023 |
| [Adobe Pass Authentication 2.67发行说明](notes-releases/auth-rn-267.md) | 09/12/2023 - 09/14/2023 |
| [Adobe Pass Authentication 2.66发行说明](notes-releases/auth-rn-266.md) | 07/11/2023 - 07/13/2023 |
| [Adobe Pass Authentication 2.65.1发行说明](notes-releases/auth-rn-2651.md) | 06/20/2023 - 06/22/2023 |
| [Adobe Pass Authentication 2.65发行说明](notes-releases/auth-rn-265.md) | 2023年4月25日至2023年4月27日 |
| [Adobe Pass Authentication 2.64.1发行说明](notes-releases/auth-rn-2641.md) | 01/31/2023 - 02/02/2023 |
| [Adobe Pass Authentication iOS / tvOS SDK 3.8.3发行说明](notes-releases/authn-rn-ios-tvos-383.md) | 11/16/2023 |
| [Adobe Pass Authentication iOS / tvOS SDK 3.8.2发行说明](notes-releases/authn-rn-ios-tvos-382.md) | 02/10/2023 |
| [Adobe Pass Authentication iOS / tvOS SDK 3.8.1发行说明](notes-releases/authn-rn-ios-tvos-381.md) | 2023年5月26日 |
| [Adobe Pass Authentication Android SDK 3.7.3发行说明](notes-releases/authn-rn-android-373.md) | 09/19/2023 |

### 2022 {#product-releases-2022}

| 发行说明 | 日期 |
|-----------------------------------------------------------------------------------------------------------|-------------------------|
| [Adobe Pass Authentication 2.64发行说明](notes-releases/auth-rn-264.md) | 11/08/2022 - 11/10/2022 |
| [Adobe Pass Authentication 2.63发行说明](notes-releases/auth-rn-263.md) | 09/20/2022 - 09/22/2022 |
| [Adobe Pass Authentication 2.62.1发行说明](notes-releases/auth-rn-2621.md) | 08/02/2022 - 08/04/2022 |
| [Adobe Pass Authentication JavaScript SDK 4.6.0发行说明](notes-releases/authn-rn-javascript-460.md) | 09/20/2022 - 09/22/2022 |

### 2021 {#product-releases-2021}

| 发行说明 | 日期 |
|-----------------------------------------------------------------------------------------------------------|------------|
| [Adobe Pass Authentication JavaScript SDK 4.4.0发行说明](notes-releases/authn-rn-javascript-440.md) | 06/22/2021 |
| [Adobe Pass Authentication iOS / tvOS SDK 3.7.0发行说明](notes-releases/authn-rn-ios-tvos-370.md) | 09/03/2021 |
