---
title: Amazon fireTV SSO — 程序员启动指南
description: Amazon fireTV SSO — 程序员启动指南
exl-id: cf9ba614-57ad-46c3-b154-34204b38742d
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '805'
ht-degree: 0%

---

# （旧版）Amazon fireTV SSO — 程序员启动指南 {#amazon-firetv-sso---programmer-kick-off-guide}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

>[!IMPORTANT]
>
> 确保随时了解汇总在[产品公告](/help/authentication/product-announcements.md)页中的最新Adobe Pass身份验证产品公告和停用时间表。

</br>

## 简介 {#intro}

本文档描述了在fireTV应用程序中集成新&#x200B;**Adobe Pass身份验证的fireTV SDK**&#x200B;所需的信息。 此新SDK利用Amazon的fireTV平台上的操作系统级集成，从而提供&#x200B;**单点登录**&#x200B;支持。 为了从单点登录中获益，您需要花费一些精力将应用程序从无客户端API迁移到新的fireTV SDK。 身份验证流程有一些更改，将在下面详细介绍。

## 高级体系结构和操作系统级别的集成 {#high}

为了在Amazon fireTV平台上实现TV Everywhere应用程序之间的单点登录，并提高该平台的整体体验，我们决定在fireTV OS级别集成我们的核心SDK。 程序员需要根据Adobe提供的存根库进行编译。 Adobe的库将在Amazon的fireTV OS中提供实际功能。

在Amazon提供fireTV模拟器、在操作系统级别将我们的库结合起来之前，只能使用真实的fireTV设备进行开发。

## 优点 {#bene}

* 在Amazon fireTV平台上由所有Adobe支持的TV Everywhere应用程序与所有集成的MVPD之间进行单点登录。
* 能够从HBA中获益（具有受支持的MVPD）。
* 能够使用最新的fireTV SDK，而无需在每次发布新SDK版本时更新您的应用程序。
* 所有TVE应用程序都可以通过使用共享系统库受益，因为无需拥有AccessEnabler库的本地副本。 这还可以确保所有应用程序都使用相同的SDK版本。
* 单屏幕身份验证 — 无需注册码和第二屏幕工作流程。

## 从基于无客户端API的应用程序迁移到基于FireTV SDK的应用程序 {#migra1}

要从无客户端API迁移到fireTV SDK，您需要删除与无客户端API相关的代码库，并集成新的fireTV SDK。

与基于无客户端API的应用程序相比，随着新的fireTV SDK将身份验证移动到第一个屏幕，不再需要第二个屏幕身份验证。

这要求程序员将MVPD选取器添加到他们的应用程序中，以便用户可以直接在fireTV设备上选取其电视提供商。 选择MVPD后，将会在fireTV设备上向用户显示MVPD登录页面。

可在[Amazon Fire TV - MVVPD登录用户流](https://xd.adobe.com/view/9058288e-4b67-43a1-9d5b-5f76ede6c51e/)中找到描述fireTV上的常规、HBA和SSO方案的用户流的线框。

## 从基于Android SDK的应用程序迁移到基于fireTV SDK的应用程序 {#migra2}

这个新的fireTV SDK与我们现有的Android SDK非常相似，在我们准备好fireTV SDK文档之前，可以使用我们当前用于&#x200B;**集成Android SDK** <!--http://tve.helpdocsonline.com/android-technical-overview-->的文档。 如果您已有使用我们Android SDK的Android应用程序，那么fireTV应用程序中的fireTV SDK集成应该非常简单。

与现有的Android SDK相比，在fireTV SDK上，身份验证过程将更易于开发，因为管理/展示MVPD登录页面和检索AuthN令牌的任务将由AccessEnabler库在内部执行。

## 常见问题解答 {#faq}

1. **SSO**&#x200B;如何工作？

   * SSO将在由Adobe Pass Authentication提供支持的所有程序员应用程序上运行，这些应用程序在同一台Amazon fireTV设备上使用新的fireTV SDK
   * 不支持在无客户端REST API上实现的程序员应用程序与在fireTV SDK **上实现的应用程序之间的SSO**

1. MVPD对fireTV SSO进行了哪些报道？

   * 从Adobe Pass身份验证集成的&#x200B;**所有MVPD**&#x200B;在fireTV SDK上将SSO技术上受支持。

1. 除了使用新的SDK之外，程序员还应了解其他&#x200B;**工作流发生了哪些变化**？

   * 程序员需要为fireTV平台实施MVPD选取器。

1. 身份验证&#x200B;**TTL**&#x200B;是否有任何更改？

   * 关于身份验证TTL的行为没有变化。
   * 第一个有效的身份验证令牌将用于执行SSO，在这种情况下，所有其他将通过SSO进行身份验证的应用程序将使用相同的TTL，直到它过期。 因此，当从一个应用程序导航到另一个应用程序时，第二个应用程序将共享验证第一个应用程序的TTL。

1. **降级API**&#x200B;如何工作？

   * 降级API不需要进行任何更改，用户体验将与在Android设备上相同。

1. **TempPass**&#x200B;流会受到什么影响？

   * TempPass流是单屏幕的，其行为与任何其他本机设备一样。

1. 其他Adobe功能会像以前一样工作吗？

   * 所有Adobe Pass身份验证功能都将在FireTV上正常运行，就像在Android设备上一样。
