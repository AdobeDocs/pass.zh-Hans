---
title: 轮廓
description: 轮廓
source-git-commit: edfde4b463dd8b93dd770bc47353ee8ceb6f39d2
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 0%

---

# 轮廓 {#profiles}

>[!IMPORTANT]
>
> 此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

用户通过Adobe Pass身份验证[REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)创建配置文件，用户随后成功与其付费电视提供商(MVPD)进行身份验证。

配置文件类型因所使用的身份验证方法而异：

* **常规**

  通过基本身份验证创建。

* **Apple SSO**

  使用Apple的视频订阅者帐户框架，通过单点登录(SSO)创建。

* **平台SSO**

  使用平台标识通过单点登录(SSO)创建。

* **服务令牌SSO**

  使用服务令牌通过单点登录(SSO)创建。

配置文件存储关键数据，使客户端应用程序能够：

* 确定用户的身份验证状态。
* 标识使用的身份验证方法。
* 标识标识提供者。
* 访问[用户元数据](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)。

用户档案安全地存储在Adobe Pass身份验证的后端中，并链接到请求的应用程序、设备和服务提供商标识符。 它们在有限的时间范围内保持有效，如[身份验证生存时间(TTL)](#authentication-ttl-management)所定义。

## 身份验证生存时间(TTL)管理 {#authentication-ttl-management}

身份验证生存时间(TTL)定义用户在需要重新身份验证之前保持身份验证状态的时间。 此时间范围是有限的，必须与MVPD代表商定。 TTL值可能因以下原因而异：

* 平台类别（例如，台式机、移动设备、电视连接设备）
* 特定平台(例如iOS、Android、tvOS、Roku、FireTV)

您的组织管理员或代表您行事的Adobe Pass身份验证代表可通过Adobe Pass [TVE仪表板](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)查看和更改身份验证(authN) TTL。

有关更多详细信息，请参阅[TVE仪表板集成用户指南](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows)文档。

## REST API V2 {#rest-api-v2}

可以使用以下API检索用户档案：

* [检索用户档案](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [检索特定mvpd的配置文件](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [检索特定代码的配置文件](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

请参阅上述API的&#x200B;**响应**&#x200B;和&#x200B;**示例**&#x200B;部分以了解配置文件的结构。

有关如何以及何时集成上述API的更多详细信息，请参阅以下文档：

* [基本配置文件在主要应用程序中执行](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [基本配置文件在辅助应用程序中执行](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

>[!MORELIKETHIS]
>
> [身份验证阶段常见问题解答](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)
