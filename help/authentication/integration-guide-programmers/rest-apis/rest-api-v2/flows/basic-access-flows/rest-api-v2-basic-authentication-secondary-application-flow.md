---
title: 基本身份验证 — 辅助应用程序 — 流程
description: REST API V2 — 基本身份验证 — 辅助应用程序 — 流程
exl-id: 83bf592e-c679-4cfe-984d-710a9598c620
source-git-commit: 6b803eb0037e347d6ce147c565983c5a26de9978
workflow-type: tm+mt
source-wordcount: '2010'
ht-degree: 0%

---

# 在辅助应用程序中执行的基本身份验证流程 {#basic-authentication-flow-performed-within-secondary-application}

>[!IMPORTANT]
>
> 此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

>[!IMPORTANT]
>
> REST API V2实施受[限制机制](/help/authentication/integration-guide-programmers/throttling-mechanism.md)文档限制。

>[!MORELIKETHIS]
>
> 确保也访问[REST API V2常见问题解答](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)。

Adobe Pass身份验证权利中的&#x200B;**身份验证流程**&#x200B;允许流式应用程序验证用户是否拥有有效的MVPD帐户。 此过程要求用户拥有活动的MVPD帐户，并在MVPD登录页面上输入有效的登录凭据。

在以下情况下需要验证流程：

* 用户首次打开应用程序时。
* 用户以前的身份验证过期时。
* 用户从MVPD帐户注销时。
* 当用户希望使用其他MVPD进行身份验证时。

在所有这些情况下，调用任何配置文件端点的应用程序都会收到空响应或一个或多个配置文件，但目标对象是不同的MVPD。

**身份验证流**&#x200B;需要用户代理（浏览器）完成从应用程序到Adobe Pass后端，再到MVPD登录页面，最后返回到应用程序的一系列调用。 此流程可能包括多次重定向到MVPD系统并管理为每个域存储的Cookie或会话，如果没有用户代理，实现起来和安全可能比较困难。

基于支持用户交互以选择MVPD并在用户代理中使用所选MVPD进行身份验证的主要应用程序（流应用程序）功能，身份验证方案包括：

* [在主应用程序中执行身份验证](rest-api-v2-basic-authentication-primary-application-flow.md)
* [使用预选的mvpd在辅助应用程序中执行身份验证](./rest-api-v2-basic-authentication-secondary-application-flow.md)
* [无需预选mvpd就可在辅助应用程序中执行身份验证](./rest-api-v2-basic-authentication-secondary-application-flow.md)

## 使用预选的mvpd在辅助应用程序中执行身份验证 {#perform-authentication-within-secondary-application-with-preselected-mvpd}

### 先决条件 {#prerequisites-perform-authentication-within-secondary-application-with-preselected-mvpd}

在启动主应用程序中的身份验证流程并通过辅助应用程序中的用户交互完成该流程之前，请确保满足以下先决条件：

* 流应用程序必须选择一个MVPD。
* 流应用程序必须启动身份验证会话，才能使用选定的MVPD登录。
* 辅助应用程序必须在用户代理中使用选定的MVPD进行身份验证。

>[!IMPORTANT]
>
> 假设
>
> <br/>
> 
> * 流应用程序支持用户交互以选择MVPD。
> * 辅助应用程序（通常在辅助设备上）支持用户交互以在用户代理中与选定的MVPD进行身份验证。

### 工作流 {#workflow-perform-authentication-within-secondary-application-with-preselected-mvpd}

按照给定的步骤实施基本身份验证流程，该流程通过预选的MVPD在辅助应用程序中执行，如下图所示。

![使用预选的mvpd在辅助应用程序中执行身份验证](../../../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-perform-authentication-within-secondary-application-with-preselected-mvpd.png)

*使用预选的mvpd在辅助应用程序中执行身份验证*

1. **创建身份验证会话：**&#x200B;流应用程序通过调用会话终结点来收集启动身份验证会话所需的所有数据。

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
   > 创建身份验证会话时，流应用程序必须在单个调用中提供所有必需的参数。

1. **指示下一个操作：**&#x200B;会话终结点响应包含指导流应用程序执行下一个操作所需的数据。

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

1. **继续决策流：**&#x200B;会话终结点响应包含以下数据：
   * `actionName`属性设置为“授权”。
   * `actionType`属性设置为“直接”。

   如果Adobe Pass后端标识有效的配置文件，则流应用程序无需使用选定的MVPD重新进行身份验证，因为已存在可用于后续决策流的配置文件。

1. **显示身份验证代码：**&#x200B;会话终结点响应包含以下数据：
   * 可用于在辅助应用程序中恢复身份验证会话的`code`。
   * `actionName`属性设置为“身份验证”。
   * `actionType`属性设置为“交互式”。

   如果Adobe Pass后端未标识有效的配置文件，则流应用程序显示可用于在辅助应用程序中恢复身份验证会话的`code`。

