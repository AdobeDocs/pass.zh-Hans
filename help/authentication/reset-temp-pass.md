---
title: 重置临时传递
description: 重置临时传递
exl-id: ab39e444-eab2-4338-8d09-352a1d5135b6
source-git-commit: 3cff9d143eedb35155aa06c72d53b951b2d08d39
workflow-type: tm+mt
source-wordcount: '478'
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

为了&#x200B;**重置特定临时密码**，Adobe Pass身份验证为程序员提供了&#x200B;*公共* Web API：

* **环境：**&#x200B;指定将接收重置临时传递网络调用的Adobe付费电视传递服务器终结点。 可能的值： **Prequal** (*mgmt* prequal.auth.adobe.com*)、**Release** (*mgmt.auth.adobe.com*)或&#x200B;**Custom** (保留用于Adobe内部测试)。
* **OAuth2访问令牌：** OAuth2令牌是授权Adobe付费电视身份验证的程序员所必需的。 如[检索访问令牌](./dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) API文档中所述，可以获得此类令牌。
* **临时传递ID：**&#x200B;要重置的临时传递MVPD的唯一标识。（一个程序员可以使用多个Temp Pass MVPD并要重置一个特定的MVPD）
* **通用键：**&#x200B;一些临时传递MVPD（即[提升临时传递](promotional-temp-pass.md)）。

上述所有参数（除&#x200B;*泛型键*&#x200B;之外）都是必需的。 以下是参数和关联网络调用的示例（示例采用*curl *命令的形式）：

* **环境：**&#x200B;版本(*mgmt.auth.adobe.com*)
* **OAuth2访问令牌：** &lt;access_token>，如[检索访问令牌](./dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) API文档中所述
* **程序员ID：**&#x200B;引用
* **临时传递ID：**&#x200B;临时传递引用
* **泛型键：** null （未提供值）

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>" "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991&requestor_id=REF&mvpd_id=TempPassREF"
```

将向&#x200B;**/reset**&#x200B;端点发出DELETE的HTTP请求，在授权标头中传递&#x200B;*OAuth2访问令牌*，并将&#x200B;*设备ID*、*请求者ID*&#x200B;和&#x200B;*临时传递ID (MVPD ID)*&#x200B;作为参数。

如果程序员为&#x200B;*泛型键*&#x200B;提供值，则将执行另一个HTTP调用（这次是对&#x200B;**/reset/泛型**&#x200B;终结点），在&#x200B;*键*&#x200B;请求参数中传递&#x200B;*泛型键*。

例如，将&#x200B;*通用键*设置为电子邮件地址哈希(对于
临时传递支持此类功能的MVPD将产生
HTTP调用后(电子邮件为`user@domain.com`其SHA-256
哈希为`f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`)：

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>"
"https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```


为了&#x200B;**重置所有设备的特定Temp Pass**，Adobe Pass身份验证为程序员提供了&#x200B;*公共* Web API：

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v3/reset
```

>[!NOTE]
>上述URL取代了以前的重置API。 旧重置API (v1)不再受支持。

* **协议：** HTTPS
* **主机：**
   * 发行版 — mgmt.auth.adobe.com
   * 先决条件 — mgmt-prequal.auth.adobe.com
* **路径：** /reset-tempass/v3/reset
* **查询参数：** `device_id=all&requestor_id=REQUESTOR_ID&mvpd_id=TEMPPASS_MVPD_ID`
* **标头：**&#x200B;授权：持有者&lt;access_token_here>
* **响应：**
   * 成功 — HTTP 204
   * 失败：
      * 错误请求的HTTP 400
      * 如果访问被拒绝，则返回HTTP 401。 客户端必须请求新的access_token。
      * 如果不再允许客户端ID执行请求，则使用HTTP 403。 应生成新的客户端凭据。


例如：

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=all&requestor_id=AdobeBEAST&mvpd_id=TempPass"
```
