---
title: 上报程序
description: 上报程序
exl-id: 1d754e5a-d5fa-4411-8932-2a36294da6eb
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '879'
ht-degree: 0%

---

# 上报程序 {#escalation-procedures}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

>[!IMPORTANT]
> 
>致电热线：**+1-205-693-9813**，并发送电子邮件至&#x200B;**tve-support@adobe.com**（主题行中包含&#x200B;**URGENT - INCIDENT**）。

## 简介 {#introduction}

本文档介绍了对影响Adobe Pass身份验证、Adobe Pass并发监控及其合作伙伴的重大事件（**严重级别1**&#x200B;级别）的支持过程。


## 严重级别1事件的定义 {#definition-of-a-severity-1-level-incident}

**严重性1**&#x200B;级别事件是&#x200B;**实时**&#x200B;情况，即&#x200B;**在生产环境中发生**，该情况不允许完成一个渠道和一个MVPD的身份验证和/或授权流，这会影响执行该流的MVPD的大量订阅者。


## 严重级别1事件的示例 {#examples-of-severity-1-incidentcs}

* 在`https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js`（或`https://entitlement.auth.adobe.com/entitlement/js/AccessEnabler.js`）托管的生产访问启用程序不可用。

* 对于特定的MVPD，在Adobe选择MVPD（在任何支持的浏览器中）后，用户不再重定向/显示登录页面。

* 合作伙伴会收到大量报告，指出用户无法通过特定MVPD进行身份验证/授权。

* 在身份验证过程中，用户卡在Adobe错误页面上，无法重新启动身份验证/授权流程。


| **NOT**&#x200B;严重级别为1的事件的示例 |
|---|
| 对于这些类型的问题，Adobe将为调查提供支持，但它们不是严重级别为1的事件：<ul><li>由于Flash版本问题(缺少Flash、Flash拦截器、Flash版本错误)，一个或几个订阅者无法执行流。</li><li>一个或多个订阅者无法进行身份验证并停留在MVPD登录页面上。</li><li>一个或几个订阅者已经过身份验证，但无法播放视频。</li><li>一个/少数/所有订阅者在程序员网站上遇到JavaScript错误</li></ul> |

## 严重级别1升级流程 {#severity-1-escalation-flows}

严重级别1事件可能由Adobe或Adobe Pass身份验证合作伙伴启动。 每个页面的步骤如下所示。

### 合作伙伴启动的流程 {#partner-initiated-flow}

1. 合作伙伴发现严重级别为1的事件（如上所述），需要Adobe立即关注。
1. 合作伙伴向&#x200B;**tve-support@adobe.com**&#x200B;发送一封电子邮件，主题行中包含&#x200B;**URGENT - INCIDENT**，并添加了以下信息：
   * 标题
   * 描述和重现步骤
   * 操作系统/浏览器
   * SDK和版本
   * 受影响的设备
   * %受影响的用户
   * 说明问题的HTTP跟踪或设备日志
   * （可选）任何可用于演示问题的屏幕截图或视频捕获
1. 如果Adobe在30分钟内未响应票证，则合作伙伴将调用以下号码：
   **1-205-693-9813**
   >[!IMPORTANT]
   >如果票证的标题中未包含“URGENT-INCIDENT”（紧急事件），我们的通知系统**将不会拾取该事件。

### Adobe启动的流 {#adobe-initiated-flow}

#### ...有关Adobe Pass身份验证问题 {#adobe-initiated-flow-authn-issue}

1. Adobe会识别内部问题，并通过我们的跟踪系统打开票证。

1. Adobe会通知合作伙伴的项目经理和技术联系人，具体说明票证编号和问题的预计影响。

1. Adobe致力于解决该事件，并随时向所有受影响的合作伙伴通报情况。

#### ...对于合作伙伴问题（程序员/MVPD） {#adobe-initiated-flow-partner-issue}

1. Adobe标识的问题与与MVPD的集成或程序员网站上的集成相关。

1. Adobe将按照与该合作伙伴</u>一起提供的支持过程通知受影响的合作伙伴<u>，并向该合作伙伴的支持组织打开一个票证。

1. 在影响分析过程中，如果Adobe确定问题属于预先商定的事件方案决策之一，请参阅&#x200B;**预先商定的事件方案决策**，它将采取相应的措施，而无需等待合作伙伴的输入。

1. 当服务恢复后，Adobe将等待合作伙伴的更新和合作伙伴的通知。

## 关于事件情景的预先商定决策 {#pre-agreed-descn}

在某些情况下，当场景出现时，将执行默认操作：

|   | 方案 | 描述 | 操作 |
|---|---|---|---|
| S1 | Adobe标识正常生产操作期间MVPD集成出现的问题。 | 在正常生产操作期间，Adobe识别一个MVPD的问题，该问题使得无法执行身份验证/授权流（例如，过期的证书、过期的SAML响应、端口关闭、改变的参数等） | -Adobe将通知受影响的MVPD和程序员。 </br> </br> -Adobe将为所有受影响的程序员停用此MVPD。</br> </br> -Adobe将使用MVPD按照与该MVPD商定的支持过程打开票证 |
| S2 | Adobe为程序员激活新的MVPD，而程序员在启动日期之前允许MVPD。 | Adobe正在为程序员站点激活新的MVPD，并且该站点已在选取器中显示新的MVPD，即使不应显示它。 | -Adobe将在预定日期之前通知程序员在选取器中出现新的MVPD。</br> </br> — 如有必要，程序员将采取措施将其从选取器中移除。 |
| S3 | Adobe为程序员激活新的MVPD，即使MVPD未准备好投入生产 | Adobe正在为程序员激活新的MVPD，但MVPD尚未部署对集成的支持，因此无法执行身份验证/授权流 |  — 仅当程序员</br>询问时，Adobe才会进行部署 </br> — 一旦执行了所有测试，程序员将负责确保MVPD的许可。 |

## 严重级别1事件的响应预期 {#response-expectations-for-severity-one-incidents}

* 初始响应：30分钟(24/7)
* 行动计划：1小时(24/7)
* 决议： ASAP (24/7)
