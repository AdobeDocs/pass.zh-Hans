---
title: 启动授权
description: 启动授权
exl-id: 2f8a5499-e94f-40dd-9fb0-aac8e080de66
source-git-commit: c1f891fabd47954dc6cf76a575c3376ed0f5cd3d
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 0%

---

# （旧版）启动授权 {#initiate-authorization}

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

获取授权响应。

| 端点 | </br>调用者 | 输入   </br>参数 | HTTP </br>方法 | 响应 | HTTP </br>响应 |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/authorize | 流式处理应用程序</br></br>或</br></br>程序员服务 | 1.请求者（必需）</br>2。  deviceId （必需）</br>3。  资源（必需）</br>4。  device_info/X-Device-Info （必需）</br>5。  _deviceType_</br> 6。  _deviceUser_ （已弃用）</br>7。  _appId_ （已弃用）</br>8。  额外参数（可选） | GET | 包含授权详细信息或错误详细信息（如果未成功）的XML或JSON。 请参阅下面的示例。 | 200 — 成功</br>403 — 无成功 |

{style="table-layout:auto"}

</br>


| 输入参数 | 描述 |
| --- |--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 请求者 | 此操作有效的程序员requestorId。 |
| deviceId | 设备ID字节。 |
| 资源 | 一个字符串，它包含resourceId（或MRSS片段），可标识用户请求的内容并由MVPD授权端点识别。 |
| 设备信息/</br></br>X — 设备信息 | 流设备信息。</br></br>**注意**：可以将此device_info作为URL参数传递，但由于此参数的潜在大小以及GET URL的长度限制，它应作为X-Device-Info传递到http标头。 </br></br>在[传递设备和连接信息](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md)中查看完整的详细信息。 |
| _deviceType_ | 设备类型（例如Roku、PC）。</br></br>如果此参数设置正确，ESM提供的量度在使用无客户端程序时按设备类型[进行](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type)划分，因此可以对Roku、AppleTV、Xbox等执行不同类型的分析。</br></br>查看[传递量度中无客户端设备类型参数的好处&#x200B;](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**注意**： device_info将替换此参数。 |
| _设备用户_ | 设备用户标识符。 |
| _appId_ | 应用程序id/名称。 </br></br>**注意**： device_info将替换此参数。 |
| 额外参数 | 调用可能还包含可选参数，这些参数可启用其他功能，如：</br></br>* generic_data — 允许使用[促销临时传递](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#promotional-temp-pass)</br></br>示例： `generic_data=("email":"email@domain.com")` |

{style="table-layout:auto"}

>[!CAUTION]
>
>**流设备IP地址**</br>
>对于客户端到服务器实施，流设备IP地址将随此调用隐式发送。  对于服务器到服务器实施，如果由程序员服务而不是流设备进行&#x200B;**regcode**&#x200B;调用，则需要以下标头来传递流设备IP地址：</br></br>
>
>```
>X-Forwarded-For : <streaming\_device\_ip>
>```
>
>其中`<streaming\_device\_ip>`是流设备公共IP地址。</br></br>
>示例：</br>
>
>```
>POST /reggie/v1/{req_id}/regcode HTTP/1.1
>X-Forwarded-For:203.45.101.20
>```
>


### 示例响应 {#sample-response}

* **用例1：成功**
</br>
  * **XML：**
  </br>

    &grave;&grave;XML
    &lt;？xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot; standalone=&quot;yes&quot;？>
    &lt;authorization>
    &lt;expires>1348148289000&lt;/expires>
    &lt;mvpd>sampleMvpdId&lt;/mvpd>
    &lt;requestor>sampleRequestorId&lt;/requestor>
    &lt;resource>sampleResourceId&lt;/resource>
    &lt;/authorization>
    &grave;&#39;



* **JSON：**

  ```JSON
  {
    "mvpd": "sampleMvpdId",
    "resource": "sampleResourceId",
    "requestor": "sampleRequestorId",
    "expires": "1348148289000"
  }
  ```

>[!IMPORTANT]
>
>当响应来自代理MVPD时，它可能包含一个名为`proxyMvpd`的附加元素。



* **案例2：授权被拒绝**


  ```JSON
  <error>
    <status>403</status>
    <message>User not authorized</message>
    <details>Your subscription package does not include the "ASFAFD" channel.
    Please go to http://www.ca.ble/upgrade in order to upgrade your subscription.</details>
  </error>
  ```
