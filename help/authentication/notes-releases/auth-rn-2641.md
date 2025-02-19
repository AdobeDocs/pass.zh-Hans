---
title: Adobe Pass Authentication 2.64.1发行说明
description: Adobe Pass Authentication 2.64.1发行说明
exl-id: b0edbd90-ebb5-40a7-9034-1699dccfadb5
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '133'
ht-degree: 0%

---

# Adobe Pass Authentication 2.64.1发行说明 {#authn-264-rn}

>[!IMPORTANT]
>
> 确保随时了解汇总在[产品公告](/help/authentication/product-announcements.md)页中的最新Adobe Pass身份验证产品公告和停用时间表。

本页介绍了此版本的新增功能、更改和已知问题：

## 服务器端和Web客户端 {#server-side-web-clients-2641}

* [内部版本号](#build-number-2641)
* [发行版概述](#release-overview-2641)

### 内部版本号 {#build-number-2641}

Adobe Pass身份验证： adobe-pass-**2.64.1**

发行日期：**01/31/2023 - 02/02/2023**

### 发行版概述 {#release-overview-2641}

此版本添加了以下内容：

* 能够阻止来自MVPD的未经请求的身份验证响应，其中SAML断言中缺少“in_response_to”参数。
* 改进了对redirect_url参数的验证，以符合安全要求。
* 改进了使用从初始设备提供的信息来记录用于第二屏幕认证请求的设备信息。
