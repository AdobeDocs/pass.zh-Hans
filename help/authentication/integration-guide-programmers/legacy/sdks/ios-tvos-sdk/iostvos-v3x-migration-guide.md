---
title: iOS/tvOS v3.x迁移指南
description: iOS/tvOS v3.x迁移指南
exl-id: 4c43013c-40af-48b7-af26-0bd7f8df2bdb
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '582'
ht-degree: 0%

---

# （旧版） iOS/tvOS v3.x迁移指南 {#iostvos-v3x-migration-guide}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

>[!IMPORTANT]
>
> 确保随时了解汇总在[产品公告](/help/authentication/product-announcements.md)页中的最新Adobe Pass身份验证产品公告和停用时间表。

>[!TIP]
> 
> **注释：**
>
> - 从iOS sdk版本3.1开始，实施人员现在可以交替使用WKWebView或UIWebView。 由于UIWebView已被弃用，应用程序应迁移到WKWebView，以避免未来的iOS版本出现问题。
> - 请注意，迁移仅意味着使用WKWebView切换UIWebView类，没有针对Adobe的AccessEnabler的具体工作要做。

</br>

## 更新内部版本设置 {#update}

此版本包含用SWIFT语言编写的功能。 如果您的应用程序完全是Objective-C，则需要将target构建设置中的“始终嵌入Swift标准库”复选框设置为“是”。 设置此选项后，Xcode会扫描应用程序中的捆绑框架，如果其中任何包含Swift代码，则会将相关库复制到应用程序的捆绑包中。 如果不更新生成设置，应用程序可能会崩溃，并出现错误，指出它无法加载AccessEnabler.framework或各种`ibswift*`库。

</br>

## 添加软件声明 {#add}

> 有关如何获取软件声明的信息，请转到此
> 页面：
> [应用程序注册](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-application-registration.md)

一旦您拥有了软件语句，我们建议将它托管在远程服务器上，这样您就可以轻松地撤销或更改它，而不用在App Store中部署应用程序的新版本。 应用程序启动时，请从远程位置获取软件语句，并将其传递到AccessEnabler构造函数：

```swift
    accessEnabler = AccessEnabler("YOUR_SOFTWARE_STATEMENT_HERE");
```

> 此处的API信息：[iOS / tvOS API参考](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)

</br>

## 添加自定义URL方案 {#add-custom}

> 有关如何获取自定义URL方案的信息，请访问此页面： [获取客户URL方案](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-application-registration.md)

获取自定义URL方案后，您需要将其添加到应用程序的info.plist文件中。 自定义方案的格式为： `adbe.u-XFXJeTSDuJiIQs0HVRAg://`。 将冒号和正斜线添加到文件时需要省略。 上述示例将变为`adbe.u-XFXJeTSDuJiIQs0HVRAg`。

```plist
    <key>CFBundleURLTypes</key>
    <array>
        <dict>
            <key>CFBundleURLSchemes</key>
            <array>
                <string>CUSTOM_URL_SCHEME_HERE</string>
            </array>
        </dict>
    </array>
```

</br>

## 截获对自定义URL方案的调用 {#intercept}

这仅适用于应用程序之前通过[setOptions(\[&quot;handleSVC&quot;：true&quot;\])](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)调用启用了手动Safari视图控制器(SVC)处理的情况，以及需要Safari视图控制器(SVC)的特定MVPD，因此需要由SFSafariViewController而不是UIWebView/WKWebView控制器加载身份验证和注销终结点的URL的情况。

在身份验证和注销流期间，应用程序必须监控`SFSafariViewController `控制器的活动，因为它经过多次重定向。 您的应用程序必须检测其加载`application's custom URL scheme`定义的特定自定义URL的时刻（例如`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com)`）。 当控制器加载此特定自定义URL时，应用程序必须关闭`SFSafariViewController`并调用AccessEnabler的`handleExternalURL:url `API方法。

在您的`AppDelegate`中添加以下方法：

```swift
    func application(_ app: UIApplication, open url: URL, options: [UIApplicationOpenURLOptionsKey: Any]) -> Bool {
            if (url.absoluteString.hasPrefix("adbe.")) {
                accessEnabler.handleExternalURL(url.description)
                return true;
            } 
        }
```

> 此处的API信息：[处理外部URL](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)

</br>

## 更新setRequestor方法签名 {#update-setreq}

由于新的SDK使用新的身份验证机制，因此不需要signedRequestId参数或公钥和密钥（对于tvOS）。 `setRequestor`方法已简化，它只需要请求者ID。

### iOS

此代码：

```swift
    accessEnabler.setRequestor(requestorId, setSignedRequestorId: signedRequestorId)
```

变为：

```swift
    accessEnabler.setRequestor(requestorId)
```

</br>

### tvOS

此代码：

```swift
    accessEnabler.setRequestor(requestorId, setSignedRequestorId: signedRequestorId,
                    secret: "secret", publicKey: "public_key")
```

变为：

```swift
    accessEnabler.setRequestor(requestorId)
```

> 此处的API信息：[设置请求者](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)

</br>

## 将getAuthenticationToken方法替换为句柄ExternalURL方法 {#replace}

过去使用`getAuthentication`方法完成身份验证流程。 由于名称有误导性，因此将其重命名为`handleExternalURL`，并将URL作为参数。

更改以下内容的所有实例：

```swift
    accessEnabler.getAuthenticationToken()
```

更改为以下内容：

```swift
    accessEnabler.handleExternalURL(request.url?.description);
```

> 此处的API信息：[处理外部URL](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)
