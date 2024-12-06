---
title: 程序员权利流
description: 程序员权利流
exl-id: b1c8623a-55da-4b7b-9827-73a9fe90ebac
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1823'
ht-degree: 0%

---

# 程序员权利流 {#prog-entitlement-flow}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## 概述 {#overview}

本文档从程序员的角度描述了基本权利流程。  有关此处介绍的基本TVE集成之外的功能和用例的信息，请参阅[程序员用例](/help/authentication/integration-guide-programmers/programmer-use-cases.md)。

Adobe Pass身份验证通过提供安全、一致的界面为双方协调程序员和MVPD之间的权限流。  在程序员方面，Adobe Pass身份验证提供两种常规类型的权利文件界面：

1. AccessEnabler — 一个客户端组件，为可以呈现网页的设备（例如，Web应用程序、智能手机/平板电脑应用程序）上的应用程序提供API库。
2. 无客户端API — 用于无法呈现网页的设备（例如，机顶盒、游戏机、智能电视）的RESTful Web服务。 呈现网页的要求来自MVPD要求用户在MVPD的网站上进行身份验证。

除了此处提供的平台中性概述之外，此处还提供了特定于无客户端API的概述：无客户端API文档。 AccessEnabler在支持的平台(Web上的AS/JS、iOS上的Objective-C和Android上的Java)上原生运行。 AccessEnabler API在支持的平台上是一致的。 所有不支持AccessEnabler的平台都使用相同的无客户端API。

对于这两种类型的界面，Adobe Pass身份验证都能安全地协调程序员应用程序与用户的MVPD之间的授权流：

![](../assets/prog-entitlement-flow.png)


*图： Adobe Pass身份验证生态系统*

>[!IMPORTANT]
>
>请注意，在上图中，授权流的一部分不会通过Adobe Pass身份验证服务器：MVPD登录。 用户必须登录到其MVPD的登录页面。 由于此要求，在无法呈现网页的设备上，程序员的应用程序必须指示用户切换到支持Web的设备以使用其MVPD登录，然后用户返回原始设备以剩余权利流。

## 权利流 {#entitlement-flow}

基本权利由四个不同的子流组成
流量：

