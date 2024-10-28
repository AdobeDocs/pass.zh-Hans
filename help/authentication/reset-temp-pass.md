---
title: 重置临时传递
description: 重置临时传递
exl-id: ab39e444-eab2-4338-8d09-352a1d5135b6
source-git-commit: 32060798501abe2bf7314474d55cf844efb3f7b1
workflow-type: tm+mt
source-wordcount: '336'
ht-degree: 0%

---


# 重置临时传递 {#reset-temp-pass}

>[!IMPORTANT]
>
> 此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

>[!IMPORTANT]
>
> 在使用Reset Temp Pass API之前，请确保满足以下先决条件：
>
> * 按照[检索客户端凭据](./dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) API文档中的说明获取客户端凭据。
> * 按照[检索访问令牌](./dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) API文档中的说明获取访问令牌。
>
> 有关如何创建已注册的应用程序和下载软件语句的详细信息，请参阅[动态客户端注册概述](./dcr-api/dynamic-client-registration-overview.md)文档。

为了&#x200B;**重置设备或所有设备的特定Temp Pass**，Adobe Pass身份验证为程序员提供了&#x200B;*公共* Web API：

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v3/reset
```

* **协议：** HTTPS
* **主机：**
   * 发行版 — mgmt.auth.adobe.com
   * 先决条件 — mgmt-prequal.auth.adobe.com
* **路径：** /reset-tempass/v3/reset
* **查询参数：** device_id（未提供任何值时，默认为all）、requestor_id（必需）、mvpd_id（必需）、appId、deviceUser、environment
* **标头：**&#x200B;授权：持有者&lt;access_token_here>
* **响应：**
   * 成功 — HTTP 204
   * 失败：xw
      * 错误请求的HTTP 400
      * 如果访问被拒绝，则返回HTTP 401。 客户端必须请求新的access_token。
      * 如果不再允许客户端id执行请求，则使用HTTP 403。 应生成新的客户端凭据。


例如：

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=all&requestor_id=AdobeBEAST&mvpd_id=TempPass"
```

为了&#x200B;**重置通用密钥或所有密钥**&#x200B;的Flexible Temp Pass，Adobe Pass身份验证为程序员提供了&#x200B;*公共* Web API：

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic
```

* **协议：** HTTPS
* **主机：**
   * 发行版 — mgmt.auth.adobe.com
   * 先决条件 — mgmt-prequal.auth.adobe.com
* **路径：** /reset-tempass/v3/reset/generic
* **查询参数：**&#x200B;键（未提供任何值时，默认为all）、requestor_id（必需）、mvpd_id（必需）、环境
* **标头：**&#x200B;授权：持有者&lt;access_token_here>
* **响应：**
   * 成功 — HTTP 204
   * 失败：
      * 错误请求的HTTP 400
      * 如果访问被拒绝，则返回HTTP 401。 客户端必须请求新的access_token。
      * 如果不再允许客户端id执行请求，则使用HTTP 403。 应生成新的客户端凭据。


例如，将&#x200B;*通用键*设置为电子邮件地址哈希(对于
临时传递支持此类功能的MVPD将产生
HTTP调用后(电子邮件为`user@domain.com`其SHA-256
哈希为`f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`)：

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>"
"https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```
