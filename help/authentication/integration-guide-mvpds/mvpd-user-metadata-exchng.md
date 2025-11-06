---
title: MVPD用户元数据交换
description: MVPD用户元数据交换
exl-id: 8bce6acc-cd33-476c-af5e-27eb2239cad1
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '940'
ht-degree: 0%

---

# MVPD用户元数据交换

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## 简介 {#intro-user-metadata-exchange}

MVPD维护有关其客户的特定于用户的元数据，这些元数据在某些情况下与程序员共享。 Adobe Pass身份验证的目标是代理此“用户元数据”的交换，但不实施任何有关该交换的规则。 交换规则是供MVPD与其程序员合作伙伴协商的。

可用于Exchange的用户元数据类型当前包括：

* 邮政编码
* 最大评级（VChip或MPAA）
* 用户 ID
* 家庭ID
* 渠道ID

使用此功能，MVPD和程序员可以实施特殊用例，如家长控制。 例如，MVPD可以将家长评级数据传递给程序员，然后程序员使用该数据过滤用户的可用查看选择。

用户元数据关键点：

* MVPD在身份验证和授权流期间，将用户元数据传递到程序员的应用程序
* Adobe Pass身份验证将元数据值保存在身份验证和身份验证令牌中
* Adobe Pass身份验证可以规范以不同格式提供用户元数据的MVPD的值
* 某些参数可以使用程序员的密钥进行加密
* Adobe通过配置更改提供特定值

>[!NOTE]
>
>用户元数据是以前在Adobe Pass身份验证中可用的静态元数据（身份验证令牌TTL、授权令牌TTL和设备ID）的扩展。

## 示例 {#example-mvpd-user-metadata-exch}

### 家长控制 {#example-parental-control}

此示例显示了以下内容的交换：

