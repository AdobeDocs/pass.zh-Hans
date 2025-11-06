---
title: 单点登录 — 合作伙伴 — 流程
description: REST API V2 — 单点登录 — 合作伙伴 — 流程
exl-id: 5735d67f-a311-4d03-ad48-93c0fcbcace5
source-git-commit: d8097b8419aa36140e6ff550714730059555fd14
workflow-type: tm+mt
source-wordcount: '1468'
ht-degree: 0%

---

# 使用合作伙伴流程进行单点登录 {#single-sign-on-partner-flows}

>[!IMPORTANT]
>
> 此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

>[!IMPORTANT]
>
> REST API V2实施受[限制机制](/help/authentication/integration-guide-programmers/throttling-mechanism.md)文档限制。

>[!MORELIKETHIS]
>
> 确保也访问[REST API V2常见问题解答](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)。

使用Adobe Pass服务时，Partner方法可让多个应用程序使用合作伙伴框架状态有效负载在设备级别实现单点登录(SSO)。

这些应用程序负责使用Adobe Pass系统之外的合作伙伴特定框架或库检索合作伙伴框架状态有效负载。

应用程序负责将此合作伙伴框架状态有效负载包含到所有指定它的`AP-Partner-Framework-Status`标头中。

有关`AP-Partner-Framework-Status`标头的更多详细信息，请参阅[AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md)文档。

Adobe Pass身份验证REST API V2支持在iOS、iPadOS或tvOS上运行的客户端应用程序的最终用户的合作伙伴单点登录(SSO)。

有关Apple平台单点登录(SSO)的更多详细信息，请参阅[Apple SSO指南(REST API V2)](/help/authentication/integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-cookbook-rest-api-v2.md)文档。

## 检索合作伙伴身份验证请求 {#retrieve-partner-authentication-request}

### 先决条件 {#prerequisites-retrieve-partner-authentication-request}

在检索合作伙伴身份验证请求之前，请确保满足以下先决条件：

* 合作伙伴框架必须选择一个MVPD。
* 流应用程序必须从合作伙伴框架中获取合作伙伴框架状态信息，并将其传递给Adobe Pass服务器。
* 流应用程序必须从Adobe Pass服务器获取合作伙伴身份验证请求，并将其传递到合作伙伴框架。

>[!IMPORTANT]
>
> 假设
> 
> <br/>
> 
> * 合作伙伴框架支持用户通过交互选择MVPD。
> * 合作伙伴框架支持用户通过交互使用选定的MVPD进行身份验证。
> * 合作伙伴框架提供用户权限和提供程序信息。

### 工作流 {#workflow-retrieve-partner-authentication-request}

执行给定步骤以检索合作伙伴身份验证请求，如下图所示。

![检索合作伙伴身份验证请求](../../../../../assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-retrieve-partner-authentication-request-flow.png)

*检索合作伙伴身份验证请求*

1. **检索合作伙伴框架状态：**&#x200B;流式应用程序在Adobe Pass系统之外调用合作伙伴框架，以获取用户权限和提供程序信息。

1. **返回合作伙伴框架状态信息：**&#x200B;流式应用程序验证响应数据以确保满足基本条件：
   * 已授予用户权限访问状态。
   * 用户提供程序映射标识符存在且有效。
   * 用户提供程序配置文件的到期日期（如果可用）有效。

1. **检索合作伙伴身份验证请求：**&#x200B;流应用程序通过调用会话合作伙伴终结点，收集启动身份验证会话所需的所有数据。

   >[!IMPORTANT]
   >
   > 有关以下内容的详细信息，请参阅[检索合作伙伴身份验证请求](../../apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md) API文档：
   >
   > * 所有&#x200B;_必需的_&#x200B;参数，如`serviceProvider`和`partner`
   > * 所有&#x200B;_必需的_&#x200B;标头，如`Authorization`、`AP-Device-Identifier`、`Content-Type`、`X-Device-Info`和`AP-Partner-Framework-Status`
   > * 所有&#x200B;_可选_&#x200B;标头和参数
   >
   > <br/>
   >
   > 流应用程序在发出请求之前，必须确保它包含合作伙伴框架状态的有效值。
   >
   > <br/>
   > 
   > 有关`AP-Partner-Framework-Status`标头的更多详细信息，请参阅[AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md)文档。

