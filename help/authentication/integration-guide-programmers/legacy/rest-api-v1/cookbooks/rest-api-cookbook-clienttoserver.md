---
title: REST API指南（客户端到服务器）
description: Rest API指南客户端到服务器。
exl-id: f54a1eda-47d5-4f02-b343-8cdbc99a73c0
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '899'
ht-degree: 0%

---

# （旧版）REST API指南（客户端到服务器） {#rest-api-cookbook-client-to-server}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

>[!IMPORTANT]
>
> 确保随时了解汇总在[产品公告](/help/authentication/product-announcements.md)页中的最新Adobe Pass身份验证产品公告和停用时间表。

## 概述 {#overview}

此文档为程序员的工程团队提供了分步说明，介绍如何使用REST API服务将“智能设备”（游戏机、智能电视应用程序、机顶盒等）与Adobe Pass身份验证集成。 这种客户端到服务器方法使用REST API而不是客户端SDK，允许更广泛地支持各种平台，对于这些平台，开发大量独特SDK将不可行。 有关无客户端解决方案工作方式的广泛技术概述，请参阅[无客户端技术概述](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-overview.md)。


此方法需要两个组件（流应用程序和AuthN应用程序）来完成所需的流程：流应用程序中的启动、注册、授权和查看媒体流，以及AuthN应用程序中的身份验证流。

### 节流机构

Adobe Pass身份验证REST API受[限制机制](/help/authentication/integration-guide-programmers/throttling-mechanism.md)控制。

## 组件 {#components}

在使用中的客户端到服务器解决方案中，涉及到以下组件：



| 类型 | 组件 | 描述 |
| --- | --- | --- |
| 流设备 | 流应用程序 | 驻留在用户流设备上并播放经过验证的视频的程序员应用程序。 |
| | \[Optional\]身份验证模块 | 如果流设备具有User Agent（即Web Browser），则AuthN模块负责在MVPD IdP上验证用户。 |
| \[可选\]身份验证设备 | AuthN应用程序 | 如果流设备没有用户代理（即Web浏览器），则AuthN应用程序是程序员Web应用程序，可使用Web浏览器从单独的用户设备访问。 |
| Adobe基础架构 | Adobe Pass服务 | 与MVPD IdP和AuthZ服务集成并提供身份验证和授权决策的服务。 |
| MVPD基础架构 | MVPD IdP | MVPD端点，它提供基于凭据的身份验证服务以验证其用户的身份。 |
| | MVPD AuthZ服务 | MVPD端点，根据用户的订阅、家长控制等提供授权决策。 |



流中使用的其他术语在[术语表](/help/authentication/kickstart/glossary.md)中定义。

## 流{#flows}

### 动态客户端注册(DCR)

Adobe Pass使用DCR来保护程序员应用程序或服务器与Adobe Pass服务之间的客户端通信。 DCR流程是独立的，在[Dynamic Client Registration Overview](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)文档中进行了说明。


### 流（智能设备）应用程序流量

![](../../../../assets/smart-device-app-flow.png)

#### 启动流程

1. 您的应用程序启动并加载其初始UI。

2. 获取/生成设备ID。

3. 发出Check-authentication调用以查看设备是否已通过身份验证。  例如： [`<SP_FQDN>/api/v1/checkauthn [device ID]`](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md)

4. 如果`checkauthn`调用成功，则继续从步骤2开始执行授权流程。  如果失败，则启动注册流。



#### 注册流程

1. 获取用户用于访问第二屏登录应用程序的注册码和URL，并将它们呈现给用户：

   a.向Adobe注册代码服务发送POST请求，传递经过哈希处理的设备ID和“注册URL”。  例如： [`<REGGIE_FQDN>/reggie/v1/[requestorId]/regcode [device ID]`](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md)

   b.向用户呈现返回的注册码和URL。

   c.指示用户切换到支持Web的设备，导航到URL，然后输入注册码。



#### 授权流程

1. 用户从第二屏应用程序返回并按设备上的“继续”按钮。 或者，您可以实施轮询机制来检查身份验证状态，但Adobe Pass身份验证建议使用继续按钮方法而不是轮询。 <!--(For information on employing a "Continue" button versus polling the Adobe Pass Authentication backend server, see the Clientless Technical Overview: Managing 2nd-Screen Workflow Transition.)-->例如： [\&lt;SP\_FQDN\>/api/v1/tokens/authn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md)

2. 向Adobe Pass Authentication Authorization Service发送GET请求以启动授权。 例如： `<SP_FQDN>/api/v1/authorize [device ID, Requestor ID, Resource ID]`

<!-- end list -->

* 如果响应指示成功：用户具有有效的AuthN令牌，并且用户有权观看请求的媒体（此用户具有有效的AuthZ令牌）。

* 如果响应指示失败：请检查引发的异常，以确定其类型（AuthN、AuthZ或其他）：

   * 如果是AuthN错误，则重新启动注册流。

   * 如果是AuthZ错误，则用户无权观看请求的媒体，应向用户显示某种错误消息。

   * 如果发生其他错误（连接错误、网络错误等），则向用户显示相应的错误消息。



#### 查看媒体流

1. 显示介质选项。 用户选择要查看的媒体。

2. 媒体是否受保护？

   a.您的应用程序检查媒体是否受保护。

   b.如果媒体受到保护，您的应用程序将启动授权
(AuthZ)以上流量。

   c.如果媒体未受保护，则播放
用户。

3. 播放媒体。


### AuthN（第2屏）应用程序流程

![](../../../../assets/secnd-screen-authn-flow.png)

1. 获取此用户的MVPD列表。 例如： [`<SP_FQDN>/api/v1/config/[requestorID]`](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md)

1. 启动身份验证流程。  例如： [`<SP_FQDN>/api/v1/authenticate [requestorID, MVPD ID, Redirect URL, Domain name, Registration Code, "noflash=true"]`](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md)

1. 检查身份验证是否成功。 例如：[`<SP_FQDN>/api/v1/checkauthn/[registration code][requestor ID]`](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md)

1. 将用户发送回您的智能设备应用程序以完成授权流程。

## 合作伙伴单点登录 {#partner-sso}

某些设备为合作伙伴单点登录(SSO)提供专用支持：

* [APPLE SSO](/help/authentication/integration-guide-programmers/legacy/sso-access/apple-sso-cookbook-rest-api-v1.md)

## Platform单点登录 {#platform-sso}

某些设备提供对Platform单点登录(SSO)的专用支持：

* [AMAZON SSO](../../sso-access/amazon-sso-cookbook-rest-api-v1.md)
* [Roku SSO](../../../features-standard/sso-access/platform-sso/roku-single-sign-on/roku-sso-overview.md)

## REST API的TempPass和提升TempPass {#temppass}

对于不需要用户输入凭据的TempPass和Promotional TempPass实施，可以直接在流应用程序中实施身份验证。

**要使用此API，流应用程序需要确保设备ID的唯一性，因为此设备ID正用于标识令牌以及可选的额外数据。**


![](../../../../assets/temp-pass-promo-temppass.png)
