---
title: 最低系统要求
description: 最低系统要求
exl-id: 57b21e2a-abd7-4b4b-85f1-25584a850e40
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 0%

---

# 最低系统要求 {#minimum-system-requirements}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。


## 概述 {#overview}

本文档介绍在支持的平台上实施Adobe Pass身份验证集成的当前软件和硬件要求。 下面列出的所有受支持的Web/移动浏览器和操作系统都将从绑定到商定SLA的Adobe Pass身份验证团队的全面支持中受益。

作为Adobe Pass身份验证团队，我们鼓励使用最新稳定版本的浏览器和操作系统；我们也承认存在不合规的/较旧的平台和浏览器，这些平台和浏览器当前正在使用中。 这些过时的设备仍可以正常运行，但会更加容易出错。

缓解这些过时平台上出现的任何问题的初始方法应该是升级到最新版本；这可以是操作系统版本、浏览器版本或已安装应用程序的版本。

Adobe Pass身份验证团队将尽最大努力解决这些平台上出现的任何问题。

Adobe Pass鼓励我们的客户和合作伙伴考虑升级到最新版本，以便在性能、效率和安全性改进之外，在任何潜在问题上都能从Adobe的完全支持中受益。


## 浏览器和操作系统要求 {#browser-OS-system-requirements}


| Web/移动浏览器(†) | 支持的版本 |
|---|---|
| Google Chrome | **70**&#x200B;或更高版本 |
| Mozilla Firefox | **57**&#x200B;或更高版本 |
| Apple Safari | **14**&#x200B;或更高版本 |
| Microsoft Edge | **100**&#x200B;或更高版本 |

(†)Adobe建议不要使用私密或无痕模式。

| 操作系统 | 支持的版本 |
|---|---|
| *Android* | **7.0** (Nougat)或更高版本 |
| *iOS* | **14**&#x200B;或更高版本 |
| *iPadOS* | **14**&#x200B;或更高版本 |
| *tvOS* | **14**&#x200B;或更高版本 |
| *触发操作系统* | **5 (Android 5.1)**&#x200B;或更高版本 |
| *Mac OS* | **10.13**&#x200B;或更高版本 |
| *Microsoft Windows* | **10**&#x200B;或更高版本 |




>[!NOTE]
>
>第三方Cookie — 在禁用第三方Cookie时，Adobe Pass身份验证权利流可能会失败。  此问题仅在修改浏览器设置时才会出现。 对于所有受支持的浏览器，Adobe Pass身份验证应在默认设置下正常运行。


## 无客户端(REST)实施的设备要求 {#general_clientless_reqs}


任何将通过无客户端实现使用Adobe Pass身份验证服务的设备都必须能够&#x200B;**：**

* 提供唯一的哈希设备ID。 如果设备不提供唯一的经过哈希处理的设备ID，则必须能够保留由Adobe Pass身份验证提供的唯一ID。 设备应该能够在其本地存储中永久保留唯一ID，并在调用Adobe Pass身份验证API时提供唯一ID作为设备ID。
* 使用HMAC-SHA1算法生成数字签名
* 设置任意HTTP标头
* 使用RESTful Web服务
* 解析XML和JSON数据格式
* 使用HTTPS发送流量
* 处理HTTP错误代码
