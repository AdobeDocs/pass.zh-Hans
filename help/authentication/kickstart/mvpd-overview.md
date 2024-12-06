---
title: MVPD概述
description: MVPD概述
exl-id: b918550b-96a8-4e80-af28-0a2f63a02396
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '2734'
ht-degree: 0%

---

# MVPD概述 {#mvpd-overview}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## 简介 {#intro}

此概述适用于多渠道视频编程分发商(MVPD)。 有关其他文档，包括Kickstart和集成指南，请参阅本文档末尾的“相关信息”部分。



TV Everywhere (TVE)是如今广为人知的行业运动，它使付费电视订阅者能够跨多台设备访问他们已付费的内容，无论是在家中还是户外。  对于付费电视提供商，TVE创造了新的机会，既可以保持现有的客户关系，也可以启用新的客户关系。 然而，伴随着这些机遇，也带来了挑战。 在TVE环境中，程序员提供内容，但MVPD包含客户信息，用于验证潜在查看者是否为有效订阅者。



与一个程序员协调查看者身份验证和授权可能很简单，但与数十或数百个不同的程序员的协调变得越来越复杂。 但是，通过Adobe® Pass，MVPDs只需实施单一、简单的集成即可访问整个TVE生态系统，包括NBC环球媒体、Turner Broadcasting(TBS、TNT、CNN)、Fox Broadcast Networks、Hulu等程序员。  Adobe Pass身份验证提供了一个集成框架，可让您轻松安全地确定用户权限。



要点：Adobe Pass身份验证安全地协调程序员和MVPD之间的授权事务，便于查看者访问订阅内容。 换言之，Adobe Pass身份验证使适当的客户能够轻松快速地访问适当的内容。


通过Adobe Pass身份验证，MVPD接收：

与程序员轻松集成。  通过单个集成提供来自多个内容所有者的即时连接。

增强了客户参与度。  当客户跨多个平台和设备查看内容时，支持流畅的品牌体验。

安全身份验证。  确保仅授权的用户和设备被授予访问高级内容的权限，并（可选）限制每个家庭帐户可以连接的设备和并发流的数量。

## 常见问题解答 {#faq}

Adobe Pass身份验证的安全程度如何？ Adobe Pass身份验证架构的首要任务是确保仅授权查看者通过身份验证并被授予对高级内容的访问权限。 Adobe Pass身份验证将访问与观看设备紧密绑定，并帮助限制给定家庭的流、会话和/或设备。


是否需要Flash Player？ 适用于所有电视的Adobe Pass身份验证与播放器和平台无关，可与任何播放应用程序(包括Silverlight和HTML5)集成。 此外，Adobe Pass身份验证还为运行iOS和Android的设备（如手机和平板电脑）提供本机支持。


Adobe Pass身份验证支持哪些设备？ 几乎任何具有HTML5 Web工具包的设备都支持Adobe Pass身份验证，以便在浏览器中查看体验。 此外，Adobe Pass Authentication将继续为各种特定于设备的平台(包括iOS、Android™、Xbox360（已弃用）和AdobeAir®（已弃用）应用程序)推出本机软件开发工具包(SDK)。 最近，Adobe Pass身份验证推出了无客户端解决方案，适用于无法呈现浏览器页面的设备（例如，“智能”电视、机顶盒和游戏机）。  要使用MVPD对用户进行身份验证，需要能够呈现浏览器页面。


Adobe Pass身份验证是否支持世界各地电视的新兴标准？ Adobe Pass身份验证符合CableLabs OLCA（在线内容访问）规范，该规范为从在线来源向付费电视客户交付视频提供了技术要求和架构。 Adobe在2011年6月参与了CableLabs的联合互操作测试项目，并通过了服务提供商实施的测试流程。 Adobe Pass身份验证已根据OLCA身份验证规范进行验证（完成并测试）。 授权组件已完成，但测试验证当前等待CableLabs测试环境的发布。 Adobe也是开放式认证技术联合会的积极成员，作为该机构的一部分，参与了小组委员会的几个规格起草项目。



什么是身份验证？ 认证是MVPD确认给定用户是已知客户的过程。



什么是授权？ 授权是MVPD确认经过身份验证的用户具有对给定资源的有效预订的过程。



## 架构 {#architecture}

