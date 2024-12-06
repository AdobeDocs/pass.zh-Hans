---
title: 检索授权令牌
description: 检索授权令牌
exl-id: 0b010958-efa8-4dd9-b11b-5d10f51f5680
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '350'
ht-degree: 0%

---

# 检索授权令牌 {#retrieve-authorization-token}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

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

检索授权(AuthZ)令牌。


| 端点 | </br>调用者 | 输入   </br>参数 | HTTP </br>方法 | 响应 | HTTP </br>响应 |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/tokens/authz</br></br>例如：</br></br>&lt;SP_FQDN>/api/v1/tokens/authz | 流式处理应用程序</br></br>或</br></br>程序员服务 | 1.请求者（必需）</br>2。  deviceId （必需）</br>3。  资源（必需）</br>4。  device_info/X-Device-Info （必需）</br>5。  _deviceType_</br> 6。  _deviceUser_ （已弃用）</br>7。  _appId_（已弃用） | GET | 1.成功</br>2。  身份验证令牌</br>    未找到或已过期：   </br>    XML说明原因</br>    找不到authn令牌</br>3。  授权令牌</br>    未找到： </br>    XML说明</br>4。  授权令牌</br>    已过期：</br>    XML说明 | 200 — 成功</br>412 — 无身份验证</br></br>404 — 无身份验证</br></br>410 — 身份验证已过期 |

{style="table-layout:auto"}

</br>

| 输入参数 | 描述 |
| --- | --- |
| 请求者 | 此操作有效的程序员requestorId。 |
| deviceId | 设备ID字节。 |
| 资源 | 一个字符串，它包含resourceId（或MRSS片段），标识用户请求的内容并被MVPD授权端点识别。 |
| 设备信息/</br></br>X — 设备信息 | 流设备信息。</br></br>**注意**：可以将此device_info作为URL参数传递，但由于此参数的潜在大小以及GETURL的长度限制，它应作为X-Device-Info传递到http标头。 </br></br>在[传递设备和连接信息](/help/authentication/integration-guide-programmers/passing-client-information-device-connection-and-application.md)中查看完整的详细信息。 |
| _deviceType_ | 设备类型（例如，Roku、PC）。</br></br>如果该参数设置正确，ESM提供的量度在使用无客户端程序时按设备类型](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type)进行[划分，以便可以执行不同类型的分析，例如Roku、AppleTV和Xbox。</br></br>查看，[在传递量度中使用无客户端设备类型参数的好处&#x200B;](/help/authentication/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**注意**： device_info将替换此参数。 |
| _设备用户_ | 设备用户标识符。 |
| _appId_ | 应用程序id/名称。 </br></br>**注意**： device_info将替换此参数。 |

{style="table-layout:auto"}


### 示例响应 {#response}



#### 成功

**XML：**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <authorization>
        <expires>1348148289000</expires>
        <mvpd>sampleMvpdId</mvpd>
        <requestor>sampleRequestorId</requestor>
        <resource>sampleResourceId</resource>
        <proxyMvpd>sampleProxyMvpdId</proxyMvpd>
    </authorization>
```



**JSON：**

```JSON
    {
        "mvpd": "sampleMvpdId",
        "resource": "sampleResourceId",
        "requestor": "sampleRequestorId",
        "expires": "1348148289000",
        "proxyMvpd": "sampleProxyMvpdId"
    }
```

</br>


#### 未找到身份验证令牌或已过期：

**XML：**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <error>
        <status>412</status>
        <message>User not authenticated</message>
    </error>
```



**JSON：**

```JSON
    {
        "status": 412,
        "message": "User not authenticated",
        "details": null
    }
```

</br>


#### 未找到授权令牌：

**XML：**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <error>
        <status>404</status>
        <message>Not found</message>
    </error>
```



**JSON：**

```JSON
    {
        "status": 404,
        "message": "Not Found",
        "details": null
    }
```

</br>



#### 授权令牌已过期：

**XML：**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <error>
        <status>410</status>
        <message>Gone</message>
    </error>
```



**JSON：**

```JSON
    {
        "status": 410,
        "message": "Gone",
        "details": null
    }
```