1. **指示下一个操作：**&#x200B;会话合作伙伴终结点响应包含指导流应用程序执行下一个操作所需的数据。

   >[!IMPORTANT]
   >
   > 有关会话响应中提供的信息的详细信息，请参阅[检索合作伙伴身份验证请求](../../apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md) API文档。
   > 
   > <br/>
   > 
   > 会话合作伙伴端点验证请求数据以确保满足基本条件：
   >
   > * _必需_&#x200B;参数和标头必须有效。
   > * 提供的`serviceProvider`和`mvpd`之间的集成必须处于活动状态。
   >
   > <br/>
   > 
   > 如果基本验证失败，将生成错误响应，提供附加信息，这些信息将遵循[增强型错误代码](../../../../features-standard/error-reporting/enhanced-error-codes.md)文档。
   >
   > <br/>
   >
   > 会话合作伙伴端点验证请求数据，以确保满足合作伙伴单点登录条件：
   >
   >  * Adobe Pass服务器中的合作伙伴单点登录配置必须有效且已启用。
   >  * 通过[AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md)标头接收的合作伙伴框架状态有效负载必须有效。
   >
   > <br/>
   >
   > 如果合作伙伴单点登录验证失败，则响应将默认使用基本身份验证流程。

1. **使用合作伙伴身份验证响应继续配置文件检索流程：**&#x200B;会话合作伙伴终结点响应包含以下数据：
   * `actionName`属性设置为“partner_profile”。
   * `actionType`属性设置为“直接”。
   * `authenticationRequest - type`属性包括合作伙伴框架用于MVPD登录的安全协议（当前仅设置为SAML）。
   * `authenticationRequest - request`属性包含传递到合作伙伴框架的SAML请求。
   * `authenticationRequest - attributesNames`属性包含传递到合作伙伴框架的SAML属性。

   如果Adobe Pass后端未识别有效的配置文件，并且合作伙伴单点登录验证通过，则流应用程序会收到一个包含操作和数据的Response，以便传递到Partner Framework，从而开始使用MVPD的身份验证流程。

   有关使用合作伙伴身份验证响应的配置文件检索流程的更多详细信息，请参阅[使用合作伙伴身份验证响应创建和检索配置文件](#create-and-retrieve-profile-using-partner-authentication-response)部分。

1. **继续基本身份验证流程：**&#x200B;会话合作伙伴终结点响应包含以下数据：
   * `actionName`属性设置为“身份验证”或“恢复”。
   * `actionType`属性设置为“interactive”或“direct”。

   如果Adobe Pass后端未识别有效的配置文件，并且合作伙伴单点登录验证失败，则Adobe Pass服务器将回退到基本身份验证流程。

   有关基本身份验证流程的更多详细信息，请参阅以下文档：
   * [在主应用程序中执行身份验证](../basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
   * [使用预选的mvpd在辅助应用程序中执行身份验证](../basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [无需预选mvpd就可在辅助应用程序中执行身份验证](../basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

1. **继续决策流：**&#x200B;会话合作伙伴终结点响应包含以下数据：
   * `actionName`属性设置为“授权”。
   * `actionType`属性设置为“直接”。

   如果Adobe Pass后端标识有效的配置文件，则流应用程序无需使用选定的MVPD重新进行身份验证，因为已存在可用于后续决策流的配置文件。

   >[!IMPORTANT]
   >
   > 流应用程序在发出请求之前，必须确保它包含合作伙伴框架状态的有效值。
   >
   > <br/>
   > 
   > 有关`AP-Partner-Framework-Status`标头的更多详细信息，请参阅[AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md)文档。

## 使用合作伙伴身份验证响应创建和检索配置文件 {#create-and-retrieve-profile-using-partner-authentication-response}

### 先决条件 {#prerequisites-create-and-retrieve-profile-using-partner-authentication-response}

在使用合作伙伴身份验证响应检索配置文件之前，请确保满足以下先决条件：

* 合作伙伴框架必须使用选定的MVPD执行身份验证。
* 流应用程序必须从合作伙伴框架中获取合作伙伴身份验证响应以及合作伙伴框架状态信息，并将其传递到Adobe Pass服务器。

>[!IMPORTANT]
>
> 假设
>
> * 合作伙伴框架支持用户通过交互选择MVPD。
> * 合作伙伴框架支持用户通过交互使用选定的MVPD进行身份验证。
> * 合作伙伴框架提供用户权限和提供程序信息。

### 工作流 {#workflow-create-and-retrieve-profile-using-partner-authentication-response}

执行给定步骤以使用合作伙伴身份验证响应实施配置文件检索流程，如下图所示。

![使用合作伙伴身份验证响应创建和检索配置文件](../../../../../assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-retrieve-profile-using-partner-authentication-response-flow.png)

*使用合作伙伴身份验证响应创建和检索经过身份验证的配置文件*

1. **使用合作伙伴框架完成MVPD身份验证：**&#x200B;如果身份验证流程成功，合作伙伴框架与MVPD的交互将生成合作伙伴身份验证响应（SAML响应），该响应将随合作伙伴框架状态信息一起返回。

1. **返回合作伙伴身份验证响应：**&#x200B;流式应用程序验证响应数据以确保满足基本条件：
   * 已授予用户权限访问状态。
   * 用户提供程序映射标识符存在且有效。
   * 用户提供程序配置文件的到期日期（如果可用）有效。

1. **使用合作伙伴身份验证响应创建和检索配置文件：**&#x200B;流应用程序通过调用Profiles Partner终结点来收集创建和检索配置文件所需的所有数据。

   >[!IMPORTANT]
   >
   > 有关以下内容的详细信息，请参阅[使用合作伙伴身份验证响应创建和检索配置文件](../../apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md) API文档：
   >
   > * 所有&#x200B;_必需的_&#x200B;参数，如`serviceProvider`、`partner`和`SAMLResponse`
   > * 所有&#x200B;_必需的_&#x200B;标头，如`Authorization`、`AP-Device-Identifier`、`Content-Type`、`X-Device-Info`和`AP-Partner-Framework-Status`
   > * 所有&#x200B;_可选_&#x200B;标头和参数
   >
   > <br/>
   > 
   > 流应用程序在发出请求之前，必须确保它包含合作伙伴框架状态的有效值。
   >
   > <br/>
   > 
   > 有关`AP-Partner-Framework-Status`标头的更多详细信息，请参阅[AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md)文档。

1. **创建和保存合作伙伴配置文件：**&#x200B;在确保所有条件都得到满足之后，Adobe Pass服务器将创建和保存合作伙伴配置文件。

1. **返回有关合作伙伴配置文件的信息：** Profiles终结点响应包含有关合作伙伴配置文件的信息，包括设置为“appleSSO”的属性`type`。

   >[!IMPORTANT]
   >
   > 有关配置文件响应中提供的信息的详细信息，请参阅[使用合作伙伴身份验证响应创建和检索配置文件](../../apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md) API文档。
   > 
   > <br/>
   > 
   > Profiles Partner端点验证请求数据，以确保满足基本条件：
   >
   > * _必需_&#x200B;参数和标头必须有效。
   > * 提供的`serviceProvider`和`mvpd`之间的集成必须处于活动状态。
   >
   > <br/>
   > 
   > 如果验证失败，将生成错误响应，提供附加信息以遵守[增强型错误代码](../../../../features-standard/error-reporting/enhanced-error-codes.md)文档。
   >
   > <br/>
   >
   > Profiles Partner端点验证请求数据，以确保满足合作伙伴单点登录条件：
   >
   >  * Adobe Pass服务器中的合作伙伴单点登录配置必须有效且已启用。
   >  * 通过[AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md)标头接收的合作伙伴框架状态有效负载必须有效。
   >
   > <br/>
   >
   > 如果合作伙伴单一登录验证失败，则响应将默认使用基本用户档案检索流程。

1. **继续决策流：**&#x200B;流式应用程序可以继续后续决策流。

   >[!IMPORTANT]
   >
   > 流应用程序在发出请求之前，必须确保它包含合作伙伴框架状态的有效值。
   >
   > <br/>
   > 
   > 有关`AP-Partner-Framework-Status`标头的更多详细信息，请参阅[AP-Partner-Framework-Status](../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md)文档。