1. [启动流程](/help/authentication/integration-guide-programmers/entitlement-flow.md#startup)
1. [身份验证流程](/help/authentication/integration-guide-programmers/entitlement-flow.md#authentication)
1. [授权流程](/help/authentication/integration-guide-programmers/entitlement-flow.md#authorization)
1. [注销流程](/help/authentication/integration-guide-programmers/entitlement-flow.md#logout)

当用户首次访问程序员网站时，权利流将按上述顺序进行。 但是，在后续访问中，根据身份验证和授权令牌是否过期，或者根据查看策略，用户可能只能通过子流中的一个或两个。

### 启动流程 {#startup}

确定程序员和设备的身份，执行初始化任务。 这是所有后续权利调用的先决条件。

**AccessEnabler**

* **`setRequestor()`** — 使用AccessEnalber以及Adobe Pass身份验证服务器扩展来建立您的身份。 这一呼叫是剩余权利流的前奏。 例如，在JavaScript中：

  ```JavaScript
    /* Define the requestor ID (Programmer/aggregator ID). */
      var requestorID = "sample_requestor_Id";
      ...
      // Callback indicating that the AccessEnabler swf has initialized
      function swfLoaded() {
          // AccessEnabler is loaded so we can use the API function it provides
          accessEnablerObject.setRequestor(requestorID); 
      ...
      }
  ```

**无客户端API**

* **`\<REGGIE\_FQDN\>/reggie/v1/{requestorId}/regcode`** — 根据平台，在应用程序调用regcode之前，可能需要完成一些先决任务。 有关详细信息，请参阅&#x200B;**无客户端API文档**。 例如，Xbox平台要求您在调用regcode之前完成规定的安全步骤。

### 身份验证流程 {#authentication}

成功的身份验证会生成一个与设备和请求者绑定的AuthN令牌。 身份验证成功是获得授权的先决条件。

**AccessEnabler**

* `checkAuthentication()` — 检查本地令牌缓存中是否存在有效的缓存身份验证令牌，而不实际触发完整的身份验证流程。 这会触发`setAuthenticationStatus()`回调函数。
* `getAuthentication()` — 启动完整的身份验证流程。 如果成功，Adobe Pass身份验证会生成一个AuthN令牌，并在客户端上缓存该令牌。 用户在其选定的MVPD站点上登录，该站点在iFrame、弹出窗口或Webview中显示，具体取决于平台。 这会触发displayProviderDialog()。

**无客户端API**

* `<FQDN>/.../checkauthn` — 上面的`checkAuthentication()`的Web服务版本。
* `<FQDN>/.../config` — 向第二屏应用程序返回MVPD列表。
* `<FQDN>/.../authenticate` — 从第二屏应用程序启动身份验证流程，将用户重定向到其选定的MVPD以供登录。 如果成功，Adobe Pass身份验证会生成一个AuthN令牌并将其存储在服务器上，然后用户会返回到其原始设备以完成授权流。

如果以下两点为true，则AuthN令牌被视为有效：

* AuthN令牌未过期
* 与AuthN令牌关联的MVPD位于当前请求者ID的允许的MVPD列表中

#### 通用AccessEnabler初始身份验证工作流 {#generic-ae-initial-authn-flow}

1. 您的应用程序通过调用`getAuthentication()`来启动身份验证工作流，该调用会检查缓存的有效身份验证令牌。 此方法具有可选的`redirectURL`参数；如果不提供`redirectURL`的值，则在成功进行身份验证后，用户将返回到初始化身份验证的URL。
1. AccessEnabler确定当前的身份验证状态。 如果用户当前已通过身份验证，则AccessEnabler将调用您的`setAuthenticationStatus()`回调函数，传递一个身份验证状态以指示成功。
1. 如果用户未经过身份验证，则AccessEnabler通过确定用户的最后一次身份验证尝试是否成功使用给定的MVPD来继续身份验证流程。 如果已缓存MVPD ID并且`canAuthenticate`标志为true或使用`setSelectedProvider()`选择了MVPD，则不会使用MVPD选择对话框提示用户。 身份验证流继续使用MVPD的缓存值（即在上次成功身份验证期间使用的MVPD值）。 对后端服务器进行网络调用，用户将被重定向到MVPD登录页面。

1. 如果未缓存MVPD ID，并且未使用`setSelectedProvider()`选择MVPD，或者`canAuthenticate`标志设置为false，则调用`displayProviderDialog()`回调。 此回调会指示您的应用程序创建UI，向用户显示可供选择的MVPD列表。 提供了一个MVPD对象数组，其中包含构建MVPD选择器所需的信息。 每个MVPD对象描述一个MVPD实体，并包含MVPD的ID和可以找到MVPD徽标的URL等信息。

1. 选择MVPD后，您的应用程序必须通知AccessEnabler用户所做的选择。 对于非Flash客户端，一旦用户选择了所需的MVPD，您就会通过调用`setSelectedProvider()`方法向AccessEnabler通知用户选择的内容。 Flash客户端改为调度类型为“`mvpdSelection`”的共享`MVPDEvent`，传递选定的提供程序。

1. 当通知AccessEnabler用户的MVPD选择时，将对后端服务器进行网络调用，并将用户重定向到MVPD登录页面。

1. 在身份验证工作流中，AccessEnabler与Adobe Pass Authentication和选定的MVPD进行通信，以请求用户的凭据（用户ID和密码）并验证其身份。 虽然有些MVPD会重定向到其自己的站点以进行登录，但另一些MVPD则需要您在iFrame中显示其登录页面。 您的页面必须包含创建iFrame的回调，以防客户选择这些MVPD之一。<!-- For more information on creating a login iFrame, see  [Managing the Login IFrame](https://tve.helpdocsonline.com/managing-the-login-iframe)-->

1. 用户成功登录后，AccessEnabler会检索身份验证令牌并通知您的应用程序身份验证流程已完成。 AccessEnabler使用状态代码1调用`setAuthenticationStatus()`回调，表示成功。 如果在执行这些步骤的过程中出现错误，则会触发`setAuthenticationStatus()`回调，状态代码为0，表示身份验证失败以及相应的错误代码。

>[!IMPORTANT]
>Comcast是目前唯一不为徽标提供静态URL的MVPD。 程序员应从[XFINITY开发人员门户](https://developers.xfinity.com/products/tv-everywhere)中提取最新的徽标。
>

### 授权流程 {#authorization}

授权是查看受保护内容的先决条件。 成功的授权会生成一个AuthZ令牌以及一个短期媒体令牌，该令牌会提供给程序员的应用程序用于安全目的。 请注意，要支持授权工作流，您必须以前执行了必要的请求者设置，并且集成了[媒体令牌验证器](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-token-verifier-int.md)。 完成这些操作后，即可启动授权。

当用户请求访问受保护的资源时，您的应用程序会启动授权。 您可以传递一个资源ID，以指定请求的资源（例如，渠道、剧集等）。 您的应用程序首先会检查是否存储了身份验证令牌。 如果找不到，则启动身份验证过程。

**AccessEnabler**

* `checkAuthorization()` — 检查授权而不启动完整的授权流程。 这通常用于更新程序员应用程序UI中显示的状态信息。

* `getAuthorization()` — 启动完整授权流程。

提供以下回调函数以处理
授权调用：

* `setToken()` — 如果身份验证之前成功且授权成功，则AccessEnabler将调用您的`setToken()`回调函数，传递短期媒体令牌，这表示已成功完成Adobe Pass身份验证权利流。 (在允许用户查看受保护的内容之前，程序员的应用程序使用媒体令牌验证器检查媒体令牌的有效性。

* `tokenRequestFailed()` — 如果用户未被授权使用所请求的资源（或者查询由于任何其他原因失败），AccessEnabler将调用此回调函数（以及您自己的错误报告函数），并传递有关失败的详细信息。

**无客户端API**

* `\<FQDN\>/.../authorize` — 启动完整授权流程。

#### 通用AccessEnabler授权工作流 {#generic-ae-authr-wf}

1. 提供一个回调函数，该函数使用`setReqestor()`向Access Enabler注册您分配的程序员GUID。 此回调函数在AccessEnabler
下载成功。

1. 当用户请求访问受保护的资源时，调用`getAuthorization()`。 使用`getAuthorization()`，传递一个指定所请求资源的资源ID（例如，渠道、剧集等）。 AccessEnabler会查找要随授权请求一起传递的缓存身份验证令牌。 如果找不到，则会启动身份验证流程。
1. 提供回调函数以处理授权结果：

   * `setToken()` — 如果授权成功，或者如果用户以前已被授权，则Access Enabler将继续执行授权过程，方法是调用`setToken()`回调函数，传递短期的授权令牌。

   * `tokenRequestFailed()` — 如果用户未被授权使用所请求的资源（或者查询由于任何其他原因而失败），AccessEnabler将调用您已注册的任何错误报告函数以及`tokenRequestFailed()`回调，以传递有关失败的详细信息。

### 注销流程 {#logout}

清除与当前用户权利流关联的令牌和其他数据。

**AccessEnabler**

* `logout()`

**无客户端API**

* `\<FQDN\>/.../logout`

## 了解AccessEnabler行为 {#ae-behavior}

所有AccessEnabler API调用都是异步调用（除了一个例外，如API引用中所述）。 您可以任意次数调用API，但无法有力保证会触发操作
按调用执行的操作将按照调用执行的相同顺序完成。 (当前的Flash Player运行时是一个例外；若不是多线程，它将确保调用*do*按顺序完成
它们被称为。)

为了区分响应，并能够配对响应与调用，所有回调都将回显其输入参数。 这包括`setToken()`和`tokenRequestFailed()`，它们最终由`checkAuthorization()`触发。 （对于`checkAuthorization()`回调，使用的资源将被回调。） 利用此功能，您可以区分哪个响应对应于哪个调用。 要使用此功能，您可以编写类似以下内容的代码：

```JavaScript
    for each (resource in ["TNT", "CNN", "TBS", "AdultSwim"] ) {
         ae.checkAuthorization(resource);
    }
    
    // Success callback
    function setToken(resource, token) {
         // Use "resource" to figure 
         // out which checkAuthorization
         // call triggered this response
    }
    
    // Old error callback
    function tokenRequestFailed(resource, error, details) {
         // use "resource" to figure
         // out in response to which
         // checkAuthorization call
         // this was triggered
    }
    
    // Error callback using new error api
    ae.bind("errorEvent',"errorHandler");
    
    function errorHandler(error) {
         if(error.resource) {        
              // Use error.resource to figure
              // which checkAuthorization call
              // triggered this response
         }
    }
```

### AE行为常见问题解答 {#ae-beh-faq}

**问题。 如果在第一次调用完成之前进行第二次AccessEnabler调用，会发生什么情况？**

当第二调用执行时，第一调用继续执行（异步通信）。

**问题。 AccessEnabler可以支持的同时调用次数是否达到上限？**

AccessEnabler代码中没有明确设置任何限制，因此您仅受可用系统资源以及MVPD容量的限制。

<!--

>[!MORELIKETHIS]
>
>*   [Programmer use cases](/help/authentication/programmer-use-cases.md)
>*   Error Reporting
>**Platform Cookbooks:**
>*   Clientless integration cookbook
>*   iOS Integration Cookbook
>*   Android Integration Cookbook
>*   JavaScript Integration Cookbook
>*   ActionScript Integration Cookbook
>*   Windows 8 Integration Cookbook
>*   AIR Native Extension Overview
-->
