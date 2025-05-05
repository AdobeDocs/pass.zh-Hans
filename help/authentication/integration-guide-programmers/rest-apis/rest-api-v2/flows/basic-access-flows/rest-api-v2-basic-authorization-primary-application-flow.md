---
title: 基本授权 — 主要应用程序 — 流程
description: REST API V2 — 基本授权 — 主应用程序 — 流程
exl-id: 46bc9326-966e-44fc-8546-2f58be01b7bc
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '603'
ht-degree: 0%

---

# 在主应用程序中执行的基本授权流程 {#basic-authorization-flow-performed-within-primary-application}

>[!IMPORTANT]
>
> 此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

>[!IMPORTANT]
>
> REST API V2实施受[限制机制](/help/authentication/integration-guide-programmers/throttling-mechanism.md)文档限制。

Adobe Pass身份验证权利中的&#x200B;**授权流**&#x200B;允许流式应用程序确定MVPD是允许还是拒绝用户流式传输内容的请求。 如果决策为`Permit`，则响应包含媒体令牌。 Adobe Pass服务器对媒体令牌进行签名，并允许流应用程序使用媒体令牌验证器库在释放流之前检查其真实性。

在链接在从CDN发布流的权限链中的流应用程序后端服务上，应该使用媒体令牌验证器库进行验证。

## 使用特定的mvpd检索授权决策 {#retrieve-authorization-decisions-using-specific-mvpd}

### 先决条件 {#prerequisites-retrieve-authorization-decisions-using-specific-mvpd}

在使用特定MVPD检索授权决策之前，请确保满足以下先决条件：

* 流应用程序必须具有使用基本身份验证流之一为MVPD成功创建的有效常规配置文件：
   * [在主应用程序中执行身份验证](rest-api-v2-basic-authentication-primary-application-flow.md)
   * [使用预选的mvpd在辅助应用程序中执行身份验证](rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [无需预选mvpd就可在辅助应用程序中执行身份验证](rest-api-v2-basic-authentication-secondary-application-flow.md)
* 流应用程序必须先检索授权决策，然后才能播放用户选择的资源。

### 工作流 {#workflow-retrieve-authorization-decisions-using-specific-mvpd}

按照给定的步骤，使用在主应用程序中执行的特定MVPD实施基本授权流，如下图所示。

![使用特定的mvpd检索授权决策](../../../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-retrieve-authorization-decisions-within-primary-application-using-specific-mvpd.png)

*使用特定的mvpd检索授权决策*

1. **检索授权决定：**&#x200B;流应用程序通过调用Decisions Authorize终结点，收集所有必需的数据以获取特定资源的授权决定。

   >[!IMPORTANT]
   >
   > 有关以下各项的详细信息，请参阅使用特定mvpd[&#128279;](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API检索授权决策：
   >
   > * 所有&#x200B;_必需的_&#x200B;参数，如`serviceProvider`、`mvpd`和`resources`
   > * 所有&#x200B;_必需的_&#x200B;标头，如`Authorization`和`AP-Device-Identifier`
   > * 所有&#x200B;_可选_&#x200B;参数和标头

1. **查找常规配置文件：** Adobe Pass服务器根据收到的参数和标头标识有效的配置文件。

1. **检索所请求资源的MVPD决策：** Adobe Pass服务器调用MVPD授权终结点以获取从流应用程序接收的特定资源的`Permit`或`Deny`决策。

1. **返回具有媒体令牌的`Permit`决策：**&#x200B;决策授权终结点响应包含`Permit`决策和媒体令牌。

   >[!IMPORTANT]
   >
   > 有关决策响应中提供的信息的详细信息，请参阅使用特定mvpd[&#128279;](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API检索授权决策。
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

1. **使用媒体令牌启动流：**&#x200B;流应用程序使用媒体令牌播放内容。

1. **返回包含详细信息的`Deny`决策：** Decisions Authorize终结点响应包含`Deny`决策和错误有效负载，该有效负载遵守[增强型错误代码](../../../../features-standard/error-reporting/enhanced-error-codes.md)文档。

   >[!IMPORTANT]
   >
   > 有关决策响应中提供的信息的详细信息，请参阅使用特定mvpd[&#128279;](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API检索授权决策。
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

1. **处理`Deny`决策详细信息：**&#x200B;流式应用程序处理来自响应的错误信息，并可以使用它选择性地在用户界面上显示特定消息。
