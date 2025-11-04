---
title: 基本配置文件 — 主要应用程序 — 流量
description: REST API V2 — 基本配置文件 — 主应用程序 — 流量
exl-id: 19ddf382-9a32-4b94-aa84-7611c0e1780e
source-git-commit: 92417dd4161be8ba97535404e262fd26d67383e4
workflow-type: tm+mt
source-wordcount: '974'
ht-degree: 0%

---

# 基本配置文件在主要应用程序中执行 {#basic-profiles-flow-primary-application}

>[!IMPORTANT]
>
> 此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

>[!IMPORTANT]
>
> REST API V2实施受[限制机制](/help/authentication/integration-guide-programmers/throttling-mechanism.md)文档限制。

>[!MORELIKETHIS]
>
> 确保也访问[REST API V2常见问题解答](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)。

Adobe Pass身份验证权利文件中的&#x200B;**配置文件流**&#x200B;允许流式应用程序访问有关活动用户登录的信息。

基本用户档案流允许您查询以下方案：

* [检索用户档案](#retrieve-profiles)
* [检索特定mvpd的配置文件](#retrieve-profile-for-specific-mvpd)
* [检索特定代码的配置文件](#retrieve-profile-for-specific-code)

## 检索用户档案 {#retrieve-profiles}

### 先决条件 {#prerequisites-retrieve-profiles}

在检索用户档案之前，请确保满足以下先决条件：

* 流应用程序想要检索所有常规配置文件。

### 工作流 {#workflow-retrieve-profiles}

按照给定的步骤实施在主应用程序中执行的基本用户档案检索流程，如下图所示。

![检索配置文件](/help/authentication/assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-retrieve-profiles-within-primary-application.png)

*检索配置文件*

1. **检索用户档案：**&#x200B;流式应用程序通过向“用户档案”端点发送请求，收集检索所有用户档案信息所需的所有数据。

   >[!IMPORTANT]
   >
   > 有关以下内容的详细信息，请参阅[检索配置文件](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) API文档：
   >
   > * 所有&#x200B;_必需的_&#x200B;参数，如`serviceProvider`
   > * 所有&#x200B;_必需的_&#x200B;标头，如`Authorization`、`AP-Device-Identifier`
   > * 所有&#x200B;_可选_&#x200B;参数和标头

1. **查找常规配置文件：** Adobe Pass服务器根据收到的参数和标头标识所有有效的配置文件。

1. **返回有关常规配置文件的信息：**&#x200B;配置文件端点响应包含有关找到的与收到的参数和标头关联的配置文件的信息。

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

1. **选择一个配置文件并继续决策流：**&#x200B;如果配置文件端点响应包含配置文件，则流应用程序将使用其内部逻辑（最终通过与最终用户交互）选择一个可用的配置文件以继续后续决策流。

1. **指示新的基本身份验证流程：**&#x200B;如果Profiles终结点响应不包含配置文件，则流式应用程序指示用户启动新的基本身份验证流程。

## 检索特定mvpd的配置文件 {#retrieve-profile-for-specific-mvpd}

### 先决条件 {#prerequisites-retrieve-profile-for-specific-mvpd}

在检索特定MVPD的配置文件之前，请确保满足以下先决条件：

* 具有选定或缓存的`mvpd`标识符的流应用程序想要检索特定MVPD的常规配置文件。

### 工作流 {#workflow-retrieve-profile-for-specific-mvpd}

按照给定的步骤，为主要应用程序中执行的特定MVPD实施基本配置文件检索流程，如下图所示。

![检索特定mvpd的配置文件](/help/authentication/assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-retrieve-profile-within-primary-application-for-specific-mvpd.png)

*检索特定mvpd的配置文件*

1. **检索特定mvpd的配置文件：**&#x200B;流应用程序通过向“配置文件”端点发送请求，收集检索该特定MVPD的配置文件信息所需的所有数据。

   >[!IMPORTANT]
   >
   > 有关以下内容的详细信息，请参阅特定mvpd [ API的](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)检索配置文件：
   >
   > * 所有&#x200B;_必需的_&#x200B;参数，如`serviceProvider`和`mvpd`
   > * 所有&#x200B;_必需的_&#x200B;标头，如`Authorization`、`AP-Device-Identifier`
   > * 所有&#x200B;_可选_&#x200B;参数和标头

1. **查找常规配置文件：** Adobe Pass服务器根据收到的参数和标头标识有效的配置文件。

1. **返回有关常规配置文件的信息：**&#x200B;配置文件终结点响应包含有关找到的与收到的参数和标头关联的配置文件的信息。

   >[!IMPORTANT]
   >
   > 有关配置文件响应中提供的信息的详细信息，请参阅特定mvpd [ API的](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)检索配置文件。
   > 
   > <br/>
   > 
   > 配置文件端点验证请求数据，以确保满足基本条件：
   >
   > * _必需_&#x200B;参数和标头必须有效。
   > * 提供的`serviceProvider`和`mvpd`之间的集成必须处于活动状态。
   >
   > <br/>
   > 
   > 如果验证失败，将生成错误响应，提供附加信息以遵守[增强型错误代码](../../../../features-standard/error-reporting/enhanced-error-codes.md)文档。

1. **继续决策流：**&#x200B;如果配置文件终结点响应包含配置文件，则流应用程序将使用配置文件信息继续后续决策流。

1. **指示新的基本身份验证流程：**&#x200B;如果Profiles终结点响应不包含配置文件，则流式应用程序指示用户启动新的基本身份验证流程。

## 检索特定代码的配置文件 {#retrieve-profile-for-specific-code}

### 先决条件 {#prerequisites-retrieve-profile-for-specific-code}

在检索特定身份验证代码的配置文件之前，请确保满足以下先决条件：

* 流式应用程序希望检索特定身份验证代码的配置文件，该应用程序具有用于与MVPD进行交互式身份验证的`code`。

### 工作流 {#workflow-retrieve-profile-for-specific-code}

按照给定的步骤为在主应用程序中执行的特定身份验证代码实施基本配置文件检索流程，如下图所示。

![检索特定代码的配置文件](/help/authentication/assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-retrieve-profile-within-primary-application-for-specific-code.png)

*检索特定代码的配置文件*

1. **检索特定代码的配置文件：**&#x200B;流应用程序通过向“配置文件”端点发送请求，收集所有必要的数据以检索该特定身份验证代码的配置文件信息。

   >[!IMPORTANT]
   >
   > 有关以下内容的详细信息，请参阅[检索特定代码](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) API的配置文件：
   >
   > * 所有&#x200B;_必需的_&#x200B;参数，如`serviceProvider`和`code`
   > * 所有&#x200B;_必需的_&#x200B;标头，如`Authorization`
   > * 所有&#x200B;_可选_&#x200B;参数和标头

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

1. **继续决策流：**&#x200B;如果配置文件终结点响应包含配置文件，则流应用程序将使用配置文件信息继续后续决策流。

1. **指示新的基本身份验证流程：**&#x200B;如果Profiles终结点响应不包含配置文件，则主应用程序指示用户启动新的基本身份验证流程。
