---
title: iOS/tvOS指南
description: iOS/tvOS指南
exl-id: 4743521e-d323-4d1d-ad24-773127cfbe42
source-git-commit: 2ccfa8e018b854a359881eab193c1414103eb903
workflow-type: tm+mt
source-wordcount: '2404'
ht-degree: 0%

---

# iOS/tvOS SDK指南 {#iostvos-sdk-cookbook}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## 介绍 {#intro}

本文档介绍了权利工作流，程序员的上层应用程序可以通过iOS/tvOS AccessEnabler库公开的API实施这些工作流。

适用于iOS/tvOS的Adobe Pass身份验证权利解决方案最终分为两个域：

* UI域 — 这是上层应用程序层，用于实施UI并使用AccessEnabler库提供的服务来提供对受限内容的访问。

* AccessEnabler域 — 这是权利工作流的实施形式：

   * 向Adobe后端服务器发出的网络调用
   * 与身份验证和授权工作流相关的业务逻辑规则
   * 管理各种资源和处理工作流状态（如令牌缓存）

AccessEnabler域的目标是隐藏授权工作流的所有复杂内容，并（通过AccessEnabler库）向上层应用程序提供一组用于实施授权工作流的简单授权基元：

1. 设置请求者身份
1. 检查并获取针对特定身份提供者的身份验证
1. 检查并获取特定资源的授权
1. 注销
1. Apple SSO通过代理Apple VSA框架来流程

AccessEnabler的网络活动在其自己的线程中进行，因此从不阻止UI线程。 因此，两个应用程序域之间的双向通信渠道必须遵循完全异步模式：

* UI应用程序层通过AccessEnabler库公开的API调用，将消息发送到AccessEnabler域。
* AccessEnabler通过AccessEnabler协议（UI层向AccessEnabler库注册）中包含的回调方法来响应UI层。

## 配置Experience CloudID服务（访客ID） {#visitorIDSetup}

