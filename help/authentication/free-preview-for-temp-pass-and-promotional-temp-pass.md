---
title: 临时通行证和促销临时通行证的免费预览
description: 临时通行证和促销临时通行证的免费预览
exl-id: c584bf0c-15c4-4a4d-b6a2-8d15ee786fe3
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '406'
ht-degree: 1%

---

# 临时通行证和促销临时通行证的免费预览 {#free-preview-for-temp-pass-and-promotional-temp-pass}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## REST API端点 {#clientless-endpoints}

&lt;reggie_fqdn>：

* 生产 —  [api.auth.adobe.com](http://api.auth.adobe.com/)
* 暂存 —  [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;sp_fqdn>：

* 生产 —  [api.auth.adobe.com](http://api.auth.adobe.com/)
* 暂存 —  [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## 描述 {#description}

允许为Temp Pass和Promotional Temp Pass创建身份验证令牌，而无需第二个屏幕。


| 端点 | 已调用  </br>按 | 输入   </br>参数 | HTTP  </br>方法 | 响应 | HTTP  </br>响应 |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/authenticate/freepreview | 流应用程序</br></br>或</br></br>程序员服务 | 1. requestor_id（必需）</br>    </br>2.  deviceId（必需）</br>    </br>3.  mso_id（必需）</br>    </br>4.  domain_name（必需）</br>    </br>5.  device_info/X-Device-Info（必需）</br>6.  设备类型</br>    </br>7.  deviceUser（已弃用）</br>    </br>8.  appId（已弃用）</br>    </br>9.  generic_data（可选） | POST | 成功的响应将为“204无内容”，这表示已成功创建令牌并准备好用于授权流。 | 204 — 无内容   </br>400 — 错误请求 |

<div>


| 输入参数 | 描述 |
| --- | --- |
| requestor_id | 此操作有效的程序员requestorId。 |
| deviceId | 设备ID字节。 |
| mso_id | 此操作有效的MVPD ID。 |
| 域名 | 将向其授予令牌的域名。 正在向授权令牌授予时与服务提供商的域进行比较。 |
| device_info/</br></br>X-Device-Info | 流设备信息。</br></br>**注意**：可以将此device_info作为URL参数进行传递，但由于此参数的潜在大小以及GETURL长度的限制，它应当作为X-Device-Info在http标头中传递。 </br></br><!--See the full details in [Passing Device and Connection Information](http://tve.helpdocsonline.com/passing-device-information)-->. |
| _设备类型_ | 设备类型（例如Roku、PC）。</br></br>如果此参数设置正确，则ESM提供的量度 [按设备类型细分](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) 使用无客户端应用程序时，以便可以对Roku、AppleTV、Xbox等执行不同类型的分析。</br></br>请参阅 [使用无客户端设备类型参数的好处&#x200B;](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**注意**：device_info将替换此参数。 |
| _设备用户_ | 设备用户标识符。</br></br>**注意**：如果使用，则deviceUser的值应当与 [创建注册码](/help/authentication/registration-code-request.md) 请求。 |
| _appId_ | 应用程序id/名称。 </br></br>**注意**：device_info将替换此参数。 如果使用， `appId` 应具有与中的相同的值 [创建注册码](/help/authentication/registration-code-request.md) 请求。 |
| generic_data | 用于限制提升临时传递的令牌范围。 |


### [返回REST API参考](/help/authentication/rest-api-reference.md)
