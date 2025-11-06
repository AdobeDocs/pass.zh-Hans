---
title: 如何提出隐私请求
description: 如何提出隐私请求
exl-id: abb21306-98d6-4899-914a-bdfa85cbd204
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '558'
ht-degree: 0%

---

# 如何提出隐私请求 {#howto-make-privacy-request}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## 标识符和命名空间 {#identifier-namespace}

在发送访问或删除隐私请求时，客户应用程序需要包含以下标识符：

* **mvpdID** - MVPD的唯一标识符。
* **userID** — 唯一标识程序员应用程序的用户，但源自MVPD。 请参阅程序员概述中的了解用户ID 。
* **IMSOrgID** - Adobe Experience Cloud Identity Management服务组织ID，用于在Adobe Experience Cloud中唯一标识客户


请检查下面的示例：

```JSON
"userIDs": [{
     "namespace":"http://www.adobe.com/primetimeAuthentication/Dish",  -----> "Dish" is the id of the MVPD
     "type":"unregistered",
     "value":"1234-5678-8765-4321" ----> "1234-5678-8765-4321" is the userId associated with the MVPD account
}]
```

>[!IMPORTANT]
>
>用户必须经过身份验证，才能为Adobe Pass身份验证生成隐私请求。 否则，程序员必须找到提取MVPD userID的其他方法。

## 请求类型 {#req-type}

Adobe Pass身份验证支持访问和删除请求。

### 访问 {#access-req}

对于访问请求：

我们将提供一个JSON文件，其中包含为该数据主体创建的身份验证和授权请求总数的摘要。
所有这些事件都是按客户筛选的。


**请求示例**

您必须上载包含要为其提交数据访问请求的Adobe Pass身份验证标识符的JSON。 要查看格式正确的JSON的外观，请参阅以下示例：

```JSON
{
    "companyContexts": [{
            "namespace": "imsOrgID",
            "value": "1234567890@AdobeOrg"
        }
    ],
    "users": [{
            "key": "John Dow",
            "action": ["access"],
            "userIDs": [{
                "namespace":"http://www.adobe.com/primetimeAuthentication/Dish",
                "type":"unregistered",
                "value":"1234-5678-8765-4321"
             }]
         
        }
    ],
    
    "include":["primetimeAuthentication"],
    "regulation" : "ccpa"
}
```

**响应示例**

```JSON
{
    "jobId": "d9a6b417-f619-4420-82a3-09f61fa8eff3",
    "requestId": "15765127177927284RX-739",
    "userKey": "John Dow",
    "action": "access",
    "status": "complete",
    "submittedBy": "564f7c9a-e0c8-4e74-99b8-20317ae1e235@techacct.adobe.com",
    "createdDate": "12/16/2019 04:11 PM GMT",
    "lastModifiedDate": "12/16/2019 04:15 PM GMT",
    "userIds": [
        {
            "namespace": "http://www.adobe.com/primetimeAuthentication/Dish",
            "value": "1234-5678-8765-4321",
            "type": "unregistered",
            "isDeletedClientSide": false
        }
    ],
    "productResponses": [
        {
            "product": "Adobe Pass Authentication",
            "retryCount": 0,
            "processedDate": "12/16/2019 04:15 PM GMT",
            "productStatusResponse": {
                "status": "complete",
                "results": {
                    "userContexts": [
                        {
                            "namespace": "http://www.adobe.com/primetimeAuthentication/Dish",
                            "namespaceId": 0,
                            "value": "1234-5678-8765-4321",
                            "type": "unregistered"
                        }
                    ],
                    "receiptData": {
                        "createdAt": "2019-12-16T16:15:23.4Z",
                        "message": "Data summary",
                        "numberOfAuthenticationSessions": "6",
                        "numberOfAuthorizationDecisions": "11"
                    }
                },
                "message": "Success"
            }
        }
    ],
    "downloadUrl": "https://va7gdprdevblob.blob.core.windows.net/va7gdprdevblobpublic/usa/4161962b9e8ef0027453d7cc02ecd93d/d9a6b417-f619-4420-82a3-09f61fa8eff3/d9a6b417-f619-4420-82a3-09f61fa8eff3.zip",
    "regulation": "ccpa"
}
```

### 删除 {#delete-req}

您必须上载包含要为其提交数据删除请求的Adobe Pass身份验证标识符的JSON。 要查看格式正确的JSON的外观，请参阅以下示例：

**请求示例**

```JSON
{
    "companyContexts": [{
            "namespace": "imsOrgID",
            "value": "1234567890@AdobeOrg"
        }
    ],
    "users": [{
            "key": "John Dow",
            "action": ["delete"],
            "userIDs": [{
                "namespace":"http://www.adobe.com/primetimeAuthentication/Dish",
                "type":"unregistered",
                "value":"1234-5678-8765-4321"
             }]
         
        }
    ],
    
    "include":["primetimeAuthentication"],
    "regulation" : "ccpa"
}
```

