---
title: 节流机构
description: 了解Adobe Pass身份验证中使用的限制机制。 在此页面中浏览此机制的概述。
exl-id: f00f6c8e-2281-45f3-b592-5bbc004897f7
source-git-commit: 6b803eb0037e347d6ce147c565983c5a26de9978
workflow-type: tm+mt
source-wordcount: '1141'
ht-degree: 0%

---

# 节流机构 {#throttling-mechanism}

所有Pass Authentication客户都需要能够根据说明和业务案例为其每位用户访问Pass Authentication API。

Pass Authentication引入了Throttling机制，以确保在客户的用户之间公平分配资源。

此机制之所以重要，原因如下：

- 多租户架构的黄金原则之一是，一个用户的行为不应影响其他用户。
- 速率限制对于API很重要，因为与API集成时容易出错。 由于API采用编程方式，因此意外发送比预期更多的请求相对容易。

## 机制概述 {#mechanism-overview}

### 客户端实施

通过身份验证提供了与API交互的准则和SDK，但无法控制客户如何使用它。 某些实施可能具有基本的实施，并且可能会向服务中添加大量不必要的API请求（无论是否意外），这意味着其他用户可能会遇到速度减慢或容量问题。

服务本身应能够处理任何合理的容量。 但无论服务的可扩展性或性能如何，总会有一些限制。 因此，服务必须针对特定时间间隔内接受的呼叫数配置限制。

### 引入限制

通过身份验证基于用户标识和具有预定义值的令牌桶速率限制算法，用于控制每个用户的设备访问我们的API。

### 装置识别机制

所提出的节流机制在“X-Forwarded-For”标头的帮助下单独使用已识别的设备。 限制将以相同的方式应用于每个设备。

### 所需更新

服务器到服务器实施必须使用“X-Forwarded-For”标头机制转发其客户端的IP地址。

您可以在[此处](legacy/rest-api-v1/cookbooks/rest-api-cookbook-servertoserver.md)找到有关如何传递X-Forwarded-For标头的更多详细信息。

### 实际限制和端点 {#throttling-mechanism-limits}

目前，默认限制允许每秒最多1个请求，初始突发为10个请求（所标识的客户端第一次交互允许一次性使用，这应允许成功完成初始化）。 这应该不会影响我们所有客户的任何常规业务案例。

将在以下端点上启用限制机制：

- /o/client/register
- /o/client/token
- /o/client/scopes
- /o/client/validate
- /api/v2/
- /api/v1/tokens/usermetadata
- /api/v1/tokens/authn
- /api/v1/tokens/authz
- /api/v1/tokens/media
- /api/v1/config/
- /api/v1/checkauthn
- /api/v1/logout
- /api/v1/authorize
- /api/v1/preauthorize
- /api/v1/mediatoken
- /api/v1/authenticate/freepreview
- /api/v1/authenticate/
- /api/v1/。+/profile-requests/.+
- /api/v1/identities
- /adobe-services/config/
- /reggie/v1/。+/regcode
- /reggie/v1/。+/regcode/.+

### SDK实施消除混淆

由于使用Adobe Pass身份验证提供的SDK的客户端不会与任何端点显式交互，因此本节将介绍已知功能、它们在遇到限制响应时的行为以及应执行的操作。

#### setRequestor

在从SDK使用`setRequestor`函数达到限制限制时，SDK将通过`errorHandler`回调返回CFG429错误代码。

#### getAuthorization

从SDK使用`getAuthorization`函数达到限制时，SDK将通过`errorHandler`回调返回Z100错误代码。

#### checkPreauthorizedResources

从SDK使用`checkPreauthorizedResources`函数达到限制时，SDK将通过`errorHandler`回调返回P100错误代码。

#### getMetadata

使用SDK中的`getMetadata`函数达到限制时，SDK将通过`setMetadataStatus`回调返回空响应。

有关每个特定实施的详细信息，请参阅特定的SDK文档。

- [JavaScript SDK API参考](legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md)
- [Android SDK API参考](legacy/sdks/android-sdk/android-sdk-api-reference.md)
- [iOS/tvOS API参考](legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)

### API响应更改和响应 {#throttling-mechanism-response}

当我们确定已违反限制时，我们会将此请求标记为特定的响应状态（HTTP 429请求过多），并指示您已使用在时间间隔内分配给用户设备（IP地址）的所有令牌。

限制在前429个响应中的一秒后过期。 每个收到429响应的应用程序都必须等待至少1秒才能生成新请求。

所有客户应用程序都应适当处理“429请求过多”响应。

以下是429响应消息的示例：

