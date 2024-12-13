---
title: 印前检查功能，如何启用、诊断或确定问题
description: 印前检查功能，如何启用、诊断或确定问题
exl-id: 9e4ec343-371f-4116-915f-191e5f42cb47
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '499'
ht-degree: 0%

---

# （旧版）印前检查功能：如何启用、排除故障或确定问题 {#preflight-feature}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

Adobe Pass身份验证计算preAuthorizeResources的方式已更改。 PreAuthorization API具有新实施。 此实施取代了仅包含进行多个授权调用的旧解决方案。
PreAuthorization API的外部接口保持不变，程序员的应用程序不需要更新。

有三种方式计算Preflight资源：

* **向MVPD创建分支并加入方法**：这涉及Adobe向MVPD发起多个授权调用（客户端仍需发起一个预检调用）。
* **渠道组合**： MVPD在SAML身份验证响应中公开登录用户的渠道组合，并且Adobe根据该渠道组合返回授权资源。 SAML跟踪器中的SAML authN响应应公开该列表。
* **多渠道授权**：客户端和Adobe身份验证均对一组资源的MVPD进行单个调用。

不论MVPD如何，客户端应用程序都将对Preflight端点(checkPreauthorizedResources API)进行单个调用，从而传递一组资源ID。 根据MVPD支持的上述方法之一，Adobe将返回预授权的资源ID。

如果Preflight基于fork &amp; join方法，则Adobe Pass身份验证后端会检查其配置中为“最大预授权调用”设置的值。 这由Adobe配置。

“max preauthorization calls”配置的默认值为“5”，这意味着最多只能在Preflight中为分支和连接MVPD发送5个资源。 传递超过5个资源将导致异常，并返回空列表。 这是预期行为。 如果MVPD不支持渠道排列或多渠道授权，我们可以将其配置为任何值，但只有在将其咨询为多个分支和连接授权调用会增加其加载时间之后，我们才能将其配置为任何值。

因此，在为MVPD启用Preflight/排除故障时，需要注意以下事项：

* MVPD支持的方法（分支和联接、渠道排列或多渠道）。
* 如果仅支持分支和连接，则需要询问程序员将在Preflight调用中发送多少资源ID。
* 需要咨询MVPD，并需要了解发出“n”个分支和加入授权调用的影响。 之后，如果该值大于5，则必须在config中进行配置。

**限制**

请注意，对于某些MVPD（如AT&amp;T和TWC），如果其中任何resourceID是在预检调用中发送的resourceID列表中的伪ID或无法识别的ID，则我们不会从Preflight调用中获取任何resourceID，即使我们在该列表中也具有有效且经过授权的资源。
