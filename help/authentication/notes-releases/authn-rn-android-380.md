---
title: Adobe Pass Authentication Android 3.8.0发行说明
description: Adobe Pass Authentication Android 3.8.0发行说明
source-git-commit: f7db5b71723c02b2924e73105769ab5b22e05bca
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 0%

---

# Adobe Pass Authentication Android 3.8.0发行说明 {#android-sdk-380-rn}

>[!IMPORTANT]
>
> 确保随时了解汇总在[产品公告](/help/authentication/product-announcements.md)页中的最新Adobe Pass身份验证产品公告和停用时间表。

本页介绍了此版本的新增功能、更改和已知问题：

## 内部版本号 {#build-number-380}

Adobe Pass身份验证： Android 3.8.0

发行日期：**09/18/2025**

## 发行版概述 {#release-overview-380}

* 修复了SDK存储广播接收器的漏洞。 恶意应用程序可能会提供虚假链接来查询Adobe令牌的共享存储。
但是，不会存储任何关键信息，任何用户受此漏洞影响的可能性很低。
   * 注意：作为更改的结果，用户将被注销。

## 发行包 {#release-package-380}

您可以从[此处](https://tve.zendesk.com/hc/en-us/articles/204963219-Android-Native-AccessEnabler-Library)下载Android SDK v3.8.0。
