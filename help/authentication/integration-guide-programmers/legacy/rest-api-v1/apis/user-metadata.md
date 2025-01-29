---
title: 用户元数据
description: 用户元数据
exl-id: 3d7b6429-972f-4ccb-80fd-a99870a02f65
source-git-commit: ae2e61152695b738b0bb08d1dcd81417f3bbdfb5
workflow-type: tm+mt
source-wordcount: '518'
ht-degree: 0%

---

# （旧版）用户元数据 {#user-metadata}

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

`<REGGIE_FQDN>`：

* 生产 — [api.auth.adobe.com](http://api.auth.adobe.com/)
* 暂存 — [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

`<SP_FQDN>`：

* 生产 — [api.auth.adobe.com](http://api.auth.adobe.com/)
* 暂存 — [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## 描述 {#description}

检索MVPD共享的有关经过身份验证的用户的元数据。


| 端点 | </br>调用者 | 输入   </br>参数 | HTTP </br>方法 | 响应 | HTTP </br>响应 |
| --- | --- | --- | --- | --- | --- |
| `<SP_FQDN>`/api/v1/tokens/usermetadata | 流式处理应用程序</br></br>或</br></br>程序员服务 | 1.请求者</br>2。  deviceId （必需）</br>3。  device_info/X-Device-Info （必需）</br>4。  deviceType</br>5。  deviceUser（已弃用）</br>6。  appId（已弃用） | GET | XML或JSON，其中包含用户元数据或错误详细信息（如果失败）。 | 200 — 成功<p>404 — 未找到元数据<p>412 — 无效的AuthN令牌（例如，过期的令牌） |


| 输入参数 | 描述 |
|------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 请求者 | 此操作有效的程序员requestorId。 |
| deviceId | 设备ID字节。 |
| device_info/<p>X-Device-Info | 流式传输设备信息。</br></br> **注意：**&#x200B;这可能作为URL参数传递device_info，但由于此参数的潜在大小以及GETURL的长度限制，它应作为X-Device-Info传递到http标头。 </br></br>在[传递设备和连接信息](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md)中查看完整的详细信息。 |
| _deviceType_ | 设备类型（例如Roku、PC）。</br></br>如果该参数设置正确，则在使用无客户端程序时，ESM提供的量度按设备类型](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#progr-filter-metrics)进行[划分，以便可以对Roku、AppleTV、Xbox等执行不同类型的分析。</br></br>查看[在Pass量度中使用无客户端设备类型参数的好处](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md) </br></br> **注意：**`device_info`将替换此参数。 |
| _设备用户_ | 设备用户标识符。</br></br> **注意：**&#x200B;如果使用，`deviceUser`应具有与[创建注册码](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md)请求中相同的值。 |
| _appId_ | 应用程序id/名称。</br></br> **注意：**`device_info`将替换此参数。 如果使用，`appId`应具有与[创建注册码](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md)请求中相同的值。 |

>[!NOTE]
> 
>用户元数据信息应在身份验证流程完成后可用，但可以在授权流程中更新，具体取决于MVPD和元数据类型。




## 示例响应 {#sample-response}

成功调用后，服务器将使用XML（默认）或JSON对象进行响应，该对象的结构与下面显示的结构类似：


```JSON
    {
        updated: 1334243471,
        encrypted: ["encryptedProp"],
        data: {
              zip: ["12345", "34567"],
              maxRating: { 
                  "MPAA": "PG-13",
                  "VCHIP": "TV-Y", 
                  "URL": "http://exam.pl/e/manage/ratings"
                         },
              householdID: "3456",
              userID: "BgSdasfsdk23/dsaf3+saASesadgfsShggssd=",
              channelID: ["channel-1", "channel-2"]
              }
    }
```

在对象的根目录下将有三个节点：

* *已更新*：指定表示上次更新元数据的UNIX时间戳。 此属性最初将由服务器在验证阶段生成元数据时设置。 后续调用（在元数据更新后）将导致时间戳递增。
* *数据*：包含实际的元数据值。
* *encrypted*：列出加密属性的数组。 要解密特定的元数据值，程序员必须对元数据执行Base64解码，然后使用它自己的私钥(Adobe使用程序员的公共证书加密服务器上的元数据)对结果值应用RSA解密。

如果出现错误，服务器将返回指定详细错误消息的XML或JSON对象。

有关详细信息，请参阅[用户元数据](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)。

[返回REST API参考](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
