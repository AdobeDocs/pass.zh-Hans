---
title: 增强的错误代码
description: 增强的错误代码
exl-id: 2b0a9095-206b-4dc7-ab9e-e34abf4d359c
source-git-commit: 27aaa0d3351577e60970a4035b02d814f0a17e2f
workflow-type: tm+mt
source-wordcount: '2649'
ht-degree: 3%

---

# 增强的错误代码 {#enhanced-error-codes}

>[!IMPORTANT]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

增强的错误代码表示Adobe Pass身份验证功能，该功能向与集成的客户端应用程序提供其他错误信息：

* Adobe Pass身份验证REST API：
   * [REST API v2](../../rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)
   * [（旧版）REST API v1](../../legacy/rest-api-v1/rest-api-overview.md)
* Adobe Pass身份验证SDK预授权API：
   * [（旧版）JavaScript SDK（预授权API）](../../legacy/sdks/javascript-sdk/preauthorize-api-javascript-sdk.md)
   * [（旧版）iOS/tvOS SDK（预授权API）](../../legacy/sdks/ios-tvos-sdk/preauthorize-api-ios-tvos-sdk.md)
   * [（旧版）Android SDK（预授权API）](../../legacy/sdks/android-sdk/preauthorize-api-android-sdk.md)

  _(*) Preauthorize API是唯一支持增强错误代码的Adobe Pass身份验证SDK API。_

