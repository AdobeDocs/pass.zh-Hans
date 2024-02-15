---
title: 节流机构
description: 节流机构
source-git-commit: cbb45cae576332e2b63027992c597b834210988d
workflow-type: tm+mt
source-wordcount: '624'
ht-degree: 1%

---


# 节流机构 {#throttling-mechanism}

## 简介 {#introduction}

Adobe作为您的数据处理者，必须采取适当措施，确保客户的用户公平使用资源，并且服务不会充斥着不必要的API请求。 为此，我们建立了一个节流机制。
一个并发监控应用程序可以由多个用户使用，一个用户可以有多个会话。 因此，该服务将针对特定时间间隔内每个用户/会话接受的呼叫数配置限制。
当达到限制时，请求将标记为特定的响应状态（HTTP 429请求过多）。 在收到“429个过多请求”响应之后进行的任何后续调用应至少在一分钟的冷却期内完成，以确保其将获得有效的业务响应。

## 机制概述 {#mechanism-overview}

该机制可确定在特定时间间隔内每个并发监控端点接受的最大调用数。
一旦达到最大呼叫数，我们的服务将作出“429请求过多”的响应。 429响应“Expires”标头包含下一次调用被视为有效或限制过期的时间戳。 现在，限制在前429响应开始一分钟后过期。

配置了节流的端点包括：
1. 创建新会话：POST/sessions/{idp}/{subject}
2. 心率调用：POST/sessions/{idp}/{subject}/{sessionId}
3. 终止会话：DELETE/sessions/{idp}/{subject}/{sessionId}

可在两个级别上配置限制：
1. 会话：相同唯一 {sessionId} 参数已发送 `Heartbeat` 调用和 `Terminate a session` 呼叫。
2. 用户：相同唯一 {subject} 参数已发送 `Create a new session` 呼叫。

会话级别限制设置为1分钟内200个请求。\
用户级别限制设置为1分钟内200个请求。\
这两个限制（会话级别限制和用户级别限制）均可配置，并且我们将更新它们，以防通过有效的集成方案实现它们。 为此，我们建议您联系支持团队。

**会话级别限制方案：**

| 时间 | 请求发送至CM | 请求数 | 从CM收到的响应 | 说明 |
|-----------|-----------------------------------------|--------------------|------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| 第二个10 | POST/sessions/idp1/subject1/session1 | 50 | 所有呼叫均接收“202已接受” | 从限制消耗了50个调用 |
| 第二个50 | POST/sessions/idp1/subject1/session1 | 151 | 150个呼叫接收“202已接受”，1个呼叫接收“429请求过多” | 从限制消耗了200个呼叫，1个呼叫将收到429响应 |
| 第二个61 | DELETE/sessions/idp1/subject1/session1 | 1 | 1个调用收到“429请求太多” | 在限制内没有可用的调用 |
| 第二个70 | DELETE/sessions/idp1/subject1/session1 | 1 | 1个呼叫收到“202已接受” | 限制设置为200个可用调用，因为自第10秒起已过60秒 |

**用户级别限制方案：**

| 时间 | 请求发送至CM | 请求数 | 从CM收到的响应 | 说明 |
|-----------|------------------------------|--------------------|------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| 第二个10 | POST/sessions/idp1/subject1 | 50 | 50个来电接收“202已接受” | 从限制消耗了50个调用 |
| 第二个50 | POST/sessions/idp1/subject1 | 151 | 150个呼叫接收“202已接受”，1个呼叫接收“429请求过多” | 从限制消耗了200个呼叫，1个呼叫将收到429响应 |
| 第二个61 | POST/sessions/idp1/subject1 | 1 | 1个调用收到“429请求太多” | 在限制内没有可用的调用 |
| 第二个70 | POST/sessions/idp1/subject1 | 1 | 1个呼叫收到“202已接受” | 限制设置为200个可用调用，因为自第10秒起已过60秒 |

**429响应示例：**

```
HTTP/2 429
date: Thu, 15 Feb 2024 07:54:20 GMT
content-length: 0
vary: Origin
vary: Access-Control-Request-Method
vary: Access-Control-Request-Headers
cache-control: no-store
expires: Thu, 15 Feb 2024 07:54:41 GMT
strict-transport-security: max-age=31536000 ; includeSubDomains
x-xss-protection: 1; mode=block
x-frame-options: DENY
x-content-type-options: nosniff
```

## 客户集成建议 {#customer-integration-recommendations}

如果实施正确，客户将不会收到“429太多请求”响应。
不过，Adobe仍建议每位客户使用上述技术细节正确处理“429个太多请求”响应。 处理响应时，应使用“Expires”标头来确定何时发送下一个有效请求。
