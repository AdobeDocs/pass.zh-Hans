---
title: Apple SSO概述
description: Apple SSO概述
exl-id: 7cf47d01-a35a-4c85-b562-e5ebb6945693
source-git-commit: 2afe9ea2a814817757f1ab28484a84466da68d62
workflow-type: tm+mt
source-wordcount: '1260'
ht-degree: 0%

---

# Apple SSO概述 {#apple-sso-overview}

>[!IMPORTANT]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

Apple为用户提供了在设备系统级别登录其电视提供商帐户的功能，而无需逐个应用程序进行身份验证。

Adobe Pass身份验证与Apple合作，为iPhone、iPad和Apple电视所有者在TV Everywhere生态系统中创建合作伙伴单点登录(SSO)用户体验。

为了从Apple设备上的单点登录(SSO)用户体验中获益，必须完成下面列出的先决条件。

最终结果将创建一个与以下用户流程一致的体验，我们建议您在开始开发应用程序之前进行咨询：

* 用于iPhone和iPad[设备的单点登录(SSO) &#x200B;](https://tve.zendesk.com/hc/article_attachments/205624966/User_flows_AppleSSO_iOS_v2.pdf)用户流程。
* Apple TV[设备的单点登录(SSO) &#x200B;](https://tve.zendesk.com/hc/article_attachments/206669126/User_flows_tvOS.pdf)用户流。

## 先决条件 {#apple-sso-prerequisites}

载入先决条件可能适用于TVE业务中涉及的一个或多个实体，例如程序员、MVPD、Adobe Pass身份验证或Apple。

### 程序员 {#apple-sso-prerequisites-programmer}

为了从单点登录(SSO)用户体验中获益，一位程序员必须：

* 请联系Apple以将[视频订阅者帐户框架](https://developer.apple.com/documentation/videosubscriberaccount)作为Apple团队ID的一部分启用，并将[视频订阅者单点登录权利](https://developer.apple.com/documentation/bundleresources/entitlements/com_apple_developer_video-subscriber-single-sign-on)作为Apple开发人员帐户的一部分配置。

   * 使用Xcode版本8或更高版本，以及iOS/tvOS版本10或更高版本。

* 通过将[属性设置为](https://experience.adobe.com/#/pass/authentication)，通过`Enable Single Sign On`Adobe Pass TVE功能板`Yes`为每个所需的集成和平台(iOS/tvOS)启用单点登录(SSO)。

| Adobe启用单点登录 | Apple **已载入（支持）** MVPD | Apple **选取器** MVPD | Apple **未载入（不支持）** MVPD |
|-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| 是（已启用） | 身份验证和注销流将同时涉及Apple和Adobe Pass身份验证解决方案，而所有其他流（授权、预授权、元数据等）将仅由Adobe Pass身份验证提供服务。 | 身份验证和注销流将回退到仅由Adobe Pass身份验证提供服务的常规流。 | 身份验证和注销流将回退到仅由Adobe Pass身份验证提供服务的常规流。 |
| 否（已禁用） | 身份验证和注销流将回退到仅由Adobe Pass身份验证提供服务的常规流。 | 身份验证和注销流将回退到仅由Adobe Pass身份验证提供服务的常规流。 | 身份验证和注销流将回退到仅由Adobe Pass身份验证提供服务的常规流。 |

* 使用Adobe Pass身份验证为iOS、iPadOS或tvOS上运行的客户端应用程序的最终用户提供的以下解决方案之一，集成单点登录(SSO)用户流。

   * Adobe Pass身份验证REST API V2支持合作伙伴单点登录(SSO)。

     请参阅[Apple SSO指南(REST API V2)](apple-sso-cookbook-rest-api-v2.md)文档。

   * 旧版Adobe Pass身份验证REST API V1支持合作伙伴单点登录(SSO)。

     请参阅[（旧版） Apple SSO指南(REST API V1)](/help/authentication/integration-guide-programmers/legacy/sso-access/apple-sso-cookbook-rest-api-v1.md)文档。

   * 旧版Adobe Pass Authentication AccessEnabler iOS/tvOS SDK支持合作伙伴单点登录(SSO)。

     请参阅[（旧版） Apple SSO指南(iOS/tvOS SDK)](/help/authentication/integration-guide-programmers/legacy/sso-access/apple-sso-cookbook-iostvos-sdk.md)文档。

### MVPD {#apple-sso-prerequisites-mvpd}

为了从单点登录(SSO)用户体验中获益，一个MVPD必须：

* 联系Apple以在Apple一方启动载入流程。

   * 请求获取有关如何集成和开发能够处理用户登录表单的JavaScript TVML应用程序的技术文档。

* 联系Adobe Pass身份验证以在Adobe一方启动载入流程。

   * 提供表示Apple在新用户引导过程中分配的电视提供商标识符的字符串值。

## 常见问题解答 {#FAQ}

* 如果Apple SSO工作流出现问题，使用Adobe Pass Authentication AccessEnabler iOS/tvOS SDK的应用程序能否回退到常规身份验证流程？

  此操作是可能的，但需要通过[Adobe Pass TVE仪表板](https://experience.adobe.com/#/pass/authentication)执行配置更改，以便为所需的集成和平台(iOS/tvOS)在&#x200B;**NO**&#x200B;上设置&#x200B;**启用单点登录**。 请注意，只有在调用[setRequestor](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setReqV3) API之后，客户端应用程序才会确认配置更改。


* 应用程序是否知道通过Apple SSO登录后何时发生身份验证？

  此信息作为用户元数据密钥&#x200B;*tokenSource*&#x200B;的一部分提供，在本例中，该密钥应返回字符串值“Apple”。


* 当在其他应用程序上通过Apple SSO登录后，应用程序是否知道发生了身份验证？

  此信息不可用。


* 如果用户使用未与应用程序集成的MVPD登录到iOS/iPadOS上的&#x200B;*`Settings -> TV Provider`*&#x200B;或tvOS上的&#x200B;*`Settings -> Accounts -> TV Provider`*&#x200B;分区，会发生什么情况？

  当用户启动应用程序时，将无法通过Apple SSO工作流对用户进行身份验证。 因此，应用程序必须回退到常规身份验证流程，并显示自己的MVPD选取器。


* 如果用户通过使用在&#x200B;*`Settings -> TV Provider`* NO *`Settings -> Accounts -> TV Provider`*&#x200B;上设置&#x200B;**启用单点登录**&#x200B;的MVPD(通过适用于iOS/tvOS平台的&#x200B;**Adobe Pass TVE功能板**)，转到iOS/iPadOS上的[或tvOS上的](https://experience.adobe.com/#/pass/authentication)分区进行登录，会出现什么情况？

  当用户启动应用程序时，将无法通过Apple SSO工作流对用户进行身份验证。 因此，应用程序必须回退到常规身份验证流程，并显示自己的MVPD选取器。


* 如果用户具有不受Apple载入（不支持）的MVPD，但它存在于Apple选取器中，会发生什么情况？

  当用户启动应用程序时，用户将仅通过Apple SSO工作流选择MVPD，而不完成身份验证流程。 因此，应用程序必须回退到常规身份验证流程，但可以使用已选择的MVPD。


* 如果用户具有不受Apple载入（不支持）的MVPD，会发生什么情况？

  当用户启动应用程序时，将通过Apple SSO工作流选择“其他电视提供商”选取器选项。 因此，应用程序必须回退到常规身份验证流程，并显示自己的MVPD选取器。


* 如果用户的MVPD通过[Adobe Pass TVE仪表板](https://experience.adobe.com/#/pass/authentication)介质降级，会发生什么情况？

  当用户启动应用程序时，将通过降级机制而不是通过Apple SSO工作流对用户进行身份验证。 该体验应该对用户来说是无缝的，但如果应用程序使用Adobe Pass Authentication AccessEnabler iOS/tvOS SDK，则会通过&#x200B;*N010*&#x200B;警告代码通知应用程序。


* MVPD用户ID在Apple SSO和非Apple SSO身份验证流程之间是否会更改？

  预计用户ID不会发生更改，但需要为每个选择的提供商验证该ID。


* 身份验证TTL是否会有任何更改？

  Adobe Pass身份验证将继续遵守程序员为与每个MVPD的集成所需的TTL。 当通过Apple SSO从一个程序员应用程序导航到另一个程序员应用程序时，第二个应用程序将拥有其相应的程序员x MVPD集成的TTL（它不会共享验证第一个应用程序的TTL）

|                                      | Adobe Pass身份验证TTL已过期 | Adobe Pass身份验证TTL有效 |
|--------------------------------------|------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| **Apple的设备令牌TTL已过期** | 用户未经过身份验证(应显示MVPD选取器) | 用户已完成身份验证，并且TTL是其Adobe Pass身份验证令牌/配置文件的剩余时间 |
| **Apple的设备令牌TTL有效** | 用户通过静默身份验证，并使用TVE仪表板中指定的TTL获取另一个Adobe Pass身份验证令牌/配置文件 | 用户已完成身份验证，并且TTL是其Adobe Pass身份验证令牌/配置文件的剩余时间 |
