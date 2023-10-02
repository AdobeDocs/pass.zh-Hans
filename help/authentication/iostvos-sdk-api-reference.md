---
title: iOS/tvOS API参考
description: iOS/tvOS API参考
exl-id: 017a55a8-0855-4c52-aad0-d3d597996fcb
source-git-commit: 854698397d9d14c1bfddcc10eecc61c7e3c32b71
workflow-type: tm+mt
source-wordcount: '7018'
ht-degree: 0%

---

# iOS/tvOS SDK API参考 {#iostvos-sdk-api-reference}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## 介绍 {#intro}

本页介绍用于Adobe Pass身份验证的iOS/tvOS本机客户端公开的方法和回调函数。 此处介绍的方法和回调函数在中定义 `AccessEnabler.h` 和 `EntitlementDelegate.h` 头文件；您可以在iOS AccessEnabler SDK中的以下位置找到它们： `[SDK directory]/AccessEnabler/headers/api/`


相关文档：

* 有关基本Adobe Pass身份验证权利流的描述，请参阅 [权利流](/help/authentication/entitlement-flow.md).
* 有关如何使用此API实施Adobe Pass身份验证权利流的分步说明，请参阅 [iOS集成指南](/help/authentication/iostvos-sdk-cookbook.md).
* 有关最新的iOS AccessEnabler SDK，请参阅 [iOS Native Access Enabler库](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library).

>[!NOTE]
>
>Adobe建议您仅使用Adobe Pass身份验证 *公共* API：
>
>* 公共API可用，并在所有受支持的客户端类型上进行了全面测试。 对于任何公共功能，我们确保每种客户端类型都有相关方法的相应版本。
>* 公共API需要尽可能保持稳定，以支持向后兼容性并确保合作伙伴集成不会中断。 但是，对于非公共API，我们保留在未来任何时候更改其签名的权利。 如果您遇到无法通过当前公共Adobe Pass身份验证API调用组合支持的特定流程，最好的方法是告知我们。 根据您的需求，我们可以修改公共API，并在以后提供稳定的解决方案。

</br>

## API参考 {#apis}

