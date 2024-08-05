---
title: 单次注销 — 流量
description: REST API V2 — 单次注销 — 流量
source-git-commit: 4d1ce1301d6baf7309e8ee52c43b02403aa2fab9
workflow-type: tm+mt
source-wordcount: '566'
ht-degree: 0%

---


# 单个注销流程 {#single-logout-flow}

>[!NOTE]
>
> 此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## 为特定mvpd启动单一注销 {#initiate-single-logout-for-specific-mvpd}

### 先决条件 {#prerequisites-initiate-single-logout-for-specific-mvpd}

在启动特定MVPD的单次注销之前，请确保满足以下先决条件：

* 第二个流应用程序必须具有有效的单一登录配置文件，该配置文件已使用单一登录身份验证流之一为MVPD成功创建：
   * [使用平台身份通过单点登录执行身份验证](./rest-api-v2-single-sign-on-platform-identity-flows.md)
   * [使用服务令牌通过单点登录执行身份验证](./rest-api-v2-single-sign-on-service-token-flows.md)
* 第二个流应用程序在需要注销MVPD时必须启动单个注销流。

>[!IMPORTANT]
> 
> 假设
>
> <br/>
> 
> * 第一和第二流应用程序获得与`JWS`或`JWE`相同的唯一平台标识符有效负载或与`JWS`相同的唯一用户标识符有效负载。

### 工作流 {#workflow-initiate-single-logout-for-specific-mvpd}

执行给定步骤以实施特定MVPD的单个注销流，如下图所示。

![启动特定mvpd的单一注销](../../../assets/rest-api-v2/flows/single-sign-on-flows/rest-api-v2-initiate-single-logout-for-specific-mvpd-flow.png)

*启动特定mvpd的单一注销*

1. **启动Adobe Pass注销：**&#x200B;流应用程序通过调用Adobe Pass注销终结点来收集启动注销流所需的所有数据。

   有关以下内容的详细信息，请参阅特定mvpd ](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) API的[Initiate注销：
   * 所有&#x200B;_必需的_&#x200B;参数，如`serviceProvider`、`mvpd`和`redirectUrl`
   * 所有&#x200B;_必需的_&#x200B;标头，如`Authorization`、`AP-Device-Identifier`
   * 所有&#x200B;_可选_&#x200B;参数和标头

   >[!IMPORTANT]
   > 
   > 在发出请求之前，流应用程序必须确保它包含唯一平台标识符或唯一用户标识符的有效值。
   >
   > <br/>
   > 
   > 有关`Adobe-Subject-Token`标头的更多详细信息，请参阅[Adobe主题令牌](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md)文档。
   > 
   > <br/>
   > 
   > 有关`AD-Service-Token`标头的更多详细信息，请参阅[AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md)文档。

1. **查找常规和单点登录配置文件：** Adobe Pass服务器根据收到的参数和标头识别常规和单点登录的有效配置文件。

1. **删除常规和单点登录配置文件：** Adobe Pass服务器从Adobe Pass后端删除已识别的常规和单点登录配置文件。

1. **指示下一个操作：** Adobe Pass注销终结点响应包含指导流应用程序执行下一个操作所需的数据。

   有关注销响应中提供的信息的详细信息，请参阅特定mvpd ](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) API的[启动注销。

   >[!IMPORTANT]
   >
   > Adobe Pass注销端点验证请求数据以确保满足基本条件：
   >
   > * _必需_&#x200B;参数和标头必须有效。
   > * 提供的`serviceProvider`和`mvpd`之间的集成必须处于活动状态。
   >
   > <br/>
   > 
   > 如果验证失败，将生成错误响应，提供附加信息以遵守[增强型错误代码](../../../enhanced-error-codes.md)文档。

1. **指示注销完成：**&#x200B;如果MVPD不支持注销流程，则流应用程序将处理该响应，并可以使用它选择性地在用户界面上显示特定消息。

1. **启动MVPD注销：**&#x200B;如果MVPD支持注销流程，则流应用程序将处理响应并使用用户代理启动与MVPD的注销流程。 该流可能包括到MVPD系统的多个重定向。 但是，结果是MVPD执行其内部清理，并将最终注销确认发送回Adobe Pass后端。

1. **指示注销完成：**&#x200B;流应用程序可以等待用户代理到达提供的`redirectUrl`，并可以将它用作信号，以选择在用户界面上显示特定消息。

>[!NOTE]
>
> 如果是从第一个流应用程序启动的，则单个注销流的步骤与上述步骤相同。
