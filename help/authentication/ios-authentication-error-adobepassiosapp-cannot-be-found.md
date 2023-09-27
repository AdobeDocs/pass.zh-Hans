---
title: iOS身份验证错误 — 找不到adobepass.ios.app
description: iOS身份验证错误 — 找不到adobepass.ios.app
exl-id: cd97c6fb-f0fa-45c2-82c1-f28aa6b2fd12
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 0%

---

# iOS身份验证错误 — 找不到adobepass.ios.app {#ios-authentication-error-adobepass.ios.app-cannot-be-found}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## 问题 {#issue}

用户正在经历身份验证流程，当他们成功地向提供商输入凭据后，他们会被重定向回错误页面、搜索页面或其他自定义页面，告知他们 `adobepass.ios.app` 无法找到/解析。

## 说明 {#explanation}

在iOS上， `adobepass.ios.app` 用作最终重定向URL，以指示AuthN流已完成。 此时，应用程序需要向AccessEnabler发出请求，以获取身份验证令牌并完成身份验证流程。

问题是 `adobepass.ios.app` 不存在，并且会在中触发错误消息 `webView`. 旧版iOS DemoApp假定此错误将始终在AuthN流结束时触发，并设置为相应地处理此错误(`indidFailLoadWithError`)。

**注意：** 此问题已在DemoApp的更高版本(随iOS SDK下载提供)中修复。

不幸的是，这一假设并不正确。 有一些所谓的“智能” DNS或代理服务器不仅传递所引发的错误，而且会执行以下操作之一：

- 创建自定义错误页面
- 转发到搜索页面或其他类型的客户页面或门户。

在这些情况下，返回到iOS webView的响应将是对webView而言完全有效的响应，并且不会触发旧DemoApp所依赖的错误。

## 解决方案 {#solution}

请勿做出与DemoApp相同的假设。 相反，在执行请求之前截获该请求(在 `shouldStartLoadWithRequest`)，并正确处理它。

在执行请求之前截获请求的示例：

```obj-c
- (BOOL)webView:(UIWebView*)localWebView shouldStartLoadWithRequest:(NSURLRequest*)request navigationType:(UIWebViewNavigationType)navigationType {

NSString *absolutePath = [[request URL] absoluteString]; 
if ([absolutePath isEqualToString:ADOBEPASS_REDIRECT_URL] && ![APP_DELEGATE getAuthenticationWasCalled]) {

// user was logged ok => call getAuthenticationToken() 
[APP_DELEGATE setGetAuthenticationWasCalled:YES]; 
[[APP_DELEGATE accessEnabler] getAuthenticationToken];
return NO;

}

return YES;

}
```

需要注意以下几点：

- 从不使用 `adobepass.ios.app` 直接访问代码中的任意位置。 改为使用常量 `ADOBEPASS_REDIRECT_URL`
- 此 `return NO;` 语句将阻止加载页面
- 绝对要确保 `getAuthenticationToken` 调用在您的代码中只调用一次。 多次调用 `getAuthenticationToken` 将导致未定义的结果。