1. **验证身份验证代码：**&#x200B;辅助应用程序验证提供的用户`code`，以确保它可以在用户代理中继续进行MVPD身份验证。

   >[!IMPORTANT]
   >
   > 有关以下内容的详细信息，请参阅[检索身份验证会话信息](../../apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) API文档：
   >
   > * 所有&#x200B;_必需的_&#x200B;参数，如`serviceProvider`和`code`
   > * 所有&#x200B;_必需的_&#x200B;标头，如`Authorization`
   > * 所有&#x200B;_可选_&#x200B;参数和标头

1. **返回有关身份验证会话的信息：**&#x200B;会话终结点响应包含以下数据：
   * `existing`属性包含已提供的现有参数。
   * `missing`属性包含缺少的参数，需要提供这些参数才能完成身份验证流程。

   >[!IMPORTANT]
   >
   > 有关会话验证响应中提供的信息的详细信息，请参阅[检索身份验证会话信息](../../apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) API文档。
   >
   > <br/>
   >
   > 会话端点验证请求数据以确保满足基本条件：
   >
   > * _必需_&#x200B;参数和标头必须有效。
   >
   > <br/>
   >
   > 如果验证失败，将生成错误响应，提供附加信息以遵守[增强型错误代码](../../../../features-standard/error-reporting/enhanced-error-codes.md)文档。

   >[!TIP]
   >
   > 建议：辅助应用程序可以通知用户在错误响应指示缺少身份验证会话的情况下使用的`code`无效，并建议他们使用新的身份验证会话重试。

