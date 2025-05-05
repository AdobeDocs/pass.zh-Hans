---
title: 获取短媒体令牌
description: 获取短媒体令牌
exl-id: 667eaaba-423e-4d54-9dbe-084b3c049e1f
source-git-commit: ae2e61152695b738b0bb08d1dcd81417f3bbdfb5
workflow-type: tm+mt
source-wordcount: '393'
ht-degree: 0%

---

# （旧版）获取短媒体令牌 {#obtain-short-media-token}

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

* 生产 — [`api.auth.adobe.com`](http://api.auth.adobe.com/)
* 正在暂存 — [`api.auth-staging.adobe.com`](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>：

* 生产 — [`api.auth.adobe.com`](http://api.auth.adobe.com/)
* 正在暂存 — [`api.auth-staging.adobe.com`](http://api.auth-staging.adobe.com/)

</br>

## 描述 {#description}

获取短媒体令牌。

| 端点 | </br>调用者 | 输入   </br>参数 | HTTP </br>方法 | 响应 | HTTP </br>响应 |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/mediatoken</br></br>或</br></br>&lt;SP_FQDN>/api/v1/tokens/media</br></br>例如：</br></br>&lt;SP_FQDN>/api/v1/tokens/media | 流式处理应用程序</br></br>或</br></br>程序员服务 | 1.请求者（必需）</br>2。  deviceId （必需）</br>3。  资源（必需）</br>4。  device_info/X-Device-Info （必需）</br>5。  _deviceType_</br> 6。  _deviceUser_ （已弃用）</br>7。  _appId_（已弃用） | GET | 包含Base64编码媒体令牌的XML或JSON，如果失败，则显示错误详细信息。 | 200 — 成功</br>403 — 无成功 |

{style="table-layout:auto"}

<!--
| Endpoint | Called  </br>By | Input   </br>Params | HTTP  </br>Method | Response | HTTP  </br>Response |
| --- | --- | --- | --- | --- | --- |
| `<SP_FQDN>/api/v1/mediatoken`</br></br>  or</br></br>`<SP_FQDN>/api/v1/tokens/media`</br></br>For example:</br></br>`<SP_FQDN>/api/v1/tokens/media` | Streaming App</br></br>or</br></br>Programmer Service | <ol><li>requestor (Mandatory)</l><li>deviceId (Mandatory)</li><li>resource (Mandatory)</li><li>device_info/X-Device-Info (Mandatory)</li><li>_deviceType_</li><li>_deviceUser_ (Deprecated)</li><li>_appId_ (Deprecated)</li></ol> | GET | XML or JSON containing an Base64 encoded media token or error details if unsuccessful. | 200 - Success  </br>403 - No Success |
-->

</br>

| 输入参数 | 描述 |
|-------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 请求者 | 此操作有效的程序员requestorId。 |
| deviceId | 设备ID字节。 |
| 资源 | 一个字符串，它包含resourceId（或MRSS片段），可标识用户请求的内容并由MVPD授权端点识别。 |
| 设备信息/</br></br>X — 设备信息 | 流设备信息。</br></br>**注意**：可以将此device_info作为URL参数传递，但由于此参数的潜在大小以及GETURL的长度限制，它应作为X-Device-Info传递到http标头。 </br></br>在[传递设备和连接信息](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md)中查看完整的详细信息。 |
| _deviceType_ | 设备类型（例如，Roku、PC）。</br></br>如果此参数设置正确，ESM会在使用无客户端程序时提供按设备类型&rbrack;/(help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type)划分的&lbrack;个量度，以便可以对其执行不同类型的分析。 例如，Roku、AppleTV和Xbox。</br></br>查看[使用无客户端设备类型参数的好处&#x200B;](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**注意**： device_info将替换此参数。 |
| _设备用户_ | 设备用户标识符。</br></br>**注意**：如果使用，则deviceUser的值应与[创建注册码](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md)请求中的值相同。 |
| _appId_ | 应用程序id/名称。 </br></br>**注意**： device_info将替换此参数。 如果使用，`appId`应具有与[创建注册码](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md)请求中相同的值。 |

{style="table-layout:auto"}

### 示例响应 {#response}

**XML：**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <play>
        <expires>1348649569000</expires>
        <mvpdId>sampleMvpdId</mvpdId>
        <requestor>sampleRequestorId</requestor>
        <resource>sampleResourceId</resource>
        <serializedToken>PHNpZ25hdH[...]</serializedToken>
        <userId>sampleScrambledUserId</userId>
    </play>
```



**JSON：**

```JSON
    {
        "resource": "sampleResourceId",
        "requestor": "sampleRequestorId",
        "expires": "1348649614000",
        "serializedToken": "PHNpZ25hdH[...]",
        "userId": "sampleScrambledUserId",
        "mvpdId": "sampleMvpdId"
    }
```



### 介质验证库兼容性

“获取短媒体令牌”调用中的字段`serializedToken`是一个Base64编码令牌，可以根据Adobe Medium验证库进行验证。
