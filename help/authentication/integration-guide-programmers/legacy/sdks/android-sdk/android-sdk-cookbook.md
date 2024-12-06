---
title: Android SDK指南
description: Android SDK指南
exl-id: 7f66ab92-f52c-4dae-8016-c93464dd5254
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1681'
ht-degree: 0%

---

# Android SDK指南 {#android-sdk-cookbook}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

</br>

## 简介 {#intro}

本文档介绍了权利工作流，程序员的高级别应用程序可以通过Android AccessEnabler库公开的API实施这些工作流。


适用于Android的Adobe Pass身份验证权利解决方案最终分为两个域：

- UI域 — 这是上层应用程序层，用于实施UI并使用AccessEnabler库提供的服务来提供对受限内容的访问。
- AccessEnabler域 — 这是权利工作流的实施形式：
   - 向Adobe后端服务器发出的网络调用
   - 与身份验证和授权工作流相关的业务逻辑规则
   - 管理各种资源和处理工作流状态（如令牌缓存）

AccessEnabler域的目标是隐藏授权工作流的所有复杂内容，并（通过AccessEnabler库）向上层应用程序提供一组用于实施授权工作流的简单授权基元：

1. 设置请求者身份。

1. 检查并获取针对特定身份提供者的身份验证。

1. 检查并获取特定资源的授权。

1. 注销。

AccessEnabler的网络活动发生在不同的线程中，因此从不阻止UI线程。 因此，两个应用程序域之间的双向通信渠道必须遵循完全异步模式：

- UI应用程序层通过AccessEnabler库公开的API调用，将消息发送到AccessEnabler域。
- AccessEnabler通过AccessEnabler协议（UI层向AccessEnabler库注册）中包含的回调方法来响应UI层。

## 权利流 {#entitlement}

