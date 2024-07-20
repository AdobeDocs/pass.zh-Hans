---
title: 面向程序员的概述
description: 面向程序员的概述
exl-id: 64a12e49-0ecb-4b81-977d-60c10925bb59
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '4274'
ht-degree: 0%

---

# 程序员概述 {#programmers-overview}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## 简介 {#introduction}

此概述适用于计划将Adobe®传递到其网站或应用程序的内容提供商（程序员）。 有关其他文档，包括Kickstart和集成指南，请参阅下面的相关信息。

现在，您的查看者可以随时随地访问Internet，并直接向程序员请求访问受保护的内容。 他们可能希望观看一次性活动，或者可能希望获得您正在播放的整个电视剧的观看权。

但在您允许访问受保护内容之前，您必须确定客户是否有权限查看该内容。 他们是否订阅了多频道视频节目分发商(MVPD)？ 如果是，该订阅是否包含您的编程？

对于程序员来说，确定查看者的权利并不总是简单的。 MVPD拥有客户的标识数据和访问权限。  另外，查看者尝试访问您的受保护内容时，会订阅各种MVPD（每个都有不同的系统），而且很容易看出，确定查看者是否有权访问受保护内容会很快变得复杂且具有技术挑战性：

![](assets/user-ent-by-progr.png)

*图：由程序员直接决定的用户权利*

Adobe Pass Authentication for TV Everywhere可在程序员和MVPD之间安全地协调这些授权事务。 Adobe Pass身份验证使程序员可以轻松、快速且安全地向有效客户提供受保护的内容：

![](assets/user-ent-mediatedby-authn.png)

*图：由Adobe Pass身份验证调解的用户权利*

Adobe Pass身份验证作为您的代理，与参与的MVPD进行交换，因此您可以使用一致的跨站点界面呈现查看器。 Adobe Pass身份验证还允许您为查看器提供单点登录(SSO)身份验证和授权。 将跟踪所有参与服务的身份验证和授权，这样用户在其自己的系统上首次进行身份验证后就不需要再次登录。

* **身份验证** — 向MVPD确认给定用户是已知客户的过程。
* **授权** — 向MVPD确认经过身份验证的用户具有指定资源的有效订阅的过程。

### Adobe Pass身份验证的工作方式 {#HowItWorks}

程序员的内容查看应用程序使用Access Enabler客户端组件或无客户端API的RESTful Web服务（适用于不支持Web的设备，例如智能电视、游戏机、机顶盒等）与Adobe Pass身份验证交互。 Access Enabler在用户系统上运行，它有助于执行所有权利工作流。  当客户访问您的网站并Adobe请求受保护的内容时，将从其托管网站上的Access Enabler组件下载。  Adobe Pass身份验证服务器托管无客户端解决方案中使用的RESTful Web服务。

Adobe Pass身份验证处理实际的授权工作流，同时提供用于以下目的的原始项：

* 设置您的身份。 (程序员是Adobe Pass身份验证权利流中的“请求者”。)
* 使用特定MVPD验证用户。  (MVPD是Adobe Pass身份验证权利流中的“身份提供程序”或“IdP”。)
* 使用MVPD授权用户访问特定资源。
* 注销用户。

程序员负责其上层网页或播放器应用程序，这些网页或播放器应用程序执行以下操作：

* 实施用户界面
* 与Access Enabler或无客户端API Web服务交互

Adobe Pass身份验证的目标是为程序员和MVPD创建一种简单的模块化方式来处理授权验证。

## 了解令牌 {#understanding-tokens}

Adobe Pass身份验证权利解决方案重点关注成功完成身份验证和授权工作流后创建的特定数据段的生成。 这些数据段称为令牌。 令牌的生命周期有限；在过期时，需要通过重新启动身份验证和授权工作流重新颁发令牌。

有关令牌的详细信息，请参阅以下部分：

