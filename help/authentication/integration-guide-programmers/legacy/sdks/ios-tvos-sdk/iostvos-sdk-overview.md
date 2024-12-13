---
title: iOS/tvOS SDK概述
description: iOS/tvOS SDK概述
exl-id: b02a6234-d763-46c0-bc69-9cfd65917a19
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '3754'
ht-degree: 0%

---

# （旧版）iOS/tvOS SDK概述 {#iostvos-sdk-overview}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

>[!IMPORTANT]
>
> 确保随时了解汇总在[产品公告](/help/authentication/product-announcements.md)页中的最新Adobe Pass身份验证产品公告和停用时间表。

</br>


## 简介 {#intro}

iOS AccessEnabler是一个Objective C iOS/tvOS库，它使移动应用程序能够将Adobe Pass身份验证用于TV Everywhere的授权服务。 该实现包括定义权利API的&#x200B;*AccessEnabler*&#x200B;接口，以及描述库触发的回调的&#x200B;*EntitlementDelegate*&#x200B;和&#x200B;*[EntitlementStatus](#ios%20entitlement%20status)*&#x200B;协议。 接口和协议在一个常用名称AccessEnabler库下被引用。

## iOS和tvOS要求 {#reqs}

有关iOS和tvOS平台以及Adobe Pass身份验证的当前技术要求，请参阅[平台/设备/工具要求](#ios)，并查阅SDK下载随附的发行说明。 在本页面的其余部分中，您会看到一些部分，其中记录了适用于特定SDK版本及更高版本的更改。 例如，以下是关于1.7.5 SDK的合理说明：

## 了解本机客户端工作流 {#flows}

本机客户端工作流通常与基于浏览器的Adobe Pass身份验证客户端的工作流相同或非常相似。 但是，有一些例外，如下所述。

- [初始化后工作流](#post-init)
- [通用初始身份验证工作流](#generic)
- [注销工作流](#logout)


### 初始化后工作流 {#post-init}

AccessEnabler支持的所有授权工作流都假定您之前已调用[`setRequestor()`](#setReq)来建立您的身份。 进行此调用以仅提供一次请求者ID，通常在应用程序的初始化/设置阶段进行。


使用iOS Native Client，在首次调用[`setRequestor()`](#setReq)后，您可以选择如何继续：

- 您可以立即开始发出授权调用，并在需要时允许静默排队等候。

- 通过实施[`setRequestorComplete()`](#setReqComplete)回调，您可以接收[`setRequestor()`](#setReq)成功/失败的确认。

- 您可以执行上述两项操作。

您可以选择让应用等待[`setRequestor()`](#setReq)成功通知，或者依赖AccessEnabler的呼叫队列机制。 由于所有后续授权和身份验证请求都需要请求者ID和相关配置信息，因此[`setRequestor()`](#setReq)方法会在初始化完成之前有效地阻止所有身份验证和授权API调用。



### 通用初始身份验证工作流 {#generic}

此工作流的目的是使用用户的MVPD登录用户。 成功登录后，后端服务器会向用户颁发身份验证令牌。 虽然身份验证通常作为授权过程的一部分完成，但下面描述了身份验证如何单独工作，并且不包括任何授权步骤。

请注意，虽然此工作流对于本机客户端与典型的基于浏览器的身份验证工作流不同，但步骤1-5对于本机客户端和基于浏览器的客户端均相同。

1. 您的应用程序通过调用AccessEnabler的`getAuthentication() `API方法来启动身份验证工作流，此方法检查有效的缓存身份验证令牌。
1. 如果用户当前已通过身份验证，则AccessEnabler将调用您的[`setAuthenticationStatus()`](#setAuthNStatus)回调函数，传递表示成功的身份验证状态，并结束流。
1. 如果用户当前未经过身份验证，则AccessEnabler通过确定用户的最后一次身份验证尝试在给定MVPD中是否成功来继续身份验证流程。 如果缓存了MVPD ID并且`canAuthenticate`标志为true或使用[`setSelectedProvider()`](#setSelProv)选择了MVPD，则不会通过MVPD选择对话框提示用户。 身份验证流程继续使用MVPD的缓存值(即，在上次成功身份验证期间使用的MVPD值)。 系统会向后端服务器发出网络调用，并将用户重定向到MVPD登录页面（下面的步骤6）。
1. 如果未缓存MVPD ID，并且未使用[`setSelectedProvider()`](#setSelProv)选择MVPD，或者`canAuthenticate`标志设置为false，则会调用[`displayProviderDialog()`](#dispProvDialog)回调。 此回调指示应用程序创建UI，向用户显示可供选择的MVPD列表。 提供了一系列MVPD对象，其中包含构建MVPD选择器所需的信息。 每个MVPD对象都描述一个MVPD实体，并包含MVPD的ID（例如XFINITY、AT\&amp;T等）和可以找到MVPD徽标的URL等信息。
1. 选择特定的MVPD后，您的应用程序必须通知AccessEnabler用户所做的选择。 用户选择所需的MVPD后，您可以通过调用[`setSelectedProvider()`](#setSelProv)方法向AccessEnabler通知用户选择的内容。
1. iOS AccessEnabler调用您的`navigateToUrl:`回调或`navigateToUrl:useSVC:`回调以将用户重定向到MVPD登录页面。 通过触发其中任一项，AccessEnabler会向应用程序发出请求，以创建`UIWebView/WKWebView or SFSafariViewController`控制器并加载回调的`url`参数中提供的URL。 这是后端服务器上的身份验证终结点的URL。 对于tvOS AccessEnabler，使用`statusDictionary`参数调用[status()](#status_callback_implementation)回调，并立即开始轮询第二个屏幕身份验证。 `statusDictionary`包含需要用于第二个屏幕身份验证的`registration code`。
1. 如果是iOS AccessEnabler，则用户登陆MVPD的登录页面，通过应用程序`UIWebView/WKWebView or SFSafariViewController `控制器的介质输入其凭据。 请注意，在此传输过程中将执行多次重定向操作，并且您的应用程序必须监控控制器在多次重定向操作期间加载的URL。
1. 对于iOS AccessEnabler，当`UIWebView/WKWebView or SFSafariViewController`控制器加载特定的自定义URL时，应用程序必须关闭该控制器并调用AccessEnabler的`handleExternalURL:url `API方法。 请注意，此特定自定义URL实际上无效，控制器不会将其实际加载。 只能由应用程序将其解释为验证流程已完成并且关闭`UIWebView/WKWebView or SFSafariViewController`控制器是安全的信号。 如果您的应用程序需要使用`SFSafariViewController `控制器，则特定自定义URL由`application's custom scheme`定义（例如： `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`），否则此特定自定义URL由`ADOBEPASS_REDIRECT_URL`常量（即`adobepass://ios.app`）定义。
1. 在您的应用程序关闭`UIWebView/WKWebView or SFSafariViewController`控制器并调用AccessEnabler的`handleExternalURL:url `API方法后，AccessEnabler将从后端服务器检索身份验证令牌并通知您的应用程序身份验证流程已完成。 AccessEnabler使用状态代码1调用[`setAuthenticationStatus()`](#setAuthNStatus)回调，表示成功。 如果在执行这些步骤的过程中出现错误，则会触发[`setAuthenticationStatus()`](#setAuthNStatus)回调，状态代码为0，表示身份验证失败以及相应的错误代码。


>[!WARNING]
>
> 在AccessEnabler将控制权移交给应用程序的步骤中（即显示提供程序选择对话框时，或显示UIWebView/WKWebView或SFSafariViewController时），用户有机会取消身份验证流程。 在这些情况下，应用程序负责通知AccessEnabler此事件，并调用[`setSelectedProvider()`](#setSelProv) API方法，将null作为参数传递。 这使AccessEnabler有机会清理其内部状态并重置身份验证流程。 在tvOS上，您可以使用相同的方法取消身份验证轮询。


### 注销工作流 {#logout}

对于本机客户端，注销的处理方式与上面描述的身份验证过程类似。

1. 您的应用程序通过调用AccessEnabler的`logout() `API方法来启动注销工作流。 注销是一系列HTTP重定向操作的结果，这是因为用户既需要从Adobe Pass身份验证服务器注销，也需要从MVPD服务器注销。 由于此流程不能使用AccessEnabler库发出的简单HTTP请求来完成，因此需要实例化`UIWebView/WKWebView or SFSafariViewController`控制器才能遵循HTTP重定向操作。

1. 采用类似于验证流的模式。 iOS AccessEnabler触发`navigateToUrl:`回调或`navigateToUrl:useSVC:`以创建`UIWebView/WKWebView or SFSafariViewController`控制器并加载回调的`url`参数中提供的URL。 这是后端服务器上注销端点的URL。 对于tvOS AccessEnabler，不调用`navigateToUrl:`回调或`navigateToUrl:useSVC:`回调。

1. 在经历多次重定向时，您的应用程序必须监控`UIWebView/WKWebView or SFSafariViewController `控制器的活动，并检测其加载特定自定义URL的时间。 请注意，此特定自定义URL实际上无效，控制器不会将其实际加载。 必须由应用程序将其解释为注销流程已完成并且关闭控制器是安全的信号。 当控制器加载此特定自定义URL时，应用程序必须关闭控制器并调用AccessEnabler的`handleExternalURL:url `API方法。 如果您的应用程序需要使用`SFSafariViewController `控制器，则特定自定义URL由`application's custom scheme`定义（如`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`），否则此特定自定义URL由` ADOBEPASS_REDIRECT_URL  `常量（如`adobepass://ios.app`）定义。

1. 最终，AccessEnabler将调用[`setAuthenticationStatus()`](#setAuthNStatus)回调，状态代码为0，表示注销流成功。

注销流与身份验证流的不同之处在于，用户不需要以任何方式与`UIWebView/WKWebView or SFSafariViewController`控制器交互。 因此，Adobe建议您在注销过程中使控件不可见（即隐藏）。

## 令牌 {#tokens}

- [定义和使用情况](#definitions)
- [缓存准则](#caching)
- [持久性](#persistence)
- [格式化](#format)
- [设备绑定](#device_binding)


### 定义和使用情况 {#definitions}

Adobe Pass身份验证权利解决方案围绕Adobe Pass身份验证在成功完成身份验证和授权工作流后生成的特定数据段（令牌）的生成。 这些令牌存储在客户端的iOS设备本地。



令牌的生命周期有限；过期时，需要通过重新启动身份验证和/或授权工作流重新颁发令牌。



在授权工作流中颁发了三种类型的令牌：

- **身份验证令牌：**&#x200B;用户身份验证工作流的最终结果将是一个身份验证GUID，AccessEnabler可以使用它代表用户进行授权查询。 此身份验证GUID将具有关联的生存时间(TTL)值，该值可能与用户的身份验证会话本身不同。 通过将身份验证GUID绑定到启动身份验证请求的设备，将生成身份验证令牌。
- **授权令牌：**&#x200B;授予对由唯一资源ID标识的特定受保护资源的访问权限。 它包含授权方颁发的授权授权授权以及原始resourceID。 此信息将绑定到发起请求的设备。
- **短期媒体令牌：** AccessEnabler通过返回短期媒体令牌来授予对给定资源的托管应用程序的访问权限。 此令牌基于之前为该特定资源获取的授权令牌生成。 此外，此令牌不会绑定到设备，并且关联的生命周期会显着缩短（默认值： 5分钟）。

身份验证和授权成功后，Adobe Pass身份验证将颁发身份验证、授权和短期媒体令牌。 这些令牌应缓存在用户设备上，并在其相关生命周期内使用。



### 缓存准则 {#caching}

- 身份验证令牌
- 授权令牌
- 短期媒体令牌


#### 身份验证令牌

- **AccessEnabler 1.7：**&#x200B;此SDK引入了一种新的令牌存储方法，可启用多个Programmer-MVPD存储桶，从而启用多个身份验证令牌。 现在，“每个请求者的身份验证”方案和正常的身份验证流程都使用相同的存储布局。 两者之间的唯一区别在于执行身份验证的方式：“每个请求者的身份验证”包含一项新的改进（被动身份验证），使AccessEnabler能够根据存储中是否存在身份验证令牌执行后端通道身份验证（对于不同的程序员）。 用户只需验证一次，此会话将用于在其他应用程序中获取身份验证令牌。 此后端通道流在[`setRequestor()`](#setReq)调用期间发生，对程序员基本上是透明的。 **但是，这里有一个重要的要求：程序员必须从主UI线程中调用setRequestor()。**
- **AccessEnabler 1.6及更早版本：**&#x200B;身份验证令牌在设备上缓存的方式取决于与当前MVPD关联的“**每个请求者的身份验证”**&#x200B;标志：

<!-- end list -->

1. 如果“每个请求者的身份验证”功能被禁用，则单个身份验证令牌将本地存储在全局粘贴板中；此令牌将在与当前MVPD集成的所有应用程序之间共享。
1. 如果启用了“每个请求者的身份验证”功能，则令牌将与执行身份验证流的程序员显式关联（令牌不会存储在全局粘贴板中，而是存储在只能由该程序员的应用程序看到的私有文件中）。 更具体地说，将禁用不同应用程序之间的单点登录(SSO)；在切换到新应用程序时，用户将需要明确执行身份验证流程(前提是第二个应用程序的程序员已与当前MVPD集成，并且本地缓存中没有该程序员的身份验证令牌)。



#### 授权令牌

在任何给定时刻，AccessEnabler只缓存每个RESOURCE的一个授权令牌。 可以缓存多个授权令牌，但它们与不同的资源相关联。 每当发出新的授权令牌并且&#x200B;*同一资源*&#x200B;已存在旧的授权令牌时，新令牌将覆盖现有的缓存值。



#### 媒体令牌

短时间的媒体令牌根本不应缓存。 每次调用授权API时，应该从服务器检索媒体令牌，因为它仅限于一次性使用。



### 持久性 {#persistence}

令牌需要在同一应用程序的连续运行中保持持久性。 这意味着，一旦获取了身份验证和授权令牌并且用户关闭了应用程序，当用户重新打开应用程序时，应用程序可以使用相同的令牌。 此外，还希望这些令牌能够在多个应用程序中持续存在。 换言之，当用户使用一个应用程序与特定的身份提供者登录后（成功获取身份验证和授权令牌），可以通过不同的应用程序使用相同的令牌，并且在通过同一身份提供者登录时不再提示用户输入凭据。 正是这种无缝的身份验证/授权工作流程使Adobe Pass身份验证解决方案成为了无处不在的真正电视节目
实现。



## iOS

iOS AccessEnabler库通过将令牌数据存储到名为&#x200B;*粘贴板*&#x200B;的“类似剪贴板”数据结构来解决跨应用程序数据共享问题。 此系统级共享资源提供支持实施所需永久令牌用例的关键组件：

- **支持结构化存储** — 粘贴板不仅仅是一种简单的线性缓冲存储器结构。 它提供了一种类似字典的存储机制，允许根据用户指定的键值进行数据索引。
- **支持使用基础文件系统进行数据持久性** — 粘贴板结构的内容可以标记为永久性，在这种情况下，数据将保存在设备的内部内存中。



一旦特定令牌放入令牌缓存中，AccessEnabler库将在不同时间检查其有效性。  有效的令牌定义为：

- 令牌的TTL未过期。
- 令牌的颁发者包含在允许的身份提供程序列表中。



## tvOS

由于粘贴板在tvOS上不可用，因此tvOS AccessEnabler库使用NsUserDefaults作为存储选项。 这解决了持久化身份验证和授权令牌的问题，但是所存储的信息不能在不同应用程序之间共享。



**iOS 10粘贴板更改 —**&#x200B;从以前版本的iOS升级时，将擦除粘贴板。 这意味着所有应用程序都需要重新进行身份验证。



**iOS 7粘贴板更改 —**&#x200B;由于粘贴板在iOS 7上的功能发生了更改，在iOS 7上运行的应用程序之间的跨SSO将受限。 具有相同`<Bundle Seed ID>`（也称为`<Team ID>`）的应用程序将共享令牌，这意味着来自同一程序员X的应用程序A1和A2将共享令牌，而应用程序A1 （程序员X）和应用程序A3 （程序员Y）将不共享令牌。

- 如果两个应用程序中的捆绑包种子ID/团队ID由同一配置配置文件生成，则它们是相同的。 要查找更多信息，请访问以下链接：
  [http://developer.apple.com/library/ios/\#documentation/general/conceptual/DevPedia-CocoaCore/AppID.html](http://developer.apple.com/library/ios/#documentation/general/conceptual/DevPedia-CocoaCore/AppID.html)
- 无论使用什么Adobe Pass身份验证SDK，iOS 7中将存在这种“跨SSO”限制。

请阅读此技术说明，了解有关在iOS 7和更高版本上配置SSO的更多信息（此技术说明适用于Access Enabler v1.8和更高版本）： <https://tve.zendesk.com/entries/58233434-Configuring-Pay-TV-pass-SSO-on-iOS>



### 令牌存储(AccessEnabler 1.7)

从AccessEnabler 1.7开始，令牌存储可支持多个程序员 — MVPD组合，依赖于可保存多个身份验证令牌的多级别嵌套映射结构。 此新存储不会以任何方式影响AccessEnabler公共API，并且不需要程序员进行更改。 下面是一个示例，
说明了此新功能：

1. 打开应用程序1（由程序员1开发）。
1. 使用MVPD1（与程序员1集成）进行身份验证。
1. 暂停/关闭当前应用程序，然后打开App2（由Programmer2开发）。
1. 我们假设Programmer2未与MVPD2集成；因此，用户在App2中将不会进行身份验证。
1. 在App2中使用MVPD2（与程序员2集成）进行身份验证。
1. 切换回App1；用户仍将通过Programmer1进行身份验证。

在旧版AccessEnabler中，步骤6会将用户呈现为“未验证”，因为以前令牌存储仅支持一个身份验证令牌。



从某个程序员/MVPD会话中注销将会清除整个底层存储，包括设备上的所有其他程序员/MVPD身份验证令牌。 另一方面，取消身份验证流程（调用[`setSelectedProvider(null)`](#setSelProv)）将不会清除基础存储，但只会影响当前的程序员/MVPD身份验证尝试(通过清除当前程序员的MVPD)。



### 令牌导入器(AccessEnabler 1.7)

AccessEnabler 1.7中包含的另一个与存储相关的功能允许从旧存储区域导入身份验证令牌。 此“令牌导入器”有助于在连续的AccessEnabler版本之间实现兼容性，即使在存储版本升级时也能保持SSO状态。 导入程序在[`setRequestor()`](#setReq)流期间执行，并在以下两种情况下运行（假设当前存储中不存在当前程序员的有效身份验证令牌）：

- 由特定程序员开发的1.7应用程序的第一次安装
- 升级至使用新存储的未来AccessEnabler的路径

导入操作对程序员是透明的，不需要在客户端应用程序中进行任何代码更改。



### 令牌清理器(AccessEnabler 1.7.5)

从AccessEnabler 1.7.5开始，此服务在[`setRequestor()`](#setReq)`. `上运行。它是通过iOS 7从WiFi MAC地址切换到IDFA进行跟踪而开发的。 清理器可确保当前存储仅包含有效的身份验证令牌(根据设备ID有效，以前使用MAC地址计算，在iOS7之前)。 令牌清理器会删除所有无效令牌。



在iOS 6上使用AccessEnabler 1.7.5应用程序，然后用户更新到iOS 7时，“令牌清理器”服务最有用。 发生这种情况时，在iOS 6上获取的所有身份验证令牌现在都将无效(因为设备ID算法已由使用MAC地址更改为IDFA)。 清理器将清除所有无效令牌，然后用户将进行未经身份验证。



如果没有令牌清理器删除无效令牌，AccessEnabler将无法获取授权，因为存在无效的身份验证令牌。 这将证明最终用户很难调试，因为授权错误消息可能很隐秘，并且对于确定导致问题的原因没有太大帮助。



### 格式化 {#format}

- [身份验证令牌](#authn_token)
- [AuthZ令牌](#authz_token)
- [短媒体令牌](#short_token)
- [设备绑定](#device_binding)


请注意，此处包含的AuthN和AuthZ令牌格式仅供背景信息使用。 Adobe Pass身份验证可随时更改这些令牌的结构。 程序员无需知道AuthN和AuthZ令牌的确切结构即可实施其应用程序，因为AuthN和AuthZ令牌不会公开在本地设备上。 短媒体令牌&#x200B;*对程序员的应用程序公开*。



#### 身份验证令牌 {#authn_token}

下表显示了身份验证令牌的格式：

```
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

```
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


#### 短媒体令牌 {#short_token}

以下列表显示了短媒体令牌的格式。 此令牌向程序员的应用程序公开。 在成功的权利流程结束时，它会传递给程序员应用程序：

```
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


### 设备绑定 {#device_binding}

在上面的XML列表中，记下标题为`simpleTokenFingerprint`的标记。 此标记用于保存本机设备ID个性化信息。 AccessEnabler库能够获得此类个性化信息，并在授权调用期间将其提供给Adobe Pass身份验证服务。 该服务将使用此信息并将其嵌入到实际令牌中，从而有效地将令牌绑定到特定设备。 这样做的最终目标是使令牌无法跨设备传输。



由于这显然是安全相关功能，因此从安全的角度来看，此信息本身就具有“敏感性”。 因此，需要保护此信息不被篡改和窃听。 通过通过HTTPS协议发送身份验证/授权请求，解决了窃听问题。 通过数字签名设备标识信息来处理篡改保护。 AccessEnabler库根据设备提供的信息计算设备ID，然后将设备ID“完全清除”作为请求参数发送到Adobe Pass身份验证服务器。 Adobe Pass身份验证服务器使用Adobe的私钥对设备ID进行数字签名，并将其添加到返回到AccessEnabler的身份验证令牌。 因此，设备ID与身份验证令牌绑定。 在授权流期间，AccessEnabler再次发送清除的设备ID以及身份验证令牌。 验证过程失败将自动导致身份验证/授权工作流失败。 Adobe Pass身份验证服务器将私钥应用于设备ID，并将其与身份验证令牌中的值进行比较。 如果二者不匹配，则权利流将失败。



**关于AccessEnabler 1.7.5中的设备绑定的注意：**&#x200B;从AccessEnabler 1.7.5开始，设备ID的计算方式发生了变化，从而为iOS设备提供个性化信息。 此更改反映了iOS 7中的更改：从iOS 7开始，Apple不再提供WiFi MAC地址作为跟踪选项，而是提供其广告商标识符(IDFA)。 由于在iOS 7上运行的应用程序的个性化信息基于IDFA，并且该信息嵌入在权利流令牌中，这意味着此更改可能会对用户体验产生许多不同的影响。 不同的可能效果取决于用户从中升级的iOS版本，以及程序员从中升级的AccessEnabler版本。 有关此更改的详细信息，请参阅AccessEnabler SDK 1.7.5中包含的发行说明。

<!--
## Related Information {#related}


- [iOS/tvOS Integration Cookbook](#)
- [iOS/tvOS API Reference](#)
- [Handling MVPDs with 'Not Trusted Certificates' in Adobe Pass
  authentication native SDK (Tech Note)](#)
- [Registering Native Clients](#)
- [Generating Digital Certificates](#)
- [Understanding Tokens](#understanding_tokens)
- [Identifying Protected Resources](#)
- [SSO on iOS when using the Adobe Pass Authentication Access
  Enabler](#)
-->
