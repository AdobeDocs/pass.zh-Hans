---
title: Dynamic Client注册概述
description: Dynamic Client注册概述
exl-id: 9f98dfcd-4375-48c3-beff-259dfb1d3a26
source-git-commit: 9dcc649b4216cccc9be35cd6553308bfc345b5f4
workflow-type: tm+mt
source-wordcount: '813'
ht-degree: 0%

---

# Dynamic Client注册概述 {#dynamic-client-registration-overview}

>[!IMPORTANT]
>
> 确保随时了解汇总在[产品公告](/help/authentication/product-announcements.md)页中的最新Adobe Pass身份验证产品公告和停用时间表。

<a href="https://experienceleague.adobe.com/en/docs/pass/authentication/product-announcements">![直播网络研讨会系列](/help/authentication/assets/rest-api-v2/live-webinar-series-banner.png)</a>

动态客户端注册表示由[RFC 7591](https://datatracker.ietf.org/doc/html/rfc7591)定义的授权机制，它基于[RFC 6749](https://datatracker.ietf.org/doc/html/rfc6749)描述的OAuth 2.0授权框架。

Adobe Pass提供动态客户端注册服务，允许访问以下受保护的API：

* Adobe Pass身份验证管理API：
   * [重置临时传递API](../../features-premium/temporary-access/temp-pass-feature.md#reset-tempass-api-access)
   * [降级API](../../features-premium/degraded-access/degradation-feature.md#degradation-api-access)
   * [代理MVPD API](../../../integration-guide-mvpds/proxy-mvpd-webserv.md)
   * [授权服务监控API](../../features-premium/esm/entitlement-service-monitoring-api.md)
* Adobe Pass身份验证REST API：
   * [REST API V2](../rest-api-v2/apis/rest-api-v2-apis-overview.md)
   * [（旧版）REST API V1](../../legacy/rest-api-v1/rest-api-reference.md)
* Adobe Pass身份验证SDK：
   * [（旧版）JavaScript SDK](../../legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md)
   * [（旧版）iOS/tvOS SDK](../../legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)
   * [（旧版）Android SDK](../../legacy/sdks/android-sdk/android-sdk-api-reference.md)
   * [（旧版）FireOS SDK](../../legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md)

>[!IMPORTANT]
>
> 动态客户端注册授权机制取代了将停止使用的旧版Adobe Pass身份验证解决方案：
>
> * 已签名的请求者ID机制。
> * 域列表机制。
> * API密钥机制。

采用动态客户注册的主要优势包括：

* 增强的安全性。
* 跨平台的统一模型。
* 对应用程序生命周期的精细控制。

要了解有关如何管理和使用动态客户端注册的更多信息，请参阅以下部分。

## 动态客户端注册管理 {#dynamic-client-registration-management}

动态客户端注册管理流程允许在特定平台上运行且需要访问特定Adobe Pass身份验证API的客户端应用程序通过[Adobe Pass TVE仪表板](https://experience.adobe.com/#/pass/authentication)进行注册。

Adobe Pass TVE Dashboard是一款用于Adobe Pass身份验证客户（程序员）管理其配置和数据的工具。 此自助仪表板启用了[Adobe Pass TVE仪表板用户指南](../../../user-guide-tve-dashboard/tve-dashboard-overview.md)文档中描述的一系列功能。

如果您有权访问[Adobe Pass TVE仪表板](https://experience.adobe.com/#/pass/authentication)，请按照以下部分中的步骤创建注册的应用程序并下载软件语句。

### 管理注册的应用程序 {#manage-registered-applications}

>[!IMPORTANT]
>
> 如果您无权访问Adobe Pass TVE仪表板，请通过我们的[Zendesk](https://adobeprimetime.zendesk.com)创建票证，并请求技术客户经理(TAM)创建注册的应用程序并与您共享软件报表。

创建注册的应用程序有两种可用方法：

* **程序员级别**

  程序员级别的注册过程允许您创建链接到所有可用渠道或所选渠道子集的已注册应用程序。 有关更多详细信息，请参阅[面向程序员的TVE仪表板用户指南](../../../user-guide-tve-dashboard/tve-dashboard-programmers.md)文档。


* **渠道级别**

  渠道级别的注册流程允许您创建仅链接到当前选定渠道的已注册应用程序。 有关更多详细信息，请参阅[TVE仪表板用户指南](../../../user-guide-tve-dashboard/tve-dashboard-channels.md)文档。

>[!IMPORTANT]
>
> 建议创建具有更具体且更有限权限的已注册应用程序，以增强安全性并防止未经授权的访问。 因此，在创建已注册的应用程序时，请考虑对分配的`channels`、`platforms`和`scopes`使用更窄的选项。
>
> 建议为客户端应用程序的每次重大更新创建新的注册应用程序，以管理其生命周期和使用情况。 如有必要，请通过我们的[Zendesk](https://adobeprimetime.zendesk.com)创建一个票证，然后要求您的技术客户经理(TAM)撤销已注册的应用程序，以阻止特定客户端应用程序版本的功能。

### 管理软件语句 {#manage-software-statements}

>[!IMPORTANT]
>
> 如果您无权访问Adobe Pass TVE仪表板，请通过我们的[Zendesk](https://adobeprimetime.zendesk.com)创建票证，并请求技术客户经理(TAM)创建注册的应用程序并与您共享软件报表。

在下载软件语句之前，请确保已按照[管理已注册的应用程序](#manage-registered-applications)部分中所述创建了符合您的客户端应用程序要求的已注册应用程序。

根据注册应用程序的创建级别，可通过两种可用方式下载软件语句：

* **程序员级别**

  有关更多详细信息，请参阅[面向程序员的TVE仪表板用户指南](../../../user-guide-tve-dashboard/tve-dashboard-programmers.md)文档。

* **渠道级别**

  有关更多详细信息，请参阅[TVE仪表板用户指南](../../../user-guide-tve-dashboard/tve-dashboard-channels.md)文档。

软件语句是一个JSON Web令牌(`JWT`)，其中包含有关客户端应用程序软件作为捆绑包的信息。 向[检索客户端凭据](apis/dynamic-client-registration-apis-retrieve-client-credentials.md) API显示时，使用JSON Web签名(`JWS`)对软件语句进行数字签名。

有关软件语句是什么及其工作方式的更多详细说明，请参阅[RFC 7591](https://tools.ietf.org/html/rfc7591)文档。

## 动态客户端注册流程 {#dynamic-client-registration-flow}

总之，动态客户端注册授权机制涉及几个步骤：

**管理**

* 客户端代表必须按照[管理已注册的应用程序](#manage-registered-applications)部分中的说明创建已注册的应用程序。
* 客户端代表必须下载并嵌入软件语句，如[管理软件语句](#manage-software-statements)部分中所述。

**流量**

* 客户端应用程序必须按照[检索客户端凭据](apis/dynamic-client-registration-apis-retrieve-client-credentials.md) API文档中所述获取客户端凭据。
* 客户端应用程序必须按照[检索访问令牌](apis/dynamic-client-registration-apis-retrieve-access-token.md) API文档中所述获取访问令牌。

请参阅[动态客户端注册流程](flows/dynamic-client-registration-flow.md)文档，以更好地了解如何访问受Adobe Pass保护的API。 此外，您还可以观看此[网络研讨会](https://my.adobeconnect.com/pzkp8ujrigg1/)录像，其中提供了更多背景信息并包括演示。
