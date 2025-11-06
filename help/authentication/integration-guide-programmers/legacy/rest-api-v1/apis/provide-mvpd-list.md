---
title: 提供MVPD列表
description: 提供MVPD列表
exl-id: db2d8f19-d0b9-4195-bf0b-f9de0d96062b
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '254'
ht-degree: 2%

---

# （旧版）提供MVPD列表 {#provide-mvpd-list}

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

返回请求者的已配置MVPD列表。

| 端点 | </br>调用者 | 输入   </br>参数 | HTTP </br>方法 | 响应 | HTTP </br>响应 |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/config/{requestorId}</br></br>例如：</br></br>&lt;SP_FQDN>/api/v1/config/sampleRequestorId | Adobe Pass 身份验证 | 1.请求者</br>    （路径组件）</br>_2。  deviceType（已弃用）_ | GET | 包含MVPD列表的XML或JSON。 | 200 |

{style="table-layout:auto"}


| 输入参数 | 描述 |
| --------------- | ------------------------------------------------------------- |
| 请求者 | 此操作有效的程序员requestorId。 |
| *deviceType* | 设备类型。 |

{style="table-layout:auto"}

### 示例响应 {#sample-response}

与对/config servlet的现有MVPD XML响应相同

注意：配置为使用Platform SSO的所有MVPD在其相应节点(JSON/XML)中将具有以下额外属性：

* **enablePlatformServices （布尔值）：**&#x200B;指示此MVPD是否通过Platform SSO集成的标志
* **boardingStatus (string)：**&#x200B;标记，指示MVPD是否完全支持平台SSO （支持），或者MVPD是否仅显示在平台选取器（选取器）中
* **displayInPlatformPicker （布尔值）：**&#x200B;如果此MVPD出现在平台选取器中
* **platformMappingId （字符串）：**&#x200B;平台已知的此MVPD的标识符
* **requiredMetadataFields （字符串数组）：**&#x200B;用户元数据字段应在成功登录时可用
