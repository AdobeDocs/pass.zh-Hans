---
title: Amazon FireOS集成指南
description: Amazon FireOS集成指南
exl-id: 1982c485-f0ed-4df3-9a20-9c6a928500c2
source-git-commit: 1b8371a314488335c68c82882c930b7c19aa64ad
workflow-type: tm+mt
source-wordcount: '1416'
ht-degree: 0%

---

# Amazon FireOS集成指南 {#amazon-fireos-integration-cookbook}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

</br>


## 介绍 {#intro}

本文档介绍了权利工作流，程序员的高级别应用程序可以通过Amazon FireOS公开的API实施这些工作流 `AccessEnabler` 库。

适用于Amazon FireOS的Adobe Pass身份验证权利解决方案最终分为两个域：

- UI域 — 这是上层应用程序层，用于实施UI并使用 `AccessEnabler` 库以提供对受限内容的访问权限。
- 此 `AccessEnabler` 域 — 权利工作流的实施形式为：
   - 向Adobe后端服务器发出的网络调用
   - 与身份验证和授权工作流相关的业务逻辑规则
   - 管理各种资源和处理工作流状态（如令牌缓存）

的目标 `AccessEnabler` 域用于隐藏授权工作流的所有复杂内容，并向上层应用程序提供(通过 `AccessEnabler` 库)一组简单的权利基元。 通过此流程，您可以实施授权工作流：

1. 设置请求者身份。
1. 检查并获取针对特定身份提供者的身份验证。
1. 检查并获取特定资源的授权。
1. 注销。

此 `AccessEnabler`的网络活动发生在不同的线程中，因此从不阻止UI线程。 因此，两个应用程序域之间的双向通信渠道必须遵循完全异步模式：

