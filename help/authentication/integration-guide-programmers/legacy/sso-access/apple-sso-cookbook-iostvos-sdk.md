---
title: Apple SSO指南(iOS/tvOS SDK)
description: Apple SSO指南(iOS/tvOS SDK)
exl-id: 2d59cd33-ccfd-41a8-9697-1ace3165bc44
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '1834'
ht-degree: 0%

---

# （旧版）Apple SSO指南(iOS/tvOS SDK) {#apple-sso-cookbook-iostvos-sdk}

>[!IMPORTANT]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

>[!IMPORTANT]
>
> 确保随时了解汇总在[产品公告](/help/authentication/product-announcements.md)页中的最新Adobe Pass身份验证产品公告和停用时间表。

Adobe Pass Authentication AccessEnabler iOS/tvOS SDK支持在iOS、iPadOS或tvOS上运行的客户端应用程序的最终用户使用合作伙伴单点登录(SSO)。

此文档可用作现有AccessEnabler iOS/tvOS SDK文档的扩展，该文档可在[此处](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)找到。

## 指南 {#apple-sso-cookbook-iostvos-sdk-cookbook}

为了从Apple SSO用户体验中获益，该应用程序需要集成AccessEnabler iOS/tvOS SDK并遵循下面介绍的步骤顺序。

### 先决条件 {#apple-sso-cookbook-iostvos-sdk-prerequisites}

#### 权限 {#apple-sso-cookbook-iostvos-sdk-permission}

>[!TIP]
>
> **<u>专业提示：</u>**&#x200B;流式应用程序必须请求访问保存在设备级别的用户订阅信息，用户必须授予应用程序继续操作的权限，类似于提供对设备摄像头或麦克风的访问权限。 必须使用Apple的[视频订阅者帐户框架](https://developer.apple.com/documentation/videosubscriberaccount)为每个应用程序请求此权限，设备将保存用户的选择。

>[!TIP]
>
> **<u>专业提示：</u>**&#x200B;我们建议通过说明Apple单点登录用户体验的好处，来激励拒绝授予访问订阅信息权限的用户，但请注意，用户可以通过转到应用程序设置（访问电视提供商权限）、转到iOS和iPadOS上的&#x200B;*`Settings -> TV Provider`*&#x200B;或tvOS上的&#x200B;*`Settings -> Accounts -> TV Provider`*&#x200B;来更改其决策。

>[!TIP]
>
> **<u>专业提示：</u>**&#x200B;当应用程序进入前台状态时，流式应用程序可以请求用户的权限，因为在需要用户身份验证之前，应用程序可以随时检查[访问](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)用户订阅信息的权限。

>[!TIP]
>
> **<u>专业提示：</u>**&#x200B;如果用户未授予其订阅信息的访问权限，或者如果与视频订阅者帐户框架的通信失败，则AccessEnabler iOS/tvOS SDK将回退到常规身份验证流程。

```swift
    ...
    let videoSubscriberAccountManager: VSAccountManager = VSAccountManager();
    
    videoSubscriberAccountManager.checkAccessStatus(options: [VSCheckAccessOption.prompt: true]) { (accessStatus, error) -> Void in
                switch (accessStatus) {
                // The user allows the application to access subscription information.
                case VSAccountAccessStatus.granted:
                   // Do nothing.
                
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                   // Incentivize users who refuse to give permission to access subscription information by explaining the benefits of the Single Sign-On (SSO) user experience. Please bear in mind that the user can change its decision by going to the application settings (TV Provider permission access) or to the section from Settings -> TV Provider on iOS/iPadOS or Settings -> Accounts -> TV Provider on tvOS.
                   ...
                }
    }
    ... 
```

#### 回调 {#apple-sso-cookbook-iostvos-sdk-callbacks}

>[!TIP]
>
> **<u>专业提示：</u>**&#x200B;实施以下特定于Apple SSO工作流的[回调](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)列表。

