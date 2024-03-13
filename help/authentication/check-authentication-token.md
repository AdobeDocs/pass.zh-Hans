---
title: 检查身份验证令牌
description: 检查身份验证令牌
exl-id: 9020f261-44d8-4bd5-b85b-a8667679f563
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 0%

---

# 检查身份验证令牌 {#check-authentication-token}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

>[!NOTE]
>
> REST API实施受限制 [节流机构](/help/authentication/throttling-mechanism.md)

## REST API端点 {#clientless-endpoints}

&lt;reggie_fqdn>：

* 生产 —  [api.auth.adobe.com](http://api.auth.adobe.com/)
* 暂存 —  [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;sp_fqdn>：

* 生产 —  [api.auth.adobe.com](http://api.auth.adobe.com/)
* 暂存 —  [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## 描述 {#description}

指示设备是否具有未过期的身份验证令牌。

| 端点 | 已调用  </br>按 | 输入   </br>参数 | HTTP  </br>方法 | 响应 | HTTP  </br>响应 |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/checkauthn | 流应用程序</br></br>或</br></br>程序员服务 | 1.申请人（必填）</br>2.  deviceId（必需）</br>3.  device_info/X-Device-Info（必需）</br>4.  _设备类型_ </br>5.  _设备用户_ （已弃用）</br>6.  _appId_ （已弃用） | GET | 如果失败，则包含错误详细信息的XML或JSON。 | 200 — 成功   </br>403 — 未成功 |

{style="table-layout:auto"}


| 输入参数 | 描述 |
| --- | --- |
| 请求者 | 此操作有效的程序员requestorId。 |
| deviceId | 设备ID字节。 |
| device_info/</br></br>X-Device-Info | 流设备信息。</br></br>**注意**：可以将此device_info作为URL参数传递，但由于此参数可能的大小以及GETURL的长度限制，它应该作为X-Device-Info传递到http标头。 </br></br><!--See the full details in [Passing Device and Connection Information](http://tve.helpdocsonline.com/passing-device-information)(/help/authentication/passing-client-information-device-connection-and-application.md)-->. |
| _设备类型_ | 设备类型（例如Roku、PC）。</br></br>如果此参数设置正确，则ESM提供的量度 [按设备类型细分](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) 使用无客户端应用程序时，以便可以对Roku、AppleTV、Xbox等执行不同类型的分析。</br></br>有关详细信息，请参阅 [在Adobe Pass身份验证量度中使用无客户端deviceType参数的好处&#x200B;](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br>**注意**：device_info将替换此参数。 |
| _设备用户_ | 设备用户标识符。 |
| _appId_ | 应用程序id/名称。</br>**注意**：device_info将替换此参数。 |

{style="table-layout:auto"}


## 响应（如果失败） {#response}

```JSON
    <error>
      <status>403</status>
      <message>Authentication token expired</message>
    </error>
```

### [返回REST API参考](/help/authentication/rest-api-reference.md)
