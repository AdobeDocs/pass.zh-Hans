---
title: 印前检查授权
description: 印前检查授权
exl-id: 036b1a8e-f2dc-4e9a-9eeb-0787e40c00d9
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1518'
ht-degree: 0%

---

# 印前检查授权 {#preflight-authorization}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

</br>

## 概述 {#overview}

此功能为多个资源提供轻量级授权检查。 此轻量级检查的目的是装饰UI（例如，使用锁定和解锁图标指示访问状态）。 预检授权尽可能轻便高效，以便单个API调用可以为资源列表生成授权状态。 请注意，此功能在授权资源方面不具有权威性。

A `getAuthorization(resource)` 或 `checkAuthorization(resource)` 在允许播放之前，仍必须调用。

印前检查授权还支持不同的用例，在该用例中，程序员需要请求对多个资源ID的授权，以允许播放媒体内容的一个项目。 程序员可以对所需资源进行初步预检检查，如果不满足业务条件，则根据响应情况，可能会提前失败。

有关支持预检授权的MVPD列表，请参见 [MVPD预检授权](/help/authentication/mvpd-preflight-authz.md#preflight_support_list) 页面。

>[!NOTE]
>
> 请注意，需要与Adobe和MVPD事先商定将此功能用于没有完全预检授权支持的MVPD。 在这些MVPD上使用预检授权将属于所描述的“最坏情况” [此处](/help/authentication/mvpd-preflight-authz.md#intro) 并可能导致性能问题和响应速度缓慢。 </br>
> 此外，请注意，与有关的预检授权的使用情况 **需要Adobe明确同意5个以上的资源**.

## AccessEnabler Preflight API {#AE_pre_api}

AccessEnabler公开API/回调函数对以实现预检授权。 API调用采用由资源列表组成的单个参数。 回调函数采用代表实际授权资源的单个参数。 以下示例在ActionScript中，但调用在AccessEnabler的所有风格中均可用。

### checkPreauthorizedResources(Array：resources)：void {#checkPreauthRes}

在AccessEnabler对象上调用此函数可请求资源列表的授权状态。

resources参数是应检查其授权的资源列表。 列表中的每个元素都应该是一个表示资源ID的字符串。 资源ID与中的资源ID须遵守相同的限制 `getAuthorization()` 调用，即程序员与MVPD或媒体RSS片段之间已商定的值。 请注意，Adobe Pass身份验证不会以任何方式管理资源，除了可以根据MVPD实际支持的内容转换资源格式的精简中继层。

### preauthorizedResources(Array：authorizedResources) {#preauthRes}

这是一个回调函数，必须在程序员的上层应用程序中实现。 AccessEnabler将在计算授权资源列表后调用此函数。


### JS示例

```javascript
    checkPreauthorizedResources(["CNBC","MSNBC"]);
    ...
    preauthorizedResources() {
        var resource = arguments[0];
        for(i in resources) {
            // Do things with resource list
            // such as decorate UI
            ...
        }
    }
```

## 实施信息 {#details}

- [使用ChannelID预检](#preflight_using_channelID)
- [POST/预授权](#post)
- [存储](#storage)

API调用会尝试在客户端的本地存储中找到当前用户的已授权资源的缓存列表。 如果没有缓存的列表，则会对AdobePass服务器进行HTTPS调用以检索该列表。

缓存机制通过完全跳过网络调用而缩短后续调用的性能时间。 此外，缓存的列表可在身份验证过程中预先填充。  (有关设置此方案的信息，请参阅 [Preflight授权集成](/help/authentication/authz-usecase.md#preflight_authz_int) （请参阅《MVPD集成指南》的授权部分）。

此外，缓存的资源列表可以潜在地用于优化授权流，其意义在于，如果存在缓存的资源列表， `checkAuthorization()` 可以在执行网络调用之前检查它。 如果资源不在预授权资源的列表中，则检查可能会失败，而无需调用Adobe Pass身份验证服务器。


### 使用ChannelID预检 {#preflight_using_channelID}

从Adobe Pass Authentication 2.4.1版本开始，Preflight流程的工作方式如下所示：

1. 在身份验证期间，Adobe Pass身份验证会读取 `channelIID` 元素，并使用此值设置 `authorizedResources` 元素。
1. 内部 `checkPreauthorizedResources()` API函数， Adobe Pass身份验证检查 `authorizedResources` 元素已设置。
1. 如果 `authorizedResources` 设置元素后，Adobe Pass身份验证会读取该值，并在资源列表之间执行交集，从 `authorizedResources` 元素和接收的资源列表 `checkPreauthorizedResources()` 参数。  此交集的结果是预授权资源的最终列表。
1. 如果 `authorizedResources` 未设置元素，请执行先前实现的流，其中接收自 `checkPreauthorizedResources()` 参数传递给PreAuthorizationServlet。 此servlet执行对MVPD端点的授权调用并返回预授权资源的列表。

### 使用ChannelID预检的示例

下面的示例显示了一个示例通道排列。 请注意，用于发送渠道列表的属性的名称可能因一个MVPD而异：

```XML
    <saml:Attribute Name="visible_channels" NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic">
        <saml:AttributeValue xsi:type="xs:string">MSNBC</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">CNBC</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">FBN</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">FNC</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">TNT</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">TBS</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">CNN</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">TRUTV</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">TOON</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">HBO</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">MAX</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">EPIXHD</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">BTN-BTN2GO</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">SPEED-SPEED2</saml:AttributeValue>
    </saml:Attribute>
```


此 `authorizedResources` authentication元素中的元素如下所示：

```JSON
    <authorizedResources>
        <authorizedResource resourceID="MSNBC"/>
        <authorizedResource resourceID="CNBC"/>
        <authorizedResource resourceID="FBN"/>
        <authorizedResource resourceID="FNC"/>
        <authorizedResource resourceID="TNT"/>
        <authorizedResource resourceID="TBS"/>
        <authorizedResource resourceID="CNN"/>
        <authorizedResource resourceID="TRUTV"/>
        <authorizedResource resourceID="TOON"/>
        <authorizedResource resourceID="HBO"/>
        <authorizedResource resourceID="MAX"/>
        <authorizedResource resourceID="EPIXHD"/>
        <authorizedResource resourceID="BTN-BTN2GO"/>
        <authorizedResource resourceID="SPEED-SPEED2"/>
    </authorizedResources>
```

程序员执行 `checkPreauthorizedResources()` API调用，传递以下参数列表：</span>

- “MSNBC”
- “FBN”
- “天通电视”
- &quot;fbc-fox&quot;

当前Preflight实现使用来自的资源的列表执行交集 `authorizedResources` 元素并返回此列表：

- “MSNBC”
- “FBN”
- “天通电视”



**注意：** 请注意，交叉点不区分大小写。



### POST/预授权 {#post}

当不存在缓存的授权资源列表时，此调用将由AccessEnabler自动执行。


#### 请求 {#req}

| 参数 | 类型 | 必填 | 描述 |
| --- | --- | --- | --- |
| `authentication_token` | 字符串 | 是 | 身份验证令牌。 |
| `resource_id` | 字符串 | 是 | 单个资源。 可以多次指定，一次指定给 `checkPreauthorizedResources()` API调用。 |


**注意：** 可以配置请求资源的最大数量。
**最大默认值为5。**


#### 响应 {#resp}

预授权servlet发回的响应具有以下格式：

```XML
    <?xml version="1.0" encoding="UTF-8"?>
    <resources>
      <resource>
        <id>TNT</id>
        <authorized>true</authorized>
      </resource>
      <resource>
        <id>TBS</id>
        <authorized>true</authorized>
      </resource>
      <resource>
        <id>CNN</id>
        <authorized>false</authorized>
      </resource>
    </resources>
```

### 存储 {#storage}

AccessEnabler从服务提供程序获得的预授权资源列表。 此资源列表：

- 与AuthN和AuthZ令牌一起存储
- 只要用户位于同一网站，或身份验证令牌过期之前有效
- 每次用户登陆新的Adobe Pass身份验证集成网站时都会重新检索

每个条目都包含用户预授权的资源ID。

例如：


| 资源ID | 已授权 |
| ----------- | ---------- |
| CNN | true |
| TNT | false |
| ... | ... |


此列表名为“预授权缓存”。

#### 流量 {#flow}

1. 程序员的应用程序/站点可制作 `checkPreauthorizedResources(resourceList)` 呼叫。
1. AccessEnabler验证授权资源的身份验证令牌：
   1. 如果身份验证令牌包含授权的资源 — 此列表是权威的，不应进行调用以获取此信息。 在身份验证令牌的授权资源列表中搜索resourceList中的资源，只有找到的资源才会返回 `preauthorizedResources()` 回调。
   1. 如果身份验证令牌不包含授权的资源 —  `resourceList` 会与预授权缓存中的资源列表进行比较。
      1. 如果列表包含相同的资源 — 这表示已调用服务器，并且响应已在预授权缓存中。 只有经过授权的资源才会返回 `preauthorizedResources()` 回调。
      1. 如果列表不包含相同的资源 — 客户端需要调用服务器，以获取resourceList中资源的授权状态。 将获取响应并将其存储在预授权缓存中，完全替换旧资源。 只有经过授权的资源才会返回 `preauthorizedResources()` 回调。


#### 列表检索 {#listRetrieve}

每当 `checkPreauthorizedResources()` 将会调用，验证授权的资源列表将针对预授权缓存进行检查。 如果列表包含同一组资源，则不会调用服务提供商，因为触发 `preauthorizedResources()` 回调已在缓存中。


#### logout() {#logout}

注销时会清空预授权缓存。


## 依赖关系 {#depends}

Preflight API的性能取决于特定的MVPD实施。  有关实施选项，请参阅 [Preflight授权集成](/help/authentication/authz-usecase.md#preflight_authz_int) ，请参阅MVPD集成指南的授权部分。


## 安全性 {#security}

客户端API可供所有程序员使用。

该实施使用HTTPS作为传输，但为了进行较轻的调用，不采用其他安全措施（无签名，无FAXS）。

**注意：** 请勿以权威方式使用此API来确定是否应授予用户访问受保护资源的权限。 此API的用途是UI修饰和/或预览业务决策。 此 `getAuthorization()` 和 `checkAuthorization()` 应始终在允许播放之前进行调用。


## 兼容性 {#compat}

AccessEnabler的所有版本都支持此功能：AS、JS、AIR、iOS、Android、Xbox（在第2屏身份验证流中）。

预检授权不支持包含CDATA部分的预授权资源。 当前预检系统的重点是支持信道级滤波。 包含CDATA部分的资源可能是资产级别的资源。 Preflight支持简单 `mrss` 用于渠道级别预授权的资源，只要它们不包含CDATA。

## 与其他功能集成 {#integ_w_other_features}

如果资源不在预授权资源列表中，则可能会对授权流进行可能的优化以便快速失败。

在此优化中，服务器预检端点与降级机制集成。

如果为MVPD和请求者设置了“AuthN All”规则，则预检端点将仅镜像回请求中收到的所有资源。

如果为请求中存在的至少一个资源启用了“AuthZ All”，则同样如此。

<!--
## Related Information

  - `checkPreauthorizedResources()`
      - [iOS](#checkPreauth)
      - [Android](#checkPreauth)
      - [JavaScript](#checkPreauthRes)
      - [ActionScript](#checkPreauthRes)
  - [Xbox](#2nd%20Screen%20Authentication)
  - [MVPD Integration Guide: Preflight AuthZ](#)
-->
