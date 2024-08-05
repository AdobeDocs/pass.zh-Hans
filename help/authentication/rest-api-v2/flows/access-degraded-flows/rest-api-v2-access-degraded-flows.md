---
title: 降级访问流
description: REST API V2 — 降级访问流
source-git-commit: 4d1ce1301d6baf7309e8ee52c43b02403aa2fab9
workflow-type: tm+mt
source-wordcount: '1594'
ht-degree: 0%

---


# 降级访问流 {#degraded-access-flows}

>[!NOTE]
>
> 此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

降级提供临时绕过特定MVPD身份验证和授权端点。 通常，程序员会启动此操作，但无论谁触发了降级事件，该操作都取决于与受影响的MVPD所做的预先安排。

有关降级功能的更多详细信息，请参阅[降级](../../../degradation-api-overview.md)文档。

降级访问流允许您查询以下情况：

* [在应用降级时执行身份验证](#perform-authentication-while-degradation-is-applied)
* [在应用降级时检索授权决策](#retrieve-authorization-decisions-while-degradation-is-applied)
* [在应用降级时检索预授权决策](#retrieve-preauthorization-decisions-while-degradation-is-applied)
* [应用降级时检索配置文件](#retrieve-profile-while-degradation-is-applied)

## 在应用降级时执行身份验证 {#perform-authentication-while-degradation-is-applied}

### 先决条件 {#prerequisites-perform-authentication-while-degradation-is-applied}

在应用降级的情况下执行身份验证流之前，请确保满足以下先决条件：

* 当流应用程序需要使用MVPD登录时，它必须启动身份验证会话。

>[!IMPORTANT]
> 
> 假设
> 
> <br/>
> 
> * 流应用程序没有保存在Adobe Pass后端中的该特定MVPD的有效配置文件。
> * 提供的`serviceProvider`和`mvpd`之间的集成应用了AuthNAll降级规则。

### 工作流 {#workflow-perform-authentication-while-degradation-is-applied}

在应用降级时，请按照给定的步骤实施身份验证流程，如下图所示。

![在应用降级时执行身份验证](../../../assets/rest-api-v2/flows/access-degraded-flows/rest-api-v2-perform-authentication-while-degradation-is-applied-flow.png)

*在应用降级时执行身份验证*

1. **创建身份验证会话：**&#x200B;流应用程序通过调用会话终结点来收集启动身份验证会话所需的所有数据。

   有关以下内容的详细信息，请参阅[创建身份验证会话](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) API文档：
   * 所有&#x200B;_必需的_&#x200B;参数，如`serviceProvider`、`mvpd`、`domainName`和`redirectUrl`
   * 所有&#x200B;_必需的_&#x200B;标头，如`Authorization`和`AP-Device-Identifier`
   * 所有&#x200B;_可选_&#x200B;参数和标头

1. **检查降级规则：** Adobe Pass服务器验证提供的`serviceProvider`和`mvpd`之间的集成是否应用了AuthNAll降级规则。

1. **指示下一个操作：**&#x200B;会话终结点响应包含指导流应用程序执行下一个操作的必需数据：
   * `actionName`属性设置为“授权”。
   * `actionType`属性设置为“直接”。

   有关会话响应中提供的信息的详细信息，请参阅[创建身份验证会话](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) API文档。

   >[!IMPORTANT]
   >
   > 会话端点验证请求数据以确保满足基本条件：
   >
   > * _必需_&#x200B;参数和标头必须有效。
   > * 提供的`serviceProvider`和`mvpd`之间的集成必须处于活动状态。
   >
   > <br/>
   > 
   > 如果基本验证失败，将生成错误响应，提供附加信息，这些信息将遵循[增强型错误代码](../../../enhanced-error-codes.md)文档。
   >
   > <br/>
   > 
   > 会话端点使用请求数据来检查是否满足降级访问条件：
   >
   > * 提供的`serviceProvider`和`mvpd`之间的集成必须应用AuthNAll降级规则。
   >
   > <br/>
   > 
   > 如果降级访问验证失败，响应将默认使用基本验证流程。

1. **继续决策流：**&#x200B;流式应用程序可以继续后续决策流。

## 在应用降级时检索授权决策 {#retrieve-authorization-decisions-while-degradation-is-applied}

### 先决条件 {#prerequisites-retrieve-authorization-decisions-while-degradation-is-applied}

在应用降级的情况下检索授权决策之前，请确保满足以下先决条件：

* 流应用程序必须先检索授权决策，然后才能播放用户选择的资源。

>[!IMPORTANT]
>
> 假设
> 
> <br/>
> 
> * 流应用程序没有该特定MVPD的有效配置文件。
> * 提供的`serviceProvider`和`mvpd`之间的集成应用了AuthZAll或AuthNAll降级规则。

### 工作流 {#workflow-retrieve-authorization-decisions-while-degradation-is-applied}

在应用降级时，请按照给定的步骤实施授权流，如下图所示。

![应用降级时检索授权决策](../../../assets/rest-api-v2/flows/access-degraded-flows/rest-api-v2-retrieve-authorization-decisions-while-degradation-is-applied-flow.png)

*应用降级时检索授权决策*

1. **检索授权决定：**&#x200B;流应用程序通过调用Decisions Authorize终结点，收集所有必需的数据以获取特定资源的授权决定。

   有关以下各项的详细信息，请参阅使用特定mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API检索[授权决策：
   * 所有&#x200B;_必需的_&#x200B;参数，如`serviceProvider`、`mvpd`和`resources`
   * 所有&#x200B;_必需的_&#x200B;标头，如`Authorization`和`AP-Device-Identifier`
   * 所有&#x200B;_可选_&#x200B;参数和标头

1. **检查降级规则：** Adobe Pass服务器验证提供的`serviceProvider`和`mvpd`之间的集成是否应用了AuthZAll或AuthNAll降级规则。

1. **返回具有媒体令牌的`Permit`决策：**&#x200B;决策授权终结点响应包含`Permit`决策和媒体令牌。

   有关决策响应中提供的信息的详细信息，请参阅使用特定mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API检索[授权决策。

   >[!IMPORTANT]
   >
   > Decisions Authorize端点验证请求数据以确保满足基本条件：
   >
   > * _必需_&#x200B;参数和标头必须有效。
   > * 提供的`serviceProvider`和`mvpd`之间的集成必须处于活动状态。
   >
   > <br/>
   > 
   > 如果基本验证失败，将生成错误响应，提供附加信息，这些信息将遵循[增强型错误代码](../../../enhanced-error-codes.md)文档。
   >
   > <br/>
   >
   > Decisions Authorize端点使用请求数据检查是否满足降级访问条件：
   >
   > * 提供的`serviceProvider`和`mvpd`之间的集成必须应用AuthZAll或AuthNAll降级规则。
   >
   > <br/>
   > 
   > 如果降级访问验证失败，响应将默认使用基本授权流。

1. **使用媒体令牌启动流：**&#x200B;流应用程序使用媒体令牌播放内容。

## 在应用降级时检索预授权决策 {#retrieve-preauthorization-decisions-while-degradation-is-applied}

### 先决条件 {#prerequisites-retrieve-preauthorization-decisions-while-degradation-is-applied}

在应用降级的情况下检索预授权决策之前，请确保满足以下先决条件：

* 流应用程序希望检索预授权决策，以显示资源列表及其关联状态。

>[!IMPORTANT]
>
> 假设
>
> <br/>
> 
> * 流应用程序没有该特定MVPD的有效配置文件。
> * 提供的`serviceProvider`和`mvpd`之间的集成应用了AuthZAll或AuthNAll降级规则。

### 工作流 {#workflow-retrieve-preauthorization-decisions-while-degradation-is-applied}

在应用降级时，请按照给定的步骤实施预授权流，如下图所示。

![应用降级时检索预授权决策](../../../assets/rest-api-v2/flows/access-degraded-flows/rest-api-v2-retrieve-preauthorization-decisions-while-degradation-is-applied-flow.png)

*应用降级时检索预授权决策*

1. **检索预授权决策：**&#x200B;流应用程序通过调用Decisions Preauthorize终结点，收集所有必要的数据以获取资源列表的预授权决策。

   有关以下各项的详细信息，请参阅使用特定mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) API检索[预授权决策：
   * 所有&#x200B;_必需的_&#x200B;参数，如`serviceProvider`、`mvpd`和`resources`
   * 所有&#x200B;_必需的_&#x200B;标头，如`Authorization`和`AP-Device-Identifier`
   * 所有&#x200B;_可选_&#x200B;参数和标头

1. **检查降级规则：** Adobe Pass服务器验证提供的`serviceProvider`和`mvpd`之间的集成是否应用了AuthZAll或AuthNAll降级规则。

1. **返回预授权决策：**&#x200B;决策预授权终结点响应包含每个资源的`Permit`决策。

   有关决策响应中提供的信息的详细信息，请参阅使用特定mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) API检索[预授权决策。

   >[!IMPORTANT]
   >
   > Decisions Preauthorize端点验证请求数据以确保满足基本条件：
   >
   > * _必需_&#x200B;参数和标头必须有效。
   > * 提供的`serviceProvider`和`mvpd`之间的集成必须处于活动状态。
   >
   > <br/>
   > 
   > 如果基本验证失败，将生成错误响应，提供附加信息，这些信息将遵循[增强型错误代码](../../../enhanced-error-codes.md)文档。
   >
   > <br/>
   >
   > Decisions Preauthorize端点使用请求数据检查是否满足降级访问条件：
   >
   > * 提供的`serviceProvider`和`mvpd`之间的集成必须应用AuthZAll或AuthNAll降级规则。
   >
   > <br/>
   > 
   > 如果降级访问验证失败，响应将默认使用基本预授权流程。

1. **处理预授权决策：**&#x200B;流式应用程序处理响应，并可以使用它选择性地在用户界面上显示每个资源的相应状态。

## 应用降级时检索配置文件 {#retrieve-profile-while-degradation-is-applied}

>[!IMPORTANT]
>
> 在应用降级时，配置文件端点查询是可选的。
>
> <br/>
> 
> 在应用降级时，会话端点响应会指示应用程序继续处理决策流。 有关更多详细信息，请参阅[应用降级时执行身份验证](#perform-authentication-while-degradation-is-applied)部分。

### 先决条件 {#prerequisites-retrieve-profile-while-degradation-is-applied}

在应用降级的情况下检索特定MVPD的配置文件之前，请确保满足以下先决条件：

* 具有选定或缓存的`mvpd`标识符的流应用程序想要检索特定MVPD的配置文件。

>[!IMPORTANT]
>
> 假设
>
> <br/>
> 
> * 流应用程序没有该特定MVPD的有效配置文件。
> * 提供的`serviceProvider`和`mvpd`之间的集成应用了AuthNAll降级规则。

### 工作流 {#workflow-retrieve-profile-while-degradation-is-applied}

在应用降级的情况下，按照给定的步骤实施特定MVPD的配置文件检索流，如下图所示。

![应用降级时检索配置文件](../../../assets/rest-api-v2/flows/access-degraded-flows/rest-api-v2-retrieve-profile-while-degradation-is-applied-flow.png)

*应用降级时检索配置文件*

1. **检索特定mvpd的配置文件：**&#x200B;流应用程序通过向“配置文件”端点发送请求，收集所有必需的数据以检索该特定MVPD的配置文件信息。

   有关以下内容的详细信息，请参阅特定mvpd ](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles-for-specific-mvpd.md) API的[检索配置文件：
   * 所有&#x200B;_必需的_&#x200B;参数，如`serviceProvider`和`mvpd`
   * 所有&#x200B;_必需的_&#x200B;标头，如`Authorization`和`AP-Device-Identifier`
   * 所有&#x200B;_可选_&#x200B;参数和标头

1. **检查降级规则：** Adobe Pass服务器验证提供的`serviceProvider`和`mvpd`之间的集成是否应用了AuthNAll降级规则。

1. **返回有关已降级配置文件的信息：**&#x200B;配置文件终结点响应包含有关已降级配置文件的信息，包括设置为“已降级”的属性`type`。

   有关配置文件响应中提供的信息的详细信息，请参阅特定mvpd ](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles-for-specific-mvpd.md) API的[检索配置文件。

   >[!IMPORTANT]
   >
   > 配置文件端点验证请求数据，以确保满足基本条件：
   >
   > * _必需_&#x200B;参数和标头必须有效。
   > * 提供的`serviceProvider`和`mvpd`之间的集成必须处于活动状态。
   >
   > <br/>
   > 
   > 如果基本验证失败，将生成错误响应，提供附加信息，这些信息将遵循[增强型错误代码](../../../enhanced-error-codes.md)文档。
   >
   > <br/>
   > 
   > 配置文件端点使用请求数据检查是否满足降级访问条件：
   >
   > * 提供的`serviceProvider`和`mvpd`之间的集成必须应用AuthNAll降级规则。
   >
   > <br/>
   > 
   > 如果降级访问验证失败，响应将默认使用基本配置文件检索流程。

1. **继续决策流：**&#x200B;如果配置文件终结点响应包含配置文件，则流应用程序使用降级配置文件信息继续后续决策流。

1. **指示新的基本身份验证流程：**&#x200B;如果Profiles终结点响应不包含配置文件，则流式应用程序指示用户启动新的基本身份验证流程。

>[!NOTE]
>
> 特定身份验证代码的配置文件检索流程的步骤与上述步骤相同，不同之处在于使用的端点是[检索特定代码的配置文件](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles-for-specific-code.md)文档中描述的端点。
