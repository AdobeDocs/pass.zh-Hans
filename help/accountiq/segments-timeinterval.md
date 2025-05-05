---
title: 区段和时间间隔
description: 在Account IQ中定义同类群组或选择订阅者区段，以衡量渠道查看者使用图形工具和报表时的帐户共享可能性和模式。
exl-id: c38cde37-70d9-486d-b8d0-7c1cbd2baf2e
source-git-commit: cfcaa00ab05c99a64bcb0edfe5af60845a6769a9
workflow-type: tm+mt
source-wordcount: '698'
ht-degree: 0%

---


# 区段和时间间隔 {#segment-timeinterval}

登录Account IQ时，位于仪表板上方的区段和时间间隔面板允许您定义订阅者[区段](product-concepts.md#segmet-def)。 此面板有助于筛选结果并显示有关订阅者共享行为和模式的报告。 默认情况下，当前已选定您属性&#x200B;**中名为**&#x200B;所有帐户的区段，您可以在其中查看以下选项：

![](assets/new-segment-selector-collapsed.png){align="left"}

*具有折叠区段摘要的区段和时间间隔面板*

**A.**&#x200B;当前选择的区段名称&#x200B;**B.**&#x200B;打开区段列表&#x200B;**C.**&#x200B;编辑区段&#x200B;**D.**&#x200B;创建新的区段&#x200B;**E.**&#x200B;粒度和时间间隔选择器&#x200B;**F.**&#x200B;图标以展开区段摘要&#x200B;**G.**&#x200B;折叠的区段摘要&#x200B;**H.**&#x200B;所选时间间隔的区段中的帐户数

>[!NOTE]
>
> 折叠的区段摘要显示在Account IQ的TV Everywhere版本中使用的[视频类别](product-concepts.md#video-category-def)。 如果您以D2C服务身份登录，则这些标签会显示您公司的特定视频类别。

在左侧面板的&#x200B;**区段**&#x200B;选项卡中阅读有关[如何创建](work-with-segments.md#create-new-segment)和[管理区段](work-with-segments.md#manage-segment)的更多信息。

## 区段选择 {#segment-selection}

要选择特定区段，请执行以下步骤：

1. 导航到区段和时间间隔面板中的&#x200B;**[!UICONTROL Open segment]**&#x200B;选项。
1. 选择要查看其帐户共享报表的&#x200B;**区段名称**。

   ![](assets/open-segment.png){align="left"}

   *选择区段名称*

   >[!NOTE]
   >
   > 在上一图像中显示的视频类别，如&#x200B;**MVPD**、**程序员**&#x200B;和&#x200B;**频道**，表示在Account IQ的TV Everywhere版本中使用的标签。 如果您以D2C服务身份登录，则这些标签会显示您公司的特定视频类别。

1. 选择&#x200B;**[!UICONTROL Open segment]**。


## 粒度和时间间隔选择 {#granularity-timeinterval}

**粒度和时间间隔**&#x200B;选择器允许您指定每周/每月聚合的日期和持续时间，以观察订阅者共享行为。 默认选择为本周。

![粒度和时间间隔](assets/granularity-timeinterval-weekwise.png){align="left"}

*粒度和时间间隔对话框*

**A.**&#x200B;粒度和时间间隔选择器&#x200B;**B.**&#x200B;转至下个月/周的向右箭头&#x200B;**C.**&#x200B;按周/月选择粒度的选项&#x200B;**D.**&#x200B;当前选择的时间间隔&#x200B;**E.**&#x200B;转至上个月/周

您可以使用以下步骤修改持续时间：

1. 从日期选取器中选择&#x200B;**[!UICONTROL Granularity and Time Interval]**。

1. 从&#x200B;**[!UICONTROL Aggregate By]**&#x200B;选项中选择&#x200B;**[!UICONTROL Week]**&#x200B;或&#x200B;**[!UICONTROL Month]**&#x200B;以设置评估粒度。

1. 选择粒度后，您可以使用向前或向后箭头在时间范围内导航。

1. 选择用于评估的特定时间段。

1. 选择&#x200B;**[!UICONTROL Apply]**&#x200B;以确保您的选择生效。

这允许您将问题语句定义为“在12月选定的一周内观看频道X、Y和Z的MVPD A订阅者”。

## 区段摘要 {#segment-summary}

D2C服务和各处的电视的区段摘要非常相似。 每个Account IQ版本的视频类别将有所不同。

选择 <img alt= "展开区段摘要" src="./assets/expand-segment-summary.svg" width="25">图标以查看详细的区段摘要。 它还提供在所选时段内订户帐户的数量及其回放请求的信息。

+++ D2C服务

![](assets/segment-panel-d2c.png){align="left"}

D2C服务的&#x200B;*区段摘要*

>[!NOTE]
>
>上图中显示的[视频类别](product-concepts.md#video-category-def)，如区段中的&#x200B;**区域**&#x200B;和&#x200B;**内容类型**&#x200B;只是示例。 登录Account IQ时，这些标签会显示您公司的特定视频类别。

**区段摘要**&#x200B;包含以下定义区段的条件：

区段&#x200B;**中的**&#x200B;[&#x200B;区域和内容类型](product-concepts.md#video-category-def)是指与由帐户共享报表中表示的共享帐户观看的视频流关联的元数据标签。

区段&#x200B;**中的**&#x200B;[&#x200B;量度](product-concepts.md#metric)是指订阅者必须满足才能在帐户共享报表中识别的属性或条件。

+++

+++ 到处都是电视

![](assets/segment-panel-programmers-mvpd.png){align="left"}

*程序员/MVPD的区段摘要*

**区段摘要**&#x200B;包含以下定义区段的条件：

区段&#x200B;**中的**&#x200B;[&#x200B;程序员](product-concepts.md#programmer-def)是指其视频流由帐户共享报表中表示的共享帐户观看的内容提供商。

区段&#x200B;**中的**&#x200B;[&#x200B;频道](product-concepts.md#channel-def)是指其视频流由帐户共享报表中表示的共享帐户观看的频道。

区段&#x200B;**中的**&#x200B;[ MVPD](product-concepts.md#mvpd-def)是指与订阅者相关联以便在帐户共享报表中标识的多视频编程分发服务器。

区段&#x200B;**中的**&#x200B;[&#x200B;量度](product-concepts.md#metric)是指订阅者必须满足才能在帐户共享报表中识别的属性或条件。

+++
