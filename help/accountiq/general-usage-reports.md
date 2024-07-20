---
title: 一般使用情况报表
description: 了解一般使用情况报表
exl-id: 1272073a-61fe-47ec-aced-2e8055b6b11e
source-git-commit: 4a8a73d6c67508e88ba3ffbb9033b7e339f4fe8f
workflow-type: tm+mt
source-wordcount: '1055'
ht-degree: 0%

---

# [!UICONTROL General usage]个报告 {#general-usage-reports}

[!UICONTROL Account IQ]报告是基本的分析工具，可让您深入查看数据以隔离[同类群组](/help/accountiq/product-concepts.md#segmet-def)、识别异常并了解您的帐户特征。

[!UICONTROL General usage]报表页面提供了一些工具，用于根据正在使用的帐户设备数量、检测到的IP及其各自的邮政编码来划分子组量度。

这些报告全部基于从[区段和时间间隔](/help/accountiq/segments-timeinterval.md)面板中选择的当前区段。 可以通过在[快照概述 — 帐户超过阈值](#snapshot-overview)面板中指定（设备数、IP数和邮政编码数）阈值来优化选择并进一步缩小选择范围。

## 播放请求和独特订阅者 {#playreq-uniquesubs}

此处的线形图为您提供一段时间内值的变化视图，例如所定义区段在选定时间间隔内的播放请求和独特订阅者。

+++ D2C服务：播放请求/独特订阅者

![](assets/d2c-line-graph-gu.png)


*播放D2C服务的请求/唯一订阅者*

+++

+++程序员：播放请求/独特订阅者

![](assets/progr-line-graph-gu.png)


*程序员的播放请求/独特订阅者*

+++

+++MVPDs：独特订阅者

![](assets/mvpd-line-graph-gu.png)

MVPD的&#x200B;*唯一订阅者*

+++

<br/>

x轴表示基于当前间隔的时间，y轴表示该期间的基本订户活动度量。 折线图可帮助您可视化并比较当前区段中订阅者的活动。 根据Account IQ的版本，这些量度包括：

* **AuthN OK**：成功的身份验证次数。 详细了解[AuthN确定](/help/accountiq/product-concepts.md#authn-ok-def)。

* **AuthZ OK**：成功的授权数。 详细了解[AuthZ OK](/help/accountiq/product-concepts.md#authz-ok-def)。

* **播放请求**：播放请求数。 阅读有关[播放请求](/help/accountiq/product-concepts.md#play-requests-def)的更多信息。

* **独特订阅者**：成功的独特订阅者数。 详细了解[独特订阅者](/help/accountiq/product-concepts.md#unique-subscriber-def)。

>[!NOTE]
>
>量度的可用性因Account IQ的版本而异。

## 快照概述 — 帐户超过阈值 {#snapshot-overview}

使用此附加过滤器优化分析和报告，以设置各种使用阈值。 选择区段后，您还可以使用以下过滤器来进一步分析订阅者行为：

* 设备数量阈值

* IP数量阈值

* 邮编数阈值

当您根据所选阈值](#account-segments-basedon-segments)在基于[帐户区段的面板中更新阈值时，您将在以下位置查看效果：

* [每个帐户的每周（或每月）设备数](#devices-week-account)

* [每个帐户的每周（或每月）位置数](#locations-week-account)

* [每个帐户每周（或每月）的IP数](#ip-week-account)

* [帐户段的历史视图](#account-segment-historical-view)

>[!NOTE]
>
>每个阈值都设置为默认值4。 这意味着，“一般用法”页面显示对使用四个以上设备（使用四个以上不同的IP地址、*和*&#x200B;四个以上不同的邮政编码）的订阅者的分析。

### 基于所选阈值的帐户区段 {#account-segments-basedon-segments}

基于所选阈值的&#x200B;**帐户区段**&#x200B;面板为您提供了设置设备数、IP数和邮政编码数阈值（介于1和10之间）的选项。

该图显示了：

* 订阅者帐户的绝对数。

* 在区段中正在使用设备数、IP数以及阈值指定的邮政编码数的订阅者帐户总数中的百分比。

![](assets/select-thresholds.png)

## 每个帐户的每周（或每月）设备数 {#devices-week-account}

此条形图提供了关于订阅者如何使用其设备访问内容的使用行为的见解。

x轴绘制帐户数，y轴绘制设备数。 根据您为每个帐户设置的设备数阈值，它将标记一周时间内从特定数量设备消费内容的订阅者帐户的绝对数量。

![](assets/bar-gr-devices-w-acc.png)

将鼠标悬停在栏上（特定于设备数）时，会出现一个标签，提供有关一周内使用这些许多设备流式传输渠道内容的订户帐户数（以及区段中的订户帐户总数占订户帐户总数的百分比）的信息。

该图还标记了以下内容：

* 用于标记您设置的阈值的红线。

* 一条绿线，用于标记订阅者帐户每周（或每月）使用的不同设备的平均数。

圆环图提供了当前段中帐户在设定的阈值以上使用的设备的替代视图。

![](assets/donut-devices-w-acc.png)

## 每个帐户的每周（或每月）位置数 {#locations-week-account}

与每个帐户](#devices-week-account)每周（或每月）设备[的量度类似，每个帐户每周（或每月）的位置量度允许您从不同位置分析订阅者帐户的使用情况。 x轴绘制帐户数，y轴绘制位置数。

![](assets/graph-loc-week-acc.png)

在设置了位置数量的阈值后，可以使用图形来标识以下内容：

* 一周内从（特定）x个位置消费内容的订阅者数量（和百分比）。

* 从超过阈值的更多位置查看内容的订阅者帐户总数的百分比。

* 将每周平均值（帐户的不同位置数量）与阈值进行比较。

## 每个帐户每周（或每月）的IP {#ip-week-account}

与&#x200B;**每个帐户每周的位置数**&#x200B;的量度类似，**每个帐户每周IP数**&#x200B;量度允许您评估当前区段的流源更改量。

x轴绘制帐户数，y轴绘制IP数。

![](assets/graph-ip-week-acc.png)

定义区段并设置IP数量的阈值后，可以使用图形来标识以下内容：

* 一周内使用特定数量IP内容的订阅者的数量（和百分比）。

* 从超过阈值的IP地址查看内容的总订户帐户数的百分比。

* 将每周平均值（帐户的不同IP数量）与阈值进行比较。

## 帐户段 — 历史视图 {#account-segment-historical-view}

历史视图条形图可帮助您比较不同时间间隔内的使用度量。 此外，它还集体绘制各种使用量度，如每个帐户](#devices-week-account)每周（或每月）的[设备、每个帐户](#locations-week-account)每周（或每月）的[位置，以及每个帐户](#ip-week-account)每周（或每月）的[IP。

* x轴绘制时间间隔，y轴绘制订户帐户、设备、位置和IP的数量。

* 橙色条表示不同时间间隔中的区段。

* 折线图以曲线表示每个帐户](#devices-week-account)每周（或每月）的[个设备、每个帐户](#locations-week-account)每周（或每月）的[个位置以及整个时间间隔内（基于阈值）每个帐户](#ip-week-account)值的[周（或每月）IP数的变化。

![](assets/historical-view.png)

* 蓝色条表示在某个时间间隔内整个行业的活跃订阅者总数。

* 您可以选择特定的图例，它们可帮助您缩放图形。

![](assets/historical-view-total.png)
