---
title: MVPD身份验证
description: MVPD身份验证
exl-id: 9ff4a46e-a37b-414c-a163-9e586252a9c3
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1882'
ht-degree: 0%

---

# MVPD身份验证 {#mvpd-authn}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## 概述 {#mvpd-authn-overview}

实际的服务提供商(SP)角色由程序员担任，但Adobe Pass身份验证充当该程序员的SP代理。 使用Adobe Pass身份验证作为中介，可让MVPD和程序员避免根据具体情况自定义其授权过程。

当程序员从支持SAML的MVPD请求身份验证时，以下步骤使用Adobe Pass身份验证显示事件的序列。 请注意，Adobe Pass身份验证访问启用程序组件在用户/订阅者的客户端上处于活动状态。 从那里， Access Enabler简化了身份验证流程的所有步骤。

1. 当用户请求访问受保护的内容时，Access Enabler代表程序员(SP)启动身份验证(AuthN)。
1. SP的应用程序向用户显示“MVPD选取器”，以获取其付费电视提供商(MVPD)。 然后，SP将用户的浏览器重定向到所选MVPD的身份提供者(IdP)服务。  这是“**程序员启动的登录**”。  MVPD将IdP的响应发送到Adobe的SAML断言使用者服务，并在其中进行处理。
1. 最后，Access Enabler将浏览器重定向回SP站点，通知SP AuthN请求的状态（成功/失败）。

## 身份验证请求 {#authn-req}

如上述步骤所示，在AuthN流期间，MVPD必须同时接受基于SAML的AuthN请求和发送SAML AuthN响应。

