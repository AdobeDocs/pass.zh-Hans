---
title: Amazon FireOS技术概述
description: Amazon FireOS技术概述
exl-id: 939683ee-0dd9-42ab-9fde-8686d2dc0cd0
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '2189'
ht-degree: 0%

---

# （旧版）Amazon FireOS技术概述 {#amazon-fireos-technical-overview}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

>[!IMPORTANT]
>
> 确保随时了解汇总在[产品公告](/help/authentication/product-announcements.md)页中的最新Adobe Pass身份验证产品公告和停用时间表。

</br>

## 简介 {#intro}

Amazon FireOS AccessEnabler由两个组件表示：一个是应用程序使用的AccessEnabler存根库，另一个是系统级别的Java Android库，它使移动应用程序能够将Adobe Pass身份验证用于TV Everywhere的授权服务。 适用于Amazon FireOS的Android实施包括定义权利API的AccessEnabler接口以及描述库触发的回调的EntitlementDelegate协议。 系统级别的AccessEnabler Android库允许访问Amazon服务，从而在平台级别启用单点登录。

## 了解本机客户端工作流 {#native_client_workflows}

本机客户端工作流通常与基于浏览器的Adobe Pass身份验证客户端的工作流相同或非常相似。 但是，有一些例外，如下所述。


### 初始化后工作流 {#post-init}

