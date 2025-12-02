---
title: Adobe Pass Authentication 3.5.0发行说明
description: 了解此版本的新增功能、更改和已知问题。
source-git-commit: 6ff46a124f5f3c78173028ae3efed68d71ee6e41
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 0%

---


# Adobe Pass Authentication 3.5.0发行说明

上次更新：2025年12月9日星期二00:00:00 GMT+0000（协调世界时）

* 主题：
* 身份验证

>[!IMPORTANT]
>
> 确保随时了解汇总在[产品公告](https://experienceleague.adobe.com/zh-hans/docs/pass/authentication/product-announcements)页中的最新Adobe Pass身份验证产品公告和停用时间表。

本页介绍了此版本的新增功能、更改和已知问题：

## 服务器端和Web客户端 {#server-side-web-clients-350}

* [内部版本号](#build-number-350)
* [发行版概述](#release-overview-350)

### 内部版本号 {#build-number-350}

Adobe Pass身份验证： adobe-pass-**3.5.0**\
发行日期：**2025年12月9日 — 2025年12月11日**

### 发行版概述 {#release-overview-350}

#### REST API v2

* 在ESM (“api”)中添加了一个新维度，以帮助客户构建报表，以根据实施类型区分事件：SDK、REST V1或REST V2。

#### 错误修复

* 修复了TVE功能板中设置的自定义性能下降消息未显示在REST API V2错误详细信息中的问题。
* 修复了REST API V2中的一个问题：当经过身份验证的配置文件过期时，未返回`authenticated_profile_expired`错误代码。
* 修复了REST API V2中的授权延迟计算和预检TTL值不正确的问题。
* 修复了DCR令牌过期时返回不一致的响应格式的问题。
