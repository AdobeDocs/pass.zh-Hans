---
title: 基本配置文件 — 辅助应用程序 — 流量
description: REST API V2 — 基本配置文件 — 辅助应用程序 — 流量
source-git-commit: 150e064d0287eaac446c694fb5a2633f7ea4b797
workflow-type: tm+mt
source-wordcount: '408'
ht-degree: 0%

---


# 基本配置文件在辅助应用程序中执行 {#basic-profiles-flow-secondary-application}

>[!IMPORTANT]
>
> 此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

>[!IMPORTANT]
>
> REST API V2实施受[限制机制](/help/authentication/throttling-mechanism.md)文档限制。

Adobe Pass身份验证权利中的&#x200B;**配置文件流**&#x200B;允许辅助应用程序访问有关活动用户登录的信息。

基本用户档案流允许您查询以下方案：

* [检索特定代码的配置文件](#retrieve-profile-for-specific-code)

## 检索特定代码的配置文件 {#retrieve-profile-for-specific-code}

### 先决条件 {#prerequisites-retrieve-profile-for-specific-code}

在检索特定身份验证代码的配置文件之前，请确保满足以下先决条件：

* 辅助应用程序具有用于与MVPD执行交互式身份验证的`code`，它想要检索特定身份验证代码的配置文件。

### 工作流 {#workflow-retrieve-profile-for-specific-code}

按照给定的步骤为在辅助应用程序中执行的特定身份验证代码实施基本配置文件检索流程，如下图所示。

![检索特定代码的配置文件](../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-retrieve-profile-within-secondary-application-for-specific-code.png)

*检索特定代码的配置文件*

1. **检索特定代码的配置文件：**&#x200B;辅助应用程序通过向“配置文件”端点发送请求，收集所有必需的数据以检索该特定身份验证代码的配置文件信息。

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
   > 如果验证失败，将生成错误响应，提供附加信息以遵守[增强型错误代码](../../../enhanced-error-codes.md)文档。

1. **指示验证流程已成功完成：**&#x200B;如果Profiles终结点响应包含配置文件，则辅助应用程序会处理该响应，并可以使用它选择性地在用户界面上显示特定消息。

1. **指示身份验证流程遇到问题：**&#x200B;如果Profiles终结点响应不包含配置文件，则辅助应用程序会处理该响应，并可以使用它选择性地在用户界面上显示特定消息。
