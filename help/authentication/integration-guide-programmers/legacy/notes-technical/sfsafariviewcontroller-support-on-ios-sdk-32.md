---
title: iOS SDK 3.2+上的SFSafariViewController支持
description: iOS SDK 3.2+上的SFSafariViewController支持
exl-id: 6691550f-c36f-4fae-aa77-082ca7d8a60a
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '436'
ht-degree: 0%

---

# （旧版）iOS SDK 3.2+上的SFSafariViewController支持 {#sfsafariviewcontroller-support-on-ios-sdk-3.2}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

>[!IMPORTANT]
>
> 确保随时了解汇总在[产品公告](/help/authentication/product-announcements.md)页中的最新Adobe Pass身份验证产品公告和停用时间表。

</br>


**由于安全要求，某些MVPD的登录页必须以SFSafariViewController显示，而非Web视图。**

有些MVPD要求他们的登录页必须在安全的浏览器控件（如SFSafariViewController）中显示。 它们会主动阻止Web视图，因此为了能够使用它们进行身份验证，我们必须使用SVC。

## 兼容性 {#compatiblity}

从iOS SDK版本3.1开始，AccessEnabler SDK会根据服务器配置自动显示SFSafariViewController中特定MVPD的登录页面。

SDK版本3.1自动从应用程序的根视图控制器中显示SFSafariViewController。 虽然这简化了实施者的登录页面管理，但在某些情况下，由于应用程序的特定实施（如模态控制器已经可见），无法从根视图控制器中演示SFSafariViewController。

对于这种情况，3.2版本引入了程序员手动管理SVC的功能。

## 手动SVC管理 {#manual-svc-management}

要手动管理SVC，实施人员必须执行以下步骤：


1. 在AccessEnabler初始化后调用&#x200B;**setOptions([&quot;handleSVC&quot;：true])** （确保在身份验证开始前执行此调用）。 这将启用“手动”SVC管理，SDK不会自动提供SVC，而是在需要时提供     调用&#x200B;**navigate(toUrl：*{url}* useSVC：true)**。

1. 在实施中实施可选回调&#x200B;**`navigateToUrl:useSVC:`**，您必须使用提供的URL使用SFSafariViewController实例创建svc实例，并将其显示在屏幕上：

   ```obj-c
   func navigate(toUrl url: String!, useSVC: Bool) {
       svc =  SFSafariViewController(url: URL(string: url)!)
       svc.delegate = self
       myController.present(svc, animated: true)
       }
   ```

   ***注释：***

   - *您可以根据需要自定义SFSafariViewController。 例如，在iOS 11+上，您可以将“完成”标签更改为“取消”。*
   - *为了能够关闭svc，您需要引用它，请不要在&#x200B;**navigateToUrl：useSVC***的范围内创建它
   - *对“myController”使用您自己的视图控制器*


1. 在应用程序的&#x200B;**应用程序(\_app： UIApplication)的委托实现中，打开URL： URL，选项： \[UIApplicationOpenURLOptionsKey： Any\]) -\> Bool**，添加代码以关闭svc。 您应该已经有一些调用&#x200B;**accessEnabler.handleExternalURL()**&#x200B;的代码。 就在下面，添加：

   ```obj-c
   if(svc != nil) {
       svc.dismiss(animated: true)
   }
   ```

   同样，svc是对您在步骤2中创建的SFSafariViewController的引用。


1. 从&#x200B;**SFSafariViewControllerDelegate**&#x200B;实现&#x200B;**safariViewControllerDidFinish（\_控制器： SFSafariViewController）**，以便捕获用户何时使用“完成”按钮取消了svc。 在此函数中，要通知SDK身份验证已取消，您需要调用：

   ```obj-c
   accessEnabler.setSelectedProvider(nil)
   ```
