---
title: Cookie更新 — SameSite和Secure标记
description: Cookie更新 — SameSite和Secure标记
exl-id: cc1f60fd-fa64-48cb-a185-dba562a54c33
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '910'
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

从Chrome版本80（版本82除外）开始，未指定 *SameSite* 属性将被视为是 *SameSite=Lax*. 因此，需要在跨站点上下文中交付的Cookie必须明确指定 *SameSite=None*，并且还必须使用 *安全* 属性和传送于 *HTTPS*. 有关这些更新的更多详细信息，请参阅官方的chromium页面： <https://www.chromium.org/updates/same-site> 以及来自 <https://web.dev/samesite-cookies-explained/>.


### Adobe Pass身份验证更新 {#Pass-Updates}

为了与某些平台和版本的Adobe Pass Authentication SDK结合使用，Adobe Pass身份验证服务当前依赖于两个Cookie（从浏览器的角度而言被视为第三方Cookie），包括Chrome。 因此，为了遵循即将到来的更改并在跨站点上下文中继续从这些旧版SDK提供这些Cookie，Adobe Pass身份验证服务将在中实施所需的更改 *adobe-pass-2.55.1* 版本。

这些更改来自 *adobe-pass-2.55.1* 版本涉及添加 *安全* 和 *SameSite=None* 使用从版本80及更高版本（版本82除外）开始的Chrome浏览器时，其所有Cookie的属性会传递回所有Adobe Pass身份验证SDK。

下一部分将介绍当一个用户使用Chrome浏览器80及更高版本（版本82除外）时平台和Adobe Pass身份验证SDK版本列表的一些潜在问题。

## 疑难解答 {#Troubleshooting}

浏览本节时，请记住，所有Adobe Pass身份验证服务Cookie必须具有 *安全* 属性集于 *adobe-pass-2.55.1* 版本，而 *SameSite=None* 只能为Chrome浏览器版本80及更高版本（版本82除外）设置属性。


### 一般故障排除 {#General}

1. 请注意，某些用户代理已知与 *SameSite=None* 属性。

   - 从Chrome 51到Chrome 66的Chrome版本（两端包含）。 这些Chrome版本将拒绝包含 *SameSite=None*. 这也会影响旧版本的Chromium派生浏览器以及Android WebView。 根据当时Cookie规范的版本，此行为是正确的，但随着规范中新增的“无”值，此行为已在Chrome 67及更高版本中更新。 (在Chrome 51之前，SameSite属性完全被忽略，所有Cookie都被视为与其相同 *SameSite=None*.)
   - Android上12.13.2版之前的UC浏览器版本。旧版本会拒绝包含以下内容的Cookie： *SameSite=None*. 根据当时Cookie规范的版本，此行为是正确的，但随着规范中新增的“无”值，此行为已在较新版本的UC浏览器中更新。
   - macOS 10.14上的Safari和嵌入式浏览器的版本以及iOS 12上的所有浏览器。 这些版本会错误地处理标有 *SameSite=None* 好像它们被标记了一样 *SameSite=严格*. 此错误已在较新版本的iOS和MacOS上修复。


1. 请注意， Cookie具有 *安全* 属性必须发送过 *HTTPS*，否则，Cookie将不会访问Adobe Pass身份验证服务。

   - AccessEnabler JavaScript SDK：
      - 强制要求与 *sp.auth.adobe.com* 用途 *HTTPS* 版本 *2.35* 和 *3.5.0*，然后才引入动态客户端注册。
   - AccessEnabler iOS/tvOS SDK：
      - 强制要求与 *sp.auth.adobe.com* 用途 *HTTPS* 之前的版本 *3.0.0*，然后才引入动态客户端注册。
   - AccessEnabler Android SDK：
      - 强制要求与 *sp.auth.adobe.com* 用途 *HTTPS* 之前的版本 *3.0.0*，然后才引入动态客户端注册。
   - AccessEnabler FireOS SDK：
      - 强制要求与 *sp.auth.adobe.com* 用途 *HTTPS* for version *2.0.4*.

</br>

### AccessEnabler JavaScript SDK版本2.35疑难解答 {#235-Troubleshooting}

在Chrome 80及更高版本（版本82除外）中，用户的身份验证流程可能会受到影响。 为了确保用户不会因上述更新而遇到身份验证问题，可以：

- 检查 *JSESSIONID* Cookie在浏览器中设置，并具有 *SameSite=None* 和 *安全* 属性集。
- 检查 *JSESSIONID* 来自的Cookie *https://sp.auth.adobe.com/authenticate/saml* 网络请求与 *JSESSIONID* 来自的Cookie *https://sp.auth.adobe.com/session* 网络请求。


### AccessEnabler JavaScript SDK 3.5.0版疑难解答 {#350-Troubleshooting}

在Chrome 80及更高版本（版本82除外）中，用户的身份验证流程可能会受到影响。 为了确保用户不会因上述更新而遇到身份验证问题，可以：

- 检查 *JSESSIONID* Cookie在浏览器中设置，并具有 *SameSite=None* 和 *安全* 属性集。
- 检查 *JSESSIONID* 来自的Cookie *https://sp.auth.adobe.com/authenticate/saml* 网络请求与 *JSESSIONID* 来自的Cookie *https://sp.auth.adobe.com/session* 网络请求。
- 检查 *pass\_sfp* Cookie在浏览器中设置，并具有 *SameSite=None* 和 *安全* 属性集。
- 检查 *pass\_sfp* Cookie在中设置 *https://sp.auth.adobe.com/session* 网络请求。


在Chrome 80及更高版本（版本82除外）中，用户的授权流可能会受到影响。 为了确保用户没有观看受保护资源的问题，在成功进行身份验证后，由于进行了上述更新，用户可以：

- 检查 *pass\_sfp* Cookie在浏览器中设置，并具有 *SameSite=None* 和 *安全* 属性集。
- 检查 *pass\_sfp* Cookie在中设置 *https://sp.auth.adobe.com/adobe-services/authorize* 网络请求。
- 检查 *pass\_sfp* Cookie在中设置 *https://sp.auth.adobe.com/adobe-services/shortAuthorize* 网络请求。
