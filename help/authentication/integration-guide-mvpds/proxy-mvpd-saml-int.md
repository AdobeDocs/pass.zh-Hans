---
title: 代理MVPD SAML集成
description: 代理MVPD SAML集成
exl-id: 6c83e703-d8cd-476b-8514-05b8230902be
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '711'
ht-degree: 0%

---

# 代理MVPD SAML集成

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## 概述 {#overview-proxy-mvpd-saml-int}

本文档介绍了用于代理集成的SAML身份验证流程。  这些流依赖于Adobe Pass身份验证服务器配置中存在的代理配置数据。 代理MVPD通过Adobe Pass身份验证代理Web服务将其代理配置数据推送到Adobe Pass身份验证服务器。

## 代理配置数据 {#proxy-config-data}

每个MVPD代理都向Adobe Pass身份验证代理Web服务提供其代理MVPD的代理配置数据。  代理Web服务文档中涵盖的这些的详细信息。   为了使SAML AuthN流正常工作，代理配置数据需要包含以下属性：

| 属性 | 描述 |
|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| MVPD ID | 表示代理的MVPD在内部到Adobe Pass身份验证的字符串。  在Adobe Pass身份验证的上下文中将由Adobe确认为唯一。 |
| MVPD默认徽标URL | 可在用户的MVPD选择器体验中显示的徽标的URL。  应使用透明背景。 |
| MVPD显示名称 | 用作显示名称文本（可能作为替换文本）的字符串，该文本可以与徽标一起显示。 |



## SAML集成流 {#saml-int-flows}

当MVPD订阅者访问程序员的网站或应用程序时，Adobe Pass身份验证使用为该程序员激活的MVPD列表来响应来自网站或应用程序的API调用。  集成可以是直接的或代理的；对于程序员来说，它们之间没有区别。 这允许程序员以他们认为合适的任何方式显示活动MVPD列表。 订户选择其MVPD，Adobe Pass身份验证将订户重定向到MVPD的特定身份提供者。

在集成的MVPD代理的情况下，会在Adobe Pass身份验证和MVPD代理之间完成集成。 Adobe Pass身份验证将用户身份验证请求发送到MVPD代理，而MVPD代理处理重定向。 为了让MVPD代理知道从何处重定向用户身份验证请求，Adobe Pass身份验证在SAML身份验证请求中发送MVPD标识符。  此标识符是由代理提供程序通过上述代理Web服务指定的MVPD ID。

### 身份验证 {#authn-saml-int}

为了使Adobe Pass身份验证与代理MVPD集成，需要满足以下条件：

* 代理MVPD提供了已代理MVPD的列表，已推送到Adobe代理Web服务

* 父MVPD代理的SAML元数据

* （推荐） — 代理MVPD处理对代理MVPD的登录页面URL的其他重定向

* MVPD代理需要为以下IP打开端口443和80：
   * 192.150.4.5
   * 192.150.10.200
   * 192.150.11.4
   * 4.53.93.130
   * 193.105.140.131
   * 193.105.140.132
   * 76.74.170.204
   * 63.140.39.4
   * 66.235.132.38
   * 66.235.139.38
   * 66.235.139.168


#### 身份验证SAML请求和响应 {#authn-saml-req-resp}

在SAML AuthN请求中，代理集成包含以下需要由MVPD代理处理的其他属性。  要代表代理的MVPD正确处理请求者并呈现正确的登录体验，此属性是必需的。 （此属性在下面的示例请求中突出显示。）

**作用域属性** — 包含包含特定MVPD_ID和MVPD名称的IDPEntry项。  它表示用户实际从程序员的选取器中选择的MVPD，并且与代理Web服务中指定的MVPD_ID匹配。

RequestorID还有一个作用域属性，可用于自定义程序员特定品牌的登录（如果需要）。 或者，它可以仅用于分析请求的来源。

在SAML AuthN响应中，代理MVPD应在以下属性中将代理MVPD指定为IdP实体：

* SAML颁发者
* 名称限定符


**示例AuthN请求**

