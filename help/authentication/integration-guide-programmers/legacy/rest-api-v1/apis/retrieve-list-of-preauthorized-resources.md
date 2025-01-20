---
title: 检索预授权资源的列表
description: 检索预授权资源的列表
exl-id: 3821378c-bab5-4dc9-abd7-328df4b60cc3
source-git-commit: 1c357b918fa4f6d4b92a9055de018c55ee5861e0
workflow-type: tm+mt
source-wordcount: '390'
ht-degree: 0%

---

# （旧版）检索预授权资源的列表 {#retrieve-list-of-preauthorized-resources}

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

请求Adobe Pass身份验证以获取预授权资源的列表。

有两组API：一组用于流应用程序或程序员服务，另一组用于第二屏幕Web应用程序。 本页介绍适用于流应用程序或程序员服务的API。


| 端点 | </br>调用者 | 输入   </br>参数 | HTTP </br>方法 | 响应 | HTTP </br>响应 |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/预授权 | 流式处理应用程序</br></br>或</br></br>程序员服务 | 1.请求者（必需）</br>2。  deviceId （必需）</br>3。  资源（必需）</br>4。  device_info/X-Device-Info （必需）</br>5。  _deviceType_</br> 6。  _deviceUser_ （已弃用）</br>7。  _appId_（已弃用） | GET | 包含各个预授权决策或错误详细信息的XML或JSON。 请参阅下面的示例。 | 200 — 成功</br></br>400 — 错误请求</br></br>401 — 未授权</br></br>405 — 不允许的方法</br></br>412 — 前提条件失败</br></br>500 — 内部服务器错误 |


| 输入参数 | 描述 |
| --- | --- |
| 请求者 | 此操作有效的程序员requestorId。 |
| deviceId | 设备ID字节。 |
| 资源 | 一个字符串，其中包含以逗号分隔的resourceId列表，以标识用户可能可以访问并被MVPD授权端点识别的内容。 |
| 设备信息/</br></br>X — 设备信息 | 流设备信息。</br></br>**注意**：可以将此device_info作为URL参数传递，但由于此参数的潜在大小以及GETURL的长度限制，它应作为X-Device-Info传递到http标头。 </br></br>在[传递设备和连接信息](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md)中查看完整的详细信息。 |
| _deviceType_ | 设备类型（例如，Roku、PC）。</br></br>如果该参数设置正确，ESM提供的量度在使用无客户端程序时按设备类型](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type)进行[划分，以便可以执行不同类型的分析，例如Roku、AppleTV和Xbox。</br></br>查看[在传递量度中使用无客户端设备类型参数的好处&#x200B;](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**注意**： `device_info`将替换此参数。 |
| _设备用户_ | 设备用户标识符。 |
| _appId_ | 应用程序id/名称。 </br></br>**注意**： device_info将替换此参数。 |



### 示例响应 {#sample-response}



**XML：**

```XML
HTTP/1.1 200 OK
Adobe-Request-Id : 7af28ec2-a068-45c2-8009-f5443049baf4
Adobe-Response-Confidence : full
Content-Type: application/xml; charset=utf-8

<resources>
  <resource>
    <id>TestStream1</id>
    <authorized>true</authorized>
  </resource>
  <resource>
    <id>TestStream2</id>
    <authorized>false</authorized>
    <error>
      <status>403</status>
      <code>authorization_denied_by_mvpd</code>
      <message>User not authorized</message>
      <details>Your subscription package does not include the "TestStream3" channel.</details>
      <helpUrl>https://experienceleague-review.corp.adobe.com/docs/primetime/authentication/auth-features/error-reportn/enhanced-error-codes.html#error-codes</helpUrl>
      <trace>0453f8c8-167a-4429-8784-cd32cfeaee58</trace>
      <action>none</action>
    </error>
  </resource>
</resources>
```

</br>

**JSON：**

```JSON
HTTP/1.1 200 OK
Adobe-Request-Id : 7af28ec2-a068-45c2-8009-f5443049baf4
Adobe-Response-Confidence : full
Content-Type: application/json; charset=utf-8
 
{
   "resources" : [
        {
            "id" : "TestStream1",
            "authorized" : true
        },
        {
            "id" : "TestStream3",
            "authorized" : false,
            "error" : {
               "status" : 403,
               "code" : "authorization_denied_by_mvpd",
               "message" : "User not authorized",
               "details" : "Your subscription package does not include the "TestStream3" channel.",
               "helpUrl" : "https://experienceleague-review.corp.adobe.com/docs/primetime/authentication/auth-features/error-reportn/enhanced-error-codes.html#error-codes",
               "trace" : "0453f8c8-167a-4429-8784-cd32cfeaee58",
               "action" : "none"
            }
        } 
    ]
}
```
