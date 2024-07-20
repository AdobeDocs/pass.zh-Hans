---
title: Cookie更新 — SameSite和Secure标记
description: Cookie更新 — SameSite和Secure标记
exl-id: cc1f60fd-fa64-48cb-a185-dba562a54c33
source-git-commit: 2dbb45aebb1a00863a9344114963f6df95763dfc
workflow-type: tm+mt
source-wordcount: '933'
ht-degree: 0%

---

# Cookie更新 — SameSite和Secure标记 {#cookies-updates---samesite-and-secure-flags}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

</br>


## 更新 {#Updates}

本节重点介绍Chrome浏览器和Adobe Pass身份验证引入的用于处理第三方Cookie的更改。



### Chrome 80更新 {#Chrome}

从Chrome版本80开始（版本82除外），未指定&#x200B;*SameSite*&#x200B;属性的Cookie将被视为&#x200B;*SameSite=Lax*。 因此，需要在跨站点上下文中交付的Cookie必须明确指定&#x200B;*SameSite=None*，还必须使用&#x200B;*Secure*&#x200B;属性标记并通过&#x200B;*HTTPS*&#x200B;交付。 有关这些更新的更多详细信息，可从官方的chromium页面<https://www.chromium.org/updates/same-site>以及<https://web.dev/samesite-cookies-explained/>中阅读。


### Adobe Pass身份验证更新 {#Pass-Updates}

为了与某些平台和版本的Adobe Pass Authentication SDK结合使用，Adobe Pass身份验证服务当前依赖于两个Cookie(从浏览器的角度来说，它们被视为第三方Cookie，包括Chrome)。 因此，为了遵循即将进行的更改并在跨站点上下文中继续从这些旧版SDK提供这些Cookie，Adobe Pass身份验证服务在&#x200B;*adobe-pass-2.55.1*&#x200B;版本中实施所需的更改。

从&#x200B;*adobe-pass-2.55.1*&#x200B;版本进行的这些更改涉及在使用80版及更高版本（82版除外）的Chrome浏览器时，为传递回所有Adobe Pass身份验证SDK的所有Cookie添加&#x200B;*Secure*&#x200B;和&#x200B;*SameSite=None*&#x200B;属性。

下一部分将介绍当一个用户使用Adobe Pass Browser 80及更高版本（版本82除外）时，平台和Chrome Authentication SDK版本列表可能会出现的一些问题。

## 故障排除 {#Troubleshooting}

浏览本节时，请记住，对于所有浏览器，所有Adobe Pass身份验证服务Cookie都必须在&#x200B;*adobe-pass-2.55.1*&#x200B;版本中设置了&#x200B;*Secure*&#x200B;属性，而对于Chrome浏览器版本80及更高版本（版本82除外），则必须设置&#x200B;*SameSite=None*&#x200B;属性。


### 一般故障排除 {#General}

1. 请注意，某些用户代理已知与&#x200B;*SameSite=None*&#x200B;属性不兼容。

   - 从Chrome 51到Chrome 66的Chrome版本（两端包含）。 这些Chrome版本将拒绝具有&#x200B;*SameSite=None*&#x200B;的Cookie。 这也会影响旧版本的Chromium派生浏览器以及Android WebView。 根据当时Cookie规范的版本，此行为是正确的，但随着规范中新增的“无”值，此行为已在Chrome 67及更高版本中更新。 (在Chrome 51之前，SameSite属性被完全忽略，所有Cookie都被视为是&#x200B;*SameSite=None*。)
   - Android上12.13.2版之前的UC浏览器版本。旧版本将拒绝&#x200B;*SameSite=None*&#x200B;的Cookie。 根据当时Cookie规范的版本，此行为是正确的，但随着规范中新增的“无”值，此行为已在较新版本的UC浏览器中更新。
   - macOS 10.14上的Safari和嵌入式浏览器的版本以及iOS 12上的所有浏览器。 这些版本将错误地将标记为&#x200B;*SameSite=None*&#x200B;的Cookie视为标记为&#x200B;*SameSite=Strict*。 此错误已在较新版本的iOS和MacOS上修复。


