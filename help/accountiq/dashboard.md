---
title: Account IQ仪表板
description: 仪表板通过分析大量订阅者数据来帮助查明密码共享实例。
exl-id: 616da2a5-c9fe-40ea-90cf-f565bc13e764
source-git-commit: ffc7caa9a857a60380d15f69abf346718de6fa4c
workflow-type: tm+mt
source-wordcount: '594'
ht-degree: 0%

---

# 仪表板 {#dashboard}

仪表板汇总和汇总一组图表和报表中的数据，这些图表和报告旨在全面概述帐户共享的范围和影响。 它提供一个页面，其中包含的主要报表和量度，来自 [!UICONTROL Account IQ].


+++程序员 — 仪表板

![仪表板 [!UICONTROL Account IQ] 程序员用户](assets/dashboard-programr.png)


图：程序员用户的仪表板

+++

+++MVPD — 仪表板

MVPD用户的仪表板与程序员用户的仪表板略有不同。

![仪表板 [!UICONTROL Account IQ] 程序员用户](assets/dashboard-mvpd.png)

图：MVPD用户的仪表板

+++

## 平均共享分数 — 针对当前区段汇总 {#aggregated-sharing}

“汇总共享分数”面板提供顶行读数，以总结帐户和流数量方面的共享数量和影响。

这些值可帮助您了解订阅者共享凭据的规模，从而提供对其采取操作的必要性度量。

![](assets/aggregate-sharing-score.png)


*图：平均共享得分面板 — 针对当前区段汇总*

以下三个指标是平均共享分数的组成部分。

### 共享级别 {#sharing-level}

共享级别量规显示所选时间范围内共享的所有订阅者帐户（在定义的区段中）的百分比。

根据所选MVPD集合中每个帐户计算的平均共享概率计算出的值，该组所选MVPD在所选时间帧期间从所选程序员信道之一进行流式处理。

![](assets/sharing-level.png)


*图：共享级别*

趋势指示器显示指标值与上一个时间范围相比的百分比变化。

### 共享帐户的使用情况 {#usage-from-shared-accounts}

此量规指示在定义的段和时间段内，所有订阅者帐户的使用百分比来自共享帐户。 量规以0到100%的比例标记使用范围（从共享帐户）。 这些范围（名为“低”、“中”、“高”和“异常”）基于行业平均值。

您还可以看到趋势指示器，该指示器描述共享帐户的使用量与上一个时间范围相比上升或下降。

![](assets/usage-4mshared-accounts.png)


*图：共享帐户的使用情况*

### 总体共享得分 {#overall-sharing-score}

总体共享得分由共享得分组成，包括“共享级别”和“来自共享帐户的z使用情况”。

它提供的价值旨在反映共享相对于行业的影响。 其目的类似于信用评分，用单个数字汇总情况。 但在本例中，数字越大，潜在危害就越大。

![](assets/overall-sharing-score.png)


*图：总体共享分数*

<!--### MVPDs in segment {#mvpd-in-segment}

It is a table of risk indices and accounts totals for the top MVPDs ranked by overall usage or account sharing.

![](assets/mvpds-in-segment.png)-->

## 全行业的MVPD总体共享分数 {#top-mvpds}

此表提供了有关该区段中MVPD的不同汇总共享分数的比较视图。

>[!NOTE]
>
>下表使用整体行业数据作比较用途，而非该分部之该等MVPD代表之数据。

![](assets/top-mvpds.png)


*图：按总体分数划分的区段中的排名靠前的MVPD*

## 按渠道和MVPD共享得分 {#sharin-score-by-channels-and-mvpds}

此表提供当前区段中MVPD的所选通道共享分数的比较视图。

![](assets/sharing-scores-by-channels-mvpds.png)


*图：按渠道和MVPD共享分数*

## 帐户共享概率 {#accounts-sharing-probability}

此图表将按概率分成5个等级，从非常低(0-20%)到非常高(80=100%)。

>[!NOTE]
>
>条形图使用对数刻度。


![](assets/dashboard-ac-sharing-prob.png)


*图：不同共享概率范围内的订户帐户的数量和百分比*

## 通过共享概率级别进行的帐户数和使用情况 {#number-of-accounts-usage-sharing-probability}

此面板提供表格形式的帐户视图，这些帐户按从极低(0-20%)到极高(80-100%)的概率五分位数分成不同的范围，每个百分位数与共享帐户中的关联使用情况相同。

![](assets/no-acc-usage-prob-level.png)


