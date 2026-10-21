---
title: Adobe Pass Authentication 3.10.0发行说明
description: Adobe Pass Authentication 3.10.0发行说明
hold: true
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: b754c97fc3c045e9511d07e22e0cc5387306a67d
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 0%
---
# Adobe Pass Authentication 3.10.0发行说明 {#authn-3100-rn}

>[!IMPORTANT]
>
> 确保随时了解汇总在[产品公告](/help/authentication/product-announcements.md)页中的最新Adobe Pass身份验证产品公告和停用时间表。

本页介绍了此版本的新增功能、更改和已知问题：

## 服务器端和Web客户端 {#server-side-web-clients-3100}

* [内部版本号](#build-number-3100)
* [发行版概述](#release-overview-3100)

### 内部版本号 {#build-number-3100}

Adobe Pass身份验证： adobe-pass-**3.10.0**\
发行日期：**2026年10月27日至2026年10月29日**

### 发行版概述 {#release-overview-3100}

此版本重点针对REST API V2和MVPD配置管理的单点登录控件、用户元数据增强和可靠性改进。

#### 增强功能

* 为服务令牌单点登录(SSO)添加了集成级别控制。 仅当为集成显式启用服务令牌SSO时，才会使用服务令牌SSO。
* 在身份验证、授权和印前检查期间添加了对自定义用户元数据（例如`packageType`）的支持。 REST API V2授权和预检决策现在包含配置的用户元数据，包括缓存的响应，同时遵循元数据访问和加密设置。

#### 错误修复

* 修复了对于通过SAML使用OAuth2 MVPD的集成，导致REST API V2 Apple SSO配置文件请求失败并出现无效SAML响应错误的问题。
* 修复了同一资源的预检和授权决策可能会覆盖彼此缓存的过期时间和用户元数据的问题。
* 修复了删除代理的MVPD时会保留未引用的平台设置的问题，该问题会阻止后续配置保存。
