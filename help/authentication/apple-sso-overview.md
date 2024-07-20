---
title: Apple SSO概述
description: Apple SSO概述
exl-id: 7cf47d01-a35a-4c85-b562-e5ebb6945693
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '1417'
ht-degree: 0%

---

# Apple SSO概述 {#apple-sso-overview}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## 简介 {#Introduction}

Apple提供了一个API，允许用户在设备系统级别登录其电视提供商帐户，而无需逐个应用程序进行身份验证。

因此，Apple与Adobe Pass身份验证合作，在iPhone、iPad和Apple电视的所有者的TV Everywhere生态系统中创建Platform Single Sign-On (SSO)用户体验。

为了从Apple设备上的单点登录(SSO)用户体验中获益，必须完成一系列先决条件。

</br>

## 先决条件 {#Prerequisites}

前提条件可能适用于TVE业务中涉及的一个或多个实体，例如程序员、MVPD、Adobe Pass身份验证或Apple。

</br>

### 程序员 {#Programmer}

为了从单点登录(SSO)用户体验中获益，一位程序员必须：

1. 请至少使用Xcode版本8和iOS/tvOS版本10。

1. 将[视频订阅者单点登录权利](https://developer.apple.com/documentation/bundleresources/entitlements/com_apple_developer_video-subscriber-single-sign-on)配置为其Apple开发人员帐户。 请联系Apple为您的Apple团队ID启用[视频订阅者帐户框架](https://developer.apple.com/documentation/videosubscriberaccount)。

1. 通过[Adobe Primetime TVE Dashboard](https://console.auth.adobe.com/)，为每个所需的集成(Channel x MVPD)和所需的平台(iOS / tvOS)启用单点登录（是）。

1. 使用Apple身份验证团队提供的以下两个解决方案之一集成Adobe Pass SSO工作流：

   - Adobe Pass身份验证REST API可为在iOS、iPadOS或tvOS上运行的客户端应用程序的最终用户支持平台单点登录(SSO)身份验证。 另请参阅[Apple SSO指南(REST API)](/help/authentication/apple-sso-cookbook-rest-api.md)。

   - Adobe Pass Authentication AccessEnabler iOS/tvOS SDK可为在iOS、iPadOS或tvOS上运行的客户端应用程序的最终用户支持平台单点登录(SSO)身份验证。 另请参阅[Apple SSO指南(iOS/tvOS SDK)](/help/authentication/apple-sso-cookbook-iostvos-sdk.md)。

   - **<u>专业提示：</u>**&#x200B;要访问用户的订阅信息，用户必须授予应用程序继续操作的权限，类似于提供对设备摄像头或麦克风的访问权限。 必须为每个应用程序请求此权限，设备将保存用户的选择。 请记住，用户可以通过转到应用程序设置（电视提供商权限访问权限）或从iOS/iPadOS上的&#x200B;*`Settings -> TV Provider`*&#x200B;或tvOS上的&#x200B;*`Settings -> Accounts -> TV Provider`*&#x200B;部分更改其决定。

   - **<u>专业提示：</u>**&#x200B;我们建议在应用程序进入前台状态时请求用户的权限，但这只是个建议，因为应用程序可以在要求用户身份验证之前随时检查[访问](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)用户订阅信息的权限。 此外，AccessEnabler iOS/tvOS SDK API将在需要时自动请求用户的权限。

   - **<u>专业提示：</u>**&#x200B;我们建议通过解释单点登录(SSO)用户体验的好处，来激励拒绝授予访问订阅信息权限的用户。 请记住，用户可以通过转到应用程序设置（电视提供商权限访问权限）或从iOS/iPadOS上的&#x200B;*`Settings -> TV Provider`*&#x200B;或tvOS上的&#x200B;*`Settings -> Accounts -> TV Provider`*&#x200B;部分更改其决定。

这样产生的结果应会创建符合以下用户流程的体验，我们建议您在开始开发应用程序之前查阅这些用户流程：

- [iPhone / iPad](http://tve.zendesk.com/hc/article_attachments/205624966/User_flows_AppleSSO_iOS_v2.pdf)用户流
- [Apple TV](http://tve.zendesk.com/hc/article_attachments/206669126/User_flows_tvOS.pdf)用户流


>[!IMPORTANT]
>
> 当iOS/tvOS **和**&#x200B;的单点登录功能为&#x200B;**已启用**&#x200B;时(对于Apple **已载入（受支持）或选取器** MVPD)，来自Apple SSO工作流的身份验证/注销流将同时涉及Apple和Adobe Pass身份验证解决方案，而所有其他流（授权、预授权、元数据等） 将仅由Adobe Pass身份验证提供服务。


>[!IMPORTANT]
>
> 当iOS/tvOS **的单点登录功能**&#x200B;被禁用&#x200B;**时，或者Apple**&#x200B;未上线（不支持）**MVPD的情况下，身份验证/注销流将从Apple SSO工作流回退到仅由Adobe Pass身份验证提供服务的常规工作流。**


>[!IMPORTANT]
>
> Apple SSO工作流程的一个主要增益由单屏身份验证用户流表示，当为tvOS **启用单点登录功能**&#x200B;时，该用户流也可以在Apple电视上交付；如果是Apple **已载入（受支持）** MVPD，该功能为&#x200B;**时。**


### MVPD {#MVPD}

为了从单点登录(SSO)用户体验中受益，请启用
MVPD必须：



1. 被载入Apple的Apple SSO工作流。 请联系Apple以简化入门培训流程。
1. 提供一个能够处理用户登录表单的JavaScript TVML应用程序。 请联系Apple以接收正确的文档。
1. 提供一个字符串值，表示Apple在新用户引导过程中分配的提供商标识符。 请联系Adobe Pass身份验证以执行配置更改。

</br>

## 常见问题解答 {#FAQ}

1. 如果Apple SSO工作流出现问题，使用AccessEnabler iOS/tvOS SDK的应用程序能否回退到常规身份验证流程？
   - 这是可能的，但需要对[Adobe Primetime TVE仪表板](https://console.auth.adobe.com/)执行配置更改。 对于所需的集成(Channel x MVPD)和所需的平台(iOS/tvOS)，必须在&#x200B;*NO*&#x200B;上设置&#x200B;*启用单点登录*。
   - 如果应用程序使用AccessEnabler iOS/tvOS SDK，则只有在调用[setRequestor](/help/authentication/iostvos-sdk-api-reference.md#setReqV3) API后，它才会确认配置更改。
1. 当通过其他设备或其他应用程序上的平台SSO登录后，应用程序是否知道发生了身份验证？
   - 此信息将不可用。
1. 由于通过同一设备上的平台SSO登录，应用程序是否知道何时发生身份验证？
   - 此信息作为用户元数据密钥&#x200B;*tokenSource*&#x200B;的一部分提供，在本例中，该密钥应返回字符串值“Apple”。
1. 如果用户通过使用未与应用程序集成的MVPD登录到iOS/iPadOS上的&#x200B;*`Settings -> TV Provider`*&#x200B;或tvOS上的&#x200B;*`Settings -> Accounts -> TV Provider`*&#x200B;分区，会发生什么情况？
   - 当用户启动应用程序时，将无法通过Apple SSO工作流对用户进行身份验证。 因此，应用程序必须回退到常规身份验证流程，并显示自己的MVPD选取器。
1. 如果用户通过使用在iOS/tvOS平台的[iOS TVE功能板](https://console.auth.adobe.com/)上的&#x200B;*NO*&#x200B;上设置的&#x200B;*启用单点登录*&#x200B;的MVPD登录到Adobe Primetime/iPadOS上的&#x200B;*`Settings -> TV Provider`*&#x200B;或tvOS上的&#x200B;*`Settings -> Accounts -> TV Provider`*，则会发生什么情况？
   - 当用户启动应用程序时，将无法通过Apple SSO工作流对用户进行身份验证。 因此，应用程序必须回退到常规身份验证流程，并显示自己的MVPD选取器。
1. 如果用户具有不受Apple载入（不支持）但存在于Apple选取器中的MVPD，会发生什么情况？
   - 当用户启动应用程序时，用户将仅通过Apple SSO工作流选择MVPD，而不完成身份验证流程。 因此，应用程序必须回退到常规身份验证流程，但可以使用已选择的MVPD。
1. 如果用户具有Apple未载入（不支持）的MVPD，会发生什么情况？
   - 当用户启动应用程序时，将通过Apple SSO工作流选择“其他电视提供商”选取器选项。 因此，应用程序必须回退到常规身份验证流程，并显示自己的MVPD选取器。
1. 如果用户的MVPD通过[Adobe Primetime TVE Dashboard](https://console.auth.adobe.com/)介质降级，会发生什么情况？
   - 当用户启动应用程序时，将通过降级机制而不是通过Apple SSO工作流对用户进行身份验证。
   - 该体验应该对用户来说是无缝的，但如果应用程序使用AccessEnabler iOS/tvOS SDK，则会通过&#x200B;*N010*&#x200B;警告代码通知应用程序。
1. Apple SSO和非Apple SSO身份验证流程之间的MVPD用户ID是否会更改？
   - 预计用户ID不会发生更改，但需要为每个选择的提供商验证该ID。
1. 身份验证TTL是否会有任何更改？
   - Adobe Pass身份验证将继续遵循程序员集成每个MVPD所需的TTL。
   - 当通过Apple SSO从一个程序员应用程序导航到另一个程序员应用程序时，第二个应用程序将拥有其相应程序员x MVPD集成的TTL（它不会共享验证第一个应用程序的TTL）

|                                      | Adobe Pass身份验证TTL已过期 | Adobe Pass身份验证TTL有效 |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| **Apple的设备令牌TTL已过期** | 用户未验证（应显示MVPD选取器） | 用户已完成身份验证，且TTL是其Adobe Pass身份验证令牌的剩余时间 |
| **Apple的设备令牌TTL有效** | 用户通过静默式身份验证，并使用TVE功能板中指定的TTL获取另一个Adobe Pass身份验证令牌 | 用户已完成身份验证，且TTL是其Adobe Pass身份验证令牌的剩余时间 |

<!--

## Resources {#Resources}

- [Apple SSO Cookbook (REST API)](/help/authentication/apple-sso-cookbook-rest-api.md)
- [Apple SSO Cookbook (iOS/tvOS SDK)](/help/authentication/apple-sso-cookbook-iostvos-sdk.md)
- [Sign in with your TV provider on your iPhone, iPad, or iPod touch](https://support.apple.com/en-us/HT207035)
- [Use your pay TV or cable provider with Apple TV](https://support.apple.com/en-us/HT207035)
- [TV providers that let you sign in on your iPhone, iPad, or Apple TV](https://support.apple.com/en-us/HT208084)
- [TV Provider Authentication](https://developer.apple.com/design/human-interface-guidelines/tvos/system-capabilities/tv-provider-authentication/)
- [Apple Developer Documentation - Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount)
-->
