---
title: 启动注销
description: 启动注销
exl-id: 9625b5a2-31d9-4e20-8703-4a9e4eeb1618
source-git-commit: ae2e61152695b738b0bb08d1dcd81417f3bbdfb5
workflow-type: tm+mt
source-wordcount: '337'
ht-degree: 0%

---

# （旧版）启动注销 {#initiate-logout}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

>[!IMPORTANT]
>
> 确保随时了解汇总在[产品公告](/help/authentication/product-announcements.md)页中的最新Adobe Pass身份验证产品公告和停用时间表。

>[!NOTE]
>
> REST API实现受[限制机制](/help/authentication/integration-guide-programmers/throttling-mechanism.md)限制

## REST API端点 {#clientless-endpoints}

&lt;REGGIE_FQDN>：

* 生产 — [api.auth.adobe.com](http://api.auth.adobe.com/)
* 暂存 — [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>：

* 生产 — [api.auth.adobe.com](http://api.auth.adobe.com/)
* 暂存 — [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## 描述 {#description}

从存储中删除AuthN和AuthZ标记。


| 端点 | </br>调用者 | 输入   </br>参数 | HTTP </br>方法 | 响应 | HTTP </br>响应 |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/logout | 流式处理应用程序</br></br>或</br></br>程序员服务 | 1.请求者</br>2。  deviceId （必需）</br>3。  device_info/X-Device-Info （必需）</br>4。  _deviceType_</br> 5。  _deviceUser_ （已弃用）</br>6。  _appId_（已弃用） | DELETE | 无 | 204 |


| 输入参数 | 描述 |
|-------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 请求者 | 此操作有效的程序员requestorId。 |
| deviceId | 设备ID字节。 |
| 设备信息/</br></br>X — 设备信息 | 流设备信息。</br></br>**注意**：可以将此device_info作为URL参数传递，但由于此参数的潜在大小以及GET URL的长度限制，它应作为X-Device-Info传递到http标头。 </br></br>在[传递设备和连接信息](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md)中查看完整的详细信息。 |
| _deviceType_ | 设备类型（例如Roku、PC）。</br></br>如果此参数设置正确，ESM提供的量度在使用无客户端程序时按设备类型[进行](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type)划分，因此可以对Roku、AppleTV、Xbox等执行不同类型的分析。</br></br>查看[在传递量度中使用无客户端设备类型参数的好处&#x200B;](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**注意**： device_info将替换此参数。 |
| _设备用户_ | 设备用户标识符。</br></br>**注意**：如果使用，则deviceUser的值应与[创建注册码](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md)请求中的值相同。 |
| _appId_ | 应用程序id/名称。 </br></br>**注意**： device_info将替换此参数。 如果使用，`appId`应具有与[创建注册码](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md)请求中相同的值。 |

>[!IMPORTANT]
> 
>注销调用当前具有以下限制：它从存储(即程序员/Adobe Pass身份验证端)中清除AuthN和AuthZ令牌，但&#x200B;**不**&#x200B;调用MVPD注销端点。
