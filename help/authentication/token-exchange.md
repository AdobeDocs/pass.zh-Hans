---
title: 交换Adobe令牌的平台SSO令牌
description: 交换Adobe令牌的平台SSO令牌
exl-id: 5ab60268-8f97-4755-8281-be45e812ed7f
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 2%

---

# 交换Adobe令牌的平台SSO令牌 {#exchange-a-platform-sso-token-for-an-adobe-token}

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

允许将平台SSO配置文件“交换”为Adobe令牌。

| 端点 | 已调用  </br>按 | 输入   </br>参数 | HTTP  </br>方法 | 响应 | HTTP  </br>响应 |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/tokens/authn | 流应用程序</br></br>或</br></br>程序员服务 | 1.申请人（必填）</br>    </br>2.  deviceId（必需）</br>    </br>3.  mvpd（必需）</br>    </br>4.  deviceType（必需）</br>    </br>5.  SAMLResponse（必需）</br>    </br>6.  deviceUser（已弃用）</br>    </br>7.  appId（已弃用） | POST | 成功的响应将为“204无内容”，这表示已成功创建令牌并准备好用于授权流。 | 204 — 无内容   </br>400 — 错误请求 |


| 输入参数 | 描述 |
| --- | --- |
| 请求者 | 此操作有效的程序员requestorId。 |
| deviceId | 设备ID字节。 |
| mvpd | 此操作有效的MVPD ID。 |
| 设备类型 | 我们尝试为其获取配置文件请求的Apple平台。  或者 **iOS** 或 **tvOS**. |
| SAMLResponse | Platform SSO返回的实际配置文件。 |
| _设备用户_ | 设备用户标识符。 |
| _appId_ | 应用程序id/名称。 |
