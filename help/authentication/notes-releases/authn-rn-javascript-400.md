---
title: Adobe Pass Authentication JavaScript 4.0.0发行说明
description: Adobe Pass Authentication JavaScript 4.0.0发行说明
exl-id: 2ded9ad8-56f7-44b5-87a2-12a195cd0829
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '258'
ht-degree: 0%

---

# Adobe Pass Authentication JavaScript 4.0.0发行说明 {#javascript-sdk-400-rn}

>[!IMPORTANT]
>
> 确保随时了解汇总在[产品公告](/help/authentication/product-announcements.md)页中的最新Adobe Pass身份验证产品公告和停用时间表。

本页介绍了此版本的新增功能、更改和已知问题：

## 内部版本号 {#build-number-400}

Adobe Pass身份验证： JavaScript 4.0.0

发行日期： **07/05/2018**

## 发行版概述 {#release-overview-400}

* 为动态客户端注册机制实施支持，这是一种跨平台的统一安全机制。 跨平台的安全访问管理可通过TVE仪表板由程序员自己完成。
* 消除对所有功能使用所有第三方Cookie和几乎所有第一方Cookie（除了仍使用第一方Cookie的个性化外，将来将删除它们），因此它与围绕第三方Cookie或总体Cookie的新兴浏览器策略更兼容且更符合未来需要(例如 Safari的ITP)。 请注意，以前的3.x版本仅使用第一方Cookie即可像预期的那样顺利实现流量，但此情况可能会随着新浏览器版本的发布而改变。
* 支持禁用预检授权检查的缓存。
* 此版本不向后兼容，且托管在新路径下： https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js 。 为了从这一新的JS SDK中获益，需要程序员执行一个迁移过程，以便使用新的动态客户端注册机制注册其Web应用程序并指向新的JS SDK URL。

## 发行包 {#release-package-400}

生产URL为：https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js

暂存URL为：https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js