[联机内容访问(OLCA)身份验证和授权接口规范](https://www.cablelabs.com/specifications/search?query=&category=&subcat=&doctype=&content=false&archives=false){target=_blanck}提供了标准的AuthN请求和响应。 虽然Adobe Pass身份验证不要求MVPD使其授权消息基于此标准，但查看规范可以深入了解AuthN事务所需的关键属性。

>[!NOTE]
>
>MVPD通过Adobe Pass身份验证收到的AuthN请求包含数字签名。 但是，出于简短的原因，以下示例不显示签名。 有关显示数字签名的示例，请参阅以下部分中[身份验证响应](#authn-response)中的示例。

示例SAML身份验证请求：

```XML
<?xml version="1.0" encoding="UTF-8"?>
<samlp:AuthnRequest  
    AssertionConsumerServiceURL=http://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer          
    Destination=http://idp.com/SSOService
    ForceAuthn="false"
    ID="_c0fc667e-ad12-44d6-9cae-bc7cf04688f8"
    IsPassive="false"
    IssueInstant="2010-08-03T14:14:54.372Z"
    ProtocolBinding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST"
    Version="2.0"
    xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">
        http://saml.sp.adobe.adobe.com
    </saml:Issuer>
    <ds:Signature xmlns:ds=_signature_block_goes_here_
    </ds:Signature>
    <samlp:NameIDPolicy
        AllowCreate="true"
        Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"
        SPNameQualifier="http://saml.sp.adobe.adobe.com"/>
</samlp:AuthnRequest> 
```

下表说明了需要在身份验证请求中的属性和标记，以及默认预期值。

**SAML身份验证请求详细信息**

| samlp：AuthnRequest | 服务提供者向身份提供者发出的&lt;AuthnRequest>。 |
|-----------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| AssertionConsumerServiceURL | 这是要在后续响应中使用的Adobe端点。 默认值： **http://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer** |
| 目标 | URI引用，指示此请求已发送到的地址。 这有助于防止将请求恶意转发给意外收件人，这是某些协议绑定所需的保护。 如果存在，则实际的收件人必须检查URI引用是否标识接收消息的位置。 如果不能，则必须放弃请求。 某些协议绑定可能需要使用此属性。 |
| ForceAuthn | 如果存在ForceAuthn属性，且值为true，则标识提供者有义务重新建立此标识，而不是依赖其可能与主体之间的现有会话。 |
| ID | 请求的标识符。 有关详细信息，请参阅[SAML core 2.0-os](http://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf){target=_blank}第1.3.4节。 |
| IsPassive | 布尔值。 如果为“true”，则身份提供者和用户代理本身不得从请求者那里明显控制用户界面并以明显的方式与演示者交互。 如果未提供值，则默认值为“false”。 |
| 问题即时 | 响应的发布时间。 时间值以UTC编码，如[SAML core 2.0-os](http://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf){target=_blank}第1.3.3节中所述。 |
| 协议绑定 | 一个URI引用，标识在返回&lt;Response>消息时要使用的SAML协议绑定。 有关为其定义的协议绑定和URI引用的详细信息，请参阅[SAMLBind]。 默认值： urn:oasis:names:tc:SAML：2.0:bindings:HTTPPOST |
| 版本 | 此请求的版本。 |
| saml：Issuer | 标识生成响应消息的实体。 （有关此元素的更多信息，请参阅SAML core 2.0-os部分2.2.5） |
| ds：签名 | XML签名，用于保护声明的完整性并验证声明的签发者，如SAML core 2.0-os中的第5节所述 |
| samlp：NameIDPolicy | 指定用于表示所请求主体的名称标识符的约束。 |
| 允许创建 | 一个布尔值，用于指示在执行请求过程中是否允许身份提供者创建新标识符以表示主体。 默认值： true |
| 格式化 | 指定与名称标识符格式对应的URI引用默认： urn:oasis:名称:tc:SAML：2.0:nameid-format:临时Adobe建议： urn:oasis:名称:tc:SAML：2.0:nameid-format:持久 |
| Spnamequalifier | （可选）指定在除请求者之外的服务提供商的命名空间中返回（或创建）断言主体的标识符。 默认：http://saml.sp.adobe.adobe.com |

## 身份验证响应 {#authn-response}

收到并处理了身份验证请求后，MVPD现在必须发送身份验证响应。

**示例SAML身份验证响应**

```XML
<?xml version="1.0" encoding="UTF-8"?> 
<samlp:Response Destination="https://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer"
                ID="_0ac3a9dd5dae0ce05de20912af6f4f83a00ce19587"                             
                InResponseTo="_c0fc667e-ad12-44d6-9cae-bc7cf04688f8"
                IssueInstant="2010-08-17T11:17:50Z" Version="2.0"              
                xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion"
                xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol"
                xmlns:xs="http://www.w3.org/2001/XMLSchema"
                xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">
             http://idp.com/SSOService
    </saml:Issuer>
    <samlp:Status>
       <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success"/>
    </samlp:Status>
    <saml:Assertion ID="pfxb0662d76-17a2-a7bd-375f-c11046a86742"
                   IssueInstant="2010-08-17T11:17:50Z"
                   Version="2.0">
        <saml:Issuer>http://idp.com/SSOService</saml:Issuer>
        <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
          <ds:SignedInfo>
            <ds:CanonicalizationMethod
                     Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
            <ds:SignatureMethod
                     Algorithm="http://www.w3.org/2000/09/xmldsig#rsa-sha1"/>
            <ds:Reference URI="#pfxb0662d76-17a2-a7bd-375f-c11046a86742">
              <ds:Transforms>
                 <ds:Transform
                    Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature"/>        
                 <ds:Transform
                            Algorithm=http://www.w3.org/2001/10/xml-exc-c14n#"/>
              </ds:Transforms>
              <ds:DigestMethod Algorithm="http://www.w3.org/2000/09/xmldsig#sha1"/>
              <ds:DigestValue>LgaPI2ASx/fHsoq0rB15Zk+CRQ0=</ds:DigestValue>
            </ds:Reference>
          </ds:SignedInfo>
          <ds:SignatureValue>
                POw/mCKF__shortened_for_brevity__9xdktDu+iiQqmnTs/NIjV5dw==
          </ds:SignatureValue>
          <ds:KeyInfo>
            <ds:X509Data>
                <ds:X509Certificate>
                 MIIDVDCCAjygAwIBA__shortened_for_brevity_utQ==
                </ds:X509Certificate>
            </ds:X509Data>
          </ds:KeyInfo>
      </ds:Signature>
      <saml:Subject>
        <saml:NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"
                     SPNameQualifier="https://saml.sp.auth.adobe.com">
            _5afe9a437203354aa8480ce772acb703e6bbb8a3ad
        </saml:NameID>
        <saml:SubjectConfirmation
                     Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
            <saml:SubjectConfirmationData
                  InResponseTo="_c0fc667e-ad12-44d6-9cae-bc7cf04688f8"
                  NotOnOrAfter="2010-08-17T11:22:50Z"                                          
                  Recipient="https://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer"/>
           </saml:SubjectConfirmation>
       </saml:Subject>
       <saml:Conditions NotBefore="2010-08-17T11:17:20Z"
                        NotOnOrAfter="2010-08-17T19:17:50Z">
           <saml:AudienceRestriction>
              <saml:Audience>https://saml.sp.auth.adobe.com</saml:Audience>
           </saml:AudienceRestriction>
       </saml:Conditions>
       <saml:AuthnStatement AuthnInstant="2010-08-17T11:17:50Z"
                   SessionIndex="_1adc7692e0fffbb1f9b944aeafce62aaa7d770cd9e">
        <saml:AuthnContext>
            <saml:AuthnContextClassRef>
                   urn:oasis:names:tc:SAML:2.0:ac:classes:Password
            </saml:AuthnContextClassRef>
        </saml:AuthnContext>
    </saml:AuthnStatement>
  </saml:Assertion>
</samlp:Response>
```


在上述示例中，AdobeSP期望从Subject/NameId中获取用户ID。 可以将AdobeSP配置为从自定义属性中获取用户ID；响应应包含如下元素：

```XML
<saml:AttributeStatement>
     <saml:Attribute Name="guid" NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic">
         <saml:AttributeValue xsi:type="xs:string">
               71C69B91-F327-F185-F29E-2CE20DC560F5
         </saml:AttributeValue>
    </saml:Attribute>
</saml:AttributeStatement>
```

**SAML身份验证响应详细信息**

| samlp：Response | Adobe Pass身份验证收到的响应。 |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 目标 | URI引用，指示此请求已发送到的地址。 这有助于防止将请求恶意转发给意外收件人，这是某些协议绑定所需的保护。 如果存在，则实际的收件人必须检查URI引用是否标识接收消息的位置。 如果不能，则必须放弃请求。 某些协议绑定可能需要使用此属性。 |
| ID | 请求的标识符。 它的类型为xs：ID，并且必须遵循[SAML core 2.0-os](http://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf){target=_blank}的第1.3.4节中指定的标识符唯一性要求。 请求中ID属性的值与相应响应中的InResponseTo属性的值必须匹配。 |
| InResponseTo | SAML协议消息的ID，证明实体可以针对该ID提供声明。 该值必须等于在身份验证请求中发送的ID属性中的值。 请参阅SAML core 2.0-os |
| 问题即时 | 发出请求的时间。 |
| 版本 | 请求的版本。 |
| saml：Issuer | 标识生成请求消息的实体。 (有关此元素的更多信息，请参阅第2.2.5部分。SAML core 2.0-os ) |
| samlp：Status | 表示相应请求状态的代码。 |
| samlp：StatusCode | 表示响应相应请求而执行的活动的状态的代码。 |
| saml：Assertion | 此类型指定所有断言通用的基本信息。 |
| ID | 此断言的标识符。 |
| 版本 | 此断言的版本。 |
| 问题即时 | 发出请求的时间。 |
| ds：签名 | XML签名，用于保护声明的完整性并验证声明的签发者，如SAML core 2.0-os中的第5节所述 |
| ds：SignedInfo | SignedInfo的结构包括规范化算法、签名算法和一个或多个引用。 SignedInfo元素可能包含可选的ID属性，该属性允许其他签名和对象引用它。 请参阅XML签名语法和处理 |
| ds：CanonicalizationMethod | CanonicalizationMethod是指定在执行签名计算之前应用于SignedInfo元素的规范化算法的必需元素。 请参阅XML签名语法和处理 |
| ds：SignatureMethod | SignatureMethod是指定用于生成和验证签名的算法的必需元素。 此算法标识签名操作中涉及的所有加密函数（例如散列、公钥算法、MAC、填充等） 请参阅XML签名语法和处理 |
| ds：Reference | 引用是一个元素，可能会出现一次或多次。 它指定摘要算法和摘要值，以及可选的被签名对象的标识符、对象的类型和/或在摘要之前要应用的转换列表。 请参阅XML签名语法和处理 |
| ds：转换 | 可选的Transforms元素包含Transform元素的有序列表；这些元素描述签名者如何获取已提取的数据对象。 每个转换的输出用作下一个转换的输入。 第一个转换的输入是取消引用Reference元素的URI属性的结果。 上次转换的输出是DigestMethod算法的输入。 请参阅XML签名语法和处理 |
| ds：DigestMethod | DigestMethod是标识要应用于已签名对象的摘要算法的必需元素。 请参阅XML签名语法和处理 |
| ds：DigestValue | DigestValue是一个包含摘要的编码值的元素。 摘要始终使用base64编码。 请参阅XML签名语法和处理 |
| ds：SignatureValue | SignatureValue元素包含数字签名的实际值；它始终使用base64进行编码。 请参阅XML签名语法和处理 |
| ds：KeyInfo | KeyInfo是一个可选元素，它允许收件人获取验证签名所需的密钥。 请参阅XML签名语法和处理 |
| ds：X509Data | KeyInfo中的X509Data元素包含密钥或X509证书的一个或多个标识符。 请参阅XML签名语法和处理 |
| ds：X509证书 | X509Certificate元素，它包含base64编码的[X509v3]证书 |
| saml：Subject | 声明中的语句的主题。 |
| saml：NameID | &lt;NameID>元素的类型为NameIDType（请参阅SAML core 2.0-os中的第2.2.2部分），用于各种SAML断言结构（如&lt;Subject>和&lt;SubjectConfirmation>元素）以及各种协议消息中。 |
| 格式化 | 表示基于字符串的标识符信息的分类的URI引用。 |
| Spnamequalifier | 进一步使用服务提供者的名称或提供者的隶属关系来限定名称。 此属性提供了基于依赖方或依赖方联合名称的附加方法。 |
| saml：SubjectConfirmation | 允许确认主体的信息。 如果提供了多个主题确认，则满足其中任何一个主题就足以确认用于应用断言的主题。 |
| saml：SubjectConfirmationData | 由特定确认方法使用的其他确认信息。 例如，此元素的典型内容可能是XML签名语法和处理规范中定义的<!--<ds:KeyInfo>-->元素 |
| NotOnOrAfter | 无法再确认受众的时间。 |
| 收件人 | 一个URI，它指定证明实体可以向其提供断言的实体或位置。 例如，此属性可能表示必须将断言传递到特定网络端点，以防止中间人将其重定向到其他位置。 |
| saml：Conditions | &lt;Condition>元素用作新条件的扩展点。 |
| NotBefore | NotBefore属性指定有效期开始的时间。 |
| saml：AudienceRestriction | &lt;AudienceRestriction>元素指定声明面向由&lt;Audience>元素标识的一个或多个特定受众。 |
| saml：Audience | 标识预期受众的URI引用。 |
| saml：AuthnStatement | 身份验证语句。 |
| AuthnInstant | 指定身份验证发生的时间。 |
| 会话索引 | 指定由主体标识的主体与身份验证机构之间特定会话的索引。 |
| saml：AuthnContext | 身份验证机构使用的上下文，一直到生成此语句的身份验证事件为止（包括该事件）。 |
| saml：AuthnContextClassRef | 标识身份验证上下文类的URI引用，该类描述后面的身份验证上下文声明。 |
