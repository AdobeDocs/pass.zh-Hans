---
title: Adobe Pass Authentication 3.2.0发行说明
description: Adobe Pass Authentication 3.2.0发行说明
exl-id: 4008512a-3155-4d96-9988-da0b0a496612
source-git-commit: 13b0bb640aa599109e8c2f68d1e16fbdc3840951
workflow-type: tm+mt
source-wordcount: '190'
ht-degree: 0%

---

# Adobe Pass Authentication 3.2.0发行说明 {#authn-320-rn}

>[!IMPORTANT]
>
> 确保随时了解汇总在[产品公告](/help/authentication/product-announcements.md)页中的最新Adobe Pass身份验证产品公告和停用时间表。

本页介绍了此版本的新增功能、更改和已知问题：

## 服务器端和Web客户端 {#server-side-web-clients-320}

* [内部版本号](#build-number-320)
* [发行版概述](#release-overview-320)

### 内部版本号 {#build-number-320}

Adobe Pass身份验证： adobe-pass-**3.2.0**

发行日期：**06/10/2025 - 06/12/2025**

### 发行版概述 {#release-overview-320}

#### REST API v2

* 已为[会话API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)响应中缺少参数的情况添加了新原因`missing_parameters_fallback`。
* 新字段“device”已添加到[会话API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)响应。

#### 新增功能

* 扩展降级API以添加在单个调用中为多个MVPD应用降级规则的功能

#### 错误修复

* 修复了在用户元数据处理失败的情况下阻止身份验证流成功完成的问题。
* 修复了授权原因计算不正确的问题。
* 修复了配置文件响应中不存在upstreamUserID的问题。
* 修复了导致身份验证请求不包含REST API V2启动身份验证请求的范围信息的问题。