1. **在用户代理中打开URL：**&#x200B;辅助应用程序打开用户代理以加载自行计算的`url`，向身份验证终结点发出请求。 此流程可能包括多次重定向，最终将用户引导至MVPD登录页面并提供有效凭据。

   >[!IMPORTANT]
   >
   > 有关以下内容的详细信息，请参阅用户代理](../../apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) API文档中的[执行身份验证：
   >
   > * 所有&#x200B;_必需的_&#x200B;参数，如`serviceProvider`和`code`
   > * 所有&#x200B;_可选_&#x200B;参数和标头

1. **完成MVPD身份验证：**&#x200B;如果身份验证流程成功，用户代理交互将在Adobe Pass后端保存常规配置文件并到达提供的`redirectUrl`。

1. **检索特定代码的配置文件：**&#x200B;流应用程序通过向“配置文件”端点发送请求，收集检索配置文件信息所需的所有数据。

   >[!IMPORTANT]
   >
   > 有关以下内容的详细信息，请参阅[检索特定代码](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) API的配置文件：
   > 
   > * 所有&#x200B;_必需的_&#x200B;参数，如`serviceProvider`和`code`
   > * 所有&#x200B;_必需的_&#x200B;标头，如`Authorization`、`AP-Device-Identifier`
   > * 所有&#x200B;_可选_&#x200B;参数和标头

   >[!TIP]
   >
   > 建议：流式应用程序可以使用`code`实施轮询机制，以检查是否成功生成并保存了常规配置文件。

1. **返回有关常规配置文件的信息：**&#x200B;配置文件终结点响应包含有关与收到的参数和标头关联的常规配置文件的信息。

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

## 无需预选mvpd就可在辅助应用程序中执行身份验证 {#perform-authentication-within-secondary-application-without-preselected-mvpd}

### 先决条件 {#prerequisites-perform-authentication-within-secondary-application-without-preselected-mvpd}

在启动主应用程序中的身份验证流程并通过辅助应用程序中的用户交互完成该流程之前，请确保满足以下先决条件：

* 当流应用程序需要登录时，它必须启动身份验证会话。
* 辅助应用程序必须选择MVPD。
* 辅助应用程序必须在用户代理中使用选定的MVPD进行身份验证。

>[!IMPORTANT]
>
> 假设
>
> <br/>
> 
> * 辅助应用程序（通常在辅助设备上）支持用户交互以选择MVPD。
> * 辅助应用程序（通常在辅助设备上）支持用户交互以在用户代理中与选定的MVPD进行身份验证。

### 工作流 {#workflow-perform-authentication-within-secondary-application-without-preselected-mvpd}

按照给定的步骤实施在辅助应用程序中执行的基本身份验证流程，而无需预先选择的MVPD，如下图所示。

![在辅助应用程序中执行身份验证，无需预先选择mvpd](../../../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-perform-authentication-within-secondary-application-without-preselected-mvpd.png)

*在辅助应用程序中执行身份验证，无需预先选择mvpd*

1. **创建身份验证会话：**&#x200B;流应用程序通过调用会话终结点来收集一些启动身份验证会话所需的数据。

   >[!IMPORTANT]
   >
   > 有关以下内容的详细信息，请参阅[创建身份验证会话](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) API文档：
   >
   > * 所有&#x200B;_必需的_&#x200B;参数，如`serviceProvider`
   > * 所有&#x200B;_必需的_&#x200B;标头，如`Authorization`、`AP-Device-Identifier`
   > * 所有&#x200B;_可选_&#x200B;参数和标头
   >
   > <br/>
   > 
   > 创建身份验证会话时，流应用程序无法在一次调用中提供所有必需的参数。

1. **指示下一个操作：**&#x200B;会话终结点响应包含指导流应用程序执行下一个操作的必需数据：
   * 可用于在辅助应用程序中恢复身份验证会话的`code`。
   * `actionName`属性设置为“resume”。
   * `actionType`属性设置为“直接”。

   >[!IMPORTANT]
   >
   > 有关会话响应中提供的信息的详细信息，请参阅[创建身份验证会话](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) API文档。
   > 
   > <br/>
   > 
   > 会话端点验证请求数据以确保满足基本条件：
   >
   > * _必需_&#x200B;参数和标头必须有效。
   >
   > <br/>
   > 
   > 如果验证失败，将生成错误响应，提供附加信息以遵守[增强型错误代码](../../../../features-standard/error-reporting/enhanced-error-codes.md)文档。

1. **显示身份验证代码：**&#x200B;流式应用程序显示可用于在辅助应用程序中恢复身份验证会话的`code`。

1. **提供身份验证会话缺少参数：**&#x200B;辅助应用程序收集恢复身份验证会话所需的所有缺失数据，并调用会话终结点。

   >[!IMPORTANT]
   >
   > 有关以下内容的详细信息，请参阅[恢复身份验证会话](../../apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) API文档：
   >
   > * 所有&#x200B;_必需的_&#x200B;参数，如`serviceProvider`、`mvpd`、`domainName`和`redirectUrl`
   > * 所有&#x200B;_必需的_&#x200B;标头，如`Authorization`、`AP-Device-Identifier`
   > * 所有&#x200B;_可选_&#x200B;参数和标头

1. **指示下一个操作：**&#x200B;会话终结点响应包含指导流应用程序执行下一个操作所需的数据。

   >[!IMPORTANT]
   >
   > 有关会话响应中提供的信息的详细信息，请参阅[恢复身份验证会话](../../apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) API文档。
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

   >[!TIP]
   >
   > 建议：辅助应用程序可以通知用户在错误响应指示缺少身份验证会话的情况下使用的`code`无效，并建议他们使用新的身份验证会话重试。

1. **指示现有配置文件：**&#x200B;会话终结点响应包含以下数据：
   * `actionName`属性设置为“授权”。
   * `actionType`属性设置为“直接”。

   如果Adobe Pass后端标识有效的配置文件，则流应用程序无需使用选定的MVPD重新进行身份验证，因为已存在可用于后续决策流的配置文件。

1. **在用户代理中打开URL：**&#x200B;会话终结点响应包含以下数据：
   * `url`可用于在MVPD登录页面中启动交互式身份验证。
   * `actionName`属性设置为“身份验证”。
   * `actionType`属性设置为“交互式”。

   如果Adobe Pass后端未识别有效的配置文件，则辅助应用程序将打开用户代理以加载提供的`url`，并向身份验证端点发出请求。 此流程可能包括多次重定向，最终将用户引导至MVPD登录页面并提供有效凭据。

1. **完成MVPD身份验证：**&#x200B;如果身份验证流程成功，用户代理交互将在Adobe Pass后端保存常规配置文件并到达提供的`redirectUrl`。

1. **检索特定代码的配置文件：**&#x200B;流应用程序通过向“配置文件”端点发送请求，收集检索配置文件信息所需的所有数据。

   >[!IMPORTANT]
   >
   > 有关以下内容的详细信息，请参阅[检索特定代码](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) API的配置文件：
   >
   > * 所有&#x200B;_必需的_&#x200B;参数，如`serviceProvider`和`code`
   > * 所有&#x200B;_必需的_&#x200B;标头，如`Authorization`、`AP-Device-Identifier`
   > * 所有&#x200B;_可选_&#x200B;参数和标头

   >[!TIP]
   >
   > 建议：流式应用程序可以使用`code`实施轮询机制，以检查是否成功生成并保存了常规配置文件。

1. **返回有关常规配置文件的信息：**&#x200B;配置文件终结点响应包含有关与收到的参数和标头关联的常规配置文件的信息。

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
