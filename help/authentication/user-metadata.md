---
title: 用户元数据
description: 用户元数据
exl-id: 3d7b6429-972f-4ccb-80fd-a99870a02f65
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '485'
ht-degree: 0%

---

# 用户元数据 {#user-metadata}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## REST API端点 {#clientless-endpoints}

`<REGGIE_FQDN>`:

* 生产 —  [api.auth.adobe.com](http://api.auth.adobe.com/)
* 暂存 —  [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

`<SP_FQDN>`:

* 生产 —  [api.auth.adobe.com](http://api.auth.adobe.com/)
* 暂存 —  [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## 描述 {#description}

检索MVPD共享的有关经过身份验证的用户的元数据。


| 端点 | 已调用  </br>按 | 输入   </br>参数 | HTTP  </br>方法 | 响应 | HTTP  </br>响应 |
| --- | --- | --- | --- | --- | --- |
| `<SP_FQDN>`/api/v1/tokens/usermetadata | 流应用程序</br></br>或</br></br>程序员服务 | 1.请求人</br>2.  deviceId（必需）</br>3.  device_info/X-Device-Info（必需）</br>4.  设备类型</br>5.  deviceUser（已弃用）</br>6.  appId（已弃用） | GET | XML或JSON，其中包含用户元数据或错误详细信息（如果失败）。 | 200 — 成功<p>404 — 未找到元数据<p>412 — 无效的AuthN令牌（例如，过期的令牌） |


| 输入参数 | 描述 |
| --- | --- |
| 请求者 | 此操作有效的程序员requestorId。 |
| deviceId | 设备ID字节。 |
| device_info/<p>X-Device-Info | 流设备信息。</br></br> **注意：** 此URL可以作为URL参数传递，但由于此参数可能的大小以及GETURL的长度限制，它应该作为X-Device-Info传递到http标头。 </br></br> 欲知详情，请参阅 [传递设备和连接信息](/help/authentication/passing-client-information-device-connection-and-application.md). |
| _设备类型_ | 设备类型（例如Roku、PC）。</br></br> 如果此参数设置正确，则ESM提供的量度 [按设备类型细分](/help/authentication/entitlement-service-monitoring-overview.md#progr-filter-metrics) 使用无客户端应用程序时，以便可以对Roku、AppleTV、Xbox等执行不同类型的分析。</br></br> 请参阅 [在Pass量度中使用无客户端设备类型参数的好处](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md) </br></br> **注意：** 此 `device_info` 替换此参数。 |
| _设备用户_ | 设备用户标识符。</br></br> **注意：** 如果使用， `deviceUser` 应具有与中的相同的值 [创建注册码](/help/authentication/registration-code-request.md) 请求。 |
| _appId_ | 应用程序id/名称。 </br></br> **注意：** 此 `device_info` 替换此参数。 如果使用， `appId` 应具有与中的相同的值 [创建注册码](/help/authentication/registration-code-request.md) 请求。 |

>[!NOTE]
> 
>用户元数据信息应在身份验证流程完成后可用，但可以在授权流程上更新，具体取决于MVPD和元数据类型。




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

* *已更新*：指定一个UNIX时间戳，该时间戳表示上次更新元数据的时间。 此属性最初将由服务器在验证阶段生成元数据时设置。 后续调用（在元数据更新后）将导致时间戳递增。
* *数据*：包含实际的元数据值。
* *已加密*：列出加密属性的数组。 要解密特定的元数据值，程序员必须对元数据执行Base64解码，然后使用它自己的私钥(Adobe使用程序员的公共证书加密服务器上的元数据)对结果值应用RSA解密。

如果出现错误，服务器将返回指定详细错误消息的XML或JSON对象。

有关更多信息，请参阅 [用户元数据](/help/authentication/user-metadata-feature.md).

[返回REST API参考](/help/authentication/rest-api-reference.md)