*图：各种概率范围内的帐户、趋势和使用情况的数量*

<!--
+++Dashboard for programmers

![dashboard of account IQ](assets/dashboard-capture.png)


*Figure: The dashboard*

>>>>>>> 7ab48cf61552febab21a5d5c05586e0aefe8ce17
## Average sharing score - aggregated for the current segment {#aggregated-sharing}

The Aggregated Sharing Score panel provides a top line readout summarizing the quantity and impact of sharing in terms of accounts and streaming volume.

The values help you understand the magnitude of credential sharing by your subscribers, hence providing a measure of the need to act upon it.

![](assets/aggregate-sharing-score.png)


*Figure: Average sharing score panel - aggregated for the current segment*

The following three metrics are components of the Average Sharing Score.

### Sharing level {#sharing-level}

The sharing level gauge shows the percentage of all your subscriber accounts (in the defined segment) that are shared, during the selected time frame.  

A value calculated based on an average of the sharing probability computed for every account for the selected MVPD(s) that has streamed from a one of the selected programmer channels during the selected time frame.

![](assets/sharing-level.png)


*Figure: Sharing level*

The Trend indicator shows the percentage change in the value of the metric in from the previous time frame.

### Usage from shared accounts {#usage-from-shared-accounts}

This gauge indicates what percent of the usage of all the subscriber accounts is from the shared accounts for the defined segment and time period. The gauge marks the ranges of usage (from shared accounts) on the scale of 0 to 100%. These ranges (named Low, Medium, High, and Abnormal) are based on the industry average.

You can also see the Trend indicator, which depicts a rise or fall in the usage from shared accounts as compared to the previous time frame.

![](assets/usage-4mshared-accounts.png)


*Figure: Usage from shared accounts*

### Overall sharing score {#overall-sharing-score}

Overall sharing score is composite of sharing scores including "Sharing level" and "Usage from shared accounts".

It provides a value meant to reflect the relative impact of sharing when compared to the industry. Its purpose is similar to that of a credit score, summarizing the situation with a single number. But in this case, the higher the number the greater the potential harm.

![](assets/overall-sharing-score.png)


*Figure: Overall sharing score*

## Industrywide overall sharing scores {#mvpd-in-segment}

+++Programmer- MVPDs in segment

This table provides a comparative view of the different Aggregated Sharing Scores for the MVPDs in the segment.

![](assets/mvpds-in-segment.png)


*Figure: Panel showing top MVPDs in a segment*


>[!NOTE]
>
>This table uses overall industry data for comparative purposes, not the data represented by those MVPDs in the segment.

+++

+++MVPD- Programmers in segment

This table provides a comparative view of the different Aggregated Sharing Scores for the programmers in the segment.

![](assets/programmers-in-segment.png)


*Figure: Panel showing top programmers in a segment*

+++


## Sharing score by channels and MVPDs {#sharin-score-by-channels-and-mvpds}

+++Programmer- MVPDs in segment

This table provides a comparative view of sharing scores of the selected channels for the MVPDs in the current segment.

![](assets/sharing-scores-by-channels-mvpds.png)


*Figure: Sharing scores by channels and MVPDs*

>[!NOTE]
>
>**Sharing score by channels and MVPDs** panel is available only for programmer login.

+++

## Accounts sharing probability distribution{#accounts-sharing-probab-dist}

This panel partitions accounts into ranges of sharing probability quintiles from very low (0-20%) to very high (80-100%).

Pie chart shows the proportions (in term of percentages) of user accounts in various sharing probability ranges. Whereas, column chart shows the absolute numbers of accounts in different probability ranges.

>[!NOTE]
>
>The column chart uses a logarithmic scale.


![](assets/dashboard-ac-sharing-prob.png)


*Figure: Percentages and number of subscriber accounts in different sharing probability ranges*

### Accounts over threshold in current segment {#acc-over-threshold-in-segment}

You can select a level of sharing probability, out of the following to view number and percentage of accounts above it:

* Over very low (0%-20%) probability

* Over low (20%-40%) probability

* Over moderate (40%-60%) probability

* Over high (60%-80%) probability

## Number of accounts and usage by sharing probability level {#number-of-accounts-usage-sharing-probability}

This panel provides tabular view of  accounts partitioned into ranges of sharing probability quintiles from very low (0-20%) to very high (80-100%) with each quintile's associated usage from shared accounts.

![](assets/no-acc-usage-prob-level.png)

*Figure: Number of accounts, trends, and usages falling in various probability ranges*

-->