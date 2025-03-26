---
title: 程序员集成指南
description: 程序员集成指南
exl-id: 51461caf-08ef-459e-b284-8f317f45e7b1
source-git-commit: 07bb12f7983f39b58e1b9795fdaa1bec4f68e674
workflow-type: tm+mt
source-wordcount: '2119'
ht-degree: 0%

---

# 程序员集成指南 {#programmer-integration-guide}

>[!IMPORTANT]
>
> 此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

本集成指南适用于计划与Adobe® Pass身份验证集成的内容提供商（程序员）。

在当今的数字环境中，查看者可以随时随地访问Internet，并请求访问您的受保护内容。 他们可能希望观看一次性的活动，或者希望获得播放您正在播放的整个电视剧的权利。

在授予对受保护内容的访问权限之前，您必须确定查看者是否有权使用它。 关键问题包括：

* **查看器是否具有多频道视频节目分发服务器(MVPD)的活动订阅？**
* **该订阅是否包含您的编程？**

## 适用于所有电视的Adobe Pass身份验证 {#adobe-pass-authentication-for-tv-everywhere}

对于程序员而言，确定权利并不总是简单明了。 MVPD是其客户标识数据和访问权限的托管者。 让问题进一步复杂化的是，程序员可能会订阅各种MVPD，每个都使用独特的系统进行操作。 这些复杂性使得验证权利在技术上既具有挑战性，又具有资源密集性。

![由程序员直接决定的用户权利](../assets/user-ent-by-progr.png){align="center"}

*由程序员直接决定的用户权利*

Adobe Pass身份验证可安全地促进程序员和MVPD之间的授权事务，使得向符合条件的查看者提供受保护内容变得快速、轻松和安全。

由Adobe Pass身份验证调解的![用户权利](../assets/user-ent-mediatedby-authn.png){align="center"}

由Adobe Pass身份验证调解的&#x200B;*用户权利*

Adobe Pass Authentication充当代理，通过为双方提供安全一致的界面，促进程序员和MVPD之间的权利流。

对于程序员，Adobe Pass身份验证将API作为&#x200B;**Standard**&#x200B;或&#x200B;**Premium**&#x200B;层的一部分提供：

