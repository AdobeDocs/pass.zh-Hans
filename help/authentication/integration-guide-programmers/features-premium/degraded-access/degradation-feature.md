---
title: 退化特征
description: 退化特征
exl-id: c7d6685b-a235-42eb-9c9c-0ffa1747f614
source-git-commit: 49a6a75944549dbfb062b1be8a053e6c99c90dc9
workflow-type: tm+mt
source-wordcount: '477'
ht-degree: 0%

---

# 退化特征 {#degradation-feature}

>[!IMPORTANT]
>
> 此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

在体育赛事和重大活动的动态世界中，确保观看者获得无缝体验至关重要。 在这些事件期间的高流量可能会吞噬多频道视频节目分发商(MVPD)身份验证和授权端点，从而导致延迟或中断。

Adobe Pass身份验证通过其&#x200B;**降级功能**&#x200B;解决这些挑战，该解决方案允许临时绕过特定的MVPD身份验证和授权端点。 在流量高峰期时，此功能尤其有用，因为高峰期的响应时间可能会因MVPD系统负载过重而缩短。

**降级功能**&#x200B;对程序员来说是一个重要的保障，可确保服务的连续性。 虽然其主要受众包括直播体育和新闻频道，但其实用程序适用于任何试图降低MVPD端点造成的中断风险的程序员。

>[!IMPORTANT]
>
> 降级API是一项高级功能，需要Adobe的当前许可证。

通过应用降级规则，程序员可以临时启用自动身份验证或自动授权，确保在应用降级期间对内容的访问不中断。 退化操作始终由程序员根据与MVPD预先安排的协议来启动。 虽然Adobe当前不会直接触发降级，但如果建立了服务级别协议(SLA)，则未来功能可能包括主动管理。

此功能旨在与[使用情况监控API](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md)结合使用，并且基于与MVPD的预协议，Adobe Pass身份验证提供了强大的工具，可在关键时刻平衡用户体验、可靠性和操作控制。

>[!IMPORTANT]
>
> 此功能不允许绕过Adobe Pass身份验证服务本身。 如果Adobe Pass身份验证不可用，则此服务中没有内置机制来方便用户访问。 在这种情况下，站点或应用程序可能会选择实施自己的替代路由解决方案来维护内容交付。

## 降级API访问 {#degradation-api-access}

在访问[降级API](#degradation-api)之前，您必须完成动态客户端注册(DCR)过程中的所需步骤。 此强制流程可确保您具有必需的访问令牌以与降级API交互。

有关全面说明，请参阅[动态客户端注册概述](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)文档。

## 降级API {#degradation-api}

降级API是一个RESTful API，它允许程序员管理特定MVPD的降级规则。 API提供了激活、删除以及检索处于活动状态的降级规则状态的方法。

要了解有关降级API的更多信息，请参阅以下Zendesk文档[Adobe Pass身份验证 | 降级API v3](https://tve.zendesk.com/hc/en-us/articles/33912526308372-Adobe-Pass-Authentication-Degradation-API-v3)并查找要下载的PDF文件。

## REST API V2 {#rest-api-v2}

要利用降级功能，需要实施代码更新以修改您随时随地播放电视(TVE)应用程序与Adobe Pass身份验证[REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)交互的方式。

有关这些更新和相关工作流的全面指南，请参阅[已降级访问流](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/degraded-access-flows/rest-api-v2-access-degraded-flows.md)文档。
