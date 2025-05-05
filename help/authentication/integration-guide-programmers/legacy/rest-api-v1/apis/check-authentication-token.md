---
title: 检查身份验证令牌
description: 检查身份验证令牌
exl-id: 9020f261-44d8-4bd5-b85b-a8667679f563
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '292'
ht-degree: 0%

---

# （旧版）检查身份验证令牌 {#check-authentication-token}

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

指示设备是否具有未过期的身份验证令牌。

| 端点 | </br>调用者 | 输入   </br>参数 | HTTP </br>方法 | 响应 | HTTP </br>响应 |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/checkauthn | 流式处理应用程序</br></br>或</br></br>程序员服务 | 1.请求者（必需）</br>2。  deviceId （必需）</br>3。  device_info/X-Device-Info （必需）</br>4。  _deviceType_ </br>5。  _deviceUser_ （已弃用）</br>6。  _appId_（已弃用） | GET | 如果失败，则包含错误详细信息的XML或JSON。 | 200 — 成功   </br>403 — 未成功 |

{style="table-layout:auto"}


| 输入参数 | 描述 |
| --- | --- |
| 请求者 | 此操作有效的程序员requestorId。 |
| deviceId | 设备ID字节。 |
| 设备信息/</br></br>X — 设备信息 | 流设备信息。</br></br>**注意**：可以将此device_info作为URL参数传递，但由于此参数的潜在大小以及GETURL的长度限制，它应作为X-Device-Info传递到http标头。</br></br><!--See the full details in [Passing Device and Connection Information](/help/authentication/passing-client-information-device-connection-and-application.md)(/help/authentication/passing-client-information-device-connection-and-application.md)-->。 |
| _deviceType_ | 设备类型（例如Roku、PC）。</br></br>如果此参数设置正确，ESM提供的量度在使用无客户端程序时按设备类型[&#128279;](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type)进行划分，因此可以对Roku、AppleTV、Xbox等执行不同类型的分析。</br></br>有关详细信息，请参阅[在Adobe Pass身份验证量度中使用Clienless deviceType参数的好处&#x200B;](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br>**注意**： device_info将替换此参数。 |
| _设备用户_ | 设备用户标识符。 |
| _appId_ | 应用程序id/名称。</br>**注意**： device_info将替换此参数。 |

{style="table-layout:auto"}


## 响应（如果失败） {#response}

```JSON
    <error>
      <status>403</status>
      <message>Authentication token expired</message>
    </error>
```

### [返回REST API引用](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