配置 [EXPERIENCE CLOUDID](https://experienceleague.adobe.com/docs/id-service/using/home.html) 值非常重要，请参阅 [!DNL Analytics] 观点。 一次 `visitorID` 如果设置了value，SDK会发送此信息以及每个网络调用和 [!DNL Adobe Pass] 身份验证服务器将收集此信息。 您可以将Adobe Pass身份验证服务中的分析与其他应用程序或网站中的任何其他分析报表相关联。 可以找到有关如何设置visitorID的信息 [此处](#setOptions).

## 权利流 {#entitlement}

答：  [先决条件](#prereqs) </br>
B.  [启动流程](#startup_flow) </br>
C.  [没有Apple SSO的身份验证流程](#authn_flow_wo_applesso)  </br>
D.  [iOS上使用Apple SSO的身份验证流程](#authn_flow_with_applesso) </br>
E.  [使用tvOS上的Apple SSO的身份验证流程](#authn_flow_with_applesso_tvOS) </br>
F.  [授权流程](#authz_flow) </br>
G.  [查看媒体流](#media_flow) </br>
H.  [不使用Apple SSO的注销流程](#logout_flow_wo_AppleSSO) </br>
I.  [使用Apple SSO的注销流程](#logout_flow_with_AppleSSO) </br>


### A.先决条件 {#prereqs}

1. 创建回调函数：
   * `setRequestorComplete()` </br>
   * 触发者 [setRequestor()](#$setReq)，会返回成功或失败。 </br>
   * 成功表示您可以继续权利调用。

   * [`displayProviderDialog(mvpds)`](#$dispProvDialog) </br>
      * 触发者 [`getAuthentication()`](#$getAuthN) 仅当用户尚未选择提供程序(MVPD)且尚未进行身份验证时。 </br>
      * 此 `mvpds` parameter是用户可用的提供程序数组。

   * `setAuthenticationStatus(status, errorcode)` </br>
      * 触发者 `checkAuthentication()` 每次。 </br>
      * 触发者 [`getAuthentication()`](#$getAuthN) 仅当用户已进行身份验证并已选择提供程序时。 </br>
      * 返回的状态是成功或失败，错误代码描述失败的类型。

   * [`navigateToUrl(url)`](#$nav2url) </br>
      * 触发者 [`getAuthentication()`](#$getAuthN) 在用户选择MVPD之后。 此 `url` 参数提供MVPD登录页面的位置。

   * `sendTrackingData(event, data)` </br>
      * 触发者 `checkAuthentication()`， [`getAuthentication()`](#$getAuthN)， `checkAuthorization()`， [`getAuthorization()`](#$getAuthZ)， `setSelectedProvider()`.
      * 此 `event` 参数指示发生的权利事件； `data` parameter是与事件相关的值的列表。

   * `setToken(token, resource)`

      * 触发者 [checkAuthorization()](#checkAuthZ) 和 [getAuthorization()](#$getAuthZ) 成功授权查看资源后。
      * 此 `token` 参数是短期媒体令牌； `resource` parameter是用户有权查看的内容。

   * `tokenRequestFailed(resource, code, description)` </br>
      * 触发者 [checkAuthorization()](#checkAuthZ) 和 [getAuthorization()](#$getAuthZ) 授权失败后。
      * 此 `resource` 参数是用户尝试查看的内容； `code` 参数是指示发生何种类型的故障的错误代码； `description` 参数描述与错误代码关联的错误。

   * `selectedProvider(mvpd)` </br>
      * 触发者 [`getSelectedProvider()`](#getSelProv).
      * 此 `mvpd` 参数提供有关用户选择的提供程序的信息。

   * `setMetadataStatus(metadata, key, arguments)`
      * 触发者 `getMetadata().`
      * 此 `metadata` 参数提供您请求的特定数据； `key` 参数是中使用的键 [getMetadata()](#getMeta) 请求；以及 `arguments` 参数是传递到的相同字典 [getMetadata()](#getMeta).

   * [&#39;preauthorizedResources(authorizedResources)&#39;](#preauthResources)

      * 触发者 [`checkPreauthorizedResources()`](#checkPreauth).

      * 此 `authorizedResources` 参数表示用户有权查看的资源。

   * [&#39;presentTvProviderDialog(viewController)&#39;](#presentTvDialog)

      * 触发者 [getAuthentication()](#getAuthN) 当前请求者至少支持具有SSO支持的MVPD时。
      * viewController参数是Apple SSO对话框，需要显示在主视图控制器上。

   * [&#39;dissistTvProviderDialog(viewController)&#39;](#dismissTvDialog)

      * 由用户操作触发(通过从Apple SSO对话框中选择“取消”或“其他电视提供商”)。
      * viewController参数是Apple SSO对话框，需要从主视图控制器中取消。

![](assets/iOS-flows.png)

### B.启动流程 {#startup_flow}

1. 启动上层应用程序。</br>
1. 启动Adobe Pass身份验证 </br>

   a.呼叫 [`init`](#$init) 创建Adobe Pass Authentication AccessEnabler的单个实例。
   * **依赖关系：** Adobe Pass身份验证本机iOS/tvOS库(AccessEnabler)

   b.呼叫 `setRequestor()` 确定程序员的身份；传入程序员的 `requestorID` 和（可选）Adobe Pass身份验证端点数组。 对于tvOS，您还需要提供公钥和密钥。 请参阅 [无客户端文档](#create_dev) 以了解详细信息。

   * **依赖关系：** 有效的Adobe Pass身份验证请求者ID(请与Adobe Pass身份验证客户经理合作安排此过程)。

   * **触发器：**
     [setRequestorComplete()](#$setReqComplete) 回调。

   >[!NOTE]
   >
   >在完全建立请求者身份之前，无法完成任何授权请求。 这实际上意味着 [`setRequestor()`](#$setReq)  仍在运行，所有后续权利请求。 例如， [`checkAuthentication()`](#checkAuthN) 被阻止。

   您有两个实施选项：将请求者标识信息发送到后端服务器后，UI应用程序层可以选择以下两种方法之一： </br>

   1. 等待触发 [`setRequestorComplete()`](#setReqComplete) 回调（AccessEnabler委托的一部分）。 此选项最确信 [`setRequestor()`](#$setReq) 已完成，因此建议对大多数实施使用此功能。

   1. 无需等待触发即可继续 [`setRequestorComplete()`](#setReqComplete) 回调，并开始发布权利请求。 这些调用(checkAuthentication、checkAuthorization、getAuthorization、getAuthorization、checkPreauthorizedResource、getMetadata、logout)将由AccessEnabler库排队，该库将在 [`setRequestor()`](#$setReq). 例如，如果网络连接不稳定，则此选项有时可能会中断。

1. 调用 `checkAuthentication()` 检查现有身份验证，而不启动完整的身份验证流程。  如果此调用成功，您可以直接进入授权流程。 如果没有，请继续进入身份验证流程。

   * **依赖关系：** 成功调用了 [setRequestor()](#$setReq) （此依赖关系也适用于所有后续调用）。

   * **触发器：** [setAuthenticationStatus()](#$setAuthNStatus) 回调。


### C.没有Apple SSO的身份验证流程 {#authn_flow_wo_applesso}

1. 调用 [`getAuthentication()`](#$getAuthN) 启动身份验证流程，或获取用户已进行身份验证的确认。

   **触发器：**

   * 此 [setAuthenticationStatus()](#$setAuthNStatus) 回调（如果用户已经过身份验证）。 在此情况下，请直接转到 [授权流程](#authz_flow).

   * 此 [displayProviderDialog()](#$dispProvDialog) 回调（如果用户尚未进行身份验证）。

1. 向用户显示已发送到的提供商列表
   [`displayProviderDialog()`](#dispProvDialog).

1. 用户选择提供程序后，请从获取用户MVPD的URL `navigateToUrl:` 或 `navigateToUrl:useSVC:` 回调并打开 `UIWebView/WKWebView` 或 `SFSafariViewController` 控制器并将该控制器定向到URL。

1. 通过 `UIWebView/WKWebView` 或 `SFSafariViewController` 在上一步中实例化后，用户登陆MVPD的登录页面并输入登录凭据。 在控制器内执行了若干重定向操作。</br>

>[!NOTE]
>
>此时，用户可以取消身份验证流程。 如果发生这种情况，您的UI层负责通过调用，向AccessEnabler通知此事件 [setSelectedProvider()](#setSelProv) 替换为 `null` 作为参数。 这允许AccessEnabler清理其内部状态并重置身份验证流程。

1. 用户成功登录后，应用程序层即会检测特定自定义URL的加载。 请注意，此特定自定义URL实际上无效，控制器不会将其实际加载。 应用程序必须将其解释为验证流程已完成，并且关闭是安全的信号。 `UIWebView/WKWebView` 或 `SFSafariViewController` 控制器。 如果是 `SFSafariViewController`需要使用控制器，特定的自定义URL由 **`application's custom scheme`** (例如`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`)，否则此特定自定义URL将由 **`ADOBEPASS_REDIRECT_URL`** 常量(即 `adobepass://ios.app`)。

1. 关闭UIWebView/WKWebView或SFSafariViewController控制器并调用AccessEnabler的 `handleExternalURL:url` API方法，指示AccessEnabler从后端服务器检索身份验证令牌。

1. （可选）调用 [`checkPreauthorizedResources(resources)`](#$checkPreauth) 以检查用户有权查看哪些资源。 此 `resources` parameter是与用户的身份验证令牌关联的受保护资源数组。 从用户的MVPD获得的授权信息的一个用途是装饰您的UI（例如，受保护内容旁边的锁定/解锁符号）。

   * **触发器：** [`preauthorizedResources()`](#preauthResources) callback
   * **执行点：** 完成身份验证流程后

1. 如果身份验证成功，请转到授权流。

### D.使用iOS上的Apple SSO的身份验证流程 {#authn_flow_with_applesso}

1. 调用 [`getAuthentication()`](#$getAuthN) 启动身份验证流程，或获取用户已进行身份验证的确认。
   **触发器：**

   * 此 [presentTvProviderDialog()](#presentTvDialog) 回调（如果用户未经身份验证，并且当前请求者至少具有支持SSO的MVPD）。 如果没有任何MVPD支持SSO，将使用经典身份验证流程。

1. 用户选择提供商后，AccessEnabler库将获取包含Apple VSA框架所提供信息的身份验证令牌。

1. 此 [setAuthenticationStatus()](#setAuthNStatus) 将触发回调。 此时，用户应通过Apple SSO进行身份验证。

1. [可选] 调用 [`checkPreauthorizedResources(resources)`](#$checkPreauth) 以检查用户有权查看哪些资源。 此 `resources` parameter是与用户的身份验证令牌关联的受保护资源数组。 从用户的MVPD获得的授权信息的一个用途是装饰您的UI（例如，受保护内容旁边的锁定/解锁符号）。

   * **触发器：** [`preauthorizedResources()`](#preauthResources) callback
   * **执行点：** 完成身份验证流程后

1. 如果身份验证成功，请转到授权流。

### E.使用tvOS上的Apple SSO的身份验证流程 {#authn_flow_with_applesso_tvOS}

1. 调用 [`getAuthentication()`](#$getAuthN) 启动身份验证流程，或获取用户已进行身份验证的确认。
   **触发器：**
   * 此 [`presentTvProviderDialog()`](#presentTvDialog) 回调（如果用户未经身份验证，并且当前请求者至少具有支持SSO的MVPD）。 如果没有任何MVPD支持SSO，将使用经典身份验证流程。

1. 用户选择提供商后， [`status()`](#status_callback_implementation) 将调用callback。 将提供注册码，AccessEnabler库将开始轮询服务器以获得成功的第二屏身份验证。

1. 如果提供的注册码用于在第二个屏幕上成功进行身份验证， [`setAuthenticatiosStatus()`](#setAuthNStatus) 将触发回调。 此时，用户应通过Apple SSO进行身份验证。
1. [可选] 调用 [`checkPreauthorizedResources(resources)`](#$checkPreauth) 以检查用户有权查看哪些资源。 此 `resources` parameter是与用户的身份验证令牌关联的受保护资源数组。 从用户的MVPD获得的授权信息的一个用途是装饰您的UI（例如，受保护内容旁边的锁定/解锁符号）。

   * **触发器：** [`preauthorizedResources()`](#preauthResources) callback

   * **执行点：** 完成身份验证流程后
1. 如果身份验证成功，请转到授权流。

### F.授权流程 {#authz_flow}

1. 调用 [getAuthorization()](#$getAuthZ) 以启动授权流。

   * **依赖关系：** 与MVPD约定的有效ResourceID。
   * 资源ID应当与在任何其他设备或平台上使用的资源ID相同，并且在MVPD中也将相同。 有关资源ID的信息，请参阅 [识别受保护的资源](/help/authentication/identify-protected-resources.md)

1. 验证身份验证和授权。

   * 如果 [getAuthorization()](#$getAuthZ) 调用成功：用户具有有效的AuthN和AuthZ令牌（用户经过身份验证并有权观看请求的媒体）。

   * 如果 [getAuthorization()](#$getAuthZ) 失败：检查引发的异常以确定其类型（AuthN、AuthZ或其他内容）：
      * 如果这是身份验证(AuthN)错误，则重新启动身份验证流程。
      * 如果是授权(AuthZ)错误，则用户无权观看请求的媒体，并且应向用户显示某种错误消息。
      * 是否存在其他类型的错误（连接错误、网络错误等） 然后向用户显示相应的错误消息。

1. 验证短媒体令牌。\
   使用Adobe Pass身份验证媒体令牌验证器库验证从返回的短期媒体令牌 [getAuthorization()](#$getAuthZ) 调用：

   * 如果验证成功：为用户播放请求的媒体。
   * 如果验证失败：AuthZ令牌无效，应拒绝媒体请求，并向用户显示错误消息。


1. 返回到正常应用程序流程。

### G.查看媒体流 {#media_flow}

1. 用户选择要查看的媒体。
1. 媒体是否受保护？ 您的应用程序会检查所选媒体是否受保护：

   * 如果所选媒体受保护，您的应用程序将启动 [授权流程](#authz_flow) 以上。

   * 如果所选媒体未受保护，则为用户播放该媒体。

### H.没有Apple SSO的注销流程 {#logout_flow_wo_AppleSSO}

1. 调用 [`logout()`](#$logout) 以注销用户。 AccessEnabler清除所有缓存的值和令牌。 清除缓存后，AccessEnabler会进行服务器调用以清除服务器端会话。 请注意，由于服务器调用可能会导致到IdP的SAML重定向（这允许IdP端的会话清理），因此此调用必须遵循所有重定向。 因此，必须在UIWebView/WKWebView或SFSafariViewController控制器中处理此调用。

   a.遵循与身份验证工作流相同的模式，AccessEnabler域通过 `navigateToUrl:` 或 `navigateToUrl:useSVC:` 回调，用于创建UIWebView/WKWebView或SFSafariViewController控制器并指示其加载回调中提供的URL `url` 参数。 这是后端服务器上注销端点的URL。

   b.您的应用程序必须监控 `UIWebView/WKWebView or SFSafariViewController` 并检测在加载特定自定义URL时经过多次重定向的时刻。 请注意，此特定自定义URL实际上无效，控制器不会将其实际加载。 应用程序必须将其解释为注销流程已完成，并且关闭 `UIWebView/WKWebView` 或 `SFSafariViewController` 控制器。 当控制器加载该特定自定义URL时，应用程序必须关闭 `UIWebView/WKWebView or SFSafariViewController` 控制器和调用AccessEnabler的 `handleExternalURL:url`API方法。 如果是 `SFSafariViewController`需要使用控制器，特定的自定义URL由 **`application's custom scheme`** (例如， `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`)，否则此特定自定义URL将由 **`ADOBEPASS_REDIRECT_URL`**  常量(即 `adobepass://ios.app`)。

   >[!NOTE]
   >
   >注销流与身份验证流的不同之处在于，用户无需以任何方式与UIWebView/WKWebView或SFSafariViewController进行交互。 UI应用层使用UIWebView/WKWebView或SFSafariViewController来确保遵循所有重定向。 因此，可以（并且建议）在注销过程中使控制器不可见（即隐藏）。


### I.使用Apple SSO的注销流程 {#logout_flow_with_AppleSSO}

1. 调用 [`logout()`](#$logout) 以注销用户。
1. 此 [status()](#status_callback_implementation) 将使用ID VSA203调用callback。
1. 还应指示用户从系统设置登录。 如果失败，则会在应用程序重新启动时重新进行身份验证。



<!--
### Related Information {#related}


- [iOS API Reference](#)

- [iOS Technical Overview](#)

- [Generating Digital Certificates](#)

- [Identifying Protected Resources](#)

- [Handling MVPDs with 'Not Trusted Certificates' in Adobe Pass
  authentication native SDK (Tech Note)](#)

- [iOS Authentication error - adobepass.ios.app cannot be found (Tech
  Note)](#)
-->
