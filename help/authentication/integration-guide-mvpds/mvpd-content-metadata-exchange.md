---
title: MVPD内容元数据交换
description: MVPD内容元数据交换
exl-id: d17e60dc-6c61-4ca2-bad8-1840c95261e0
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '402'
ht-degree: 0%

---

# MVPD内容元数据交换

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## 概述 {#content-metadat-exchange-overview}

本页介绍了Adobe Pass身份验证用于根据授权请求将结构化数据发送到MVPD的两个标准实施。  结构化数据表示发出请求的资源（程序员），可能还表示内容评级等附加数据。

在程序员方面，Adobe Pass身份验证支持结构化MRSS数据资源，如下所示：

1. 程序员将资源作为MRSS字符串发送。 Adobe Pass身份验证不会在Web或本机设备的客户端对其进行编码。 MRSS将作为常规字符串发送到Adobe Pass身份验证服务器。
1. 在服务器端，MRSS根据预定义架构(http://search.yahoo.com/mrss/)进行验证。  如果验证通过，Adobe Pass身份验证将从MRSS字段中提取信息，包括：
   * 频道标题
   * 项目标题
   * 资源标识符
   * 评级值和类型
1. 使用从MRSS提取的值来构建传递给MVPD的授权请求。

Adobe Pass身份验证支持两种将MRSS转换为MVPD支持的格式的方法：

* **XACML**。  第一种方法符合OLCA标准。  它使用XACML，其中提取MRSS值以构建具有映射到MRSS元素的属性的XACMLResource。  然后传递给MVPD。
* **重设**。  第二种方法是基于REST的。  MRSS经过base64编码，并在REST调用中作为URL参数传递。

在这两种方法中，MVPD通过将提取的值包含在其自己的逻辑流中并返回授权响应来处理授权请求。

## 集成详细信息 {#integration-details}

* 基于OLCA的XACML结构化资源
* 基于REST的结构资源

### 基于OLCA的XACML结构化资源 {#olca-based-xacml-struc-resource}

大多数面向缆线的MVPD都使用基于XACML的方法，但尚不支持完整的结构化数据方法。  其他支持XACML的MVPD采用渠道标题，并接受它用于ResourceID属性。 下面的示例展示了基于XACML的完整结构化方法。 Adobe Pass身份验证团队建议，对于使用XACML，但尚未支持家长控制等功能的MVPD，他们应该调整其XACML集成，以适应以下示例：

```XML
<?xml version="1.0" encoding="UTF-8"?>
<soap11:Envelope xmlns:soap11=">
    <soap11:Header/>
    <soap11:Body>
        <xacml-samlp:XACMLAuthzDecisionQuery
                xmlns:xacml-samlp="urn:oasis:names:tc:xacml:2.0:profile:saml2.0:v2:schema:protocol"
                Destination="
                ID="_f1dd34469c5aeac016760e51dbba007d" IssueInstant="2012-06-26T16:30:24.879Z" Version="2.0">
            <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">
                https://saml.sp.auth.adobe.com/
            </saml:Issuer>
            <ds:Signature xmlns:ds=">.......</ds:Signature>
            <xacml-context:Request xmlns:xacml-context="urn:oasis:names:tc:xacml:2.0:context:schema:os">
                [....info skipped for brevity....]
                <xacml-context:Resource>
 
        // The MRSS item GUID is passed as the XACML Resource resource-id
                    <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id">
                        <xacml-context:AttributeValue>DISNEY_GUID_12345</xacml-context:AttributeValue>
                    </xacml-context:Attribute>
        // The MRSS channel title is passed as the XACML Resource tv-network
                    <xacml-context:Attribute AttributeId="urn:cablelabs:ocla:1.0:attribute:content:tv-network">
                        <xacml-context:AttributeValue>Disney</xacml-context:AttributeValue>
                    </xacml-context:Attribute>
 
        // Adobe doesn't yet support an explicit namespace for the GUID, so we reuse the channel title as the GUID.  
        // We expect to add an explicit namespace later next year pulling it from the GUID scheme attribute.
                    <xacml-context:Attribute AttributeId="urn:cablelabs:ocla:1.0:attribute:content:id:namespace">
                        <xacml-context:AttributeValue>Disney</xacml-context:AttributeValue>
                    </xacml-context:Attribute>
 
        // The MRSS item title is passed as the XACML Resource content title
                    <xacml-context:Attribute AttributeId="urn:cablelabs:ocla:1.0:attribute:content:title">
                        <xacml-context:AttributeValue>Disney Program X</xacml-context:AttributeValue>
                    </xacml-context:Attribute>
 
        // The MRSS media rating is passed as the XACML Resource content rating 
                    <xacml-context:Attribute AttributeId="urn:cablelabs:ocla:1.0:attribute:content:rating:vchip">
                        <xacml-context:AttributeValue>TV-Y</xacml-context:AttributeValue>
                    </xacml-context:Attribute>
 
                </xacml-context:Resource>
 
                <xacml-context:Action>
                    <xacml-context:Attribute>
                        <xacml-context:AttributeValue>VIEW</xacml-context:AttributeValue>
                    </xacml-context:Attribute>
                </xacml-context:Action>
 
                [.....info skipped for brevity....]
            </xacml-context:Request>
        </xacml-samlp:XACMLAuthzDecisionQuery>
    </soap11:Body>
</soap11:Envelope>
 
//formatted for readability
```

### 基于REST的结构资源 {#rest-based-struct-resource}

有些MVPD已经对以下基于REST的授权协议进行了标准化。 此方法与XACML方法一样具有完整功能，但提供了“更轻”的实施。

`// The MRSS is base64 encoded by Adobe Pass Authentication, and passed in that format to the REST-based Authorization endpoint.`

`https://auth.somedomain.net/mediation/1/rest/client/authz?uuID=AC82CE4&mrss=base64encodedstring&IPAddress=123.456.78.901`

<!--
>[!RELATEDINFORMATION]
>* [User Metadata Exchange](/help/authentication/mvpd-user-metadata-exchng.md)
>* [Logout](/help/authentication/usecase-mvpd-logout.md)
>* [Programmer Integration Guide: Identifying Protected Resources](/help/authentication/identify-protected-resources.md)
>* [Programmer Integration Guide: User Metadata Exchange](/help/authentication/user-metadata.md)
-->