```XML
<samlp:AuthnRequest
  AssertionConsumerServiceURL="https://sp.auth-staging.adobe.com/sp/saml/SAMLAssertionConsumer"
  Destination="DESTIONATION_URL"
  ForceAuthn="false"
  ID="_4cb70308-b445-462e-b044-f7d0323dde0c"
  IsPassive="false"
  IssueInstant="2012-04-03T15:41:25.884Z"
  ProtocolBinding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST"
  Version="2.0"
  xmlns:ds="http://www.w3.org/2000/09/xmldsig#"
  xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">
      https://saml.sp.auth-staging.adobe.com
    </saml:Issuer>
    <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
        ...........
    </ds:Signature>
    <samlp:NameIDPolicy AllowCreate="true"
                        Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"
                        SPNameQualifier="https://saml.sp.auth-staging.adobe.com"
                        xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol" />
    <samlp:Scoping xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
        <samlp:IDPList xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
            <samlp:IDPEntry Name="MVPD NAME" ProviderID="MVPD_ID"/>
        </samlp:IDPList>
        <samlp:RequesterID xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
          RequestorID-Value
        </samlp:RequesterID>
    </samlp:Scoping>
</samlp:AuthnRequest>
```


**示例AuthN响应**

```XML
<samlp:Response Destination="https://sp.auth-staging.adobe.com/sp/saml/SAMLAssertionConsumer"
                ID="_1d39be60-66de-012f-bfd5-0030488a31a4"
                InResponseTo="_4cb70308-b445-462e-b044-f7d0323dde0c"
                IssueInstant="2012-04-12T15:00:06Z"
                Version="2.0"
                xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol" >
    <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">ISSUER_VALUE</saml:Issuer>
    <samlp:Status>
        <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success"/>
    </samlp:Status>
    <saml:Assertion ID="_1d39c280-66de-012f-bfd6-0030488a31a4"
                    IssueInstant="2012-04-12T15:00:06Z"
                    Version="2.0"
                    xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion" >
        <saml:Issuer>ISSUER_VALUE</saml:Issuer>
        <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
            ...........
        </ds:Signature>
        <saml:Subject>
            <saml:NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"
                         NameQualifier="IDP_NameQualifier"
                         SPNameQualifier="https://saml.sp.auth-staging.adobe.com">
                oRD6ALr5jlzkofNR1OaSCDbC6GaXV1cq8gF7Eotf
            </saml:NameID>
            <saml:SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
                <saml:SubjectConfirmationData
                  InResponseTo="_4cb70308-b445-462e-b044-f7d0323dde0c"
                  NotOnOrAfter="2012-04-12T15:10:06Z"
                  Recipient="https://sp.auth-staging.adobe.com/sp/saml/SAMLAssertionConsumer" />
            </saml:SubjectConfirmation>
        </saml:Subject>
        <saml:Conditions NotBefore="2012-04-12T15:00:06Z"
                         NotOnOrAfter="2012-04-12T15:10:06Z">
            <saml:AudienceRestriction>
                <saml:Audience>https://saml.sp.auth-staging.adobe.com</saml:Audience>
            </saml:AudienceRestriction>
        </saml:Conditions>
        <saml:AuthnStatement AuthnInstant="2012-04-12T15:00:06Z"
                             SessionIndex="f6d15540cf27966115028d35c94eefb9" >
            <saml:AuthnContext>
                <saml:AuthnContextClassRef>urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport
                </saml:AuthnContextClassRef>
            </saml:AuthnContext>
        </saml:AuthnStatement>
    </saml:Assertion>
</samlp:Response>
```

### 授权 {#authz-proxy-mvpd-saml-int}

对于授权部分，MVPD需要接受由程序员指定的资源进行授权。  在大多数情况下，这是信道网络的字符串标识符，如TBS或TNT。

#### 授权SAML请求和响应 {#authz-saml-req-resp}

在AuthZ响应中，ISSUER必须匹配SAML响应中的ISSUER，它应该是代理的MVPD标识符。

**示例AuthZ XACML请求**

