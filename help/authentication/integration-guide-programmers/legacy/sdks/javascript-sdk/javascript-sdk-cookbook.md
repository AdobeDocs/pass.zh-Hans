---
title: JavaScript SDK指南
description: JavaScript SDK指南
exl-id: d57f7a4a-ac77-4f3c-8008-0cccf8839f7c
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '934'
ht-degree: 0%

---

# JavaScript SDK指南 {#javascript-sdk-cookbook}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## 简介 {#intro}

本文档描述了程序员的高级别应用程序为JavaScript与Adobe Pass身份验证服务的集成实施的授权工作流。 指向JavaScript API引用的链接包括在中。

另请注意，[相关信息](#related)部分包括
链接到一组JavaScript代码示例。

## 权利流 {#entitlement}

1. [先决条件](#prereq)
2. [启动流程](#startup)
3. [身份验证流程](#authn)
4. [授权流程](#authz)
5. [查看媒体流](#logout)

</br>

![](../../../../assets/javascript-flows.png)


## 先决条件 {#prereq}

**依赖项：**

- Adobe Pass身份验证库(AccessEnabler)，请与Adobe Pass身份验证客户经理合作安排此过程。
- 有效的Adobe Pass身份验证请求者ID，请与Adobe Pass身份验证客户经理合作安排此过程。

创建回调函数：

- `entitlementLoaded`
</br>

**触发器：** AccessEnabler已加载并完成初始化。

- `displayProviderDialog(mvpds)`

  **触发器：** `getAuthentication(),`仅当用户尚未选择提供程序(MVPD)且尚未进行身份验证时
mvpds参数是用户可用的提供程序数组。

- `setAuthenticationStatus(status, errorcode)`

  **触发器：**
   - `checkAuthentication()`每次。
   - 仅当用户已经过身份验证并已选择提供程序时，`getAuthentication()`。

  返回的状态是成功或失败；错误代码描述了失败的类型。

- `createIFrame(width, height)`

  **触发器：** `setSelectedProvider(providerID)`，仅当所选提供程序配置为在IFrame中显示时。

  >[!NOTE]
  >
  >提供程序配置为在iFrame中将其身份验证屏幕呈现为重定向或，程序员需要同时考虑这两个因素。

- `sendTrackingData(event, data)`

  **触发器：** `checkAuthentication(), getAuthentication(),checkAuthorization(), getAuthorization(), setSelectedProvider()`。  `event`参数指示发生的授权事件；`data`参数是与事件相关的值列表。
- `setToken(token, resource)`
  成功授权查看资源后，**触发器：** `checkAuthorization()`和`getAuthorization()`。   `token`参数是短期媒体令牌；`resource`参数是用户有权查看的内容。

- `tokenRequestFailed(resource, code, description)`
  授权失败后&#x200B;**触发器：**`checkAuthorization()`&#x200B;和`getAuthorization()`。\
  `resource`参数是用户尝试查看的内容；`code`参数是指示所发生失败类型的错误代码；`description`参数描述与错误代码关联的错误。

- `selectedProvider(mvpd)`

  **触发器：** [`getSelectedProvider()`](#$getSelProv`mvpd`参数提供有关由选择的提供程序的信息
用户。

- `setMetadataStatus(metadata, key, arguments)`

  **触发器：** `getMetadata().`\
  `metadata`参数提供您请求的特定数据；键参数是`getMetadata()`请求中使用的键；`arguments`参数是传递给`getMetadata()`的同一词典。


## 2.启动流程

**I.加载AccessEnabler JavaScript：**

暂存配置文件的&#x200B;****

```JSON
<script type="text/javascript"         
src="https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js">
</script>"
```

或……

生产配置文件&#x200B;**的**

```JSON
<script type="text/javascript"         
src="https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js">
</script>"
```

**触发器：**初始化完成后，Adobe Pass
身份验证调用您的`entitlementLoaded()`回调函数。 这是应用程序与AccessEnabler通信的入口点。


**II。**&#x200B;调用`setRequestor()`以建立
程序员的身份；传入程序员的`requestorID`和
（可选）Adobe Pass身份验证端点数组。

**触发器：**&#x200B;无，但允许在需要时调用`displayProviderDialog()`。


**III。**&#x200B;调用`checkAuthentication()`以检查现有身份验证，而不启动完整的[身份验证流程]。  如果此调用成功，您可以直接转到`authorization flow`。  如果不是，则继续执行`authentication flow`。

**依赖项：**&#x200B;成功调用`setRequestor()`（此依赖项也适用于所有后续调用）。

**触发器：** `setAuthenticationStatus()`回调

</br>

## 3.身份验证流程</span>


**依赖项：**&#x200B;成功调用`setRequestor()`（此依赖项也适用于所有后续调用）。


调用`getAuthentication()`以获取身份验证状态或触发提供程序身份验证流程。

**触发器：**

- 如果用户尚未通过身份验证，`displayProviderDialog()`
- 如果已进行身份验证，则为`setAuthenticationStatus()`

当AccessEnabler使用`isAuthenticated == 1`调用`setAuthenticationStatus()`时到达身份验证流程的完成状态。

## 4.授权流程 {#authz}

**依赖项：**

- 成功调用`setRequestor()`（此依赖项也适用于所有后续调用）。
- 与MVPD约定的有效ResourceID。 请注意，ResourceID应当与在任何其他设备或平台上使用的相同，并且在MVPD中也将相同。

调用`getAuthorization()`并为所请求的媒体传递资源ID。 成功的调用将返回一个短媒体令牌，以确认用户有权查看所请求的媒体。

- 如果调用通过：用户具有有效的AuthN令牌，并且用户有权观看请求的媒体。
- 如果调用失败：检查引发的异常，以确定其类型（AuthN、AuthZ或其他）：
- 如果调用是AuthN错误，则重新启动AuthN流。
- 如果调用是AuthZ错误，则用户无权观看请求的媒体，并且应向用户显示某种错误消息。
- 如果发生其他错误（连接错误、网络错误等），则向用户显示相应的错误消息。

使用媒体令牌验证器验证从成功的`getAuthorization()`调用返回的shortMediaToken。


**依赖关系：**短媒体令牌验证器(包含在
AccessEnabler库)

- 如果验证通过：为用户显示/回放请求的媒体。
- 如果失败：AuthZ令牌无效，应拒绝媒体请求，并向用户显示错误消息。

## 5.查看Media流程 {#logout}

- 用户选择要查看的媒体。
   - 媒体是否受保护？
      - 您的应用程序会检查媒体是否受保护：
         - 如果媒体受到保护，您的应用程序将启动上述授权(AuthZ)流程。
         - 如果媒体未受保护，请继续查看媒体流。
         - 播放媒体

## 配置访客Id {#visitorID}

从Analytics的角度来看，配置[Experience CloudvisitorID](https://experienceleague.adobe.com/docs/id-service/using/home.html)值非常重要。 设置EC visitorID值后，SDK将在每次网络调用时发送此信息，Adobe Pass身份验证服务将收集此信息。 这样，您就可以将来自Adobe Pass身份验证服务的分析数据与来自其他应用程序或网站的任何其他分析报表相关联。 有关如何设置EC visitorID的信息可在[此处](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=en)找到。


>[!NOTE]
>
>请注意，从JS SDK版本3.1.0开始，即可提供此功能支持。

<!--
### Related Information (#related)

* [JavaScript SDK Overview](/help/authentication/javascript-sdk-overview.md)
* [JavaScript SDK API Reference](/help/authentication/javascript-sdk-api-reference.md)
* **JavaScript SDK Code Samples**
-->
