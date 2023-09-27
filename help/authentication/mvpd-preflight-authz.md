---
title: MVPD预检授权
description: MVPD预检授权
exl-id: da2e7150-b6a8-42f3-9930-4bc846c7eee9
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '745'
ht-degree: 0%

---

# MVPD预检授权

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## 介绍 {#mvpd-preflight-authz-intro}

“预检授权”是对多个资源的轻量级授权检查。 程序员主要使用它来装饰他们的UI（例如，使用锁定和解锁图标指示访问状态）。

Adobe Pass身份验证当前可以通过两种方式为MVPD支持预检授权，即通过AuthN响应属性或通过多渠道AuthZ请求。  以下情形描述了实施预检授权的不同方法的成本/好处：

* **最佳用例方案** - MVPD在授权阶段（多通道AuthZ）提供预授权资源的列表。
* **最坏情况**  — 如果MVPD不支持任何形式的多个资源授权，则Adobe Pass身份验证服务器会针对资源列表中的每个资源对MVPD执行授权调用。 此方案会影响（与资源数量成比例）预检授权请求的响应时间。 它可能会增加Adobe和MVPD服务器上的负载，从而导致性能问题。 此外，它还会生成授权请求/响应事件，而无需实际进行播放。
* **已弃用** - MVPD在身份验证阶段提供预授权资源的列表，因此不需要网络调用，甚至不需要预检请求，因为列表已缓存在客户端上。

虽然MVPD不必支持预检授权，但以下部分介绍了Adobe Pass身份验证在回退到上述最坏情况之前可以支持的一些预检授权方法。

## 在AuthN预检 {#preflight-authn}

此预检方案与OLCA兼容(Cableabs)。 身份验证和授权接口1.0规范部分7.5.2中标题为“身份验证断言中的属性语句”，它描述了SAML身份验证响应如何包含预授权资源的列表。 如果IdP支持此项，则Adobe Pass身份验证服务器将能够在身份验证时生成预定义的资源列表，并将其与身份验证令牌一起缓存到客户端上。 此方法还可以实现最佳方案，并且当程序员调用checkPreauthorizedResources()时，不会执行任何网络调用，因为客户端上已经存在所有内容。

### SAML Attribute语句中的自定义资源列表 {#custom-res-saml-attr}

IdP的SAML身份验证响应应包含包含AdobePass应授权的资源名称的AttributeStatement。  有些MVPD会按照以下格式提供此内容：

```XML
<saml:AttributeStatement>
  <saml:Attribute Name="authorized_resources">
    <saml:AttributeValue>MMOD</saml:AttributeValue>
    <saml:AttributeValue>Olympics2012</saml:AttributeValue>
  </saml:Attribute>
</saml:AttributeStatement>
```

上述示例显示了一个列表，其中包含两个预授权的资源：“MMOD”和“Oppornimum2012”。

这有效地实现了最佳方案，并且当程序员调用checkPreauthorizedResources()时，不会执行任何网络调用，因为客户端上已经存在所有内容。

## AuthZ中的多渠道预检 {#preflight-multich-authz}

此预检实施也与OLCA兼容(Cablelab)。  身份验证和授权接口1.0规范（7.5.3和7.5.4节）描述了使用SAML断言或XACML从MVPD请求授权信息的方法。 这是查询授权状态的推荐方法，这些MVPD不支持将授权状态作为身份验证流的一部分。 Adobe Pass身份验证会向MVPD发出单个网络调用，以检索授权资源的列表。


Adobe Pass身份验证从程序员的应用程序中接收资源列表。 Adobe Pass Authentication的MVPD集成随后可以发起一个AuthZ调用（包含所有这些资源），然后解析响应并提取多个允许/拒绝决策。  使用多通道AuthZ场景的预检流程的工作方式如下：

1. 程序员的应用程序通过预检客户端API发送一个逗号分隔的资源列表，例如：“TestChannel1，TestChannel2，TestChannel3”。
1. MVPD预检AuthZ请求调用包含多个资源，并具有以下结构：

