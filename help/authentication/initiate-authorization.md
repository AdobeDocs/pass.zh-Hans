---
title: 启动授权
description: 启动授权
exl-id: 2f8a5499-e94f-40dd-9fb0-aac8e080de66
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '402'
ht-degree: 0%

---

# 启动授权 {#initiate-authorization}

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

获取授权响应。

| 端点 | 已调用  </br>按 | 输入   </br>参数 | HTTP  </br>方法 | 响应 | HTTP  </br>响应 |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/authorize | 流应用程序</br></br>或</br></br>程序员服务 | 1.申请人（必填）</br>2.  deviceId（必需）</br>3.  资源（必需）</br>4.  device_info/X-Device-Info（必需）</br>5.  _设备类型_</br> 6.  _设备用户_ （已弃用）</br>7.  _appId_ （已弃用）</br>8.  额外参数（可选） | GET | 包含授权详细信息或错误详细信息（如果未成功）的XML或JSON。 请参阅下面的示例。 | 200 — 成功  </br>403 — 未成功 |

{style="table-layout:auto"}

</br>


| 输入参数 | 描述 |
| --- | --- |
| 请求者 | 此操作有效的程序员requestorId。 |
| deviceId | 设备ID字节。 |
| 资源 | 一个字符串，它包含resourceId（或MRSS片段），标识用户请求的内容并被MVPD授权端点识别。 |
| device_info/</br></br>X-Device-Info | 流设备信息。</br></br>**注意**：可以将此device_info作为URL参数进行传递，但由于此参数的潜在大小以及GETURL长度的限制，它应当作为X-Device-Info在http标头中传递。 </br></br><!--See the full details in [Passing Device and Connection Information](http://tve.helpdocsonline.com/passing-device-information)-->. |
| _设备类型_ | 设备类型（例如Roku、PC）。</br></br>如果此参数设置正确，则ESM提供的量度 [按设备类型细分](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) 使用无客户端应用程序时，以便可以对Roku、AppleTV、Xbox等执行不同类型的分析。</br></br>请参阅 [通行量度中无客户端设备类型参数的好处&#x200B;](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**注意**：device_info将替换此参数。 |
| _设备用户_ | 设备用户标识符。 |
| _appId_ | 应用程序id/名称。 </br></br>**注意**：device_info将替换此参数。 |
| 额外参数 | 调用可能还包含可选参数，用于启用其他功能，例如：</br></br>* generic_data — 允许使用 [促销临时传递](/help/authentication/promotional-temp-pass.md)</br></br>示例： `generic_data=("email":"email@domain.com")` |

{style="table-layout:auto"}

>[!CAUTION]
>
>**流设备IP地址**</br>
>对于客户端到服务器实施，流设备IP地址将随此调用隐式发送。  对于服务器到服务器实施，其中 **regcode** 调用是由程序员服务而不是流设备发起的，以下标头需要传递流设备IP地址：</br></br>
>
>```
>X-Forwarded-For : <streaming\_device\_ip>
>```
>
>位置 `<streaming\_device\_ip>` 是流设备公共IP地址。</br></br>
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

    ```XML
    &lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot; standalone=&quot;yes&quot;?>
    &lt;authorization>
    &lt;expires>1348148289000&lt;/expires>
    &lt;mvpd>sampleMvpdId&lt;/mvpd>
    &lt;requestor>sampleRequestorId&lt;/requestor>
    &lt;resource>sampleResourceId&lt;/resource>
    &lt;/authorization>
    ```



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
>当响应来自代理MVPD时，它可能包含一个名为的附加元素 `proxyMvpd`.



* **案例2：授权被拒绝**


  ```JSON
  <error>
    <status>403</status>
    <message>User not authorized</message>
    <details>Your subscription package does not include the "ASFAFD" channel.
    Please go to http://www.ca.ble/upgrade in order to upgrade your subscription.</details>
  </error>
  ```
