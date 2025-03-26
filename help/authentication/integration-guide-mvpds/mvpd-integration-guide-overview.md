---
title: MVPD集成指南
description: MVPD集成指南
exl-id: b918550b-96a8-4e80-af28-0a2f63a02396
source-git-commit: 07bb12f7983f39b58e1b9795fdaa1bec4f68e674
workflow-type: tm+mt
source-wordcount: '1307'
ht-degree: 0%

---

# MVPD集成指南 {#mvpd-integration-guide}

>[!IMPORTANT]
>
> 此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

本集成指南适用于计划与Adobe® Pass Authentication集成的多渠道视频编程分发商(MVPD)。

TV Everywhere (TVE)是付费电视行业中的一项变革性举措，它使订阅者能够跨多种设备（无论是在家还是在外出）访问他们已付费的内容。 对付费电视提供商而言，TVE提供了重大机遇，其中加强了现有客户关系，并打开了通往新客户关系的大门。 然而，这些机遇伴随着挑战。

在TVE生态系统中，**程序员**&#x200B;提供内容，而&#x200B;**MVPD** （多频道视频节目分发商）管理验证查看者是否为合格订阅者所需的客户数据。 虽然与单个程序员协调身份验证和授权可能易于管理，但与数十甚至数百个程序员一起执行此操作会引入相当大的复杂性。

这是&#x200B;**Adobe® Pass Authentication**&#x200B;简化流程的地方。 MVPD只需与Adobe Pass实施单一、简化的集成，即可访问整个TVE生态系统。 所提供的集成框架可加快上市时间，提供安全环境以减少欺诈，并通过跨多个平台提供更多电视内容来增强客户体验。

## 适用于所有电视的Adobe Pass身份验证 {#adobe-pass-authentication-for-tv-everywhere}

Adobe Pass Authentication是一种SaaS（软件即服务）解决方案，旨在实现快速后端（服务器到服务器）集成，同时遵守多频道视频节目分发商(MVPD)和节目制作商的业务规则。

### 标准和协议 {#standards-protocols}

Adobe Pass身份验证与TV Everywhere (TVE)的新兴标准和协议完全一致，支持整个生态系统的无缝集成和合规性。

* **CableLabs OLCA（在线内容访问）规范**\
  Adobe Pass身份验证遵循CableLabs OLCA规范，该规范定义了从在线源向付费电视客户交付视频内容的技术要求和架构。 Adobe在2011年6月积极参加了CableLabs互操作性测试项目，成功地通过了服务提供商实施的测试流程。

Adobe Pass身份验证旨在支持多种协议（例如SAML、OAuth 2.0等），这种灵活性允许根据不断变化的需求进行未来扩展，包括自定义协议。

大多数集成都使用SAML（安全断言标记语言）协议，该协议是身份验证的主要标准。 Adobe Pass身份验证在SAML框架中充当代理服务提供商，将SAML身份验证响应作为安全令牌保留在Adobe公共域中。

归根结底，Adobe Pass身份验证与协议无关，旨在与OLCA标准紧密配合。 这些标准为程序员、MVPD和服务提供商建立了通用框架，确保以一致的方法实施TVE功能。

### 集成和支持 {#integration-support}

Adobe Pass身份验证可与MVPD技术团队协作，根据他们的特定要求配置集成，如下所示：

* **标准集成**\
  免费提供，包括文档和基本电子邮件支持。

* **增强型支持**\
  对于重要的自定义或加快的时间表，可能需要支付支持费用。

Adobe Pass身份验证支持高效处理MVPD业务逻辑，如下所示：

* **自包含业务逻辑**\
  对于在授权请求期间由MVPD完全强制执行的业务逻辑，Adobe提供了必要的数据。 此数据可以包括但不限于发出请求的用户设备的唯一设备ID和IP地址。

* **自定义属性支持**\
  对于需要用户干预或Adobe特定处理的业务逻辑，Adobe可以维护每个MVPD的自定义配置。 这些策略支持可以在授权工作流中的特定点触发的预定义工作流。 有关详细信息，请联系您的Adobe代表。

## 权利流 {#entitlement-flow}

权利流是程序员(TVE)应用程序必须完成的一系列步骤，才能流式传输受保护的内容。 该流包括几个涉及与MVPD交互的阶段：

* [身份验证阶段](#authentication-phase)
* （可选）预授权阶段
* [授权阶段](#authorization-phase)
* 注销阶段

在用户初次访问程序员(TVE)应用程序时，权利流程将遵循所述的顺序。 但是，在后续访问中，应用程序可能会根据身份验证状态和适用的查看策略跳过某些步骤。

>[!NOTE]
>
> 本文档中使用程序员(TVE)应用程序来统一指代在Adobe Pass身份验证支持的不同平台（浏览器、移动设备、TV连接设备等）上运行的应用程序类型。

### 身份验证阶段 {#authentication-phase}

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

### 授权阶段 {#authorization-phase}

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
