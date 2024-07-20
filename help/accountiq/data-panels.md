---
title: 仪表板上的数据面板
description: 仪表板通过分析大量订阅者数据，帮助查明密码共享实例。
exl-id: 12abba05-7422-4bcc-8b11-76aca4911c0b
source-git-commit: 2bb570ab14a3295d46ee6dc0d38485697d63809c
workflow-type: tm+mt
source-wordcount: '889'
ht-degree: 0%

---

# 仪表板上的数据面板 {#data-panels}

选择区段和时间间隔后，仪表板将显示各种数据面板、表格和图形，以反映所选区段内共享活动的高级视图。

下表概述了不同[版本](/help/accountiq/versions-aiq.md)的Account IQ中数据面板之间的可用性和差异：

| 数据面板 | D2C服务 | TVE程序员 | TVE MVPDs |
|---|---|---|---|
| [当前区段汇总的平均共享分数](#aggregated-sharing) | 可用且一致 | 可用且一致 | 可用且一致 |
| 区段中的[视频类别](#video-categories-segment) | 提供时略有变化 | 提供时略有变化 | 提供时略有变化 |
| [按渠道和MVPD共享得分](#sharin-score-by-channels-and-mvpds) | 不可用 | 可用 | 不可用 |
| [帐户共享概率](#accounts-sharing-probability) | 可用且一致 | 可用且一致 | 可用且一致 |
| [通过共享概率级别使用的帐户数和使用量](#number-of-accounts-usage-sharing-probability) | 可用且一致 | 可用且一致 | 可用且一致 |


## 为当前区段汇总的平均共享分数 {#aggregated-sharing}

“平均共享分数”面板提供顶级读数，以总结帐户和流数量方面的共享数量和影响。

这些量度可帮助您了解订阅者共享凭据的大小（从低、中、高到异常），以帐户和使用情况来衡量。

![](assets/aggregate-sharing-score.png)


*当前区段的平均共享得分面板汇总*

>[!NOTE]
>
> 为当前区段&#x200B;**汇总的**&#x200B;平均共享分数中的蓝色指示器对D2C服务的用途与所有地方的电视不同。 对于D2C服务，它表示&#x200B;**服务平均索引**，如上图所示。 如果您以程序员或MVPD身份登录，则此标签将更改为&#x200B;**行业平均索引**。

以下量度是“平均共享分数”面板的组件。

### 共享级别 {#sharing-level}

共享级别量规显示所选时间间隔内定义段内所有共享订户帐户的百分比。

百分比是根据为区段中的每个帐户计算的平均共享概率计算的。 此计算包括在所选时间间隔内至少流式处理过一次的帐户。

趋势指示器显示指标值相对于上一个时间间隔的百分比变化。

![](assets/sharing-level.png){width="350" align="left"}


*共享级别*

### 共享帐户的使用情况 {#usage-from-shared-accounts}

该量规指示共享帐户在定义的段和时间段内所有订阅者帐户中的使用百分比。 这些范围分别名为“低”、“Medium”、“高”和“异常”，它们基于行业平均值。

趋势指示器，用于描述与上一个时间间隔相比，共享帐户的使用率上升或下降。

![](assets/usage-4mshared-accounts.png){width="350" align="left"}


*来自共享帐户的使用情况*

### 总体共享得分 {#overall-sharing-score}

总体共享分数是共享分数的组合，包括“共享级别”和“来自共享帐户的使用情况”。

它提供的分数反映了共享的总体影响。 其目的类似于信用评分，用单个数字汇总共享级别。 但在本例中，分数越高，共享级别越高。

![](assets/overall-sharing-score.png){width="350" align="left"}


*总体共享分数*

## 区段中的视频类别 {#video-categories-segment}

您可以选择列标题来对Account IQ所有版本中的数据排序。

+++D2C服务：区段中的区域

当您以D2C服务身份登录时，区段&#x200B;**表中的**&#x200B;区域提供了当前区段中[视频类别](/help/accountiq/product-concepts.md#video-category-def)的不同聚合共享分数的比较视图。

![](assets/sharing-scores-by-regions-in-segment.png)

*按区段中的区域共享得分*

>[!NOTE]
>
> 上图中显示的[视频类别](product-concepts.md#video-category-def)，如区段中的&#x200B;**区域**&#x200B;只是一个示例。 登录Account IQ时，此面板会显示您公司的特定视频类别。

选择&#x200B;**导出**&#x200B;以.csv文件格式下载数据。 了解[如何导出数据面板报告](/help/accountiq/export-reports.md)。

+++

+++程序员：区段中的MVPD

当您以程序员身份登录时，区段&#x200B;**表中的** MVPD提供了当前区段中MVPD的不同聚合共享分数的比较视图。

![](assets/sharing-scores-by-mvpds-in-segment.png)

选择&#x200B;**导出**&#x200B;以.csv文件格式下载数据。 了解[如何导出数据面板报告](/help/accountiq/export-reports.md)。

+++

+++MVPDs：区段中的程序员

当您以MVPD身份登录时，区段&#x200B;**表中的**&#x200B;程序员提供了当前区段中程序员的不同聚合共享分数的比较视图。

选择列标题以对数据进行排序。

![](assets/sharing-scores-by-programmers-in-segment.png)

*由程序员在区段中共享得分*

选择&#x200B;**导出**&#x200B;以.csv文件格式下载数据。 了解[如何导出数据面板报告](/help/accountiq/export-reports.md)。

+++

## 按渠道和MVPD共享得分  {#sharin-score-by-channels-and-mvpds}

当您以程序员身份登录时，此表提供当前区段中MVPD所选渠道的共享分数的比较视图。

选择列标题以对数据进行排序。

![](assets/sharing-scores-by-channels-mvpds.png)


*按渠道和MVPD共享得分*

## 帐户共享概率 {#accounts-sharing-probability}

此图表将按概率分成5个等级，从极低(0-20%)到极高(80-100%)不等。 详细了解[帐户共享概率](#accounts-sharing-probability)的范围。

>[!NOTE]
>
>条形图使用对数刻度。


![](assets/dashboard-ac-sharing-prob.png)


*不同共享概率范围内的订阅者帐户的数量和百分比*


## 通过共享概率级别进行的帐户数和使用情况 {#number-of-accounts-usage-sharing-probability}

此面板提供按共享概率五分位数的范围划分的帐户的表格视图，范围从非常低(0-20%)到非常高(80-100%)，每个五分位数的关联使用情况来自共享帐户。 详细了解[帐户共享概率](#accounts-sharing-probability)的范围。

![](assets/no-acc-usage-prob-level.png)

*帐户数量、趋势和使用情况在不同的概率范围内*

选择&#x200B;**导出**&#x200B;以.csv文件格式下载数据。 了解[如何导出数据面板报告](/help/accountiq/export-reports.md)。
