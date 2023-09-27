---
title: REST API参考
description: Rest api引用
exl-id: 67e4639e-db0b-4400-bb81-e214263e8395
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '649'
ht-degree: 4%

---

# REST API参考 {#rest-api-reference}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## 响应格式 {#response-formats}


>[!NOTE]
>
> 这些服务中提供的API可以以XML或JSON格式返回响应（对于返回响应的API）。 有3种不同的方法在请求中指定响应格式：
>
>* 将HTTP接受标头设置为 `application/xml` 或 `application/json`.
>* 在请求有效负载中，指定参数 `format=xml` 或 `format=json`.
>* 使用扩展调用Web服务端点 `.xml` 或 `.json`. 例如， `/regcode.xml` 或 `/regcode.json`
>
>您可以指定以上任意一种方法。 使用冲突的格式指定多个方法可能会导致错误或不适当的输出。

## REST API端点 {#clientless-endpoints}

&lt;reggie_fqdn>：

* 生产 —  [api.auth.adobe.com](http://api.auth.adobe.com/)
* 暂存 —  [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;sp_fqdn>：

* 生产 —  [api.auth.adobe.com](http://api.auth.adobe.com/)
* 暂存 —  [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>


## Web服务摘要 {#web_srvs_summary}

下表列出了适用于无客户端方法的Web服务。 单击Web服务端点以了解更多信息（示例请求和响应、输入参数、HTTP方法等）


| Sr | Web服务端点 | 描述 | <!--[Diag.  </br>Ref](http://tve.helpdocsonline.com/api-reference-v2-test#illustration)-->. | 托管位置 | 调用者 |
| --- | --- | --- | --- | --- | --- |
| 1. | [&lt;reggie_fqdn>/reggie/v1/  </br>  {requestorId}/regcode](/help/authentication/registration-code-request.md) | 返回随机生成的注册代码和登录页面URI | 2 | Adobe  </br>注册代码服务 | 智能设备 |
| 2. | [&lt;reggie_fqdn>/reggie/v1/  </br>  {requestorId}/regcode/  </br>  {registrationCode}](/help/authentication/return-registration-record.md) | 返回包含注册码UUID、注册码和哈希设备ID的注册码记录 | 8 | Adobe  </br>注册代码服务 | Adobe Pass身份验证 |
| 3. | [&lt;sp_fqdn>/api/v1/config/  </br>  {requestorId}](/help/authentication/provide-mvpd-list.md) | 返回请求者的已配置MVPD列表 | 5 | Adobe  </br>Adobe Pass  </br>身份验证  </br>服务 | 登录  </br>Web  </br>应用程序 |
| 4. | [&lt;sp_fqdn>/api/v1/authenticate](/help/authentication/initiate-authentication.md) | 通过通知MVPD选择事件启动AuthN进程。 在验证数据库上创建记录，在从MVPD收到成功响应时进行协调（步骤13） | 7 | Adobe  </br>Adobe Pass  </br>身份验证  </br>服务 | 登录  </br>Web  </br>应用程序 |
| 5. | SAML断言消费者 | Adobe Pass身份验证与MVPD之间的现有SAML工作流 | 13 | Adobe Pass  </br>身份验证  </br>服务 | Adobe Pass身份验证 |
| 6. | [&lt;sp_fqdn>/api/v1/checkauthn/  </br>  {registrationCode}](/help/authentication/check-authentication-flow-by-second-screen-web-app.md) | 登录Web应用程序可以检查尝试的登录流程是否成功 |     | Adobe Pass  </br>身份验证   </br>服务 | 登录   </br>Web   </br>应用程序 |
| 7. | [&lt;sp_fqdn>/api/v1/tokens/authn](/help/authentication/retrieve-authentication-token.md) | 获取AuthN令牌相关的元数据 | 15 | Adobe Pass  </br>身份验证  </br>服务 | 智能设备 |
| 8. | [&lt;reggie_fqdn>/reggie/v1/  </br>  {requestorId}/regcode/  </br>  {registrationCode}](/help/authentication/delete-registration-record.md) | 删除注册代码记录并释放注册代码以供重用 | 16 | Adobe  </br>注册代码服务 | Adobe Pass身份验证 |
| 9. | [&lt;sp_fqdn>/api/v1/authorize](/help/authentication/initiate-authorization.md) | 获取授权响应。 | 17 | Adobe Pass  </br>身份验证  </br>服务 | 智能设备 |
| 10. | [&lt;sp_fqdn>/api/v1/checkauthn](/help/authentication/check-authentication-token.md) | 指示设备是否具有未过期的身份验证令牌。 |     | Adobe Pass  </br>身份验证  </br>服务 | 智能设备 |
| 11. | [&lt;sp_fqdn>/api/v1/tokens/authn](/help/authentication/retrieve-authentication-token.md) | 如果找到，则返回AuthN标记。 |     | Adobe Pass  </br>身份验证  </br>服务 | 智能设备 |
| 12. | [&lt;sp_fqdn>/api/v1/tokens/authz](/help/authentication/retrieve-authorization-token.md) | 如果找到，则返回AuthZ令牌。 |     | Adobe Pass  </br>身份验证  </br>服务 | 智能设备 |
| 13. | [&lt;sp_fqdn>/api/v1/tokens/media](/help/authentication/obtain-short-media-token.md) | 如果找到，则返回短媒体令牌 — 与/api/v1/mediatoken相同 |     | Adobe Pass  </br>身份验证  </br>服务 | 智能设备 |
| 14. | [&lt;sp_fqdn>/api/v1/mediatoken](/help/authentication/obtain-short-media-token.md) | 获取短媒体令牌 |     | Adobe Pass  </br>身份验证  </br>服务 | 智能设备 |
| 15. | [&lt;sp_fqdn>/api/v1/preauthorize](/help/authentication/retrieve-list-of-preauthorized-resources.md) | 检索预授权资源的列表 |     | Adobe Pass  </br>身份验证  </br>服务 | 智能设备 |
| 16. | [&lt;sp_fqdn>/api/v1/preauthorize/{code}](/help/authentication/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md) | 检索预授权资源的列表 |     | Adobe Pass  </br>身份验证  </br>服务 | 登录Web应用程序 |
| 17. | [&lt;sp_fqdn>/api/v1/logout](/help/authentication/initiate-logout.md) | 从存储中删除AuthN和AuthZ令牌 |     | Adobe Pass  </br>身份验证   </br>服务 | 智能设备 |
| 18. | [&lt;sp_fqdn>/api/v1/tokens/usermetadata](/help/authentication/user-metadata.md) | 在验证流完成后获取用户元数据 | 不适用 | 不适用 | 智能设备 |
| 19. | [&lt;sp_fqdn>/api/v1/authenticate/freepreview](/help/authentication/free-preview-for-temp-pass-and-promotional-temp-pass.md) | 为临时传递或提升临时传递创建身份验证令牌 | 不适用 | Adobe Pass  </br>身份验证  </br>服务 | 智能设备 |


## REST API安全性 {#security}

必须使用HTTPS协议调用所有Adobe Pass身份验证无客户端API，才能进行安全通信。 此外，调用的大多数API应包含由提供的访问令牌 [动态客户端注册](/help/authentication/dynamic-client-registration.md).
