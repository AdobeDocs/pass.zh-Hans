---
title: 术语表
description: 术语表
exl-id: e64a94f6-7460-4aa8-8d6b-e0553ba1e4ec
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '836'
ht-degree: 0%

---

# 术语表 {#glossary}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## AccessEnabler {#accessEnabler}

Adobe Pass身份验证的客户端组件。 Adobe Pass身份验证为每个支持的平台提供一个AccessEnabler库。

## 身份验证N {#authn}

用作“身份验证”的简写，如“AuthN令牌”或“AuthN流量”。


## 身份验证令牌{#authn-token}

身份验证令牌，在用户使用MVPD成功进行身份验证后由Adobe Pass身份验证生成。 根据程序员的集成平台，令牌会存储在用户的设备或Adobe Pass身份验证服务器上。

## AuthZ {#authz}

用作“授权”的简写，如“AuthZ令牌”或“AuthZ流”中所示。

## AuthZ令牌 {#authz-token}

授权令牌，在用户获得查看受保护内容的授权后，由Adobe Pass身份验证生成。 AuthZ令牌存储在Adobe Pass身份验证服务器上，用于生成[短期媒体令牌](#short-lived-token)。

## 渠道ID（已弃用） {#channel_id}

资源ID的前术语。

## 无客户端API {#clientless-api}

Adobe Pass身份验证集成解决方案，它采用Web服务而不是AccessEnabler客户端组件。

## 设备ID {#device-id}

在Adobe Pass身份验证中唯一标识设备（例如手机、平板电脑等）。 此ID由程序员的应用程序获取/提供。


## 权利流{#entitlement_flow}

Adobe Pass身份验证文档中使用的术语是指使用Adobe Pass身份验证注册设备/用户、使用MVPD验证用户、为用户授权资源以及注销用户的整个过程。


## GUID {#guid}

查看[用户ID](#user-id)。

## IdP {#idp}

标识提供程序；在Adobe Pass身份验证集成中的MVPD角色上下文中，与MVPD同义。 （客户必须通过付费电视提供商的登录页面验证其身份。）

## 媒体令牌验证器 {#media-token-verifier}

Adobe提供的库，程序员可使用它来验证Adobe Pass身份验证在成功完成授权流后生成的短期媒体令牌。

## MVPD {#mvpd}

多频道视频节目分发商；与“付费电视提供商”同义。

## MVPD ID {#mvpd-id}

查看[用户ID](#user-id)。

## 合作伙伴 ID {#partner-id}

Adobe传递给MVPD的标识符，MVPD使用它来标识Adobe Pass身份验证请求身份验证的对象。 有时，它用于为特定程序员配置其UI，有时，它对于所有程序员都是相同的，这取决于MVPD的需求。

## 付费电视提供商 {#pay-tv-provider}

与[MVPD](#mvpd)同义。

## 程序员 {#programmer}

是“content provider”、“account”、“channel”、“service provider”、“brand”等同义词。

## 代理MVPD {#proxy-mvpd}

为其他MVPD提供身份服务的MVPD；直接与Adobe Pass身份验证集成。

## 代理的MVPD {#proxied-mvpd}

与AdobeSP没有直接集成，而是通过代理MVPD集成的MVPD。

## 请求者ID {#requestor-id}

在Adobe Pass身份验证中唯一标识[程序员](#programmer)（帐户、品牌、渠道或属性）。 此ID是在帐户初始设置期间在程序员和Adobe之间确定的。 在Web上，请求者ID与一组已列入白名单的域相关联；任何使用来自外部域的ID的调用都将被拒绝。 程序员还使用请求者ID进行分析。 通常每个程序员只有一个请求者ID。 与请求者ID相关的另一项功能是，程序员必须向Adobe提供公共证书，因为setRequestor API调用需要发送加密数据，用于在Adobe Pass身份验证系统中验证程序员。

## 资源ID {#resource-id}

向MVPD标识[程序员](#programmer)的字符串或mRSS资源。 它是在程序员和MVPD之间达成一致的；Adobe Pass身份验证传递未经接触的资源ID，因此对于所有MVPD，它必须是相同的。 只要MVPD知道每个ID代表的含义，程序员就可以使用多个资源ID。

## 会话GUID {#sessionGUID}

查看[用户ID](#user-id)。

## 短期媒体令牌 {#short-lived-token}

此令牌由Adobe Pass身份验证在成功完成特定用户的授权过程后生成。 令牌将传递到程序员，程序员使用短期媒体令牌上的Adobe Pass身份验证令牌验证器来验证授权过程的安全性。

## 智能设备 {#smart-device}

在Adobe Pass身份验证文档中使用的术语，指机顶盒、游戏控制台和智能电视。 这些设备具有联网功能，但无法呈现网页。

## SP{#sp}

服务提供商；这通常指的是SP的&#x200B;*角色*，它由Adobe Pass身份验证扮演，代表与[MVPD](#mvpd)集成中的程序员。

## 临时传递 {#temp-pass}

一项功能，允许程序员提供临时免费访问内容付费的功能。 对于程序员指定的时间段，访问权限是每个请求者。

## TTL {#ttl}

生存时间。 这是令牌有效的指定时间长度。

## TVE {#tve}

到处都是电视。

## 用户 ID {#user-id}

唯一标识程序员应用程序的用户，但源自MVPD。 对于不同的用例，以不同的形式提供。 请参阅程序员概述中的[了解用户ID](/help/authentication/kickstart/programmer-overview.md#user-ids)。

## 允许列表 {#whitelist}

为与Adobe Pass身份验证通信而指定为合法的域的列表。

## XSTS令牌 {#xsts-token}

由Microsoft颁发用于Xbox控制台应用程序开发的安全令牌，用于Xbox/Adobe Pass身份验证集成。