* [init](#initWithSoftwareStatement)：softwareStatement — 实例化AccessEnabler对象。

* **[已弃用]** [init](#init)  — 实例化AccessEnabler对象。

* [setOptions:options:](#setOptions)  — 配置全局SDK选项，如profile或visitorID。

* [设置请求者：](#setReqV3)[请求者ID](#setReqV3)，[setRequestor:requestorID:服务提供商：](#setReqV3)  — 确定程序员的身份。

* **[已弃用]** [setRequestor:signedRequestorId:](#setReq)，[setRequestor:signedRequestorId:服务提供商：](#setReq)  — 确定程序员的身份。

* **[已弃用]** [setRequestor:signedRequestorId:secret：publicKey](#setReq_tvos)， [setRequestor:signedRequestorId:服务提供商:secret:公共密钥](#setReq_tvos) — 确定程序员的身份。

* [setRequestorComplete：](#setReqComplete)  — 通知应用程序配置阶段已完成。

* [checkAuthentication](#checkAuthN)  — 检查当前用户的身份验证状态。

* [getAuthentication](#getAuthN)， [getAuthentication:withData:](#getAuthN)  — 启动完整的身份验证工作流。

* [getAuthentication：filter](#getAuthN_filter)，[getAuthentication:withData:](#getAuthN)[andFilter](#getAuthN_filter)  — 启动完整的身份验证工作流。

* [displayProviderDialog：](#dispProvDialog)  — 通知您的应用程序实例化相应的UI元素，以便用户选择MVPD。

* [setSelectedProvider：](#setSelProv)  — 通知AccessEnabler用户的MVPD选择。

* [navigateToUrl：](#nav2url)  — 通知您的应用程序需要向用户显示MVPD登录页面。

* [navigateToUrl:useSVC:](#nav2urlSVC)  — 使用SFSafariViewController通知应用程序需要向用户显示MVPD登录页面

* [handleExternalURL：url](#handleExternalURL)  — 完成身份验证/注销流程。

* **[已弃用]** [getAuthenticationToken](#getAuthNToken)  — 从后端服务器请求身份验证令牌。

* [setAuthenticationStatus:errorCode:](#setAuthNStatus)  — 通知应用程序身份验证流的状态。

* [checkPreauthorizedResources：](#checkPreauth)  — 确定用户是否已获得查看特定受保护资源的授权。

* [checkPreauthorizedResources:cache:](#checkPreauthCache)  — 确定用户是否已获得查看特定受保护资源的授权。

* [预授权资源：](#preauthResources)  — 提供用户已被授权查看的资源列表。

* [checkAuthorization：](#checkAuthZ)， [checkAuthorization:withData:](#checkAuthZ)  — 检查当前用户的授权状态。

* [getAuthorization：](#getAuthZ)， [getAuthorization:withData:](#getAuthZ)  — 启动授权流。

* [setToken:forResource:](#setToken)  — 通知您的应用程序已成功完成授权流程。

* [tokenRequestFailed:errorCode:errorDescription：](#tokenReqFailed)  — 通知应用程序授权流失败。

* [注销](#logout)  — 启动注销流程。

* [getselectedprovider](#getSelProv)  — 确定当前选择的提供程序。

* [selectedProvider：](#selProv)  — 将有关当前选定的MVPD的信息提供给应用程序。

* [getMetadata：](#getMeta)  — 检索AccessEnabler库公开为元数据的信息。

* [presentTvProviderDialog：](#presentTvDialog)  — 通知应用程序显示Apple SSO对话框。

* [cissisTvProviderDialog：](#dismissTvDialog)  — 通知应用程序隐藏Apple SSO对话框。

* [setmetadatastatus:encrypted:forKey:andArguments:](#setMetaStatus)  — 提供所请求的元数据 [getMetadata：](#getMeta) 呼叫。

* [sendTrackingData:forEventType:](#sendTracking)  — 提供跟踪数据信息。

* [MVPD](#mvpd) - MVPD类。 [包含有关MVPD的信息]

### init：softwareStatement {#initWithSoftwareStatement}

**文件：** AccessEnabler/headers/AccessEnabler.h

**描述：** 实例化AccessEnabler对象。 每个应用程序实例应有一个AccessEnabler实例。

| **API调用： iOS AccessEnabler构造函数** |
| --- |
| `- (id) init:` <br> |
| `(NSString *)softwareStatement;` |


**可用性：** v3.0+

**参数：**

* **软件声明：** 一个字符串，用于标识Adobe系统中的应用程序。 了解如何获取软件声明。

[返回页首……](#apis)



### 初始化 —  [已弃用]{#init}

**文件：** AccessEnabler/headers/AccessEnabler.h

**描述：** 实例化AccessEnabler对象。 每个应用程序实例应有一个AccessEnabler实例。

| API调用： iOS AccessEnabler构造函数 |
| --- |
| ```- (id) init;``` |

**可用性：** v1.0+ **直到：** v3.0

**参数：** 无

[返回页首……](#apis)

</br>

### 设置选项：选项 {#setOptions}

**文件：** AccessEnabler/headers/AccessEnabler.h

**描述：** 配置全局SDK选项。 它接受NSDictionary作为参数。 词典中的值将与SDK发出的每个网络调用一起传递到服务器。

**注意：** 这些值将传递到服务器，与当前流（身份验证/授权）无关。 如果要更改值，可以随时调用此方法。

| API调用： setOptions |
| --- |
| ```- (void) setOptions:(NSDictionary *)options;``` |

**可用性：** v2.3.0+

**参数：**

* *options*：包含全局SDK选项的NSDictionary。 目前，以下选项可用：
   * **applicationprofile**  — 可用于根据此值进行服务器配置。
   * **visitorID** - Experience CloudID服务。 此值以后可用于高级分析报表。
   * **handleSVC**  — 布尔值，指示程序员是否将处理SFSafariViewControllers。 请参阅 [iOS SDK 3.2+上的SFSafariViewController支持](/help/authentication/sfsafariviewcontroller-support-on-ios-sdk-32.md) 以了解更多详细信息。
      * 如果设置为 **false，** 该SDK将自动向最终用户呈现SFSafariViewController。 SDK将进一步导航到MVPD登录页面URL。
      * 如果设置为 **正确，** SDK将 **NOT** 自动向最终用户呈现SFSafariViewController。 SDK将进一步触发 **navigate(toUrl：{url}， useSVC：YES)**.
* **device\_info**  — 客户端信息，如中所述 [传递客户端信息](/help/authentication/passing-client-information-device-connection-and-application.md).

[返回页首……](#apis)


### setRequestor：requestorID， setRequestor:requestorID:服务提供商： {#setReqV3}

**文件：** AccessEnabler/headers/AccessEnabler.h

**描述：** 确定程序员的身份。 在为Adobe Pass身份验证系统注册Adobe时，每个程序员都会分配一个唯一的ID。 在处理SSO和远程令牌时，当应用程序处于后台时，身份验证状态可能会更改，当应用程序进入前台时，可以再次调用setRequestor以便与系统状态同步（如果启用了SSO，则获取远程令牌，如果同时发生注销，则删除本地令牌）。

服务器响应包含MVPD列表以及附加到程序员标识的一些配置信息。 服务器响应由AccessEnabler代码在内部使用。 只有操作的状态（即SUCCESS/FAIL）才会通过向应用程序显示 `setRequestorComplete:` 回调。

如果 `urls` 未使用参数，生成的网络调用将定向默认服务提供商URL：AdobeRELEASE/production environment。


如果为提供了值， `urls` 参数，则生成的网络调用将定向于 `urls` 参数。 所有配置请求都在单独的线程中同时触发。 在编译MVPD列表时，优先使用第一个响应程序。 对于列表中的每个MVPD，AccessEnabler会记住关联服务提供商的URL。 所有后续权利请求都定向到与服务提供程序相关联的URL，该URL在配置阶段与目标MVPD配对。

| API调用：请求者配置 |
| --- |
| ```- (void) setRequestor:(NSString *)requestorID``` |


**可用性：** v3.0+

| API调用：请求者配置 |
| --- |
| `- (void) setRequestor:(NSString *)requestorID serviceProviders:(NSArray *)urls;` |


**可用性：** v3.0+

**参数：**

* *请求者ID*：与程序员关联的唯一ID。 首次注册Adobe Pass身份验证服务时，请将Adobe分配的唯一ID传递到您的网站。
* *url*：可选参数；默认情况下，使用Adobe服务提供程序(http://sp.auth.adobe.com/)。 此阵列允许您为Adobe提供的身份验证和授权服务指定端点（其他实例可能用于调试目的）。 您可以使用此选项指定多个Adobe Pass身份验证服务提供程序实例。 这样做时，MVPD列表由来自所有服务提供商的端点组成。 每个MVPD都与最快的服务提供程序相关联；即首先响应并支持该MVPD的服务提供程序。

>[!NOTE]
>
>如果在调用时不使用 `serviceProviders` 参数，则库将从默认服务提供程序(即， `https://sp.auth.adobe.com` 对于生产配置文件或 `https://sp.auth-staging.adobe.com` （对于暂存配置文件）。 如果 `serviceProviders` 如果提供了参数，则必须是URL的数组。从所有指定的端点检索配置信息并合并。 如果不同的服务提供者响应中存在重复信息，则冲突将得到解决，以支持响应最快的服务器（即，响应时间最短的服务器优先）。

**触发的回调：** [`setRequestorComplete:`](#setReqComplete)

[返回页首……](#apis)

</br>

### setRequestor:setSignedRequestorId:， setRequestor:setSignedRequestorId:服务提供商： - [已弃用] {#setReq}

**文件：** AccessEnabler/headers/AccessEnabler.h

**描述：** 确定程序员的身份。 在为Adobe Pass身份验证系统注册Adobe时，每个程序员都会分配一个唯一的ID。 在处理SSO和远程令牌时，当应用程序处于后台时，身份验证状态可能会更改，当应用程序进入前台时，可以再次调用setRequestor以便与系统状态同步（如果启用了SSO，则获取远程令牌，如果同时发生注销，则删除本地令牌）。

服务器响应包含MVPD列表以及附加到程序员标识的一些配置信息。 服务器响应由AccessEnabler代码在内部使用。 只有操作的状态（即SUCCESS/FAIL）才会通过向应用程序显示 `setRequestorComplete:` 回调。

如果 `urls` 未使用参数，生成的网络调用将定向默认服务提供商URL：AdobeRELEASE/production environment。

如果为提供了值， `urls` 参数，则生成的网络调用将定向于 `urls` 参数。 所有配置请求都在单独的线程中同时触发。 在编译MVPD列表时，优先使用第一个响应程序。 对于列表中的每个MVPD，AccessEnabler会记住关联服务提供商的URL。 所有后续权利请求都定向到与服务提供程序相关联的URL，该URL在配置阶段与目标MVPD配对。

| API调用：请求者配置 |
| --- |
| </br>`- (void) setRequestor:(NSString *)requestorID`</br>`signedRequestorID:(NSString *)signedRequestorID;` </br></br> |

**可用性：** v1.0+ **直到：** v3.0

| API调用：请求者配置 |
| --- |
| `- (void) setRequestor:(NSString *)requestorID ` <br> `       signedRequestorID:(NSString *)signedRequestorID` <br> `         serviceProviders:(NSArray *)urls;` |

**可用性：** v1.0+ **直到：** v3.0

**参数：**

* *请求者ID*：与程序员关联的唯一ID。 首次向Adobe Pass身份验证服务注册时，将Adobe分配的唯一ID传递到您的网站。
* *signedRequestID*： **iOS AccessEnabler 1.2及更高版本中存在此参数。** 用您的私钥进行数字签名的请求者ID的副本。 <!--For more details, see [Registering Native Clients](https://tve.helpdocsonline.com/registering-native-clients)-->.
* *url*：可选参数；默认情况下，使用Adobe服务提供程序(http://sp.auth.adobe.com/)。 此阵列允许您为Adobe提供的身份验证和授权服务指定端点（其他实例可能用于调试目的）。 您可以使用此选项指定多个Adobe Pass身份验证服务提供程序实例。 这样做时，MVPD列表由来自所有服务提供商的端点组成。 每个MVPD都与最快的服务提供程序相关联；即首先响应并支持该MVPD的服务提供程序。

**注释：** 如果在调用时不使用 `serviceProviders` 参数，则库将从默认服务提供程序(即，`https://sp.auth.adobe.com` 对于生产配置文件或 `https://sp.auth-staging.adobe.com` （对于暂存配置文件）。 如果 `serviceProviders` 如果提供了参数，则必须是URL的数组。从所有指定的端点检索配置信息并合并。 如果不同的服务提供者响应中存在重复信息，则冲突将得到解决，以支持响应最快的服务器（即，响应时间最短的服务器优先）。

**触发的回调：** [`setRequestorComplete:`](#setReqComplete)


[返回页首……](#apis)

### setRequestor:setSignedRequestorId:密钥：publicKey， setRequestor:setSignedRequestorId:服务提供商:secret:公钥 —  [已弃用] {#setReq_tvos}

**文件：** AccessEnabler/headers/AccessEnabler.h

**描述：** 确定程序员的身份。 在为Adobe Pass身份验证系统注册Adobe时，每个程序员都会分配一个唯一的ID。 在应用程序的生命周期中，只应执行一次此设置。

服务器响应包含MVPD列表以及附加到程序员标识的一些配置信息。 服务器响应由AccessEnabler代码在内部使用。 只有操作的状态（即SUCCESS/FAIL）才会通过向应用程序显示 `setRequestorComplete:` 回调。

如果 `urls` 未使用参数，生成的网络调用将定向默认服务提供商URL：AdobeRELEASE/production environment。

如果为提供了值， `urls` 参数，则生成的网络调用将定向于 `urls` 参数。 所有配置请求都在单独的线程中同时触发。 在编译MVPD列表时，优先使用第一个响应程序。 对于列表中的每个MVPD，AccessEnabler会记住关联服务提供商的URL。 所有后续权利请求都定向到与服务提供程序相关联的URL，该URL在配置阶段与目标MVPD配对。



<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API调用：请求者配置</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setRequestor:(NSString *)requestorID 
    signedRequestorID:(NSString *)signedRequestorID
               secret:(NSString *)secret
            publicKey:(NSString *)publicKey;</code></pre></td>
</tr>
</tbody>
</table>


**可用性：** v2.0+ **直到：** v3.0

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API调用：请求者配置</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setRequestor:(NSString *)requestorID 
    signedRequestorID:(NSString *)signedRequestorID 
     serviceProviders:(NSArray *)urls</code></pre>
<p><code class="sourceCode objectivec">               secret:(NSString *)secret</code></p>
<p><code class="sourceCode objectivec">            publicKey:(NSString *)publicKey;</code></p></td>
</tr>
</tbody>
</table>

**可用性：** v2.0+ **直到：** v3.0

**参数：**

* *请求者ID*：与程序员关联的唯一ID。 首次向Adobe Pass身份验证服务注册时，将Adobe分配的唯一ID传递到您的网站。
* *signedRequestID*： **iOS AccessEnabler 1.2及更高版本中存在此参数。** 用您的私钥进行数字签名的请求者ID的副本。 <!--For more details, see [Registering Native Clients](https://tve.helpdocsonline.com/registering-native-clients)-->.
* *url*：可选参数；默认情况下，使用Adobe服务提供程序(http://sp.auth.adobe.com/)。 此阵列允许您为Adobe提供的身份验证和授权服务指定端点（其他实例可能用于调试目的）。 您可以使用此选项指定多个Adobe Pass身份验证服务提供程序实例。 这样做时，MVPD列表由来自所有服务提供商的端点组成。 每个MVPD都与最快的服务提供程序相关联；即首先响应并支持该MVPD的服务提供程序。
* secret和publicKey：用于签署第二个屏幕调用的密钥和公钥。 欲知更多信息，请参见 [无客户端文档](#create_dev).

如果在调用时不使用 `serviceProviders` 参数，则库将从默认服务提供商(即， `https://sp.auth.adobe.com` (对于生产配置文件，请使用https://sp.auth-staging.adobe.com；对于暂存配置文件，请使用)。 如果 `serviceProviders` 如果提供了参数，则必须是URL的数组。从所有指定的端点检索配置信息并合并。 如果不同的服务提供者响应中存在重复信息，则冲突将得到解决，以支持响应最快的服务器（即，响应时间最短的服务器优先）。

**触发的回调：** [`setRequestorComplete:`](#setReqComplete)

[返回页首……](#apis)

</br>

### setRequestorComplete： {#setReqComplete}

**文件：** AccessEnabler/headers/EntitlementDelegate.h

**描述** 由AccessEnabler触发的回调，通知应用程序配置阶段已完成。 这是一个信号，表明应用程序可以开始发出授权请求。 在配置阶段完成之前，应用程序无法发出任何授权请求。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>回调：请求者配置完成</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setRequestorComplete:(int)status;</code></pre></td>
</tr>
</tbody>
</table>


**可用性：** v1.0+

**参数**：

* *状态*：可以采用以下任一值：
   * `ACCESS_ENABLER_STATUS_SUCCESS`  — 配置阶段已成功完成
   * `ACCESS_ENABLER_STATUS_ERROR`  — 配置阶段失败

**触发者：**
`setRequestor:setSignedRequestorId:, `[`setRequestor:setSignedRequestorId:serviceProviders:`](#setReq)

[返回页首……](#apis)

</br>

### checkAuthentication {#checkAuthN}

**文件：** AccessEnabler/headers/AccessEnabler.h

**描述：** 检查当前用户的身份验证状态。
它通过在本地令牌存储空间中搜索有效的身份验证令牌来完成此操作。 此方法不执行任何网络调用，我们建议在主线程上调用它。
应用程序使用它来查询用户的身份验证状态并相应地更新UI（即更新登录/注销UI）。 身份验证状态通过 [`setAuthenticationStatus:errorCode:`](#setAuthNStatus) 回调。


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API调用：检查身份验证状态</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkAuthentication;</code></pre></td>
</tr>
</tbody>
</table>

**可用性：** v1.0+

**参数：** 无

**触发的回调：**
[`setAuthenticationStatus:errorCode:`](#setAuthNStatus)

[返回页首……](#apis)

</br>

### getAuthentication， getAuthentication:withData: {#getAuthN}

**文件：** AccessEnabler/headers/AccessEnabler.h

**描述：** 启动完整的身份验证工作流。 首先检查身份验证状态。 如果尚未经过身份验证，则身份验证流状态 — 计算机将启动：

* 如果上次身份验证尝试成功，则会跳过MVPD选择阶段，并且 [`navigateToUrl:`](#nav2url) 会触发回调。 应用程序使用此回调实例化向用户显示MVPD登录页的WebView控件。 **[注意：自Access Enabler 1.5起，由于SDK中的限制，此功能不可用].**
* 如果上次验证尝试不成功，或者用户明确注销，则 [`displayProviderDialog:`](#dispProvDialog) 会触发回调。 您的应用程序使用此回调来显示MVPD选择UI。 此外，您的应用程序需要通过通知AccessEnabler库有关用户的MVPD选择，来恢复身份验证流程。 [`setSelectedProvider:`](#setSelProv) 方法。

在MVPD登录页面上验证用户的凭据时，应用程序需要监视用户在MVPD登录页面上验证时发生的多个重定向操作。 输入正确的凭据后，WebView控件将被重定向到由定义的自定义URL。 `ADOBEPASS_REDIRECT_URL` 常量。 此URL不打算由WebView加载。 应用程序必须拦截此URL，并将此事件解释为登录阶段已完成的信号。 然后，它应该将控制权移交给AccessEnabler，以便完成身份验证流程(通过调用 [handleExternalURL](#handleExternalURL) 方法)。

最后，验证状态通过 [setAuthenticationStatus:errorCode:](#setAuthNStatus) 回调。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API调用：启动身份验证流程</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication;</code></pre></td>
</tr>
</tbody>
</table>

**可用性：** v1.0+

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API调用：启动身份验证流程</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(BOOL)forceAuthn:
                  withData:(NSDictionary* )data;</code></pre>
<div>

</div></td>
</tr>
</tbody>
</table>



**可用性：** v1.9+

**参数：**

* *forceAuthn*：指定是否应启动身份验证流程的标记，无论用户是否已进行身份验证。
* *数据*：包含要发送到Pay-TV密码服务的键值对的词典。 Adobe可以使用此数据启用未来的功能，而无需更改SDK。

**触发的回调：** ` setAuthenticationStatus:errorCode:, `[`displayProviderDialog:`](#dispProvDialog)`,`` sendTrackingData:forEventType:`


[返回页首……](#apis)

</br>

### getAuthentication：filter， getAuthentication:withData:andFilter {#getAuthN_filter}

**文件：** AccessEnabler/headers/AccessEnabler.h

**描述：** 启动完整的身份验证工作流。 首先检查身份验证状态。 如果尚未经过身份验证，则身份验证流状态 — 计算机将启动：

* [presentTvProviderDialog()](#presentTvDialog) 如果当前请求者至少有一个支持SSO的MVPD，则将调用。 如果没有任何MVPD支持SSO，则将开始经典身份验证流程并忽略过滤器参数。
* 用户完成Apple SSO流程后 [cissisTvProviderDialog()](#dismissTvDialog) 将触发，身份验证过程将结束。

最后，验证状态通过 [setAuthenticationStatus:errorCode:](#setAuthNStatus) 回调。

**可用性：** v2.4+

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API调用：启动身份验证流程</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(NSDictionary *)filter;</code></pre></td>
</tr>
</tbody>
</table>



<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API调用：启动身份验证流程</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(BOOL)forceAuthn:
                  withData:(NSDictionary* )data
                 andFilter:(NSDictionary *)filter;</code></pre>
<div>

</div></td>
</tr>
</tbody>
</table>



**参数：**

* *forceAuthn*：指定是否应启动身份验证流程的标记，无论用户是否已进行身份验证。
* *数据*：包含要发送到Pay-TV密码服务的键值对的词典。 Adobe可以使用此数据启用未来的功能，而无需更改SDK。
* 过滤器：一个字典，其中包含应显示在Apple SSO对话框中的两个MVPD ID列表。 任何不支持SSO的MVPD将被忽略，但将遵守该顺序。 字典需要两把钥匙：
   * TV\_PROVIDERS：包含所有应显示在选取器中的MVPD的列表
   * FEATURED\_TV\_PROVIDERS：包含所有应在选取器中标记为特色的MVPD的列表。 还必须在TV\_PROVIDERS列表中指定此列表中的MVPD。

**可用性：** v2.0 - v2.3.1


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API调用：启动身份验证流程</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(NSArray *)filter;</code></pre></td>
</tr>
</tbody>
</table>



<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API调用：启动身份验证流程</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(BOOL)forceAuthn:
                  withData:(NSDictionary* )data
                 andFilter:(NSArray *)filter;</code></pre>
<div>

</div></td>
</tr>
</tbody>
</table>



**参数：**

* *forceAuthn*：指定是否应启动身份验证流程的标记，无论用户是否已进行身份验证。
* *数据*：包含要发送到Pay-TV密码服务的键值对的词典。 Adobe可以使用此数据启用未来的功能，而无需更改SDK。
* 过滤器：应显示在Apple SSO对话框中的MVPD ID列表。 任何不支持SSO的MVPD将被忽略，但将遵守该顺序。

**触发的回调：** `setAuthenticationStatus:errorCode:, presentTvProviderDialog, dismissTvProviderDialog`


[返回页首……](#apis)

</br>

#### displayProviderDialog： {#dispProvDialog}

**文件：** AccessEnabler/headers/EntitlementDelegate.h

**描述** 由AccessEnabler触发的回调，用于通知应用程序需要实例化适当的UI元素以允许用户选择所需的MVPD。 回调会提供MVPD对象的列表以及其他有助于正确构建选择UI面板的信息（例如指向MVPD徽标的URL、友好显示名称等）

一旦用户选择了所需的MVPD，则上层应用程序需要通过调用来恢复验证流 `setSelectedProvider:` 并向其传递与用户选择相对应的MVPD的ID。

**中止身份验证流程**  — 这是用户能够按下“返回”按钮的时刻，这相当于中止身份验证流程。 在这种情况下，应用程序需要调用 [setSelectedProvider：](#setSelProv) 方法，传递null作为参数，为AccessEnabler提供重置其身份验证状态机的机会。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>回调：显示MVPD选择UI</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) displayProviderDialog:(NSArray *)mvpds;</code></pre></td>
</tr>
</tbody>
</table>


**可用性：** v1.0+

**参数**：

* *mvpds*：MVPD对象列表，其中包含应用程序可用于构建MVPD选择UI元素的MVPD相关信息。

**触发者：** ` getAuthentication, `[getAuthentication:withData:](#getAuthN)，` getAuthorization:, `[getAuthorization:withData:](#getAuthZ)


[返回页首……](#apis)

</br>

#### setSelectedProvider： {#setSelProv}

**文件：** AccessEnabler/headers/AccessEnabler.h

**描述：** 您的应用程序调用此方法以告知访问启用程序用户的MVPD选择。 应用程序可以使用此方法选择或更改用于身份验证的服务提供程序。

如果所选MVPD是TempPass MVPD，它随后将自动通过该MVPD进行身份验证，而无需调用getAuthentication()。

请注意，这不适用于提升临时传递，因为getAuthentication()方法提供了额外的参数。

传递时 *null* 作为参数， Access Enabler假定用户已取消身份验证流程（即按下“后退”按钮），并通过重置身份验证状态机和调用 [setAuthenticationStatus:errorCode:](#setAuthNStatus) 使用进行回调 `AccessEnabler.PROVIDER_NOT_SELECTED_ERROR` 错误代码。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API调用：设置当前选定的提供程序</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setSelectedProvider:(NSString *)mvpdId;</code></pre></td>
</tr>
</tbody>
</table>

**可用性：** v1.0+

**参数：** 无

**触发的回调：** ` setAuthenticationStatus:errorCode:,sendTrackingData:forEventType:,  `[`navigateToUrl:`](#nav2url)

[返回页首……](#apis)

</br>

#### navigateToUrl： {#nav2url}

**文件：** AccessEnabler/headers/EntitlementDelegate.h

**描述：** AccessEnabler触发的回调，用于请求应用程序实例化UIWebView/WKWebView控制器并加载回调中提供的URL **`url`** 参数。 回调通过 **`url`** 表示身份验证端点的URL或注销端点的URL的参数。

作为UIWebView/WKWebView` `控制器经过多次重定向，您的应用程序必须监控控制器的活动，并检测它何时加载由定义的特定自定义URL。 `ADOBEPASS_REDIRECT_URL `常量(即 `adobepass://ios.app`)。 请注意，此特定自定义URL实际上无效，控制器不会将其实际加载。 必须由应用程序将其解释为验证或注销流程已完成，并且关闭控制器是安全的信号。 当控制器加载此特定自定义URL时，应用程序必须关闭UIWebView/WKWebView并调用AccessEnabler的 `handleExternalURL:url `API方法。

**注意：** 请注意，对于身份验证流程，用户可以按下“返回”按钮，这相当于身份验证流程中止。 在这种情况下，应用程序需要调用 [setSelectedProvider：](#setSelProv) 方法传递 **`nil`** 作为参数，并让AccessEnabler有机会重置其身份验证状态计算机。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>“回调：显示MVPD登录”页</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) navigateToUrl:(NSString *)url; </code></pre></td>
</tr>
</tbody>
</table>

**可用性：** v1.0+

**参数**：

* *url*：指向MVPD登录页面的URL

**触发者：** [setSelectedProvider：](#setSelProv)



[返回页首……](#apis)

</br>

#### navigateToUrl:useSVC: {#nav2urlSVC}

**文件：** AccessEnabler/headers/EntitlementDelegate.h

**描述：** 由AccessEnabler触发的回调，而不是 `navigateToUrl:` 如果您的应用程序之前通过启用了手动Safari视图控制器(SVC)处理，请回调 [setoptions(\[&quot;handleSVC&quot;：true&quot;\])](#setOptions) 调用，并且仅在MVPD需要Safari视图控制器(SVC)的情况下调用。 对于所有其他MVPD ，使用 `navigateToUrl:` 将调用callback。 请参阅[iOS SDK 3.2+上的SFSafariViewController支持](/help/authentication/sfsafariviewcontroller-support-on-ios-sdk-32.md) 有关如何管理Safari视图控制器(SVC)的详细信息。

类似于 `navigateToUrl:` 回调 `navigateToUrl:useSVC:` 由AccessEnabler触发，请求您的应用程序实例化 `SFSafariViewController` 控制器和来加载回调中提供的URL **`url`** 参数。 回调通过 **`url`** 表示身份验证端点的URL或注销端点的URL的参数，以及 **`useSVC`** 参数，指定应用程序必须使用 `SFSafariViewController`.

作为 `SFSafariViewController` 控制器经过多次重定向，您的应用程序必须监控控制器的活动，并检测它加载由您定义的特定自定义URL的时刻。 `application's custom scheme` (例如** **`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`)。 请注意，此特定自定义URL实际上无效，控制器不会将其实际加载。 必须由应用程序将其解释为验证或注销流程已完成，并且关闭控制器是安全的信号。 当控制器加载该特定自定义URL时，应用程序必须关闭 `SFSafariViewController` 并调用AccessEnabler的 `handleExternalURL:url `API方法。

**注意：** 请注意，对于身份验证流程，用户可以按下“返回”按钮，这相当于身份验证流程中止。 在这种情况下，应用程序需要调用 [setSelectedProvider：](#setSelProv) 方法传递 **`nil`** 作为参数，并让AccessEnabler有机会重置其身份验证状态计算机。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>回调：在SFSafariViewController中显示MVPD登录页</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>@optional
- (void) navigateToUrl:(NSString *)url useSVC:(BOOL)useSVC; </code></pre></td>
</tr>
</tbody>
</table>

**可用性：**v 3.2+

**参数**：

* *url：* 指向MVPD登录页面的URL
* *useSVC：* url是否应加载到SFSafariViewController中。

**触发者：**[ setOptions：](#setOptions) 早于 [setSelectedProvider：](#setSelProv)

[返回页首……](#apis)

</br>

#### handleExternalURL：url {#handleExternalURL}

**文件：** AccessEnabler/headers/AccessEnabler.h

**描述：** 应用程序调用此方法以完成身份验证或注销流程。 当应用程序检测到以下情况时，应立即调用此方法： `UIWebView/WKWebView or SFSafariViewController` 控制器将被重定向到特定的自定义URL。 如果您的应用程序需要使用 `SFSafariViewController `控制器特定的自定义URL由贵机构的 `application's custom scheme` (例如`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`)，否则此特定自定义URL将由 `ADOBEPASS_REDIRECT_URL `常量(即 `adobepass://ios.app`)。

在身份验证流中，AccessEnabler通过从后端服务器检索身份验证令牌并将其本地存储在令牌存储中来完成该流。 AccessEnabler将通过调用 `setAuthenticationStatus()`<!--(http://tve.helpdocsonline.com/ios-technical-overview#setAuthNStatus)--> 带状态代码1的回调，表示成功。 如果在执行这些步骤的过程中出现错误， `setAuthenticationStatus()`<!--(http://tve.helpdocsonline.com/ios-technical-overview#setAuthNStatus)--> 回调会以状态代码0触发，指示身份验证失败以及相应的错误代码。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API调用：完成身份验证或注销流程</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code> (void) handleExternalURL:(NSString *)url; </code></pre></td>
</tr>
</tbody>
</table>

**可用性：** v3.0+

**参数：**

* *url*：截获的URL ` UIWebView/WKWebView or SFSafariViewController ` 控件。


**触发的回调：** `setAuthenticationStatus:errorCode, sendTrackingData:forEventType:`

[返回页首……](#apis)

</br>

#### getAuthenticationToken - [已弃用] {#getAuthNToken}

**文件：** AccessEnabler/headers/AccessEnabler.h

**描述：** 通过从后端服务器请求身份验证令牌来完成身份验证流程。 仅当发生以下情况时，应用程序才应调用此方法：托管MVPD登录页面的WebView控件被重定向到由定义的自定义URL `ADOBEPASS_REDIRECT_URL` 常量。


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API调用：检索身份验证令牌</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthenticationToken; </code></pre></td>
</tr>
</tbody>
</table>

**可用性：** v1.0+ **直到：** v3.0

**参数：** 无

**触发的回调：** `setAuthenticationStatus:errorCode,sendTrackingData:forEventType:`

[返回页首……](#apis)

&lt;/br

#### setAuthenticationStatus:errorCode: {#setAuthNStatus}

**文件：** AccessEnabler/headers/EntitlementDelegate.h

**描述** 由AccessEnabler触发的回调，通知应用程序身份验证流的状态。 在很多地方，由于用户的交互或其他不可预见的情形（如网络连接问题等），此流量可能会失败。 此回调会通知应用程序身份验证流的成功/失败状态，同时还会在需要时提供有关失败原因的其他信息。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>回调：报告身份验证流的状态</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setAuthenticationStatus:(int)status 
                       errorCode:(NSString *)code; </code></pre></td>
</tr>
</tbody>
</table>


**可用性：** v1.0+

**参数**：

* *状态*：可以采用以下任一值：
   * `ACCESS_ENABLER_STATUS_SUCCESS`  — 身份验证流程已成功完成
   * `ACCESS_ENABLER_STATUS_ERROR`  — 身份验证流失败
* *代码*：失败原因。 如果 *状态* 是 `ACCESS_ENABLER_STATUS_SUCCESS`，则 *代码* 是一个空字符串(即，由 `USER_AUTHENTICATED` 常量)。 如果失败，此参数可以采用以下值之一：
   * `USER_NOT_AUTHENTICATED_ERROR`  — 用户未经过身份验证。 回应 [checkAuthentication：](#checkAuthN) 当本地令牌缓存中没有有效的身份验证令牌时，方法调用。
   * `PROVIDER_NOT_SELECTED_ERROR` - AccessEnabler在上层应用程序通过后重置了身份验证状态计算机 *null* 到 [`setSelectedProvider:`](#setSelProv) 中止身份验证流程。  用户可能已取消身份验证流程（例如，已按下“后退”按钮）。
   * `GENERIC_AUTHENTICATION_ERROR`  — 由于网络不可用或用户明确取消身份验证流等原因，身份验证流失败。

**触发者：** ` checkAuthentication, getAuthentication, `[getAuthentication:withData:](#getAuthN)，` checkAuthorization:, `[checkAuthorization:withData:](#checkAuthZ)

[返回页首……](#apis)

</br>

### checkPreauthorizedResources： {#checkPreauth}


**文件：** AccessEnabler/headers/AccessEnabler.h

**描述：** 应用程序使用此方法来确定用户是否已获得查看特定受保护资源的授权。 此方法的主要目的是检索用于装饰UI的信息 **（例如，使用锁定和解锁图标指示访问状态）。**

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API调用：设置当前选定的提供程序</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkPreauthorizedResources:(NSArray *)resources; </code></pre></td>
</tr>
</tbody>
</table>

**可用性：** v1.3+

**参数：**

* *资源：* 应检查其授权的资源数组。 列表中的每个元素都应该是一个表示资源ID的字符串。 资源ID受到与调用中的资源ID相同的限制，即，它应该是程序员和MVPD或媒体RSS片段之间建立的商定值。

**已触发回调：** [`preauthorizedResources:`](#preauthResources)

[返回页首……](#apis)

</br>

### checkPreauthorizedResources:cache: {#checkPreauthCache}

**文件：** AccessEnabler/headers/AccessEnabler.h

**描述：** 应用程序使用此方法来确定用户是否已获得查看特定受保护资源的授权。 此方法的主要目的是检索用于装饰UI的信息（例如，使用锁定和解锁图标指示访问状态）。 此 **缓存** 参数控制内部缓存是否用于解析资源。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API调用：设置当前选定的提供程序</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkPreauthorizedResources:(NSArray *)resources cache:(BOOL)cache; </code></pre></td>
</tr>
</tbody>
</table>



**可用性：** v3.1+



**参数：**

* *资源：* 应检查其授权的资源数组。 列表中的每个元素都应该是一个表示资源ID的字符串。 资源ID与中的资源ID须遵守相同的限制 `getAuthorization:` 调用，即它应为程序员与MVPD或媒体RSS片段之间建立的商定值。
* *缓存：* 布尔值，指定是否使用内部缓存来解析资源。 如果为false，将绕过缓存，导致每次调用此API时都进行服务器调用。

**已触发回调：** [`preauthorizedResources:`](#preauthResources)

[返回页首……](#apis)

</br>

### 预授权资源： {#preauthResources}

**文件：** AccessEnabler/headers/EntitlementDelegate.h

**描述：** 回调触发方式 `checkPreauthorizedResources:`. 提供用户已被授权查看的资源列表。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API调用：设置当前选定的提供程序</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkPreauthorizedResources:(NSArray *)resources; </code></pre></td>
</tr>
</tbody>
</table>

**可用性：** v1.3+

**参数：**

* `resources`：用户已被授权查看的资源数组。

**触发者：** [`checkPreauthorizedResources:`](#checkPreauth)



[返回页首……](#apis)

</br>

### checkAuthorization：， checkAuthorization:withData: {#checkAuthZ}

**文件：** AccessEnabler/headers/AccessEnabler.h

**描述：** 应用程序使用此方法来检查授权状态。 首先检查身份验证状态。 如果未经过身份验证， [tokenRequestFailed:errorCode:errorDescription：](#tokenReqFailed) 会触发回调，并且方法退出。 如果用户通过了身份验证，它还会触发授权流。 欲知详情，请参阅 [`getAuthorization:`](#getAuthZ) 方法。


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API调用：检查授权状态</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkAuthorization:(NSString *)resource; </code></pre></td>
</tr>
</tbody>
</table>

**可用性：** v1.0+

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API调用：检查授权状态</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkAuthorization:(NSString *)resource:
                   withData:(NSDictionary *)data; </code></pre></td>
</tr>
</tbody>
</table>

**可用性：** v1.9+

**参数：**

* *资源*：用户请求授权的资源的ID。
* *数据*：包含要发送到Pay-TV密码服务的键值对的词典。 Adobe可以使用此数据启用未来的功能，而无需更改SDK。

**触发的回调：**
[tokenRequestFailed:errorCode:errorDescription：](#tokenReqFailed)`,setToken:forResource:, sendTrackingData:forEventType:, setAuthenticationStatus:errorCode:`

[返回页首……](#apis)

</br>

### getAuthorization：， getAuthorization:withData: {#getAuthZ}

**文件：** AccessEnabler/headers/AccessEnabler.h

**描述：** 应用程序使用此方法来启动授权流。 如果用户尚未经过身份验证，它还会启动身份验证流程。 如果用户通过了身份验证，AccessEnabler将继续发出对授权令牌（如果本地令牌缓存中不存在有效的授权令牌）和短期媒体令牌的请求。 获得短媒体令牌后，授权流程即被视为完成。 此 [setToken:forResource:](#setToken) 会触发回调，并将短媒体令牌作为参数提供给应用程序。 如果由于任何原因，授权失败， [tokenRequestFailed:forEventType:](#tokenReqFailed) 会触发回调，并提供错误代码/详细信息。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API调用：启动授权流</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthorization:(NSString *)resource; </code></pre></td>
</tr>
</tbody>
</table>

**可用性：** v1.0+

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API调用：启动授权流</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthorization:(NSString *)resource:
                 withData:(NSDictionary *)data; </code></pre></td>
</tr>
</tbody>
</table>



**可用性：** v1.9+

**参数：**

* *资源*：用户请求授权的资源的ID。
* *数据*：包含要发送到Pay-TV密码服务的键值对的词典。 Adobe可以使用此数据启用未来的功能，而无需更改SDK。

**触发的回调：** `tokenRequestFailed:errorCode:errorDescription:, setToken:forResource:,sendTrackingData:forEventType:`

**触发的其他回调：**\
此方法还可以触发以下回调（如果还启动了身份验证流程）： `setAuthenticationStatus:errorCode:`， `displayProviderDialog:`

**注意：请使用checkAuthorization： / checkAuthorization:withData: 而不是getAuthorization： / getAuthorization:withData: 尽可能。 getAuthorization： / getAuthorization:withData: 方法将启动一个完整的身份验证流程（如果用户未经身份验证），这可能会导致程序员端的复杂实施。**

[返回页首……](#apis)

</br>

### setToken:forResource: {#setToken}

**文件：** AccessEnabler/headers/EntitlementDelegate.h

**描述** 由AccessEnabler触发的回调，通知应用程序授权流已成功完成。 短期媒体令牌还会作为参数提供。


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>回调：授权流已成功完成</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setToken:(NSString *)token 
      forResource:(NSString *)resource; </code></pre></td>
</tr>
</tbody>
</table>

**可用性：** v1.0+

**参数**：

* *令牌*：短期媒体令牌
* *资源*：获得授权的资源

**触发者：** [checkAuthorization：](#checkAuthZ)` , `[checkAuthorization:withData:](#checkAuthZ)，` `[getAuthorization：](#getAuthZ)， [getAuthorization:withData:](#getAuthZ)

[返回页首……](#apis)

</br>

### tokenRequestFailed:errorCode:errorDescription： {#tokenReqFailed}

**文件：** AccessEnabler/headers/EntitlementDelegate.h

**描述** 由AccessEnabler触发的回调，通知上层应用程序授权流失败。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>回调：授权流失败</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) tokenRequestFailed:(NSString *)resource 
                  errorCode:(NSString *)code 
           errorDescription:(NSString *)description; </code></pre></td>
</tr>
</tbody>
</table>

**可用性：** v1.0+

**参数**：

* *资源*：获得授权的资源。
* *代码*：与失败方案关联的错误代码。 可能的值：
   * `USER_NOT_AUTHORIZED_ERROR`  — 用户无法授权给定资源
* *描述*：有关失败场景的其他详细信息。 如果此描述性字符串由于任何原因不可用，Adobe Pass身份验证将发送空字符串 **(“”)**.\
  MVPD可使用此字符串传递自定义错误消息或与销售相关的消息。 例如，如果订阅者被拒绝对资源的授权，则MVPD会发送消息，例如：“您当前在包中无法访问此渠道。 如果要升级包，请单击 **此处**“ 消息由Adobe Pass身份验证通过此回调传递给程序员，程序员可以选择显示或忽略该消息。 Adobe Pass身份验证还可以使用此参数来提供可能导致错误的状况通知。 例如，“与提供商的授权服务通信时出现网络错误”。

**触发者：** ` checkAuthorization:, `[checkAuthorization:withData:](#checkAuthZ)， `getAuthorization:, `[getAuthorization:withData:](#getAuthZ)

[返回页首……](#apis)

</br>

### 注销 {#logout}

**文件：** AccessEnabler/headers/AccessEnabler.h

**描述：** 应用程序调用此方法以启动注销流程。 注销是一系列HTTP重定向操作的结果，这是因为用户既需要从Adobe Pass身份验证服务器注销，也需要从MVPD服务器注销。 由于使用AccessEnabler库发出的简单HTTP请求无法完成此流程，因此 `UIWebView/WKWebView or SFSafariViewController` 控制器需要实例化才能遵循HTTP重定向操作。

注销流与身份验证流的不同之处在于，用户不需要与 `UIWebView/WKWebView or SFSafariViewController`  控制者。 因此，Adobe建议您在注销过程中使控件不可见（即隐藏）。

采用类似于验证流的模式。 iOS AccessEnabler触发 `navigateToUrl:` 回调或 `navigateToUrl:useSVC:` 创建 `UIWebView/WKWebView or SFSafariViewController` 控制器和来加载回调中提供的URL `url` 参数。 这是后端服务器上注销端点的URL。 对于tvOS AccessEnabler，两者都不是 `navigateToUrl:` 回调或 `navigateToUrl:useSVC:` 调用回调。

在经历多次重定向时，您的应用程序必须监控 `UIWebView/WKWebView or SFSafariViewController `控制器并检测其加载特定自定义URL的时刻。 请注意，此特定自定义URL实际上无效，控制器不会将其实际加载。 必须由应用程序将其解释为注销流程已完成并且关闭控制器是安全的信号。 当控制器加载此特定自定义URL时，应用程序必须关闭控制器并调用AccessEnabler的 `handleExternalURL:url `API方法。 如果您的应用程序需要使用 `SFSafariViewController `控制器特定的自定义URL由贵机构的 `application's custom scheme` (例如`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`)，否则此特定自定义URL将由 `ADOBEPASS_REDIRECT_URL `常量(即 `adobepass://ios.app`)。

最后，AccessEnabler将调用 [`setAuthenticationStatus()`](#setAuthNStatus) 带状态代码0的回调，表示注销流成功。

**注意：** 如果用户使用Apple SSO登录，则会触发VSA203状态。 如果是这种情况，应指示用户也从系统设置注销。 如果未能这样做，则会在应用程序重新启动时导致重新身份验证。


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API调用：启动注销流程</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) logout; </code></pre></td>
</tr>
</tbody>
</table>

**可用性：** v1.0+

**参数：** 无

**触发的回调：** `navigateToUrl:, `[`setAuthenticationStatus:errorCode:`](#setAuthNStatus)



[返回页首……](#apis)

</br>

### getselectedprovider {#getSelProv}

**文件：** AccessEnabler/headers/AccessEnabler.h

**描述：** 使用此方法可确定当前选择的提供程序。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API调用：确定当前选定的MVPD</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getSelectedProvider; </code></pre></td>
</tr>
</tbody>
</table>

**可用性：** v1.0+

**参数：** 无

**触发的回调：** [`selectedProvider:`](#selProv)

[返回页首……](#apis)

</br>

### selectedprovider {#selProv}

**文件：** AccessEnabler/headers/EntitlementDelegate.h

**描述** 由AccessEnabler触发的回调，它将有关当前所选MVPD的信息传递给应用程序。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>回调：有关当前所选MVPD的信息</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) selectedProvider:(MVPD *)mvpd;</code></pre></td>
</tr>
</tbody>
</table>

**可用性：** v1.0+

**参数**：

* *mvpd*：包含有关当前所选MVPD信息的对象

**触发者：** [`getSelectedProvider`](#getSelProv)

[返回页首……](#apis)

</br>

### getMetadata： {#getMeta}

**文件：** AccessEnabler/headers/AccessEnabler.h

**描述：** 使用此方法可检索AccessEnabler库公开为元数据的信息。 应用程序可以通过提供基于字典的数据来访问此数据 *键* 输入参数。

有两种类型的元数据可供程序员使用：

* 静态元数据（身份验证令牌TTL、授权令牌TTL和设备ID）
* 用户元数据（特定于用户的信息，例如用户ID、邮政编码；可以在身份验证和授权流期间从MVPD传递到用户设备）

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>API调用：查询AccessEnabler以获取元数据</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getMetadata:(NSDictionary *)keyDictionary; </code></pre></td>
</tr>
</tbody>
</table>

**可用性：** v1.0+

**参数：**

* *keyDictionary*：字典数据结构，格式如下：
   * 如果键为 `METADATA_OPCODE_KEY` 而值是 `METADATA_AUTHENTICATION`，然后进行查询以获取身份验证令牌过期时间。
   * 如果键为 `METADATA_OPCODE_KEY` 而值是 `METADATA_AUTHORIZATION` **和**\
     键是 `METADATA_RESOURCE_ID_KEY` 并且值是特定的资源ID，然后进行查询以获取与指定资源关联的授权令牌的过期时间。
   * 如果键为 `METADATA_OPCODE_KEY` 而值是 `METADATA_DEVICE_ID`，然后执行查询以获取当前设备id。 请注意，此功能默认处于禁用状态，程序员应联系Adobe以获取有关启用和费用的信息。
   * 如果键为 `METADATA_OPCODE_KEY` 而值是 `METADATA_USER_META` **和** 键是 `METADATA_USER_META_KEY` 而value是元数据的名称，则会对用户元数据进行查询。 可用用户元数据类型的列表：
      * `zip`  — 邮政编码列表
      * `householdID`  — 家庭标识符。 如果MVPD不支持子帐户，则它与 `userID`.
      * `maxRating`  — 用户的最大家长分级的集合
      * `userID`  — 用户标识符。 如果MVPD支持子帐户，并且用户不是主帐户， `userID` 将不同 `householdID.`
      * `channelID`  — 用户有权查看的渠道列表。

  >[!NOTE]
  >
  >程序员可用的实际用户元数据取决于MVPD提供的内容。 此列表将随着新元数据的发布和添加到Adobe Pass身份验证系统中而展开。

**触发的回调：** [`setMetadataStatus:encrypted:forKey:andArguments:`](#setMetaStatus)

**更多信息：** [用户元数据](/help/authentication/user-metadata.md)

[返回页首……](#apis)

</br>

### presentTVProviderDialog {#presentTvDialog}

**文件：** AccessEnabler/headers/EntitlementDelegate.h

**描述** AccessEnabler在调用后触发的回调[getAuthentication()](#getAuthN) 如果当前请求者至少支持一个支持SSO的MVPD。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>回调：SSO流的结果</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) presentTvProviderDialog: (UIViewController *) viewController; </code></pre></td>
</tr>
</tbody>
</table>

**可用性：** v2.0+

**参数**：

* viewController：表示Apple SSO对话框。 此viewController需要显示在屏幕上。

**触发者：** [`getAuthentication`](#getAuthN)

**更多信息：** [iOS/tvOS单点登录](#presentTvDialog)

[返回页首……](#apis)

</br>

### dissiseTVProviderDialog {#dismissTvDialog}

**文件：** AccessEnabler/headers/EntitlementDelegate.h

**描述** 用户关闭Apple SSO对话框后，AccessEnabler触发的回调。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>回调：SSO流的结果</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) dismissTvProviderDialog: (UIViewController *) viewController; </code></pre></td>
</tr>
</tbody>
</table>

**可用性：** v2.0+

**参数**：

* viewController：表示Apple SSO对话框。 需要从屏幕中移除此viewController。

**触发者：** 用户操作

**更多信息：** [iOS/tvOS单点登录](#presentTvDialog)

[返回页首……](#apis)

</br>

### setmetadatastatus:encrypted:forKey:andArguments: {#setMetaStatus}

**文件：** AccessEnabler/headers/EntitlementDelegate.h

**描述** 由AccessEnabler触发的回调，它通过 [`getMetadata:`](#getMeta) 呼叫。

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>回调：元数据检索请求的结果</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setMetadataStatus:(id)metadata
                 encrypted:(bool)encrypted 
                    forKey:(int)key 
              andArguments:(NSDictionary *)arguments; </code></pre></td>
</tr>
</tbody>
</table>

**可用性：** v1.0+

**参数**：

* *元数据*：请求的元数据。 此值是一个 `NSString` 对于静态元数据（身份验证TTL、授权TTL、设备ID）。  请求特定于用户的元数据时，这是一个复杂的对象。 此复杂对象通常是JSON有效负载的Objective-C表示形式(例如&#39;{&quot;street&quot;： &quot;Main Avenue&quot;， &quot;builders&quot;： [“150”、“320”]}&#39;在Objective-C中翻译为NSDictionary(&quot;street&quot; -> &quot;Main Avenue&quot;， &quot;buildings&quot; -> NSArray(&quot;150&quot;， &quot;320&quot;))。   示例元数据JSON对象：

```JSON
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
```

* *已加密*：布尔值，它指定是否对检索到的元数据加密。 此参数仅对用户元数据请求有意义，对于始终未加密接收的静态元数据（例如，身份验证TTL）没有意义。 如果此参数设置为true，则程序员需要通过使用白名单私钥（与在中签名请求者ID时使用的私钥）执行RSA解密来获取未加密的用户元数据值。 [`setRequestor:setSignedRequestorId:`](#setReq) 和 `setRequestor:setSignedRequestorId:serviceProviders: `调用)。

* *键*：用于制定元数据检索请求的键。

* *参数*：传递给的同一词典 [`getMetadata:`](#getMeta) 呼叫。 提供此操作是为了让应用程序能够将请求与响应进行匹配。

**触发者：** [`getMetadata:`](#getMeta)

**更多信息：** [用户元数据](/help/authentication/user-metadata.md)


[返回页首……](#apis)

</br>

### MVPD {#mvpd}

**文件：** AccessEnabler/headers/model/MVPD.h

**描述** 描述MVPD对象。 可用于获取有关MVPD属性的信息。

**可用性：** v1.0+ [boardingStatus属性在v2.2中提供]

**属性**：

* (NSString) ID - MVPD ID。
* (NSString) displayName - MVPD名称。 [这应该用于显示在选取器中]
* (NSString) logoURL - MVPD徽标地址。
* (BOOL) enablePlatformServices — 如果为true，则MVPD支持SSO服务，例如 [APPLE SSO](#presentTvDialog).
* (NSString) boardingStatus — 可以有3个值：
   * 无 — MVPD不支持Apple SSO。
   * 选取器 — MVPD可显示在Apple选取器中，但身份验证流程由Adobe完成。
   * 支持 — Apple完全支持MVPD，它将使用Apple的SSO令牌。

[返回页首……](#apis)

</br>

## 跟踪事件 {#tracking}

AccessEnabler会触发一个附加回调，该回调不一定与权利文件流相关。 实施 [`sendTrackingData()`](#sendTracking) 回调函数是可选的，但它使应用程序能够跟踪特定事件并编译统计信息，如成功/失败的身份验证/授权尝试次数。

### sendTrackingData:forEventType: {#sendTracking}

**文件：** AccessEnabler/headers/EntitlementDelegate.h

**描述** 由AccessEnabler向应用程序发出各种事件（如身份验证/授权流的完成/失败）的信号所触发的回调。 使用Adobe Pass Authentication 1.6时，设备类型、AccessEnabler客户端类型和操作系统由报告 [`sendTrackingData()`](#sendTracking). 此 [`sendTrackingData()`](#sendTracking) callback保持向后兼容。

**回调：跟踪事件**

```
(void) sendTrackingData:(NSArray *)data 
             forEventType:(int)event;
```

**可用性：** v1.0+

**注意：** 设备类型和操作系统是通过使用公共Java库(<http://java.net/projects/user-agent-utils>)和用户代理字符串。 请注意，此信息仅以粗略的方式提供，用于按设备类别细分操作量度，但该Adobe对错误结果不承担任何责任。 请相应地使用新功能。

* 设备类型的可能值：
   * `computer`
   * `tablet`
   * `mobile`
   * `gameconsole`
   * `unknown`

* AccessEnabler客户端类型的可能值：
   * `flash`
   * `html5`
   * `ios`
   * `android`


**参数**：

* *事件*：被跟踪的事件的代码。 有三种可能的跟踪事件类型：
   * **授权检测：** 每当授权令牌请求返回时(事件为 `TRACKING_AUTHORIZATION`)
   * **authenticationDetection：** 每当进行身份验证检查时(事件为 `TRACKING_AUTHENTICATION`)
   * **mvpdSelection：** 当用户在MVPD选择表单中选择MVPD时(事件为 `TRACKING_GET_SELECTED_PROVIDER`)
* *数据*：与所报告事件关联的其他数据。 此数据以值列表的形式提供。

**触发者：** `checkAuthentication, getAuthentication, `[getAuthentication:withData:](#getAuthN)， `checkAuthorization:, `[checkAuthorization:withData:](#checkAuthZ)， `getAuthorization:, `[getAuthorization:withData:](#getAuthZ)， `setSelectedProvider:`

有关解释 *数据* 数组：

* 对于trackingEventType `TRACKING_AUTHENTICATION:`
   * **0**  — 令牌请求是否成功(true/false)，如果成功：
   * **1** - MVPD ID字符串
   * **2** - GUID（md5散列）
   * **3**  — 令牌已在缓存中(true/false)
   * **4**  — 设备类型
   * **5** - AccessEnabler客户端类型
   * **6**  — 操作系统类型

* 对于trackingEventType `TRACKING_AUTHORIZATION:`
   * **0**  — 令牌请求是否成功(true/false)，如果成功：
   * **1** - MVPD ID
   * **2** - GUID（md5散列）
   * **3**  — 令牌已在缓存中(true/false)
   * **4**  — 错误
   * **5**  — 详细信息
   * **6**  — 设备类型
   * **7** - AccessEnabler客户端类型
   * **8**  — 操作系统类型
* 对于trackingEventType `TRACKING_GET_SELECTED_PROVIDER:`
   * **0**  — 当前所选MVPD的ID
   * **1**  — 设备类型
   * **2** - AccessEnabler客户端类型
   * **3**  — 操作系统类型

</br>
