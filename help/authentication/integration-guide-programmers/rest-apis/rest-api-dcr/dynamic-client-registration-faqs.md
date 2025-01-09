---
title: 动态客户端注册(DCR)常见问题解答
description: 动态客户端注册(DCR)常见问题解答
source-git-commit: 5622cad15383560e19e8111f12a1460e9b118efe
workflow-type: tm+mt
source-wordcount: '836'
ht-degree: 0%

---

# 动态客户端注册(DCR)常见问题解答 {#rest-api-dcr-faqs}

>[!IMPORTANT]
>
> 此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

本文档提供了有关Adobe Pass身份验证动态客户端注册(DCR)采用情况的常见问题解答的高级概述。

有关整个动态客户端注册(DCR)的更多信息，请参阅[动态客户端注册概述](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)文档。

>[!MORELIKETHIS]
>
> * [REST API v2常见问题解答](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md)

## 一般常见问题解答 {#general-faqs}

如果您正在处理的应用程序需要集成Dynamic Client Registration (DCR)，则无论该应用程序是新应用程序还是从以前的某个机制迁移的现有应用程序，都可以从本节开始。

### 注册阶段常见问题解答 {#registration-phase-faqs-general}

+++注册阶段常见问题解答

#### 1.登记阶段的目的是什么？ {#registration-phase-faq1}

注册阶段的目的是通过[动态客户端注册(DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#dcr)进程针对Adobe Pass身份验证注册客户端应用程序。

动态客户端注册(DCR)过程要求客户端应用程序获取一对客户端凭据，并检索访问令牌作为注册阶段的最终目标。

有关详细信息，请参阅[动态客户端注册概述](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)文档。

#### 2.登记阶段是否为强制性的？ {#registration-phase-faq2}

注册阶段是强制性的，但如果客户端应用程序具有缓存的客户端凭据对和仍然有效的访问令牌，则该客户端应用程序可以跳过此阶段。

#### 3.什么是软件声明，它的有效期是多久？ {#registration-phase-faq3}

软件语句是[术语表](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#software-statement)文档中定义的术语。

软件语句包含一个JSON Web令牌(JWT)，可以由您的组织管理员或代表您行事的Adobe Pass身份验证代表从Adobe Pass [TVE仪表板](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)生成和下载。

软件语句的有效期限不受限制，但您可以随时要求Adobe Pass身份验证代表撤销该语句。

客户端应用程序必须存储软件语句，并在需要检索客户端凭据时使用它。

有关更多详细信息，请参阅[动态客户端注册概述](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)文档。

#### 4.如何生成和下载软件声明？ {#registration-phase-faq4}

此操作可以由您的组织管理员或代表您行事的Adobe Pass身份验证代表通过Adobe Pass [TVE仪表板](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)完成。

有关更多详细信息，请参阅[TVE仪表板渠道用户指南](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#registered-applications)或[TVE仪表板程序员用户指南](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#registered-applications)文档。

#### 5.如果软件声明被撤销，会发生什么情况？ {#registration-phase-faq5}

撤销软件语句时，需要考虑以下重要后果：

* 使用已撤销的软件声明的客户端应用程序将无法再通过[权利](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#entitlement)流程，这意味着用户将被阻止播放内容。

#### 6.什么是客户端凭据以及这些凭据的有效期？ {#registration-phase-faq6}

客户端凭据是[词汇表](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#client-credentials)文档中定义的术语。

客户端凭据由客户端标识符和客户端密钥对组成，可从客户端注册端点检索。

客户端凭据的有效时间范围不受限制。

客户端应用程序必须无限期地存储客户端凭据，并在需要检索访问令牌时使用它们。

有关详细信息，请参阅[检索客户端凭据](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)文档。

#### 7.如何管理客户端凭证？ {#registration-phase-faq7}

对于与Adobe Pass身份验证的客户端到服务器和服务器到服务器集成，我们建议客户端应用程序为每个用户应用程序实例管理一对唯一的客户端凭据。

客户端应用程序必须无限期地存储客户端凭据，并在需要检索访问令牌时使用它们。

#### 8.如果缓存的客户端凭据丢失，会发生什么情况？ {#registration-phase-faq8}

当缓存的客户端凭据丢失时，需要考虑三个重要后果：

* 客户端应用程序必须获取一对新的客户端凭据。
* 客户端应用程序必须使用新客户端凭据对获取新的访问令牌。
* 客户端应用程序将需要请求用户重新进行身份验证，因为客户端应用程序将失去对之前获取的已验证配置文件的访问权限。

#### 9.访问令牌是什么以及它有效多长时间？ {#registration-phase-faq9}

访问令牌是[术语表](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#access-token)文档中定义的术语。

访问令牌包含可从客户端令牌端点检索的[持有者令牌](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md)。

访问令牌在问题时刻指定的有限且较短的时间范围内有效。

客户端应用程序必须存储访问令牌并使用它，直到它在定位REST API V2时过期。

客户端应用程序必须在当前访问令牌过期之前获取新的访问令牌，以防止未授权的请求。

有关详细信息，请参阅[检索访问令牌](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)文档。

#### 10.客户端应用程序如何刷新访问令牌？ {#registration-phase-faq10}

客户端应用程序刷新访问令牌的方式必须与检索新访问令牌的方式相同，但会使用缓存的客户端凭据。

客户端应用程序不得重新注册以刷新访问令牌，而是必须使用存储的客户端凭据，否则用户需要重新进行身份验证。

有关详细信息，请参阅[检索访问令牌](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)文档。
