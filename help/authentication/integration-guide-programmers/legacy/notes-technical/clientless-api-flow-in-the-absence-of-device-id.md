---
title: 缺少设备ID时的无客户端API流
description: 缺少设备ID时的无客户端API流
exl-id: 6549a6d6-03a9-4d95-99fb-d3ada832323d
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 0%

---

# （旧版）缺少设备ID时的无客户端API流 {#clientless-api-flow-in-the-absence-of-device-id}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

>[!IMPORTANT]
>
> 确保随时了解汇总在[产品公告](/help/authentication/product-announcements.md)页中的最新Adobe Pass身份验证产品公告和停用时间表。

</br>


## 问题

并非所有智能设备应用程序都能提供唯一的设备ID。  由于deviceId是必需参数，因此，如果未传递此参数，服务将返回400错误。


## 临时解决方案/解决方法

对于没有设备ID的客户端：

1. 使用`deviceId=dummy`第一次调用注册代码服务
1. 从响应中提取UUID。 UUID在注册代码响应（XML和JSON响应格式）的“id”元素中可用。
1. 再次调用注册服务。 这一次，传递`deviceId=<uuid obtained in step #2>`
1. 在控制台UI上显示在步骤3中获得的注册码


完成这些步骤后，Adobe Pass身份验证将使用UUID作为设备ID。 将此设备ID (UUID)存储在设备的本地存储中。 如果用户生成新的注册码，您应再次运行步骤1至4，然后将之前存储的设备ID (UUID)替换为新的设备ID。



## 永久解决方案

Adobe将在未来版本中更改此设置，方法是在创建注册代码时将`deviceId`设为可选有效负载，并在`deviceId`不存在时将UUID而不是`deviceId`用作令牌键。

<!--
## Related Information

- [Clientless API Reference](/help/authentication/rest-api-reference.md)
-->
