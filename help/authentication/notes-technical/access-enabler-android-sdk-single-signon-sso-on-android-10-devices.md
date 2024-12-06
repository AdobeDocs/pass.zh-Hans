---
title: Android 10应用程序上的Access Enabler Android SDK单点登录(SSO)
description: Android 10应用程序上的Access Enabler Android SDK单点登录(SSO)
exl-id: dedade15-c451-4757-b684-d3728e11dd87
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '379'
ht-degree: 0%

---

# Android 10应用程序上的Access Enabler Android SDK单点登录(SSO) {#access-enabler-android-sdk-single-sign-on-sso-on-android-10-apps}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## 概述

使用Adobe Pass操作系统的设备可通过Access Enabler Android SDK在Android Authentication支持的应用程序之间进行单点登录(SSO)。 为了在Android设备上提供单点登录(SSO)，Access Enabler Android SDK版本3.2.1（最新）及之前的版本使用保存在Android存储实施中的共享数据库文件，可供所有Adobe Pass身份验证支持的应用程序访问。

但是，Google在最新的Android 10版本中做出了一些更改，“为了使用户能够更好地控制其文件并限制文件混乱，默认情况下，面向Android 10 （API级别29）及更高版本的应用程序被授予对外部存储设备或作用域存储的作用域访问权限。 此类应用只能查看其应用特定的目录`\[...\]`。 有关这些Android 10存储更改的更多详细信息，请参阅Android](https://developer.android.com/training/data-storage/files/external-scoped)的[数据和文件存储文档。

由于这些更改，Access Enabler Android版本&#x200B;**3.2.1 SDK（最新）**&#x200B;和先前版本提供的单点登录(SSO)可能会在Android 10设备上受到影响，具体如下节所述。

## 行为

根据您应用程序的&#x200B;**[!UICONTROL target SDK level]**&#x200B;或&#x200B;**android：requestLegacyExternalStorage**&#x200B;清单属性的使用情况，Access Enabler Android版本3.2.1 SDK（最新）和先前版本提供的单点登录(SSO)当前将按如下方式运行：

- 您的应用程序目标&#x200B;**Android 9 （API级别28）**&#x200B;或更低的&#x200B;**-\>**&#x200B;单点登录(SSO) **将工作**
- 应用程序以&#x200B;**Android 10** **（API级别29）**&#x200B;为目标，并&#x200B;**将**&#x200B;应用程序清单文件&#x200B;**-\>**&#x200B;单点登录(SSO) **中的** requestLegacyExternalStorage的值设置为true ****&#x200B;将起作用
- 应用程序以&#x200B;**Android 10** **（API级别29）**&#x200B;为目标，并且&#x200B;**未将**&#x200B;应用程序清单文件&#x200B;**-\>**&#x200B;单点登录(SSO) **中的** requestLegacyExternalStorage的值设置为true ****&#x200B;将无法工作

>[!TIP]
>
> 在Adobe Pass Authentication Access Enabler Android SDK与作用域存储完全兼容之前，您可以根据应用程序的Target SDK级别或requestLegacyExternalStorage清单属性临时选择退出，如公共[Android文档](https://developer.android.com/training/data-storage/files/external-scoped#opt-out-of-scoped-storage)中所述。
