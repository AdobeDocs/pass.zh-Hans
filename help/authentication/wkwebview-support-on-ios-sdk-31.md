---
title: iOS SDK 3.1+上的WKWebView支持
description: iOS SDK 3.1+上的WKWebView支持
exl-id: 90062be0-1a0a-44ae-8d8e-f4d97a92b17a
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '311'
ht-degree: 0%

---

# iOS SDK 3.1+ {#wkwebview-support-on-ios-sdk-3.1}上的WKWebView支持

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

</br>

**由于Apple在iOS上弃用UIWebView，我们已更新支持WKWebView的iOS SDK 3.1。**

## 兼容性 {#compatibility}

从iOS SDK版本3.1开始，实施人员现在可以交替使用WKWebView或UIWebView。 由于Apple已弃用UIWebView，因此应用程序应迁移到WKWebView，以避免将来的iOS版本出现问题。

请注意，迁移仅意味着使用WKWebView切换UIWebView类，没有针对Adobe的AccessEnabler的具体工作要做。

## 已知问题 {#known-issues}

Adobe的AccessEnabler使用隐藏的内部UIWebView实例对某些MVPD执行“[被动身份验证](/help/authentication/sso-passive-authn.md)”。 “被动”流对于要求为每个请求者ID进行身份验证的MVPD很有用，并且通过该流，那些在多个iOS应用程序中使用同一团队ID以模拟SSO体验(AdobeSSO)的程序员将从中受益。 此功能当前由有限数量的MVPD使用。

该功能使用了UIWebView的行为，允许Adobe捕获身份验证Cookie并在“被动”流期间重放它们。 WKWebView引入了更强的安全性，可阻止Adobe捕获在登录时设置的Cookie，并使用WKWebView的隐藏实例重放它们。 由于此安全改进，并且考虑到“被动”流仅惠及非常特定的实施方案（多个应用程序使用相同的团队ID）中的有限的一组MVPD，Adobe删除了使用Web视图进行身份验证的MVPD的“被动身份验证”功能。

该功能仍然适用于配置为使用SFSafariViewController的MVPD，但请注意，在这种情况下，用户将能够看到“被动”身份验证，因为SFSafariViewController不能以“隐藏”方式使用。
