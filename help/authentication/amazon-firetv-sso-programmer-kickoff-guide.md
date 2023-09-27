---
title: Amazon fireTV SSO — 程序员启动指南
description: Amazon fireTV SSO — 程序员启动指南
exl-id: cf9ba614-57ad-46c3-b154-34204b38742d
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '793'
ht-degree: 0%

---

# Amazon fireTV SSO — 程序员启动指南 {#amazon-firetv-sso---programmer-kick-off-guide}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

</br>

## 介绍 {#intro}

本文档介绍了集成新环境所需的信息 **Adobe Pass身份验证的fireTV SDK** 在您的fireTV应用程序中。 此新SDK利用Amazon的fireTV平台上的操作系统级别集成，从而提供 **单点登录** 支持。 为了从单点登录中获益，您需要花费一些精力将应用程序从无客户端API迁移到新的fireTV SDK。 身份验证流程有一些更改，将在下面详细介绍。

## 高级体系结构和操作系统级别的集成 {#high}

为了在Amazon fireTV平台上实现TV Everywhere应用程序之间的单点登录，并提高该平台的整体体验，我们决定在fireTV OS级别集成我们的核心SDK。 程序员需要根据Adobe提供的存根库进行编译。 Amazon的fireTV操作系统中的Adobe库将提供实际功能。

在Amazon提供fireTV模拟器、在操作系统级别将我们的库结合起来之前，只能使用真实的fireTV设备进行开发。

## 优点 {#bene}

* Amazon fireTV平台上的所有Adobe支持TV Everywhere应用程序与所有集成MVPD之间的单点登录。
* 能够从HBA中获益（具有受支持的MVPD）。
* 能够使用最新的fireTV SDK，而无需在每次发布新SDK版本时更新应用程序。
* 所有TVE应用程序都可以通过使用共享系统库受益，因为无需拥有AccessEnabler库的本地副本。 这还可以确保所有应用程序都使用相同的SDK版本。
* 单屏幕身份验证 — 无需注册码和第二屏幕工作流程。

## 从基于无客户端API的应用程序迁移到基于fireTV SDK的应用程序 {#migra1}

要从无客户端API迁移到fireTV SDK，您需要删除与无客户端API相关的代码库，并集成新的fireTV SDK。

与基于无客户端API的应用程序相比，随着新的fireTV SDK将身份验证移动到第一个屏幕，不再需要第二个屏幕身份验证。

这要求程序员将MVPD选取器添加到他们的应用程序中，以便用户可以直接在fireTV设备上选取其电视提供商。 选择MVPD后，将在fireTV设备上向用户显示MVPD登录页面。

描述fireTV上的常规、HBA和SSO情况的用户流的线框位于 [Amazon Fire TV - MVVPD登录用户流程](https://xd.adobe.com/view/9058288e-4b67-43a1-9d5b-5f76ede6c51e/).

## 从基于Android SDK的应用程序迁移到基于fireTV SDK的应用程序 {#migra2}

这个新的fireTV SDK与我们现有的Android SDK以及我们当前拥有的文档非常相似 **集成我们的Android SDK** <!--http://tve.helpdocsonline.com/android-technical-overview-->在我们准备好fireTV SDK文档之前可以使用。 如果您已有使用我们Android SDK的Android应用程序，那么fireTV应用程序中的fireTV SDK集成应该非常简单。

与现有的Android SDK相比，在fireTV SDK上，身份验证过程将更易于开发，因为管理/呈现MVPD登录页面和检索AuthN令牌的任务将由AccessEnabler库在内部执行。

## 常见问题解答 {#faq}

1. 如何 **SSO** 工作？

   * SSO将在由Adobe Pass Authentication提供支持的所有程序员应用程序上运行，这些应用程序在同一台Amazon fireTV设备上使用新的fireTV SDK
   * 在无客户端REST API上实现的程序员应用程序与在fireTV SDK上实现的应用程序之间的SSO **将不受支持**

1. MVPD对fireTV SSO进行了哪些报道？

   * **所有MVPD** 从技术上讲，fireTV SDK支持由Adobe Pass身份验证集成的SSO。

1. 除了使用新SDK之外， **工作流更改** 程序员应该注意吗？

   * 程序员需要为fireTV平台实施MVPD选取器。

1. 身份验证是否会有任何更改 **TTL**？

   * 关于身份验证TTL的行为没有变化。
   * 第一个有效的身份验证令牌将用于执行SSO，在这种情况下，所有其他将通过SSO进行身份验证的应用程序将使用相同的TTL，直到它过期。 因此，当从一个应用程序导航到另一个应用程序时，第二个应用程序将共享验证第一个应用程序的TTL。

1. 如何 **降解API** 工作？

   * 降级API无需进行更改，用户体验将与Android设备上的相同。

1. 如何 **临时传递** 流量是否受到影响？

   * TempPass流是单屏幕的，其行为与任何其他本机设备一样。

1. 其他Adobe功能会像以前一样工作吗？

   * 所有Adobe Pass身份验证功能都将在FireTV上正常运行，就像在Android设备上一样。