Adobe Pass身份验证是一项托管服务，允许根据MVPD和程序员所需的业务规则快速进行后端（服务器到服务器）集成。 这意味着，可以快速面向各方进行营销，提供更安全的环境以防止欺诈，并提供卓越的客户体验，让更多平台上的更多人能够看到更多电视内容。


Adobe Pass身份验证通过软件即服务(SaaS)模型提供，支持在最终用户、MVPD和程序员之间进行更安全的通信，以验证对内容的权限。 服务的核心组件包括：

服务器端 — 托管的Adobe Pass身份验证服务器。 这是一个应用程序服务器，它与MVPD的验证系统进行后端通道（服务器到服务器）通信。
客户端：
客户端访问启用程序 — 访问启用程序是加载到程序员的网页或播放器应用程序中的小文件。 它为程序员的内容查看应用程序提供授权API，并与Adobe Pass Authentication Server通信。
无客户端Web服务（用于不具有Web功能的设备） — RESTful Web服务，为智能电视、游戏控制台和机顶盒等设备提供授权API。

>[!NOTE]
>
>作为MVPD，您的Web服务必须能够识别来自Adobe Pass Authentication的身份验证和授权请求，并以预期格式响应所需数据。
>

Adobe Pass身份验证允许您为客户提供联合身份管理，也称为单点登录(SSO)身份验证和授权。 使用Adobe Pass身份验证时，只要MVPD允许保留该身份验证，订阅者就不需要在首次身份验证后再次登录。 （通常为30天。） 为实现此目标，Adobe Pass身份验证为客户提供了一个用于身份验证令牌的通用域。 此身份验证状态信息适用于与给定MVPD集成的所有参与站点。


目前，大多数Adobe Pass与MVPD的身份验证集成都使用SAML协议，该协议是主要身份验证标准之一。 Adobe Pass身份验证在SAML架构中充当代理服务提供商，并将SAML身份验证响应作为安全令牌保留在Adobe公共域中。 Adobe Pass身份验证符合SAML 2.0标准。 但是，虽然Adobe Pass身份验证目前通常与SAML SSO解决方案一起使用，但Adobe Pass身份验证架构未绑定到任何特定协议。 因此，可以逐渐增加对新协议的支持，例如基于OAuth 2.0或自定义协议的支持。


Adobe与MVPD的技术团队合作，配置Adobe Pass身份验证以满足任何现有集成的需求。 MVPD可免费集成，前提是“标准”集成且支持要求最低（文档和基本电子邮件支持）。 如果MVPD需要大量支持或升级的时间表，则可能需要收取支持费用，或者提供商可能需要与熟悉我们的解决方案的第三方（如Synacor）合作。


Adobe Pass身份验证还支持高效处理MVPD业务逻辑，如下所示：

对于自包含并且当接收到授权请求时可由MVPD应用的业务逻辑，当MVPD接收到授权请求时，Adobe提供支持业务逻辑实施所需的必要数据。 此数据可以包括但不限于发出请求的用户唯一设备ID和设备的IP地址。

对于需要Adobe干预和/或用户解决方案进行特定处理的业务逻辑，Adobe可以为每个MVPD维护一些自定义属性。 这些特定于MVPD的配置/策略包括支持预定义的工作流，这些工作流可在顶级工作流的某些特定点启动。 有关自定义资产支持的详细信息，请联系您的Adobe代表。

下图说明了MVPD和程序员与这些Adobe Pass身份验证组件的关系：

![](../assets/high-level-architecture-nflows.png)

*图：高级架构和流程*

## Adobe Pass身份验证组件 {#components}

下面概述了Adobe Pass Authentication生态系统的一些主要组件。 这些功能包括：