- UI应用层将消息发送到 `AccessEnabler` 通过公开的API调用的域 `AccessEnabler` 库。
- 此 `AccessEnabler` 通过包含在中的回调方法响应UI层 `AccessEnabler` UI层向注册的协议 `AccessEnabler` 库。

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

      - 触发者 `setRequestor()`，会返回成功或失败。     成功表示您可以继续权利调用。

   - [displayProviderDialog(mvpd)](#$displayProviderDialog)

      - 触发者 `getAuthentication()` 仅当用户尚未选择提供程序(MVPD)且尚未进行身份验证时。 此 `mvpds` parameter是用户可用的提供程序数组。

   - [&#39;setAuthenticationStatus(status， reason)&#39;](#$setAuthNStatus)

      - 触发者 `checkAuthentication()` 每次。 触发者 `getAuthentication()` 仅当用户已进行身份验证并已选择提供程序时。

      - 返回的状态已验证或未验证，原因描述了验证失败或注销操作。

   - [navigateToUrl(url)](#$navigateToUrl)

      - 在AmazonFireOS SDK中忽略，该方法在Android平台上使用，触发平台为 `getAuthentication()` 在用户选择MVPD之后。  此 `url` 参数提供MVPD登录页面的位置。

   - [&#39;sendTrackingData(event， data)&#39;](#$sendTrackingData)

      - 触发者 `checkAuthentication(), getAuthentication(), checkAuthorization(), getAuthorization(), setSelectedProvider()`.
此 `event` 参数指示发生的权利事件； `data` parameter是与事件相关的值的列表。

   - [&#39;setToken(token， resource)&#39;](#$setToken)

      - 触发者 `checkAuthorization()` 和 `getAuthorization()` 成功授权查看资源后。
      - 此 `token` 参数是短期媒体令牌；而 `resource` parameter是用户有权查看的内容。

   - [&#39;tokenRequestFailed(resource， code， description)&#39;](#$tokenRequestFailed)

      - 触发者 `checkAuthorization()` 和 `getAuthorization()` 授权失败后。
      - 此 `resource` 参数是用户尝试查看的内容； `code` 参数是指示发生何种类型的故障的错误代码； `description` 参数描述与错误代码关联的错误。

   - [&#39;selectedProvider(mvpd)&#39;](#$selectedProvider)

      - 触发者 `getSelectedProvider()`.
      - 此 `mvpd` 参数提供有关用户选择的提供程序的信息。

   - [&#39;setMetadataStatus(metadata， key， arguments)&#39;](#$setMetadataStatus)

      - 触发者 `getMetadata().`
      - 此 `metadata` 参数提供您请求的特定数据； `key` 参数是中使用的键 `getMetadata()` 请求；以及 `arguments` 参数是传递到的相同字典 `getMetadata()`.

   - [&#39;预授权资源（资源）&#39;](#$preauthResources)

      - 触发者 `checkPreauthorizedResources()`.
      - 此 `authorizedResources` 参数表示用户有权查看的资源。


![](assets/android-entitlement-flows.png)


### B.启动流程 {#startup_flow}

1. 启动上层应用程序。
1. 启动Adobe Pass身份验证。

   1. 调用 [`getInstance`](#$getInstance) 创建单个Adobe Pass身份验证实例 `AccessEnabler`.

      - **依赖关系：** Adobe Pass Authentication Native Amazon FireOS库(`AccessEnabler`)

   1. 调用` setRequestor()` 建立程序员的身份，传递程序员的身份 `requestorID` 和（可选）Adobe Pass身份验证端点数组。

      - **依赖关系：** 有效的Adobe Pass身份验证请求者ID(请与Adobe Pass身份验证客户经理合作安排此过程。)

      - **触发器：** setRequestorComplete()回调

   在完全建立请求者身份之前，无法完成任何授权请求。 这实际上意味着，在setRequestor()仍在运行时，所有后续权利请求(例如，`checkAuthentication()`)被阻止。

   您有两个实施选项：将请求者标识信息发送到后端服务器后，UI应用程序层可以选择以下两种方法之一：</p>

   1. 等待触发 `setRequestorComplete()` 回调(的一部分 `AccessEnabler` 委派)。  此选项最确信 `setRequestor()` 已完成，因此建议对大多数实施使用此功能。
   1. 无需等待触发即可继续 `setRequestorComplete()` 回调，并开始发布权利请求。 这些调用(checkAuthentication、checkAuthorization、getAuthorization、getAuthorization、checkPreauthorizedResource、getMetadata、logout)由 `AccessEnabler` 库，将在 `setRequestor()`. 例如，如果网络连接不稳定，则此选项有时可能会中断。

1. 调用 [checkAuthentication()](#$checkAuthN) 检查现有身份验证，而不启动完整的身份验证流程。  如果此调用成功，您可以直接进入授权流程。  如果没有，请继续进入身份验证流程。

- **依赖关系：** 成功调用了 `setRequestor()` （此依赖关系也适用于所有后续调用）。

- **触发器：** setAuthenticationStatus()回调

### C.身份验证流程 {#authn_flow}

1. 调用 [`getAuthentication()`](#$getAuthN) 启动身份验证流程，或获取用户已进行身份验证的确认。

   **触发器：**

   - setAuthenticationStatus()回调（如果用户已经过身份验证）。  在此情况下，请直接转到 [授权流程](#authz_flow).
   - displayProviderDialog()回调（如果用户尚未经过身份验证）。

1. 向用户显示已发送到的提供商列表 `displayProviderDialog()`.
1. 用户选择提供程序后，WebView将打开供用户登录的提供程序页面

   >[!NOTE]
   >
   >此时，用户可以取消身份验证流程。 如果发生这种情况， `AccessEnabler` 将清除其内部状态并重置身份验证流程。

1. 用户成功登录后，WebView将关闭。
1. 调用 `getAuthenticationToken(),` 指示 `AccessEnabler` 以从后端服务器检索身份验证令牌。
1. [可选] 调用 [`checkPreauthorizedResources(resources)`](#$checkPreauth) 以检查用户有权查看哪些资源。 此 `resources` parameter是与用户的身份验证令牌关联的受保护资源数组。

   **触发器：** `preAuthorizedResources()` callback\
   **执行点：** 完成身份验证流程后

1. 如果身份验证成功，请转到授权流。


### D.授权流程 {#authz_flow}

1. 调用 [`getAuthorization()`](#$getAuthZ) 以启动授权流。

   依赖项：与MVPD商定的有效ResourceID。

   **注意：** ResourceID应当与在任何其他设备或平台上使用的相同，并且在MVPD中也将相同。

1. 验证身份验证和授权。

   - 如果 `getAuthorization()` 调用成功：用户具有有效的AuthN和AuthZ令牌（用户经过身份验证并有权观看请求的媒体）。
   - 如果 `getAuthorization()` 失败：检查引发的异常以确定其类型（AuthN、AuthZ或其他内容）：
      - 如果这是身份验证(AuthN)错误，则重新启动身份验证流程。
      - 如果是授权(AuthZ)错误，则用户无权观看请求的媒体，并且应向用户显示某种错误消息。
      - 是否存在其他类型的错误（连接错误、网络错误等） 然后向用户显示相应的错误消息。

1. 验证短媒体令牌。

   使用Adobe Pass身份验证媒体令牌验证器库验证从返回的短期媒体令牌 `getAuthorization()` 调用：

   - 如果验证成功：为用户播放请求的媒体。
   - 如果验证失败：AuthZ令牌无效，应拒绝媒体请求，并向用户显示错误消息。

1. 返回到正常应用程序流程。

### E.查看媒体流 {#media_flow}

1. 用户选择要查看的媒体。
1. 媒体是否受保护？  您的应用程序会检查所选媒体是否受保护：
   - 如果所选媒体受保护，您的应用程序将启动 [授权流程](#authz_flow) 以上。
   - 如果所选媒体未受保护，则为用户播放该媒体。

### F.注销流程 {#logout_flow}

1. 调用 [`logout()`](#$logout) 以注销用户。 此 `AccessEnabler` 清除用户通过单点登录共享登录的所有请求者上为当前MVPD获取的所有缓存值和令牌。 清除缓存后， `AccessEnabler` 进行服务器调用以清除服务器端会话。  请注意，由于服务器调用可能会导致到IdP的SAML重定向（这允许IdP端的会话清理），因此此调用必须遵循所有重定向。 因此，此调用将在WebView控件中处理，对用户不可见。

   **注意：** 注销流与身份验证流的不同之处在于，用户不需要以任何方式与WebView交互。 因此，可以（并且建议）在注销过程中使WebView控件不可见（即：隐藏）。
