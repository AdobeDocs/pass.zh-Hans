---
title: 检索身份验证令牌
description: 检索身份验证令牌
exl-id: 7fb03854-edad-41e7-b218-1858fc071876
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '264'
ht-degree: 0%

---

# 检索身份验证令牌 {#retrieve-authentication-token}

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

检索身份验证(AuthN)标记。

| 端点 | 已调用  </br>按 | 输入   </br>参数 | HTTP  </br>方法 | 响应 | HTTP  </br>响应 |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/tokens/authn</br></br>例如：</br></br>&lt;sp_fqdn>/api/v1/tokens/authn | 流应用程序</br></br>或</br></br>程序员服务 | 1.申请人（必填）</br>2.  deviceId（必需）</br>3.  device_info/X-Device-Info（必需）</br>4.  _设备类型_ （已弃用）</br>5.  _设备用户_ （已弃用）</br>6.  _appId_ （已弃用） | GET | XML或JSON，其中包含身份验证信息或错误详细信息（如果未成功）。 | 200 — 成功。  </br>404 — 未找到令牌  </br>410 — 令牌已过期 |

{style="table-layout:auto"}


| 输入参数 | 描述 |
| --- | --- |
| 请求者 | 此操作有效的程序员requestorId。 |
| deviceId | 设备ID字节。 |
| device_info/</br></br>X-Device-Info | 流设备信息。</br></br>**注意**：可以将此device_info作为URL参数进行传递，但由于此参数的潜在大小以及GETURL长度的限制，它应当作为X-Device-Info在http标头中传递。 </br></br><!--See the full details in [Passing Device and Connection Information](http://tve.helpdocsonline.com/passing-device-information)-->. |
| _设备类型_ | 设备类型（例如，Roku、PC）。</br></br>**注意**：device_info将替换此参数。 |
| _设备用户_ | 设备用户标识符。</br></br>**注意**：如果使用，则deviceUser的值应当与 [创建注册码](/help/authentication/registration-code-request.md) 请求。 |
| _appId_ | 应用程序id/名称。 </br></br>**注意**：device_info将替换此参数。 如果使用， `appId` 应具有与中的相同的值 [创建注册码](/help/authentication/registration-code-request.md) 请求。 |

{style="table-layout:auto"}

</br>

### 示例响应 {#response}



#### 成功

**XML：**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <authentication>
         <expires>1601114932000</expires>
         <userId>sampleUserId</userId>
         <mvpd>sampleMvpdId</mvpd>
         <requestor>sampleRequestor</requestor>
    </authentication>
```


**JSON：**

```JSON
    {
         "requestor": "sampleRequestor",
         "mvpd": "sampleMvpdId",
         "userId": "sampleUserId",
         "expires": "1601114932000"
    }
```





#### 未找到身份验证令牌：

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
        "message": "Not Found"
    }
```
