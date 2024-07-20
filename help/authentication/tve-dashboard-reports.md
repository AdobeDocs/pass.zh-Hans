---
title: 报告
description: 了解如何在TVE功能板报表中聚合数据。
exl-id: d8ba48de-d743-4dc2-866c-7d6e3ff94773
source-git-commit: c2dcea9e4170a3e10654bcd3f8d2f5cdb82c9603
workflow-type: tm+mt
source-wordcount: '976'
ht-degree: 0%

---

# 报告 {#Reports}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

TVE仪表板的&#x200B;**报表**&#x200B;部分提供对AuthN TTL、AuthZ TTL和SSO报表的聚合数据的访问权限。 这些报告包含所有[平台](#platforms)上与不同MVPD的渠道集成。

通过报表，可跨[个特定的渠道或MVPD](#selecting-specific-channels-mvpds)过滤数据和收集见解。 您还可以将报表导出为CSV文件以供进一步分析。

## 查看报表 {#view-reports}

按照以下步骤查看特定报告。

1. 在左侧面板中选择&#x200B;**报表**&#x200B;选项卡。
1. 选择以下选项卡之一以查看和导出包含的通道和MVPD的聚合数据：
   * [AuthN TTL报表](#authn-ttl-reports)
   * [AuthZ TTL报表](#authz-ttl-reports)
   * [SSO报告](#sso-reports)

   ![报告类型](assets/type-of-reports.png)

   *报告类型*

### AuthN TTL报表 {#authn-ttl-reports}

AuthN TTL报告(也称为身份验证生存时间(TTL))显示了在所有[平台](#platforms)上为您的渠道与各种MVPD的集成配置身份验证令牌的持续时间。 这些报告允许您检查用户在特定MVPD和平台中保持身份验证的时间。 持续时间值以用户友好的格式显示，如&#x200B;**天**、**小时**、**分钟**&#x200B;和&#x200B;**秒**。 AuthN TTL报告表具有水平滚动和垂直滚动功能，以适应不同的屏幕大小。

您还可以查看和下载[特定渠道或MVPD](#selecting-specific-channels-mvpds)的数据。

![导出AuthN TTL报告](assets/authn-ttl-reports.png)

*导出AuthN TTL报告*

>[!IMPORTANT]
>
> 当MVPD强制使用AuthN TTL值而非Adobe Pass身份验证配置时，将使用MVPD设置的&#x200B;**占位符**。

选择&#x200B;**导出报表**&#x200B;以将数据另存为CSV文件保存到本地计算机上。

### AuthZ TTL报表 {#authz-ttl-reports}

AuthZ TTL报告(也称为授权生存时间(TTL))显示为渠道与所有[平台](#platforms)上的各种MVPD集成配置的授权令牌的持续时间。 利用这些报告，可检查用户保持有权观看特定MVPD和平台内容的时间。 持续时间值以用户友好的格式显示，如&#x200B;**天**、**小时**、**分钟**&#x200B;和&#x200B;**秒**。 AuthZ TTL报告表具有水平滚动和垂直滚动功能，以适应不同的屏幕大小。

您还可以查看和下载[特定渠道或MVPD](#selecting-specific-channels-mvpds)的数据。

![导出AuthZ TTL报告](assets/authz-ttl-reports.png)

*导出AuthZ TTL报告*

>[!IMPORTANT]
>
> 当MVPD强制使用AuthZ TTL值而非Adobe Pass身份验证配置时，将使用MVPD设置的&#x200B;**占位符**。

选择&#x200B;**导出报表**&#x200B;以将数据另存为CSV文件保存到本地计算机上。

### SSO报告 {#sso-reports}

SSO报告（也称为单点登录）显示为您的渠道与所有[平台](#platforms)上的各种MVPD的集成配置的单点登录状态。 这些报告允许您检查特定MVPD和平台的预期用户身份验证SSO体验。 这些值以用户友好的格式显示，如&#x200B;**SSO Disabled**、**SSO Enabled**&#x200B;和&#x200B;**SSO Unquantical**。 SSO报告表具有水平滚动和垂直滚动功能，以适应不同的屏幕大小。

您还可以查看和下载[特定渠道或MVPD](#selecting-specific-channels-mvpds)的数据。

![导出SSO报告](assets/sso-reports.png)

*导出SSO报告*

>[!IMPORTANT]
>
> **SSO不确定**&#x200B;占位符指示单点登录(SSO)已启用，并且可能正在运行。 但是，下面列出的设置可能会禁止SSO身份验证，如以下示例中所述：
>
> * 用户平台设置：阻止第三方Cookie的选项。
> * 用户决策：用户拒绝平台访问其电视提供商订阅。
> * MVPD设置： MVPD请求每个通道的身份验证。

选择&#x200B;**导出报表**&#x200B;以将数据另存为CSV文件保存到本地计算机上。

## 平台 {#platforms}

[AuthN TTL报告](#authn-ttl-reports)、[AuthZ TTL报告](#authz-ttl-reports)和[SSO报告](#sso-reports)显示跨各种平台的数据，例如：

* **桌面**：显示通过Adobe Pass Authentication JavaScript SDK应用于程序员实施的值。

* **移动设备**

  **iOS**：显示使用Adobe Pass身份验证iOS SDK应用的值。

  **Android**：显示通过Adobe Pass身份验证Android SDK应用的值。

  **其他**：显示使用为移动设备开发的Adobe Pass身份验证REST API应用的值。

* **TVCD**

  **Roku**：显示通过Adobe Pass身份验证REST API应用的值，将Roku标识为设备类型。

  **FireTV**：显示通过Adobe Pass身份验证FireTV SDK应用的值。

  **AppleTV**：显示通过Adobe Pass身份验证tvOS SDK应用的值。

  **其他**：显示使用Adobe Pass身份验证REST API为电视连接设备应用的值。

* **平台未识别**：当Adobe Pass身份验证服务检测到未知设备类型时，显示应用于程序员实施的值。

要了解有关如何与Adobe Pass身份验证REST API或SDK共享所需设备类型（如&#x200B;**Roku**）的更多信息，请查看[传递客户端信息](/help/authentication/passing-client-information-device-connection-and-application.md)的机制。

>[!IMPORTANT]
>
> 聚合的数据基于每个Adobe Pass身份验证环境的特定配置。 在不同的TVE功能板环境之间切换时，需要报表中的数据变化。 请参阅[Adobe Pass身份验证环境](/help/authentication/tve-dashboard-environments.md)以了解更多信息。

## 选择特定渠道和MVPD {#selecting-specific-channels-mvpds}

默认情况下，[AuthN TTL报告](#authn-ttl-reports)、[AuthZ TTL报告](#authz-ttl-reports)和[SSO报告](#sso-reports)显示&#x200B;**所有渠道**&#x200B;与&#x200B;**所有MVPD**&#x200B;集成的数据。

>[!NOTE]
>
> 如果取消选择相应下拉菜单中的&#x200B;**所有渠道**&#x200B;或&#x200B;**所有MVPD**，则会显示一条消息，以便选择是否查看有意义的报告。

要生成特定渠道的报告，请执行以下操作：

1. 选择位于所选报告顶部的&#x200B;**包含的渠道**&#x200B;下拉菜单。

   ![包含的渠道下拉菜单](assets/include-channels.png)

   *包含的渠道下拉菜单*

1. 取消选择&#x200B;**所有渠道**。
1. 从要为其生成数据的&#x200B;**包含的渠道**&#x200B;下拉菜单中选择所需的渠道。

>[!NOTE]
>
> 若要在&#x200B;**包含的MVPD**&#x200B;下拉菜单中拥有可用选项，您必须在&#x200B;**包含的渠道**&#x200B;下拉菜单中选择至少一个渠道。

要为特定MVPD生成报告，请执行以下操作：

1. 选择位于所选报告顶部的&#x200B;**包含的MVPD**&#x200B;下拉菜单。

   ![包含的MVPD下拉菜单](assets/include-mvpds.png)

   *包含的MVPD下拉菜单*

1. 取消选择&#x200B;**所有MVPD**。
1. 从&#x200B;**包含的MVPD的**&#x200B;下拉菜单中选择要为其生成数据的所需MVPD。