1. [先决条件](#prereqs)
1. [启动流程](#startup_flow)
1. [身份验证流程](#authn_flow)
1. [授权流程](#authz_flow)
1. [查看媒体流](#media_flow)
1. [注销流程](#logout_flow)

### A.先决条件 {#prereqs}

1. 创建回调函数：
   - [&#39;setRequestorComplete()&#39;](#$setRequestorComplete)

     由`setRequestor()`触发，返回成功或失败。\
     成功表示您可以继续权利调用。

   - [displayProviderDialog(mvpd)](#$displayProviderDialog)

     仅当用户尚未选择提供程序(MVPD)且尚未进行身份验证时，才由`getAuthentication()`触发。\
     `mvpds`参数是用户可用的提供程序数组。

   - [&#39;setAuthenticationStatus(status， errorcode)&#39;](#$setAuthNStatus)

     每次由`checkAuthentication()`触发。\
     仅当用户已验证并已选择提供程序时，才由`getAuthentication()`触发。

     返回的状态是成功或失败，错误代码描述失败的类型。

   - [navigateToUrl(url)](#$navigateToUrl)

     在用户选择MVPD后由`getAuthentication()`触发。 `url`参数提供MVPD登录页的位置。

   - [&#39;sendTrackingData(event， data)&#39;](#$sendTrackingData)

     由`checkAuthentication(), getAuthentication(), checkAuthorization(), getAuthorization(), setSelectedProvider()`触发。\
     `event`参数指示发生的授权事件；`data`参数是与事件相关的值列表。

   - [&#39;setToken(token， resource)&#39;](#$setToken)

     成功授权查看资源后由`checkAuthorization()`和`getAuthorization()`触发。\
     `token`参数是短期媒体令牌；`resource`参数是用户有权查看的内容。

   - [&#39;tokenRequestFailed(resource， code， description)&#39;](#$tokenRequestFailed)

     在授权失败后由`checkAuthorization()`和`getAuthorization()`触发。\
     `resource`参数是用户尝试查看的内容；`code`参数是指示所发生失败类型的错误代码；`description`参数描述与错误代码关联的错误。

   - [&#39;selectedProvider(mvpd)&#39;](#$selectedProvider)

     由`getSelectedProvider()`触发。\
     `mvpd`参数提供有关用户选择的提供程序的信息。

   - [&#39;setMetadataStatus(metadata， key， arguments)&#39;](#$setMetadataStatus)

     由`getMetadata().`触发\
     `metadata`参数提供您请求的特定数据；`key`参数是`getMetadata()`请求中使用的键；`arguments`参数是传递给`getMetadata()`的同一词典。

   - [&#39;预授权资源（资源）&#39;](#$preauthResources)

     由`checkPreauthorizedResources()`触发。\
     `authorizedResources`参数表示用户有权查看的资源。


![](../../../../assets/android-entitlement-flows.png)


### B.启动流程 {#startup_flow}

1. 启动上层应用程序。
1. 启动Adobe Pass身份验证

   a.调用[`getInstance`](#$getInstance)以创建Adobe Pass Authentication AccessEnabler的单个实例。

   - **依赖项：** Adobe Pass身份验证（本机）
Android Library (AccessEnabler)

   b.调用` setRequestor()`以建立程序员的标识；传入程序员的`requestorID`和（可选）Adobe Pass身份验证终结点数组。

   - **依赖项：**&#x200B;有效的Adobe Pass身份验证请求者ID\
     (请与您的Adobe Pass身份验证客户经理合作安排此操作。)

   - **触发器：** setRequestorComplete()回调

   | 注意 |     |
   | --- | --- |  
   | ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/icons/1313859077_lightbulb.png) | 在完全建立请求者身份之前，无法完成任何授权请求。 这实际上意味着，在setRequestor()仍在运行时，所有后续的权利请求（例如，`checkAuthentication()`）都将被阻止。<br><br>您有两个实施选项：将请求者标识信息发送到后端服务器后，UI应用层可以选择以下两种方法之一：<br><br>1.  等待`setRequestorComplete()`回调的触发（AccessEnabler委托的一部分）。  此选项可让您最确定已完成`setRequestor()`，因此建议对大多数实施使用此选项。<br>2。  继续而不等待`setRequestorComplete()`回调的触发，并开始发出授权请求。 这些调用(checkAuthentication、checkAuthorization、getAuthorization、getAuthorization、checkPreauthorizedResource、getMetadata、logout)由AccessEnabler库排队，该库将在`setRequestor(). `后进行实际的网络调用。例如，如果网络连接不稳定，此选项有时可能会中断。 |

1. 调用[checkAuthentication()](#$checkAuthN)以检查现有身份验证，而不启动完整的身份验证流程。   如果此调用成功，您可以直接进入授权流程。  如果没有，请继续进入身份验证流程。

   - **依赖项：**&#x200B;成功调用`setRequestor()`（此依赖项也适用于所有后续调用）。

   - **触发器：** setAuthenticationStatus()回调

### C.身份验证流程 {#authn_flow}

1. 调用[`getAuthentication()`](#$getAuthN)以启动身份验证流程，或获取用户已进行身份验证的确认。\
   **触发器：**
   - setAuthenticationStatus()回调（如果用户已经过身份验证）。  在这种情况下，请直接进入[授权流](#authz_flow)。
   - displayProviderDialog()回调（如果用户尚未经过身份验证）。

1. 向用户显示发送给`displayProviderDialog()`的提供程序列表。

1. 用户选择提供程序后，从`navigateToUrl()`回调中获取用户MVPD的URL。  打开WebView并将该WebView控件定向到URL。

1. 通过上一步中实例化的WebView，用户登陆MVPD的登录页面并输入登录凭据。 在WebView中执行了若干重定向操作。

   **注意：**&#x200B;此时，用户有机会取消身份验证流程。 如果发生这种情况，您的UI层将负责使用`null`作为参数调用`setSelectedProvider()`，以便向AccessEnabler通知此事件。 这允许AccessEnabler清理其内部状态并重置身份验证流程。

1. 用户成功登录后，应用层将检测“自定义重定向URL”（即： `http://adobepass.android.app`）的加载。 此自定义URL实际上是一个无效URL，不适用于WebView加载。 它是身份验证流程已完成，并且需要关闭WebView的信号。

1. 关闭WebView控件并调用`getAuthenticationToken()`，这会指示AccessEnabler从后端服务器检索身份验证令牌。

1. [可选]调用[`checkPreauthorizedResources(resources)`](#$checkPreauth)以检查用户有权查看哪些资源。 `resources`参数是与用户的身份验证令牌关联的受保护资源的数组。\
   **触发器：** `preAuthorizedResources()`回调\
   完成身份验证流程后&#x200B;**执行点：**

1. 如果身份验证成功，请转到授权流。



### D.授权流程 {#authz_flow}

1. 调用[getAuthorization()](#$getAuthZ)以启动授权
流量。

   依赖项：与MVPD商定的有效ResourceID。

   **注意：** ResourceID应该与任何其他设备或平台上使用的相同，并且在MVPD中应该相同。

1. 验证身份验证和授权。

   - 如果`getAuthorization()`调用成功：用户具有有效的AuthN和AuthZ令牌（用户已通过身份验证并有权观看请求的媒体）。
   - 如果`getAuthorization()`失败：检查引发的异常以确定其类型（AuthN、AuthZ或其他内容）：
      - 如果这是身份验证(AuthN)错误，则重新启动身份验证流程。
      - 如果是授权(AuthZ)错误，则用户无权观看请求的媒体，并且应向用户显示某种错误消息。
      - 如果出现其他类型的错误（连接错误、网络错误等），则向用户显示相应的错误消息。

1. 验证短媒体令牌。\
   使用Adobe Pass身份验证媒体令牌验证器库验证从上述`getAuthorization()`调用返回的短期媒体令牌：

   - 如果验证成功：为用户播放请求的媒体。
   - 如果验证失败：AuthZ令牌无效，应拒绝媒体请求，并向用户显示错误消息。

1. 返回到正常应用程序流程。

### E.查看媒体流 {#media_flow}

1. 用户选择要查看的媒体。
2. 媒体是否受保护？  您的应用程序会检查所选媒体是否受保护：
- 如果所选媒体受到保护，您的应用程序将启动上面的[授权流](#authz_flow)。
- 如果所选媒体未受保护，则为用户播放该媒体。



### F.注销流程 {#logout_flow}

1. 调用[`logout()`](#$logout)以注销用户。\
   AccessEnabler将为当前请求者和具有单点登录的请求者清除当前MVPD的所有缓存值和令牌。 清除缓存后，AccessEnabler会进行服务器调用以清除服务器端会话。  请注意，由于服务器调用可能会导致到IdP的SAML重定向（这允许IdP端的会话清理），因此此调用必须遵循所有重定向。 因此，必须在WebView控件中处理此调用。

   a.遵循与身份验证工作流相同的模式，AccessEnabler域向UI应用程序层发出请求（通过`navigateToUrl()`回调）以创建WebView控件，并指示该控件在后端服务器上加载注销端点的URL。

   b.同样，UI必须监视WebView控件的活动，并检测控件在经历多次重定向时加载应用程序的自定义URL（即： `http://adobepass.android.app/`）的时间。 发生此事件后，UI应用程序层会关闭WebView并完成注销过程。

   **注意：**&#x200B;注销流程与身份验证流程的不同之处在于，用户不需要以任何方式与WebView交互。 UI应用程序层使用WebView来确保遵循所有重定向。 因此，可以（并且建议）在注销过程中使WebView控件不可见（即隐藏）。



### 使用多个MVPD登录和注销的用户流 {#user_flows}

[此处](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/AndroidSSOUserFlows.pdf)您有一份文档描述了使用多个MVPD时的行为，以及用户从应用程序注销时发生的情况。

使用Android SDK版本>= 2.0.0时，可以使用此描述的行为。
