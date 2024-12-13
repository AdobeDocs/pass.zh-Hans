---
title: AccessEnabler iOS/tvOS 3.7.0升级路径
description: AccessEnabler iOS/tvOS 3.7.0升级路径
exl-id: f15c7414-ec9b-4e21-b457-1ecf59f47441
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '321'
ht-degree: 0%

---

# （旧版） AccessEnabler iOS/tvOS 3.7.0升级路径 {#accessenabler-iostvos-370-upgrade-path}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

</br>

来自[新AccessEnabler版本3.7.0](/help/authentication/notes-releases/authn-rn-ios-tvos-370.md)的密钥链存储更改与AccessEnabler版本3.7.0以下的密钥链存储实施不兼容。

一个采用新AccessEnabler版本3.7.0的应用程序的升级路径将从密钥链存储的先前版本迁移所有令牌。 因此，在AccessEnabler框架更新过程中，最终用户&#x200B;**不应丢失身份验证/授权会话**。

## 已知限制

实施人员可能会遇到如下所述的一些限制。


1. 常规(Adobe)SSO在使用AccessEnabler 3.7.0版的应用程序和使用3.7.0以下的AccessEnabler版本的应用程序之间不起作用，即使对于同一供应商开发的应用程序也是如此。

   >[!IMPORTANT]
   >
   >* 系统级别(Apple)SSO不会受到影响！
   >
   >* 如果两个应用程序都由同一供应商开发，并且使用的AccessEnabler版本低于3.7.0，则常规(Adobe)SSO将继续工作！
   >
   >* 如果两个应用程序都由同一供应商开发并使用AccessEnabler 3.7.0版，则常规(Adobe)SSO将正常工作！


1. 如果将使用AccessEnabler 3.7.0版的某个应用程序降级到AccessEnabler的较低版本，则不会迁移新生成的令牌。 因此，最终用户可能会遇到身份验证/授权会话丢失，而不需要身份验证/授权会话。

   >[!IMPORTANT]
   >
   >* 通过系统级别(Apple)SSO进行身份验证的最终用户不会受到影响！
   >* 在使用AccessEnabler版本3.7.0更新到新应用程序之前已经过身份验证的最终用户不会受到影响！

1. 在使用AccessEnabler 3.7.0版将一个应用程序降级到AccessEnabler的较低版本时，将不会确认已删除的令牌。 因此，最终用户可能会体验到身份验证/授权会话，而不是期望出现该会话。
