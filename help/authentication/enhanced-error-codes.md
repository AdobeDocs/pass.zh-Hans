---
title: 增强的错误代码
description: 增强的错误代码
exl-id: 2b0a9095-206b-4dc7-ab9e-e34abf4d359c
source-git-commit: 40aeba47c293f2cc4569f01a6fb1ca4bfbcd0de6
workflow-type: tm+mt
source-wordcount: '2299'
ht-degree: 2%

---

# 增强的错误代码 {#enhanced-error-codes}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## 概述 {#overview}

本文档介绍了API错误代码列表以及返回给应用程序的其他错误信息。

要在程序员应用程序中使用增强型错误代码，需要向支持团队发出请求，以便在更改配置的情况下启用支持团队。

## 响应错误处理 {#response-error-handling}

在大多数情况下，Adobe Pass身份验证API在响应正文中包含其他错误信息，以便 **有意义的上下文** 说明发生特定错误的原因和/或自动修复问题的可能解决方案。  *但是，在涉及身份验证或注销流的特定情况下，Adobe Pass身份验证服务可能会返回HTML响应或空正文 — 有关详细信息，请查看API文档。*

虽然某些类型的错误可以自动处理（例如，在网络超时的情况下重试授权请求，或者在会话过期时要求用户重新进行身份验证），但其他类型可能需要配置更改或客户关怀团队交互。 在这种情况下，程序员必须收集并提供完整的错误信息。

Adobe Pass身份验证API返回400-500范围内的HTTP状态代码，以指示故障或错误。 每个HTTP状态代码都具有特定含义：

- 4xx错误代码暗示错误是由客户端生成的，客户端需要执行额外的操作来修复它（例如，在调用受保护的API或提供任何所需的参数之前获取访问令牌）

- 5xx错误代码表示该错误是由服务器生成的，服务器需要执行额外的操作来修复它。

其他错误信息包含在响应正文的“error”字段中。

<table>
<thead>
  <tr>
    <th>名称</th>
    <th>类型</th>
    <th>示例</th>
    <th>描述</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>错误</td>
    <td><i>对象</i></td>
    <td><strong>JSON</strong>
    <br>
    <code>{<br>&nbsp;&nbsp;&nbsp;&nbsp;"status" : 403,<br>&nbsp;&nbsp;&nbsp;&nbsp;"code" : "network_connection_failure",<br>&nbsp;&nbsp;&nbsp;&nbsp;"message" : "Unable to contact your TV provider<br>&nbsp;&nbsp;&nbsp;&nbsp;services",<br>&nbsp;&nbsp;&nbsp;&nbsp;"helpUrl" : "https://tve.helpdocsonline.com/errors<br>&nbsp;&nbsp;&nbsp;&nbsp;/network_connection_failure",<br>&nbsp;&nbsp;&nbsp;&nbsp;"trace" : "12f6fef9-d2e0-422b-a9d7-60d799abe353",<br>&nbsp;&nbsp;&nbsp;&nbsp;"action" : "retry"<br>}
    </code>
    <p>
    <p>
    <strong>XML</strong>
    <br>
    <code>&lt;error&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;status&gt;403&lt;/status&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;code&gt;network_connection_failure&lt;/code&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;message&gt;Unable to contact your TV provider services&lt;/message&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;helpUrl&gt;https://tve.helpdocsonline.com/errors/network_connection_failure&lt;/helpUrl&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;trace>12f6fef9-d2e0-422b-a9d7-60d799abe353&lt;/trace&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;action>retry&lt;/action&gt;<br>&lt;/error&gt;
    </code>
    </td>
    <td>是指在尝试完成请求时收集的集合或错误对象。</td>
  </tr>
</tbody>
</table>

</br>

处理多个项目的Adobe Pass API（预授权API等）可能会指示处理特定项目是否失败，并使用项目级别错误信息处理其他项目是否成功。 在本例中， ***&quot;error&quot;*** 对象位于项目级别，并且响应正文可能包含多个 ***&quot;errors&quot;*** 对象 — 请参阅API文档。

</br>

**部分成功和项目级错误的示例**