* [令牌类型](#token-types)
* [令牌存储](#token-storage)
* [令牌安全](#token-security)
* [令牌共享](#token-sharing)

### 令牌类型 {#token-types}

在身份验证和授权工作流期间会颁发三种类型的令牌。 AuthN和AuthZ令牌是“长期”的，为用户的观看体验提供了连续性。 媒体令牌是一个生命周期较短的令牌，它支持行业最佳实践，以防止通过流盗用进行欺诈。 程序员根据与MVPD达成的协议为每种类型的令牌指定生存时间(TTL)值。 程序员决定最适合您的企业和客户的TTL价值。

* **AuthN令牌** （“长寿命”）：在成功进行身份验证后，Adobe Pass身份验证将创建一个与请求设备和全局唯一标识符(GUID)都关联的AuthN令牌。
   * Adobe Pass身份验证将AuthN令牌发送到Access Enabler，后者会在客户端系统上安全地缓存该令牌。  当AuthN令牌存在且未过期时，它可供所有使用Adobe Pass身份验证的应用程序使用。 Access Enabler将AuthN令牌用于授权流。
   * 在任何给定时刻，仅缓存一个AuthN令牌。 每当颁发了新的AuthN令牌并且存在旧令牌时，Adobe Pass身份验证都会覆盖缓存的令牌。
* **AuthZ令牌** （“长期”）：在成功授权后，Adobe Pass身份验证将创建与请求设备和特定的受保护资源关联的AuthZ令牌。  受保护的资源由唯一的资源ID标识。
   * Adobe Pass身份验证将AuthZ令牌发送到Access Enabler，后者在本地系统上安全地缓存它。 然后，访问启用程序使用AuthZ令牌创建用于实际查看访问的短暂媒体令牌。
   * 在任意给定时间，每个资源仅缓存一个AuthZ令牌。 Adobe Pass身份验证可以缓存多个AuthZ令牌，前提是它们与其他资源关联。 每当颁发了新的AuthZ令牌并且同一资源已存在旧令牌时，Adobe Pass身份验证会覆盖缓存的令牌。
* **媒体令牌** （“短期”）： Access Enabler使用AuthZ令牌生成短期（默认值： 7分钟）媒体令牌。 在这一点上，即认为播放请求已成功。
   * 在提供对受保护资源的访问权限之前，您的媒体服务器必须使用Adobe Pass身份验证组件（媒体令牌验证器）来验证媒体令牌。
   * 由于媒体令牌未绑定到设备，因此其生命周期明显短于长期AuthN和AuthZ令牌的生命周期（默认值： 7分钟）。
   * 短暂的媒体令牌限制为一次性使用，并且从不进行缓存。 每次调用授权API时，都会从Adobe Pass身份验证服务器检索授权服务。

### 令牌存储 {#token-storage}

Access Enabler将长期令牌（AuthN和AuthZ）存储在特定于其环境的位置：

* **Flash10.1**（或更高版本）：长生命周期令牌存储为本地共享对象。
* **HTML5**：长期令牌安全地保存在HTML5浏览器的本地存储中。
* **iOS**：长生命周期令牌存储在永久粘贴板上，其他Adobe Pass身份验证客户端应用程序可以在该处访问它们。
* **Android**：长生命周期令牌存储在共享数据库文件中，其他Adobe Pass身份验证客户端应用程序可以在该文件中访问它们。
* **无客户端API设备**：令牌存储在Adobe Pass身份验证服务器上。

### 令牌安全 {#token-security}

Adobe Pass Authentication Server使用设备ID（派生自设备的硬件特征）对所有长期令牌进行数字签名。 数字签名的生成、保护和验证方式因环境而异：

* **Flash10.1**（或更高版本） — 设备ID依赖于设备凭据，该凭据是从Adobe个性化服务器颁发的唯一证书。 此安全性相当于FAXS DRM技术。 此服务器端验证会将令牌中的唯一设备ID与设备凭据(安全地从Flash Player通信到Adobe Pass身份验证)进行比较。 设备凭据还标识了FAXS客户端版本以及向其发出凭据的Flash Player(或AIR)版本。 设备绑定比使用HTML5绑定更强，因此使用Flash时，令牌的生存时间(TTL)通常更长。
* **HTML5** — 设备在客户端已个性化。 它使用JavaScript提供的特征来生成虚拟设备ID，其中包括浏览器和操作系统版本、IP地址以及浏览器Cookie GUID（全局唯一标识符）。 此令牌设备ID将与设备的当前伪设备ID进行比较。 由于IP地址在正常使用期间可能会更改，因此即使在同一个会话中，Adobe Pass身份验证也会将HTML5令牌存储在两个位置：localStorage和sessionStorage。 如果IP发生更改并且sessionStorage令牌在其他情况下仍然有效，则会维护会话。 使用HTML5时，设备绑定没有那么强，因此令牌的TTL通常比Flash的TTL短。
* **本机客户端** (iOS和Android) — 长效令牌包含本机设备ID个性化信息，因此已绑定到请求设备。 身份验证和授权请求通过HTTPS发送，设备ID信息在发送到后端服务器之前由Access Enabler库进行数字签名。 在服务器端，设备ID信息将根据其相关联的数字签名进行验证。
* **无客户端API客户端** — 无客户端API解决方案具有一组涉及对所有API调用进行数字签名的安全协议。 权利流期间生成的令牌安全地存储在Adobe Pass身份验证服务器上。

Adobe Pass身份验证会验证每个长期使用的令牌，以确保访问内容的设备与颁发令牌的设备相同。 对于所有令牌，客户端验证可确保数字签名保持不变，并保留令牌的完整性。 当设备ID验证失败时，身份验证会话将失效，并提示用户再次登录，这将重置令牌。

### 令牌共享 {#token-sharing}

不同平台上的应用程序不共享令牌。 这种情况有很多原因，包括：

* 如[令牌存储](#token-storage)中所述，存储令牌的方法因平台而异(例如，用于Flash的本地共享对象，用于JavaScript的WebStorage)。
* 不同平台之间的令牌安全程度不同。 例如，使用FAXS将Flash令牌强绑定到设备。 纯JavaScript环境中的令牌不具备与Flash中相同级别的DRM支持。  如果与Flash应用程序共享JS令牌，则会增加安全级别较低的令牌利用更安全环境的可能性。

## 程序员集成生命周期 {#prog-integ-lifecycle}

![](assets/progr-flow-int-lifecycle.png)

*图：将身份验证与程序员的网站和应用程序集成*


## 逻辑流 {#logical-flows}

### 权利流程图 {#chart}

以下流程图显示了确认权利(使用Adobe Pass Authentication Access Enabler客户端组件)的整体过程：

![](assets/ent-flowchart.png)

*图：确认权利的过程*

### 身份验证步骤 {#authn-steps}

以下步骤显示了Adobe Pass身份验证流程的示例。  这是授权过程的一部分，在该过程中，程序员确定用户是否为MVPD的有效客户。  在此方案中，用户是MVPD的有效订阅者。  用户正尝试使用程序员的Flash应用程序查看受保护的内容：

1. 用户浏览到程序员的网页，该网页将程序员的Flash应用程序和Adobe Pass Authentication Access Enabler组件加载到用户的计算机上。 Flash应用程序使用Access Enabler设置程序员的身份与Adobe Pass Authentication，而Adobe Pass Authentication为该程序员（“请求者”）的配置和状态数据预定Access Enabler。 在执行任何其他API调用之前，Access Enabler必须从服务器接收此数据。 技术说明：程序员使用Access Enabler的`setRequestor()`方法设置其标识；有关详细信息，请参阅[程序员集成指南](/help/authentication/programmer-integration-guide-overview.md)。
1. 当用户尝试查看程序员的受保护内容时，程序员的应用程序向用户显示MVPD列表，用户从中选择提供程序。
1. 用户被重定向到Adobe Pass身份验证服务器，该服务器为用户选择的MVPD创建加密的[SAML](https://en.wikipedia.org/wiki/Security_Assertion_Markup_Language)请求。 此请求将作为代表程序员的身份验证请求发送到MVPD。 根据MVPD的系统，随后用户的浏览器将被重定向到MVPD的站点以登录，或者在程序员的应用程序中创建一个登录iFrame。
1. 无论是哪种情况（重定向还是iFrame），MVPD都会接受请求并显示其登录页面。
1. 用户使用MVPD登录，MVPD验证用户作为付费客户的状态，然后MVPD创建自己的HTTP会话。
1. 验证用户后，MVPD会创建一个响应（SAML和加密），MVPD会将该响应发送回Adobe Pass身份验证。
1. Adobe Pass身份验证接收MVPD响应，看到Adobe Pass身份验证HTTP会话已打开，验证MVPD中的SAML响应，并重新定向回程序员网站。
1. 重新加载程序员的站点，重新加载Access Enabler，程序员再次调用setRequestor()。  需要第二次调用setRequestor()，因为当前配置已更改 — 现在存在一个标志，用于通知Access Enabler正在服务器上等待生成AuthN令牌。
1. 访问启用程序发现存在挂起的身份验证，并向Adobe Pass身份验证服务器请求令牌。 通过调用Flash Player的DRM功能，从服务器检索令牌。
1. AuthN令牌存储在程序员的Flash PlayerLSO缓存中；身份验证现已完成，并且会话在Adobe Pass身份验证服务器上被销毁。

### 授权步骤 {#authz-steps}

从[身份验证步骤](#authn-steps)继续执行以下步骤：

1. 当用户尝试访问程序员的受保护内容时，程序员的应用程序首先会检查用户的本地计算机或设备上是否存在身份验证令牌。  如果该令牌不存在，则执行上述[身份验证步骤](#authn-steps)。  如果存在AuthN令牌，则授权流程继续进行，程序员的应用程序启动对访问启用程序的调用，请求获取用户对受保护内容的特定项目的查看权限。
1. 受保护内容的特定项目由“资源标识符”表示。  这可以是简单的字符串或更复杂的结构，但在任何情况下，资源标识符的性质都事先在程序员和MVPD之间商定。  程序员的应用程序将资源标识符传递给Access Enabler。  Access Enabler检查用户的本地计算机或设备上是否存在AuthZ令牌。  如果没有AuthZ令牌，Access Enabler会将请求传递到后端Adobe Pass身份验证服务器。
1. Adobe Pass身份验证服务器使用标准化协议与MVPDs授权端点进行通信。  如果MVPD的响应指示用户有权查看受保护的内容，则Adobe Pass身份验证服务器会创建一个AuthZ令牌并将其传递回Access Enabler，后者将AuthZ令牌存储在用户的计算机上。
1. 当用户计算机或设备上存储有AuthZ令牌时，程序员的应用程序会调用Access Enabler以从Adobe Pass身份验证服务器获取媒体令牌，并将该令牌提供给程序员的应用程序。
1. 最后，程序员的应用程序使用媒体令牌验证器组件来确认正确的用户正在查看正确的内容，并且在有媒体令牌的情况下，允许用户查看受保护的内容。


## 注册和初始化 {#reg-and-init}

使用Adobe Pass身份验证的第一步是向Adobe或Adobe Pass身份验证授权合作伙伴注册。

注册时，需提供与Adobe Pass身份验证进行通信的域列表。 例如，Turner Broadcasting System域包括tbs.com和tnt.tv，作为Adobe Pass身份验证注册的域。 这些内容站点中的每一个站点都有权访问Adobe Pass身份验证，并分配有自己的请求者ID（例如，“TBS”和“TNT”）。 如果您决定添加其他站点，则必须通知Adobe其他域名，以将其添加到允许的域列表中，并获得额外的请求者ID。

>[!WARNING]
>
>测试集成时，请确保测试服务器位于您打算在生产中使用的注册域上。 来自未列入白名单的域的请求（甚至测试请求）将被忽略。 例如，如果您请求在生产中使用domain.com ，请确保将测试集成部署在domain.com、test.domain.com和staging.domain.com下。
>
>即使已将域列入白名单，也会忽略URL中包含用户名和/或密码的请求。 示例： `//username@registered-domain/`

在与Adobe Pass Authentication的Access Enabler客户端组件进行的所有通信中，请求者ID唯一标识程序员的客户端。 与程序员关联的所有Adobe Pass身份验证静态数据都将与此ID保持键值。

>[!TIP]
>
>除了请求者ID之外，在注册时，您还会收到Access Enabler客户端组件和媒体令牌验证器的功能URL。

## 集成步骤 {#integration-steps}

>[!TIP]
>
>如果您使用AdobeOpen Source Media Framework(“OSMF”)进行媒体播放器开发，使用Adobe Pass身份验证的最快方法是将OSMF插件&#x200B;*（已弃用）*&#x200B;集成到播放器的代码中。
>
><!--For details, see [Adobe Pass Authentication Plugin For OSMF](https://tve.helpdocsonline.com/9-2-2) in the Programmer Integration Guide.-->

1. [申请人设置](#requestor)
1. [处理身份验证和授权](#authn-authz)
1. [支持单一注销](#ssl)

### 1.申请人设置 {#requestor}

#### 1a. 在Adobe中注册

第一步是向Adobe或Adobe Pass Authentication授权合作伙伴注册。  注册时，会向您颁发一个或多个全局唯一标识符(GUID)。 您颁发的每个GUID都与一个允许从中访问Adobe Pass身份验证的域相关联。 您可以为请求域传递一个GUID（称为请求者ID），以便在与Access Enabler交互的每个会话中注册您的身份。 有关详细信息，请参阅《程序员集成指南》中的[注册和初始化](#reg-and-init)。

#### 1b. Initial Access Enabler集成

下一步是将Access Enabler集成到您现有的媒体播放器应用程序或网页中：

* 您可以在基于Flash的视频播放器中嵌入Flash版本`AccessEnabler.swf`，也可以直接将其嵌入到网页的HTML中。 您可以在ActionScript或JavaScript中与Access EnablerSWF通信。 基本APIActionScript，但是，如果您希望使用JavaScript，可以在您的页面上包含完整的包装器库。
* 对于非Flash环境，您可以：
   * 使用HTML5/JavaScript版本AccessEnabler.js，并通过JavaScript API与其通信
   * 使用适用于Adobe Pass身份验证的AIR本机扩展将本机代码与内置的ActionScript类结合使用
   * 使用Access Enabler库的其中一个本机客户端版本(iOS或Android)

### 2.处理身份验证和授权 {#authn-authz}

#### 2a. 与访问启用码通信

Access Enabler与网页或播放器应用程序之间的通信是异步的。 您的应用程序调用Access Enabler方法，Access Enabler通过回调进行响应，回调是您在Access Enabler库中注册的。

* 当应用程序发出授权请求时，如果本地系统上尚不存在有效的AuthN令牌，Access Enabler会自动启动身份验证请求。 身份验证成功后，用户的AuthN令牌将存储在本地，因此他们不需要再次登录。 如果他们在任何其他上下文中通过Adobe Pass身份验证成功进行身份验证（例如，通过MVPD网站或使用其他程序员），则Access Enabler可以访问本地AuthN令牌，而无需其他身份验证。
* 当用户尝试访问您的受保护内容时，您将向Access Enabler发送授权请求。 验证（或启动）身份验证后，Access Enabler联系MVPD(通过Adobe Pass Authentication Server)，以确定客户是否有权查看受保护的内容。 您的应用程序只需要将请求发送到Access Enabler，然后处理响应（授权成功或失败）。 如果授权成功，则在客户端系统上存储AuthZ令牌。  最后，您的应用程序会收到一个短暂的媒体令牌，以便在您自己的授权过程中使用。

>[!NOTE]
>
>* 身份验证作为SAML交换，在作为服务提供程序(SP)的Adobe Pass身份验证和作为身份提供程序(IdP)的MVPD之间发生。
>
>* 授权在Adobe Pass身份验证(SP)和MVPD(IdP)之间使用后端渠道（服务器到服务器）Web服务交换。


#### 2b. 提供授权用户界面 {#entitlement-ui}

您提供自己的UI，以便用户访问您的内容。 某些元素（例如实际登录流程）由MVPD提供，而某些元素可以选择作为Adobe Pass身份验证的一部分提供。 您至少应执行以下操作：

* **实施一个MVPD选择接口，该接口允许新用户识别其MVPD并首次登录**。 对于开发， Access Enabler提供了一个基本的用户界面，使客户能够选择MVPD并启动登录过程。 对于生产，您必须实施自己的MVPD选择器对话框。 有些MVPD会重定向到自己的网站进行登录，而有些则要求在iFrame中显示其登录页面。 您必须实施创建此iFrame的回调以处理用户的MVPD在iFrame中显示其登录页的情况。
* **识别受保护的内容**。 受保护的内容需要授权才能访问。 您的界面应指示哪些内容受保护，以及哪些内容已获得授权。  授权状态通常以“已解锁”和“已锁定”图标表示。
* **显示用户已通过身份验证**。 您应该将用户的身份验证状态指示为您用于识别受保护内容的任何方法的一部分。 您可以查询Access Enabler以确定客户是否已通过身份验证。

#### 2c. 集成媒体令牌验证器 {#int-media-token-ver}

必须将Adobe Pass身份验证媒体令牌验证器组件集成到媒体服务器中。  这样，您现有的令牌验证器就能通过成功的授权识别从Adobe Pass身份验证提供的短暂媒体令牌。 在您向用户提供对受保护内容的访问权限之前，媒体令牌验证器将验证媒体令牌作为最后一个安全步骤。 注册Adobe后，您将收到下载介质令牌验证器的位置。

### 3.支持单一注销 {#ssl}

在大多数情况下，您的媒体播放器负责通过简单的Access Enabler API处理用户注销。 调用logout()时，Access Enabler将执行以下操作：

* 注销当前用户
* 清除已注销用户的所有身份验证和授权信息
* 从用户的本地系统中删除所有AuthN和AuthZ令牌

如果用户让其计算机空闲足够长的时间令其令牌过期，则用户仍可以返回到会话并成功启动注销。 Adobe Pass身份验证确保删除所有令牌，并通知MVPD删除其会话。

当从未与Adobe Pass身份验证集成的站点启动注销时，MVPD可以通过浏览器重定向调用Adobe Pass身份验证单一注销服务。

## 了解用户ID {#user-ids}

从概念上讲，启动权利流的每个用户都与一个唯一的用户ID相关联。  但是，在权利流的过程中，该用户ID可能会以不同的方式呈现，具体取决于您从哪个API获取该ID。

Short Media令牌中的sessionGUID是UserID的安全形式，可通过sendTrackingData()调用获取。   在所有当前集成中，这是跨时间和设备的用户永久GUID，但GUID的源在来自MVPD的SAML响应中以UserID开头。   但是，某些MVPD将来可能会改变主意，并开始发送临时GUID。  如果程序员希望确保AuthN响应中的MVPD源用户ID是永久性的，他们应在与MVPD的协议中对其进行安排。

以下是在Adobe Pass身份验证API中表示用户ID的不同方式：

* `sendTrackingData()` GUID属性 — 这是MVPD UserID的Adobe哈希版本。  此用户ID经过哈希处理，因此不能从MVPD跟踪回源。   此ID是唯一的，且通常具有持久性，但无法与MVPD共享，因此无法将其特定使用行为与MVPD在其一侧具有的行为进行比较。   它没有经过数字签名，因此对于防欺诈而言并非不可欺骗，但对于分析而言，它已经足够好了。  此形式的用户ID在Adobe Pass身份验证在AuthN/AuthZ流中生成的所有事件的客户端提供。
* 短媒体令牌的`sessionGUID`属性 — 通过`sendTrackingData()`与UserID相同，但是，此令牌经过数字签名以保护其完整性。  这使得此值足以很好地用于并发使用情况的欺诈跟踪。 它可在使用我们的验证器库后在服务器端进行处理，并可在向客户端发布视频流之前分析欺诈模式。  这些任务中的任何一个都由程序员来完成。
* `getMetadata() userID `属性 — 此属性将允许Adobe向程序员公开实际的源MVPD UserID。 它将使用我们从程序员那里获得的证书中的公钥进行加密，因此它不会以明文形式向客户端公开。 这为Programmer提供了来自MVPD的实际UserID，因此它可用于直接与MVPD进行帐户关联或欺诈调查。

**结论**

* MVPD用户ID通常是从MVPD生成&#x200B;**并在成功身份验证**&#x200B;时传递给Adobe的永久唯一ID，但并不保证此ID。 除某些例外情况外，所有网络通常都是一致的。
* MVPD用户ID不包含PII，它不是帐号。 它不需要以加密形式公开，因为我们已通过所有MVPD验证未发送任何PII。


用户ID的使用方式取决于用例：

* 如果您需要它来进行跟踪/分析，最切实可行的做法是从`sendTrackingData()`中获取它。
* 如果在服务器端需要用于流发布、欺诈或操作数据，则可以从“媒体令牌验证器”中获取它。
* 如果您需要它来关联帐户和进行更深入的欺诈，请咨询您的Adobe联系人以获取可用性。

<!--
>[!RELATEDINFORMATION]
>
>* **Kickstart Guides** These guides explain the initial steps to take once you have decided to begin integrating with Adobe Pass Authentication.
>* **Programmer Integration Guide** This is a lower level technical guide for Programmers, directed primarily to the software engineers who code and test the applications and systems involved in the integration.
>* [Overview For MVPDs](/help/authentication/mvpd-overview.md) This provides a similar level of conceptual information as in this Programmer overview, but is directed toward MVPDs.
-->