```
HTTP/2 429
date: Tue, 20 Feb 2024 11:21:53 GMT
content-type: text/html
content-length: 166
set-cookie: AWSALB=Btl/GzifUpMhUh+TQK63kU4i+gcJOIvAICVLnHTWt5pkrevNsMSQ5DMwM9KlRkNQ0UlXHIDbQoxDua0oVYYFKC8PDwxQjOuuRzxX2fozM+Jcazl2DSfaR7hU2mt2; Expires=Tue, 27 Feb 2024 11:21:53 GMT; Path=/
set-cookie: AWSALBCORS=Btl/GzifUpMhUh+TQK63kU4i+gcJOIvAICVLnHTWt5pkrevNsMSQ5DMwM9KlRkNQ0UlXHIDbQoxDua0oVYYFKC8PDwxQjOuuRzxX2fozM+Jcazl2DSfaR7hU2mt2; Expires=Tue, 27 Feb 2024 11:21:53 GMT; Path=/; SameSite=None; Secure
server: openresty
access-control-allow-credentials: true
access-control-allow-methods: POST,GET,OPTIONS,DELETE
access-control-allow-headers: ap_11,ap_42,ap_z,ap_19,ap_21,ap_23,authorization,content-type,pass_sfp,AP-Session-Identifier,AP-Device-Identifier,AP-SDK-Identifier,X-Device-Info
access-control-expose-headers: pass_sfp,Authzf-Error-Code,Authzf-Sub-Error-Code,Authzf-Error-Details
p3p: CP="NOI DSP COR CURa ADMa DEVa OUR BUS IND UNI COM NAV STA"

<html>
<head><title>429 Too Many Requests</title></head>
<body>
<center><h1>429 Too Many Requests</h1></center>
<hr><center>openresty</center>
</body>
</html>
```

## 影响和所需的更改

### 传递X-Forwarded-For标头

使用自定义实施（包括服务器到服务器）与Pass Authentication API进行交互的客户应确保他们能够捕获其用户IP地址并正确转发该地址，使用X-Forwarded-For标头进一步传递身份验证API。

有关更多详细信息，请参阅[此处](legacy/rest-api-v1/cookbooks/rest-api-cookbook-servertoserver.md)。

### 响应新的响应代码

使用自定义实施（包括服务器到服务器）与传递身份验证API进行交互的客户应确保在收到429个过多请求后发出的任何后续调用包含至少1秒的等待时间。 这一等待期确保有机会改变这一机制并获得有效的业务响应。

## 限制方案示例

| 自第一次请求以来的时间 | 已收到响应 | 说明 |
|--------------------------|-----------------------------------|-----------------------------------------------------------------------------------------------------------|
| 第二个0 | 呼叫接收成功状态代码 | 从限制消耗了1个调用 |
| 第二个0.3 | 呼叫接收成功状态代码 | 从限制消耗了1个调用，并有1个调用标记为突发 |
| 第二个0.6 | 呼叫接收成功状态代码 | 从限制消耗了1个调用，并有2个调用标记为突发 |
| 第二个0.9 | 呼叫接收成功状态代码 | 从限制消耗了1个调用，并有3个调用标记为突发 |
| 第二个1.2 | 呼叫接收成功状态代码 | 从限制消耗了2个调用，并有3个调用标记为突发 |
| 第二个1.3 | 呼叫接收成功状态代码 | 从限制消耗了2个调用，标记为突发的4个调用 |
| 第二个1.4 | 呼叫接收成功状态代码 | 从限制消耗了2个调用，且有5个调用标记为突发 |
| 第二个1.5 | 呼叫接收成功状态代码 | 从限制消耗了2个调用，并有6个调用标记为突发 |
| 第二个1.6 | 呼叫接收成功状态代码 | 从限制消耗了2个调用，并有7个调用标记为突发 |
| 第二个1.7 | 呼叫接收成功状态代码 | 从限制消耗了2个调用，并有8个调用标记为突发 |
| 第二个1.8 | 呼叫接收成功状态代码 | 从限制消耗了2个调用，并有9个调用标记为突发 |
| 第二个2.1 | 呼叫接收成功状态代码 | 从限制消耗了3个调用，并有9个调用标记为突发 |
| 第二个2.2 | 呼叫接收成功状态代码 | 从限制中消耗了3个调用，并有10个调用标记为突发 |
| 第二个2.4 | 呼叫接收429状态代码 | 从限制消耗了3个调用，10个调用标记为突发，1个调用接收“429请求过多” |
| 第二个2.6 | 呼叫接收429状态代码 | 从限制消耗了3个调用，10个调用标记为突发，2个调用接收“429请求过多” |
| 第二个2.8 | 呼叫接收429状态代码 | 从限制消耗了3个调用，10个调用标记为突发，3个调用接收“429请求过多” |
| 第二个3.1 | 呼叫接收成功状态代码 | 从限制消耗了4个调用，10个调用标记为突发，3个调用接收“429请求过多” |
