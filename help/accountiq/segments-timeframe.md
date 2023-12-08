---
title: 订阅者区段和时间间隔
description: 定义同类群组或选择订阅者区段，以衡量渠道查看者在帐户IQ中使用图形工具和报表时帐户共享的可能性和模式。
exl-id: c38cde37-70d9-486d-b8d0-7c1cbd2baf2e
source-git-commit: 6b790728f3d6a8eed5dfc0f8b3d0dad283af6418
workflow-type: tm+mt
source-wordcount: '346'
ht-degree: 0%

---


# 订阅者区段和时间间隔 {#cohorts-segments}


登录Account IQ时，顶部的区段启动器面板允许您指定订阅者 [区段](/help/accountiq/product-concepts.md#segment-segmet-def). 在查看关于订阅者共享行为和模式的报表时，这有助于筛选结果。 已选择名为“All accounts in your properties（资产中的所有帐户）”的默认区段，并且您会在区段启动器中看到以下选项：

![](assets/new-segment-selector-collapsed.png){width="800" align="left"}

*图：包含折叠区段摘要的区段启动器*

**A** 当前选定的区段名称<br/>
**B** 时间间隔和粒度选择器<br/>
**C** 区段摘要已折叠<br/>
**D** 用于展开区段摘要的选项<br/>
**E** 区段数据（根据一段时间内区段中的订阅者账户数计算）<br/>
**F** 打开区段列表选项<br/>
**G** “编辑区段”选项<br/>
**H** “创建新区段”选项<br/>

## 区段选择 {#segment-selection}

对于程序员或MVPD用户，导航到 **打开区段** 选项。 从列表中选择一个区段，然后选择 **打开区段** 查看帐户共享报表。

使用 **眼睛** 图标以查看详细的段摘要，其中显示有关订户帐户数的信息，以及订户在所选时间间隔内发出的回放请求。

+++程序员/MVPD的区段选择面板

![](assets/segment-panel-programmers-mvpds.png) {width="800" align="left"}

*图：程序员/MVPD的区段面板*

+++

区段摘要用于定义以下参数：

**[!UICONTROL Programmers in segment]**

**[!UICONTROL Channels in segment]**

**[!UICONTROL MVPD in segment]**

**[!UICONTROL Metrics in segment]**

<!-- The definitions of these parameters will be defined in the glossary article-->

## [!UICONTROL Granularity and time interval] {#granularity-timeinterval}

此 **[!UICONTROL Granularity and time interval]** 通过选择器，可指定每周/每月聚合的日期和持续时间，用于观察订阅者帐户共享行为。 时间间隔的默认选择为当前周，但您可以使用图像中所示的选项修改持续时间。

![[!UICONTROL Granularity and timeinterval]](assets/granularity-timeinterval-weekwise.png){width="350" align="left"}

*图：粒度和时间间隔对话框*

**A** 从日期选取器选取日期<br/>
**B** 选择向左箭头以向后移动<br/>
**C** 选择向右箭头以向前移动<br/>
**D** 按周/月选择粒度<br/>
**E** 选定的时间间隔<br/>

应用这些控件，您可以将问题语句定义为“10月份观看频道X、Y和Z的MVPD A订阅者”。

