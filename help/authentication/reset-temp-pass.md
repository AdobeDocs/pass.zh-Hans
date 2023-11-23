---
title: 重置临时传递
description: 重置临时传递
source-git-commit: 4ae0b17eff2dfcf0aaa5d11129dfd60743f6b467
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# 重置临时传递 {#reset-temp-pass}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。
>
>要使用Reset Temp Pass API，您需要：
>- 请向支持团队索取注册应用程序的软件声明
>- 获取访问令牌，基于 [动态客户端注册](dynamic-client-registration.md)
> 

为了 **重置特定临时传递**， Adobe Pass身份验证为程序员提供 *公共* Web API：

- **环境：** 指定将接收重置临时传递网络调用的付费 — 电视传递服务器终结点Adobe。 可能的值： **先决条件** (*mgmt-prequal.auth.adobe.com*)， **版本** (*mgmt.auth.adobe.com*)或 **自定义** (保留供Adobe内部测试使用)。
- **OAuth2访问令牌：** OAuth2令牌是授权程序员进行Adobe付费电视身份验证所必需的。 此类令牌可从以下位置获取： [动态客户端注册](dynamic-client-registration.md).
- **临时传递ID：** 要重置的临时传递MVPD的唯一ID。（一个程序员可以使用多个Temp Pass MVPD并要重置一个特定的MVPD）
- **通用键：** 某些Temp通过MVPD(即 [促销临时通票](promotional-temp-pass.md))。

上述所有参数(除 *通用键*)为必填项。 以下是参数和关联网络调用的示例（示例采用*curl *命令的形式）：

- **环境：** 发行版本(*mgmt.auth.adobe.com*)
- **OAuth2访问令牌：** &lt;access_token> 从 [动态客户端注册](dynamic-client-registration.md)
- **程序员ID：** 参照
- **临时传递ID：** TempPassREF
- **通用键：** null（未提供值）

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>" "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991&requestor_id=REF&mvpd_id=TempPassREF"
```

将向发出DELETEHTTP请求 **/reset** 端点，传递 *Oauth2访问令牌* 在Authorization标头和 *设备ID*， *请求者ID* 和 *临时传递ID (MVPD ID)* 作为参数。

如果程序员为 *通用键*，则会执行另一个HTTP调用(此次 **/reset/general** 端点)，传递 *通用键* 内部 *键* 请求参数。

例如，设置 *通用键* 对电子邮件地址哈希（用于支持此类功能的临时传递MVPD）将产生以下HTTP调用(电子邮件为 `user@domain.com` 其SHA-256哈希为 `f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`)：

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>"
"https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```


为了 **重置所有设备的特定Temp Pass**， Adobe Pass身份验证为程序员提供 *公共* Web API：

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v3/reset
```

>[!NOTE]
>上述URL取代了以前的重置API。 旧重置API (v1)不再受支持。

- **协议：** HTTPS
- **主机：**
   - 发行版 — mgmt.auth.adobe.com
   - 先决条件 — mgmt-prequal.auth.adobe.com
- **路径：** /reset-tempass/v3/reset
- **查询参数：** `device_id=all&requestor_id=REQUESTOR_ID&mvpd_id=TEMPPASS_MVPD_ID`
- **标头：** 授权：持有者 &lt;access_token_here>
- **响应：**
   - 成功 — HTTP 204
   - 失败：
      - 错误请求的HTTP 400
      - 如果访问被拒绝，则返回HTTP 401。 客户端必须请求新的access_token。
      - 如果不再允许客户端ID执行请求，则使用HTTP 403。 应生成新的客户端凭据。


例如：

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=all&requestor_id=AdobeBEAST&mvpd_id=TempPass"
```