* [*presentTVProviderDialog*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#presenttvproviderdialog-presenttvdialog) — 在Apple MVPD选取器打开时触发回调。
* [*dissiseTVProviderDialog*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#dismisstvproviderdialog-dismisstvdialog) — 即将关闭Apple MVPD选取器时触发的回调。

#### 错误报告 {#apple-sso-cookbook-iostvos-sdk-error-reporting}

>[!TIP]
>
> **<u>专业提示：</u>**&#x200B;实施以下[高级错误代码](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md)列表，这些错误代码特定于Apple SSO工作流。

* ***N003*** — 用户从Apple MVPD选取器中选择了“其他电视提供商”选项。
* ***N004*** — 用户从Apple MVPD选取器中选择了一个当前请求者不支持的电视提供程序（集成或单点登录）。
* ***N005*** — 用户决定取消常规MVPD选取器或Apple MVPD选取器。
* ***VSA403*** — 应用程序的用户电视提供程序权限被拒绝。
* ***VSA404*** — 应用程序的用户电视提供程序权限未确定。
* ***VSA503*** — 视频订阅者帐户元数据请求失败，*消息*&#x200B;字段中提供了更多上下文。
* ***AAPL / APPL_ERROR*** — 视频订阅者帐户元数据请求失败，*详细信息*&#x200B;字段中提供了更多上下文。

### 身份验证 {#apple-sso-cookbook-iostvos-sdk-authentication}

>[!TIP]
>
> **<u>提示：</u>**&#x200B;请按照以下步骤实施iOS/iPadOS/tvOS。

1. 应用程序必须[初始化](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#initsoftwarestatement-initwithsoftwarestatement) AccessEnabler iOS/tvOS SDK。


1. 应用程序必须[设置当前请求者标识符](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setrequestorrequestorid-setrequestorrequestoridserviceproviders-setreqv3)。

   **重要提示：**&#x200B;如果以下任一情况为true **，则第二个步骤可能会触发特定于Apple SSO工作流的[高级错误代码](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md)：**

   * ***VSA403*** — 应用程序的用户电视提供程序权限被拒绝。
   * ***VSA404*** — 应用程序的用户电视提供程序权限未确定。
   * ***APPL*** - AccessEnabler iOS/tvOS SDK与视频订阅者帐户框架之间的通信遇到错误。

   第二步将尝试静默地交换Apple SSO配置文件以获取Adobe身份验证令牌，前提是&#x200B;**以上所有条件均为false**&#x200B;且&#x200B;**以下所有条件均为true**：

   * 已为应用程序授予用户的TV提供商权限。
   * 用户已在设备系统级别登录其电视提供商帐户。
   * AccessEnabler iOS/tvOS SDK从视频订阅者帐户框架收到了用户的电视提供程序标识符。
   * 通过Adobe Primetime TVE功能板，可启用用户的电视提供程序与应用程序的集成。
   * 用户的电视提供程序通过应用程序的单点登录可通过Adobe Primetime TVE仪表板启用。
   * 用户的电视提供程序未通过Adobe Primetime TVE仪表板降级。
   * AccessEnabler iOS/tvOS SDK从视频订阅者帐户框架收到了用户的电视提供程序SAML响应。

   **<u>专业提示：</u>**&#x200B;第二个步骤不会触发除[setRequestorComplete](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setrequestorcomplete-setreqcomplete)回调之外的任何其他回调，因为应用程序未显式启动身份验证。


1. 应用程序必须[检查身份验证状态](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkauthentication-checkauthn)。

   **重要提示：**&#x200B;如果以下任一情况为true **，则第三个步骤可能会触发特定于Apple SSO工作流的[高级错误代码](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md)：**

   * ***VSA403** — 用户已在以下位置登录到其电视提供商帐户：
设备系统级别，但用户的电视提供程序权限为
已拒绝该应用程序。
   * ***VSA404** — 用户已在以下位置登录到其电视提供商帐户：
设备系统级别，但用户的电视提供程序权限
尚未确定。
   * ***APPL\_ERROR** — 用户已登录其电视提供商
帐户在设备系统级别，但与
AccessEnabler iOS/tvOS SDK和视频订阅者帐户
框架遇到错误。

   **重要信息：**&#x200B;如果以下项之一为true **，则第三个步骤将触发&#x200B;[*setAuthenticationStatus*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus)回调，其中&#x200B;*状态*等于0：**

   * 用户未在设备系统级别或通过常规身份验证流程登录到其TV Provider帐户。
   * 用户在设备系统级别或通过常规身份验证流程登录到其电视提供程序帐户，但用户的电视提供程序身份验证令牌TTL已通过。
   * 用户已在设备系统级别或通过常规身份验证流程登录到其电视提供程序帐户，但已通过Adobe Primetime TVE仪表板禁用用户与应用程序的电视提供程序集成。
   * 用户已在设备系统级别登录其电视提供商帐户，但用户的电视提供商单点登录应用程序已通过Adobe Primetime TVE仪表板禁用。
   * 用户在设备系统级别登录到其TV提供商帐户，但应用程序的用户的TV提供商权限被拒绝。
   * 用户在设备系统级别登录到其TV提供商帐户，但应用程序的用户的TV提供商权限未确定。
   * 用户在设备系统级别登录到其电视提供商帐户，但AccessEnabler iOS/tvOS SDK与视频订阅者帐户框架之间的通信遇到错误。

   **重要信息：**&#x200B;第三个步骤将触发&#x200B;[*setAuthenticationStatus*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus)&#x200B;回调，其中&#x200B;*状态*&#x200B;等于1，如果&#x200B;**以上全部为false。**


1. 如果以前的身份验证状态检查触发了&#x200B;[*setAuthenticationStatus*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus)&#x200B;回调且&#x200B;*状态*&#x200B;等于0，则应用程序将必须[初始化身份验证](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getauthentication-getauthenticationwithdata-getauthn)。

   **<u>专业提示：</u>**&#x200B;实施以下AccessEnabler iOS/tvOS SDK API [getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN)或[getAuthentication：filter](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN_filter)之一。

   **重要提示：**&#x200B;如果以下任一情况为true **，则第四个步骤可能会触发特定于Apple SSO工作流的[高级错误代码](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md)：**

   * ***VSA403*** — 应用程序的用户电视提供程序权限被拒绝。
   * ***VSA404*** — 应用程序的用户电视提供程序权限未确定。
   * ***VSA503*** - AccessEnabler iOS/tvOS SDK与视频订阅者帐户框架之间的通信遇到错误。
   * ***N003*** — 用户从Apple MVPD选取器中选择了“其他电视提供商”选项。
   * ***N004*** — 用户从Apple MVPD选取器中选择了一个当前请求者不支持的电视提供程序（集成或单点登录）。
   * ***N005*** — 用户决定取消常规MVPD选取器或Apple MVPD选取器。

   **重要提示：**&#x200B;第四步将回退到常规身份验证流程，方法是：触发上述[高级错误代码](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md)中的[displayProviderDialog](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#dispProvDialog)回调和&#x200B;**one**，前提是上述回调中的&#x200B;**一个为true**。

   **重要信息：**&#x200B;如果用户选择的电视提供程序不支持Apple SSO，但存在于Apple MVPD选取器中，则第四步将回退到常规身份验证流程，方法是触发[navigateToUrl](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#nav2url)或[navigateToUrl：useSVC](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#nav2urlSVC)回调以及上述[高级错误代码](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md)中的&#x200B;**none**。

   **<u>专业提示：</u>** AccessEnabler iOS/tvOS SDK静默调用[setSelectedPrrovder](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) API，以防用户选择的电视提供程序不支持Apple SSO，但存在于Apple MVPD选取器中。

   **重要提示：**&#x200B;如果以上所有条件均为false **且**&#x200B;以下所有条件均为true **，则第四步将尝试静默交换Apple SSO配置文件以获取Adobe身份验证令牌：**

   * 已为应用程序授予用户的TV提供商权限。
   * 用户已登录/当前已在设备系统级别登录其电视提供商帐户。
   * AccessEnabler iOS/tvOS SDK从视频订阅者帐户框架收到了用户的电视提供程序标识符。
   * 通过Adobe Primetime TVE功能板，可启用用户的电视提供程序与应用程序的集成。
   * 用户的电视提供程序通过应用程序的单点登录可通过Adobe Primetime TVE仪表板启用。
   * 用户的电视提供程序未通过Adobe Primetime TVE仪表板降级。
   * AccessEnabler iOS/tvOS SDK从视频订阅者帐户框架收到了用户的电视提供程序SAML响应。

   **<u>专业提示：</u>**&#x200B;此第四步将触发&#x200B;[*setAuthenticationStatus*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setAuthNStatus)&#x200B;回调，而不考虑&#x200B;*状态*&#x200B;结果，因为身份验证是由应用程序显式启动的。

### 元数据 {#apple-sso-cookbook-iostvos-sdk-metadata}

应用程序可以选择使用AccessEnabler iOS/tvOS SDK中的&quot;*tokenSource&quot;* [用户元数据](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getMeta) API，确定是否由于通过合作伙伴SSO登录而发生了身份验证。

```swift
    ...
    accessEnabler.getMetadata([METADATA_OPCODE_KEY:Int(METADATA_USER_META), METADATA_USER_META_KEY: "tokenSource"])
    ...
```

### 注销 {#apple-sso-cookbook-iostvos-sdk-logout}

[视频订阅者帐户框架](https://developer.apple.com/documentation/videosubscriberaccount)不提供API以编程方式注销已在设备系统级别登录其电视提供程序帐户的人员。 因此，要完全注销，最终用户必须从iOS/iPadOS上的&#x200B;*`Settings -> TV Provider`*&#x200B;或tvOS上的&#x200B;*`Settings -> Accounts -> TV Provider`*&#x200B;中显式注销。 用户将拥有的另一个选项是从特定应用程序设置部分（TV提供商权限访问）撤销访问用户订阅信息的权限。

>[!TIP]
>
> **<u>提示：</u>**&#x200B;通过AccessEnabler iOS/tvOS SDK [注销](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#logout) API实现此功能。

>[!TIP]
>
> **<u>专业提示：</u>**&#x200B;请按照以下步骤实施tvOS。

* 应用程序必须从AccessEnabler iOS/tvOS SDK [启动注销](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#logout)。 这将无助于MVPD端的会话清理。
* 仅当触发&#x200B;[*VSA203*&#x200B;状态代码](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md)时，应用程序才必须指示/提示用户从tvOS上的&#x200B;*`Settings -> Accounts -> TV Provider`*&#x200B;显式注销。

>[!TIP]
>
> **<u>专业提示：</u>**&#x200B;请按照以下步骤实施iOS/iPadOS。

* 应用程序必须从AccessEnabler iOS/tvOS SDK [启动注销](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#logout)。 这将有助于MVPD端的会话清理。
* 仅当触发&#x200B;[*VSA203*&#x200B;状态代码](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md)时，应用程序才必须指示/提示用户在iOS/iPadOS上从&#x200B;*`Settings -> TV Provider`*&#x200B;显式注销。