```XML
<?xml version="1.0" encoding="UTF-8"?>
<soap11:Envelope xmlns:soap11="http://schemas.xmlsoap.org/soap/envelope/">
<soap11:Header/>
<soap11:Body>
    <xacml-samlp:XACMLAuthzDecisionQuery
            xmlns:xacml-samlp="urn:oasis:names:tc:xacml:2.0:profile:saml2.0:v2:schema:protocol"
            ID="_c2346a8f2c9cfb205b6b8bf12c2db4d0" IssueInstant="2012-04-12T15:07:51.280Z" Version="2.0">
        <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">https://saml.sp.auth-staging.adobe.com
        </saml:Issuer>
        <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
            <ds:SignedInfo>
                <ds:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
                <ds:SignatureMethod Algorithm="http://www.w3.org/2000/09/xmldsig#rsa-sha1"/>
                <ds:Reference URI="#_c2346a8f2c9cfb205b6b8bf12c2db4d0">
                    <ds:Transforms>
                        <ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature"/>
                        <ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#">
                            <ec:InclusiveNamespaces xmlns:ec="http://www.w3.org/2001/10/xml-exc-c14n#"
                                                    PrefixList="ds saml xacml-context xacml-samlp"/>
                        </ds:Transform>
                    </ds:Transforms>
                    <ds:DigestMethod Algorithm="http://www.w3.org/2000/09/xmldsig#sha1"/>
                    <ds:DigestValue>GmEkSZI+SDS1i4vV2ApGh0mx1X4=</ds:DigestValue>
                </ds:Reference>
            </ds:SignedInfo>
            <ds:SignatureValue>..........</ds:SignatureValue>
        </ds:Signature>
        <xacml-context:Request xmlns:xacml-context="urn:oasis:names:tc:xacml:2.0:context:schema:os">          
            <xacml-context:Subject
              SubjectCategory="urn:oasis:names:tc:xacml:1.0:subject-category:access-subject">
                <xacml-context:Attribute
                  AttributeId="urn:oasis:names:tc:xacml:1.0:subject:subject-id"
                  DataType="http://www.w3.org/2001/XMLSchema#string">
                    <xacml-context:AttributeValue
                      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                      xsi:type="xacml-context:AttributeValueType">
                        oRD6ALr5jlzkofNR1OaSCDbC6GaXV1cq8gF7Eotf
                    </xacml-context:AttributeValue>
                </xacml-context:Attribute>
            </xacml-context:Subject>
            <xacml-context:Resource>
                <xacml-context:Attribute
                  AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id"
                  DataType="http://www.w3.org/2001/XMLSchema#string">
                    <xacml-context:AttributeValue
                      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                      xsi:type="xacml-context:AttributeValueType">TBS
                    </xacml-context:AttributeValue>
                </xacml-context:Attribute>
            </xacml-context:Resource>
            <xacml-context:Action>
                <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:action:action-id"
                                         DataType="http://www.w3.org/2001/XMLSchema#string">
                    <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                                                  xsi:type="xacml-context:AttributeValueType">VIEW
                    </xacml-context:AttributeValue>
                </xacml-context:Attribute>
            </xacml-context:Action>
            <xacml-context:Environment>
                <xacml-context:Attribute
                  AttributeId="urn:oasis:names:tc:xacml:1.0:subject:authn-locality:ip-address"
                  DataType="urn:oasis:names:tc:xacml:2.0:data-type:ipAddress">
                    <xacml-context:AttributeValue
                      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                      xsi:type="xacml-context:AttributeValueType">127.0.0.1
                    </xacml-context:AttributeValue>
                </xacml-context:Attribute>
            </xacml-context:Environment>
        </xacml-context:Request>
    </xacml-samlp:XACMLAuthzDecisionQuery>
</soap11:Body>
</soap11:Envelope>
```

**示例AuthZ XACML响应（已授予授权）**

```XML
<?xml version="1.0" encoding="UTF-8"?>
<soap-env:Envelope xmlns:soap-env="http://schemas.xmlsoap.org/soap/envelope/">
    <soap-env:Body>
        <samlp:Response xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol" ID="_311fa030-66df-012f-bfd7-0030488a31a4"
                        IssueInstant="2012-04-12T15:07:49Z" Version="2.0">
            <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">ISSUER</saml:Issuer>
            <samlp:Status>
                <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success"/>
            </samlp:Status>
            <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
                <ds:SignedInfo>
                    <ds:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
                    <ds:SignatureMethod Algorithm="http://www.w3.org/2000/09/xmldsig#rsa-sha1"/>
                    <ds:Reference URI="#_311fa030-66df-012f-bfd7-0030488a31a4">
                        <ds:Transforms>
                            <ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature"/>
                            <ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
                        </ds:Transforms>
                        <ds:DigestMethod Algorithm="http://www.w3.org/2000/09/xmldsig#sha1"/>
                        <ds:DigestValue>2+fDBPYnOT1w5dufJZoVsgckRkM=</ds:DigestValue>
                    </ds:Reference>
                </ds:SignedInfo>
                <ds:SignatureValue>..........</ds:SignatureValue>
                <ds:KeyInfo>
                    <ds:X509Data>
                        <ds:X509Certificate>.........</ds:X509Certificate>
                    </ds:X509Data>
                </ds:KeyInfo>
            </ds:Signature>
            <xacml-samlp:Assertion xmlns:xacml-samlp="urn:oasis:names:tc:SAML:2.0:assertion"
                                   ID="_311fa5a0-66df-012f-bfd8-0030488a31a4" IssueInstant="2012-04-12T15:07:49Z"
                                   Version="2.0">
                <xacml-samlp:Issuer>ISSUER</xacml-samlp:Issuer>
                <xacml-samlp:Conditions NotBefore="2012-04-12T15:07:49Z" NotOnOrAfter="2012-04-13T15:07:49Z">
                    <xacml-samlp:AudienceRestriction>
                        <xacml-samlp:Audience>https://saml.sp.auth-staging.adobe.com</xacml-samlp:Audience>
                    </xacml-samlp:AudienceRestriction>
                </xacml-samlp:Conditions>
                <xacml-saml:XACMLAuthzDecisionStatement
                        xmlns:xacml-saml="urn:oasis:names:tc:xacml:2.0:profile:saml2.0:v2:schema:assertion"
                        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
                    <xacml-context:Response xmlns:xacml-context="urn:oasis:names:tc:xacml:2.0:context:schema:os">
                        <xacml-context:Result ResourceId="TBS">
                            <xacml-context:Decision>Permit</xacml-context:Decision>
                        </xacml-context:Result>
                    </xacml-context:Response>
                </xacml-saml:XACMLAuthzDecisionStatement>
            </xacml-samlp:Assertion>
        </samlp:Response>
    </soap-env:Body>
</soap-env:Envelope>
```