AccessEnabler支持的所有授权工作流都假定您之前已调用[`setRequestor()`](#setRequestor)来建立您的身份。 进行此调用以仅提供一次请求者ID，通常在应用程序的初始化/设置阶段进行。

对于本机客户端（例如，AmazonFireOS），在首次调用[`setRequestor()`](#setRequestor)后，您可以选择如何继续：

- 您可以立即开始发出授权调用，并在需要时允许静默排队等候。
- 或者，您可以通过实施setRequestorComplete()回调接收[`setRequestor()`](#setRequestor)成功/失败的确认。
- 或者两者都做。

至于是等待[`setRequestor()`](#setRequestor)成功通知还是依赖AccessEnabler的呼叫队列机制，由您决定。 由于所有后续授权和身份验证请求都需要请求者ID和相关配置信息，因此[`setRequestor()`](#setRequestor)方法会在初始化完成之前有效地阻止所有身份验证和授权API调用。

### 通用初始身份验证工作流 {#generic}

此工作流的目的是使用用户的MVPD登录用户。  成功登录后，后端服务器会向用户颁发身份验证令牌。 虽然身份验证通常作为授权过程的一部分完成，但下面描述了身份验证如何单独工作，并且不包括任何授权步骤。

请注意，虽然以下本机客户端工作流与典型的基于浏览器的身份验证工作流不同，但步骤1-5对于本机客户端和基于浏览器的客户端均相同：

1. 您的页面或播放器通过调用[getAuthentication()](#getAuthN)来启动身份验证工作流，该调用会检查缓存的有效身份验证令牌。 此方法具有可选的`redirectURL`参数；如果不提供`redirectURL`的值，则在成功进行身份验证后，用户将返回到初始化身份验证的URL。
1. AccessEnabler确定当前的身份验证状态。 如果用户当前已通过身份验证，则AccessEnabler将调用您的`setAuthenticationStatus()`回调函数，传递一个表示成功的身份验证状态（下面的步骤7）。
1. 如果用户未经过身份验证，AccessEnabler将通过确定用户的最后一次身份验证尝试在给定MVPD中是否成功来继续身份验证流程。 如果缓存了MVPD ID并且`canAuthenticate`标志为true或使用[`setSelectedProvider()`](#setSelectedProvider)选择了MVPD，则不会通过MVPD选择对话框提示用户。 身份验证流程继续使用MVPD的缓存值(即，在上次成功身份验证期间使用的MVPD值)。 系统会向后端服务器发出网络调用，并将用户重定向到MVPD登录页面（下面的步骤6）。
1. 如果未缓存MVPD ID，并且未使用[`setSelectedProvider()`](#setSelectedProvider)选择MVPD，或者`canAuthenticate`标志设置为false，则会调用[`displayProviderDialog()`](#displayProviderDialog)回调。 此回调会指示您的页面或播放器创建UI，为用户显示可从中进行选择的MVPD列表。 提供了一系列MVPD对象，其中包含构建MVPD选择器所需的信息。 每个MVPD对象都描述一个MVPD实体，并包含MVPD的ID（例如XFINITY、AT\&amp;T等）和可以找到MVPD徽标的URL等信息。
1. 选择特定的MVPD后，您的页面或播放器必须通知AccessEnabler用户的选择。 对于非Flash客户端，一旦用户选择了所需的MVPD，您就会通过调用[`setSelectedProvider()`](#setSelectedProvider)方法向AccessEnabler通知用户选择的内容。 Flash客户端改为调度类型为“`MVPDEvent`”的共享`mvpdSelection`，传递选定的提供程序。
1. 对于Amazon应用程序，[`navigateToUrl()`](#navigagteToUrl)回调将被忽略。 Access Enabler库便于访问公共WebView控件以验证用户。
1. 通过`WebView`，用户访问MVPD的登录页面并输入其凭据。 请注意，在此传输过程中会执行多个重定向操作。
1. 一旦WebView完成身份验证，它将关闭并通知AccessEnabler用户已成功登录，AccessEnabler将从后端服务器检索实际的身份验证令牌。 AccessEnabler使用状态代码1调用[`setAuthenticationStatus()`](#setAuthNStatus)回调，表示成功。 如果在执行这些步骤的过程中出现错误，则会触发[`setAuthenticationStatus()`](#setAuthNStatus)回调，状态代码为0，并且还会显示相应的错误代码，以指示用户未经过身份验证。

### 注销工作流 {#logout}

对于本机客户端，注销的处理方式类似于上述身份验证过程。 按照此模式，AccessEnabler将创建一个`WebView`控件，并将使用后端服务器上注销终结点的URL加载该控件。 注销过程完成后，令牌将被清除。

请注意，注销流与身份验证流的不同之处在于，用户不需要以任何方式与`WebView`交互。 注销完成后，AccessEnabler将调用`setAuthenticationStatus()`回调，状态代码为0，表示用户未经过身份验证。

## 令牌 {#tokens}

### 定义和使用情况 {#definitions}

Adobe Pass身份验证权利解决方案围绕Adobe Pass身份验证在成功完成身份验证和授权工作流后生成的特定数据段（令牌）的生成。 这些令牌本地存储在客户端的Amazon FireOS设备上。

令牌的生命周期有限；过期时，需要通过重新启动身份验证和/或授权工作流重新颁发令牌。

在授权工作流中颁发了三种类型的令牌：

- **身份验证令牌** — 用户身份验证工作流的最终结果将是一个身份验证GUID，AccessEnabler可以使用该GUID代表用户进行授权查询。 此身份验证GUID将具有关联的生存时间(TTL)值，该值可能与用户的身份验证会话本身不同。 Adobe Pass身份验证通过将身份验证GUID绑定到启动身份验证请求的设备来生成身份验证令牌。
- **授权令牌** — 授予对由唯一`resourceID`标识的特定受保护资源的访问权限。 它由授权方与原始`resourceID`一起颁发的授权授权授权组成。 此信息将绑定到发起请求的设备。
- **短期媒体令牌** - AccessEnabler通过返回短期媒体令牌来授予对给定资源的托管应用程序的访问权限。 此令牌基于之前为该特定资源获取的授权令牌生成。 此外，此令牌不会绑定到设备，并且关联的生命周期会显着缩短（默认值： 5分钟）。

身份验证和授权成功后，Adobe Pass身份验证将颁发身份验证、授权和短期媒体令牌。 这些令牌应缓存在用户设备上，并在其相关生命周期内使用。

### 缓存准则 {#caching}


#### 身份验证令牌

- **适用于FireOS的AccessEnabler 1.10.1**&#x200B;基于Android 1.9.1的AccessEnabler — 此SDK引入了一种新的令牌存储方法，可启用多个Programmer-MVPD存储桶，因此可启用多个身份验证令牌。

#### 授权令牌

在任何给定时刻，AccessEnabler只缓存每个资源一个授权令牌。 可以缓存多个授权令牌，但它们与不同的资源相关联。 每当颁发了新授权令牌并且同一资源已存在旧授权令牌时，新令牌会覆盖现有的缓存值。

#### 媒体令牌

短时间的媒体令牌根本不应缓存。 每次调用授权API时，应该从服务器检索媒体令牌，因为它仅限于一次性使用。

### 持久性 {#persistence}

令牌需要在同一应用程序的连续运行中保持持久性。 这意味着，一旦获取了身份验证和授权令牌并且用户关闭了应用程序，当用户重新打开应用程序时，应用程序可以使用相同的令牌。 此外，还希望这些令牌能够在多个应用程序中持续存在。 换言之，当用户使用一个应用程序与特定的身份提供者登录后（成功获取身份验证和授权令牌），可以通过不同的应用程序使用相同的令牌，并且在通过同一身份提供者登录时不再提示用户输入凭据。

正是这种无缝的身份验证/授权工作流使得Adobe Pass身份验证解决方案成为了真正的“电视无处不在”的实施方式。 从纯工程的角度来看，Android AccessEnabler库通过将令牌数据存储到位于外部存储上的数据库文件中来解决跨应用程序数据共享问题。 此系统级别的共享资源提供了支持实施所需永久令牌用例的关键组件：

- 支持结构化存储 — Adobe Pass身份验证令牌存储不仅仅是简单的线性缓冲存储器结构。 它提供了一种类似字典的存储机制，允许根据用户指定的键值进行数据索引。
- 使用底层文件系统支持数据持久性 — 默认情况下保留数据库文件的内容，并将数据保存在设备的外部内存中。

一旦特定令牌放入令牌缓存中，AccessEnabler库将在不同时间检查其有效性。  有效的令牌定义为：

- 令牌的TTL未过期
- 令牌的颁发者包含在允许的身份提供程序列表中

令牌存储可支持多个程序员 — MVPD组合，依赖于可保存多个身份验证令牌的多级嵌套映射结构。 此新存储不会以任何方式影响AccessEnabler公共API，并且不需要程序员进行更改。 以下是一个示例来说明此新功能：

1. 打开应用程序1（由程序员1开发）。
1. 使用MVPD1（与程序员1集成）进行身份验证。
1. 暂停/关闭当前应用程序，然后打开App2（由Programmer2开发）。
1. 我们假设Programmer2未与MVPD2集成；因此，用户在App2中将不会进行身份验证。
1. 在App2中使用MVPD2（与程序员2集成）进行身份验证。
1. 切换回App1；用户仍将通过Programmer1进行身份验证。

### 格式化 {#format}

#### 身份验证令牌 {#authn_token}

下表显示了身份验证令牌的格式：

```JSON
    <signatureInfo>base64(...)<signatureInfo>
    <simpleAuthenticationToken>
        <simpleTokenAuthenticationGuid>71C69B91-F327-F185-F29E-2CE20DC560F5</simpleTokenAuthenticationGuid>
        <simpleTokenRequestorID>TEST_REQUESTOR</simpleTokenRequestorID>
        <simpleTokenDomainName>adobe.com</simpleTokenDomainName>
        <simpleTokenExpires>2011/03/19 02:29:34 GMT +0200</simpleTokenExpires>
        <simpleTokenMsoID>Adobe</simpleTokenMsoID>
        <simpleTokenDeviceID>
            <simpleTokenFingerprint>
                HASH(true device identification info)
            </simpleTokenFingerprint>
        </simpleTokenDeviceID>   
    </simpleAuthenticationToken>
```


#### 授权令牌 {#authz_token}

以下列表显示了授权令牌的格式：

```JSON
    <signatureInfo>base64(...)<signatureInfo>
    <simpleAuthorizationToken>
        <simpleTokenRequestorID>TEST_REQUESTOR</simpleTokenRequestorID>
        <simpleTokenResourceID>TEST_RESOURCE</simpleTokenResourceID>
        <simpleTokenTTL>2011/03/17 14:40:08 GMT +0200</simpleTokenTTL>
        <simpleTokenMsoID>Adobe</simpleTokenMsoID>
        <simpleTokenDeviceID>
            <simpleTokenFingerprint>
                HASH(true device identification info)
            </simpleTokenFingerprint>
        </simpleTokenDeviceID>
    </simpleAuthorizationToken>
```


#### 短媒体令牌 {#short_media_token}

以下列表显示了短媒体令牌的格式。  此令牌向程序员的应用程序公开。  在成功的权利流程结束时，它将传递到程序员应用程序：

```JSON
    <signatureInfo>signature<signatureInfo>
    <shortAuthorizationToken>
      <sessionGUID>session_guid</sessionGUID>
      <requestorID>requestor_id</requestorID>
      <resourceID>resource_id</resourceID>
      <ttl>ttl_in_ms</ttl>
      <issueTime>issue_time</issueTime>
      <mvpdId>mvpd_id</mvpdId>
      <proxyMvpdId>proxy_mvpd_id</proxyMvpdId>
    </shortAuthorizationToken>
```


#### 设备绑定 {#device_binding}

在上面的XML列表中，记下标题为`simpleTokenFingerprint`的标记。 此标记用于保存本机设备ID个性化信息。 AccessEnabler库能够获得此类个性化信息，并在授权调用期间将其提供给Adobe Pass身份验证服务。 该服务将使用此信息并将其嵌入到实际令牌中，从而有效地将令牌绑定到特定设备。 这样做的最终目标是使令牌无法跨设备传输。

在上面的XML列表中，请注意名为simpleTokenFingerprint的标记。 此标记用于保存本机设备ID个性化信息。 AccessEnabler库能够获得此类个性化信息，并在授权调用期间将其提供给Adobe Pass身份验证服务。 该服务将使用此信息并将其嵌入到实际令牌中，从而有效地将令牌绑定到特定设备。 这样做的最终目标是使令牌无法跨设备传输。

由于这显然是安全相关功能，因此从安全的角度来看，此信息本身就具有“敏感性”。 因此，需要保护此信息不被篡改和窃听。 通过通过HTTPS协议发送身份验证/授权请求，解决了窃听问题。 通过数字签名设备标识信息来处理篡改保护。 AccessEnabler库根据设备提供的信息计算设备ID，然后将设备ID“完全清除”作为请求参数发送到Adobe Pass身份验证服务器。  Adobe Pass身份验证服务器使用Adobe的私钥对设备ID进行数字签名，并将其添加到返回到AccessEnabler的身份验证令牌。 因此，设备ID与身份验证令牌绑定。  在授权流期间，AccessEnabler再次发送清除的设备ID以及身份验证令牌。  验证过程失败将自动导致身份验证/授权工作流失败。  Adobe Pass身份验证服务器将私钥应用于设备ID，并将其与身份验证令牌中的值进行比较。  如果二者不匹配，则权利流将失败。
