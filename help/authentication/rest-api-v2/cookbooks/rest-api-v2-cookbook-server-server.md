---
title: REST API V2指南（服务器到服务器）
description: REST API V2指南（服务器到服务器）
source-git-commit: c17e52dd52fa14c50d59945598d1913f02be2468
workflow-type: tm+mt
source-wordcount: '1566'
ht-degree: 0%

---

# REST API V2指南（服务器到服务器） {#rest-api-v2-cookbook-server-to-server}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。


## 概述 {#overview}

本指南文档的目的是详细介绍在服务器到服务器架构中使用REST API V2实施Adobe Pass身份验证的最佳实践。  它提供了生产环境和操作的基本要求、分步流程实施以及一般注意事项。

>[!IMPORTANT]
>
> REST API V2实施受[限制机制](/help/authentication/throttling-mechanism.md)文档限制。


## 组件 {#components}

在工作中的服务器到服务器解决方案中，涉及到以下组件：


| 类型 | 组件 | 描述 |
| --- | --- | --- |
| 流设备 | 流应用程序 | 驻留在用户流设备上并播放经过验证的视频的程序员应用程序。 |
| | \[Optional\]身份验证模块 | 如果流设备具有用户代理（即Web浏览器），则AuthN模块负责在MVPD IdP上验证用户。 |
| \[可选\]身份验证设备 | AuthN应用程序 | 如果流设备没有用户代理（即Web浏览器），则AuthN应用程序是程序员Web应用程序，可使用Web浏览器从单独的用户设备访问。 |
| 程序员基础架构 | 程序员服务 | 将流设备与Adobe Pass服务链接以获取身份验证和授权决策的服务。 |
| Adobe基础架构 | Adobe Pass服务 | 与MVPD IdP和AuthZ服务集成并提供身份验证和授权决策的服务。 |
| MVPD基础架构 | MVPD IdP | MVPD端点，提供基于凭据的身份验证服务以验证其用户的身份。 |
| | MVPD AuthZ服务 | MVPD端点，根据用户的订阅、家长控制等提供授权决策。 |


流中使用的其他术语定义于
[词汇表](/help/authentication/glossary.md)。

下图说明了整个流程：

![](/help/authentication/assets/rest-api-v2/cookbooks/apass-servertoserver-cookbook.png)

### 在服务器到服务器架构中实施REST API V2的步骤 {#steps-to-implement-the-rest-api-v2-in-server-to-server-infrastructure}

要实施Adobe Pass REST API V2，您需要遵循以下分阶段的步骤。

## 0.先决条件 {#prerequisites}

在服务器到服务器实现中，流应用程序和程序员服务需要建立一个协议，以便程序员服务能够：
* 唯一标识设备上的流应用程序
* 代表流应用程序采取行动并与Adobe Pass服务进行通信
* 收集并存储有关流应用程序和设备的信息（如IP地址、源端口和设备信息），以将其传递到Adobe Pass
* 将决策和说明返回到流应用程序

设备ID、客户端ID、客户端密钥（定义如下）等参数可以存储在流式应用程序或程序员服务中。

## A.登记阶段 {#registration-phase}

### 步骤1：注册应用程序 {#step-1-register-your-application}

应用程序要能够调用Adobe Pass REST API V2，它需要API安全层所需的访问令牌。 对于服务器到服务器实施，程序员服务可以代表应用程序实例进行注册。
需要为每个流设备获取客户端ID和客户端密钥值。

要获取访问令牌，程序员服务可以代表流式应用程序执行操作，并且需要执行如下所述的步骤：[动态客户端注册](../../dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md)

## B.认证阶段 {#authentication-phase}

### 步骤2：检查现有的已验证用户档案 {#step-2-check-for-existing-authenticated-profiles}

程序员服务代表流式处理应用程序检查现有的已验证配置文件： `/api/v2/{serviceProvider}/profiles` （[检索已验证的配置文件](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)）

* 如果未找到配置文件，则流应用程序将实施TempPass流
   * 按照有关如何实施[临时访问流](../flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)的文档操作
* 如果未找到配置文件，则流应用程序将实施身份验证流程
   * <b>步骤2.a：</b>程序员服务检索可用于serviceProvider <b>/api/v2/{serviceProvider}/configuration</b><br>的MVPD列表
（[检索可用MVPD的列表](../apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)）
   * 程序员服务可以在MVPD列表上实施筛选，并仅显示意图隐藏其他MVPD（TempPass、测试MVPD、正在开发的MVPD等）
   * 程序员服务应返回已过滤的MVPD列表，以便流式应用程序显示选择器，用户选择MVPD
   * 从流式应用程序中选择MVPD后，程序员服务将创建一个会话： <b>/api/v2/{serviceProvider}/sessions</b><br>
（[创建身份验证会话](../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)）<br>
      * 返回用于身份验证的代码和URL
      * 如果找到配置文件，程序员服务可能会继续到<a href="#preauthorization-phase">C。预授权阶段</a>
   * 程序员服务应将代码和URL返回到流应用程序

### 步骤3：对用户进行身份验证 {#step-3-authenticate-the-user}

使用浏览器或基于Web的第二个屏幕的应用程序：

* 选项1. 流应用程序可以打开浏览器或Web视图，加载URL以进行身份验证，并且用户登陆需要提交凭据的MVPD登录页面
   * 用户输入登录/密码，最终重定向显示成功页面
