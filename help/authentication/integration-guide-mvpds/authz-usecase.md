---
title: MVPD授权
description: MVPD授权
exl-id: 215780e4-12b6-4ba6-8377-4d21b63b6975
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '584'
ht-degree: 0%

---

# MVPD授权

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## 概述 {#mvpd-authz-overview}

授权(AuthZ)通过Adobe承载的后端服务器和MVPD AuthZ端点之间的后通道（服务器到服务器）通信来执行。

对于AuthZ请求，授权端点应能够至少处理以下参数：

* **Uid**。 从身份验证步骤收到的用户ID。

* **资源ID**。 标识给定内容资源的字符串。 此资源ID由程序员指定，并且MVPD必须增强这些资源的业务规则（例如，通过检查用户是否订阅了特定渠道）。

除了确定用户是否获得授权之外，响应还必须包括此授权的生存时间(TTL)，即授权过期的时间。 如果未设置TTL，则AuthZ请求将失败。  因此，**TTL是Adobe Pass身份验证端**&#x200B;上的强制配置设置，以便涵盖MVPD在其请求中未包含TTL的情况。

## 授权请求 {#authz-req}

AuthZ请求必须包括代表其发出请求的主题、主题尝试访问的资源、主题尝试对资源执行的操作以及将要执行操作的环境。 在Adobe Pass身份验证的特定情况下，这些元素对应于：

| XACML元素 | 对应于 |
|---------------|--------------------------------------------------------------------------------------------------------------------------------|
| 主题 | 由经过身份验证的会话标识的主体，由SAML断言的“subject-token”AttributeValue引用。 |
| 资源 | 受保护资源的URI。 |
| 操作 | 视图。 |
| 环境 | 包括SP看到的请求客户端的IP地址。 |



此时，SP必须准备一个XACML Authorization DecisionQuery并(通过HTTPPOST)将其发送到（之前商定的）Policy Decision Point (PDP)以获取IdP。 以下是简单XACML请求的示例（请参阅XACML核心规范）：

```XML
POST https://authz.site.com/XACML_endpoint
<Request  xmlns="urn:oasis:names:tc:xacm:2.0:context:schema:os"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="urn:oasis:names:tc:xacml:2.0:context:schema:os
http://docs.oasis-open.org/xacml/access_control-xacml-2.0-context-schema-os.xsd">
<Subject>
   <Attribute
        AttributeId="urn:oasis:names:tc:xacml:1.0:subject:subject-token"
        DataType="http://www.w3.org/2001/XMLSchema#base64Binary">
      <AttributeValue>{Base64 Data}</AttributeValue>
   </Attribute>
</Subject>
<Resource>
   <Attribute
        AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id"
        DataType="http://www.w3.org/2001/XMLSchema#anyURI">
<AttributeValue>urn:tve:tms:1234</AttributeValue>
   </Attribute>
</Resource>
<Action>
   <Attribute
        AttributeId="urn:oasis:names:tc:xacml:1.0:action:action-id"
        DataType="http://www.w3.org/2001/XMLSchema#string">
       <AttributeValue>VIEW</AttributeValue>
   </Attribute>
</Action>
<Environment>
   <Attribute
       AttributeId="urn:oasis:names:tc:xacml:1.0:subject:authn-locality:ip-address"
       DataType="http://www.w3.org/2001/XMLSchema#string">
      <AttributeValue>1.2.3.4</AttributeValue>
   </Attribute>
</Environment>
</Request>
```


在收到AuthZ请求后，MVPD的PDP将评估该请求并确定是否应允许主体对资源执行所请求的操作。 然后，MVPD会返回一个包含决策、状态代码和消息的响应，如下面的授权响应中所述。

## 授权响应 {#authz-response}

对AuthZ请求的响应在MVPD评估请求并应用所请求的业务规则以确定是否允许主体对资源执行所请求的操作之后发出。 返回的对Adobe Pass身份验证的响应将再次按照XACML核心规范表示，其中包含SP作为策略实施点(PEP)的“决定”、“状态代码”、“消息”和“义务”。 以下是示例响应：

```XML
<Response xmlns="urn:oasis:names:tc:xacml:2.0:context:schema:os">
  <Result>
  <Decision>Permit</Decision>
  <Status>
     <StatusCode Value="urn:oasis:names:tc:xacml:1.0:status:ok"/>
     <StatusMessage>ok</StatusMessage>
  </Status>
  <xacml:Obligations     
          xmlns:xacml="urn:oasis:names:tc:xacml:2.0:policy:schema:os">
     <xacml:Obligation    
              ObligationId="urn:cablelabs:olca:1.0:obligations:log"
              FulfillOn="Permit" />
  </xacml:Obligations>
 </Result>
</Response>
```

以下是Adobe Pass身份验证支持并使程序员能够履行的DENY义务列表：

* **urn:tve:xacml：2.0:obligations:restrict-pc** — 订阅者未通过家长控制检查，SP必须采取适当措施限制对此内容的访问。

* **urn:tve:xacml：2.0:obligations:upgrade** — 订阅服务器没有适当的订阅级别。  必须升级订阅才能访问内容。

Adobe Pass身份验证支持以下&#x200B;**PERMIT**&#x200B;义务，并使程序员能够履行这些义务：

* **urn:cablelabs:olca：1.0:obligations:log** - Adobe Pass将记录该事务，并且可以通过商定的报告机制提供该事务。

* **urn:cablelabs:olca：1.0:obligations:re-authz** - Adobe Pass身份验证在n秒内再次刷新授权（通过XACML AttributeAssignment指定为责任的参数 — 请参阅XACML核心规范，第5.46节）。

<!--
>![RelatedInformation]
>* [Preflight Authorization](/help/authentication/preflight-authz.md)
>* [Authentication](/help/authentication/authn-usecase.md)
-->