>[!IMPORTANT]
>
> 集成Adobe Pass身份验证REST API v2的应用程序将默认受益于增强错误代码，而无需其他配置。
>
> <br/>
>
> 集成Adobe Pass身份验证REST API v1或SDK预授权API的应用程序只有在明确启用该功能的情况下才能受益于增强错误代码。
>
> <br/>
>
> 要明确启用此功能，请通过我们的[Zendesk](https://adobeprimetime.zendesk.com)创建票证，并咨询您的技术客户经理(TAM)以获得帮助。

## 呈现 {#enhanced-error-codes-representation}

增强的错误代码可以采用`JSON`或`XML`格式表示，具体取决于集成的Adobe Pass身份验证API和使用的“接受”标头值（即`application/json`或`application/xml`）：

| Adobe Pass身份验证API | JSON | XML |
|-------------------------------|---------|---------|
| REST API v2 | &amp;amp；检查； |         |
| REST API v1 | &amp;amp；检查； | &amp;amp；检查； |
| SDK预授权API | &amp;amp；检查； |         |

>[!IMPORTANT]
>
> Adobe Pass身份验证可以通过两种形式将增强的错误代码传达给客户端应用程序：
>
> <br/>
>
> * **顶级错误信息**：在这种情况下，***&quot;error&quot;***&#x200B;对象位于顶级，因此响应正文只能包含&#x200B;***&quot;error&quot;***&#x200B;对象。
> * **项级别错误信息**：在这种情况下，***&quot;error&quot;***&#x200B;对象位于项级别，因此对于在服务时遇到错误的所有项，响应正文可以包含&#x200B;***&quot;error&quot;***&#x200B;对象。
>
> <br/>
>
> 查看每个集成Adobe Pass身份验证API的公共文档，确定增强的错误代码表示细节。

**REST API v2**

请参阅以下包含适用于REST API v2的增强错误代码示例的HTTP响应，示例表示为`JSON`。

>[!BEGINTABS]

>[!TAB REST API v2 — 项目级错误信息(JSON)]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json

{
  "decisions": [
    {
      "resource": "REF30",
      "serviceProvider": "REF30",
      "mvpd": "Cablevision",
      "source": "mvpd",
      "authorized": true,
      "token": {
        "issuedAt": 1697094207324,
        "notBefore": 1697094207324,
        "notAfter": 1697094802367,
        "serializedToken": "PHNpZ25hdHVyZUluZm8..."
      }
    },
    {
      "resource": "REF40",
      "serviceProvider": "REF40",
      "mvpd": "Cablevision",
      "source": "mvpd",
      "authorized": false,
      "error" : {
        "action": "none",
        "status": 403,
        "code": "authorization_denied_by_mvpd",
        "message": "The MVPD has returned a \"Deny\" decision when requesting authorization for the specified resource",
        "details": "Your subscription package does not include the \"Live\" channel",
        "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html?lang=zh-Hans",
        "trace": "12f6fef9-d2e0-422b-a9d7-60d799abe353"
      }
    }
  ]
}
```

>[!TAB REST API v2 — 顶级错误信息(JSON)]

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
  "action": "none",
  "status": 400,
  "code": "invalid_parameter_service_provider",
  "message": "The service provider parameter value is missing or invalid.",
  "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html?lang=zh-Hans",
  "trace": "12f6fef9-d2e0-422b-a9d7-60d799abe353"
}
```

>[!ENDTABS]

**REST API v1**

请参阅以下HTTP响应，其中包含适用于REST API v1的表示为`JSON`或`XML`的增强错误代码示例。

>[!BEGINTABS]

>[!TAB REST API v1 — 项目级错误信息(JSON)]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json

{
  "resources": [
    {
      "id": "TestStream1",
      "authorized": true
    },
    {
      "id": "TestStream2",
      "authorized": false,
      "error": {
        "action": "none",
        "status": 403,
        "code": "authorization_denied_by_mvpd",
        "message": "The MVPD has returned a \"Deny\" decision when requesting authorization for the specified resource",
        "details": "Your subscription package does not include the \"Live\" channel",
        "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html?lang=zh-Hans",
        "trace": "12f6fef9-d2e0-422b-a9d7-60d799abe353"
      }
    }
  ]
}
```

>[!TAB REST API v1 — 顶级错误信息(JSON)]

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json
        
{
  "action": "none",
  "status": 400,
  "code": "invalid_requestor",
  "message": "The requestor parameter is missing or invalid.",
  "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html?lang=zh-Hans",
  "trace": "8bcb17f9-b172-47d2-86d9-3eb146eba85e"
}
```

>[!TAB REST API v1 — 顶级错误信息(XML)]

```XML
HTTP/1.1 400 Bad Request
Content-Type: application/xml

<error>
  <action>none</action>
  <status>400</status>
  <code>invalid_requestor</code>
  <message>The requestor parameter is missing or invalid.</message>
  <helpUrl>https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html?lang=zh-Hans</helpUrl>
  <trace>8bcb17f9-b172-47d2-86d9-3eb146eba85e</trace>
</error>
```

>[!ENDTABS]

### 结构 {#enhanced-error-codes-representation-structure}

增强的错误代码包括以下带有示例的`JSON`字段或`XML`属性：

| 名称 | 类型 | 示例 | 受限 | 描述 |
|-----------|-----------|---------------------------------------------------------------------------------------------------------------------|:----------:|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| *操作* | *字符串* | *无* | &amp;amp；检查； | Adobe Pass身份验证推荐的操作，该操作可能会修正本文档中定义的情况。 <br/><br/>有关更多详细信息，请参阅[操作](#enhanced-error-codes-action)部分。 |
| *状态* | *整数* | *403* | &amp;amp；检查； | [RFC 7231](https://tools.ietf.org/html/rfc7231#section-6)文档中定义的HTTP响应状态代码。 <br/><br/>有关更多详细信息，请参阅[状态](#enhanced-error-codes-status)部分。 |
| *代码* | *字符串* | *authorization_denied_by_mvpd* | &amp;amp；检查； | 与本文档中定义的错误关联的Adobe Pass身份验证唯一标识符代码。 <br/><br/>有关更多详细信息，请参阅[代码](#enhanced-error-codes-code)部分。 |
| *消息* | *字符串* | *请求对指定资源的授权时，MVPD返回了“拒绝”决定* |            | 在某些情况下，可以向最终用户显示的可读消息。 <br/><br/>有关更多详细信息，请参阅[响应处理](#enhanced-error-codes-response-handling)部分。 |
| *详细信息* | *字符串* | *您的订阅包不包含“实时”频道* |            | 在某些情况下，服务合作伙伴可以提供的详细消息，<br/><br/>如果服务合作伙伴不提供任何自定义消息，则此字段可能不存在。 |
| *helpUrl* | *url* | *https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html?lang=zh-Hans* |            | Adobe Pass身份验证公共文档URL，该URL链接到有关出现此错误的原因和可能解决方案的更多信息。 <br/><br/>此字段包含绝对URL，不应从错误代码推断，根据错误上下文，可以提供不同的URL。 |
| *跟踪* | *字符串* | *12f6fef9-d2e0-422b-a9d7-60d799abe353* |            | 响应的唯一标识符，在联系Adobe Pass身份验证支持团队以解决特定问题时可以使用该标识符。 |

>[!IMPORTANT]
>
> **Restricted**&#x200B;列指示相应字段是否包含有限集中的值，而无限制字段可以包含任何数据。
>
> <br/>
>
> 将来对此文档的更新可以将值添加到有限集，但不会移除或更改现有值。

### 操作 {#enhanced-error-codes-representation-action}

增强的错误代码包括一个“action”字段，该字段提供了可能纠正此情况的推荐操作。

“action”字段的可能值包括：

| 操作 | 描述 | 类别 |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------|
| 无 | 没有预定义的操作可修复此问题，但在某些情况下，这可能指示对API的调用不正确。 | 修复请求上下文。 |
| 配置 | 客户端应用程序要求进行配置更改，大部分时间都是通过Adobe Pass TVE Dashboard执行的。 | 修复集成配置上下文。 |
| application-register | 客户端应用程序需要再次注册自身。 | 修复客户端应用程序上下文。 |
| 身份验证 | 客户端应用程序要求验证或重新验证用户。 | 修复客户端应用程序上下文。 |
| 授权 | 客户端应用程序要求获取指定资源的授权。 | 修复客户端应用程序上下文。 |
| 重试 | 客户端应用程序要求重试该请求。 | 修复请求上下文。 |

_(*)对于某些错误，多个操作可能是可能的解决方案，但“action”字段指示修复错误的概率最高的操作。_

### 状态 {#enhanced-error-codes-representation-status}

增强的错误代码包含一个“状态”字段，该字段指示与错误关联的HTTP状态代码。

“status”字段的可能值包括：

| 代码 | 原因短语 |
|------|-----------------------|
| 400 | 错误请求 |
| 401 | 未授权 |
| 403 | 禁止 |
| 404 | 未找到 |
| 405 | 不允许使用该方法 |
| 410 | 不存在 |
| 412 | 先决条件失败 |
| 500 | 内部服务器错误 |

当客户端生成错误并且大多数时间这意味着客户端需要额外的操作来修复它时，通常会显示带有4xx“状态”的增强错误代码。

具有5xx“状态”的增强型错误代码通常在服务器生成错误时出现，大多数情况下，这意味着服务器需要额外的工作来修复错误。

>[!IMPORTANT]
>
> 有时，HTTP响应状态代码与增强型错误代码“状态”字段不同，特别是当与将增强型错误代码作为项目级错误信息传达的Adobe Pass身份验证API交互时。

### 代码 {#enhanced-error-codes-representation-code}

增强的“错误代码”包含一个“代码”字段，该字段提供与错误关联的Adobe Pass身份验证唯一标识符。

基于集成的Adobe Pass身份验证API，“代码”字段的可能值汇总在两个列表中[低于](#enhanced-error-codes-list)。

## 列表 {#enhanced-error-codes-lists}

### REST API v2 {#enhanced-error-codes-lists-rest-api-v2}

下表列出了客户端应用程序在与Adobe Pass身份验证REST API v2集成时可能遇到的增强错误代码。

| 操作 | 代码 | 状态 | 消息 |
|------------------------------|--------------------------------------------------------|--------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **无** | *invalid_parameter_service_provider* | 400 | 服务提供商参数值缺失或无效。 |
|                              | *invalid_parameter_mvpd* | 400 | mvpd参数值缺失或无效。 |
|                              | *invalid_parameter_code* | 400 | 代码参数值缺失或无效。 |
|                              | *invalid_parameter_resources* | 400 | 资源参数值缺失或无效。 |
|                              | *invalid_parameter_redirect_url* | 400 | 重定向URL参数值缺失或无效。 |
|                              | *invalid_parameter_partner* | 400 | 缺少伙伴参数值或伙伴参数值无效。 |
|                              | *invalid_parameter_saml_response* | 400 | SAML响应参数值缺失或无效。 |
|                              | *invalid_header_device_info* | 400 | 设备信息标头值缺失或无效。 |
|                              | *invalid_header_device_identifier* | 400 | 设备标识符标头值缺失或无效。 |
|                              | *invalid_header_identity_for_temporary_access* | 400 | 临时访问标头值的标识缺失或无效。 |
|                              | *invalid_header_pfs_permission_access_not_present* | 400 | 合作伙伴框架状态标头中的权限访问状态值不存在。 |
|                              | *invalid_header_pfs_permission_access_not_determined* | 400 | 合作伙伴框架状态标头中的权限访问状态值不确定。 |
|                              | *invalid_header_pfs_permission_access_not_granted* | 400 | 未授予来自合作伙伴框架状态标头的权限访问状态值。 |
|                              | *invalid_header_pfs_provider_id_not_determined* | 400 | 合作伙伴框架状态标头中的提供程序ID值未与已知的mvpd关联。 |
|                              | *invalid_header_pfs_provider_id_mismatch* | 400 | 合作伙伴框架状态标头中的提供程序ID值与作为参数发送的mvpd不匹配。 |
|                              | *invalid_header_pfs_provider_info_expired* | 400 | 合作伙伴框架状态标头中的提供程序信息已过期。 |
|                              | *无效集成* | 400 | 指定的服务提供程序与mvpd之间的集成不存在或已禁用。 |
|                              | *invalid_authentication_session* | 400 | 与此请求关联的身份验证会话缺失或无效。 |
|                              | *preauthorization_denied_by_mvpd* | 403 | 在请求指定资源的预授权时，MVPD返回了“拒绝”决定。 |
|                              | *authorization_denied_by_mvpd* | 403 | 请求对指定资源的授权时，MVPD返回了“拒绝”决定。 |
|                              | *authorization_denied_by_parental_controls* | 403 | 由于指定资源的家长控制设置，MVPD已返回“拒绝”决定。 |
|                              | *authorization_denied_by_degradation_rule* | 403 | 指定的服务提供商与mvpd之间的集成应用了拒绝授权所请求的资源的降级规则。 |
|                              | *内部服务器错误* | 500 | 由于内部服务器错误，请求失败。 |
| **配置** | *太多资源* | 403 | 授权或预授权请求失败，因为查询的资源过多。 请联系支持团队以正确配置授权和预授权限制。 |
|                              | *invalid_configuration_user_metadata_certificate* | 500 | 用户元数据证书配置缺失或无效。 |
|                              | *invalid_configuration_temporary_access* | 500 | 临时访问配置无效。 |
|                              | *invalid_configuration_platform* | 500 | 集成缺少平台配置或平台配置无效。 |
|                              | *invalid_configuration_platform_id* | 500 | 平台ID配置缺失或无效。 |
|                              | *invalid_configuration_platform_trait* | 500 | 平台特征配置缺失或无效。 |
|                              | *invalid_configuration_platform_category_trait* | 500 | 平台类别特征配置缺失或无效。 |
|                              | *invalid_configuration_platform_services* | 500 | 缺少平台服务配置或集成无效。 |
|                              | *invalid_configuration_mvpd_platform* | 500 | mvpd平台配置缺失或对于mvpd和平台无效。 |
|                              | *invalid_configuration_mvpd_platform_boarding_status* | 500 | mvpd平台载入状态配置缺失或对于mvpd和平台无效。 |
|                              | *invalid_configuration_mvpd_platform_profile_exchange* | 500 | mvpd平台配置文件交换配置缺失或对于mvpd和平台无效。 |
| **应用程序注册** | *invalid_access_token_service_provider* | 401 | 由于服务提供商无效，访问令牌无效。 |
|                              | *invalid_access_token_client_application* | 401 | 由于客户端应用程序无效，访问令牌无效。 |
| **身份验证** | *authenticated_profile_missing* | 403 | 缺少与此请求关联的已验证配置文件。 |
|                              | *authenticated_profile_expires* | 403 | 与此请求关联的已验证配置文件已过期。 |
|                              | *authenticated_profile_invalid* | 403 | 与此请求关联的已验证配置文件已失效。 |
|                              | *temporary_access_duration_limit_exceeded* | 403 | 已超过临时访问持续时间限制。 |
|                              | *temporary_access_resources_limit_exceeded* | 403 | 已超出临时访问资源限制。 |
|                              | *authorization_denied_by_hba_policies* | 403 | 由于基于主页的身份验证策略，MVPD已返回“拒绝”决策。 当前身份验证是通过基于家乡的身份验证流程获得的，但在请求指定资源的授权时，设备不再位于家乡中。 用户必须使用支持的MVPD重新进行身份验证才能继续。 |
|                              | *authorization_denied_by_session_invalidated* | 403 | 身份验证会话被身份提供程序失效。 用户必须使用支持的MVPD重新进行身份验证才能继续。 |
|                              | *identity_not_recovered_by_mvpd* | 403 | 由于MVPD无法识别用户身份，授权请求失败。 |
| **重试** | *network_received_error* | 403 | 从关联的合作伙伴服务检索响应时出现读取错误。 重试请求可能会解决此问题。 |
|                              | *network_connection_timeout* | 403 | 与关联的合作伙伴服务的连接超时。 重试请求可能会解决此问题。 |
|                              | *maximum_execution_time_exceeded* | 403 | 请求未在允许的最长时间内完成。 重试请求可能会解决此问题。 |

### REST API v1 {#enhanced-error-codes-lists-rest-api-v1}

下表列出了客户端应用程序在与Adobe Pass身份验证REST API v1集成时可能遇到的增强错误代码。

| 操作 | 代码 | 状态 | 消息 |
|--------------------|---------------------------------------------------|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **无** | *invalid_requestor* | 400 | 请求者参数缺失或无效。 |
|                    | *invalid_device_info* | 400 | 设备信息缺失或无效。 |
|                    | *invalid_device_id* | 400 | 设备标识符缺失或无效。 |
|                    | *missing_resource* | 400， 412 | 缺少资源参数。 |
|                    | *格式错误的authz_request* | 400， 412 | 授权请求为null或无效。 |
|                    | *preauthorization_denied_by_mvpd* | 403 | 在请求指定资源的预授权时，MVPD返回了“拒绝”决定。 |
|                    | *authorization_denied_by_mvpd* | 403 | 请求对指定资源的授权时，MVPD返回了“拒绝”决定。 |
|                    | *authorization_denied_by_parental_controls* | 403 | 由于指定资源的家长控制设置，MVPD已返回“拒绝”决定。 |
|                    | *内部错误* | 400， 405， 500 | 由于内部服务器错误，请求失败。 |
| **配置** | *未知集成* | 400， 412 | 指定的程序员和标识提供程序之间的集成不存在。 使用TVE功能板创建所需的集成。 |
|                    | *太多资源* | 403 | 授权或预授权请求失败，因为查询的资源过多。 请联系支持团队以正确配置授权和预授权限制。 |
| **身份验证** | *authentication_session_issuer_mismatch* | 400 | 授权请求失败，因为为授权流指示的MVPD与颁发身份验证会话的不同。 用户必须使用所需的MVPD重新进行身份验证才能继续。 |
|                    | *authorization_denied_by_hba_policies* | 403 | 由于基于主页的身份验证策略，MVPD已返回“拒绝”决策。 当前身份验证是使用基于主目录的身份验证流程(HBA)获得的，但在请求指定资源的授权时，设备不再位于主目录。 用户必须使用支持的MVPD重新进行身份验证才能继续。 |
|                    | *authorization_denied_by_session_invalidated* | 403 | 身份验证会话被身份提供程序失效。 用户必须使用支持的MVPD重新进行身份验证才能继续。 |
|                    | *identity_not_recovered_by_mvpd* | 403 | 由于MVPD无法识别用户身份，授权请求失败。 |
|                    | *authentication_session_invalidated* | 403 | 身份验证会话被身份提供程序失效。 用户必须使用支持的MVPD重新进行身份验证才能继续。 |
|                    | *authentication_session_missing* | 403， 412 | 无法检索与此请求关联的身份验证会话。 用户必须使用支持的MVPD重新进行身份验证才能继续。 |
|                    | *authentication_session_expired* | 403， 412 | 当前的身份验证会话已过期。 用户必须使用支持的MVPD重新进行身份验证才能继续。 |
|                    | *preauthorization_authentication_session_missing* | 412 | 无法检索与此请求关联的身份验证会话。 用户必须使用支持的MVPD重新进行身份验证才能继续。 |
|                    | *preauthorization_authentication_session_expired* | 412 | 当前的身份验证会话已过期。 用户必须使用支持的MVPD重新进行身份验证才能继续。 |
| **授权** | *authorization_not_found* | 403， 404 | 未找到指定资源的授权。 用户必须获得新授权才能继续。 |
|                    | *authorization_expired* | 410 | 指定资源的先前授权已过期。 用户必须获得新授权才能继续。 |
| **重试** | *network_received_error* | 403 | 从关联的合作伙伴服务检索响应时出现读取错误。 重试请求可能会解决此问题。 |
|                    | *network_connection_timeout* | 403 | 与关联的合作伙伴服务的连接超时。 重试请求可能会解决此问题。 |
|                    | *maximum_execution_time_exceeded* | 403 | 请求未在允许的最长时间内完成。 重试请求可能会解决此问题。 |

### SDK预授权API {#enhanced-error-codes-lists-sdks-preauthorize-api}

有关客户端应用程序在与Adobe Pass Authentication SDK预授权API集成时可能遇到的增强错误代码，请参阅上一个[部分](#enhanced-error-codes-list-rest-api-v1)。

## 响应处理 {#enhanced-error-codes-response-handling}

>[!IMPORTANT]
>
> 增强型错误代码可以在客户端应用程序代码中自动处理，例如，在网络超时的情况下重试授权请求，或者在会话过期时要求用户重新进行身份验证，但其他类型可能需要配置更改或Adobe Pass身份验证客户关怀团队交互。
>
> <br/>
>
> 因此，在通过我们的[Zendesk](https://adobeprimetime.zendesk.com)创建票证时，务必收集并提供完整的错误信息，以确保在启动新应用程序或新功能之前进行必要的更改。

总之，在处理包含增强型错误代码的响应时，应考虑以下事项：

1. **检查两个状态值**：始终检查HTTP响应状态代码和增强型错误代码“状态”字段。 它们可能有所不同，并且都提供了宝贵的信息。

1. **与顶级和项级错误信息无关**：处理与传递方式无关的顶级和项级错误信息，确保可以处理两种形式的传输增强型错误代码。

1. **重试逻辑**：对于需要重试的错误，请确保通过指数回退完成重试，以避免使服务器不堪重负。 此外，如果是Adobe Pass身份验证API同时处理多个项目（例如，预授权API），您应在重复请求中仅包含标记为“重试”的项目，而不包含整个列表。

1. **配置更改**：对于需要配置更改的错误，请确保在启动新应用程序或新功能之前进行必要的更改。

1. **身份验证和授权**：对于与身份验证和授权相关的错误，必须提示用户根据需要重新身份验证或获取新授权。

1. **用户反馈**：可选，使用人类可读的“消息”和（可能的）“详细信息”字段通知用户该问题。 在应用降级规则时，可能会从MVPD预授权或授权端点或程序员传递“详细信息”短信。
