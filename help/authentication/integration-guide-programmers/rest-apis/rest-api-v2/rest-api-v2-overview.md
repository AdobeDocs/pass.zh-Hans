---
title: REST API V2概述
description: REST API V2概述
exl-id: a5595193-82c4-4033-bd98-596b4908b401
source-git-commit: f0001d86f595040f4be74f357c95bd2919dadf15
workflow-type: tm+mt
source-wordcount: '493'
ht-degree: 0%

---

# REST API V2概述 {#rest-api-v2-overview}

>[!IMPORTANT]
>
> 确保随时了解汇总在[产品公告](/help/authentication/product-announcements.md)页中的最新Adobe Pass身份验证产品公告和停用时间表。

您是否希望提高TVE应用程序的成本效益？

您是否希望减少在多个平台上支持TVE应用程序所需的开发时间和资源？

您是否希望确保跨平台提供一致的用户体验？

是否要减少维护工作并简化提供更新、错误修复或改进功能？

## REST API V2简介

Adobe Pass身份验证很高兴宣布REST API V2的推出，该版本旨在增强用户体验并简化与Pass服务的集成。

我们对于新REST API的前景感到振奋，这标志着我们的平台向前迈进了一大步，并为新功能和应用程序流敞开了大门。

## 新增功能

跨所有平台的独特实施

客户的应用程序现在可以跨平台使用相同的实施，从而更轻松地启动新功能或维护实时应用程序。

### 跨设备SSO

REST API V2允许在不同的设备之间安全地传递身份验证会话。 通过在设备之间简单地传递会话，用户可以在他们的移动设备上验证，并在与电视连接的设备上流式传输视频，而无需重新验证。

### 多个活动身份验证会话

现在可以使用其他活动MVPD会话，客户可以在需要时选择在TempPass和常规MVPD集成之间切换。

### 增强的安全机制

动态客户端注册是跨所有流程和功能使用的安全机制。 它允许对客户的应用程序进行更安全、更精细的控制，并可在所有平台上注册应用程序。

### 改进了性能，缩短了响应时间

增强的缓存机制允许减少发往MVPD的流量，缩短响应时间并减少延迟。 总的来说，在视频开始之前，API调用的数量会减少。

### 所有流上的增强错误代码

现在，所有传递流均以相同格式提供高级错误代码，并包含指导应用程序改善整体用户体验的更多详细信息。

### 改进了对所有身份验证会话的控制

新的REST API V2允许同时对多个经过身份验证的会话执行操作。

### 降低维护成本

所有响应和错误信息现在都已标准化。

## 接下来呢？

由于我们计划在2025年底之前继续提供支持，因此当前通过SDK或REST调用使用我们API的所有客户都可以继续这样做。

但是，所有未来开发都将基于REST API V2进行。 我们强烈建议启动迁移过程以从最新的Adobe Pass功能中获益。

## 想要了解更多信息？

要开始配置，请访问我们的公共文档：

- [清单](rest-api-v2-checklist.md)
- [术语表](rest-api-v2-glossary.md)
- [常见问题解答](rest-api-v2-faqs.md)
- [API](apis/rest-api-v2-apis-overview.md)
- [流](flows/rest-api-v2-flows-overview.md)
- 指南
- 附录
- [最低系统要求](/help/authentication/integration-guide-programmers/minimum-system-requirements.md)

我们专门的支持团队还可以帮助您解答任何可能需要的疑问或技术帮助。

## 想要尝试REST API V2吗？

您现在可以通过[Adobe Developer](https://developer.adobe.com/adobe-pass/)网站中的产品专用页面浏览REST API V2。
