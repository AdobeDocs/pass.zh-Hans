---
title: 单点登录 — 服务令牌 — 流程
description: REST API V2 — 单点登录 — 服务令牌 — 流程
exl-id: b0082d2a-e491-4cb5-bb40-35ba10db6b1a
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '1857'
ht-degree: 0%

---

# 使用服务令牌流进行单点登录{#single-sign-on-service-token-full-flows}

>[!IMPORTANT]
>
> 此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

>[!IMPORTANT]
>
> REST API V2实施受[限制机制](/help/authentication/integration-guide-programmers/throttling-mechanism.md)文档限制。

>[!MORELIKETHIS]
>
> 确保也访问[REST API V2常见问题解答](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)。

使用Adobe Pass服务时，服务令牌方法使多个应用程序能够使用唯一的用户标识符跨多个设备和平台实现单点登录(SSO)。

这些应用程序负责在Adobe Pass系统之外，使用外部身份服务检索唯一用户标识符有效负载，例如：

* 一种直接到使用者(DTC)服务，用户使用相同的凭据登录每台设备，并与相同的用户ID或用户帐户名相关联。
* 第三方身份验证服务，例如Google或Facebook，其中用户使用相同的凭据登录每台设备并与相同的电子邮件地址关联。

应用程序负责将此唯一用户标识符有效负载包含在所有指定该有效负载的`AD-Service-Token`标头中。

有关`AD-Service-Token`标头的更多详细信息，请参阅[AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md)文档。

## 使用服务令牌通过单点登录执行身份验证 {#performing-authentication-flow-using-service-token-single-sign-on-method}

### 先决条件 {#prerequisites-scenario-performing-authentication-flow-using-service-token-single-sign-on-method}

在使用服务令牌通过单点登录执行身份验证流程之前，请确保满足以下先决条件：

* 外部标识服务必须在多个设备和平台上跨所有应用程序以`JWS`有效负载的形式返回一致的信息。
* 第一个流应用程序必须检索唯一用户标识符，并将`JWS`有效负载作为所有指定该标识符的请求的[AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md)标头的一部分包含。
* 第一个流应用程序必须选择MVPD。
* 第一个流应用程序必须启动身份验证会话，才能使用选定的MVPD登录。
* 第一个流应用程序必须在用户代理中使用选定的MVPD进行身份验证。
* 第二个流应用程序必须检索唯一用户标识符，并将`JWS`有效负载作为所有指定该标识符的请求的[AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md)标头的一部分包含。

>[!IMPORTANT]
>
> 假设
> 
> <br/>
> 
> * 第一个流应用程序支持用户交互以选择MVPD。
> * 第一流应用程序支持用户交互以在用户代理中与所选MVPD进行身份验证。

### 工作流 {#workflow-steps-scenario-performing-authentication-flow-using-service-token-single-sign-on-method}

执行给定步骤，使用服务令牌实施通过单点登录的身份验证流程，如下图所示。

![使用服务令牌通过单点登录执行身份验证](../../../../../assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-perform-authentication-through-single-sign-on-using-service-token-flow.png)

*使用服务令牌通过单点登录执行身份验证*

1. **使用标识服务进行身份验证：**&#x200B;第一个流式应用程序在Adobe Pass系统之外调用标识服务，以获取与唯一用户标识符关联的`JWS`有效负载。

1. **将唯一用户标识符返回为JWS：**&#x200B;第一个流应用程序验证响应数据以确保满足基本安全条件：
   * 有效负载未过期。
   * 有效负载已签名。

1. **创建身份验证会话：**&#x200B;第一个流应用程序通过调用会话终结点来收集启动身份验证会话所需的所有数据。

   >[!IMPORTANT]
   >
   > 有关以下内容的详细信息，请参阅[创建身份验证会话](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) API文档：
   >
   > * 所有&#x200B;_必需的_&#x200B;参数，如`serviceProvider`、`mvpd`、`domainName`和`redirectUrl`
   > * 所有&#x200B;_必需的_&#x200B;标头，如`Authorization`、`AP-Device-Identifier`
   > * 所有&#x200B;_可选_&#x200B;参数和标头
   >
   > <br/>
   > 
   > 在发出请求之前，流应用程序必须确保它包含唯一用户标识符的有效值。
   >
   > <br/>
   > 
   > 有关`AD-Service-Token`标头的更多详细信息，请参阅[AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md)文档。

1. **指示下一个操作：**&#x200B;会话终结点响应包含指导第一个流应用程序执行下一个操作的必需数据。

   >[!IMPORTANT]
   >
   > 有关会话响应中提供的信息的详细信息，请参阅[创建身份验证会话](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) API文档。
   >
   > <br/>
   > 
   > 会话端点验证请求数据以确保满足基本条件：
   >
   > * _必需_&#x200B;参数和标头必须有效。
   > * 提供的`serviceProvider`和`mvpd`之间的集成必须处于活动状态。
   >
   > <br/>
   > 
   > 如果验证失败，将生成错误响应，提供附加信息以遵守[增强型错误代码](../../../../features-standard/error-reporting/enhanced-error-codes.md)文档。

