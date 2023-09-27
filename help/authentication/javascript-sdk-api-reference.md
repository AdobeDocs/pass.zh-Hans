---
title: JavaScript SDK API参考
description: JavaScript SDK API参考
exl-id: 48d48327-14e6-46f3-9e80-557f161acd8a
source-git-commit: 4eb5fc1eb1eea4c5e27bbee298db4b9b4ba2daef
workflow-type: tm+mt
source-wordcount: '2836'
ht-degree: 0%

---

# JavaScript SDK API参考 {#javascript-sdk-api-reference}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## API参考 {#api-reference}

这些函数启动与MVPD交互的请求。 所有调用都是异步调用；您必须实施 [回调](#callbacks) 处理响应：

- [setRequestor()](#setReq)
- [getAuthorization()](#getAuthZ)
- [getAuthentication()](#getAuthN)
- [checkAuthentication()](#checkAuthN)
- [checkAuthorization()](#checkAuthZ)
- [checkPreauthorizedResources()](#checkPreauthRes)
- [getMetadata()](#getMeta)
- [setSelectedProvider()](#setSelProv)
- [logout()](#logout)


## setRequestor（inRequestorID、端点、选项）{#setrequestor(inRequestorID,endpoints,options)}

**描述：** 标识请求源自的站点。  在通信会话中，必须在任何其他API调用之前进行此调用。

**参数：**

- *Inrequestorid* -Adobe在注册期间分配给原始站点的唯一标识符。

- *端点*  — 此参数是可选的。 它可以是以下值之一：

   - 一个数组，允许您为Adobe提供的身份验证和授权服务指定端点（其他实例可用于调试目的）。 如果提供了多个URL，则MVPD列表由来自所有服务提供商的端点组成。 每个MVPD都与最快的服务提供程序相关联；即首先响应并支持该MVPD的服务提供程序。 默认情况下（如果未指定值），将使用Adobe服务提供程序(<http://sp.auth.adobe.com/>)。

  示例：
   - `setRequestor("IFC", ["http://sp.auth-dev.adobe.com/adobe-services"])`

- *options* - JSON对象，其中包含应用程序ID值、访客ID值无刷新设置（后台登录注销）和MVPD设置(iFrame)。 所有值都是可选的。
   1. 如果指定，将在库执行的所有网络调用中报告Experience CloudvisitorID。 该值以后可用于高级分析报表。
   2. 如果指定了应用程序的唯一标识符 — `applicationId`  — 此值将作为X-Device-Info HTTP标头的一部分添加到应用程序发出的所有后续调用中。 此值以后可以从 [ESM](/help/authentication/entitlement-service-monitoring-overview.md) 使用正确的查询创建报表。

  **注意：** 所有JSON键都区分大小写。

  示例：

```JSON
   setRequestor("IFC", {
      "visitorID": "THE_ECID_VALUE",
      "applicationId": "APP_ID_VALUE"
  })
```

- 通过指定登录是否需要iFrame，程序员可以覆盖在Adobe Pass身份验证中配置的MVPD设置(*iFrameRequired* 键)和iFrame尺寸(*iFrameWidth* 和 *iFrameHeight* 键)。 JSON对象具有以下模板：

```JSON
    {  
       "visitorID": <string>,
       "backgroundLogin": <boolean>,
       "backgroundLogout": <boolean>,
       "mvpdConfig":{  
          "MVPD_ID_1":{  
             "iFrameRequired": <boolean>,
             "iFrameWidth": <integer>,
             "iFrameHeight": <integer>
          },
          ...
          "MVPD_ID_N":{  
             "iFrameRequired": <boolean>,
             "iFrameWidth": <integer>,
             "iFrameHeight": <integer>
          }
       }
    }
```


上述模板中的所有顶级键均为可选键，且具有默认值(*backgroundlogin*， *backgroundLogut*&#x200B;默认为false，而mvpdConfig为null — 表示不覆盖任何MVPD设置)。


- **注意**：为上述参数指定无效值/类型将导致未定义的行为。



以下是以下方案的示例配置：激活无刷新登录和注销，将MVPD1更改为全页重定向登录（非iFrame），并将MVPD2更改为iFrame登录(width=500 and height=300)：

```JSON
    {  
       "backgroundLogin": true,
       "backgroundLogout": true,
       "mvpdConfig":{  
          "MVPD1":{  
             "iFrameRequired": false
          },
          "MVPD2":{  
             "iFrameRequired": true,
             "iFrameWidth": 500,
             "iFrameHeight": 300
          }
       }
    }
```


**触发的回调：** [setConfig()](#setconfigconfigxml-setconfigconfigxml)
</br>

[返回页首](#top)

</br>

## getAuthorization(inResourceID， redirect_url) {#getauthorization(inresourceid,redirect_url)}

**描述：** 请求指定资源的授权。 每次客户尝试访问可授权资源时，均调用此函数以从Access Enabler获取短期授权令牌。 资源ID与提供授权的MVPD协商一致。

使用当前客户的缓存身份验证令牌。 如果未找到此类令牌，则先启动身份验证过程，然后继续授权。

**参数：**

- `inResourceID`  — 用户请求授权的资源的ID。
- `redirect_url`  — 可以选择提供重定向URL，以便MVPD的授权进程将用户返回到该页面，而不是启动授权的页面。


**触发的回调：** [setToken()](#settokeninrequestedresourceid-intoken-settokeninrequestedresourceidintoken) 成功时， [tokenRequestFailed](#tokenrequestfailedinrequestedresourceid-inrequesterrorcode-inrequestdetailederrormessage-tokenrequestfailedinrequestedresourceidinrequesterrorcodeinrequestdetailederrormessage) 失败时

>[!CAUTION]
>
>请尽量使用checkAuthorization()而不是getAuthorization()。 getAuthorization()方法将启动一个完整的身份验证流程（如果用户未经过身份验证），这可能会导致程序员端的复杂实施。

</b>

[返回页首](#top)

</br>

## getAuthentication(redirect_url) {#getauthentication(redirect_url}

**描述：** 请求当前客户的身份验证。 通常为响应单击登录按钮而调用。 检查当前客户的缓存身份验证令牌。 如果未找到此类令牌，则启动身份验证过程。 这将调用默认或自定义提供程序选择对话框，然后使用选定的提供程序重定向到MVPD的登录界面。

成功后，为用户创建并存储身份验证令牌。 如果身份验证失败，提供程序会向您的服务器返回相应的错误消息， [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode) 回调。

**参数：**

- redirect_url — 可以选择提供重定向URL，以便MVPD的身份验证进程将用户返回到该页面，而不是启动身份验证的页面。

**触发的回调：** [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode)， [displayProviderDialog()](#displayproviderdialogproviders-displayproviderdialogproviders)， [sendTrackingData()](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata)

</br>

[返回页首](#top)

</br>

## checkAuthN {#checkauthn}

**描述：** 检查当前客户的当前身份验证状态。  未与任何UI关联。

**触发的回调：** [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode)

</br>

[返回页首](#top)

</br>

## checkAuthorization(inResourceID) {#checkauthorization(inresourceid)}

**描述：** 应用程序使用此方法来检查当前客户和给定资源的授权状态。 首先检查身份验证状态。 如果未经过身份验证，则触发tokenRequestFailed()回调，此方法将退出。 如果用户通过了身份验证，它还会触发授权流。 欲知详情，请参阅 [getAuthorization()](#getAuthZ方法。

>[!TIP]
>
> **使用检查状态函数**  在请求授权之前，您不需要检查身份验证或授权状态。 例如，您可以调用这些函数来更新您自己的状态显示。 在需要任何用户交互时不要使用它们。

**参数：**

- `inResourceID`  — 用户请求授权的资源的ID。


**触发的回调：**
[setToken()](#settokeninrequestedresourceid-intoken-settokeninrequestedresourceidintoken)， [tokenRequestFailed()](#tokenrequestfailedinrequestedresourceid-inrequesterrorcode-inrequestdetailederrormessage-tokenrequestfailedinrequestedresourceidinrequesterrorcodeinrequestdetailederrormessage)， [sendTrackingData()](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata)， [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode)

</br>

## checkPreauthorizedResources(resources) {#checkPreauthorizedResources(resources)}

**描述：** 请求资源列表的“预检”授权状态。

**参数：**

- *资源*：resources参数是应检查其授权的资源数组。 列表中的每个元素都应该是一个表示资源ID的字符串。 资源ID与中的资源ID须遵守相同的限制 `getAuthorization()` 调用，即它是程序员与MVPD或媒体RSS片段之间建立的约定值。

</br>

## checkPreauthorizedResources(resources-cache=true) {#checkPreauthorizedResources(resources-cache=true)}

从JS SDK版本4.0开始，提供此API变体


**参数：**

- *资源*：resources参数是应检查其授权的资源数组。 列表中的每个元素都应该是一个表示资源ID的字符串。 资源ID与中的资源ID须遵守相同的限制 `getAuthorization()` 调用，即它是程序员与MVPD或媒体RSS片段之间建立的约定值。

- *缓存*：检查预授权资源时是否使用内部缓存。 这是一个可选参数，默认为 **true**. 如果为true，则其行为与上述API相同，这意味着对此函数的后续调用将使用内部缓存来解析预授权的资源。 传递 **false** 对于此参数，将禁用内部缓存，从而导致每次 **checkPreauthorizedResources** 调用API。

**触发的回调：** [preauthorizedResources()](#preauthorizedresourcesauthorizedresources-preauthorizedresourcesauthorizedresources)

</br>

[返回页首](#top)
</br>

## getMetadata(Key) {#getMetadata}

**描述：** 检索Access Enabler库公开为元数据的信息。

有两种类型的元数据：

- **静态** （身份验证令牌TTL、授权令牌TTL和设备ID）
- **用户元数据** （这包括在身份验证和/或授权流期间从MVPD传递到用户设备的用户特定信息）

**更多信息：** [用户元数据](#UserMetadata)

**参数：**

- *键*：指定所请求元数据的ID：
   - 如果键为 `"TTL_AUTHN",` 然后进行查询以获取身份验证令牌过期时间。

   - 如果键为 `"TTL_AUTHZ"` params是一个数组，其中包含资源id作为字符串，然后进行查询以获取与指定资源关联的授权令牌的过期时间。

   - 如果键为 `"DEVICEID"` 然后进行查询以获取当前设备id。 请注意，此功能默认处于禁用状态，程序员应联系Adobe以获取有关启用和费用的信息。

   - 如果键来自以下用户元数据类型列表，则会将包含相应用户元数据的JSON对象发送到 [`setMetadataStatus()`](#setmetadatastatuskey-encrypted-data-setmetadatastatuskeyencrypteddata) 回调函数：

   - `"zip"`  — 邮政编码

   - `"encryptedZip"`  — 加密的邮政编码

   - `"householdID"`  — 家庭标识符。 在MVPD不支持子帐户的情况下，它将与用户ID相同。

   - `"maxRating"`  — 用户的最大家长评级

   - `"userID"`  — 用户标识符。 如果MVPD支持子帐户，并且用户不是主帐户，则userID将不同于householdID。

   - `"channelID"`  — 用户有权查看的渠道列表

   - `"is_hoh"`  — 标识用户是否为户主的标记

   - `"encryptedZip"`  — 加密的邮政编码

   - `"typeID"`  — 标识用户帐户是否为主/辅助帐户的标志

   - `"primaryOID"`  — 家庭标识符

   - `"postalCode"`  — 类似于邮政编码

   - `"acctID"`  — 帐户ID

   - `"acctParentID"`  — 帐户父级ID

  **注意**：程序员可用的实际用户元数据取决于MVPD提供的内容。  请参阅 [用户元数据](#UserMetadata) 当前可用的用户元数据列表。


例如：

```JSON
    // Assume that a reference to the AccessEnabler has been previously 
    // obtained and stored in the "ae" variable
     
    ae.setRequestor("SITE");
    ae.checkAuthentication();
     
    function setAuthenticationStatus(status, reason) {
        if (status ==  1) {
            //user is authenticated, request metadata
            ae.getMetadata("zip");
            ae.getMetadata("maxRating");
        } else {
            ...
      }
    }
```


**触发的回调：** [setMetadataStatus()](#setmetadatastatuskey-encrypted-data-setmetadatastatuskeyencrypteddata)

</br>

[返回页首](#top)

</br>


## setSelectedProvider(providerid) {#setSelectedProvider}

**描述：** 当用户从提供程序选择UI中选择MVPD以便将提供程序选择发送到Access Enabler时，调用此函数，或者当用户取消您的提供程序选择UI而未选择提供程序时，使用null参数调用此函数。

**触发的回调：**[ setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode)， [sendTrackingData()](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata)

</br>

[返回页首](#top)

</br>

## getSelectedProvider() {#getSelectedProvider}

**描述：** 在“供应商选择”对话框中检索客户选择的结果。 这可以在初次身份验证检查后的任何时间使用。

此函数是异步函数，并将其结果返回给 `selectedProvider()` 回调函数。

- **MVPD** 当前选择的MVPD，如果未选择MVPD，则为空。
- **AE_State** 当前客户为“新用户”、“用户未验证”或“用户已验证”之一进行验证的结果

**触发的回调：** [selectedProvider()](#getselectedprovider-getselectedprovider)

</br>

[返回页首](#top)

</br>

## 注销 {#logout}

**描述：** 注销当前客户，清除该用户的所有身份验证和授权信息。 从客户系统中删除所有authN和authZ令牌。

**触发的回调：** [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode)
</br>

[返回页首](#top)

</br>

## 回调定义 {#calllback-definitions}

您必须实施这些回调以处理对异步请求调用的响应：

- [entitlementLoaded()](#entitlementloaded-entitlementloaded)
- [setConfig()](#setconfigconfigxml-setconfigconfigxml)
- [displayProviderDialog()](#displayproviderdialogproviders-displayproviderdialogproviders)
- [createIFrame()](#createiframe-createiframeinwidthminheight)
- [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode)
- [sendTrackingData()](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata)
- [setToken()](#settokeninrequestedresourceid-intoken-settokeninrequestedresourceidintoken)
- [tokenRequestFailed()](#tokenrequestfailedinrequestedresourceid-inrequesterrorcode-inrequestdetailederrormessage-tokenrequestfailedinrequestedresourceidinrequesterrorcodeinrequestdetailederrormessage)
- [preauthorizedResources()](#preauthorizedresourcesauthorizedresources-preauthorizedresourcesauthorizedresources)
- [setMetadataStatus()](#setmetadatastatuskey-encrypted-data-setmetadatastatuskeyencrypteddata)
- [selectedProvider()](#selectedproviderresult-selectedprovider)

</br>

## entitlementLoaded() {#entitlementLoaded}

**描述：** 当Access Enabler已完成初始化并准备好接收请求时触发。 实施此回调以了解何时可以使用Access Enabler API开始通信。
</br>

[返回页首](#top)

</br>

## setConfig(configXML) {#setconfig(configXML)}

**描述：** 实施此回调以接收配置信息和MVPD列表。

**参数：**

- *configXML*：保存当前REQUESTOR的配置（包括MVPD列表）的xml对象。


**触发者：** [setRequestor()](#setrequestor-inrequestorid-endpoints-optionssetreq)

</br>

[返回页首](#top)

</br>

## displayProviderDialog(providers) {#displayproviderdialog(providers)}

**描述：** 实施此回调以调用您自己的自定义提供商选择UI。 您的对话框应使用显示名称（和可选徽标）提供客户的选择。 当客户作出选择并取消对话框时，在调用中将所选提供商的关联ID发送到 *setSelectedProvider()*.

**参数：**

- *提供程序*  — 表示所请求MVPD的对象数组：

```JSON
    var mvpd = {
        ID: "someprov",
        displayName: "Some Provider",
        logoURL: "http://www.someprov.com/images/logo.jpg"
    }
```

**触发者：** [getAuthentication()](#getauthenticationredirecturl-getauthenticationredirecturl)， [getAuthorization()](#getauthorizationinresourceid-redirecturl-getauthorizationinresourceidredirecturl)

</br>[返回页首](#top)

</br>

## createIFrame(inWidth， inHeight) {#createIFrame(inWidth,inHeight)}

**描述：** 如果用户选择了需要使用iFrame来显示其身份验证登录页面UI的MVPD，则实施此回调。

**触发者：**[ setSelectedProvider()](#setselectedproviderproviderid-setselectedprovider)

</br> [返回页首](#top)

</br>

## setAuthenticationStatus(isAuthenticated， errorCode) {#set-authn-status-isauthn-error}

**描述：** 实施此回调以接收身份验证状态（1=已通过身份验证或0=未通过身份验证）以及尝试确定身份验证状态时出现的任何错误时的描述性错误消息（检查成功完成时为空字符串）。

>[!NOTE]
> 
>如果您使用的是当前， [高级错误报告](/help/authentication/error-reporting.md) 系统，则可以忽略发送到此函数的errorCode参数。  但是， isAuthenticated标记仍用于跟踪权利流中用户的身份验证状态


**参数：**

- *isAuthenticated*  — 提供身份验证状态： 1 （已通过身份验证）或0 （未通过身份验证）。
- *错误代码*  — 确定身份验证状态时发生的任何错误。 空字符串（如果无）。


**触发者：** [checkAuthentication()](#checkauthn-checkauthn)， [getAuthentication()](#getauthenticationredirecturl-getauthenticationredirecturl)， [checkAuthorization()](#checkauthorizationinresourceid-checkauthorizationinresourceid)

</br>

[返回页首](#top)

</br>

## sendTrackingData(trackingEventType， trackingData) {#sendTrackingData(trackingEventType,trackingData)}

>[!CAUTION]
>
>设备类型和操作系统是通过使用公共Java库(<http://java.net/projects/user-agent-utils>)和用户代理字符串。 请注意，此信息仅以粗略的方式提供，用于按设备类别细分操作量度，但该Adobe对错误结果不承担任何责任。 请相应地使用新功能。

**描述：** 实施此回调可在发生特定事件时接收跟踪数据。 例如，您可以使用它来跟踪有多少用户使用相同的凭据登录。 跟踪当前不可配置。 使用Adobe Pass Authentication 1.6， `sendTrackingData()` 还报告有关设备、 Access Enabler客户端和操作系统类型的信息。 此 `sendTrackingData()` callback保持向后兼容。

- 设备类型的可能值：
   - 计算机
   - 平板电脑
   - 移动设备
   - gameconsole
   - 未知

- Access Enabler客户端类型的可能值：
   - html5
   - ios
   - android


传递事件类型和关联信息的数组。 事件类型包括：

| mvpdSelection | 用户在提供程序选择对话框中选择了MVPD。 |
| ----------------------- | --------------------------------------------------------- |
| authenticationDetection | 身份验证检查已完成。 |
| authorizationDetection | 授权请求已完成。 |

</br>
数据特定于每种事件类型：
</br>

| 事件类型（字符串） | 数据（数组） |
|:--- | :--- |
| mvpdSelection | 0：选定的MVPD |
|  | 1：设备类型 |
|  | 2：Access Enabler客户端类型 |
|  | 3：操作系统 |
| authenticationDetection | 0：令牌请求是否成功(true/false) |
|  | 1： MVPD ID |
|  | 2： GUID |
|  | 3：令牌已在缓存中(true/false) |
|  | 4：设备类型 |
|  | 5：Access Enabler客户端类型 |
|  | 6：操作系统 |
| authorizationDetection | 0：令牌请求是否成功(true/false) |
|  | 1： MVPD ID |
|  | 2： GUID |
|  | 3：令牌已在缓存中(true/false) |
|  | 4：错误 |
|  | 5：详细信息 |
|  | 6：设备类型 |
|  | 7：Access Enabler客户端类型 |
|  | 8：操作系统 |


**触发者：** [checkAuthentication()](#checkauthn-checkauthn)， [getAuthentication()](#getauthenticationredirecturl-getauthenticationredirecturl)， [checkAuthorization()](#checkauthorizationinresourceid-checkauthorizationinresourceid)， [getAuthorization()](#getauthorizationinresourceid-redirecturl-getauthorizationinresourceidredirecturl)

</br>

[返回页首](#top)

</br>

## setToken(inRequestedResourceID， inToken) {#setToken(inRequestedResourceID,inToken)}

**描述：** 实施此回调以接收短期媒体令牌(inToken)以及已发出授权请求或检查授权请求且已成功完成的资源(inRequestedResourceID)的ID。

**触发者：** [checkAuthorization()](#checkAuthZ)， [getAuthorization()](#getAuthZ)
</br>

[返回页首](#top)

</br>

## tokenRequestFailed(inRequestedResourceID， inRequestErrorCode， inRequestDetailedErrorMessage) {#token-request-failed-error-msg}

**描述：** 实施此回调以在授权或检查授权请求失败时发出信号。 可以选择由MVPD用来提供要由程序员显示的自定义消息。

>[!IMPORTANT]
>
>此回调函数是旧版原始Adobe Pass身份验证错误报告系统的一部分。 为了向后兼容，该函数会保留；但是，如果您已经使用当前的高级错误报告系统实施了自己的回调，则根本不需要使用此函数。 较新的错误报告系统提供了有关授权（或其他操作）失败原因的更多详细信息，以及针对每种错误或警告类型的建议操作过程。

**参数：**

- *inRequestedResourceID*  — 一个字符串，提供授权请求中使用的资源ID。
- *inRequestErrorCode*  — 一个字符串，显示Adobe Pass身份验证错误代码，指示失败的原因；可能的值是“用户未经身份验证错误”和“用户未经授权错误”；有关详细信息，请参阅下面的“回调错误代码”。
- *inRequestDetailedErrorMessage*  — 适用于显示的附加描述性字符串。 如果此描述性字符串由于任何原因不可用，Adobe Pass身份验证将发送空字符串 **(“”)**.  MVPD可使用此命令来传递自定义错误消息或与销售相关的消息。 例如，如果订阅者拒绝对资源的授权，则MVPD可以使用 `*inRequestDetailedErrorMessage*` 例如： **“您当前在包中无法访问此渠道。 如果要升级包，请单击\*此处\*。”** 消息由Adobe Pass身份验证通过此回调传递到程序员网站。 然后，程序员可以选择显示或忽略它。 Adobe Pass身份验证还可以使用 `*inRequestDetailedErrorMessage*` 通知程序员可能导致错误的情况。 例如， **“与提供商的授权服务通信时出现网络错误”。**



**触发者：**  [checkAuthorization()](#checkauthorizationinresourceid-checkauthorizationinresourceid)， [getAuthorization()](#getauthorizationinresourceid-redirecturl-getauthorizationinresourceidredirecturl)
</br>

[返回页首](#top)

</br>


## preauthorizedResources(authorizedResources) {#preauthorizedResources(authorizedResources)}

**描述：** 由Access Enabler触发的回调，用于提供在调用后返回的授权资源列表 `checkPreauthorizedResources()`.

**参数：**

- *authorizedresources*：已授权资源的列表。

**触发者：** [checkPreauthorizedResources()](#checkPreauthRes)
</br>

[返回页首](#top)

</br>

## setMetadataStatus（密钥，加密，数据） {#setMetadataStatus(key,encrypted,data)}

**描述：** 由Access Enabler触发的回调，用于提供通过 `getMetadata()` 呼叫。

**更多信息：** [用户元数据](#userMetadata)

**参数：**

- *键（字符串）*：发出请求的元数据的键。
- *encrypted（布尔值）*：表示“value”是否已加密的标记。 如果为“true”，则“value”实际上将是实际值的JSON Web加密表示形式。
- *数据（JSON对象）*：表示元数据的JSON对象。对于简单请求(&#39;`TTL_AUTHN`&#39;， &#39;`TTL_AUTHZ`&#39;， &#39;`DEVICEID`&#39;)，结果是一个字符串（表示身份验证TTL、授权TTL或设备ID）。 在用户元数据请求的情况下，结果可以是表示元数据有效负载的原始或JSON对象。 JSON用户元数据对象的实际结构类似于以下内容：

```JSON
    {
        updated: 1334243471,
        encrypted: ["encryptedProp"],
        data: {
            zip: ["12345", "34567"],
            maxrating: { 
                "MPAA": "PG-13",
                "VCHIP": "TV-Y", 
                "URL": "http://exam.pl/e/manage/ratings"
            },
            householdid: "3456",
            uid: "BgSdasfsdk23/dsaf3+saASesadgfsShggssd=",
            channelID: ["channel-1", "channel-2"]
    }
```


例如：

```JSON
    // Implement the setMetadataStatus() callback
    function setMetadataStatus(key, encrypted, data) {
        if (encrypted) {
            //the metadata value is encrypted
            //needs to be decrypted by the programmer
            data = decrypt(data);
        }
        alert(key + "=" + data);
    }
```


**触发者：** [`getMetadata()`](#getmetadatakey-getmetadata)
</br>
[返回页首](#top)

</br>

## selectedProvider(result) {#selectedProvider(result)}

**描述：** 实施此回调以接收当前选定的MVPD以及封装在 `result` 参数。 此 `result` 参数是一个具有以下属性的对象：

- **MVPD** 当前选择的MVPD，如果未选择MVPD，则为空。
- **AE\_State** 当前用户的身份验证结果，即“新用户”、“用户未身份验证”或“用户已身份验证”之一

**触发者：** [getSelectedProvider()](#getSelProv)

</br>

[返回页首](#top)

</br>

### 回调错误代码 {#callback-error-codes}

| 一般错误 | |
|:--- | :--- | 
| 内部错误 | 尝试处理请求时发生系统错误。 |
| 未选择提供程序错误 | 当客户在提供商选择对话框中取消时发生 |
| 提供程序不可用错误 | 当没有可用的提供程序时发生。 |

| 身份验证错误 | |
|:--- | :--- | 
| 一般身份验证错误 | 当原因未知或无法发布时返回。 |
| 内部身份验证错误 | 尝试进行身份验证时出现系统错误。 |
| 用户未验证错误 | 用户未经过身份验证。 |
| 多个身份验证请求错误 | 在完成第一个身份验证请求之前，已收到其他身份验证请求。 |

| 授权错误 | |
|:--- | :--- | 
| 一般授权错误 | 当原因未知或无法发布时返回。 |
| 内部授权错误 | 尝试授权时出现系统错误。 |
| 用户未授权错误 | 客户无权查看请求的内容。 |

<!--

### Related Information {#Related-Infornation}

* [JavaScript SDK Overview](/help/authentication/javascript-sdk-overview.md)
* [JavaScript SDK Cookbook](/help/authentication/javascript-sdk-cookbook.md)
* **JavaScript Code Samples**
* [Error Reporting](/help/authentication/error-reporting.md)
* [Understanding Tokens](#understanding_tokens)
* **Tracking Data in Adobe Pass Authentication**
-->

[返回页首](#top)
