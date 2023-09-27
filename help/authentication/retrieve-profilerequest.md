---
title: 检索Platform SSO配置文件请求
description: 检索Platform SSO配置文件请求
exl-id: 44fd4e26-4d9a-4607-ac2c-b85d848f5fc6
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '183'
ht-degree: 0%

---

# 检索Platform SSO配置文件请求 {#retrieve-platform-sso-profile-request}

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

此资源会为请求者ID和MVPD元组生成配置文件请求。


| 端点 | 已调用  </br>按 | 输入   </br>参数 | HTTP  </br>方法 | 响应 | HTTP  </br>响应 |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/{requestor}/profile-requests/{mvpd} | 流应用程序</br></br>或</br></br>程序员服务 | 1.请求者（路径参数）</br>2. mvpd（路径参数）</br>3. deviceType（必需） | GET | 响应Content-Type将为application/octet-stream，因为实际有效负载对客户端应用程序是不透明的。</br></br>应用程序应将响应转发到平台</br></br>用于获取配置文件SSO的SSO引擎。 | 200 — 成功   </br>400 — 错误请求 |


| 输入参数 | 描述 |
| --------------- | -------------------------------------------------------------------------------------------------------- |
| 请求者 | 此操作有效的程序员requestorId。 |
| mvpd | 此操作有效的MVPD ID。 |
| 设备类型 | 我们尝试为其获取配置文件请求的Apple平台。  或者 **iOS** 或 **tvOS**. |