1. **在用户代理中打开URL：**&#x200B;会话终结点响应包含以下数据：
   * `url`可用于在MVPD登录页面中启动交互式身份验证。
   * `actionName`属性设置为“身份验证”。
   * `actionType`属性设置为“交互式”。

   如果Adobe Pass后端未识别有效的配置文件，则第一个流应用程序将打开用户代理以加载提供的`url`，并向身份验证端点发出请求。 此流程可能包括多次重定向，最终将用户引导至MVPD登录页面并提供有效凭据。

1. **完成MVPD身份验证：**&#x200B;如果身份验证流程成功，用户代理交互将在Adobe Pass后端保存常规配置文件并到达提供的`redirectUrl`。

1. **检索特定代码的配置文件：**&#x200B;第一个流应用程序通过向“配置文件”端点发送请求，收集检索配置文件信息所需的所有数据。

   >[!IMPORTANT]
   >
   > 有关以下内容的详细信息，请参阅[检索特定代码](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) API的配置文件：
   > 
   > * 所有&#x200B;_必需的_&#x200B;参数，如`serviceProvider`、`code`
   > * 所有&#x200B;_必需的_&#x200B;标头，如`Authorization`、`AP-Device-Identifier`
   > * 所有&#x200B;_可选_&#x200B;参数和标头

   >[!TIP]
   >
   > 流应用程序必须等待用户代理到达提供的`redirectUrl`以检查是否成功生成并保存了常规配置文件。

1. **查找常规配置文件：** Adobe Pass服务器根据收到的参数和标头标识有效的配置文件。

1. **返回有关常规配置文件的信息：**&#x200B;配置文件终结点响应包含有关找到的与收到的参数和标头关联的配置文件的信息。

   >[!IMPORTANT]
   >
   > 有关配置文件响应中提供的信息的详细信息，请参阅[检索特定代码](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) API的配置文件。
   >
   > <br/>
   > 
   > 配置文件端点验证请求数据，以确保满足基本条件：
   >
   > * _必需_&#x200B;参数和标头必须有效。
   >
   > <br/>
   > 
   > 如果验证失败，将生成错误响应，提供附加信息以遵守[增强型错误代码](../../../../features-standard/error-reporting/enhanced-error-codes.md)文档。

1. **继续决策流：**&#x200B;第一个流应用程序可以继续后续决策流。

   >[!IMPORTANT]
   >
   > 在发出请求之前，流应用程序必须确保它包含唯一用户标识符的有效值。
   >
   > <br/>
   > 
   > 有关`AD-Service-Token`标头的更多详细信息，请参阅[AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md)文档。

1. **使用标识服务进行身份验证：**&#x200B;第二个流式应用程序在Adobe Pass系统之外调用标识服务，以获取与唯一用户标识符关联的`JWS`有效负载。

1. **将唯一用户标识符返回为JWS：**&#x200B;第二个流应用程序验证响应数据以确保满足基本安全条件：
   * 有效负载未过期。
   * 有效负载已签名。

1. **检索配置文件：**&#x200B;第二个流应用程序通过向“配置文件”端点发送请求，收集检索所有配置文件信息所需的所有数据。

   >[!IMPORTANT]
   >
   > 有关以下内容的详细信息，请参阅[检索配置文件](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) API文档：
   >
   > * 所有&#x200B;_必需的_&#x200B;参数，如`serviceProvider`
   > * 所有&#x200B;_必需的_&#x200B;标头，如`Authorization`、`AP-Device-Identifier`
   > * 所有&#x200B;_可选_&#x200B;参数和标头
   >
   > <br/>
   > 
   > 在发出请求之前，流应用程序必须确保它包含唯一用户标识符的有效值。
   >
   > <br/>
   > 
   > 有关`AD-Service-Token`标头的更多详细信息，请参阅[AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md)文档。

1. **查找单点登录配置文件：** Adobe Pass服务器根据收到的参数和标头标识有效的单点登录配置文件。

1. **返回有关单点登录配置文件的信息：**&#x200B;配置文件终结点响应包含有关找到的与收到的参数和标头关联的配置文件的信息。

   >[!IMPORTANT]
   >
   > 有关配置文件响应中提供的信息的详细信息，请参阅[检索配置文件](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) API文档。
   > 
   > <br/>
   > 
   > 配置文件端点验证请求数据，以确保满足基本条件：
   >
   > * _必需_&#x200B;参数和标头必须有效。
   >
   > <br/>
   > 
   > 如果验证失败，将生成错误响应，提供附加信息以遵守[增强型错误代码](../../../../features-standard/error-reporting/enhanced-error-codes.md)文档。

1. **继续决策流：**&#x200B;第二个流应用程序可以继续后续决策流。

   >[!IMPORTANT]
   >
   > 在发出请求之前，流应用程序必须确保它包含唯一用户标识符的有效值。
   >
   > <br/>
   > 
   > 有关`AD-Service-Token`标头的更多详细信息，请参阅[AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md)文档。

