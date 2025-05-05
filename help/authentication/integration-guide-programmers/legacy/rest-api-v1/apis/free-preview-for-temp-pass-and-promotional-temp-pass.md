---
title: 临时通行证和促销临时通行证的免费预览
description: 临时通行证和促销临时通行证的免费预览
exl-id: c584bf0c-15c4-4a4d-b6a2-8d15ee786fe3
source-git-commit: ae2e61152695b738b0bb08d1dcd81417f3bbdfb5
workflow-type: tm+mt
source-wordcount: '432'
ht-degree: 0%

---

# （旧版）临时通行证和促销临时通行证的免费预览 {#free-preview-for-temp-pass-and-promotional-temp-pass}

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

允许为Temp Pass和Promotional Temp Pass创建身份验证令牌，而无需第二个屏幕。


| 端点 | </br>调用者 | 输入   </br>参数 | HTTP </br>方法 | 响应 | HTTP </br>响应 |
|-------------------------------------------|-------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------|
| &lt;SP_FQDN>/api/v1/authenticate/freepreview | 流式处理应用程序</br></br>或</br></br>程序员服务 | 1. requestor_id（必需）</br>    </br>2。  deviceId （必需）</br>    </br>3。  mso_id （必需）</br>    </br>4。  domain_name （必需）</br>    </br>5。  device_info/X-Device-Info （必需）</br>6。  deviceType</br>    </br>7。  deviceUser（已弃用）</br>    </br>8。  appId（已弃用）</br>    </br>9。  generic_data（可选） | POST | 成功的响应将为“204无内容”，这表示已成功创建令牌并准备好用于授权流。 | 204 — 无内容   </br>400 — 错误请求 |

<div>


| 输入参数 | 描述 |
|-------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| requestor_id | 此操作有效的程序员requestorId。 |
| deviceId | 设备ID字节。 |
| mso_id | 此操作对其有效的MVPD ID。 |
| 域名 | 将向其授予令牌的域名。 正在向授权令牌授予时与服务提供商的域进行比较。 |
| 设备信息/</br></br>X — 设备信息 | 流设备信息。</br></br>**注意**：可以将此device_info作为URL参数传递，但由于此参数的潜在大小以及GETURL的长度限制，它应作为X-Device-Info传递到http标头。 </br></br>在[传递设备和连接信息](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md)中查看完整的详细信息。 |
| _deviceType_ | 设备类型（例如Roku、PC）。</br></br>如果此参数设置正确，ESM提供的量度在使用无客户端程序时按设备类型[&#128279;](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type)进行划分，因此可以对Roku、AppleTV、Xbox等执行不同类型的分析。</br></br>查看[使用无客户端设备类型参数的好处&#x200B;](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**注意**： device_info将替换此参数。 |
| _设备用户_ | 设备用户标识符。</br></br>**注意**：如果使用，则deviceUser的值应与[创建注册码](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md)请求中的值相同。 |
| _appId_ | 应用程序id/名称。 </br></br>**注意**： device_info将替换此参数。 如果使用，`appId`应具有与[创建注册码](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md)请求中相同的值。 |
| generic_data | 用于限制提升临时传递的令牌范围。 |


### [返回REST API引用](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