```json
{
   "resources" : [
        {
            "id" : "TestStream1",
            "authorized" : true
        },
        {
            "id" : "TestStream2",
            "authorized" : false,
            "error" : {
 
               "status" : 403,
               "code" : "network_connection_failure",
               "message" : "Unable to contact your TV provider services",
               "details" : "",
               "helpUrl" : "https://tve.helpdocsonline.com/errors/network_connection_failure",
               "trace" : "8bcb17f9-b172-47d2-86d9-3eb146eba85e",
               "action" : "retry"
            }
 
        }
    ]
}
```

</br>

每个错误对象都有以下参数：

| 名称 | 类型 | 示例 | 受限 | 描述 |
|---|---|----|:---:|---|
| *状态* | *整数* | *403* | 检查(&amp;C)； | RFC 7231中记录的响应HTTP状态代码(<https://tools.ietf.org/html/rfc7231#section-6>) <ul><li>400错误请求</li><li>401未授权</li><li>403禁止访问</li><li>404未找到</li><li>405不允许使用该方法</li><li>409冲突</li><li>410不存在</li><li>412先决条件失败</li><li>429请求过多</li><li>500间隔服务器错误</li><li>503服务不可用</li></ul> |
| *代码* | *字符串* | *network_connection_failure* | 检查(&amp;C)； | 标准Adobe Pass身份验证错误代码。 下面包含完整的错误代码列表。 |
| *message* | *字符串* | *无法联系您的电视提供商服务* | | 易于用户识别的消息，可向最终用户显示。 |
| *详细信息* | *字符串* | *您的订阅包不包含“实时”渠道* | | 在某些情况下，MVPD授权端点或程序员通过降级规则提供了详细消息。 <p> 请注意，如果未从合作伙伴服务收到自定义消息，则错误字段中可能不存在此字段。 |
| *helpUrl* | *url* | &quot;`http://`&quot; | | 一个URL，可链接到有关此错误发生原因和可能解决方案的更多信息。 <p>URI表示绝对URL，不应从错误代码推断。 根据错误上下文，可以提供不同的URL。 例如，相同的bad_request错误代码将为身份验证和授权服务生成不同的url。 |
| *trace* | *字符串* | *12f6fef9-d2e0-422b-a9d7-60d799abe353* | | 此响应的唯一标识符，在联系支持人员以识别更复杂场景中的特定问题时，可以使用该标识符。 |
| *操作* | *字符串* | *重试* | 检查(&amp;C)； | 建议采取的补救措施： <ul><li> *无*  — 很遗憾，没有预定义操作来修复此问题。 这可能表明对公共API的调用不正确</li><li>*配置*  — 需要通过TVE仪表板或联系支持部门来更改配置。 </li><li>*application-register*  — 应用程序必须重新注册自身。 </li><li>*身份验证*  — 用户必须验证或重新验证。 </li><li>*授权*  — 用户必须获得特定资源的授权。 </li><li>*降解*  — 应该应用某种形式的降级。 </li><li>*重试*  — 重试请求可能会解决此问题</li><li>*重试之后*  — 在指定的时间段后重试请求可能会解决问题。</li></ul> |

</br>

**注释：**

- ***受限*** 列 *指示相应的字段值是否表示有限集* (例如&#39;&#39;的现有HTTP状态代码&#x200B;*状态*”字段)。 此规范的未来更新可能会向受限列表添加值，但不会移除或更改现有值。 无限制字段通常可以包含任何数据，但为确保合理的大小，可能存在限制。

- 每个Adobe响应将包含一个“Adobe请求ID”，用于标识整个HTTP服务中的客户端请求。 “**trace**”字段是对该报告的补充，它们应一起报告。

## HTTP状态代码和错误代码 {#http-status-codes-and-error-codes}

由于与旧版sdk和应用程序(例如， *未知\_application* 生成400个错误请求，而 *未知\_software\_statement* 产生401（未授权）。 解决这些不一致将在以后的迭代中定向。

## 操作和错误代码 {#actions-and-error-codes}

对于大多数错误代码，多个操作可以作为修复手头问题的途径，甚至可能需要多个操作才能自动修复它们。 我们选择指示修复错误概率最高的那一个。 此 **操作** 可以分为三类：

1. 尝试修复请求上下文（重试、后重试）的服务器
1. 尝试修复应用程序中的用户上下文（应用程序注册、身份验证、授权）的用户
1. 尝试修复应用程序和身份提供者之间的集成上下文（配置、降级）的用户

对于第一个类别（重试和在此之后重试），仅重试同一请求可能足以解决此问题。 如果API处理多个项目，则应用程序应重复请求并仅包含那些具有“重试”或“在此之后重试”操作的项目。 对于&quot;*重试之后*“操作，a ”<u>重试之后</u>”标题将指示应用程序在重复请求之前应等待的秒数。

对于第2类和第3类，实际操作实施在很大程度上取决于应用程序功能。 例如，“*降解*“可以实施为”切换到15分钟临时回放以允许用户播放内容“或”实施为“自动工具应用AUTHN-ALL或AUTHZ-ALL降级以将其与指定的MVPD集成”。 类似于&quot;*身份验证*”操作可以在平板电脑上触发被动身份验证（后通道身份验证），并在连接的电视上触发完整的第二屏身份验证流程。 因此，我们的确选择提供包含架构和所有参数的完全成熟的URL。

## 错误代码 {#error-codes}

下面的错误表列出了可能的错误代码、相关消息和可能的操作。

| 操作 | 错误代码 | HTTP状态代码 | 描述 |
|---|---|---|---|
| **无** | *authorization_denied_by_mvpd* | 403 | MVPD在请求指定资源的授权时返回了“拒绝”决定。 |
|  | *authorization_denied_by_parental_controls* | 403 | 由于指定资源的家长控制设置，MVPD返回了“拒绝”决定。 |
|  | *authorization_denied_by_programmer* | 403 | 程序员应用的降级规则强制当前用户作出“拒绝”决定。 |
|  | *bad_request* | 400 | API请求无效或格式不正确。 查看API文档以确定请求要求。 |
|  | *individualization_service_unavailable* | 503 | 由于个性化服务不可用，请求失败。 |
|  | *internal_error* | 500 | 由于内部服务器错误，请求失败。 |
|  | *invalid_client_time* | 400 | 客户端计算机日期/时间/时区设置不正确。 这可能会导致身份验证/授权错误。 |
|  | *invalid_custom_scheme* | 400 | 无法识别应用程序注册中使用的指定自定义方案。 请检查TVE仪表板配置以了解正确的自定义方案值。 |
|  | *invalid_domain* | 400 | 请求者正在使用无效域。 特定请求者ID使用的所有域需要按Adobe列入白名单。 |
|  | *无效标头* | 400 | 请求失败，因为它包含无效标头。 查看API文档，确定哪些标头对您的请求有效，以及它们的值是否存在任何限制。 |
|  | *invalid_http_method* | 405 | 不支持与请求关联的HTTP方法。 查看API文档，确定请求支持的HTTP方法。 |
|  | *invalid_parameter_value* | 400 | 请求失败，因为它包含无效参数或参数值。 查看API文档，确定哪些参数对于您的请求有效，以及它们的值是否存在任何限制。 |
|  | *invalid_resource_value* | 400 | 由于使用了无效或格式错误的资源，请求失败。 查看API文档，确定必须如何针对请求对复杂资源进行编码，以及它们的值和/或大小是否存在任何限制。 |
|  | *invalid_registration_code* | 404 | 指定的注册码不再有效或已过期。 |
|  | *invalid_service_configuration* | 500 | 由于服务配置不正确，请求失败。 |
|  | *missing_authentication_header* | 400 | 请求失败，因为它不包含特定API所需的身份验证标头。 |
|  | *missing_resource_mapping* | 400 | 指定的资源没有相应的映射。 联系支持人员以修复所需的映射。 |
|  | *preauthorization_denied_by_mvpd* | 403 | MVPD在请求指定资源的预授权时返回了“拒绝”决定。 |
|  | *preauthorization_denied_by_programmer* | 403 | 程序员应用的降级规则可为当前用户强制执行“拒绝”决定。 |
|  | *registration_code_service_unavailable* | 503 | 请求失败，因为注册码服务不可用。 |
|  | *service_available* | 503 | 由于身份验证或授权服务不可用，请求失败。 |
|  | *access_token_unavailable* | 400 | 由于检索访问令牌时出现意外错误，请求失败。 请检查TVE仪表板配置，了解可用的软件语句和注册的自定义方案。 |
|  | *unsupported_client_version* | 400 | 此版本的Adobe Pass Authentication SDK太旧，不再受支持。 请查看API文档，了解升级到最新版本所需的步骤。 |
| **配置** | *network_required_ssl* | 403 | 目标合作伙伴服务存在SSL连接问题。 请联系支持团队。 |
|  | *太多资源* | 403 | 授权或预授权请求失败，因为查询的资源过多。 请联系支持团队以正确配置授权和预授权限制。 |
|  | *未知程序员* | 400 | 无法识别程序员或服务提供商。 使用TVE仪表板注册指定的程序员。 |
|  | *未知应用程序* | 400 | 无法识别应用程序。 使用TVE仪表板注册指定的应用程序。 |
|  | *未知集成* | 400 | 指定的程序员和标识提供程序之间的集成不存在。 使用TVE功能板创建所需的集成。 |
|  | *unknown_software_statement* | 401 | 无法识别与访问令牌关联的软件语句。 请与支持团队联系，以澄清软件声明的状态。 |
| **application-register** | *access_token_expired* | 401 | 访问令牌已过期。 应用程序应刷新API文档中所述的访问令牌。 |
|  | *invalid_access_token_signature* | 401 | 访问令牌签名不再有效。 应用程序应刷新API文档中所述的访问令牌。 |
|  | *invalid_client_id* | 401 | 无法识别关联的客户端标识符。 应用程序应遵循API文档中所述的应用程序注册流程。 |
| **身份验证** | *authentication_session_expired* | 410 | 当前的身份验证会话已过期。 用户必须使用支持的MVPD重新进行身份验证才能继续。 |
|  | *authentication_session_missing* | 401 | 无法检索与此请求关联的身份验证会话。 用户必须使用支持的MVPD重新进行身份验证才能继续。 |
|  | *authentication_session_invalid* | 401 | 身份验证会话被身份提供程序失效。 用户必须使用支持的MVPD重新进行身份验证才能继续。 |
|  | *authentication_session_issuer_mismatch* | 400 | 由于为授权流指定的MVPD与发出身份验证会话的MVPD不同，授权请求失败。 用户必须使用所需的MVPD重新进行身份验证才能继续。 |
|  | *authorization_denied_by_hba_policies* | 403 | 由于基于主的身份验证策略，MVPD已返回“拒绝”决策。 当前身份验证是使用基于主目录的身份验证流程(HBA)获得的，但在请求指定资源的授权时，设备不再位于主目录。 用户必须使用支持的MVPD重新进行身份验证才能继续。 |
|  | *identity_not_recovered_by_mvpd* | 403 | 由于MVPD无法识别用户身份，授权请求失败。 |
| **授权** | *authorization_expired* | 410 | 指定资源的先前授权已过期。 用户必须获得新授权才能继续。 |
|  | *authorization_not_found* | 404 | 未找到指定资源的授权。 用户必须获得新授权才能继续。 |
|  | *device_identifier_mismatch* | 403 | 指定的设备标识符与授权设备标识不匹配。 用户必须获得新授权才能继续。 |
| **重试** | *network_connection_failure* | 403 | 与关联的合作伙伴服务连接失败。 重试请求可能会解决此问题。 |
|  | *network_connection_timeout* | 403 | 与关联的合作伙伴服务的连接超时。 重试请求可能会解决此问题。 |
|  | *network_received_error* | 403 | 从关联的合作伙伴服务检索响应时出现读取错误。 重试请求可能会解决此问题。 |
|  | *maximum_execution_time_exceeded* | 403 | 请求未在允许的最长时间内完成。 重试请求可能会解决此问题。 |
| **重试之后** | *太多请求* | 429 | 在给定时间间隔内发送的请求过多。 应用程序可以在建议的时间段后重试请求。 |
|  | *user_rate_limit_exceeded* | 429 | 特定用户在指定时间间隔内发出的请求过多。 应用程序可以在建议的时间段后重试请求。 |
