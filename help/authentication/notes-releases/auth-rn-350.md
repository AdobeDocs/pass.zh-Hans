---
title: Adobe Pass Authentication 3.5.0发行说明
description: 了解此版本的新增功能、更改和已知问题。
exl-id: b196f636-26a5-4974-903e-40b5f8b93a24
source-git-commit: 1cbddf081fc7d57a187c9701e4ade8593baf8759
workflow-type: tm+mt
source-wordcount: '273'
ht-degree: 0%

---

# Adobe Pass Authentication 3.5.0发行说明

上次更新：2025年12月9日星期二00:00:00 GMT+0000（协调世界时）

* 主题：
* 身份验证

>[!IMPORTANT]
>
> 确保随时了解汇总在[产品公告](https://experienceleague.adobe.com/en/docs/pass/authentication/product-announcements)页中的最新Adobe Pass身份验证产品公告和停用时间表。

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

## 维护更新 — 2026年2月 {#maintenance-update-february-2026}

Adobe Pass身份验证： adobe-pass-**3.5.0.5**\
发行日期：**02/24/2026 - 02/26/2026**

此维护更新版本包括增强系统可靠性和安全性的重要改进：

### 增强功能

* 改进了REST API V2中代理MVPD配置的身份验证降级处理，确保在MVPD服务中断期间具有更一致的行为。
* 增强了URL参数验证和重定向处理，以加强安全控制并改善整体系统完整性。