* 标准Adobe Pass身份验证API：
   * [REST API DCR](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
   * [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)

* Premium Adobe Pass身份验证API：
   * [重置临时传递API](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#reset-tempass-api-access)
      * [TempPass功能](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md)
   * [降级API](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md#degradation-api-access)
      * [退化特征](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md)
   * [授权服务监控API](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-api.md)

### 用例 {#use-cases}

本节进一步概述了Adobe Pass身份验证支持的程序员集成用例：

* 具有单通道网络的程序员(TVE)应用程序

  这使程序员能够让查看者从TVE应用程序内的单品牌渠道网络访问内容。

* 具有多信道网络的程序员(TVE)应用程序

  这使程序员能够在单个TVE应用程序中向查看者提供从多个渠道网络访问内容的权限。

* 用于特殊事件的程序员(TVE)应用程序

  这可以让程序员向查看者提供对特殊事件内容的访问权限，这些特殊事件可能不是MVPD权利数据库（如普通渠道）中的资源。

| **阶段** | **优先级** | **用例** | **文档** |
|----------------------|--------------|-------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **身份验证** | **高** | 身份验证 | 有关更多详细信息，请参阅[身份验证阶段](#authentication-phase)部分下汇总的文档。 |
|                      | **高** | 基于家庭的身份验证(HBA) | 有关更多详细信息，请参阅[基于主页的身份验证](/help/authentication/integration-guide-programmers/features-standard/hba-access/home-based-authentication.md)。 |
|                      | **高** | 单点登录(SSO) | 有关更多详细信息，请参阅[单点登录(SSO)](#sso)部分下汇总的文档。 |
|                      | **高** | 选择MVPD | 有关更多详细信息，请参阅[配置阶段](#configuration-phase)部分下汇总的文档。 |
|                      | **Medium** | 品牌化MVPD登录页面 | 使MVPD能够为登录页提供特定于程序员或服务提供商的品牌，包括对默认语言首选项的支持。 |
|                      | **高** | 为每个平台配置生存时间(TTL)值 | 有关详细信息，请参阅[TVE仪表板集成用户指南](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows)。 |
| **预授权** | **低** | 预授权（预检授权） | 有关更多详细信息，请参阅[预授权阶段](#preauthorization-phase)部分下汇总的文档。 |
|                      | **Medium** | 增强的错误代码 | 有关更多详细信息，请参阅[增强型错误代码](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)。 |
| **授权** | **高** | 授权 | 有关更多详细信息，请参阅[授权阶段](#authorization-phase)部分下汇总的文档。 |
|                      | **高** | 不同渠道授权 | 使用户能够在一个TVE应用程序中从多个渠道网络访问内容。 程序员可以进行特定于渠道的授权调用以验证权限。 |
|                      | **低** | 资产级授权 | 使MVPD能够在授权期间收集单个内容资产的详细分析。 |
|                      | **Medium** | 增强的错误代码 | 有关更多详细信息，请参阅[增强型错误代码](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)。 |
|                      | **高** | 程序员Federated Player — 具有页面级授权 | 有关详细信息，请参阅[媒体令牌](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md)。 |
|                      | **Medium** | 程序员联合播放器 — 具有内部播放器授权 | 有关详细信息，请参阅[媒体令牌](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md)。 |
|                      | **高** | 联合播放器 — 托管在MVPD Portal上，具有页面级授权 | 有关详细信息，请参阅[媒体令牌](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md)。 |
|                      | **低** | 家长控制 — 授权请求中的内容评级 | 使程序员能够在MVPD的授权请求中包含对资产级授权有用的内容评级。 |
|                      | **低** | 家长控制 — 基于用户属性的内容过滤 | 使程序员能够检查用户允许的最大内容评级，并相应地筛选可用内容。 |
| **注销** | **Medium** | 注销 | 有关更多详细信息，请参阅[注销阶段](#logout-phase)部分下汇总的文档。 |

## 权利流 {#entitlement-flow}

权利流是程序员(TVE)应用程序必须完成的一系列步骤，才能流式传输受保护的内容。 该流包含以下阶段：

* [注册阶段](#registration-phase)
* [配置阶段](#configuration-phase)
* [身份验证阶段](#authentication-phase)
* [（可选）预授权阶段](#preauthorization-phase)
* [授权阶段](#authorization-phase)
* [注销阶段](#logout-phase)

在用户初次访问程序员(TVE)应用程序时，权利流程将遵循所述的顺序。 但是，在后续访问中，应用程序可能会根据注册或身份验证的状态以及适用的查看策略来绕过某些步骤。

有关权利流及其阶段的详细探讨，请继续阅读本文档，并在参阅随附的指南指南指南指南以获取更多见解：

* [REST API V2指南（客户端到服务器）](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/cookbooks/rest-api-v2-cookbook-client-server.md)
* [REST API V2指南（服务器到服务器）](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/cookbooks/rest-api-v2-cookbook-server-server.md)

>[!NOTE]
>
> 本文档中使用程序员(TVE)应用程序来统一指代在Adobe Pass身份验证支持的不同平台（浏览器、移动设备、TV连接设备等）上运行的应用程序类型。

### 注册阶段 {#registration-phase}

注册阶段的目的是通过[动态客户端注册(DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)进程针对Adobe Pass身份验证注册客户端应用程序。

动态客户端注册(DCR)过程要求客户端应用程序获取一对客户端凭据，并检索访问令牌作为注册阶段的最终目标。

**API**

* [检索客户端凭据](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [检索访问令牌](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)

**流**

* [动态客户端注册流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

**常见问题解答**

* [注册阶段常见问题解答](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#registration-phase-faqs-general)。

### 配置阶段 {#configuration-phase}

配置阶段的目的是向客户端应用程序提供与其主动集成的MVPD列表，以及Adobe Pass身份验证为每个MVPD保存的配置详细信息。

当客户端应用程序需要要求用户选择其电视提供商时，配置阶段是身份验证阶段的先决条件步骤。

**API**

* [检索特定服务提供商的配置](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)

**常见问题解答**

* [配置阶段常见问题解答](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#configuration-phase-faqs-general)。

>[!TIP]
>
> TVE应用程序应包括一个MVPD选择界面，使用户能够轻松识别和选择其电视提供商。

### 身份验证阶段 {#authentication-phase}

身份验证阶段的目的是为客户端应用程序提供使用MVPD验证用户身份以及获取用户元数据信息的功能。

当客户端应用程序需要播放内容时，身份验证阶段是预授权阶段或授权阶段的先决步骤。

成功的身份验证会生成一个绑定到应用程序、设备和服务提供商的配置文件，其中还包含用户元数据信息。

**高级步骤**

以下步骤概述了实施SAML集成时的高级步骤：

1. **程序员应用程序（网站）加载**\
   用户导航到程序员的应用程序（网站），该应用程序集成了Adobe Pass身份验证[REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)。

1. **受保护的内容请求**\
   当用户尝试访问受保护的内容时，程序员的应用程序会显示一个可供用户选择的MVPD列表。

1. **身份验证请求初始化**\
   选择MVPD后，用户将被重定向到Adobe Pass身份验证服务器。 如果是SAML集成，将在此处生成选定MVPD的加密SAML身份验证请求。 此请求将代表程序员发送到MVPD。 根据MVPD系统的不同，用户的浏览器会被重定向到MVPD的登录页面，或者登录iFrame会嵌入到程序员的应用程序中。

1. **MVPD登录**\
   MVPD接受请求并通过重定向或iFrame显示其登录界面。

1. **用户登录和验证**\
   用户使用其MVPD凭据登录。 MVPD会验证用户的订阅状态并建立其自己的HTTP会话。

1. **MVPD对Adobe Pass身份验证的响应**\
   验证完成后，MVPD会生成一个SAML响应（已加密），并将其发送回Adobe Pass身份验证。

1. **配置文件生成**\
   Adobe Pass身份验证验证SAML响应，生成缓存的用户配置文件，并将用户重定向回程序员的应用程序（网站）。

**API**

* [创建身份验证会话](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [恢复身份验证会话](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [检索身份验证会话](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)
* [在用户代理中执行身份验证](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)
* [检索用户档案](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [检索特定mvpd的配置文件](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [检索特定代码的配置文件](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

**流**

* [在主应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [在辅助应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
* [基本配置文件在主要应用程序中执行](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [基本配置文件在辅助应用程序中执行](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

**常见问题解答**

* [身份验证阶段常见问题解答](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)。

>[!TIP]
>
> TVE应用程序应清楚地传达用户的身份验证状态，例如，在“锁定”或“解锁”图标旁显示用户的MVPD徽标，以指示受保护内容的可访问性。

#### 单点登录(SSO) {#single-sign-on}

**API**

* [检索合作伙伴身份验证请求](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md)
* [使用合作伙伴身份验证响应创建和检索配置文件](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md)

**流**

* [使用合作伙伴流程进行单点登录](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md)
* [使用平台身份流进行单点登录](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md)
* [使用服务令牌流进行单点登录](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md)

### （可选）预授权阶段 {#preauthorization-phase}

预授权阶段的目的是让客户端应用程序能够呈现用户有权访问的目录中的资源子集。

当用户首次打开客户端应用程序或导航到新部分时，预授权阶段可以增强用户体验。

**API**

* [检索预授权决策](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)

**流**

* [在主应用程序中执行的基本预授权流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

**常见问题解答**

* [预授权阶段常见问题解答](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#preauthorization-phase-faqs-general)。

>[!TIP]
>
> TVE应用程序应通过使用可视指示器（例如，受限内容的“已锁定”图标和授权内容的“已解锁”图标），明确区分受限内容和授权内容。

### 授权阶段 {#authorization-phase}

授权阶段的目的是让客户端应用程序能够在使用MVPD验证用户请求的资源后，播放这些资源。

成功的授权会生成一个决策，其中还包含为程序员(TVE)应用程序提供的用于安全目的的媒体令牌。

**高级步骤**

以下步骤概述了高级步骤：

1. **资源标识符处理**\
   受保护的内容由[资源标识符](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#resource-identifier)标识，该标识符可以是简单字符串或更复杂的结构。 该标识符由程序员和MVPD预定义并商定。 程序员的应用程序将资源标识符发送到Adobe Pass身份验证[REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)。

1. **MVPD授权检查**\
   Adobe Pass身份验证服务器使用标准化协议与MVPD的授权端点进行通信。

1. **MVPD对Adobe Pass身份验证的响应**\
   验证完成后，MVPD会确认用户是否有权访问内容，并将响应发送回Adobe Pass身份验证。

1. **决策和媒体令牌生成**\
   Adobe Pass身份验证验证响应，生成缓存的[决策](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md)，并将包含媒体令牌的决策返回给程序员的应用程序（网站）。

1. **内容访问验证**\
   程序员的应用程序使用[媒体令牌验证器](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier)来确认正确的用户正在访问正确的内容。 验证后，授予用户查看受保护内容的权限。

**API**

* [检索授权决策](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

**流**

* [在主应用程序中执行的基本授权流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

**常见问题解答**

* [授权阶段常见问题解答](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authorization-phase-faqs-general)。

>[!TIP]
>
> TVE应用程序应通过使用可视指示器（例如，受限内容的“已锁定”图标和授权内容的“已解锁”图标），明确区分受限内容和授权内容。

### 注销阶段 {#logout-phase}

注销阶段的目的是让客户端应用程序能够根据用户请求在Adobe Pass身份验证中终止用户的已验证配置文件。

**API**

* [启动特定mvpd的注销](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)

**流**

* [在主应用程序中执行的基本注销流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)

**常见问题解答**

* [注销阶段常见问题解答](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#logout-phase-faqs-general)。

#### 单次注销(SLO) {#single-logout}

**流**

* [单个注销流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-logout-flow.md)

## 了解权利 {#understanding-entitlements}

Adobe Pass身份验证解决方案围绕着权限创建来考虑的，权限是在成功完成身份验证和授权工作流后生成的特定数据段。 这些权限授予对受保护内容的访问权限，但具有有限的生命周期。 权利到期后，必须通过重新启动身份验证或授权过程来续订权利。

有关授权的更多详细信息，请参阅以下文档：

* **配置文件**

  在成功进行身份验证后，Adobe Pass身份验证会创建一个已身份验证配置文件（“长期”），该配置文件与请求应用程序、设备和服务提供商标识符（请求者标识符）相关联。

* **[用户元数据](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)**

  在成功进行身份验证后（在某些情况下，在身份验证之后也是如此），Adobe Pass身份验证将从MVPD接收用户元数据，这些元数据可能会向请求应用程序公开这些元数据。

* **[决策](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md)**

  成功授权后，Adobe Pass身份验证会创建一个与请求应用程序、设备、服务提供商标识符（请求者标识符）和特定受保护资源（资源标识符）关联的授权决策（“长期”）。

* **[媒体令牌](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md)**

  成功授权后，Adobe Pass身份验证会创建一个与成功的播放请求关联的媒体令牌（“短暂的”），并为减少欺诈（例如，流翻录）的行业最佳实践提供支持。

用户档案和决策的生存时间(“TTL”)值是根据程序员和付费电视提供商之间的协议设置的，这些提供商就最适合所有相关人员的价值达成了共识。
