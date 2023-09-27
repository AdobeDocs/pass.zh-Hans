---
title: 导出共享得分较高的帐户的信息
description: 导出共享得分较高的帐户的信息。
exl-id: df41ddd2-fde3-4861-abd4-6e32f0be9ea5
source-git-commit: d543bbe972944ad83f4cb28c8a17ea6e10f66975
workflow-type: tm+mt
source-wordcount: '697'
ht-degree: 1%

---

# 导出共享得分较高的帐户的信息 {#export-account-info-high-score}

[!UICONTROL Account IQ] 使您可以选择根据前1000个订阅者帐户的帐户共享详细信息 [共享概率](/help/accountiq/product-concepts.md#account-sharing-probability-def). 导出的CSV文件中的数据按订阅者帐户的共享概率递减的顺序排序，这些共享概率是 [区段](/help/accountiq/product-concepts.md#segment-def)，表示 [指定的时间范围](/help/accountiq/product-concepts.md#time-frame-def).

导出帐户共享信息的选项位于 [一般使用情况报表](/help/accountiq/general-usage-reports.md) 和 [共享帐户报表](/help/accountiq/shared-acc-reports.md) 页数。

>[!NOTE]
>
>对于“常规使用”和“共享帐户”报表页面，下载的CSV文件中的数字不同。 这是因为，常规使用情况报表页面具有额外的过滤器，可供程序员为设备数、IP和邮政编码选择阈值。 因此，从常规使用情况报表导出的数据基于应用的附加阈值过滤器。

![导出选项（一般用法）](assets/export.png)

导出订阅者的帐户共享信息：

1. 按照中的步骤定义所需的区段 [如何定义区段并选择时间范围](/help/accountiq/howto-select-segment-timeframe.md) 评估来源 [区段和时间范围](/help/accountiq/segments-timeframe.md) 面板。

1. 选择 **[!UICONTROL Export top 1000 accounts]** 用于导出1000个订阅者的帐户信息且共享概率最高的选项。

使用导出选项时，具有最高共享概率（在定义的时间范围内）的1000个帐户的统计信息将下载到本地计算机的Downloads文件夹中。

>[!NOTE]
>
>可以使用任何读取CSV文件的应用程序(例如Microsoft Excel)打开下载的CSV文件。

![以csv格式导出数据](assets/exported-csv.png)

*图：以CSV格式导出的共享帐户数据*

## 导出的报告中的列 {#columns-in-export}

**周/月**

您在上选择的周或月 **[!UICONTROL Granularity and Time Frame]** 区段选择器中的选项，用于查找共享统计信息。

**MVPD**

如果您是程序员用户，列会显示订阅者帐户属于哪个MVPD。

**订阅者ID**

我们连续讨论的特定帐户。

**设备最小数量**

设备（流内容）的实际数量几乎肯定大于为特定帐户指定的设备最小数量。

>[!NOTE]
>
>设备（流内容）的实际数量肯定大于为特定帐户指定的设备最小数量。

**最小人员数**

使用这些设备激活流媒体内容的绝对最小人数。

>[!NOTE]
>
>实际人数（该流内容）几乎肯定远远大于为特定帐户指定的最低人数。

**[!UICONTROL # IPs]**

从中对内容进行流式处理的IP地址数。

**[!UICONTROL # Locations]**

从中流式传输内容的位置的数量（基于邮政编码）。

**[!UICONTROL # Cities]**

进行流化的城市数量。

**[!UICONTROL # States]**

已进行流处理的州数。

**[!UICONTROL # Clusters]**

非重复的数量 [集群](/help/accountiq/product-concepts.md#cluster-def) 播放流播放的位置。

**[!UICONTROL Geographic span (miles)]**

与帐户关联的流位置之间的最大距离。

**[!UICONTROL # AuthN OK]**

用户在该时段内使用该帐户登录的次数。

**[!UICONTROL # AuthZ OK]**

MVPD授权流或授予该帐户访问（内容）权的次数。

>[!NOTE]
>
>此 **[!UICONTROL # AuthZ OK]** 与 **[!UICONTROL # Play Requests]**；它小于 **[!UICONTROL # Play Requests]** 因为Adobe通常会在24小时内缓存针对MVPD产生的授权。

**[!UICONTROL # Play Requests]**

时段内的实际流数量。

**[!UICONTROL # Channels]**

帐户在时段内已观看的不同渠道的总数。

>[!NOTE]
>
>**[!UICONTROL # Channels]** 包括不一定属于登录程序员的渠道。
>
>该帐户的此号码显示是因为该帐户观看了您的频道，但在该时间段内还访问了其他频道。

**使用模式**

此列中的数字是标识符，这些标识符映射到我们标识所有用户帐户的14种模式之一。

*表：带使用模式的导出CSV映射中的使用模式标识符*

| ID | 1 | 2 | 3 | 4 | 5和8 | 6 | 7 | 9 | 10和11 | 12 | 13 | 14 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 使用模式 | 普通用户 | 旅行者或通勤者 | 大家庭 | 亲朋好友 | 社交组共享 | 一大群朋友 | 并发流 | 社区共享 | 不确定行为 | 小型家庭 | 第二个主页 | 使用异常 |

{style="table-layout:auto"}

**共享概率**

共享概率是特定帐户共享其凭据的概率。

>[!NOTE]
>
> 所有帐户（在所选区段中）的共享概率的平均值用于计算 [共享级别](/help/accountiq/dashboard.md#sharing-level) 的 [聚合共享分数](/help/accountiq/dashboard.md#aggregated-sharing).
