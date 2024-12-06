---
title: 基本注销 — 主要应用程序 — 流程
description: REST API V2 — 基本注销 — 主应用程序 — 流程
exl-id: 21dbff4a-0d69-4f81-b04f-e99d743c35b3
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '948'
ht-degree: 0%

---

# 在主应用程序中执行的基本注销流程 {#basic-logout-flow-performed-within-primary-application}

>[!IMPORTANT]
>
> 此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

>[!IMPORTANT]
>
> REST API V2实施受[限制机制](/help/authentication/integration-guide-programmers/throttling-mechanism.md)文档限制。

Adobe Pass身份验证权利文件中的&#x200B;**注销流**&#x200B;允许流式应用程序执行两个主要步骤：

* 删除保存在Adobe Pass后端上的常规配置文件。
* 使用用户代理（浏览器）导航到MVPD注销端点，从而触发MVPD后端上的清理。

基本注销流程允许您查询以下方案：

* [使用注销端点启动特定mvpd的注销](#initiate-logout-for-specific-mvpd-with-logout-endpoint)
* [启动特定mvpd的注销，但不注销端点](#initiate-logout-for-specific-mvpd-without-logout-endpoint)

## 为具有注销终结点的特定mvpd启动注销 {#initiate-logout-for-specific-mvpd-with-logout-endpoint}

### 先决条件 {#prerequisites-initiate-logout-for-specific-mvpd-with-logout-endpoint}

在使用注销端点为特定MVPD启动注销之前，请确保满足以下先决条件：

* 流应用程序必须具有使用基本身份验证流之一为MVPD成功创建的有效常规配置文件：
   * [在主应用程序中执行身份验证](rest-api-v2-basic-authentication-primary-application-flow.md)
   * [使用预选的mvpd在辅助应用程序中执行身份验证](rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [无需预选mvpd就可在辅助应用程序中执行身份验证](rest-api-v2-basic-authentication-secondary-application-flow.md)
* 当流应用程序需要注销MVPD时，它必须启动注销流程。

>[!IMPORTANT]
>
> 假设
>
> <br/>
> 
> * MVPD支持注销流并具有注销端点。

### 工作流 {#workflow-initiate-logout-for-specific-mvpd-with-logout-endpoint}

按照给定的步骤实施特定MVPD的基本注销流程，该流程具有在主应用程序中执行的注销端点，如下图所示。

![使用注销终结点启动特定mvpd的注销](../../../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-initiate-logout-within-primary-application-for-specific-mvpd-with-logout-endpoint.png)

*使用注销终结点启动特定mvpd的注销*

1. **启动Adobe Pass注销：**&#x200B;流应用程序通过调用Adobe Pass注销终结点来收集启动注销流所需的所有数据。

   >[!IMPORTANT]
   >
   > 有关以下内容的详细信息，请参阅特定mvpd ](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) API的[Initiate注销：
   >
   > * 所有&#x200B;_必需的_&#x200B;参数，如`serviceProvider`、`mvpd`和`redirectUrl`
   > * 所有&#x200B;_必需的_&#x200B;标头，如`Authorization`、`AP-Device-Identifier`
   > * 所有&#x200B;_可选_&#x200B;参数和标头

1. **查找常规配置文件：** Adobe Pass服务器根据收到的参数和标头标识有效的配置文件。

1. **删除常规配置文件：** Adobe Pass服务器从Adobe Pass后端删除已识别的常规配置文件。

1. **指示下一个操作：** Adobe Pass注销终结点响应包含指导流应用程序执行下一个操作所需的数据：
   * `url`属性存在，因为MVPD支持注销流。
   * `actionName`属性设置为“注销”。
   * `actionType`属性设置为“交互式”。

   >[!IMPORTANT]
   >
   > 有关注销响应中提供的信息的详细信息，请参阅特定mvpd ](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) API的[启动注销。
   > 
   > <br/>
   > 
   > Adobe Pass注销端点验证请求数据以确保满足基本条件：
   >
   > * _必需_&#x200B;参数和标头必须有效。
   > * 提供的`serviceProvider`和`mvpd`之间的集成必须处于活动状态。
   >
   > <br/>
   > 
   > 如果验证失败，将生成错误响应，提供附加信息以遵守[增强型错误代码](../../../../features-standard/error-reporting/enhanced-error-codes.md)文档。

1. **启动MVPD注销：**&#x200B;流应用程序读取`url`并使用用户代理启动与MVPD的注销流程。 该流可能包括到MVPD系统的多个重定向。 但是，结果是MVPD执行其内部清理，并将最终注销确认发送回Adobe Pass后端。

1. **指示注销完成：**&#x200B;流应用程序可以等待用户代理到达提供的`redirectUrl`，并可以将它用作信号，以选择在用户界面上显示特定消息。

## 启动特定mvpd的注销，但不注销端点 {#initiate-logout-for-specific-mvpd-without-logout-endpoint}

### 先决条件 {#prerequisites-initiate-logout-for-specific-mvpd-without-logout-endpoint}

在为没有注销端点的特定MVPD启动注销之前，请确保满足以下先决条件：

* 流应用程序必须具有使用基本身份验证流之一为MVPD成功创建的有效常规配置文件：
   * [在主应用程序中执行身份验证](rest-api-v2-basic-authentication-primary-application-flow.md)
   * [使用预选的mvpd在辅助应用程序中执行身份验证](rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [无需预选mvpd就可在辅助应用程序中执行身份验证](rest-api-v2-basic-authentication-secondary-application-flow.md)
* 当流应用程序需要注销MVPD时，它必须启动注销流程。

>[!IMPORTANT]
>
> 假设
>
> <br/>
> 
> * MVPD不支持注销流，并且没有注销端点。

### 工作流 {#workflow-initiate-logout-for-specific-mvpd-without-logout-endpoint}

按照给定的步骤实施特定MVPD的基本注销流程，而无需在主应用程序中执行注销端点，如下图所示。

![启动特定mvpd的注销，但不注销终结点](../../../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-initiate-logout-within-primary-application-for-specific-mvpd-without-logout-endpoint.png)

*启动特定mvpd的注销，但不注销终结点*

1. **启动Adobe Pass注销：**&#x200B;流应用程序通过调用Adobe Pass注销终结点来收集启动注销流所需的所有数据。

   >[!IMPORTANT]
   >
   > 有关以下内容的详细信息，请参阅特定mvpd ](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) API的[Initiate注销：
   >
   > * 所有&#x200B;_必需的_&#x200B;参数，如`serviceProvider`、`mvpd`和`redirectUrl`
   > * 所有&#x200B;_必需的_&#x200B;标头，如`Authorization`、`AP-Device-Identifier`
   > * 所有&#x200B;_可选_&#x200B;参数和标头

1. **查找常规配置文件：** Adobe Pass服务器根据收到的参数和标头标识有效的配置文件。

1. **删除常规配置文件：** Adobe Pass服务器删除标识的常规配置文件。

1. **指示下一个操作：** Adobe Pass注销终结点响应包含指导流应用程序执行下一个操作所需的数据：
   * 缺少`url`属性，因为MVPD不支持注销流。
   * `actionName`属性设置为“complete”。
   * `actionType`属性设置为“无”。

   >[!IMPORTANT]
   >
   > 有关注销响应中提供的信息的详细信息，请参阅特定mvpd ](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) API的[启动注销。
   > 
   > <br/>
   > 
   > Adobe Pass注销端点验证请求数据以确保满足基本条件：
   >
   > * _必需_&#x200B;参数和标头必须有效。
   > * 提供的`serviceProvider`和`mvpd`之间的集成必须处于活动状态。
   >
   > <br/>
   > 
   > 如果验证失败，将生成错误响应，提供附加信息以遵守[增强型错误代码](../../../../features-standard/error-reporting/enhanced-error-codes.md)文档。

1. **指示注销完成：**&#x200B;流式应用程序处理该响应，并可以使用它选择性地在用户界面上显示特定消息。
