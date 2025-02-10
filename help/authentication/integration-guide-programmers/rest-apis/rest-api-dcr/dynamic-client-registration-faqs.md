---
title: 动态客户端注册(DCR)常见问题解答
description: 动态客户端注册(DCR)常见问题解答
exl-id: 12268163-632e-4884-b35d-a29cc8ef45bf
source-git-commit: 747c3d9b6de537be5e7e0a0244b2b301603d9b18
workflow-type: tm+mt
source-wordcount: '1135'
ht-degree: 0%

---

# 动态客户端注册(DCR)常见问题解答 {#rest-api-dcr-faqs}

>[!IMPORTANT]
>
> 此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

本文档提供了有关Adobe Pass身份验证动态客户端注册(DCR)采用情况的常见问题解答的高级概述。

有关整个动态客户端注册(DCR)的更多信息，请参阅[动态客户端注册概述](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)文档。

## 一般常见问题解答 {#general-faqs}

如果您正在处理的应用程序需要集成Dynamic Client Registration (DCR)，则无论该应用程序是新应用程序还是从以前的某个机制迁移的现有应用程序，都可以从本节开始。

>[!MORELIKETHIS]
>
> * [REST API v2常见问题解答](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#general-faqs)

### REST API V2访问常见问题解答 {#rest-api-v2-access-faqs}

+++REST API V2访问常见问题解答

#### 1.登记阶段的目的是什么？ {#rest-api-v2-access-faq1}

注册阶段的目的是通过[动态客户端注册(DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#dcr)进程针对Adobe Pass身份验证注册客户端应用程序。

动态客户端注册(DCR)过程要求客户端应用程序获取一对客户端凭据，并检索访问令牌作为注册阶段的最终目标。

有关详细信息，请参阅[动态客户端注册概述](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)文档。

#### 2.登记阶段是否为强制性的？ {#rest-api-v2-access-faq2}

注册阶段是强制性的，但如果客户端应用程序具有缓存的客户端凭据对和仍然有效的访问令牌，则该客户端应用程序可以跳过此阶段。

#### 3.什么是软件声明，它的有效期是多久？ {#rest-api-v2-access-faq3}

软件语句是[术语表](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#software-statement)文档中定义的术语。

软件语句包含一个JSON Web令牌(JWT)，可以由您的组织管理员或代表您行事的Adobe Pass身份验证代表从Adobe Pass [TVE仪表板](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)生成和下载。

软件语句的有效期限不受限制，但您可以随时要求Adobe Pass身份验证代表撤销该语句。

客户端应用程序必须存储软件语句，并在需要检索客户端凭据时使用它。

有关更多详细信息，请参阅[动态客户端注册概述](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)文档。

#### 4.如何生成和下载软件声明？ {#rest-api-v2-access-faq4}

此操作可以由您的组织管理员或代表您行事的Adobe Pass身份验证代表通过Adobe Pass [TVE仪表板](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)完成。

有关更多详细信息，请参阅[TVE仪表板渠道用户指南](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#registered-applications)或[TVE仪表板程序员用户指南](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#registered-applications)文档。

#### 5.如果软件声明被撤销，会发生什么情况？ {#rest-api-v2-access-faq5}

撤销软件语句时，需要考虑以下重要后果：

* 使用已撤销的软件声明的客户端应用程序将无法再通过[权利](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#entitlement)流程，这意味着用户将被阻止播放内容。

#### 6.什么是客户端凭据以及这些凭据的有效期？ {#rest-api-v2-access-faq6}

客户端凭据是[词汇表](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#client-credentials)文档中定义的术语。

客户端凭据由客户端标识符和客户端密钥对组成，可从客户端注册端点检索。

客户端凭据的有效时间范围不受限制。

客户端应用程序必须存储客户端凭据，并在需要检索访问令牌时无限期使用这些凭据。

有关详细信息，请参阅[检索客户端凭据](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)文档。

#### 7.如何管理客户端凭证？ {#rest-api-v2-access-faq7}

对于与Adobe Pass身份验证的客户端到服务器和服务器到服务器集成，我们建议客户端应用程序为每个用户应用程序实例管理一对唯一的客户端凭据。

#### 8.客户端应用程序是否应在永久存储中缓存客户端凭据？ {#rest-api-v2-access-faq8}

客户端应用程序必须存储客户端凭据，并在需要检索访问令牌时无限期使用这些凭据。

#### 9.如果缓存的客户端凭据丢失，会发生什么情况？ {#rest-api-v2-access-faq9}

当缓存的客户端凭据丢失时，需要考虑三个重要后果：

* 客户端应用程序必须获取一对新的客户端凭据。
* 客户端应用程序必须使用新客户端凭据对获取新的访问令牌。
* 客户端应用程序将需要请求用户重新进行身份验证，因为客户端应用程序将失去对之前获取的已验证配置文件的访问权限。

#### 10.访问令牌是什么以及它有效多长时间？ {#rest-api-v2-access-faq10}

访问令牌是[术语表](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#access-token)文档中定义的术语。

访问令牌包含可从客户端令牌端点检索的[持有者令牌](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md)。

访问令牌在问题时刻指定的有限且较短的时间范围内有效。

客户端应用程序必须存储访问令牌并使用它，直到它在定位REST API V2时过期。

客户端应用程序必须在当前访问令牌过期之前获取新的访问令牌，以防止未授权的请求。

有关详细信息，请参阅[检索访问令牌](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)文档。

#### 11.客户端应用程序是否应将访问令牌缓存到永久存储中？ {#rest-api-v2-access-faq11}

客户端应用程序必须存储和使用访问令牌，直到它过期，然后丢弃它并获取一个新的访问令牌。

#### 12.客户端应用程序如何刷新访问令牌？ {#rest-api-v2-access-faq12}

客户端应用程序刷新访问令牌的方式必须与检索新访问令牌的方式相同，但会使用缓存的客户端凭据。

客户端应用程序不得重新注册以刷新访问令牌，而是必须使用存储的客户端凭据，否则用户需要重新进行身份验证。

有关详细信息，请参阅[检索访问令牌](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)文档。

+++

## 迁移常见问题解答 {#migration-faqs}

如果您使用的应用程序需要迁移现有应用程序以使用动态客户端注册(DCR)，请继续使用此部分。

>[!MORELIKETHIS]
>
> * [REST API v2常见问题解答](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#migration-faqs)

### REST API V2迁移常见问题解答 {#rest-api-v2-migration-faqs}

+++REST API V2迁移常见问题解答

#### 1.客户端应用程序能否重新使用现有的已注册应用程序（软件语句）？ {#rest-api-v2-migration-faq1}

客户端应用程序不能重复使用现有的已注册应用程序（软件语句），因此必须生成并下载专用于使用REST API V2的新已注册应用程序（软件语句）。

此操作可以由您的组织管理员或代表您行事的Adobe Pass身份验证代表通过Adobe Pass [TVE仪表板](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)完成。

有关更多详细信息，请参阅[TVE仪表板渠道用户指南](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#registered-applications)或[TVE仪表板程序员用户指南](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#registered-applications)文档。

目前，您需要请求Adobe Pass身份验证代表为您新注册的应用程序（软件语句）启用REST API V2的使用，直到更新Adobe Pass [TVE仪表板](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)以允许此操作的自我管理。

为了区分使用REST API V2的客户端应用程序中使用的已注册应用程序（软件语句），我们要求您在已注册应用程序名称中添加特定的后缀，例如“RESTV2”。

#### 2.客户端应用程序能否重复使用现有的自定义方案？ {#rest-api-v2-migration-faq2}

客户端应用程序可以重复使用通过Adobe Pass [TVE仪表板](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)生成的现有自定义方案。

有关更多详细信息，请参阅[TVE仪表板渠道用户指南](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#custom-schemes)或[TVE仪表板程序员用户指南](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#custom-schemes)文档。

+++
