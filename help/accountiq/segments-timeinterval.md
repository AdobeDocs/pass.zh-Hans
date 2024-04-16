---
title: 区段和时间间隔
description: 定义同类群组或选择订阅者区段，以衡量渠道查看者在帐户IQ中使用图形工具和报表时帐户共享的可能性和模式。
exl-id: c38cde37-70d9-486d-b8d0-7c1cbd2baf2e
source-git-commit: cfcaa00ab05c99a64bcb0edfe5af60845a6769a9
workflow-type: tm+mt
source-wordcount: '698'
ht-degree: 0%

---


# 区段和时间间隔 {#segment-timeinterval}

登录Account IQ时，位于仪表板上方的区段和时间间隔面板允许您定义订阅者 [区段](product-concepts.md#segmet-def). 此面板有助于筛选结果并显示有关订阅者共享行为和模式的报告。 名为的区段 **资产中的所有帐户** 当前为选中状态，您可以在其中查看以下选项：

![](assets/new-segment-selector-collapsed.png){align="left"}

*带有折叠区段摘要的区段和时间间隔面板*

**答：** 当前选定的区段名称 **B.** 打开区段列表 **C.** 编辑区段 **D.** 创建新区段 **E.** 粒度和时间间隔选择器 **F.** 用于展开区段摘要的图标 **G.** 折叠的区段摘要 **H.** 所选间隔内段中的帐户数

>[!NOTE]
>
> 折叠的区段摘要显示 [视频类别](product-concepts.md#video-category-def) 在电视上随处使用的Account IQ版本。 如果您以D2C服务身份登录，则这些标签会显示您公司的特定视频类别。

详细了解 [如何创建](work-with-segments.md#create-new-segment) 和 [管理区段](work-with-segments.md#manage-segment) 从 **区段** 选项卡。

## 区段选择 {#segment-selection}

要选择特定区段，请执行以下步骤：

1. 导航至 **[!UICONTROL Open segment]** 区段和时间间隔面板中的选项。
1. 选择 **区段名称** ，以便查看其帐户共享报告。

   ![](assets/open-segment.png){align="left"}

   *选择区段名称*

   >[!NOTE]
   >
   > 上一个图像中所显示的视频类别，如 **MVPDs**， **程序员**、和 **渠道** 表示在TV Everywhere版本的Account IQ中使用的标签。 如果您以D2C服务身份登录，则这些标签会显示您公司的特定视频类别。

1. 选择 **[!UICONTROL Open segment]**.


## 粒度和时间间隔选择 {#granularity-timeinterval}

此 **粒度和时间间隔** 通过选择器，可指定每周/每月聚合的日期和持续时间，用于观察订阅者共享行为。 默认选择为本周。

![粒度和时间间隔](assets/granularity-timeinterval-weekwise.png){align="left"}

*“粒度和时间间隔”对话框*

**答：** 粒度和时间间隔选择器 **B.** 右箭头指向下个月/周 **C.** 用于按周/月选择粒度的选项 **D.** 当前选定的时间间隔 **E.** 左箭头转到上个月/周

您可以使用以下步骤修改持续时间：

1. 选择 **[!UICONTROL Granularity and Time Interval]** 从日期选取器。

1. 选择 **[!UICONTROL Week]** 或 **[!UICONTROL Month]** 从 **[!UICONTROL Aggregate By]** 用于设置评估的粒度的选项。

1. 选择粒度后，您可以使用向前或向后箭头在时间范围内导航。

1. 选择用于评估的特定时间段。

1. 选择 **[!UICONTROL Apply]** 以确保您的选择生效。

这允许您将问题语句定义为“在12月选定的一周内观看频道X、Y和Z的MVPD A订阅者”。

## 区段摘要 {#segment-summary}

D2C服务和各处的电视的区段摘要非常相似。 帐户IQ的每个版本都将使用不同的视频类别。

选择 <img alt= "展开区段摘要" src="./assets/expand-segment-summary.svg" width="25"> 图标以查看详细的区段摘要。 它还提供在所选时段内订户帐户的数量及其回放请求的信息。

+++ D2C服务

![](assets/segment-panel-d2c.png){align="left"}

*D2C服务的区段摘要*

>[!NOTE]
>
>此 [视频类别](product-concepts.md#video-category-def) 如上图所示，例如 **区域** 和 **内容类型** 在区段中，只是示例。 当您登录到Account IQ时，这些标签会显示您公司的特定视频类别。

此 **区段摘要** 包括以下定义区段的条件：

**[地区和内容类型](product-concepts.md#video-category-def) 在区段中** 请参阅与由帐户共享报表中表示的共享帐户观看的视频流关联的元数据标签。

**[量度](product-concepts.md#metric) 在区段中** 请参阅订阅者必须满足的属性或条件才能在帐户共享报表中识别。

+++

+++ 到处都是电视

![](assets/segment-panel-programmers-mvpd.png){align="left"}

*程序员/MVPD的区段摘要*

此 **区段摘要** 包括以下定义区段的条件：

**[程序员](product-concepts.md#programmer-def) 在区段中**  请参阅内容提供商，其视频流已由帐户共享报表中表示的共享帐户观看。

**[渠道](product-concepts.md#channel-def) 在区段中** 指其视频流由帐户共享报表中表示的共享帐户观看的频道。

**[MVPDs](product-concepts.md#mvpd-def) 在区段中** 指与订户相关联以便在账户共享报告中被标识的多视频节目分发商。

**[量度](product-concepts.md#metric) 在区段中** 请参阅订阅者必须满足的属性或条件才能在帐户共享报表中识别。

+++
