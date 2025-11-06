---
title: 检索Platform SSO配置文件请求
description: 检索Platform SSO配置文件请求
exl-id: 44fd4e26-4d9a-4607-ac2c-b85d848f5fc6
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 0%

---

# （旧版）检索平台SSO配置文件请求 {#retrieve-platform-sso-profile-request}

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

此资源会为请求者ID和MVPD元组生成配置文件请求。


| 端点 | </br>调用者 | 输入   </br>参数 | HTTP </br>方法 | 响应 | HTTP </br>响应 |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/{requestor}/profile-requests/{mvpd} | 流式处理应用程序</br></br>或</br></br>程序员服务 | 1.请求者（路径参数）</br>2。 mvpd （路径参数）</br>3。 deviceType（必需） | GET | 响应Content-Type将为application/octet-stream，因为实际有效负载对客户端应用程序是不透明的。</br></br>应用程序应将响应转发到Platform</br></br>SSO引擎以获取配置文件SSO。 | 200 — 成功   </br>400 — 错误请求 |


| 输入参数 | 描述 |
| --------------- | -------------------------------------------------------------------------------------------------------- |
| 请求者 | 此操作有效的程序员requestorId。 |
| mvpd | 此操作对其有效的MVPD ID。 |
| 设备类型 | 我们尝试为其获取配置文件请求的Apple平台。  **iOS**&#x200B;或&#x200B;**tvOS**。 |
