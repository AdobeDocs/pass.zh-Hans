---
title: Android SDK API参考
description: Android SDK API参考
exl-id: f932e9a1-2dbe-4e35-bd60-a4737407942d
source-git-commit: 2ccfa8e018b854a359881eab193c1414103eb903
workflow-type: tm+mt
source-wordcount: '4518'
ht-degree: 0%

---

# Android SDK API参考 {#android-sdk-api-reference}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## 介绍 {#intro}

本文档详细介绍Android SDK为Adobe Pass身份验证公开的方法和回调，Adobe Pass身份验证版本1.7及更高版本支持这些方法和回调。 此处介绍的方法和回调函数在AccessEnabler.h和EntitlementDelegate.h头文件中定义。

请参阅 [https://tve.zendesk.com/hc/en-us/articles/204963219-Android-Native-AccessEnabler-Library](https://tve.zendesk.com/hc/en-us/articles/204963219-Android-Native-AccessEnabler-Library) 获取最新的Android AccessEnabler SDK。


**注意：** Adobe Pass身份验证团队建议您仅使用Adobe Pass身份验证 *公共* API：

- 公共API可用 *经过全面测试* 所有受支持的客户端类型。 对于任何公共功能，我们确保每种客户端类型都有相关方法的相应版本。</span>
- 公共API需要尽可能保持稳定，以支持向后兼容性并确保合作伙伴集成不会中断。 但是，对于 *非*-public API，我们保留在以后任何时候更改其签名的权利。 如果您遇到无法通过当前公共Adobe Pass身份验证API调用组合支持的特定流程，最好的方法是告知我们。 根据您的需求，我们可以修改公共API，并在以后提供稳定的解决方案。

## Android API {#api}

- [getInstance](#getInstance)
- [setOptions](#setOptions)
- [setRequestor](#setRequestor)
- [setRequestorComplete](#setRequestorComplete)
- [checkAuthentication](#checkAuthN)
- [getAuthentication](#getAuthN)
- [displayProviderDialog](#displayProviderDialog)
- [setSelectedProvider](#setSelectedProvider)
- [navigateToUrl](#navigagteToUrl)
- [getAuthenticationToken](#getAuthNToken)
- [setAuthenticationStatus](#setAuthNStatus)
- 预授权
- [checkAuthorization](#checkAuthZ)
- [getAuthorization](#getAuthZ)
- [setToken](#setToken)
- [tokenRequestFailed](#tokenRequestFailed)
- [注销](#logout)
- [getselectedprovider](#getSelectedProvider)
- [selectedprovider](#selectedProvider)
- [getMetadata](#getMetadata)
- [setmetadatastatus](#setMetadaStatus)
- [getVersion](#getVersion)

### Factory.getInstance {#getInstance}

**描述：** 实例化访问启用程序对象。 每个应用程序实例应有一个访问启用程序实例。

| API调用：构造函数 |
| --- |
| **公共静态** AccessEnabler **getInstance**（上下文appContext、字符串softwareStatement、字符串redirectUrl）<br>        **丢弃** AccessEnablerException <br><br>**公共静态** AccessEnabler getInstance(Context appContext， String env_url， String softwareStatement， String redirectUrl)<br>**丢弃** AccessEnablerException |

**可用性：** v3.1.2+

**参数：**

- *appContext*：Android应用程序上下文。
- env\_url：对于使用Adobe暂存环境进行测试，可以将env\_url设置为“sp.auth-staging.adobe.com”

**已弃用：**

```
    public static AccessEnabler getInstance(Context appContext)
        throws AccessEnablerException
```



### setRequestor {#setRequestor}

**描述：** 确定程序员的身份。 在为Adobe Pass身份验证系统注册Adobe时，每个程序员都会分配一个唯一的ID。 在处理SSO和远程令牌时，当应用程序处于后台时，身份验证状态可能会更改，当应用程序进入前台时，可以再次调用setRequestor以便与系统状态同步（如果启用了SSO，则获取远程令牌，如果同时发生注销，则删除本地令牌）。

服务器响应包含MVPD列表以及附加到程序员标识的一些配置信息。 服务器响应由访问启用码在内部使用。 通过setRequestorComplete()回调只向应用程序显示操作的状态（即SUCCESS/FAIL）。

如果 *url* 未使用参数，生成的网络调用将定向默认服务提供商URL：Adobe版本/生产环境。

如果为提供了值， *url* 参数，则生成的网络调用将定向于 *url* 参数。 所有配置请求都在单独的线程中同时触发。 在编译MVPD列表时，优先使用第一个响应程序。 对于列表中的每个MVPD， Access Enabler记住关联服务提供商的URL。 所有后续权利请求都定向到与服务提供程序相关联的URL，该URL在配置阶段与目标MVPD配对。

| API调用：请求者配置 |
| --- |
| ```public void setRequestor(String requestorId)``` |

**可用性：** v3.0+

| API调用：请求者配置 |
| --- |
| ```public void setRequestor(String requestorId, ArrayList<String> urls)``` |
**可用性：** v3.0+


**参数：**

- *请求者ID*：与程序员关联的唯一ID。 首次向Adobe Pass身份验证服务注册时，将Adobe分配的唯一ID传递到您的网站。

- *signedRequestID*：使用您的私钥进行数字签名的请求者ID的副本。 <!--For more details. see [Registering Native Clients](http://tve.helpdocsonline.com/registering-native-clients)-->.

- *url*：可选参数；默认情况下，使用Adobe服务提供程序(http://sp.auth.adobe.com/)。 此阵列允许您为Adobe提供的身份验证和授权服务指定端点（其他实例可能用于调试目的）。 您可以使用此选项指定多个Adobe Pass身份验证服务提供程序实例。 这样做时，MVPD列表由来自所有服务提供商的端点组成。 每个MVPD都与最快的服务提供程序相关联；即首先响应并支持该MVPD的服务提供程序。

**触发的回调：** `setRequestorComplete()`

已弃用：

    公共void setRequestor(String requestorId， String signedRequestorId)
    
    公共void setRequestor (String requestorId， String signedRequestorId， ArrayList&lt;string> url)

[返回到Android API...](#api)

### setRequestorComplete {#setRequestorComplete}

**描述：** 由Access Enabler触发的回调，通知应用程序配置阶段已完成。 这是一个信号，表明应用程序可以开始发出授权请求。 在配置阶段完成之前，应用程序无法发出任何授权请求。

| 回调：请求者配置完成 |
| --- |
| java public void setRequestorComplete(int status) |

**可用性：** v1.0+

**参数：**

- *状态*：可以采用以下值之一：
   - SDK \>= 3.4.0
      - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_SUCCESS`  — 配置阶段已成功完成
      - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_ERROR`  — 配置阶段失败
   - SDK \&lt; 3.4
      - `AccessEnabler.ACCESS_ENABLER_STATUS_SUCCESS`  — 配置阶段已成功完成
      - `AccessEnabler.ACCESS_ENABLER_STATUS_ERROR`  — 配置阶段失败

**触发者：** `setRequestor()`

[返回到Android API...](#api)

### setOptions {#setOptions}

**描述：** 配置全局SDK选项。 它接受 **映射\&lt;string string=&quot;&quot;>** 作为论据。 映射中的值将与SDK发出的每个网络调用一起传递到服务器。

这些值将传递到服务器，与当前流（身份验证/授权）无关。 如果要更改值，可以随时调用此方法。

| API调用： setOptions |
| --- |
| 公共void setOptions(HashMap&lt;string string=&quot;&quot;> options) |

**可用性：** v1.9.2+

**参数：**

- *options*：地图&lt;string string=&quot;&quot;> 包含全局SDK选项。 目前，以下选项可用：
   - **applicationprofile**  — 可用于根据此值进行服务器配置。
   - **ap_vi** -Experience CloudID (visitorID)。 此值以后可用于高级分析报表。
   - **ap_ai**  — 广告ID
   - **device_info**  — 客户端信息，如下所述： [传递客户端信息设备连接和应用程序](/help/authentication/passing-client-information-device-connection-and-application.md).

[返回页首……](#apis)


### checkAuthentication {#checkAuthN}

**描述：** 检查身份验证状态。 它通过在本地令牌存储空间中搜索有效的身份验证令牌来完成此操作。 调用此方法不会执行任何网络调用。 应用程序使用它来查询用户的身份验证状态并相应地更新UI（即更新登录/注销UI）。 身份验证状态通过 [*setAuthenticationStatus()*](#setAuthNStatus) 回调。

如果MVPD支持“每个请求者的身份验证”功能，则可以在设备上存储多个身份验证令牌。  有关此功能的详细信息，请参阅 [缓存准则](#$caching) 部分（位于Android技术概述中）。

| API调用：检查身份验证状态 |
| --- |
| public void checkAuthentication() |

**可用性：** v1.0+

**参数：** 无

**触发的回调：** `setAuthenticationStatus()`

[返回到Android API...](#api)


### getAuthentication {#getAuthN}

**描述：** 启动完整的身份验证工作流。 首先检查身份验证状态。 如果尚未经过身份验证，则身份验证流状态 — 计算机将启动：

- 如果上次身份验证尝试成功，则会跳过MVPD选择阶段，并且 [*navigateToUrl()*](#navigagteToUrl) 会触发回调。 应用程序使用此回调实例化向用户显示MVPD登录页的WebView控件。
- 如果上次验证尝试不成功，或者用户明确注销，则 [*displayProviderDialog()*](#displayProviderDialog) 会触发回调。 您的应用程序使用此回调来显示MVPD选择UI。 此外，您的应用程序需要通过通知Access Enabler库有关用户的MVPD选择，来恢复身份验证流程。 [setSelectedProvider()](#setSelectedProvider) 方法。

在MVPD登录页面上验证用户的凭据时，应用程序需要监视用户在MVPD登录页面上验证时发生的多个重定向操作。 输入正确的凭据后，WebView控件将被重定向到由定义的自定义URL。 *AccessEnabler.ADOBEPASS\_REDIRECT\_URL* 常量。 此URL不打算由WebView加载。 应用程序必须拦截此URL，并将此事件解释为登录阶段已完成的信号。 然后，它应该将控制权移交给Access Enabler，以便完成身份验证流程(通过调用 *getAuthenticationToken()* 方法)。

如果MVPD支持“每个请求者的身份验证”功能，则可以在设备上存储多个身份验证令牌（每个程序员一个）。  有关此功能的详细信息，请参阅 [缓存准则](#$caching) 部分（位于Android技术概述中）。

最后，验证状态通过 *setAuthenticationStatus()* 回调。



| API调用：启动身份验证流程 |
| --- |
| 公共void getAuthentication() |

**可用性：** v1.0+

| API调用：启动身份验证流程 |
| --- |
| 公共void getAuthentication(boolean forceAuthN， Map&lt;string object=&quot;&quot;> genericData) |

**可用性：** v1.8+

**参数：**

- *forceAuthn*：指定是否应启动身份验证流程的标记，无论用户是否已进行身份验证。
- *数据*：包含要发送到Pay-TV pass服务的键值对的映射。 Adobe可以使用此数据启用未来的功能，而无需更改SDK。

**触发的回调：** `setAuthenticationStatus(), displayProviderDialog(), navigateToUrl(), sendTrackingData()`


[返回到Android API...](#api)

### displayProviderDialog {#displayProviderDialog}

**描述** 由Access Enabler触发的回调，用于通知应用程序需要实例化适当的UI元素以允许用户选择所需的MVPD。 回调会提供MVPD对象的列表以及其他有助于正确构建选择UI面板的信息（例如指向MVPD徽标的URL、友好显示名称等）

一旦用户选择了所需的MVPD，则上层应用程序需要通过调用来恢复验证流 *setSelectedProvider()* 并向其传递与用户选择相对应的MVPD的ID。

>[!NOTE]
>
> 中止身份验证流程
> </br></br>
> 请注意，这是用户能够按下“返回”按钮的时刻，这相当于身份验证流中止。 在这种情况下，应用程序需要调用 `setSelectedProvider()` 方法，通过 *null* 作为参数，为Access Enabler提供重置其身份验证状态机的机会。

| 回调：显示MVPD选择UI |
| --- |
| `public void displayProviderDialog(ArrayList<Mvpd> mvpds)` |

**可用性：** v1.0+

**参数**：

- *mvpds*：MVPD对象列表，其中包含应用程序可用于构建MVPD选择UI元素的MVPD相关信息。

**触发者：** `getAuthentication(), getAuthorization()`

[返回到Android API...](#api)


### setSelectedProvider {#setSelectedProvider}

**描述：** 您的应用程序调用此方法以告知访问启用程序用户的MVPD选择。 应用程序可以使用此方法选择或更改用于身份验证的服务提供程序。

如果所选MVPD是TempPass MVPD，它随后将自动通过该MVPD进行身份验证，而无需调用getAuthentication()。

请注意，这不适用于提升临时传递，因为getAuthentication()方法提供了额外的参数。

传递时 *null* 作为参数， Access Enabler假定用户已取消身份验证流程（即按下“后退”按钮），并通过重置身份验证状态机和调用 *setAuthenticationStatus()* 使用进行回调 `AccessEnablerConstants.PROVIDER_NOT_SELECTED_ERROR` 错误代码。

| API调用：设置当前选定的提供程序 |
| --- |
| 公共void setSelectedProvider(String mvpdId) |

**可用性：** v1.0+

**参数：** 无

**触发的回调：** `setAuthenticationStatus(), sendTrackingData(), navigateToUrl()`

[返回到Android API...](#api)


### navigateToUrl {#navigagteToUrl}

**已弃用：** 从Android SDK 3.0开始，仅当设备上不存在Chrome自定义选项卡时，才使用navigateToUrl

**描述：** 由Access Enabler触发的回调，通知应用程序用户需要显示MVPD登录页才能输入其凭据。 Access Enabler将MVPD登录页面的URL作为参数传递。 您的应用程序需要实例化WebView控件并将其指向此URL。 此外，应用程序还需要监控WebView控件加载的URL，并截获针对由定义的自定义URL的重定向操作。 `AccessEnabler.ADOBEPASS_REDIRECT_URL (deprecated)` 常量。 发生此事件时，应用程序需要关闭或隐藏WebView控件，并通过调用 *getAuthenticationToken()* 方法。 Access Enabler通过从后端服务器检索身份验证令牌并将其本地存储在令牌存储中来完成身份验证流程。

>[!WARNING]
>
> **中止身份验证流程**  <br>请注意，这是用户能够按下“返回”按钮的时刻，这相当于身份验证流中止。 在这种情况下，应用程序需要调用 _setSelectedProvider()_ 方法传递 _null_ 作为参数，并让Access Enabler有机会重置其身份验证状态计算机。

| “回调：显示MVPD登录”页 |
| --- |
| 公共void navigateToUrl（字符串url） |

**可用性：** v1.0+

**参数：**

- *url*：指向MVPD登录页面的URL

**触发者：** `getAuthentication(), setSelectedProvider()`

[返回到Android API...](#api)


### getAuthenticationToken {#getAuthNToken}

**已弃用：** 从Android SDK 3.0开始，由于Chrome自定义选项卡用于身份验证，因此应用程序中不再使用此方法。

**描述：** 通过从后端服务器请求身份验证令牌来完成身份验证流程。 仅当发生以下情况时，应用程序才应调用此方法：托管MVPD登录页面的WebView控件被重定向到由定义的自定义URL `AccessEnabler.ADOBEPASS_REDIRECT_URL` 常量。

| API调用：检索身份验证令牌 |
| --- |
| 公共void getAuthenticationToken() |

**可用性：** v1.0+

**参数：**

- *Cookie*：在Target域中设置的Cookie（请参阅SDK中的演示应用程序以了解参考实施）。

**触发的回调：** `setAuthenticationStatus()`， `sendTrackingData()`

[返回到Android API...](#api)


### setAuthenticationStatus {#setAuthNStatus}

**描述：** 由Access Enabler触发的回调，通知应用程序身份验证流的状态。 在很多地方，由于用户的交互或其他不可预见的情形（如网络连接问题等），此流量可能会失败。 此回调会通知应用程序身份验证流的成功/失败状态，同时还会在需要时提供有关失败原因的其他信息。

| 回调：报告身份验证流的状态 |
| --- |
| 公共void setAuthenticationStatus(int status， String errorCode) |

**可用性：** v1.0+

**参数：**

- *状态*：可以采用以下值之一：
   - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_SUCCESS`  — 身份验证流程已成功完成
   - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_ERROR`  — 身份验证流失败
- *代码*：失败原因。 如果 *状态* 是 `AccessEnablerConstants.ACCESS_ENABLER_STATUS_SUCCESS`，则 *代码* 是一个空字符串(即，由 `AccessEnablerConstants.USER_AUTHENTICATED` 常量)。 如果失败，此参数可以采用以下值之一：
   - `AccessEnablerConstants.USER_NOT_AUTHENTICATED_ERROR`  — 用户未经过身份验证。 回应 *checkAuthentication()* 当本地令牌缓存中没有有效的身份验证令牌时，方法调用。
   - `AccessEnablerConstants.PROVIDER_NOT_SELECTED_ERROR` - AccessEnabler在上层应用程序通过后重置了身份验证状态计算机 *null* 到 `setSelectedProvider()` 中止身份验证流程。  用户可能已取消身份验证流程（例如，已按下“后退”按钮）。
   - `AccessEnablerConstants.GENERIC_AUTHENTICATION_ERROR`  — 由于网络不可用或用户明确取消身份验证流等原因，身份验证流失败。

**触发者：** `checkAuthentication(), getAuthentication(), checkAuthorization()`

[返回到Android API...](#api)


### checkPreauthorizedResources {#checkPreauth}

>**已弃用：** 从Android SDK 3.6开始，预授权API将取代checkPreauthorizedResources，并提供扩展错误代码。

**描述：** 应用程序使用此方法来确定用户是否已获得查看特定受保护资源的授权。 此方法的主要目的是检索用于装饰UI的信息（例如，使用锁定和解锁图标指示访问状态）。

| API调用：设置当前选定的提供程序 |
| --- |
| `public void checkPreauthorizedResources(ArrayList<String> resources)` |

**可用性：** v1.3+

**参数：** 此 `resources` parameter是应检查其授权的资源数组。 列表中的每个元素都应该是一个表示资源ID的字符串。 资源ID与中的资源ID须遵守相同的限制 `getAuthorization()` 调用，即它应为程序员与MVPD或媒体RSS片段之间建立的商定值。

**已触发回调：** `preauthorizedResources()`

[返回到Android API...](#api)


### checkPreauthorizedResources {#checkPreauth2}

**已弃用：** 从Android SDK 3.6开始，预授权API将取代checkPreauthorizedResources，并提供扩展错误代码。

**描述：** 应用程序使用此方法来确定用户是否已获得查看特定受保护资源的授权。 此方法的主要目的是检索用于装饰UI的信息（例如，使用锁定和解锁图标指示访问状态）。

| API调用：设置当前选定的提供程序 |
| --- |
| `public void checkPreauthorizedResources(ArrayList<String> resources, boolean cache)` |

**可用性：** v3.1+

**参数：** 此 `resources` parameter是应检查其授权的资源数组。 列表中的每个元素都应该是一个表示资源ID的字符串。 资源ID与中的资源ID须遵守相同的限制 `getAuthorization()` 调用，即它应为程序员与MVPD或媒体RSS片段之间建立的商定值。

此 `cache` 参数指定是否可以使用缓存的预授权响应。 默认情况下，缓存为true，SDK将返回之前缓存的响应（如果可用）。

**已触发回调：** `preauthorizedResources()`

[返回到Android API...](#api)

### preauthorizedResources {#preauthResources}

**已弃用：** 从Android SDK 3.6开始，预授权API将取代checkPreauthorizedResources，并提供扩展错误代码。 将不会对新API调用preauthorizedResources回调。


**描述：** checkPreauthorizedResources()触发的回调。 提供用户已被授权查看的资源列表。

| API调用：设置当前选定的提供程序 |
| --- |
| `public void checkPreauthorizedResources(ArrayList<String> resources)` |

**可用性：** v1.3+

**参数：** 此 `resources` parameter是用户已被授权查看的资源数组。

**触发者：** `checkPreauthorizedResources()`

[返回到Android API...](#api)

### <span id="checkAuthZ"></span>checkAuthorization

**描述：** 应用程序使用此方法来检查授权状态。 首先检查身份验证状态。 如果未经过身份验证， *setTokenRequestFailed()* 会触发回调，并且方法退出。 如果用户通过了身份验证，它还会触发授权流。 欲知详情，请参阅 *getAuthorization()* 方法。

| API调用：检查授权状态 |
| --- |
| public void checkAuthorization(String resourceId) |

**可用性：** v1.0+

| API调用：检查授权状态 |
| --- |
| public void checkAuthorization(String resourceId， Map&lt;string object=&quot;&quot;> genericData) |

**可用性：** v1.8+

**参数：**

- *resourceId*：用户请求授权的资源的ID。
- *数据*：包含要发送到Pay-TV pass服务的键值对的映射。 Adobe可以使用此数据启用未来的功能，而无需更改SDK。

**触发的回调：** `tokenRequestFailed(), setToken(),sendTrackingData(), setAuthenticationStatus()`

[返回到Android API...](#api)


### <span id="getAuthZ"></span>getAuthorization

**描述：** 应用程序使用此方法来启动授权流。 如果用户尚未经过身份验证，它还会启动身份验证流程。 如果用户通过了身份验证，Access Enabler将继续发出对授权令牌（如果本地令牌缓存中不存在有效的授权令牌）和短期媒体令牌的请求。 获得短媒体令牌后，授权流程即被视为完成。 此 *setToken()* 会触发回调，并将短媒体令牌作为参数提供给应用程序。 如果由于任何原因，授权失败， *tokenRequestFailed()* 会触发回调，并提供错误代码和详细信息。

| API调用：启动授权流 |
| --- |
| 公共void getAuthorization(String resourceId) |

**可用性：** v1.0+

| API调用：启动授权流 |
| --- |
| 公共void getAuthorization(String resourceId， Map&lt;string object=&quot;&quot;> genericData) |

**可用性：** v1.8+

**参数：**

- *resourceId*：用户请求授权的资源的ID。
- *数据*：包含要发送到Pay-TV pass服务的键值对的映射。 Adobe可以使用此数据启用未来的功能，而无需更改SDK。

**触发的回调：** `tokenRequestFailed(), setToken(), sendTrackingData()`

>[!WARNING]
>
> **触发了其他回调**  <br> 此方法还可以触发以下回调（如果还启动了身份验证流程）： *setAuthenticationStatus()*， *displayProviderDialog()*， *navigateToUrl()*

**注意：请尽量使用checkAuthorization()而不是getAuthorization()。 getAuthorization()方法将启动一个完整的身份验证流程（如果用户未经过身份验证），这可能会导致程序员端的复杂实施。**

[返回到Android API...](#api)


### setToken {#setToken}

**描述：** 由Access Enabler触发的回调，通知应用程序授权流已成功完成。 短期媒体令牌还会作为参数提供。

| 回调：授权流已成功完成 |
| --- |
| 公共void setToken(String token， String resourceId) |

**可用性：** v1.0+

**参数：**

- *令牌*：短期媒体令牌
- *resourceId*：获得授权的资源

**触发者：** `checkAuthorization()`， `getAuthorization()`


[返回到Android API...](#api)

### tokenRequestFailed {#tokenRequestFailed}

**描述：** 由Access Enabler触发的回调，通知上层应用程序授权流失败。

| 回调：授权流失败 |
| --- |
| 公共void tokenRequestFailed(String resourceId， <br>        String errorCode， String errorDescription) |

**可用性：** v1.0+

**参数：**

- *resourceId*：获得授权的资源
- *错误代码*：与失败方案关联的错误代码。 可能的值：
   - `AccessEnablerConstants.USER_NOT_AUTHORIZED_ERROR`  — 用户无法授权给定资源
- *errorDescription*：有关失败场景的其他详细信息。 如果此描述性字符串由于任何原因不可用，Adobe Pass身份验证将发送空字符串 **(“”)**.

  MVPD可使用此字符串传递自定义错误消息或与销售相关的消息。 例如，如果订阅者被拒绝对资源的授权，则MVPD会发送消息，例如：“您当前在包中无法访问此渠道。 如果要升级包，请单击此处。” 消息由Adobe Pass身份验证通过此回调传递给程序员，程序员可以选择显示或忽略该消息。 Adobe Pass身份验证还可以使用此参数来提供可能导致错误的状况通知。 例如，“与提供商的授权服务通信时出现网络错误。”

**触发者：** `checkAuthorization(), getAuthorization()`

[返回到Android API...](#api)

### 注销 {#logout}

**描述：** 使用此方法可启动注销流程。 注销是一系列HTTP重定向操作的结果，这是因为用户既需要从Adobe Pass身份验证服务器注销，也需要从MVPD服务器注销。 因此，使用Access Enabler库发出的简单HTTP请求无法完成此流程。 SDK使用Chrome自定义选项卡来执行HTTP重定向操作。 此流程将对用户可见，并在完成后关闭

| API调用：启动注销流程 |
| --- |
| public void logout() |

**可用性：** v1.0+

**参数：** 无

**触发的回调：**

- `navigateToUrl()` for SDK 3.0之前的版本
- `setAuthenticationStatus()` 适用于SDK版本> 3.0的


[返回到Android API...](#api)


### getselectedprovider {#getSelectedProvider}

**描述：** 使用此方法可确定当前选择的提供程序。

| API调用：确定当前选定的MVPD |
| --- |
| 公共void getSelectedProvider() |

**可用性：** v1.0+

**参数：** 无

**触发的回调：** `selectedProvider()`

[返回到Android API...](#api)


### <span id="selectedProvider"></span>selectedprovider

**描述：** 由Access Enabler触发的回调，它将有关当前所选MVPD的信息提供给应用程序。

| 回调：有关当前所选MVPD的信息 |
| --- |
| public void selectedProvider(Mvpd mvpd) |


**可用性：** v1.0+

**参数：**

- *mvpd*：包含有关当前所选MVPD信息的对象

**触发者：** `getSelectedProvider()`

[返回到Android API...](#api)


### getMetadata {#getMetadata}

**描述：** 使用此方法可检索Access Enabler库公开为元数据的信息。 应用程序可以通过提供复合MetadataKey对象来访问此信息。

| API调用：查询AccessEnabler以获取元数据 |
| --- |
| `public void getMetadata(MetadataKey metadataKey)` |

**可用性：** v1.0+

有两种类型的元数据可供程序员使用：

- 静态元数据（身份验证令牌TTL、授权令牌TTL和设备ID）
- 用户元数据（用户特定的信息，例如用户ID和邮政编码；在身份验证和/或授权流期间，从MVPD传递到用户设备）

**参数：**

- *metadatakey*：封装键和args变量的数据结构，其含义如下：
   - 如果键为 `METADATA_KEY_USER_META` 和参数包含名为=的SerializableNameValuePair对象 `METADATA_ARG_USER_META` 和值= `[metadata_name]`，则查询用户元数据。 可用用户元数据类型的当前列表：
      - `zip`  — 邮政编码

      - `householdID`  — 家庭标识符。 如果MVPD不支持子帐户，则它与 `userID`.

      - `maxRating`  — 用户的最大家长评级

      - `userID`  — 用户标识符。 如果MVPD支持子帐户，并且用户不是主帐户， `userID` 将不同 `householdID`.

      - `channelID`  — 用户有权查看的渠道列表
   - 如果键为 `METADATA_KEY_DEVICE_ID` 然后进行查询以获取当前设备id。 请注意，此功能默认处于禁用状态，程序员应联系Adobe以获取有关启用和费用的信息。
   - 如果键为 `METADATA_KEY_TTL_AUTHZ` 和参数包含名为=的SerializableNameValuePair对象 `METADATA_ARG_RESOURCE_ID` 和值= `[resource_id]`，然后进行查询以获取与指定资源关联的授权令牌的过期时间。
   - 如果键为 `METADATA_KEY_TTL_AUTHN` 然后进行查询以获取身份验证令牌过期时间。



>[!NOTE]
>
>对于SDK 3.4.0，常量： `METADATA_KEY_USER_META, METADATA_KEY_DEVICE_ID, METADATA_KEY_TTL_AUTHZ, METADATA_KEY_TTL_AUTHN` 可从com.adobe.adobepass.accessenabler.api.profile.UserProfileService获取。



>[!NOTE]
>
>程序员可用的实际用户元数据取决于MVPD提供的内容。  此列表将随着新元数据的推出和添加到Adobe Pass身份验证系统中而进一步扩展。

**触发的回调：** [`setMetadataStatus()`](#setMetadaStatus)

**更多信息：** [用户元数据](/help/authentication/user-metadata-feature.md)

[返回到Android API...](#api)

### setmetadatastatus {#setMetadaStatus}

**描述：** 由Access Enabler触发的回调，用于提供通过 *getMetadata()* 呼叫。

| 回调：元数据检索请求的结果 |
| --- |
| ```public void setMetadataStatus(MetadataKey key, MetadataStatus result)``` |

**可用性：** v1.0+

**参数：**

- *键*：MetadataKey对象，其中包含为其请求元数据值的键和相关参数（请参阅演示应用程序以了解参考实施）。
- *结果*：包含所请求元数据的复合对象。 该对象具有以下字段：
   - *simpleResult*：一个字符串，表示在请求身份验证TTL、授权TTL或设备ID时的元数据值。 如果为用户元数据发出请求，则此值为null。

   - *userMetadataResult*：包含JSON用户元数据有效负载的Java表示法的对象。\
     例如：

```json
          '{
          "street": "Main Avenue",
          "buildings": ["150", "320"]
          }'
```

翻译为Java的形式：

```java
          Map("street" -> "Main Avenue", "buildings" -> List("150", "320")))
```

**用户元数据对象的实际结构类似于以下内容：**

```json
          {
              updated: 1334243471,
              encrypted: ["encryptedProp"],
              data: {
                  zip: ["12345", "34567"],
                  maxRating: { 
                      "MPAA": "PG-13",
                      "VCHIP": "TV-Y", 
                      "URL": "http://exam.pl/e/manage/ratings"
                  },
                  householdID: "3456",
                  userID: "BgSdasfsdk23/dsaf3+saASesadgfsShggssd=",
                  channelID: ["channel-1", "channel-2"]
              }
          }
```

请求简单元数据（身份验证TTL、授权TTL或设备ID）时，该值为空。

- *已加密*：指定是否对检索到的元数据进行加密的布尔值。 此参数仅对用户元数据请求有意义，对于始终未加密接收的静态元数据（例如：身份验证TTL）没有意义。 如果此参数设置为True，则程序员需要通过使用白名单私钥（与在中签名请求者ID时使用的私钥）执行RSA解密来获取未加密的用户元数据值。 [`setRequestor`](#setRequestor) 调用)。

**触发者：** [`getMetadata()`](#getMetadata)

**更多信息：** [用户元数据](/help/authentication/user-metadata-feature.md)


[返回到Android API...](#api)


### getVersion {#getVersion}

**描述：** 此方法可用于检索AccessEnabler库的版本。

| API调用：获取AccessEnabler版本 |
| --- |
| ```public static String getVersion()``` |


[返回到Android API...](#api)

</br>

## 跟踪事件 {#tracking}

Access Enabler会触发一个附加回调，该回调不一定与权利文件流相关。 实现名为的事件跟踪回调函数 *sendTrackingData()* 是可选的，但它使应用程序能够跟踪特定事件并编译统计信息，如成功/失败的身份验证/授权尝试次数。 以下是 *sendTrackingData()* 回调：


### sendTrackingData {#sendTrackingData}

**描述：** Access Enabler向应用程序发出各种事件（如身份验证/授权流的完成/失败）的信号时触发的回调。 sendTrackingData()还会报告设备类型、Access Enabler客户端类型和操作系统。

>[!WARNING]
>
> 设备类型和操作系统是通过使用公共Java库([http://java.net/projects/user-agent-utils](http://java.net/projects/user-agent-utils))和用户代理字符串。 请注意，此信息仅以粗略的方式提供，用于按设备类别细分操作量度，但该Adobe对错误结果不承担任何责任。 请相应地使用新功能。


- 设备类型的可能值：
   - `computer`
   - `tablet`
   - `mobile`
   - `gameconsole`
   - `unknown`


- Access Enabler客户端类型的可能值：
   - `flash`
   - `html5`
   - `ios`
   - `android`

</br>

| 回调：跟踪事件 |
| --- |
| ```public void sendTrackingData(Event event, ArrayList<String> data)``` |

**可用性：** v1.0+

**参数：**

- *事件*：正在跟踪的事件。 有三种可能的跟踪事件类型：
   - **授权检测：** 每当授权令牌请求返回时(事件类型为 `EVENT_AUTHZ_DETECTION`)
   - **authenticationDetection：** 每当进行身份验证检查时(事件类型为 `EVENT_AUTHN_DETECTION`)
   - **mvpdSelection：** 当用户在MVPD选择表单中选择MVPD时(事件类型为 `EVENT_MVPD_SELECTION`)
- *数据*：与所报告事件关联的其他数据。 此数据以值列表的形式提供。

以下说明用于解释 *数据*
数组：

- 对于事件类型 *`EVENT_AUTHN_DETECTION`：*
   - **0**  — 令牌请求是否成功(true/false)，如果以上为true：
   - **1** - MVPD ID字符串
   - **2** - GUID（md5散列）
   - **3**  — 令牌已在缓存中(true/false)
   - **4**  — 设备类型
   - **5** - Access Enabler客户端类型
   - **6**  — 操作系统类型

- 对于事件类型 `EVENT_AUTHZ_DETECTION`
   - **0**  — 令牌请求是否成功(true/false)，如果成功：
   - **1** - MVPD ID
   - **2** - GUID（md5散列）
   - **3**  — 令牌已在缓存中(true/false)
   - **4**  — 错误
   - **5**  — 详细信息
   - **6**  — 设备类型
   - **7** - Access Enabler客户端类型
   - **8**  — 操作系统类型

- 对于事件类型 `EVENT_MVPD_SELECTION`
   - **0**  — 当前所选MVPD的ID
   - **1**  — 设备类型
   - **2** - Access Enabler客户端类型
   - **3**  — 操作系统类型

**触发者：** `checkAuthentication()`， `getAuthentication()`， `checkAuthorization()`， `getAuthorization()`， `setSelectedProvider()`

[返回到Android API...](#api)


<!--
## Related Information {#related}

- [Android Integration Cookbook](/help/authentication/android-sdk-cookbook.md)
- [Android Technical Overview](/help/authentication/android-sdk-overview.md)

-->
