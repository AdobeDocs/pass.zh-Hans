---
title: 动态客户端注册
description: 动态客户端注册
exl-id: 9bc2597d-b634-4542-849b-8e91a76cb8da
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 0%

---

# 动态客户端注册 {#dynamic-client-registration}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## 上下文 {#context}

为了与现代安全实践保持一致，改进了UX和平台所有者
要求，Adobe Pass Authentication Android SDK和iOS SDK正在朝着采用[Android Chrome自定义选项卡](https://developer.chrome.com/multidevice/android/customtabs){target=_blank}和[Apple Safari视图控制器](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller){target=_blank}的方向发展。

当前AdobePass实施使用平台特定的Web视图，以提供用于显示MVPD登录页面的Web环境。 这些Web视图不会与平台浏览器共享凭据管理，因此，在使用Adobe Pass身份验证应用程序时，用户无法使用浏览器保存的密码。 此外，出于安全原因，一些平台正逐渐弃用WebView控制器来执行身份验证任务。 Google和Apple都提供替代选项，如“Chrome自定义选项卡”和“Safari视图控制器”。 这些基本上是其各自浏览器的单一使用选项卡。 Adobe Pass身份验证将在2018年采用这些新组件。

## 详细信息 {#details}

目前，Adobe Pass身份验证识别和注册应用程序的方法有两种：

* 基于浏览器的客户端通过允许的域列表进行注册
* 本机应用程序客户端(如iOS和Android应用程序)通过签名的请求者机制进行注册

为了解决Chrome自定义选项卡和Safari视图控制器的新流程，Adobe Pass提出了一种新的客户端注册机制，以注册新的应用程序。 此机制将允许对应用程序进行更安全和更细粒度的控制，并且可用于在所有平台上注册应用程序。

<!--
## Related Information

- [Dynamic Client Registration API](/help/authentication/dynamic-client-registration-api.md)
- [Dynamic Client Registration Management](/help/authentication/dynamic-client-registration-management.md)
-->