1. 需要注意的是，具有&#x200B;*Secure*&#x200B;属性的Cookie必须通过&#x200B;*HTTPS*&#x200B;发送，否则Cookie将无法访问Adobe Pass身份验证服务。

   - AccessEnabler JavaScript SDK：
      - 在引入动态客户端注册之前，与&#x200B;*sp.auth.adobe.com*&#x200B;的通信必须使用&#x200B;*HTTPS*&#x200B;才能使用版本&#x200B;*2.35*&#x200B;和&#x200B;*3.5.0*。
   - AccessEnabler iOS/tvOS SDK：
      - 在引入动态客户端注册之前，与&#x200B;*sp.auth.adobe.com*&#x200B;的通信必须对&#x200B;*3.0.0*&#x200B;之前的版本使用&#x200B;*HTTPS*。
   - AccessEnabler Android SDK：
      - 在引入动态客户端注册之前，与&#x200B;*sp.auth.adobe.com*&#x200B;的通信必须对&#x200B;*3.0.0*&#x200B;之前的版本使用&#x200B;*HTTPS*。
   - AccessEnabler FireOS SDK：
      - 与&#x200B;*sp.auth.adobe.com*&#x200B;的通信必须使用版本&#x200B;*2.0.4*&#x200B;的&#x200B;*HTTPS*。

</br>

### AccessEnabler JavaScript SDK版本2.35故障排除 {#235-Troubleshooting}

Chrome 80及更高版本（版本82除外）中可能会影响用户的身份验证流程。 为了确保用户不会因上述更新而遇到身份验证问题，可以：

- 检查浏览器中是否设置了&#x200B;*JSESSIONID* Cookie，并设置了&#x200B;*SameSite=None*&#x200B;和&#x200B;*Secure*&#x200B;属性。
- 检查&#x200B;*https://sp.auth.adobe.com/authenticate/saml*&#x200B;网络请求中的&#x200B;*JSESSIONID* Cookie是否与&#x200B;*https://sp.auth.adobe.com/session*&#x200B;网络请求中的&#x200B;*JSESSIONID* Cookie匹配。


### AccessEnabler JavaScript SDK版本3.5.0疑难解答 {#350-Troubleshooting}

Chrome 80及更高版本（版本82除外）中可能会影响用户的身份验证流程。 为了确保用户不会因上述更新而遇到身份验证问题，可以：

- 检查浏览器中是否设置了&#x200B;*JSESSIONID* Cookie，并设置了&#x200B;*SameSite=None*&#x200B;和&#x200B;*Secure*&#x200B;属性。
- 检查&#x200B;*https://sp.auth.adobe.com/authenticate/saml*&#x200B;网络请求中的&#x200B;*JSESSIONID* Cookie是否与&#x200B;*https://sp.auth.adobe.com/session*&#x200B;网络请求中的&#x200B;*JSESSIONID* Cookie匹配。
- 检查浏览器中是否已设置&#x200B;*pass\_sfp* Cookie，是否已设置&#x200B;*SameSite=None*&#x200B;和&#x200B;*Secure*&#x200B;属性。
- 检查是否已在&#x200B;*https://sp.auth.adobe.com/session*&#x200B;网络请求中设置&#x200B;*pass\_sfp* Cookie。


在Chrome 80及更高版本（版本82除外）中，用户的授权流可能会受到影响。 为了确保用户没有观看受保护资源的问题，在成功进行身份验证后，由于进行了上述更新，用户可以：

- 检查浏览器中是否已设置&#x200B;*pass\_sfp* Cookie，是否已设置&#x200B;*SameSite=None*&#x200B;和&#x200B;*Secure*&#x200B;属性。
- 检查是否已在&#x200B;*https://sp.auth.adobe.com/adobe-services/authorize*&#x200B;网络请求中设置&#x200B;*pass\_sfp* Cookie。
- 检查是否已在&#x200B;*https://sp.auth.adobe.com/adobe-services/shortAuthorize*&#x200B;网络请求中设置&#x200B;*pass\_sfp* Cookie。
