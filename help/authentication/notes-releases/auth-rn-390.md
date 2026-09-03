---
title: Adobe Pass Authentication 3.9.0发行说明
description: Adobe Pass Authentication 3.9.0发行说明
hold: true
source-git-commit: 5ca8f29764a07ddb68abb36accb12cfb3b68b72d
workflow-type: tm+mt
source-wordcount: '156'
ht-degree: 0%

---

# Adobe Pass Authentication 3.9.0发行说明 {#authn-390-rn}

>[!IMPORTANT]
>
> 确保随时了解汇总在[产品公告](/help/authentication/product-announcements.md)页中的最新Adobe Pass身份验证产品公告和停用时间表。

本页介绍了此版本的新增功能、更改和已知问题：

## 服务器端和Web客户端 {#server-side-web-clients-390}

* [内部版本号](#build-number-390)
* [发行版概述](#release-overview-390)

### 内部版本号 {#build-number-390}

Adobe Pass身份验证： adobe-pass-**3.9.0.1**\
发行日期：**09/08/2026 - 09/10/2026**

### 发行版概述 {#release-overview-390}

此版本重点介绍REST API V2和ESM量度改进。

#### 增强功能

* 改进了REST API V2合作伙伴单点登录，以确保为使用OAuth2配置的MVPD返回有效的身份验证请求。
* 改进了REST API V2决策，以在授权失败时返回明确的错误响应，而不是空响应。
* 改进了注册代码的生成，以防止出现视觉模糊的字符，使代码更易于阅读和正确输入。
* ESM功能板增强，支持印前验证AuthZ指标。