```XML
<?xml version="1.0" encoding="UTF-8"?><soap11:Envelope xmlns:soap11="http://schemas.xmlsoap.org/soap/envelope/"> 
<soap11:Header/> 
<soap11:Body> 
  <xacml-samlp:XACMLAuthzDecisionQuery xmlns:xacml-samlp="urn:oasis:names:tc:xacml:2.0:profile:saml2.0:v2:schema:protocol" 
                                       CombinePolicies="false" Destination="https://login.idpexmaple.net/" ID="_3576604f382455d6495f342d9e07b69c" 
                                       IssueInstant="2013-02-07T10:31:40.333Z" Version="2.0"> 
  <saml2:Issuer xmlns:saml2="urn:oasis:names:tc:SAML:2.0:assertion">https://saml.sp.auth-staging.adobe.com/on-behalf-of/TestDistributors</saml2:Issuer> 
  <xacml-context:Request xmlns:xacml-context="urn:oasis:names:tc:xacml:2.0:context:schema:os"> 
  <xacml-context:Subject SubjectCategory="urn:oasis:names:tc:xacml:1.0:subject-category:access-subject"> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:subject:subject-id" DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">VFZTAQEAABQCe[...]</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Subject> 
  <xacml-context:Resource> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id" DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">TestChannel1</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Resource> 
  <xacml-context:Resource> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id" 
                           DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">TestChannel2</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Resource> 
  <xacml-context:Resource> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id" 
                           DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                                xsi:type="xacml-context:AttributeValueType">TestChannel3</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Resource> 
  <xacml-context:Action> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:action:action-id" 
                           DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">VIEW</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Action> 
  <xacml-context:Environment> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:subject:authn-locality:ip-address" 
                           DataType="urn:oasis:names:tc:xacml:2.0:data-type:ipAddress"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">127.0.0.1</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Environment> 
  </xacml-context:Request> 
  </xacml-samlp:XACMLAuthzDecisionQuery> 
</soap11:Body> 
</soap11:Envelope>
```

## 多个资源的自定义授权 {#custom-authz}

某些MVPD具有授权端点，它们支持在一个请求中对多个资源进行授权，但它们不属于多通道AuthZ中所述的场景。 这些特定的MVPD需要自定义工作。

Adobe还可以支持多渠道授权，而无需更改现有实施。  这一方法需要在Adobe和MVPD技术团队之间审查，以确保它按预期工作。

## 支持预检授权的MVPD {#mvpds-supp-preflight-authz}

下表列出了支持预检授权的MVPD，以及它们支持的预检类型和已知限制：

| 预检方法 | MVPD | 注释 |
|:-------------------------------:|:--------------------------------------------------------------------------------------------------------:|:------------------------------------------------------------------:|
| 多通道AuthZ | Comcast AT&amp;T代理Clearleap Charter_Direct代理GLDS Rogers Verizon OSN Bell Sasktel Optimum AlticeOne |                                                                    |
| 用户元数据中的渠道排列 | Suddenlink HTC | 所有Synacor直接集成也可以支持此方法。 |
| 分支和联接 | 以上未列出的所有其他项 | 默认勾选的最大资源数= 5。 |

<!--
![RelatedInformation]
>* [Logout](/help/authentication/usecase-mvpd-logout.md)
>* [Authorization](/help/authentication/authz-usecase.md)
>* [MVPD Integration Features](/help/authentication/mvpd-integr-features.md)
>* [MVPD User Metadata Exchange](/help/authentication/mvpd-user-metadata-exchng.md)
>* [Preflight Authorization - Programmer Integration Guide](/help/authentication/preflight-authz.md)
>* [AuthN and AuthZ Interface 1.0 Specification](https://www.cablelabs.com/specifications/CL-SP-AUTH1.0-I04-120621.pdf){target=_blank} 
-->
