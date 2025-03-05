---
title: REST API V2常见问题解答
description: REST API V2常见问题解答
exl-id: 2dd74b47-126e-487b-b467-c16fa8cc14c1
source-git-commit: d8097b8419aa36140e6ff550714730059555fd14
workflow-type: tm+mt
source-wordcount: '9072'
ht-degree: 0%

---

# REST API V2常见问题解答 {#rest-api-v2-faqs}

>[!IMPORTANT]
>
> 此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

本文档提供了有关Adobe Pass身份验证REST API V2采用的常见问题解答的高级概述性答案。

有关整个REST API V2的更多信息，请参阅[REST API V2概述](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)文档。

## 一般常见问题解答 {#general-faqs}

如果您正在使用的应用程序需要集成REST API V2，请从该部分开始，无论该应用程序是新应用程序还是从[REST API V1](#migration-rest-api-v1-to-rest-api-v2)或[SDK](#migration-sdk-to-rest-api-v2)迁移的现有应用程序。

有关迁移详细信息和步骤的更多信息，另请参阅下一部分。

### 注册阶段常见问题解答 {#registration-phase-faqs-general}

+++注册阶段常见问题解答

请参阅[Dynamic Client Registration (DCR)常见问题解答](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md#rest-api-v2-access-faqs)文档。

+++

### 配置阶段常见问题解答 {#configuration-phase-faqs-general}

+++配置阶段常见问题解答

#### 1.配置阶段的用途是什么？ {#configuration-phase-faq1}

配置阶段的目的是向客户端应用程序提供与其主动集成的MVPD列表以及Adobe Pass身份验证为每个MVPD保存的配置详细信息（例如，`id`、`displayName`、`logoUrl`等）。

当客户端应用程序需要要求用户选择其电视提供商时，配置阶段是身份验证阶段的先决条件步骤。

#### 2.配置阶段是否为强制阶段？ {#configuration-phase-faq2}

配置阶段不是强制性的，仅当用户需要选择其MVPD进行身份验证或重新身份验证时，客户端应用程序必须检索配置。

在以下情形中，客户端应用程序可以跳过此阶段：

* 用户已经过身份验证。
* 通过基本或提升[TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md)功能向用户提供临时访问权限。
* 用户身份验证已过期，但客户端应用程序已缓存之前选择的MVPD作为用户体验驱动型选择，并且只是提示用户确认他们仍然是该MVPD的订阅者。

#### 3.什么是配置以及它的有效期？ {#configuration-phase-faq3}

该配置是[词汇表](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#configuration)文档中定义的术语。

该配置包含由以下属性`id`、`displayName`、`logoUrl`等定义的MVPD列表，这些属性可以从[配置](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)终结点中检索。

仅当用户需要选择其MVPD进行身份验证或重新身份验证时，客户端应用程序必须检索配置。

当用户需要选择电视提供商时，客户端应用程序可使用配置响应来显示具有可用MVPD选项的UI选取器。

客户端应用程序必须存储用户选定的MVPD标识符(由MVPD的配置级别`id`属性指定)，以继续身份验证、预授权、授权或注销阶段。

有关详细信息，请参阅[检索配置](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)文档。

#### 4.客户端应用程序是否应将配置响应信息缓存到永久存储中？ {#configuration-phase-faq4}

仅当用户需要选择其MVPD进行身份验证或重新身份验证时，客户端应用程序必须检索配置。

客户端应用程序应将配置响应信息缓存到内存存储中，以避免不必要的请求，并改善以下情况下的用户体验：

* 用户已经过身份验证。
* 通过基本或提升[TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md)功能向用户提供临时访问权限。
* 用户身份验证已过期，但客户端应用程序已缓存之前选择的MVPD作为用户体验驱动型选择，并且只是提示用户确认他们仍然是该MVPD的订阅者。

#### 5.客户端应用程序能否管理自己的MVPD列表？ {#configuration-phase-faq5}

客户端应用程序可以管理自己的MVPD列表，但需要使MVPD标识符与Adobe Pass身份验证保持同步。 因此，建议使用Adobe Pass身份验证提供的配置，以确保列表是最新且准确的。

如果提供的Adobe Pass标识符无效，或者客户端应用程序与指定的[服务提供程序](rest-api-v2-glossary.md#service-provider)没有活动集成，则客户端应用程序将从MVPD身份验证REST API V2收到[错误](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2)。

#### 6.客户端应用程序能否过滤MVPD列表？ {#configuration-phase-faq6}

客户端应用程序可以通过基于其自身业务逻辑和诸如先前选择的用户位置或用户历史等要求实现定制机制来过滤配置响应中提供的MVPD列表。

客户端应用程序可以筛选[TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md) MVPD的列表或其集成仍在开发或测试中的MVPD。

#### 7.如果与MVPD的集成被禁用并标记为不活动，会发生什么情况？ {#configuration-phase-faq7}

如果禁用了与MVPD的集成并将其标记为不活动，则将从进一步配置响应中提供的MVPD列表中删除MVPD，同时需要考虑以下两个重要后果：

* 该MVPD的未经身份验证的用户将无法再使用该MVPD完成身份验证阶段。
* 经过身份验证的MVPD用户将无法再使用该MVPD完成预授权、授权或注销阶段。

如果用户选择的Adobe Pass不再与指定的[服务提供程序](rest-api-v2-glossary.md#service-provider)有效集成，则客户端应用程序将从MVPD身份验证REST API V2收到[错误](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2)。

#### 8.如果已重新启用与MVPD的集成并标记为活动，会发生什么情况？ {#configuration-phase-faq8}

重新启用与MVPD的集成并标记为活动时，MVPD将重新包含在进一步配置响应中提供的MVPD列表中，需要考虑以下两个重要后果：

* 该MVPD的未经身份验证的用户将能够再次使用该MVPD完成身份验证阶段。
* 经过身份验证的MVPD用户将能够使用该MVPD再次完成预授权、授权或注销阶段。

#### 9.如何启用或禁用与MVPD的集成？ {#configuration-phase-faq9}

此操作可以由您的组织管理员或代表您行事的Adobe Pass身份验证代表通过Adobe Pass [TVE仪表板](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)完成。

有关更多详细信息，请参阅[TVE仪表板集成用户指南](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#disable-integration)文档。

+++

### 身份验证阶段常见问题解答 {#authentication-phase-faqs-general}

+++身份验证阶段常见问题解答

#### 1.身份验证阶段的用途是什么？ {#authentication-phase-faq1}

身份验证阶段的目的是向客户端应用程序提供验证用户身份和获取用户元数据信息的能力。

当客户端应用程序需要播放内容时，身份验证阶段是预授权阶段或授权阶段的先决步骤。

#### 2.什么是身份验证会话？该会话的有效时间是多久？ {#authentication-phase-faq2}

身份验证会话是[术语表](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#session)文档中定义的术语。

身份验证会话存储有关启动身份验证过程的信息，该信息可以从会话端点检索。

身份验证会话的有效期为`notAfter`时间戳在发出问题时指定的有限且较短的时间范围，这表示在需要重新启动流之前，用户必须完成身份验证过程的时间量。

客户端应用程序可以使用身份验证会话响应来了解如何继续进行身份验证过程。 请注意，在某些情况下，用户不需要进行身份验证，例如提供临时访问、降级访问或用户已经过身份验证。

有关更多信息，请参阅以下文档：

* [创建身份验证会话API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [恢复身份验证会话API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [在主应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [在辅助应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

#### 3.什么是身份验证代码以及它的有效时间长短？ {#authentication-phase-faq3}

身份验证代码是[词汇表](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#code)文档中定义的术语。

验证代码存储当用户启动验证过程时生成的唯一值，并唯一标识用户的验证会话，直到该过程完成或验证会话过期为止。

身份验证代码在由`notAfter`时间戳启动身份验证会话时指定的有限和较短时间范围内有效，这表示在需要重新启动流之前，用户必须完成身份验证过程的时间量。

客户端应用程序可以使用身份验证代码验证用户是否已成功完成身份验证并检索用户的配置文件信息，通常通过轮询机制。

客户端应用程序还可以使用身份验证代码来允许用户在同一设备或第二个（屏幕）设备上完成或恢复身份验证过程，因为身份验证会话没有过期。

有关更多信息，请参阅以下文档：

* [创建身份验证会话API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [恢复身份验证会话API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [在主应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [在辅助应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

#### 4.客户端应用程序如何知道用户是否键入了有效的身份验证代码以及身份验证会话是否尚未过期？ {#authentication-phase-faq4}

客户端应用程序可以通过向负责恢复身份验证会话或检索与身份验证代码关联的身份验证会话信息的“会话”端点之一发送请求，来验证用户在辅助（屏幕）应用程序中键入的身份验证代码。

如果提供的身份验证代码键入错误或身份验证会话过期，客户端应用程序将收到[错误](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2)。

有关详细信息，请参阅[恢复身份验证会话](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)和[检索身份验证会话](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)文档。

#### 5.客户端应用程序如何知道用户是否已验证？ {#authentication-phase-faq5}

客户端应用程序可以查询以下能够验证用户是否已过身份验证的端点之一并返回配置文件信息：

* [配置文件端点API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [特定MVPD API的配置文件端点](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [特定（身份验证）代码API的配置文件端点](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

有关更多详细信息，请参阅以下文档：

* [基本配置文件在主要应用程序中执行](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [基本配置文件在辅助应用程序中执行](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

#### 6.什么是配置文件，它的有效时间是多久？ {#authentication-phase-faq6}

配置文件是[词汇表](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#profile)文档中定义的术语。

配置文件存储有关用户身份验证有效性的信息、元数据信息以及许多可从配置文件端点检索的信息。

客户端应用程序可以使用配置文件来了解用户的验证状态、访问用户元数据信息、查找用于验证的方法或用于提供身份的实体。

有关更多详细信息，请参阅以下文档：

* [配置文件端点API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [特定MVPD API的配置文件端点](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [特定（身份验证）代码API的配置文件端点](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
* [基本配置文件在主要应用程序中执行](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [基本配置文件在辅助应用程序中执行](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

该配置文件在被`notAfter`时间戳查询时指定的有限时间范围内有效，这表示在需要再次完成身份验证阶段之前，用户具有有效身份验证的时间量。

您组织的一名管理员或代表您行事的Adobe Pass身份验证代表可以通过Adobe Pass [TVE仪表板](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)查看和更改这个称为身份验证(authN) [TTL](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#ttl)的有限时间范围。

有关更多详细信息，请参阅[TVE仪表板集成用户指南](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows)文档。

#### 7.客户端应用程序是否应将用户的配置文件信息缓存到永久存储中？ {#authentication-phase-faq7}

客户端应用程序应将用户的配置文件信息缓存在永久性存储中，以避免不必要的请求，并提高用户体验，同时应考虑到以下方面：

| 属性 | 用户体验 |
|--------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `attributes` | 客户端应用程序可以使用此项根据不同的[用户元数据](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)密钥（例如，`zip`、`maxRating`等）个性化用户体验。 |
| `mvpd` | 客户端应用程序可以使用此项来跟踪用户选择的电视提供商。<br/><br/>当当前用户配置文件过期时，客户端应用程序可以使用记住的MVPD选择，只需请求用户确认即可。 |
| `notAfter` | 客户端应用程序可以使用此项来跟踪用户配置文件过期日期，并在过期时触发重新身份验证过程，从而避免在预授权或授权阶段出现错误。<br/><br/>客户端应用程序错误处理必须能够处理[authenticated_profile_expired](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2)错误代码，这表示客户端应用程序要求用户重新进行身份验证。 |

#### 8.客户端应用程序能否在不要求重新验证的情况下扩展用户的配置文件？ {#authentication-phase-faq8}

不适用。

在没有用户交互的情况下，用户配置文件不能超出其有效期，因为它的过期时间是由使用MVPD建立的身份验证TTL确定的。

因此，客户端应用程序必须提示用户重新进行身份验证，并与MVPD登录页面进行交互，以便刷新我们在系统上的配置文件。

但是，对于支持[基于主目录的身份验证](/help/authentication/integration-guide-programmers/features-standard/hba-access/home-based-authentication.md) (HBA)的MVPD，将不需要用户输入凭据。

#### 9.每个可用配置文件端点的用例是什么？ {#authentication-phase-faq9}

基本配置文件端点旨在为客户端应用程序提供了解用户的身份验证状态、访问用户元数据信息、查找用于身份验证的方法或用于提供身份的实体的功能。

每个端点都适合特定的用例，如下所示：

| API | 描述 | 用例 |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [配置文件端点API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) | 检索所有用户配置文件。 | **用户首次打开客户端应用程序**<br/><br/>&#x200B;在这种情况下，客户端应用程序不会将用户选定的MVPD标识符缓存到永久存储中。<br/><br/>因此，它将发送单个请求以检索所有可用的用户配置文件。 |
| 特定MVPD API的[配置文件端点](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) | 检索与特定MVPD关联的用户配置文件。 | **用户在上次访问中进行身份验证后返回客户端应用程序**<br/><br/>&#x200B;在这种情况下，客户端应用程序必须将用户之前选择的MVPD标识符缓存到永久存储中。<br/><br/>因此，它将发送一个请求，以检索该特定MVPD的用户配置文件。 |
| [特定（身份验证）代码API的配置文件端点](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 检索与特定身份验证代码关联的用户配置文件。 | **用户启动身份验证过程**<br/><br/>&#x200B;在此方案中，客户端应用程序必须确定用户是否已成功完成身份验证并检索其配置文件信息。<br/><br/>因此，它将启动轮询机制以检索与身份验证代码关联的用户配置文件。 |

有关详细信息，请参阅在主应用程序](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)中执行的[基本配置文件流和在辅助应用程序](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)文档中执行的[基本配置文件流。

配置文件SSO端点具有不同的用途，它使客户端应用程序能够使用合作伙伴身份验证响应创建用户配置文件，并在单次、一次性操作中检索该配置文件。

| API | 描述 | 用例 |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [配置文件SSO端点API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md) | 使用合作伙伴身份验证响应创建和检索用户配置文件。 | **用户允许应用程序使用合作伙伴单点登录进行身份验证**<br/><br/>&#x200B;在此方案中，客户端应用程序必须在收到合作伙伴身份验证响应后创建用户配置文件，并在单次、一次性操作中检索该配置文件。 |

对于任何后续查询，必须使用基本配置文件端点确定用户的身份验证状态、访问用户元数据信息、查找用于身份验证的方法或用于提供身份的实体。

有关更多详细信息，请参阅[使用合作伙伴流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md)的单点登录[Apple SSO指南(REST API V2)](/help/authentication/integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-cookbook-rest-api-v2.md)文档。

#### 10.如果用户有多个MVPD配置文件，客户端应用程序应该怎么做？ {#authentication-phase-faq10}

当用户有多个MVPD配置文件时，客户端应用程序负责确定处理此场景的最佳方法。

客户端应用程序可以选择提示用户选择所需的MVPD配置文件或自动进行选择，例如从响应中选择第一个用户配置文件或具有最长有效期的用户配置文件。

#### 11.用户配置文件过期后会发生什么情况？ {#authentication-phase-faq11}

用户配置文件过期后，将不再包含在来自配置文件端点的响应中。

如果Profiles端点返回空的配置文件映射响应，则客户端应用程序必须创建新的身份验证会话并提示用户重新进行身份验证。

有关详细信息，请参阅[创建身份验证会话API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)文档。

#### 12.用户配置文件何时失效？ {#authentication-phase-faq12}

在以下情况下，用户配置文件将失效：

* 身份验证TTL过期时间，如配置文件终结点响应中的`notAfter`时间戳所示。
* 客户端应用程序更改[AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)标头值时。
* 客户端应用程序更新用于检索[Authorization](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md)标头值的客户端凭据时。
* 当客户端应用程序撤销或更新用于获取客户端凭据的软件语句时。

#### 13.客户端应用程序何时应启动轮询机制？ {#authentication-phase-faq13}

为确保效率并避免不必要的请求，客户端应用程序必须在以下条件下启动轮询机制：

**在主（屏幕）应用程序内执行的身份验证**

在浏览器组件加载为[会话](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)终结点请求中的`redirectUrl`参数指定的URL后，主（流）应用程序应在用户到达最终目标页面时开始轮询。

**在辅助（屏幕）应用程序内执行的身份验证**

主（流）应用程序应在用户启动身份验证过程后立即开始轮询 — 在收到[会话](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)端点响应并向用户显示身份验证代码之后。

#### 14.客户端应用程序何时应停止轮询机制？ {#authentication-phase-faq14}

为确保效率并避免不必要的请求，客户端应用程序必须在以下条件下停止轮询机制：

**身份验证成功**

已成功检索用户的配置文件信息，确认其身份验证状态。 此时，不再需要轮询。

**身份验证会话和代码过期**

身份验证会话和代码将过期，如[会话](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)终结点响应中的`notAfter`时间戳所指示。 如果发生这种情况，用户必须重新启动身份验证过程，使用以前的身份验证代码的轮询应立即停止。

**已生成新的身份验证代码**

如果用户在主设备（屏幕）上请求新的身份验证代码，则现有会话不再有效，使用以前的身份验证代码的轮询应立即停止。

#### 15.客户端应用程序应使用何种间隔时间进行轮询机制？ {#authentication-phase-faq15}

为确保效率并避免不必要的请求，客户端应用程序必须在以下条件下配置轮询机制频率：

| **在主（屏幕）应用程序内执行的身份验证** | **在辅助（屏幕）应用程序内执行的身份验证** |
|----------------------------------------------------------------------|----------------------------------------------------------------------|
| 主（流）应用程序应每1-5秒轮询一次。 | 主（流）应用程序应每3-5秒轮询一次。 |

#### 16.客户端应用程序可以发送的轮询请求的最大数量是多少？ {#authentication-phase-faq16}

客户端应用程序必须遵守Adobe Pass身份验证[限制机制](/help/authentication/integration-guide-programmers/throttling-mechanism.md#throttling-mechanism-limits)定义的当前限制。

客户端应用程序错误处理必须能够处理[429请求过多](/help/authentication/integration-guide-programmers/throttling-mechanism.md#throttling-mechanism-response)错误代码，这表示客户端应用程序已超出允许的最大请求数。

有关更多详细信息，请参阅[节流机制](/help/authentication/integration-guide-programmers/throttling-mechanism.md)文档。

#### 17.客户端应用程序如何获取用户的元数据信息？ {#authentication-phase-faq17}

客户端应用程序可以查询以下端点之一，这些端点能够返回[用户元数据](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)信息作为配置文件信息的一部分：

* [配置文件端点API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [特定MVPD API的配置文件端点](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [特定（身份验证）代码API的配置文件端点](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

客户端应用程序不需要查询单独的端点来检索用户的元数据信息，因为验证用户是否经过身份验证时，获得的配置文件信息中已包含该信息。

有关更多详细信息，请参阅以下文档：

* [基本配置文件在主要应用程序中执行](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [基本配置文件在辅助应用程序中执行](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

#### 18.客户端应用程序应如何管理降级访问？ {#authentication-phase-faq18}

鉴于您的组织打算使用[降级](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md)功能，客户端应用程序必须处理降级的访问流，这概述了REST API v2端点在此类场景中的行为。

有关更多详细信息，请参阅[降级访问流](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/degraded-access-flows/rest-api-v2-access-degraded-flows.md)文档。

+++

### 预授权阶段常见问题解答 {#preauthorization-phase-faqs-general}

+++预授权阶段常见问题解答

#### 1.预授权阶段的目的是什么？ {#preauthorization-phase-faq1}

预授权阶段的目的是让客户端应用程序能够呈现用户有权访问的目录中的资源子集。

当用户首次打开客户端应用程序或导航到新部分时，预授权阶段可以增强用户体验。

#### 2.预授权阶段是否为强制性的？ {#preauthorization-phase-faq2}

预授权阶段不是强制性的，如果客户端应用程序希望呈现资源目录而不是首先根据用户权利筛选资源，则可以跳过此阶段。

#### 3.什么是预授权决定？ {#preauthorization-phase-faq3}

预授权是在[词汇表](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#preauthorization)文档中定义的术语，而决策术语也可在[词汇表](rest-api-v2-glossary.md#decision)中找到。

预授权决策存储有关MVPD预授权过程查询结果的信息，这些信息可以从决策预授权端点检索。

客户端应用程序可以使用预授权决策来呈现电视提供商（信息）决策将允许用户访问的资源的子集。

有关更多详细信息，请参阅以下文档：

* [检索预授权决策API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
* [在主应用程序中执行的基本预授权流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

#### 4.客户端应用程序是否应将预授权决策缓存到永久存储中？ {#preauthorization-phase-faq4}

客户端应用程序不需要将预授权决策存储在永久存储中。 但是，建议将允许决策缓存到内存中以改进用户体验。 这有助于避免对已预授权的资源的Decisions Preauthorize端点进行不必要的调用，从而减少延迟并提高性能。

#### 5.客户端应用程序如何确定预授权决定被拒绝的原因？ {#preauthorization-phase-faq5}

通过检查决策预授权终结点响应中包含的[错误代码和消息](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)，客户端应用程序可以确定拒绝预授权决策的原因。 这些详细信息可让您深入了解预授权请求被拒绝的具体原因，从而帮助告知用户体验或在应用程序中触发任何必要的处理。

请确保在预授权决策被拒绝时，为检索预授权决策而实施的任何重试机制都不会导致无限循环。

考虑将重试限制为合理数字，并通过向用户提供明确的反馈，谨慎处理拒绝请求。

#### 6.为何预授权决策缺少媒体令牌？ {#preauthorization-phase-faq6}

预授权决策缺少媒体令牌，因为预授权阶段不能用于播放资源，因为这是授权阶段的目的。

#### 7.如果已经存在预授权决定，是否可以跳过授权阶段？ {#preauthorization-phase-faq7}

不适用。

即使预授权决策可用，也不能跳过授权阶段。 预授权决策仅供参考，不会授予实际的播放权限。 预授权阶段旨在提供早期指导，但在播放任何内容之前仍需要授权阶段。

#### 8.什么是资源？支持哪些格式？ {#preauthorization-phase-faq8}

资源是[术语表](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#resource)文档中定义的术语。

资源是与MVPD商定的唯一标识符，并与客户端应用程序可以流式传输的内容相关联。

资源唯一标识符可以有两种格式：

* 简单的字符串格式，如渠道（品牌）的唯一标识符。
* 一种媒体RSS (MRSS)格式，包含标题、评级和家长控制元数据等附加信息。

有关更多详细信息，请参阅[受保护的资源](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#protected-resources)文档。

#### 9.客户端应用程序一次可获取多少资源预授权决定？ {#preauthorization-phase-faq9}

由于MVPD施加的条件，客户端应用程序可以在单个API请求中为有限数量的资源获取预授权决策，通常最多可达5个。

在通过Adobe Pass [TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)与MVPD达成一致后，您的组织管理员或代表您行事的Adobe Pass身份验证代表可以查看和更改资源的最大数量。

有关更多详细信息，请参阅[TVE仪表板集成用户指南](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#add-more-properties)文档。

+++

### 授权阶段常见问题解答 {#authorization-phase-faqs-general}

+++授权阶段常见问题解答

#### 1.授权阶段的目的是什么？ {#authorization-phase-faq1}

授权阶段的目的是让客户端应用程序能够在使用MVPD验证用户请求的资源后，播放这些资源。

#### 2.授权阶段是否为强制性的？ {#authorization-phase-faq2}

授权阶段是强制性的，如果客户端应用程序希望播放用户请求的资源，则无法跳过此阶段，因为它需要在释放流之前与MVPD验证用户是否有权使用流。

#### 3.授权决定是什么，其有效期多长？ {#authorization-phase-faq3}

授权是在[词汇表](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#authorization)文档中定义的术语，而决策术语也可在[词汇表](rest-api-v2-glossary.md#decision)中找到。

授权决策存储有关MVPD授权过程查询结果的信息，这些信息可以从Decisions Authorize端点检索。

客户端应用程序可以使用包含媒体令牌的授权决策来播放资源流，以防电视提供商（权威）决策允许用户访问它。

有关更多详细信息，请参阅以下文档：

* [检索授权决策API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [在主应用程序中执行的基本授权流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

授权决策在问题时指定的有限且较短的时间范围内有效，指示在再次查询MVPD之前，Adobe Pass身份验证将缓存该决策的时间。

您组织的一名管理员或代表您行事的Adobe Pass身份验证代表可以通过Adobe Pass [TVE仪表板](rest-api-v2-glossary.md#tve-dashboard)查看和更改这个称为授权(authZ) [TTL](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#ttl)的有限时间范围。

有关更多详细信息，请参阅[TVE仪表板集成用户指南](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows)文档。

#### 4.客户端应用程序是否应将授权决策缓存到永久存储中？ {#authorization-phase-faq4}

客户端应用程序不需要将授权决策存储在永久存储中。

#### 5.客户端应用程序如何确定授权决定被拒绝的原因？ {#authorization-phase-faq5}

通过检查决策授权终结点响应中包含的[错误代码和消息](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)，客户端应用程序可以确定拒绝授权决策的原因。 这些详细信息可让您深入了解授权请求被拒绝的具体原因，从而帮助告知用户体验或在应用程序中触发任何必要的处理。

请确保在授权决策被拒绝时，为检索授权决策而实施的任何重试机制都不会导致无限循环。

考虑将重试限制为合理数字，并通过向用户提供明确的反馈，谨慎处理拒绝请求。

#### 6.什么是媒体令牌，其有效期是多久？ {#authorization-phase-faq6}

媒体令牌是[词汇表](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#media-token)文档中定义的术语。

媒体令牌包含以明文发送的已签名字符串，可以从决策授权端点检索。

有关详细信息，请参阅[媒体令牌验证器](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier)文档。

媒体令牌在发布时指定的有限且较短的时间范围内有效，指示客户端应用程序必须验证和使用它之前的时间限制。

客户端应用程序可以使用媒体令牌播放资源流，以防电视提供商（权威）决策允许用户访问它。

有关更多详细信息，请参阅以下文档：

* [检索授权决策API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [在主应用程序中执行的基本授权流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

#### 7.客户端应用程序是否应在播放资源流之前验证媒体令牌？ {#authorization-phase-faq7}

是的。

客户端应用程序必须在开始播放资源流之前验证媒体令牌。 应使用[媒体令牌验证器](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier)执行此验证。 通过验证返回的`token`中的`serializedToken`，客户端应用程序有助于防止未经授权的访问（如流翻录），并确保只有经过适当授权的用户才能播放内容。

#### 8.客户端应用程序是否应在播放期间刷新过期的媒体令牌？ {#authorization-phase-faq8}

不适用。

当流正在主动播放时，不需要客户端应用程序刷新过期的媒体令牌。 如果媒体令牌在播放期间过期，则应该允许流继续而不会中断。 但是，下次用户尝试播放同一资源时，客户端必须请求新的授权决定，并获取新的媒体令牌。

#### 9.授权决策中每个时间戳属性的用途是什么？ {#authorization-phase-faq9}

授权决策包括多个时间戳属性，这些属性提供有关授权本身的有效期和相关媒体令牌的基本上下文。 根据时间戳与授权决策还是媒体令牌的关系，这些时间戳有不同的用途。

**决策级别的时间戳**

以下时间戳描述整体授权决策的有效期：

| 属性 | 描述 | 注释 |
|-------------|------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `notBefore` | 授权决策的发布时间。 | 这将标记授权有效性窗口的开始。 |
| `notAfter` | 授权决策到期的时间。 | [授权生存时间(TTL)](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#authorization-ttl-management)确定授权在需要重新授权之前保持有效的时间。 此TTL将与MVPD代表协商。 |

**令牌级时间戳**

以下时间戳描述与授权决策关联的媒体令牌的有效期：

| 属性 | 描述 | 注释 |
|-------------|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `notBefore` | 发布媒体令牌的时间。 | 当令牌对于播放变得有效时，此标记。 |
| `notAfter` | 媒体令牌过期的时间。 | 媒体令牌的生命周期特意缩短（通常为7分钟），以最大程度地降低误用风险，并解决令牌生成服务器和令牌验证服务器之间的潜在时钟差异。 |

#### 10.什么是资源？支持哪些格式？ {#authorization-phase-faq10}

资源是[术语表](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#resource)文档中定义的术语。

资源是与MVPD商定的唯一标识符，并与客户端应用程序可以流式传输的内容相关联。

资源唯一标识符可以有两种格式：

* 简单的字符串格式，如渠道（品牌）的唯一标识符。
* 一种媒体RSS (MRSS)格式，包含标题、评级和家长控制元数据等附加信息。

有关更多详细信息，请参阅[受保护的资源](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#protected-resources)文档。

#### 11.客户应用程序一次可获取多少资源作出授权决定？ {#authorization-phase-faq11}

由于MVPD施加的条件，客户端应用程序可以在单个API请求中获取对有限数量的资源的授权决策，通常最多可达1。

+++

### 注销阶段常见问题解答 {#logout-phase-faqs-general}

+++注销阶段常见问题解答

#### 1.注销阶段的目的是什么？ {#logout-phase-faq1}

注销阶段的目的是让客户端应用程序能够根据用户请求在Adobe Pass身份验证中终止用户的已验证配置文件。

#### 2.注销阶段是否为必填项？ {#logout-phase-faq2}

注销阶段是强制性的，客户端应用程序必须为用户提供注销功能。

+++

### 标头常见问题解答 {#headers-faqs-general}

+++标头常见问题解答

#### 1.如何计算Authorization标头的值？ {#headers-faq1}

>[!IMPORTANT]
>
> 如果客户端应用程序从REST API V1迁移到REST API V2，则客户端应用程序可以继续使用与之前相同的方法获取`Bearer`访问令牌值。

[Authorization](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md)请求标头包含客户端应用程序访问受Adobe Pass保护的API所需的`Bearer`访问令牌。

在注册阶段，必须从Adobe Pass身份验证中获取授权标头值。

有关更多详细信息，请参阅以下文档：

* [Dynamic Client注册概述](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
* [检索客户端凭据API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [检索访问令牌API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* [动态客户端注册流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

#### 2.如何计算AP-Device-Identifier标头的值？ {#headers-faq2}

>[!IMPORTANT]
>
> 如果客户端应用程序从REST API V1迁移到REST API V2，则客户端应用程序可以继续使用与之前相同的方法计算设备标识符值。

[AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)请求标头包含由客户端应用程序创建的流设备标识符。

[AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)标头文档为主要平台提供了计算值的示例，但客户端应用程序可以根据自己的业务逻辑和要求选择使用不同的方法。

#### 3.如何计算X-Device-Info标头的值？ {#headers-faq3}

>[!IMPORTANT]
>
> 如果客户端应用程序从REST API V1迁移到REST API V2，则客户端应用程序可以继续使用与之前相同的方法计算设备信息值。

[X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md)请求标头包含与实际流设备相关的客户端信息（设备、连接和应用程序）。

[X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md)标头文档为主要平台提供了计算值的示例，但客户端应用程序可以根据自己的业务逻辑和要求选择使用其他方法。

+++

### 其他常见问题解答 {#misc-faqs-general}

+++其他常见问题解答

#### 1.能否探索REST API V2请求和响应并测试API？ {#misc-faq1}

是的。

您可以通过我们的专用[Adobe Developer](https://developer.adobe.com/adobe-pass/)网站浏览REST API V2。 Adobe Developer网站提供对以下内容的无限制访问：

* [DCR API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/)
* [REST API V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/)

要与[REST API V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/)交互，必须包含[Authorization](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md)标头以及通过[DCR API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/)获得的`Bearer`访问令牌。

要使用[DCR API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/)，需要具有REST API V2范围的软件语句。 有关更多详细信息，请参阅[动态客户端注册(DCR)常见问题解答](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md)文档。

#### 2.我能否使用支持OpenAPI的API开发工具来探索REST API V2请求和响应？ {#misc-faq2}

是的。

您可以从[Adobe Developer](https://developer.adobe.com/adobe-pass/)网站下载[DCR API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/)和[REST API V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/)的OpenAPI规范文件。

要下载OpenAPI规范文件，请单击下载按钮以将以下文件保存到本地计算机：

* [DCR API JSON](https://developer.adobe.com/adobe-pass/dcrApi.json)
* [REST API V2 JSON](https://developer.adobe.com/adobe-pass/restApiV2.json)

然后，您可以将这些文件导入首选的API开发工具以探索REST API V2请求和响应并测试API。

#### 3.我是否仍可以使用托管在https://sp.auth-staging.adobe.com/apitest/api.html上的现有API测试工具？ {#misc-faq3}

不适用。

迁移到REST API V2的客户端应用程序应使用托管在https://developer.adobe.com/adobe-pass/上的新测试工具。 Adobe Developer网站提供对以下内容的无限制访问：

* [DCR API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/)
* [REST API V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/)

要与[REST API V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/)交互，必须包含[Authorization](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md)标头以及通过[DCR API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/)获得的`Bearer`访问令牌。

要使用[DCR API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/)，需要具有REST API V2范围的软件语句。 有关更多详细信息，请参阅[动态客户端注册(DCR)常见问题解答](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md)文档。

+++

## 迁移常见问题解答 {#migration-faqs}

如果您使用的应用程序需要将现有应用程序迁移到REST API V2，请继续阅读本节内容。

>[!MORELIKETHIS]
>
> * [动态客户端注册(DCR)常见问题解答](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md#migration-faqs)

### 一般迁移常见问题解答 {#general-migration-faqs}

+++一般迁移常见问题解答

#### 1.是否需要一次性向所有用户推出迁移到REST API V2的新客户端应用程序？ {#migration-faq1}

不适用。

客户端应用程序不需要同时向所有用户推出集成REST API V2的新版本。

到2025年底，Adobe Pass身份验证将继续支持集成REST API V1或SDK的较旧客户端应用程序版本。

#### 2.是否需要一次性跨所有API和流推出迁移到REST API V2的新客户端应用程序？ {#migration-faq2}

是的。

需要客户端应用程序同时跨所有Adobe Pass身份验证API和流推出集成REST API V2的新版本。

在“第二屏幕身份验证”流程中，客户端应用程序需要同时为[主](rest-api-v2-glossary.md#primary-application)和[次](rest-api-v2-glossary.md#secondary-application)应用程序推出集成REST API V2的新版本。

Adobe Pass身份验证将不支持在API和流之间集成REST API V2和REST API V1/SDK的“混合”实施。

#### 3.在更新到迁移到REST API V2的新客户端应用程序时，是否将保留用户身份验证？ {#migration-faq3}

不适用。

在集成REST API V1或SDK的旧客户端应用程序版本中获取的用户身份验证将不会保留。

因此，用户需要在迁移到REST API V2的新客户端应用程序中重新进行身份验证。

#### 4. REST API V2中是否默认启用了增强错误代码？ {#migration-faq4}

是的。

默认情况下，迁移到REST API V2的客户端应用程序将自动受益于此功能，从而提供更详细和准确的错误信息。

有关更多详细信息，请参阅[增强型错误代码](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2)文档。

#### 5.迁移到REST API V2时，现有集成是否需要更改配置？ {#migration-faq5}

不适用。

迁移到REST API V2的客户端应用程序无需对现有MVPD集成进行任何配置更改。 此外，它们将继续对现有[服务提供商](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#service-provider)和[MVPD](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#mvpd)使用相同的标识符。

此外，Adobe Pass身份验证用于与MVPD端点通信的协议保持不变。

+++

### 从REST API V1迁移到REST API V2 {#migration-rest-api-v1-to-rest-api-v2-faqs}

如果您使用的应用程序需要从REST API V1迁移到REST API V2，请继续使用此子部分。

#### 注册阶段常见问题解答 {#registration-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++注册阶段常见问题解答

##### 1.注册阶段需要哪些高级API迁移？ {#registration-phase-v1-to-v2-faq1}

在从REST API V1迁移到REST API V2时，注册阶段没有重大变化。

客户端应用程序可以继续使用相同的流程，通过[动态客户端注册(DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#dcr)进程针对Adobe Pass身份验证进行注册并获取访问令牌。

有关更多信息，请参阅以下文档：

* [Dynamic Client注册概述](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
* [检索客户端凭据API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [检索访问令牌API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* [动态客户端注册流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

+++

#### 配置阶段常见问题解答 {#configuration-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++配置阶段常见问题解答

##### 1.配置阶段需要哪些高级API迁移？ {#configuration-phase-v1-to-v2-faq1}

在从REST API V1迁移到REST API V2的过程中，下表显示了需要考虑的高级别更改：

| 范围 | REST API V1 | REST API V2 | 观察结果 |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 检索具有活动集成的MVPD列表 | [GET <br/> /api/v1/config/{serviceProvider}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

#### 身份验证阶段常见问题解答 {#authentication-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++身份验证阶段常见问题解答

##### 1.身份验证阶段需要哪些高级API迁移？ {#authentication-phase-v1-to-v2-faq1}

在从REST API V1迁移到REST API V2的过程中，下表显示了需要考虑的高级别更改：

| 范围 | REST API V1 | REST API V2 | 观察结果 |
|-------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 检索注册码（身份验证码） | [帖子<br/> /reggie/v1/{serviceProvider}/regcode](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) | [发布<br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[在辅助应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 检查注册码（身份验证码） | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在辅助应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 在第二个屏幕上恢复(MVPD)身份验证（应用程序） | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [发布<br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在辅助应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 启动(MVPD)身份验证 | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[在辅助应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 检查用户身份验证状态 | [GET <br/> /api/v1/checkauthn （第一个屏幕）](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md) <br/> [GET <br/> /api/v1/checkauthn（第二个屏幕）](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-flow-by-second-screen-web-app.md) | 使用以下选项之一： <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 客户端应用程序可以同时出于多个目的使用这些API的响应： <br/> <ul><li>检查用户身份验证状态</li><li>检索用户配置文件</li><li>检索用户元数据信息</li></ul> <br/>有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在辅助应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| 检索用户身份验证令牌（配置文件） | [GET <br/> /api/v1/tokens/authn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) | 使用以下选项之一： <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 客户端应用程序可以同时出于多个目的使用这些API的响应： <br/> <ul><li>检查用户身份验证状态</li><li>检索用户配置文件</li><li>检索用户元数据信息</li></ul> <br/>有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在辅助应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| 检索用户元数据信息 | [GET <br/> /api/v1/tokens/usermetadata](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md) | 使用以下选项之一： <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 客户端应用程序可以同时出于多个目的使用这些API的响应： <br/> <ul><li>检查用户身份验证状态</li><li>检索用户配置文件</li><li>检索用户元数据信息</li></ul> <br/>有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在辅助应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

#### 预授权阶段常见问题解答 {#preauthorization-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++预授权阶段常见问题解答

##### 1.预授权阶段需要什么高级API迁移？ {#preauthorization-phase-v1-to-v2-faq1}

在从REST API V1迁移到REST API V2的过程中，下表显示了需要考虑的高级别更改：

| 范围 | REST API V1 | REST API V2 | 观察结果 |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 检索预授权的资源（预授权决策） | [GET <br/> /api/v1/preauthorize （第一个屏幕）](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources.md) <br/> [GET <br/> /api/v1/preauthorize（第二个屏幕）](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md) | [发布<br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行了基本预授权流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

#### 授权阶段常见问题解答 {#authorization-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++授权阶段常见问题解答

##### 1.授权阶段需要哪些高级API迁移？ {#authorization-phase-v1-to-v2-faq1}

在从REST API V1迁移到REST API V2的过程中，下表显示了需要考虑的高级别更改：

| 范围 | REST API V1 | REST API V2 | 观察结果 |
|-------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 启动(MVPD)授权 | [GET <br/> /api/v1/authorize](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authorization.md) | [发布<br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | 客户端应用程序可以同时将此API的响应用于多个目的： <br/> <ul><li>启动(MVPD)授权</li><li>检索授权决策</li><li>检索短媒体令牌</li></ul> <br/>有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本授权流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| 检索授权令牌（授权决策） | [GET <br/> /api/v1/tokens/authz](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authorization-token.md) | [发布<br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | 客户端应用程序可以同时将此API的响应用于多个目的： <br/> <ul><li>启动(MVPD)授权</li><li>检索授权决策</li><li>检索短媒体令牌</li></ul> <br/>有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本授权流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| 检索短授权令牌（媒体令牌） | [GET <br/> /api/v1/tokens/media](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md) | [发布<br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | 客户端应用程序可以同时将此API的响应用于多个目的： <br/> <ul><li>启动(MVPD)授权</li><li>检索授权决策</li><li>检索短媒体令牌</li></ul> <br/>有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本授权流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

#### 注销阶段常见问题解答 {#logout-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++注销阶段常见问题解答

##### 1.注销阶段需要哪些高级API迁移？ {#logout-phase-v1-to-v2-faq1}

在从REST API V1迁移到REST API V2的过程中，下表显示了需要考虑的高级别更改：

| 范围 | REST API V1 | REST API V2 | 观察结果 |
|-----------------|---------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 启动注销 | [GET <br/> /api/v1/logout](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本注销流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++

### 从SDK迁移到REST API V2 {#migration-sdk-to-rest-api-v2}

如果您使用的应用程序需要从SDK迁移到REST API V2，请继续使用此子部分。

#### 注册阶段常见问题解答 {#registration-phase-faqs-migration-sdk-to-rest-api-v2}

+++注册阶段常见问题解答

##### 1.注册阶段需要哪些高级API迁移？ {#registration-phase-sdk-to-v2-faq1}

在从SDK迁移到REST API V2的过程中，需要考虑一些高级别的更改，这些更改如下表所示：

###### AccessEnabler JavaScript SDK

| 范围 | SDK | REST API V2 | 观察结果 |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 完成动态客户端注册(DCR) | 向构造函数提供软件语句 | [发布<br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[动态客户端注册概述](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[动态客户端注册流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

###### AccessEnabler iOS/tvOS SDK

| 范围 | SDK | REST API V2 | 观察结果 |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 完成动态客户端注册(DCR) | 向构造函数提供软件语句 | [发布<br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[动态客户端注册概述](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[动态客户端注册流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| 范围 | SDK | REST API V2 | 观察结果 |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 完成动态客户端注册(DCR) | 向构造函数提供软件语句 | [发布<br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[动态客户端注册概述](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[动态客户端注册流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

###### AccessEnabler FireOS SDK

| 范围 | SDK | REST API V2 | 观察结果 |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 完成动态客户端注册(DCR) | 向构造函数提供软件语句 | [发布<br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[动态客户端注册概述](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[动态客户端注册流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

+++

#### 配置阶段常见问题解答 {#configuration-phase-faqs-migration-sdk-to-rest-api-v2}

+++配置阶段常见问题解答

##### 1.配置阶段需要哪些高级API迁移？ {#configuration-phase-sdk-to-v2-faq1}

在从SDK迁移到REST API V2的过程中，需要考虑一些高级别的更改，这些更改如下表所示：

###### AccessEnabler JavaScript SDK

| 范围 | SDK | REST API V2 | 观察结果 |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 检索具有活动集成的MVPD列表 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

###### AccessEnabler iOS/tvOS SDK

| 范围 | SDK | REST API V2 | 观察结果 |
|------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 检索具有活动集成的MVPD列表 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

###### AccessEnabler Android SDK

| 范围 | SDK | REST API V2 | 观察结果 |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 检索具有活动集成的MVPD列表 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

###### AccessEnabler FireOS SDK

| 范围 | SDK | REST API V2 | 观察结果 |
|------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 检索具有活动集成的MVPD列表 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

#### 身份验证阶段常见问题解答 {#authentication-phase-faqs-migration-sdk-to-rest-api-v2}

+++身份验证阶段常见问题解答

##### 1.身份验证阶段需要哪些高级API迁移？ {#authentication-phase-sdk-to-v2-faq1}

在从SDK迁移到REST API V2的过程中，需要考虑一些高级别的更改，这些更改如下表所示：

###### AccessEnabler JavaScript SDK

| 范围 | SDK | REST API V2 | 观察结果 |
|------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 启动(MVPD)身份验证 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#setSelProv) | [发布<br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[在辅助应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 检查用户身份验证状态 | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkAuthN) | 使用以下选项之一： <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 客户端应用程序可以同时出于多个目的使用这些API的响应： <br/> <ul><li>检查用户身份验证状态</li><li>检索用户配置文件</li><li>检索用户元数据信息</li></ul> <br/>有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在辅助应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| 检索用户元数据信息 | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getMeta) | 使用以下选项之一： <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 客户端应用程序可以同时出于多个目的使用这些API的响应： <br/> <ul><li>检查用户身份验证状态</li><li>检索用户配置文件</li><li>检索用户元数据信息</li></ul> <br/>有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在辅助应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler iOS SDK

| 范围 | SDK | REST API V2 | 观察结果 |
|------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 启动(MVPD)身份验证 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [发布<br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[在辅助应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 检查用户身份验证状态 | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthN) | 使用以下选项之一： <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 客户端应用程序可以同时出于多个目的使用这些API的响应： <br/> <ul><li>检查用户身份验证状态</li><li>检索用户配置文件</li><li>检索用户元数据信息</li></ul> <br/>有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在辅助应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| 检索用户元数据信息 | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getMeta) | 使用以下选项之一： <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 客户端应用程序可以同时出于多个目的使用这些API的响应： <br/> <ul><li>检查用户身份验证状态</li><li>检索用户配置文件</li><li>检索用户元数据信息</li></ul> <br/>有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在辅助应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler tvOS SDK

| 范围 | SDK | REST API V2 | 观察结果 |
|-------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 检索注册码（身份验证码） | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [发布<br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[在辅助应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 检查注册码（身份验证码） | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在辅助应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 在第二个屏幕上恢复(MVPD)身份验证（应用程序） | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [发布<br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在辅助应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 启动(MVPD)身份验证 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [发布<br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[在辅助应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 检查用户身份验证状态 | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthN) | 使用以下选项之一： <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 客户端应用程序可以同时出于多个目的使用这些API的响应： <br/> <ul><li>检查用户身份验证状态</li><li>检索用户配置文件</li><li>检索用户元数据信息</li></ul> <br/>有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在辅助应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| 检索用户元数据信息 | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getMeta) | 使用以下选项之一： <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 客户端应用程序可以同时出于多个目的使用这些API的响应： <br/> <ul><li>检查用户身份验证状态</li><li>检索用户配置文件</li><li>检索用户元数据信息</li></ul> <br/>有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在辅助应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| 范围 | SDK | REST API V2 | 观察结果 |
|------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 启动(MVPD)身份验证 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#setSelectedProvider) | [发布<br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[在辅助应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 检查用户身份验证状态 | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkAuthN) | 使用以下选项之一： <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 客户端应用程序可以同时出于多个目的使用这些API的响应： <br/> <ul><li>检查用户身份验证状态</li><li>检索用户配置文件</li><li>检索用户元数据信息</li></ul> <br/>有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在辅助应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| 检索用户元数据信息 | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getMetadata) | 使用以下选项之一： <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 客户端应用程序可以同时出于多个目的使用这些API的响应： <br/> <ul><li>检查用户身份验证状态</li><li>检索用户配置文件</li><li>检索用户元数据信息</li></ul> <br/>有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在辅助应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler FireOS SDK

| 范围 | SDK | REST API V2 | 观察结果 |
|-------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 检索注册码（身份验证码） | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [发布<br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[在辅助应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 检查注册码（身份验证码） | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在辅助应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 在第二个屏幕上恢复(MVPD)身份验证（应用程序） | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [发布<br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在辅助应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 启动(MVPD)身份验证 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [发布<br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[在辅助应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 检查用户身份验证状态 | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkAuthN) | 使用以下选项之一： <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 客户端应用程序可以同时出于多个目的使用这些API的响应： <br/> <ul><li>检查用户身份验证状态</li><li>检索用户配置文件</li><li>检索用户元数据信息</li></ul> <br/>有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在辅助应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| 检索用户元数据信息 | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getMetadata) | 使用以下选项之一： <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 客户端应用程序可以同时出于多个目的使用这些API的响应： <br/> <ul><li>检查用户身份验证状态</li><li>检索用户配置文件</li><li>检索用户元数据信息</li></ul> <br/>有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在辅助应用程序中执行的基本配置文件流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

#### 预授权阶段常见问题解答 {#preauthorization-phase-faqs-migration-sdk-to-rest-api-v2}

+++预授权阶段常见问题解答

##### 1.预授权阶段需要什么高级API迁移？ {#preauthorization-phase-sdk-to-v2-faq1}

在从SDK迁移到REST API V2的过程中，需要考虑一些高级别的更改，这些更改如下表所示：

###### AccessEnabler JavaScript SDK

| 范围 | SDK | REST API V2 | 观察结果 |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 检索预授权的资源（预授权决策） | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkPreauthRes) <br/> [AccessEnabler.preauthorize](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/preauthorize-api-javascript-sdk.md) | [发布<br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行了基本预授权流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler iOS/tvOS SDK

| 范围 | SDK | REST API V2 | 观察结果 |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 检索预授权的资源（预授权决策） | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkPreauth) <br/> [AccessEnabler.preauthorize](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/preauthorize-api-ios-tvos-sdk.md) | [发布<br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行了基本预授权流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| 范围 | SDK | REST API V2 | 观察结果 |
|---------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 检索预授权的资源（预授权决策） | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkPreauth) <br/> [AccessEnabler.preauthorize](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/preauthorize-api-android-sdk.md) | [发布<br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行了基本预授权流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

| 范围 | SDK | REST API V2 | 观察结果 |
|---------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 检索预授权的资源（预授权决策） | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkPreauth) | [发布<br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行了基本预授权流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

#### 授权阶段常见问题解答 {#authorization-phase-faqs-migration-sdk-to-rest-api-v2}

+++授权阶段常见问题解答

##### 1.授权阶段需要哪些高级API迁移？ {#authorization-phase-sdk-to-v2-faq1}

在从SDK迁移到REST API V2的过程中，需要考虑一些高级别的更改，这些更改如下表所示：

###### AccessEnabler JavaScript SDK

| 范围 | SDK | REST API V2 | 观察结果 |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 检索短授权令牌（媒体令牌） | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthZ) | [发布<br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | 客户端应用程序可以同时将此API的响应用于多个目的： <br/> <ul><li>启动(MVPD)授权</li><li>检索授权决策</li><li>检索短媒体令牌</li></ul> <br/>有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本授权流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler iOS/tvOS SDK

| 范围 | SDK | REST API V2 | 观察结果 |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 检索短授权令牌（媒体令牌） | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthZ) | [发布<br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | 客户端应用程序可以同时将此API的响应用于多个目的： <br/> <ul><li>启动(MVPD)授权</li><li>检索授权决策</li><li>检索短媒体令牌</li></ul> <br/>有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本授权流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| 范围 | SDK | REST API V2 | 观察结果 |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 检索短授权令牌（媒体令牌） | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthZ) | [发布<br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | 客户端应用程序可以同时将此API的响应用于多个目的： <br/> <ul><li>启动(MVPD)授权</li><li>检索授权决策</li><li>检索短媒体令牌</li></ul> <br/>有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本授权流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler FireOS SDK

| 范围 | SDK | REST API V2 | 观察结果 |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 检索短授权令牌（媒体令牌） | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthZ) | [发布<br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | 客户端应用程序可以同时将此API的响应用于多个目的： <br/> <ul><li>启动(MVPD)授权</li><li>检索授权决策</li><li>检索短媒体令牌</li></ul> <br/>有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本授权流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

#### 注销阶段常见问题解答 {#logout-phase-faqs-migration-sdk-to-rest-api-v2}

+++注销阶段常见问题解答

##### 1.注销阶段需要哪些高级API迁移？ {#logout-phase-sdk-to-v2-faq1}

在从SDK迁移到REST API V2的过程中，需要考虑一些高级别的更改，这些更改如下表所示：

###### AccessEnabler JavaScript SDK

| 范围 | SDK | REST API V2 | 观察结果 |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 启动注销 | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本注销流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

###### AccessEnabler iOS/tvOS SDK

| 范围 | SDK | REST API V2 | 观察结果 |
|-----------------|-----------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 启动注销 | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本注销流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| 范围 | SDK | REST API V2 | 观察结果 |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 启动注销 | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本注销流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

###### AccessEnabler FireOS SDK

| 范围 | SDK | REST API V2 | 观察结果 |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 启动注销 | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | 有关更多详细信息，请参阅以下文档： <br/> <ul><li>[在主应用程序中执行的基本注销流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++
