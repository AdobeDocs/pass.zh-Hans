---
title: REST API V2常见问题解答
description: REST API V2常见问题解答
exl-id: 2dd74b47-126e-487b-b467-c16fa8cc14c1
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '7304'
ht-degree: 0%

---

# REST API V2常见问题解答 {#rest-api-v2-faqs}

>[!IMPORTANT]
>
> 此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

本文档提供了有关Adobe Pass身份验证REST API V2采用的常见问题解答的高级概述性答案。

有关整个REST API V2的更多信息，请参阅[REST API V2概述](rest-api-v2-overview.md)文档。

## 一般常见问题解答 {#general-faqs}

如果您正在使用的应用程序需要集成REST API V2，请从该部分开始，无论该应用程序是新应用程序还是从[REST API V1](#migration-rest-api-v1-to-rest-api-v2)或[SDK](#migration-sdk-to-rest-api-v2)迁移的现有应用程序。

有关迁移详细信息和步骤的更多信息，另请参阅下一部分。

+++注册阶段常见问题解答

### 1.登记阶段的目的是什么？ {#registration-phase-faq1}

注册阶段的目的是通过[动态客户端注册(DCR)](rest-api-v2-glossary.md#dcr)进程针对Adobe Pass身份验证注册客户端应用程序。

动态客户端注册(DCR)过程要求客户端应用程序获取一对客户端凭据，并检索访问令牌作为注册阶段的最终目标。

有关详细信息，请参阅[动态客户端注册概述](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)文档。

### 2.登记阶段是否为强制性的？ {#registration-phase-faq2}

注册阶段是强制性的，但如果客户端应用程序具有缓存的客户端凭据对和仍然有效的访问令牌，则该客户端应用程序可以跳过此阶段。

### 3.什么是软件声明，它的有效期是多久？ {#registration-phase-faq3}

软件语句是[术语表](rest-api-v2-glossary.md#software-statement)文档中定义的术语。

软件语句包含一个JSON Web令牌(JWT)，可以由您的组织管理员或代表您行事的Adobe Pass身份验证代表从Adobe Pass [TVE仪表板](rest-api-v2-glossary.md#tve-dashboard)生成和下载。

软件语句的有效期限不受限制，但您可以随时要求Adobe Pass身份验证代表撤销该语句。

客户端应用程序必须存储软件语句，并在需要检索客户端凭据时使用它。

有关更多详细信息，请参阅[动态客户端注册概述](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)文档。

### 4.如何生成和下载软件声明？ {#registration-phase-faq4}

此操作可以由您的组织管理员或代表您行事的Adobe Pass身份验证代表通过Adobe Pass [TVE仪表板](rest-api-v2-glossary.md#tve-dashboard)完成。

有关更多详细信息，请参阅[TVE仪表板渠道用户指南](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#registered-applications)或[TVE仪表板程序员用户指南](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#registered-applications)文档。

### 5.如果软件声明被撤销，会发生什么情况？ {#registration-phase-faq5}

撤销软件语句时，需要考虑以下重要后果：

* 使用已撤销的软件声明的客户端应用程序将无法再通过[权利](rest-api-v2-glossary.md#entitlement)流程，这意味着用户将被阻止播放内容。

### 6.什么是客户端凭据以及这些凭据的有效期？ {#registration-phase-faq6}

客户端凭据是[词汇表](rest-api-v2-glossary.md#client-credentials)文档中定义的术语。

客户端凭据由客户端标识符和客户端密钥对组成，可从客户端注册端点检索。

客户端凭据的有效时间范围不受限制。

客户端应用程序必须无限期地存储客户端凭据，并在需要检索访问令牌时使用它们。

有关详细信息，请参阅[检索客户端凭据](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)文档。

### 7.如何管理客户端凭证？ {#registration-phase-faq7}

对于与Adobe Pass身份验证的客户端到服务器和服务器到服务器集成，我们建议客户端应用程序为每个用户应用程序实例管理一对唯一的客户端凭据。

客户端应用程序必须无限期地存储客户端凭据，并在需要检索访问令牌时使用它们。

### 8.如果缓存的客户端凭据丢失，会发生什么情况？ {#registration-phase-faq8}

当缓存的客户端凭据丢失时，需要考虑三个重要后果：

* 客户端应用程序必须获取一对新的客户端凭据。
* 客户端应用程序必须使用新客户端凭据对获取新的访问令牌。
* 客户端应用程序将需要请求用户重新进行身份验证，因为客户端应用程序将失去对之前获取的已验证配置文件的访问权限。

### 9.访问令牌是什么以及它有效多长时间？ {#registration-phase-faq9}

访问令牌是[术语表](rest-api-v2-glossary.md#access-token)文档中定义的术语。

访问令牌包含可从客户端令牌端点检索的[持有者令牌](appendix/headers/rest-api-v2-appendix-headers-authorization.md)。

访问令牌在问题时刻指定的有限且较短的时间范围内有效。

客户端应用程序必须存储访问令牌并使用它，直到它在定位REST API V2时过期。

客户端应用程序必须在当前访问令牌过期之前获取新的访问令牌，以防止未授权的请求。

有关详细信息，请参阅[检索访问令牌](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)文档。

### 10.客户端应用程序如何刷新访问令牌？ {#registration-phase-faq10}

客户端应用程序刷新访问令牌的方式必须与检索新访问令牌的方式相同，但会使用缓存的客户端凭据。

客户端应用程序不得重新注册以刷新访问令牌，而是必须使用存储的客户端凭据，否则用户需要重新进行身份验证。

有关详细信息，请参阅[检索访问令牌](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)文档。

+++

+++配置阶段常见问题解答

### 1.配置阶段的用途是什么？ {#configuration-phase-faq1}

配置阶段的目的是向客户端应用程序提供与其主动集成的MVPD列表，以及Adobe Pass身份验证为每个MVPD保存的配置详细信息。

当客户端应用程序需要要求用户选择其电视提供商时，配置阶段是身份验证阶段的先决条件步骤。

### 2.配置阶段是否为强制阶段？ {#configuration-phase-faq2}

配置阶段不是强制性的，客户端应用程序可以在以下情况下跳过此阶段：

* 用户已经过身份验证。
* 通过基本或促销的TempPass功能向用户提供临时访问权限。
* 用户验证已过期，但客户端应用程序已缓存以前选择的MVPD作为用户体验驱动的选择，并且只是提示用户确认他们仍然是该MVPD的订户。

### 3.什么是配置以及它的有效期？ {#configuration-phase-faq3}

该配置是[词汇表](rest-api-v2-glossary.md#configuration)文档中定义的术语。

该配置包含可从配置端点检索的MVPD列表。

当要求用户选择其MVPD时，客户端应用程序可使用配置显示名为“选取器”的UI组件。

客户端应用程序应在用户完成身份验证阶段之前刷新配置。

有关详细信息，请参阅[检索配置](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)文档。

### 4.客户端应用程序能否管理自己的MVPD列表？ {#configuration-phase-faq4}

客户端应用程序可以管理自己的MVPD列表，但建议使用Adobe Pass身份验证提供的配置以确保列表是最新且准确的。

如果用户选择的MVPD没有与指定的[服务提供程序](rest-api-v2-glossary.md#service-provider)的有效集成，则客户端应用程序将从Adobe Pass身份验证REST API V2收到[错误](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)。

### 5.客户端应用程序能否过滤MVPD列表？ {#configuration-phase-faq5}

客户端应用程序可以通过实现基于自身业务逻辑和诸如先前选择的用户位置或用户历史记录等要求的定制机制来过滤配置响应中提供的MVPD列表。

### 6.如果与MVPD的集成被禁用并标记为不活动，会发生什么情况？ {#configuration-phase-faq6}

如果禁用了与MVPD的集成并将其标记为非活动，则将从其他配置响应中提供的MVPD列表中删除MVPD，需要考虑以下两个重要后果：

* 该MVPD的未经身份验证的用户将无法再使用该MVPD完成身份验证阶段。
* 该MVPD的经过身份验证的用户将无法再使用该MVPD完成预授权、授权或注销阶段。

如果用户选择的MVPD不再与指定的[服务提供程序](rest-api-v2-glossary.md#service-provider)进行活动集成，则客户端应用程序将从Adobe Pass身份验证REST API V2收到[错误](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)。

### 7.如果重新启用与MVPD的集成并标记为活动，会发生什么情况？ {#configuration-phase-faq7}

当重新启用与MVPD的集成并标记为活动时，MVPD将重新包含在进一步配置响应中提供的MVPD列表中，需要考虑以下两个重要后果：

* 该MVPD的未经身份验证的用户将能够再次使用该MVPD完成身份验证阶段。
* 该MVPD的经过身份验证的用户将能够再次使用该MVPD完成预授权、授权或注销阶段。

### 8.如何启用或禁用与MVPD的集成？ {#configuration-phase-faq8}

此操作可以由您的组织管理员或代表您行事的Adobe Pass身份验证代表通过Adobe Pass [TVE仪表板](rest-api-v2-glossary.md#tve-dashboard)完成。

有关更多详细信息，请参阅[TVE仪表板集成用户指南](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#disable-integration)文档。

+++

+++身份验证阶段常见问题解答

### 1.身份验证阶段的用途是什么？ {#authentication-phase-faq1}

身份验证阶段的目的是向客户端应用程序提供验证用户身份和获取用户元数据信息的能力。

当客户端应用程序需要播放内容时，身份验证阶段是预授权阶段或授权阶段的先决步骤。

### 2.客户端应用程序如何知道用户是否已验证？ {#authentication-phase-faq2}

客户端应用程序可以查询以下能够验证用户是否已过身份验证的端点之一并返回配置文件信息：

* [配置文件端点API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [特定MVPD API的配置文件端点](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [特定（身份验证）代码API的配置文件端点](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

有关更多详细信息，请参阅以下文档：

* [基本配置文件在主要应用程序中执行](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [基本配置文件在辅助应用程序中执行](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

### 3.客户端应用程序如何获取用户的元数据信息？ {#authentication-phase-faq3}

客户端应用程序可以查询以下端点之一，这些端点能够返回[用户元数据](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata-feature.md)信息作为配置文件信息的一部分：

* [配置文件端点API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [特定MVPD API的配置文件端点](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [特定（身份验证）代码API的配置文件端点](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

有关更多详细信息，请参阅以下文档：

* [基本配置文件在主要应用程序中执行](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [基本配置文件在辅助应用程序中执行](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

### 4.什么是身份验证会话？该会话的有效时间是多久？ {#authentication-phase-faq4}

身份验证会话是[术语表](rest-api-v2-glossary.md#session)文档中定义的术语。

身份验证会话存储有关启动身份验证过程的信息，该信息可以从会话端点检索。

身份验证会话在发布时指定的有限和短时间范围内有效，指示用户在需要重新启动流之前必须完成身份验证过程的时间量。

客户端应用程序可以使用身份验证会话响应来了解如何继续进行身份验证过程。 请注意，在某些情况下，用户不需要进行身份验证，例如提供临时访问、降级访问或用户已经过身份验证。

有关更多信息，请参阅以下文档：

* [创建身份验证会话API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [恢复身份验证会话API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [在主应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [在辅助应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

### 5.什么是身份验证代码以及它的有效时间长短？ {#authentication-phase-faq5}

身份验证代码是[词汇表](rest-api-v2-glossary.md#code)文档中定义的术语。

验证代码存储当用户启动验证过程时生成的唯一值，并唯一标识用户的验证会话，直到该过程完成或验证会话过期为止。

该认证代码在启动认证会话时指定的有限和短的时间范围内有效，这指示在要求重新启动流之前用户必须完成认证过程的时间量。

客户端应用程序可以使用身份验证代码来允许用户在同一设备或第二台设备上完成或恢复身份验证过程，因为身份验证会话未过期。

有关更多信息，请参阅以下文档：

* [创建身份验证会话API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [恢复身份验证会话API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [在主应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [在辅助应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

### 6.客户端应用程序如何知道用户是否键入了有效的身份验证代码以及身份验证会话是否尚未过期？ {#authentication-phase-faq6}

客户端应用程序可以通过向负责检索与验证代码关联的验证会话信息的“会话”端点发送请求来验证用户在辅助（屏幕）应用程序中键入的验证代码。

如果提供的身份验证代码键入错误或身份验证会话过期，则客户端应用程序将收到[错误](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)。

有关详细信息，请参阅[检索身份验证会话](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)文档。

### 7.什么是配置文件，它的有效时间长短？ {#authentication-phase-faq7}

配置文件是[词汇表](rest-api-v2-glossary.md#profile)文档中定义的术语。

配置文件存储有关用户身份验证有效性的信息、元数据信息以及许多可从配置文件端点检索的信息。

客户端应用程序可以使用配置文件来了解用户的验证状态、访问用户元数据信息或查找用于验证的方法。

有关更多详细信息，请参阅以下文档：

* [配置文件端点API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [特定MVPD API的配置文件端点](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [特定（身份验证）代码API的配置文件端点](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
* [基本配置文件在主要应用程序中执行](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [基本配置文件在辅助应用程序中执行](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

该配置文件在查询时指定的有限时间范围内有效，这表示在需要再次通过身份验证阶段之前，用户已进行了有效身份验证的时间。

您组织的一名管理员或代表您行事的Adobe Pass身份验证代表可以通过Adobe Pass [TVE仪表板](rest-api-v2-glossary.md#tve-dashboard)查看和更改这个称为身份验证(authN) [TTL](rest-api-v2-glossary.md#ttl)的有限时间范围。

有关更多详细信息，请参阅[TVE仪表板集成用户指南](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows)文档。

+++

+++预授权阶段常见问题解答

### 1.预授权阶段的目的是什么？ {#preauthorization-phase-faq1}

预授权阶段的目的是让客户端应用程序能够呈现用户有权访问的目录中的资源子集。

当用户首次打开客户端应用程序或导航到新部分时，预授权阶段可以增强用户体验。

### 2.预授权阶段是否为强制性的？ {#preauthorization-phase-faq2}

预授权阶段不是强制性的，如果客户端应用程序希望呈现资源目录而不是首先根据用户权利筛选资源，则可以跳过此阶段。

### 3.什么是预授权决定？ {#preauthorization-phase-faq3}

预授权是在[词汇表](rest-api-v2-glossary.md#preauthorization)文档中定义的术语，而决策术语也可在[词汇表](rest-api-v2-glossary.md#decision)中找到。

预授权决策存储有关MVPD预授权过程查询结果的信息，该查询结果可以从Decisions Preauthorize端点检索。

客户端应用程序可以使用预授权决策来呈现电视提供商（信息）决策将允许用户访问的资源的子集。

有关更多详细信息，请参阅以下文档：

* [检索预授权决策API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
* [在主应用程序中执行的基本预授权流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

### 4.为何预授权决策缺少媒体令牌？ {#preauthorization-phase-faq4}

预授权决策缺少媒体令牌，因为预授权阶段不能用于播放资源，因为这是授权阶段的目的。

### 5.什么是资源？支持哪些格式？ {#preauthorization-phase-faq5}

资源是[术语表](rest-api-v2-glossary.md#resource)文档中定义的术语。

资源是与MVPD商定的唯一标识符，并与客户端应用程序可以流式传输的内容相关联。

资源唯一标识符可以有两种格式：

* 简单的字符串格式，如渠道（品牌）的唯一标识符。
* 一种媒体RSS (MRSS)格式，包含标题、评级和家长控制元数据等附加信息。

有关更多详细信息，请参阅[识别受保护的资源](/help/authentication/integration-guide-programmers/features-standard/entitlements/identify-protected-resources.md)文档。

### 6.客户端应用程序一次可获取多少资源来作出预授权决定？ {#preauthorization-phase-faq6}

由于MVPD施加的条件，客户端应用程序可以在单个API请求中为有限数量的资源获取预授权决策，通常最多可达5个。

在通过Adobe Pass [TVE Dashboard](rest-api-v2-glossary.md#tve-dashboard)与MVPD达成一致后，您的组织管理员或代表您行事的Adobe Pass身份验证代表可以查看和更改资源的最大数量。

有关更多详细信息，请参阅[TVE仪表板集成用户指南](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#add-more-properties)文档。

+++

+++授权阶段常见问题解答

### 1.授权阶段的目的是什么？ {#authorization-phase-faq1}

授权阶段的目的是让客户端应用程序能够在使用MVPD验证用户请求的权限后，播放用户请求的资源。

### 2.授权阶段是否为强制性的？ {#authorization-phase-faq2}

授权阶段是强制性的，如果客户端应用程序要播放用户请求的资源，则无法跳过此阶段，因为它需要在释放流之前与MVPD验证用户是否有权使用资源。

### 3.授权决定是什么，其有效期多长？ {#authorization-phase-faq3}

授权是在[词汇表](rest-api-v2-glossary.md#authorization)文档中定义的术语，而决策术语也可在[词汇表](rest-api-v2-glossary.md#decision)中找到。

授权决策存储有关MVPD授权过程查询结果的信息，该查询结果可从Decisions Authorize端点检索。

客户端应用程序可以使用包含媒体令牌的授权决策来播放资源流，以防电视提供商（权威）决策允许用户访问它。

有关更多详细信息，请参阅以下文档：

* [检索授权决策API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [在主应用程序中执行的基本授权流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

授权决策在发布时指定的有限且较短的时间范围内有效，指示在再次查询MVPD之前Adobe Pass身份验证将缓存该决策的时间。

您组织的一名管理员或代表您行事的Adobe Pass身份验证代表可以通过Adobe Pass [TVE仪表板](rest-api-v2-glossary.md#tve-dashboard)查看和更改这个称为授权(authZ) [TTL](rest-api-v2-glossary.md#ttl)的有限时间范围。

有关更多详细信息，请参阅[TVE仪表板集成用户指南](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows)文档。

### 4.什么是媒体令牌，其有效期是多久？ {#authorization-phase-faq4}

媒体令牌是[词汇表](rest-api-v2-glossary.md#media-token)文档中定义的术语。

媒体令牌包含以明文发送的已签名字符串，可以从决策授权端点检索。

有关详细信息，请参阅[集成媒体令牌验证器](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-token-verifier-int.md)文档。

媒体令牌在问题时指定的有限且较短的时间范围内有效，指示在再次请求查询Decisions Authorize端点之前客户端应用程序必须使用该令牌的时间量。

客户端应用程序可以使用媒体令牌播放资源流，以防电视提供商（权威）决策允许用户访问它。

有关更多详细信息，请参阅以下文档：

* [检索授权决策API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [在主应用程序中执行的基本授权流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

### 5.什么是资源？支持哪些格式？ {#authorization-phase-faq5}

资源是[术语表](rest-api-v2-glossary.md#resource)文档中定义的术语。

资源是与MVPD商定的唯一标识符，并与客户端应用程序可以流式传输的内容相关联。

资源唯一标识符可以有两种格式：

* 简单的字符串格式，如渠道（品牌）的唯一标识符。
* 一种媒体RSS (MRSS)格式，包含标题、评级和家长控制元数据等附加信息。

有关更多详细信息，请参阅[识别受保护的资源](/help/authentication/integration-guide-programmers/features-standard/entitlements/identify-protected-resources.md)文档。

### 6.客户端应用程序一次可获取多少资源来作出授权决定？ {#authorization-phase-faq6}

由于MVPD施加的条件，客户端应用程序可以在单个API请求中获取对有限数量的资源的授权决策，通常最多可达1。

+++

+++注销阶段常见问题解答

### 1.注销阶段的目的是什么？ {#logout-phase-faq1}

注销阶段的目的是让客户端应用程序能够根据用户请求在Adobe Pass身份验证中终止用户的已验证配置文件。

+++

+++标头常见问题解答

### 1.如何计算Authorization标头的值？ {#headers-faq1}

[Authorization](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md)请求标头包含客户端应用程序访问受Adobe Pass保护的API所需的`Bearer`访问令牌。

在注册阶段，必须从Adobe Pass身份验证中获取授权标头值。

有关更多详细信息，请参阅以下文档：

* [Dynamic Client注册概述](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
* [检索客户端凭据API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [检索访问令牌API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* [动态客户端注册流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

如果客户端应用程序从REST API V1迁移到REST API V2，则客户端应用程序可以继续使用与之前相同的方法获取`Bearer`访问令牌。

### 2.如何计算AP-Device-Identifier标头的值？ {#headers-faq2}

[AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)请求标头包含由客户端应用程序创建的流设备标识符。

[AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)标头文档提供了一些有关如何计算不同平台值的示例，但客户端应用程序可以根据自己的业务逻辑和要求选择使用不同的方法。

如果客户端应用程序从REST API V1迁移到REST API V2，则客户端应用程序可以继续使用与之前相同的方法计算设备标识符。

### 3.如何计算X-Device-Info标头的值？ {#headers-faq3}

[X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md)请求标头包含与实际流设备相关的客户端信息（设备、连接和应用程序）。

[X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md)标头文档提供了一些有关如何计算不同平台值的示例，但客户端应用程序可以根据自己的业务逻辑和要求选择使用不同的方法。

如果客户端应用程序从REST API V1迁移到REST API V2，则客户端应用程序可以继续使用与之前相同的方法计算设备信息。

+++

## 迁移常见问题解答 {#migration-faqs}

如果您使用的应用程序需要将现有应用程序迁移到REST API V2，请继续阅读本节内容。

+++一般迁移常见问题解答

### 1.是否需要一次性向所有用户推出迁移到REST API V2的新客户端应用程序？ {#migration-faq1}

不适用。

客户端应用程序不需要同时向所有用户推出集成REST API V2的新版本。

到2025年底，Adobe Pass身份验证将继续支持集成REST API V1或SDK的较旧客户端应用程序版本。

### 2.是否需要一次性跨所有API和流推出迁移到REST API V2的新客户端应用程序？ {#migration-faq2}

是的。

需要客户端应用程序同时跨所有Adobe Pass身份验证API和流推出集成REST API V2的新版本。

在“第二屏幕身份验证”流程中，客户端应用程序需要同时为[主](rest-api-v2-glossary.md#primary-application)和[次](rest-api-v2-glossary.md#secondary-application)应用程序推出集成REST API V2的新版本。

Adobe Pass身份验证将不支持在API和流之间集成REST API V2和REST API V1/SDK的“混合”实施。

### 3.在更新到迁移到REST API V2的新客户端应用程序时，是否将保留用户身份验证？ {#migration-faq3}

不适用。

在集成REST API V1或SDK的较旧客户端应用程序版本中获取的用户身份验证将不会保留。

因此，用户需要在迁移到REST API V2的新客户端应用程序中重新进行身份验证。

### 4.客户端应用程序是否可以使用现有的已注册应用程序（软件语句）？ {#migration-faq4}

客户端应用程序不能重复使用现有的已注册应用程序（软件语句），因此必须生成并下载专用于使用REST API V2的新已注册应用程序（软件语句）。

此操作可以由您的组织管理员或代表您行事的Adobe Pass身份验证代表通过Adobe Pass [TVE仪表板](rest-api-v2-glossary.md#tve-dashboard)完成。

有关更多详细信息，请参阅[TVE仪表板渠道用户指南](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#registered-applications)或[TVE仪表板程序员用户指南](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#registered-applications)文档。

目前，您需要请求Adobe Pass身份验证代表为您新注册的应用程序（软件语句）启用REST API V2的使用，直到更新Adobe Pass [TVE仪表板](rest-api-v2-glossary.md#tve-dashboard)以允许此操作的自我管理。

为了区分使用REST API V2的客户端应用程序中使用的已注册应用程序（软件语句），我们要求您在已注册应用程序名称中添加特定的后缀，例如“RESTV2”。

### 5.客户端应用程序可以使用现有的自定义方案吗？ {#migration-faq5}

客户端应用程序可以重复使用通过Adobe Pass [TVE仪表板](rest-api-v2-glossary.md#tve-dashboard)生成的现有自定义方案。

有关更多详细信息，请参阅[TVE仪表板渠道用户指南](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#custom-schemes)或[TVE仪表板程序员用户指南](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#custom-schemes)文档。

### 6. REST API V2中是否默认启用了增强错误代码？ {#migration-faq6}

是的。

集成REST API V2的客户端应用程序受益于默认启用的增强错误代码功能。

有关更多详细信息，请参阅[增强型错误代码](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2)文档。

+++

### 从REST API V1迁移到REST API V2 {#migration-rest-api-v1-to-rest-api-v2-faqs}

如果您使用的应用程序需要从REST API V1迁移到REST API V2，请继续使用此子部分。

+++注册阶段常见问题解答

#### 1.注册阶段需要哪些高级API迁移？ {#registration-phase-v1-to-v2-faq1}

在从REST API V1迁移到REST API V2时，注册阶段没有重大变化。

客户端应用程序可以继续使用相同的流程，通过[动态客户端注册(DCR)](rest-api-v2-glossary.md#dcr)进程针对Adobe Pass身份验证进行注册并获取访问令牌。

有关更多信息，请参阅以下文档：

* [Dynamic Client注册概述](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
* [检索客户端凭据API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [检索访问令牌API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* [动态客户端注册流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

+++

+++配置阶段常见问题解答

#### 1.配置阶段需要哪些高级API迁移？ {#configuration-phase-v1-to-v2-faq1}

在从REST API V1迁移到REST API V2的过程中，下表显示了需要考虑的高级别更改：

| 范围 | REST API V1 | REST API V2 | 观察结果 |
|------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 检索具有活动集成的MVPD列表 | [GET<br/> /api/v1/config/{serviceProvider}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md) | [GET<br/> /api/v2/{serviceProvider}/配置](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

+++身份验证阶段常见问题解答

#### 1.身份验证阶段需要哪些高级API迁移？ {#authentication-phase-v1-to-v2-faq1}

在从REST API V1迁移到REST API V2的过程中，下表显示了需要考虑的高级别更改：

| 范围 | REST API V1 | REST API V2 | 观察结果 |
|-------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 检索注册码（身份验证码） | [POST<br/> /reggie/v1/{serviceProvider}/regcode](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) | [POST<br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[在辅助应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 检查注册码（身份验证码） | [GET<br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET<br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在辅助应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 在第二个屏幕（应用程序）上恢复(MVPD)身份验证 | [GET<br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [POST<br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET<br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在辅助应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 启动(MVPD)身份验证 | [GET<br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [GET<br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[在辅助应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 检查用户身份验证状态 | [GET<br/> /api/v1/checkauthn （第一个屏幕）](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md) <br/> [GET<br/> /api/v1/checkauthn （第二个屏幕）](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-flow-by-second-screen-web-app.md) | 使用以下选项之一： <br/><br/> [GET<br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 客户端应用程序可以同时出于多个目的使用这些API的响应： <br/> <ul><li>检查用户身份验证状态</li><li>检索用户配置文件</li><li>检索用户元数据信息</li></ul> <br/>有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在辅助应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| 检索用户身份验证令牌（配置文件） | [GET<br/> /api/v1/tokens/authn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) | 使用以下选项之一： <br/><br/> [GET<br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 客户端应用程序可以同时出于多个目的使用这些API的响应： <br/> <ul><li>检查用户身份验证状态</li><li>检索用户配置文件</li><li>检索用户元数据信息</li></ul> <br/>有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在辅助应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| 检索用户元数据信息 | [GET<br/> /api/v1/tokens/usermetadata](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md) | 使用以下选项之一： <br/><br/> [GET<br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 客户端应用程序可以同时出于多个目的使用这些API的响应： <br/> <ul><li>检查用户身份验证状态</li><li>检索用户配置文件</li><li>检索用户元数据信息</li></ul> <br/>有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在辅助应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

+++预授权阶段常见问题解答

#### 1.预授权阶段需要什么高级API迁移？ {#preauthorization-phase-v1-to-v2-faq1}

在从REST API V1迁移到REST API V2的过程中，下表显示了需要考虑的高级别更改：

| 范围 | REST API V1 | REST API V2 | 观察结果 |
|---------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 检索预授权的资源（预授权决策） | [GET<br/> /api/v1/preauthorize （第一个屏幕）](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources.md) <br/> [GET<br/> /api/v1/preauthorize （第二个屏幕）](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md) | [POST<br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行了基本预授权流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

+++授权阶段常见问题解答

#### 1.授权阶段需要哪些高级API迁移？ {#authorization-phase-v1-to-v2-faq1}

在从REST API V1迁移到REST API V2的过程中，下表显示了需要考虑的高级别更改：

| 范围 | REST API V1 | REST API V2 | 观察结果 |
|-------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 启动(MVPD)授权 | [GET<br/> /api/v1/authorize](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authorization.md) | [POST<br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | 客户端应用程序可以同时将此API的响应用于多个目的： <br/> <ul><li>启动(MVPD)授权</li><li>检索授权决策</li><li>检索短媒体令牌</li></ul> <br/>有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本授权流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| 检索授权令牌（授权决策） | [GET<br/> /api/v1/tokens/authz](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authorization-token.md) | [POST<br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | 客户端应用程序可以同时将此API的响应用于多个目的： <br/> <ul><li>启动(MVPD)授权</li><li>检索授权决策</li><li>检索短媒体令牌</li></ul> <br/>有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本授权流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| 检索短授权令牌（媒体令牌） | [GET<br/> /api/v1/tokens/media](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md) | [POST<br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | 客户端应用程序可以同时将此API的响应用于多个目的： <br/> <ul><li>启动(MVPD)授权</li><li>检索授权决策</li><li>检索短媒体令牌</li></ul> <br/>有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本授权流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

+++注销阶段常见问题解答

#### 1.注销阶段需要哪些高级API迁移？ {#logout-phase-v1-to-v2-faq1}

在从REST API V1迁移到REST API V2的过程中，下表显示了需要考虑的高级别更改：

| 范围 | REST API V1 | REST API V2 | 观察结果 |
|-----------------|-----------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 启动注销 | [GET<br/> /api/v1/logout](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md) | [GET<br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本注销流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++

### 从SDK迁移到REST API V2 {#migration-sdk-to-rest-api-v2}

如果您正在处理需要从SDK迁移到REST API V2的应用程序，请继续使用此子部分。

+++注册阶段常见问题解答

#### 1.注册阶段需要哪些高级API迁移？ {#registration-phase-sdk-to-v2-faq1}

在从SDK迁移到REST API V2的过程中，需要考虑一些高级别的更改，这些更改如下表所示：

##### AccessEnabler JavaScript SDK

| 范围 | SDK | REST API V2 | 观察结果 |
|--------------------------------------------|---------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 完成动态客户端注册(DCR) | 向构造函数提供软件语句 | [POST<br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET<br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[动态客户端注册概述](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[动态客户端注册流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

##### AccessEnabler iOS/tvOS SDK

| 范围 | SDK | REST API V2 | 观察结果 |
|--------------------------------------------|---------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 完成动态客户端注册(DCR) | 向构造函数提供软件语句 | [POST<br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET<br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[动态客户端注册概述](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[动态客户端注册流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

##### AccessEnabler Android SDK

| 范围 | SDK | REST API V2 | 观察结果 |
|--------------------------------------------|---------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 完成动态客户端注册(DCR) | 向构造函数提供软件语句 | [POST<br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET<br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[动态客户端注册概述](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[动态客户端注册流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

##### AccessEnabler FireOS SDK

| 范围 | SDK | REST API V2 | 观察结果 |
|--------------------------------------------|---------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 完成动态客户端注册(DCR) | 向构造函数提供软件语句 | [POST<br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET<br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[动态客户端注册概述](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[动态客户端注册流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

+++

+++配置阶段常见问题解答

#### 1.配置阶段需要哪些高级API迁移？ {#configuration-phase-sdk-to-v2-faq1}

在从SDK迁移到REST API V2的过程中，需要考虑一些高级别的更改，这些更改如下表所示：

##### AccessEnabler JavaScript SDK

| 范围 | SDK | REST API V2 | 观察结果 |
|------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 检索具有活动集成的MVPD列表 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthN) | [GET<br/> /api/v2/{serviceProvider}/配置](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

##### AccessEnabler iOS/tvOS SDK

| 范围 | SDK | REST API V2 | 观察结果 |
|------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 检索具有活动集成的MVPD列表 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) | [GET<br/> /api/v2/{serviceProvider}/配置](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

##### AccessEnabler Android SDK

| 范围 | SDK | REST API V2 | 观察结果 |
|------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 检索具有活动集成的MVPD列表 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthN) | [GET<br/> /api/v2/{serviceProvider}/配置](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

##### AccessEnabler FireOS SDK

| 范围 | SDK | REST API V2 | 观察结果 |
|------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 检索具有活动集成的MVPD列表 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) | [GET<br/> /api/v2/{serviceProvider}/配置](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

+++身份验证阶段常见问题解答

#### 1.身份验证阶段需要哪些高级API迁移？ {#authentication-phase-sdk-to-v2-faq1}

在从SDK迁移到REST API V2的过程中，需要考虑一些高级别的更改，这些更改如下表所示：

##### AccessEnabler JavaScript SDK

| 范围 | SDK | REST API V2 | 观察结果 |
|------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 启动(MVPD)身份验证 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#setSelProv) | [POST<br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET<br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[在辅助应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 检查用户身份验证状态 | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkAuthN) | 使用以下选项之一： <br/><br/> [GET<br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 客户端应用程序可以同时出于多个目的使用这些API的响应： <br/> <ul><li>检查用户身份验证状态</li><li>检索用户配置文件</li><li>检索用户元数据信息</li></ul> <br/>有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在辅助应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| 检索用户元数据信息 | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getMeta) | 使用以下选项之一： <br/><br/> [GET<br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 客户端应用程序可以同时出于多个目的使用这些API的响应： <br/> <ul><li>检查用户身份验证状态</li><li>检索用户配置文件</li><li>检索用户元数据信息</li></ul> <br/>有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在辅助应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

##### AccessEnabler iOS SDK

| 范围 | SDK | REST API V2 | 观察结果 |
|------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 启动(MVPD)身份验证 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [POST<br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET<br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[在辅助应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 检查用户身份验证状态 | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthN) | 使用以下选项之一： <br/><br/> [GET<br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 客户端应用程序可以同时出于多个目的使用这些API的响应： <br/> <ul><li>检查用户身份验证状态</li><li>检索用户配置文件</li><li>检索用户元数据信息</li></ul> <br/>有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在辅助应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| 检索用户元数据信息 | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getMeta) | 使用以下选项之一： <br/><br/> [GET<br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 客户端应用程序可以同时出于多个目的使用这些API的响应： <br/> <ul><li>检查用户身份验证状态</li><li>检索用户配置文件</li><li>检索用户元数据信息</li></ul> <br/>有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在辅助应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

##### AccessEnabler tvOS SDK

| 范围 | SDK | REST API V2 | 观察结果 |
|-------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 检索注册码（身份验证码） | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [POST<br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[在辅助应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 检查注册码（身份验证码） | [GET<br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET<br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在辅助应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 在第二个屏幕（应用程序）上恢复(MVPD)身份验证 | [GET<br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [POST<br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET<br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在辅助应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 启动(MVPD)身份验证 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [POST<br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET<br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[在辅助应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 检查用户身份验证状态 | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthN) | 使用以下选项之一： <br/><br/> [GET<br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 客户端应用程序可以同时出于多个目的使用这些API的响应： <br/> <ul><li>检查用户身份验证状态</li><li>检索用户配置文件</li><li>检索用户元数据信息</li></ul> <br/>有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在辅助应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| 检索用户元数据信息 | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getMeta) | 使用以下选项之一： <br/><br/> [GET<br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 客户端应用程序可以同时出于多个目的使用这些API的响应： <br/> <ul><li>检查用户身份验证状态</li><li>检索用户配置文件</li><li>检索用户元数据信息</li></ul> <br/>有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在辅助应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

##### AccessEnabler Android SDK

| 范围 | SDK | REST API V2 | 观察结果 |
|------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 启动(MVPD)身份验证 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#setSelectedProvider) | [POST<br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET<br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[在辅助应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 检查用户身份验证状态 | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkAuthN) | 使用以下选项之一： <br/><br/> [GET<br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 客户端应用程序可以同时出于多个目的使用这些API的响应： <br/> <ul><li>检查用户身份验证状态</li><li>检索用户配置文件</li><li>检索用户元数据信息</li></ul> <br/>有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在辅助应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| 检索用户元数据信息 | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getMetadata) | 使用以下选项之一： <br/><br/> [GET<br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 客户端应用程序可以同时出于多个目的使用这些API的响应： <br/> <ul><li>检查用户身份验证状态</li><li>检索用户配置文件</li><li>检索用户元数据信息</li></ul> <br/>有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在辅助应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

##### AccessEnabler FireOS SDK

| 范围 | SDK | REST API V2 | 观察结果 |
|-------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 检索注册码（身份验证码） | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [POST<br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[在辅助应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 检查注册码（身份验证码） | [GET<br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET<br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在辅助应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 在第二个屏幕（应用程序）上恢复(MVPD)身份验证 | [GET<br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [POST<br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET<br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在辅助应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 启动(MVPD)身份验证 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [POST<br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET<br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[在辅助应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 检查用户身份验证状态 | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkAuthN) | 使用以下选项之一： <br/><br/> [GET<br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 客户端应用程序可以同时出于多个目的使用这些API的响应： <br/> <ul><li>检查用户身份验证状态</li><li>检索用户配置文件</li><li>检索用户元数据信息</li></ul> <br/>有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在辅助应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| 检索用户元数据信息 | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getMetadata) | 使用以下选项之一： <br/><br/> [GET<br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 客户端应用程序可以同时出于多个目的使用这些API的响应： <br/> <ul><li>检查用户身份验证状态</li><li>检索用户配置文件</li><li>检索用户元数据信息</li></ul> <br/>有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在辅助应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

+++预授权阶段常见问题解答

#### 1.预授权阶段需要什么高级API迁移？ {#preauthorization-phase-sdk-to-v2-faq1}

在从SDK迁移到REST API V2的过程中，需要考虑一些高级别的更改，这些更改如下表所示：

##### AccessEnabler JavaScript SDK

| 范围 | SDK | REST API V2 | 观察结果 |
|---------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 检索预授权的资源（预授权决策） | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkPreauthRes) <br/> [AccessEnabler.preauthorize](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/preauthorize-api-javascript-sdk.md) | [POST<br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行了基本预授权流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

##### AccessEnabler iOS/tvOS SDK

| 范围 | SDK | REST API V2 | 观察结果 |
|---------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 检索预授权的资源（预授权决策） | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkPreauth) <br/> [AccessEnabler.preauthorize](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/preauthorize-api-ios-tvos-sdk.md) | [POST<br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行了基本预授权流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

##### AccessEnabler Android SDK

| 范围 | SDK | REST API V2 | 观察结果 |
|---------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 检索预授权的资源（预授权决策） | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkPreauth) <br/> [AccessEnabler.preauthorize](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/preauthorize-api-android-sdk.md) | [POST<br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行了基本预授权流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

| 范围 | SDK | REST API V2 | 观察结果 |
|---------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 检索预授权的资源（预授权决策） | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkPreauth) | [POST<br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行了基本预授权流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

+++授权阶段常见问题解答

#### 1.授权阶段需要哪些高级API迁移？ {#authorization-phase-sdk-to-v2-faq1}

在从SDK迁移到REST API V2的过程中，需要考虑一些高级别的更改，这些更改如下表所示：

##### AccessEnabler JavaScript SDK

| 范围 | SDK | REST API V2 | 观察结果 |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 检索短授权令牌（媒体令牌） | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthZ) | [POST<br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | 客户端应用程序可以同时将此API的响应用于多个目的： <br/> <ul><li>启动(MVPD)授权</li><li>检索授权决策</li><li>检索短媒体令牌</li></ul> <br/>有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本授权流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

##### AccessEnabler iOS/tvOS SDK

| 范围 | SDK | REST API V2 | 观察结果 |
|--------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 检索短授权令牌（媒体令牌） | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthZ) | [POST<br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | 客户端应用程序可以同时将此API的响应用于多个目的： <br/> <ul><li>启动(MVPD)授权</li><li>检索授权决策</li><li>检索短媒体令牌</li></ul> <br/>有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本授权流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

##### AccessEnabler Android SDK

| 范围 | SDK | REST API V2 | 观察结果 |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 检索短授权令牌（媒体令牌） | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthZ) | [POST<br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | 客户端应用程序可以同时将此API的响应用于多个目的： <br/> <ul><li>启动(MVPD)授权</li><li>检索授权决策</li><li>检索短媒体令牌</li></ul> <br/>有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本授权流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

##### AccessEnabler FireOS SDK

| 范围 | SDK | REST API V2 | 观察结果 |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 检索短授权令牌（媒体令牌） | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthZ) | [POST<br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | 客户端应用程序可以同时将此API的响应用于多个目的： <br/> <ul><li>启动(MVPD)授权</li><li>检索授权决策</li><li>检索短媒体令牌</li></ul> <br/>有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本授权流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

+++注销阶段常见问题解答

#### 1.注销阶段需要哪些高级API迁移？ {#logout-phase-sdk-to-v2-faq1}

在从SDK迁移到REST API V2的过程中，需要考虑一些高级别的更改，这些更改如下表所示：

##### AccessEnabler JavaScript SDK

| 范围 | SDK | REST API V2 | 观察结果 |
|-----------------|--------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 启动注销 | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#logout) | [GET<br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本注销流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

##### AccessEnabler iOS/tvOS SDK

| 范围 | SDK | REST API V2 | 观察结果 |
|-----------------|---------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 启动注销 | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#logout) | [GET<br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本注销流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

##### AccessEnabler Android SDK

| 范围 | SDK | REST API V2 | 观察结果 |
|-----------------|--------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 启动注销 | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#logout) | [GET<br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本注销流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

##### AccessEnabler FireOS SDK

| 范围 | SDK | REST API V2 | 观察结果 |
|-----------------|-----------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 启动注销 | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#logout) | [GET<br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本注销流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++
