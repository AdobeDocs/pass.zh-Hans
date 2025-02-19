---
title: Adobe Pass Authentication Android 3.7.3发行说明
description: Adobe Pass Authentication Android 3.7.3发行说明
exl-id: f335357e-c209-428d-af2a-2181551447d4
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '142'
ht-degree: 0%

---

# Adobe Pass Authentication Android 3.7.3发行说明 {#android-sdk-373-rn}

>[!IMPORTANT]
>
> 确保随时了解汇总在[产品公告](/help/authentication/product-announcements.md)页中的最新Adobe Pass身份验证产品公告和停用时间表。

本页介绍了此版本的新增功能、更改和已知问题：

## 内部版本号 {#build-number-373}

Adobe Pass身份验证： Android 3.7.3

发行日期：**09/19/2023**

## 发行版概述 {#release-overview-373}

* 更改了对Android 14和面向API级别34的应用程序的支持
   * 添加[Android 14运行时注册的广播接收器](https://developer.android.com/about/versions/14/behavior-changes-14#runtime-receivers-exported)所需的标志。
* 修复在模拟器API 32及更高版本上无法打开ChromeCustomTabs以进行MVPD登录
   * 注意：SDK &lt;3.7.3上此问题的解决方法是：在模拟器上打开Chrome应用程序，并在尝试进行MVPD登录之前完成设置

## 发行包 {#release-package-373}

您可以从[此处](https://tve.zendesk.com/hc/en-us/articles/204963219-Android-Native-AccessEnabler-Library)下载Android SDK v3.7.3。
