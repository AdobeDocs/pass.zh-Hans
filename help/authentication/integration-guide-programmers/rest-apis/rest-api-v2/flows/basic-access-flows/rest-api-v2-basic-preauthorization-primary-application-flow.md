---
title: 基本预授权 — 主要应用程序 — 流程
description: REST API V2 — 基本预授权 — 主要应用程序 — 流程
exl-id: f557f6c3-d5b2-4ec8-be51-91a90fbd31c0
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '496'
ht-degree: 0%

---

# 在主应用程序中执行的基本预授权流程 {#basic-preauthorization-flow-performed-within-primary-application}

>[!IMPORTANT]
>
> 此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

>[!IMPORTANT]
>
> REST API V2实施受[限制机制](/help/authentication/integration-guide-programmers/throttling-mechanism.md)文档限制。

Adobe Pass身份验证权利文件中的&#x200B;**预授权流**&#x200B;允许流式应用程序确定MVPD是否允许或拒绝用户访问资源列表。 此验证可确保应用程序可以向用户展示有关他们可能有资格查看的内容的准确信息。

## 使用特定的mvpd检索预授权决策 {#retrieve-preauthorization-decisions-using-specific-mvpd}

### 先决条件 {#prerequisites-retrieve-preauthorization-decisions-using-specific-mvpd}

在使用特定MVPD检索预授权决策之前，请确保满足以下先决条件：

* 流应用程序必须具有已使用以下基本身份验证流之一为MVPD成功创建的有效的常规配置文件：
   * [在主应用程序中执行身份验证](rest-api-v2-basic-authentication-primary-application-flow.md)
   * [使用预选的mvpd在辅助应用程序中执行身份验证](rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [无需预选mvpd就可在辅助应用程序中执行身份验证](rest-api-v2-basic-authentication-secondary-application-flow.md)
* 流应用程序希望检索预授权决策，以显示资源列表及其关联状态。

### 工作流 {#workflow-retrieve-preauthorization-decisions-using-specific-mvpd}

按照给定的步骤，使用在主应用程序中执行的特定MVPD实施基本的预授权流程，如下图所示。

![使用特定的mvpd检索预授权决策](../../../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-retrieve-preauthorization-decisions-within-primary-application-using-specific-mvpd.png)

*使用特定的mvpd检索预授权决策*

1. **检索预授权决策：**&#x200B;流应用程序通过调用Decisions Preauthorize终结点，收集所有必要的数据以获取资源列表的预授权决策。

   >[!IMPORTANT]
   >
   > 有关以下各项的详细信息，请参阅使用特定mvpd[&#x200B; API检索](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)预授权决策：
   >
   > * 所有&#x200B;_必需的_&#x200B;参数，如`serviceProvider`、`mvpd`和`resources`
   > * 所有&#x200B;_必需的_&#x200B;标头，如`Authorization`和`AP-Device-Identifier`
   > * 所有&#x200B;_可选_&#x200B;参数和标头

1. **查找常规配置文件：** Adobe Pass服务器根据收到的参数和标头标识有效的配置文件。

1. **检索所请求资源的MVPD决策：** Adobe Pass服务器调用MVPD预授权终结点以获取从流应用程序收到的每个资源的`Permit`或`Deny`决策。

1. **返回预授权决策：**&#x200B;决策预授权终结点响应包含每个资源的`Permit`或`Deny`决策：
   * `Permit`决策意味着资源可播放。 响应不包含媒体令牌，因为不得使用预授权流播放资源。
   * `Deny`决策意味着资源不可播放。 响应包含错误有效负载，该有效负载遵守[增强型错误代码](../../../../features-standard/error-reporting/enhanced-error-codes.md)文档。

   >[!IMPORTANT]
   >
   > 有关决策响应中提供的信息的详细信息，请参阅使用特定mvpd[&#x200B; API检索](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)预授权决策。
   > 
   > <br/>
   > 
   > Decisions Preauthorize端点验证请求数据以确保满足基本条件：
   >
   > * _必需_&#x200B;参数和标头必须有效。
   > * 提供的`serviceProvider`和`mvpd`之间的集成必须处于活动状态。
   >
   > <br/>
   > 
   > 如果验证失败，将生成错误响应，提供附加信息以遵守[增强型错误代码](../../../../features-standard/error-reporting/enhanced-error-codes.md)文档。

1. **处理预授权决策：**&#x200B;流式应用程序处理响应，并可以使用它选择性地在用户界面上显示每个资源的相应状态。
