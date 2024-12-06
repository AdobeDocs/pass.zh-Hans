---
title: Adobe Pass Authentication JavaScript 4.4.0发行说明
description: Adobe Pass Authentication JavaScript 4.4.0发行说明
exl-id: 28cc0ccc-7a1d-45bd-8455-26cfde25c5c5
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 0%

---

# Adobe Pass Authentication JavaScript 4.4.0发行说明 {#javascript-sdk-440-release-notes}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

本页介绍了此版本的新增功能、更改和已知问题：

## 内部版本号 {#build-no-javascript-sdk-440}

Adobe Pass身份验证： JavaScript 4.4.0

发行日期：**06/22/2021**


## 发行版概述 {#overview-javascript-sdk-440}

### 新增功能 {#new-features-javascript-sdk-440}

印前检查授权

* 新的预授权API — 这是要用于我们的预检授权功能的新API调用，它还为预检流引入了增强的错误报告。
* 此功能根据请求提供，因为它必须在Primetime身份验证配置中启用。 有关如何启用此功能的更多详细信息，请与您的TAM联系。
* 弃用checkPreauthorizedResources API。

平台识别

* 在所有SDK调用中添加AP-SDK-Identifier标头，以便更好地识别SDK类型和版本。

其他 

* 内部架构改进。


### 错误修复 {#bug-fixes-javascript-sdk-440}

* 修复在同时调用setRequestor和getAuthentication时生成的争用条件。
* 修复了阻止在暂存环境中正确加载权利的问题。
* 解决了阻止后台注销流在Safari浏览器上完成的问题，因此用户似乎在页面刷新之前仍保持身份验证状态。 引入了超时，当前设置为30秒，因此，如果在此期间没有来自Primetime身份验证服务器的响应，SDK将调用setAuthenticationStatus回调。

## 发行包 {#rel-pkg-javascript-sdk-440}

生产URL为：https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js

暂存URL为：https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js