**响应示例**

对于Delete请求：

* 我们只分享已删除数据的回执，而不是包含所有已删除数据的汇总文件。
* 响应中包含的收据包含为该数据主体找到的身份验证和授权令牌总数的摘要。

```JSON
{
    "jobId": "aab380d1-a0cd-4a0d-ba95-2649ee90c063",
    "requestId": "15759883098453100RX-074",
    "userKey": "John Dow",
    "action": "delete",
    "status": "complete",
    "submittedBy": "564f7c9a-e0c8-4e74-99b8-20317ae1e235@techacct.adobe.com",
    "createdDate": "12/10/2019 02:31 PM GMT",
    "lastModifiedDate": "12/10/2019 02:34 PM GMT",
    "userIds": [
        {
            "namespace": "http://www.adobe.com/primetimeAuthentication/Dish",
            "value": "1234-5678-8765-4321",
            "type": "unregistered",
            "isDeletedClientSide": false
        }
    ],
    "productResponses": [
        {
            "product": "Adobe Pass Authentication",
            "retryCount": 0,
            "processedDate": "12/10/2019 02:34 PM GMT",
            "productStatusResponse": {
                "status": "complete",
                "results": {
                    "userContexts": [
                        {
                            "namespace": "http://www.adobe.com/primetimeAuthentication/Dish",
                            "namespaceId": 0,
                            "value": "1234-5678-8765-4321",
                            "type": "unregistered"
                        }
                    ],
                    "receiptData": {
                        "createdAt": "2019-12-10T14:34:55.274Z",
                        "message": "Data summary",
                        "numberOfAuthenticationSessions": "2",
                        "numberOfAuthorizationDecisions": "3"
                    }
                },
                "message": "Success"
            }
        }
    ],
    "downloadUrl": "https://va7gdprdevblob.blob.core.windows.net/va7gdprdevblobpublic/usa/4161962b9e8ef0027453d7cc02ecd93d/aab380d1-a0cd-4a0d-ba95-2649ee90c063/aab380d1-a0cd-4a0d-ba95-2649ee90c063.zip",
    "regulation": "ccpa"
}
```

## 如何触发请求 {#trigger-req}

客户可以使用2个选项将隐私请求发送到Adobe：

* **手动** — 使用[Privacy Service用户界面](#privacy-service-ui)
* **自动** — 通过使用[Privacy Service API](#privacy-service-api)

### 使用Privacy Service UI {#privacy-service-ui}

有关如何访问和使用Privacy Service用户界面的[完整教程](https://experienceleague.adobe.com/docs/experience-platform/privacy/home.html?lang=zh-Hans#!api-specification/markdown/narrative/tutorials/privacy_service_tutorial/privacy_service_ui_tutorial.md)可通过Adobe I/O服务在线获取。 此外，客户可以使用此链接访问有关隐私法规的视频和文章库。 单击Adobe Experience Cloud和GDPR菜单。 该操作将打开多个视频 — “GDPR UI操作说明”将介绍其使用方法。

在UI中，客户需要加载他们自己的IMSOrgID和包含每个产品的GDPR请求详细信息的JSON。

### 使用Privacy Service API {#privacy-service-api}

Adobe Experience Platform Privacy Service为访问/删除请求和私人数据的选择退出销售请求提供了通用的、集中的简化操作。

**Privacy Service API文档**&#x200B;深入介绍了Adobe客户如何与Adobe API集成。

**使用Postman（免费的第三方软件）可视化API调用：**

* GitHub上的[Privacy Service API Postman收藏集](https://github.com/adobe/experience-platform-postman-samples/blob/master/apis/experience-platform/Privacy%20Service%20API.postman_collection.json)
* [用于创建Postman环境的视频指南](https://video.tv.adobe.com/v/31668?captions=chi_hans)
* [在Postman中导入环境和收藏集的步骤](https://learning.postman.com/docs/running-collections/intro-to-collection-runs/)


**API路径：**

* 平台网关URL： `https://platform.adobe.io/`
* 此API的基本路径： `/data/core/privacy/jobs`
* 完整路径示例： `https://platform.adobe.io/data/core/privacy/jobs/ping`


**必需的标头：**

* 所有调用都需要标头`Authorization`、`x-gw-ims-org-id`和`x-api-key`。 有关如何获取这些值的详细信息，请参阅&#x200B;**身份验证教程**。
* 所有在请求正文中具有有效负载的请求(如POST、PUT和PATCH调用)都必须包含标头`Content-Type`，其值为`application/json`。

<!--

>[!RELATEDINFORMATION]
>
>* [Privacy Services Overview](https://experienceleague.adobe.com/docs/experience-platform/privacy/home.html?lang=zh-Hans#!api-specification/markdown/narrative/tutorials/privacy_service_tutorial/privacy_service_ui_tutorial.md)
>* Privacy Service API documentation

-->