* [程序员到MVPD元数据交换](#progr-mvpd-metadata-exch)

* [MVPD与程序员元数据交换流程](#mvpd-progr-exchange-flow)

### 程序员到MVPD元数据交换 {#progr-mvpd-metadata-exch}

目前，程序员API、Adobe Pass身份验证和MVPD授权程序都仅支持渠道级别的授权。 该渠道在程序员的getAuthorization() API调用中指定为纯文本字符串。 此字符串会一直传播到MVPD的授权后端：

用户从程序员的应用程序或站点中选择支持XACML的MVPD（在本例中为“TNT”）。 有关XACML的信息，请参阅[可扩展访问控制标记语言](https://en.wikipedia.org/wiki/XACML){target=_blank}。
程序员的应用程序形成包含资源及其元数据的AuthZ请求。  此示例在渠道元素的媒体属性中包括“pg”的MPAA评级：

```XML
var resource = '<rss version="2.0" xmlns:media="http://video.search.yahoo.com/mrss/">
                    <channel> 
                        <title>TNT</title> 
                        <media:rating scheme="urn:mpaa">pg</media:rating>
                    </channel>
                </rss>';
getAuthorization(resource);
```

当MVPD和程序员都支持时，Adobe Pass身份验证实际上支持更细粒度的授权，具体可达到资源级别。 资源及其元数据对Adobe不透明；其目的是建立标准格式，用于以规范化的方式指定资源ID和元数据，以将资源ID发送到不同的MVPD。

>[!NOTE]
>
>如果用户选择仅支持渠道的MVPD，则Adobe Pass身份验证仅提取渠道标题（上例中为“TNT”），并仅将标题传递给MVPD。

### MVPD与程序员元数据交换流程 {#mvpd-progr-exchange-flow}

Adobe Pass身份验证做出了以下假设：

* MVPD将最大评分作为SAML响应的一部分发送
* 此信息将另存为身份验证令牌的一部分
* Adobe Pass身份验证提供了API，以便程序员可以检索此信息
* 程序员在其网站或应用程序上实施此功能（例如，隐藏超过用户最大评分的视频）

```XML
<saml:Assertion ID="pfxec5f92e0-8589-3fc3-c708-f4fb8e2fad59"
                 IssueInstant="2010-07-20T10:05:41Z" Version="2.0"
                 xmlns:xs="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <saml:AttributeStatement>
        <saml:Attribute
                Name="MaxTVRating"
                NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic">
            <saml:AttributeValue xsi:type="xs:string">tv-ma</saml:AttributeValue>
        </saml:Attribute>
        <saml:Attribute
                Name="MaxMovieRating"
                NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic">
            <saml:AttributeValue xsi:type="xs:string">nc-17</saml:AttributeValue>
        </saml:Attribute>
    </saml:AttributeStatement>
</saml:Assertion>
```

### 注释 {#notes-mvpd-progr-metadata-exch-flow}

**资源规范化和验证。**&#x200B;资源ID可以作为纯字符串或MRSS字符串进行传递。 程序员可以决定使用纯字符串格式或MRSS，但需要事先与MVPD达成协议，以便MVPD知道如何处理该资源。

**资源ID和元数据规范。** Adobe Pass身份验证使用带有Media RSS扩展的RSS标准指定资源及其元数据。 Adobe Pass身份验证与Media RSS扩展结合使用时，支持多种元数据，例如家长控制（通过`<media:rating>`）或地理位置(`<media:location>`)。

Adobe Pass身份验证还可以支持从旧版渠道字符串到需要RSS的MVPD的对应RSS资源的透明转换。 另一方面，Adobe Pass身份验证支持从RSS+MRSS转换为纯通道标题，适用于纯通道MVPD。

**Adobe Pass身份验证确保与现有集成完全向后兼容。**&#x200B;也就是说，对于使用渠道级别身份验证的程序员，Adobe Pass身份验证在将该渠道ID发送到了解该格式的MVPD之前，会注意以必要的格式对其进行打包。 反之亦然：如果程序员以新格式指定其所有资源，并且仅针对进行渠道级别授权的Adobe Pass进行授权，则MVPD身份验证会将新格式转换为简单的渠道字符串。

## 用户元数据用例 {#user-metadata-use-cases}

随着越来越多的MVPD做出法律安排并添加功能，用例正在不断变化和扩展。 以下是可用于的用户元数据的示例。

* [MVPD用户ID](#mvpd-user-id)
* [家庭ID](#household-user-id)
* [邮政编码](#zip-code)
* [最大评级（家长控制）](#max-rating-parental-control)
* [渠道排列](#channel-line-up)

### MVPD用户ID {#mvpd-user-id}

* 由MVPD提供
* 不是用户的实际登录信息，因为它由MVPD进行哈希处理
* 可用于指示特定用户的问题或针对特定用户的问题
* 已加密
* MVPD支持：所有MVPD

### 家庭用户ID {#household-user-id}

* 允许提供良好的量度信息
* 已加密
* MVPD支持：某些MVPD

### 邮政编码 {#zip-code}

* 用户的计费邮政编码
* 主要用于强制实施体育赛事冻结期规则
* 可以随AuthZ响应一起提供以进行快速更新
* MVPD支持：某些MVPD

### 最大评级（家长控制） {#max-rating-parental-control}

* AuthN初始，加上AuthZ刷新
* 从UI中筛选内容
* MPAA或VChip评级
* MVPD支持：某些MVPD

### 渠道排列 {#channel-line-up}

* MVPD可提供用户有权查看的渠道列表
* 允许快速UI绘画
* OLCA规范允许将此作为AuthN响应中的AttributeStatement
* 支持MVPD：一些MVPD

<!--
>[!RELATEDINFORMATION]
>
>* [Proxy MVPD Web Service](/help/authentication/proxy-mvpd-webserv.md)
>* [Content Metadata Exhange](/help/authentication/mvpd-content-metadata-exchange.md)
>* [OLCA AuthN / AuthZ Specification](https://www.cablelabs.com/specifications/CL-SP-AUTH1.0-I04-120621.pdf){target=_blank}
>* [User Metadata (Programmer Integration Guide)](/help/authentication/user-metadata-feature.md)
-->
