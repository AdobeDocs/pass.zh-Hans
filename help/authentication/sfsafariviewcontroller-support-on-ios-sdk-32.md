---
title: iOS SDK 3.2+上的SFSafariViewController支持
description: iOS SDK 3.2+上的SFSafariViewController支持
exl-id: 6691550f-c36f-4fae-aa77-082ca7d8a60a
source-git-commit: 929d1cc2e0466155b29d1f905f2979c942c9ab8c
workflow-type: tm+mt
source-wordcount: '413'
ht-degree: 0%

---

# iOS SDK 3.2+上的SFSafariViewController支持 {#sfsafariviewcontroller-support-on-ios-sdk-3.2}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

</br>


**由于安全要求，某些MVPD的登录页必须以SFSafariViewController而不是Web视图显示。**

有些MVPD要求它们的登录页必须在安全的浏览器控件（如SFSafariViewController）中显示。 它们会主动阻止Web视图，因此为了能够使用它们进行身份验证，我们必须使用SVC。

## 兼容性 {#compatiblity}

从iOS SDK版本3.1开始，AccessEnabler SDK根据服务器配置自动显示SFSafariViewController中特定MVPD的登录页面。

SDK版本3.1自动从应用程序的根视图控制器中显示SFSafariViewController。 虽然这简化了实施者的登录页面管理，但在某些情况下，由于应用程序的特定实施（如模态控制器已经可见），无法从根视图控制器中演示SFSafariViewController。

对于这种情况，3.2版本引入了程序员手动管理SVC的功能。

## 手动SVC管理 {#manual-svc-management}

要手动管理SVC，实施人员必须执行以下步骤：


1. 调用 **setOptions([&quot;handleSVC&quot;：true])** AccessEnabler初始化之后（确保在身份验证开始之前执行此调用）。 这将启用“手动”SVC管理，SDK不会自动呈现SVC，而是在需要时调用 **navigate(toUrl：*{url}* useSVC：true)**.

1. 实施可选回调 **`navigateToUrl:useSVC:`** 在实施中，您必须使用提供的URL通过SFSafariViewController实例创建svc实例，并将其显示在屏幕上：

   ```obj-c
   func navigate(toUrl url: String!, useSVC: Bool) {
       svc =  SFSafariViewController(url: URL(string: url)!)
       svc.delegate = self
       myController.present(svc, animated: true)
       }
   ```

   ***注释：***

   - *您可以根据需要自定义SFSafariViewController。 例如，在iOS 11+上，您可以将“完成”标签更改为“取消”。*
   - *为了能够关闭svc，您需要引用它，请不要在范围内创建它&#x200B;**navigateToUrl：useSVC***
   - *使用您自己的“myController”视图控制器*


1. 在您的应用程序委托实现中 **application（\_app： UIApplication，打开URL： URL，选项： \[UIApplicationOpenURLOptionsKey： Any\]） -\> Bool**，添加代码以关闭svc。 您应该已经有一些代码可以调用 **accessEnabler.handleExternalURL()**. 就在下面，添加：

   ```obj-c
   if(svc != nil) {
       svc.dismiss(animated: true)
   }
   ```

   同样，svc是对您在步骤2中创建的SFSafariViewController的引用。


1. 实施 **safariViewControllerDidFinish（\_控制器：SFSafariViewController）** 从 **SFSafariViewControllerDelegate** 以便捕获用户何时使用“完成”按钮取消了svc。 在此函数中，要通知SDK身份验证已取消，您需要调用：

   ```obj-c
   accessEnabler.setSelectedProvider(nil)
   ```