* [Access Enabler/无客户端Web服务](#ae)
* [Adobe承载的后端服务器](#backend)
* [令牌](#tokens)

### Access Enabler/无客户端Web服务 {#ae}

Access Enabler方便了与用户的所有身份验证和授权交互，并在其系统上本地运行。 它是Access Enabler，通过MVPD处理实际权利工作流，而程序员负责处理更高级别的网页或播放器应用程序。

无客户端Web服务由Adobe Pass身份验证为无法呈现网页的设备提供。  对于这些设备，授权过程将在智能设备上启动并查看内容，而使用MVPD的身份验证则在支持Web的设备（PC、智能手机和平板电脑）上进行。

访问启用码：

* 启动特定于MVPD的身份验证和授权工作流。
* 按程序员资源/渠道缓存成功的授权响应，以最大程度地减少不必要的请求流量。
* 可以为特定于每个MVPD的预定义工作流进行配置，例如显式设备注册。
* 它包括以下形式：
   * Flash Player运行时可以执行的SWF文件
   * 由浏览器直接执行的JS文件
   * 用于各种平台(包括iOS、Android和Xbox)的本机Access Enabler。

### Adobe承载的后端服务器 {#backend}

Adobe Pass身份验证后端服务器，由Adobe托管：

* 为身份验证和授权工作流设置需要在Adobe Pass身份验证和操作员之间进行服务器到服务器通信的MVPD。
* 维护程序员站点和应用程序的配置。
* 承载可下载的Access Enabler组件文件。
* 生成身份验证和授权令牌。

### 令牌 {#tokens}

Adobe Pass身份验证权利解决方案侧重于生成在成功完成身份验证/授权工作流后获取的特定数据段。 这些数据段称为令牌。 它们的生命周期有限，而且会安全地存储在依赖于平台的位置。 过期后，必须通过重新启动身份验证和/或授权工作流重新颁发令牌。

在身份验证/授权工作流中颁发了三种类型的令牌。 其中两个是“长期”的，提供了用户观看体验的连续性。 第三个令牌是一个短暂的令牌，它支持通过流盗用减少欺诈的行业最佳实践。 令牌的生存时间(“TTL”)值根据MVPD和程序员之间的协议进行设置。 您可以确定最适合您的企业和客户的TTL价值。

**长效身份验证令牌**。 一旦客户使用Adobe Pass身份验证成功登录到其MVPD帐户，身份验证就会成功。 然后，Adobe Pass身份验证生成与请求设备绑定的长期身份验证(“authN”)令牌，以及（根据MVPD）可匿名识别用户的全局唯一标识符(“GUID”)。

**长期授权令牌**。 成功授权后，Adobe Pass身份验证会创建一个长生命周期授权(“authZ”)令牌。 此令牌不可移植，因为它绑定到请求设备和特定的受保护资源（例如频道、系列或剧集）。 Access Enabler使用长期authZ令牌创建用于实际查看访问的短期媒体令牌。

**短期媒体令牌**。 用户获得授权后，Adobe Pass身份验证会生成一个authZ令牌，然后使用该令牌生成一个一次性使用的短期媒体令牌，该令牌经过Adobe签名并加密，以避免在交换期间被篡改。 由于短期令牌通过Access Enabler API或无客户端Web服务向嵌入站点公开，因此在提供对受保护资源的访问权限之前，程序员的媒体服务器必须使用Adobe Pass身份验证组件（媒体令牌验证器）来验证令牌。

## MVPD集成生命周期 {#lifecycle}

下图显示了Adobe Pass身份验证和MVPD之间集成的生命周期。

![](../assets/mvpd-int-lifecycle.png)

*图： MVPD集成生命周期*

## 权利流程图 {#chart}

以下流程图显示了使用Adobe Pass身份验证确认授权的整个过程：

![](../assets/authn-authz-entitlmnt-flow.png)

*图：使用Adobe Pass身份验证确认授权的过程*

## 身份验证步骤 {#authn-steps}

以下步骤显示了Adobe Pass身份验证流程的示例。  这是授权过程的一部分，在该过程中，程序员确定用户是否为MVPD的有效客户。  在此方案中，用户是MVPD的有效订阅者。  用户正尝试使用程序员的Flash应用程序查看受保护的内容：

1. 用户浏览到程序员的网页，该网页将程序员的Flash应用程序和Adobe Pass Authentication Access Enabler组件加载到用户的计算机上。 Flash应用程序使用Access Enabler设置程序员的身份与Adobe Pass Authentication，而Adobe Pass Authentication为该程序员（“请求者”）的配置和状态数据预定Access Enabler。 在执行任何其他API调用之前，Access Enabler必须从服务器接收此数据。  技术说明：程序员使用Access Enabler的`setRequestor()`方法设置其标识；有关详细信息，请参阅[程序员集成指南](/help/authentication/integration-guide-programmers/programmer-integration-guide-overview.md)。
1. 当用户尝试查看程序员的受保护内容时，程序员的应用程序向用户显示MVPD列表，用户从中选择提供程序。
1. 用户被重定向到Adobe Pass身份验证服务器，其中为用户选择的MVPD创建加密的SAML请求。 此请求将作为代表程序员的身份验证请求发送到MVPD。 根据MVPD的系统，随后用户的浏览器将被重定向到MVPD的站点以登录，或者在程序员的应用程序中创建一个登录iFrame。
1. 无论是哪种情况（重定向还是iFrame），MVPD都会接受请求并显示其登录页面。
1. 用户使用MVPD登录，MVPD验证用户作为付费客户的状态，然后MVPD创建自己的HTTP会话。
1. 验证用户后，MVPD会创建一个响应（SAML和加密），MVPD会将该响应发送回Adobe Pass身份验证。
1. Adobe Pass身份验证接收MVPD响应，看到Adobe Pass身份验证HTTP会话已打开，验证来自MVPD的[SAML](https://en.wikipedia.org/wiki/Security_Assertion_Markup_Language)响应，并重定向回程序员网站。
1. 重新加载程序员的站点，重新加载Access Enabler，程序员再次调用setRequestor()。  需要第二次调用setRequestor()，因为当前配置已更改 — 现在存在一个标志，用于通知Access Enabler正在服务器上等待生成AuthN令牌。
1. 访问启用程序发现存在挂起的身份验证，并向Adobe Pass身份验证服务器请求令牌。 通过调用Flash Player的DRM功能，从服务器检索令牌。
1. AuthN令牌存储在程序员的Flash PlayerLSO缓存中；身份验证现已完成，并且会话在Adobe Pass身份验证服务器上被销毁。

## 授权步骤 {#authz-steps}

以下步骤在上一节中继续（[身份验证步骤](#authn-steps)）：

1. 当用户尝试访问程序员的受保护内容时，程序员的应用程序首先会检查用户的本地计算机或设备上是否存在身份验证令牌。  如果该令牌不存在，则执行上述[身份验证步骤](#authn-steps)。  如果存在AuthN令牌，则授权流程继续进行，程序员的应用程序启动对访问启用程序的调用，请求获取用户对受保护内容的特定项目的查看权限。
1. 受保护内容的特定项目由“资源标识符”表示。  这可以是简单的字符串或更复杂的结构，但在任何情况下，资源标识符的性质都事先在程序员和MVPD之间商定。  程序员的应用程序将资源标识符传递给Access Enabler。  Access Enabler检查用户的本地计算机或设备上是否存在AuthZ令牌。  如果没有AuthZ令牌，Access Enabler会将请求传递到后端Adobe Pass身份验证服务器。
1. Adobe Pass身份验证服务器使用标准化协议与MVPDs授权端点进行通信。  如果MVPD的响应指示用户有权查看受保护的内容，则Adobe Pass身份验证服务器会创建一个AuthZ令牌并将其传递回Access Enabler，后者将AuthZ令牌存储在用户的计算机上。
1. 当用户计算机或设备上存储有AuthZ令牌时，程序员的应用程序会调用Access Enabler以从Adobe Pass身份验证服务器获取媒体令牌，并将该令牌提供给程序员的应用程序。
1. 最后，程序员的应用程序使用媒体令牌验证器组件来确认正确的用户正在查看正确的内容，并且在有媒体令牌的情况下，允许用户查看受保护的内容。

<!--
>![RELATEDINFORMATION]
>
>*   Kickstart Guides, [MVPD kickstart](/help/authentication/mvpd-kickstart-guide.md) and [programmer kickstart](/help/authentication/programmer-kickstart-guide.md). These guides explain the initial steps to take to begin integrating with Adobe Pass Authentication.
>
>*   [MVPD Integration Guide](/help/authentication/mvpd-kickstart-guide.md). This is a lower level technical guide for MVPDs, directed primarily to the software engineers who code and test the applications and systems involved in the integration.
>
>*   [Overview For Programmers](/help/authentication/programmer-overview.md). The same high level of conceptual information as in this MVPD overview, but directed toward the content providers (Programmers).
-->