## 使用服务令牌通过单点登录检索授权决策 {#performing-authorization-flow-using-service-token-single-sign-on-method}

### 先决条件 {#prerequisites-scenario-performing-authorization-flow-using-service-token-single-sign-on-method}

在使用服务令牌通过单点登录执行授权流之前，请确保满足以下先决条件：

* 外部标识服务必须在多个设备和平台上跨所有应用程序以`JWS`有效负载的形式返回一致的信息。
* 第一个流应用程序必须检索唯一用户标识符，并将`JWS`有效负载作为所有指定该标识符的请求的[AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md)标头的一部分包含。
* 第二个流应用程序必须在播放用户选择的资源之前检索授权决定。

>[!IMPORTANT]
>
> 假设
>
> <br/>
> 
> * 第一个流应用程序已执行身份验证，并包含[AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md)请求标头的有效值。

### 工作流 {#workflow-steps-scenario-performing-authorization-flow-using-service-token-single-sign-on-method}

执行给定步骤以使用服务令牌通过单点登录实施授权流，如下图所示。

![使用服务令牌通过单点登录检索授权决策](../../../../../assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-retrieve-authorization-decisions-through-single-sign-on-using-service-token-flow.png)

*使用服务令牌通过单点登录检索授权决策*

1. **使用标识服务进行身份验证：**&#x200B;第二个流式应用程序在Adobe Pass系统之外调用标识服务，以获取与唯一用户标识符关联的`JWS`有效负载。

1. **将唯一用户标识符返回为JWS：**&#x200B;第二个流应用程序验证响应数据以确保满足基本安全条件：
   * 有效负载未过期。
   * 有效负载已签名。

1. **检索授权决定：**&#x200B;第二个流应用程序通过调用Decisions Authorize终结点，收集所有必需的数据以获取特定资源的授权决定。

   >[!IMPORTANT]
   >
   > 有关以下各项的详细信息，请参阅使用特定mvpd[ API检索](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)授权决策：
   >
   > * 所有&#x200B;_必需的_&#x200B;参数，如`serviceProvider`、`mvpd`和`resources`
   > * 所有&#x200B;_必需的_&#x200B;标头，如`Authorization`和`AP-Device-Identifier`
   > * 所有&#x200B;_可选_&#x200B;参数和标头
   >
   > <br/>
   > 
   > 在发出请求之前，流应用程序必须确保它包含唯一用户标识符的有效值。
   >
   > <br/>
   > 
   > 有关`AD-Service-Token`标头的更多详细信息，请参阅[AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md)文档。

1. **查找单点登录配置文件：** Adobe Pass服务器根据收到的参数和标头标识有效的单点登录配置文件。

1. **检索所请求资源的MVPD决策：** Adobe Pass服务器调用MVPD授权终结点以获取从流应用程序接收的特定资源的`Permit`或`Deny`决策。

1. **返回具有媒体令牌的`Permit`决策：**&#x200B;决策授权终结点响应包含`Permit`决策和媒体令牌。

   >[!IMPORTANT]
   >
   > 有关决策响应中提供的信息的详细信息，请参阅使用特定mvpd[ API检索](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)授权决策。
   > 
   > <br/>
   > 
   > Decisions Authorize端点验证请求数据以确保满足基本条件：
   >
   > * _必需_&#x200B;参数和标头必须有效。
   > * 提供的`serviceProvider`和`mvpd`之间的集成必须处于活动状态。
   >
   > <br/>
   > 
   > 如果验证失败，将生成错误响应，提供附加信息以遵守[增强型错误代码](../../../../features-standard/error-reporting/enhanced-error-codes.md)文档。

1. **使用媒体令牌启动流：**&#x200B;第二个流应用程序使用媒体令牌播放内容。

1. **返回包含详细信息的`Deny`决策：** Decisions Authorize终结点响应包含`Deny`决策和错误有效负载，该有效负载遵守[增强型错误代码](../../../../features-standard/error-reporting/enhanced-error-codes.md)文档。

   >[!IMPORTANT]
   >
   > 有关决策响应中提供的信息的详细信息，请参阅使用特定mvpd[ API检索](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)授权决策。
   > 
   > <br/>
   > 
   > Decisions Authorize端点验证请求数据以确保满足基本条件：
   >
   > * _必需_&#x200B;参数和标头必须有效。
   > * 提供的`serviceProvider`和`mvpd`之间的集成必须处于活动状态。
   >
   > <br/>
   > 
   > 如果验证失败，将生成错误响应，提供附加信息以遵守[增强型错误代码](../../../../features-standard/error-reporting/enhanced-error-codes.md)文档。

1. **处理`Deny`决策详细信息：**&#x200B;第二个流应用程序处理来自响应的错误信息，并可用于在用户界面上显示特定消息。

>[!NOTE]
>
> 预授权流的步骤与授权流的步骤相同，不同之处在于，使用的终结点是使用特定mvpd[文档的](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)Retrieve preauthorization决策中描述的终结点。
