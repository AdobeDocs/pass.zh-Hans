---
title: 共享帐户报表
description: 共享帐户报表
exl-id: 16c5ded1-2a95-4373-8b90-b445131f333a
source-git-commit: 85316a40ba5f6564c84a5aecf689c077e936a91a
workflow-type: tm+mt
source-wordcount: '492'
ht-degree: 0%

---

# 共享帐户报表 {#shared-accounts-reports}

共享帐户报表提供另一组图表和图表，它们反映当前区段的共享行为和消耗量。 例如，当前区段的&#x200B;**[!UICONTROL Over Moderate Probability]**&#x200B;和&#x200B;**[!UICONTROL Over Low Probability]**。

## 帐户共享概率 {#accounts-sharing-probability}

此圆环图和条形图显示属于共享概率特定范围的订阅者帐户的百分比（和绝对数）。 这些范围定义为：

* 非常高(80%-100%)
* 高(60%-80%)
* 中等(40%-60%)
* 低(20%-40%)
* 非常低(0%-20%)

红线标记当前区段](#threshold-selector)面板中[帐户超过阈值中选定的阈值范围，浅红色区域包含超过该阈值的所有帐户的总数。

![](assets/accounts-sharing-probability-pie.png)

条形图在y轴上绘制每个范围中属于各个范围的帐户数量（在x轴上绘制）。

![](assets/accounts-sharing-probability-bar.png)

同样，红线标记当前阈值，浅红色区域包含该阈值以上的所有帐户的总数。

>[!NOTE]
>
> 条形图的y轴为对数。

### 当前区段中的帐户超过阈值{#threshold-selector}

此面板允许您为上面的圆环图和条形图选择阈值范围。 四个选项包括：

* 帐户&#x200B;**超低**&#x200B;共享&#x200B;**概率**

* 帐户&#x200B;**超出低**&#x200B;共享&#x200B;**概率**

* 帐户&#x200B;**超过审核**&#x200B;共享&#x200B;**概率**

* 帐户&#x200B;**超出高**&#x200B;共享&#x200B;**概率**

![](assets/threshold-selector-shared-accounts.png)

选择阈值后，该面板将显示所选段中所有订阅者帐户的帐户百分比（和数量）。

## 区段播放请求总数 {#play-request-out-total}

圆环图显示区段中订阅者发出的播放请求的百分比（和数量），可让您比较不在定义区段中的订阅者发出的播放请求。

![](assets/play-req-outof-total.png)

当您将光标移动到圆环图上时，它还会显示不同概率范围内的订户百分比和数字。

<!--![](assets/play-request-total.gif)-->

## 区段平均每帐户设备数{#avg-devices-account}

条形图显示当前区段中的订阅者当前使用的每种类型的平均设备数以及当前区段中未使用的平均设备数。

![](assets/avg-devices-per-acc.png)

## 每个帐户每个期间的区段邮政编码 {#zip-codes-period-account}

此图表告知您当前区段中在给定时间间隔内从不同位置（按邮政编码测量）使用内容的订阅者数量。

![](assets/zip-period-account.png)

>[!NOTE]
>
>您可以通过双击这些条来放大代表一组以上邮政编码(以&#x200B;**+** （加号）（例如，10+）表示)的条。


## 每个帐户的每个期间的区段 — 地理跨度 {#geo-span-period-account}

此条形图绘制了使用来自不同地理区域（以英里为单位）的内容的订阅者帐户数量。 该范围基于在时间间隔内订户已从中进行流式传输的位置之间的最大距离。

![](assets/geogr-span-account.png)

>[!NOTE]
>
> 您可以通过双击这些栏来放大代表一组以上地理距离(以&#x200B;**+** （加号）（例如，1000+）表示)的栏。

>[!MORELIKETHIS]
>
>* 了解如何使用[导出前1000个帐户](/help/accountiq/export-acc-information.md)选项，通过共享帐户报表中的筛选器，导出所选区段中前1000个订阅者的报表。