**示例AuthZ XACML响应（授权被拒绝）**

```XML
<?xml version="1.0" encoding="UTF-8"?>
<soap-env:Envelope xmlns:soap-env="http://schemas.xmlsoap.org/soap/envelope/">
<soap-env:Body>
    <samlp:Response xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol" ID="_69ed8d80-66df-012f-bfda-0030488a31a4"
                    IssueInstant="2012-04-12T15:09:24Z" Version="2.0">
        <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">ISSUER</saml:Issuer>
        <samlp:Status>
            <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success"/>
        </samlp:Status>
        <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
            <ds:SignedInfo>
                <ds:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
                <ds:SignatureMethod Algorithm="http://www.w3.org/2000/09/xmldsig#rsa-sha1"/>
                <ds:Reference URI="#_69ed8d80-66df-012f-bfda-0030488a31a4">
                    <ds:Transforms>
                        <ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature"/>
                        <ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
                    </ds:Transforms>
                    <ds:DigestMethod Algorithm="http://www.w3.org/2000/09/xmldsig#sha1"/>
                    <ds:DigestValue>2SXNFA4pb/283wq5FVQdp4Ms5SQ=</ds:DigestValue>
                </ds:Reference>
            </ds:SignedInfo>
            <ds:SignatureValue>........</ds:SignatureValue>
            <ds:KeyInfo>
                <ds:X509Data>
                    <ds:X509Certificate>........</ds:X509Certificate>
                </ds:X509Data>
            </ds:KeyInfo>
        </ds:Signature>
        <xacml-samlp:Assertion xmlns:xacml-samlp="urn:oasis:names:tc:SAML:2.0:assertion"
                               ID="_69ed91e0-66df-012f-bfdb-0030488a31a4" IssueInstant="2012-04-12T15:09:24Z"
                               Version="2.0">
            <xacml-samlp:Issuer>ISSUER</xacml-samlp:Issuer>
            <xacml-samlp:Conditions NotBefore="2012-04-12T15:09:24Z" NotOnOrAfter="2012-04-13T15:09:24Z">
                <xacml-samlp:AudienceRestriction>
                    <xacml-samlp:Audience>https://saml.sp.auth-staging.adobe.com</xacml-samlp:Audience>
                </xacml-samlp:AudienceRestriction>
            </xacml-samlp:Conditions>
            <xacml-saml:XACMLAuthzDecisionStatement
                    xmlns:xacml-saml="urn:oasis:names:tc:xacml:2.0:profile:saml2.0:v2:schema:assertion"
                    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
                <xacml-context:Response xmlns:xacml-context="urn:oasis:names:tc:xacml:2.0:context:schema:os">
                    <xacml-context:Result ResourceId="NOT_Authorized_Resouce">
                        <xacml-context:Decision>Deny</xacml-context:Decision>
                    </xacml-context:Result>
                </xacml-context:Response>
            </xacml-saml:XACMLAuthzDecisionStatement>
        </xacml-samlp:Assertion>
    </samlp:Response>
</soap-env:Body>
</soap-env:Envelope>
```

<!--
>![RelatedInformation]
>* [ProxyMVPD Web Service](/help/authentication/proxy-mvpd-webserv.md)
>* [Service Provider Scoping](/help/authentication/serv-provider-scoping.md)
-->
