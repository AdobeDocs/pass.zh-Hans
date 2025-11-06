---
title: 在Adobe Pass身份验证量度中使用无客户端deviceType参数的好处
description: 在Adobe Pass身份验证量度中使用无客户端deviceType参数的好处
exl-id: a5004887-d5fa-468e-971b-10806519175b
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '372'
ht-degree: 0%

---

# （旧版）在Adobe Pass身份验证量度中使用无客户端deviceType参数的好处 {#benefits-of-using-the-clientless-devicetype-parameter-in-primetime-authentication-metrics}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

>[!IMPORTANT]
>
> 确保随时了解汇总在[产品公告](/help/authentication/product-announcements.md)页中的最新Adobe Pass身份验证产品公告和停用时间表。

</br>

## 上下文

尽管可选，但无客户端API中的参数`deviceType`（如果存在）用于通过[授权服务监控](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md)公开的Adobe Pass身份验证指标。

考虑到`deviceType`参数及其&#x200B;**好处**&#x200B;在Adobe Pass身份验证量度上的连接最初没有声明，本技术说明的范围是添加关于它们的更多信息。

## 说明

自第一个版本以来，`deviceType`参数一直存在于无客户端API中，但更新版本中添加了它对Adobe Pass身份验证量度的影响。



>[!IMPORTANT]
>
>如果参数`deviceType`设置正确，则在授权服务监控中具有以下&#x200B;**权益**：它提供了在使用无客户端程序时按设备类型[细分的指标](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type)，以便可以对Roku、AppleTV、Xbox等执行不同类型的分析。


有关授权服务监控API的更多信息，请参阅[深入分析树](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-api.md#drill-down_tree)，该树说明了ESM 2.0中可用的[维度](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#esm_dimensions)（资源）。

>[!NOTE]
>
>此技术说明的内容也已添加到[无客户端API](#clientless_device_type)。




## 实现

为了充分利用Adobe Pass身份验证指标，当前使用了两种类型的[无客户端API](#web_srvs_summary)，它们需要设置正确的`deviceType`：

1. 将`regcode`作为必需参数的API将使用在创建`deviceType`时设置的`regcode`参数，并具有以下API调用：
   - [\&lt;REGGIE\_FQDN\>/reggie/v1/](#reg_serv)

1. 将`deviceType`作为可选参数的API：
   - [\&lt;SP\_FQDN\>/api/v1/checkauthn](#check_authn_token)
   - [&lt;span class=&quot;s1&quot;>](#retrieve_authn_token)
   - [\&lt;SP\_FQDN\>/api/v1/authorize](#init_authz)
   - [\&lt;SP\_FQDN\>/api/v1/tokens/authz](#retrieve_authz_token)
   - [\&lt;SP\_FQDN\>/api/v1/tokens/media](#short_media)
   - [\&lt;SP\_FQDN\>/api/v1/mediatoken](#short_media)
   - [\&lt;SP\_FQDN\>/api/v1/preauthorize](#PreAuthZ_Resources)
   - [\&lt;SP\_FQDN\>/api/v1/logout](#init_logout)

我们建议使用`deviceType`参数并为所有API传递正确的无客户端设备类型。