* 选项2。 流应用程序无法打开浏览器，而只显示代码。 <b>需要开发单独的Web应用程序AuthN_APP</b>，以要求用户输入代码、生成并打开URL： <b>/api/v2/authenticate/{serviceProvider}/{CODE}</b>
   * 用户输入登录/密码，最终重定向显示成功页面

### 步骤4：检查已验证的用户档案 {#step-4-check-for-authenticated-profiles}

程序员服务在浏览器或第二个屏幕中检查是否使用MVPD完成身份验证

* 建议在<b>/api/v2/{serviceProvider}/profiles/{mvpd}</b><br>上每15秒轮询一次
（[检索特定MVPD的已验证配置文件](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)）
   * 如果流应用程序中未选择MVPD，因为第二个屏幕应用程序中存在MVPD选取器，则应该使用代码<b>/api/v2/{serviceProvider}/profiles/code/{CODE}</b><br>进行轮询
（[检索特定代码的已验证配置文件](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)）
* 如果轮询时间达到30分钟，并且流应用程序仍处于活动状态，则需要启动新会话，并且会返回新的代码和URL，则轮询时间不应超过30分钟
* 身份验证完成后，返回值为200，配置文件已验证
* 程序员服务可能会继续到<a href="#preauthorization-phase">C。预授权阶段</a>

## C.预先授权阶段 {#preauthorization-phase}

### 步骤5：检查预授权的资源 {#step-5-check-for-preauthorized-resources}

有了有效的用户身份验证配置文件，程序员服务就可以检查
访问可用的视频，并将列表传递给流应用程序以供显示。

* 如果应用程序希望过滤在经过身份验证的用户包中不可用的资源，则可以选择执行此步骤
* 调用<b>/api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}</b><br>
（[使用特定的MVPD检索预授权决策](../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)）

## D.授权阶段 {#authorization-phase}

### 步骤6：检查授权资源 {#step-6-check-for-authorized-resources}

流媒体应用程序准备播放用户选择的视频/资产/资源。

* 每个播放开始都需要执行步骤
* 流式应用程序将此信息传递给程序员服务
* 为流应用程序提供程序员服务，调用<b>/api/v2/{serviceProvider}/decision/authorize/{mvpd}</b><br>
（[使用特定MVPD检索授权决策](../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)）
   * decision = &#39;允许&#39; ，程序员服务指示流应用程序开始流
   * decision = &#39;拒绝&#39;，程序员服务指示流应用程序通知用户它无权访问该视频
   * 在此过程中，程序员服务可能会评估其他业务规则，并将适当的决策返回给流应用程序

## E.注销阶段 {#logout-phase}

### 步骤7：注销 {#step-7-logout}

流应用程序：用户希望从MVPD注销

* 流应用程序会通知程序员服务，它需要从MVPD注销此特定应用程序。
* 程序员服务可清除其存储的有关已验证用户的信息
* 程序员服务调用<b>/api/v2/{serviceProvider}/logout/{mvpd}</b><br>
（[启动特定MVPD的注销](../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)）
* 如果存在response actionType=&#39;interactive&#39;和url，则程序员服务将返回流应用程序的url
* 基于现有功能，流应用程序可能会在浏览器中打开url（通常与用于身份验证的url相同）
* 如果流应用程序没有浏览器，或者它与身份验证时的实例不同，则可以停止流，因为MVPD会话未保留在浏览器缓存中。

## 环境和功能要求{#environments}

程序员应至少创建两个环境：一个用于生产，一个或多个用于暂存。


### 生产

生产环境应具有高可用性，并且可适应较大的或意外的尖峰（例如，实时运动、破碎）
新闻)。



Adobe Pass服务运行于分布在美国各地的多个数据中心上。  为了从Adobe Pass服务获得最佳响应时间（即最低的延迟），程序员还应创建类似的地理位置分散的服务
基础架构。


如果Adobe需要重新路由流量，程序员服务应将DNS缓存限制为最多30秒。 如果数据中心不可用，则可能会发生这种情况。


程序员应提供生产环境的公共IP范围。 这些将输入到Adobe Pass基础架构中的IP允许列表以进行访问，并由Adobe的公平API使用策略进行管理。

### 暂存

暂存环境可以是最小的，但应包括所有系统组件和业务逻辑。 其功能应该与生产环境类似，并且允许在生产环境之外测试版本。 理想情况下，暂存环境可以连接到Adobe Pass测试环境以供程序员使用，并在需要时通过Adobe进行连接，以便我们协助测试和故障排除。

### 功能要求

程序员服务必须为其执行流的设备传递准确的设备识别信息。 此外，程序员服务必须传递正在为其执行流的设备的IP（在x-forwarded-for标头中）以及连接源端口（在设备信息字段中）：

程序员服务应发送单个MVPD或集成应用程序所需的数据和格式（例如设备IP、源端口、设备信息、MRSS、可选数据，如ECID）。<!--Please see the documentation for [Passing Device and Connection Information Cookbook](http://tve.helpdocsonline.com/passing-device-information-cookbook)-->。

程序员服务在缓存验证配置文件和决策时必须遵守验证配置文件和决策有效性，并在收到通知时使验证或决策无效。

程序员服务必须维护与Adobe共享的证书（用于加密的用户元数据）。

## 相关信息 {#related}

* [REST API V2参考](/help/authentication/rest-api-v2/rest-api-v2-overview.md)
