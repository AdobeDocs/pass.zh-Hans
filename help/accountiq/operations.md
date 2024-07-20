---
title: 帐户IQ中的操作
description: Account IQ中的操作包括采取措施对订阅者帐户执行自动化和批量操作并跟踪其效果。
exl-id: ba6bceca-221c-42db-b207-804e4b9f6d54
source-git-commit: 85316a40ba5f6564c84a5aecf689c077e936a91a
workflow-type: tm+mt
source-wordcount: '1417'
ht-degree: 0%

---

# 操作 {#operations-tab-next-steps}

在使用[!DNL Account IQ]分析分析选定区段的订阅者使用模式并识别密码共享实例后，您可以通过[!DNL Account IQ]中称为操作的重点过程执行目标操作。

**操作**&#x200B;允许您有效地跟踪和管理一组帐户的凭据共享，以缓解密码共享并增强重要订阅者的体验。

您可以对定义的[区段](/help/accountiq/product-concepts.md#segment-def)应用操作，以便在特定的[时间间隔](/help/accountiq/product-concepts.md#time-interval-def)内解决密码共享问题，并安排在将来的日期执行该操作。 这些操作包括最小化密码共享或放宽非共享帐户限制的限制。

使用操作，您不仅可以指定操作及其范围，还可以衡量其结果。

通过评估结果，您可以优化策略以优化效果，无论是通过转化借款人、减少凭据共享还是减少流失。

您可以通过操作执行各种功能：

* [查看工序报表](#operation-reports)
* [创建新操作](#create-new-operation)
* [停止操作](#stop-operation)

## 查看工序报表 {#operation-reports}

您可以通过操作报告来查看操作效果。 要查看操作报告，请选择Account IQ应用程序左侧面板中&#x200B;**操作**&#x200B;下的&#x200B;**操作**&#x200B;选项卡。 此时将显示系统中可用的操作列表。 您可以采用表格格式访问有关每个操作的键详细信息。 详细信息包括：

* 操作的名称
* 当前状态（例如，已计划、正在运行、已结束、错误或已停止）
* 进度完成百分比
* 对其应用操作的目标受众或区段
* 为操作选择的操作类型
* 操作的开始日期
* 操作的结束日期
* 创建操作的日期
* 操作的上次修改日期

![](assets/operations-page.png)

*列出Account IQ中现有操作的列表和详细信息*

从操作列表中选择所需的&#x200B;**操作名称**。 将显示以下报告：

### 操作性能 {#operation-performance}

操作性能提供顶行读出，汇总操作[评估期](/help/accountiq/product-concepts.md#evaluation-period-def)内该区段中受影响的帐户数、操作进度和帐户的总体共享分数。

![操作性能报告](assets/operation-performance.png)

*操作性能报告*

**A.**&#x200B;影响帐户&#x200B;**B.**&#x200B;操作进度&#x200B;**C.**&#x200B;总体共享分数

#### 受影响的帐户 {#impacted-accounts}

此数字显示受操作评估期间采取的操作影响的订户帐户的计数。

#### 操作进度 {#operation-progress}

此量规显示超出计划时间表的天数以及操作完成的百分比。

#### 总体共享得分 {#overall-sharing-score}

此线形图表示[总体共享得分](/help/accountiq/data-panels.md#overall-sharing-score)，其中包括在操作的评估期间每周共享帐户的共享级别和使用情况。

### 运营影响：段中的帐户 {#impact-accounts}

此报表显示为栈叠的柱状图，以说明操作随时间推移产生的影响。

![操作对区段图中的帐户的影响](assets/accounts-in-segment.png)

*操作对区段图中的帐户的影响*

x轴表示操作的[评估期](/help/accountiq/product-concepts.md#evaluation-period-def)，而y轴表示操作区段中的帐户状态。 图形中的每个条形图都分为三种颜色：

* 粉红色表示满足该区段在此操作中使用的条件的帐户数。

* 蓝色表示在操作的[评估期](/help/accountiq/product-concepts.md#evaluation-period-def)的每周或每月中，最初位于区段中但不满足区段条件的活动帐户数。

* 灰色表示在评估期间处于非活动状态的帐户。

>[!NOTE]
>
>第一个粉红色条表示在评估期开始时符合运营区段条件的帐户数。

随着时间的推移，此图表将说明帐户行为相对于原始标准的变化（例如，共享概率超过90且使用5台以上的设备变得不活动）。

### 操作影响：共享帐户指标 {#impact-shared-accounts}

共享帐户量度提供了在操作的[评估期](/help/accountiq/product-concepts.md#evaluation-period-def)内，操作区段中的订阅者帐户的共享级别和播放请求的概述。

#### 共享级别 {#share-level}

此线形图表示在操作的评估期间每周的[共享级别](/help/accountiq/data-panels.md#sharing-level)。

![共享级别折线图](assets/share-level.png){width="550" align="left"}

*共享级别折线图*

#### 播放请求数 {#play-requests}

此折线图表示在操作的评估期间每周的[播放请求](/help/accountiq/general-usage-reports.md#playreq-uniquesubs)。

![播放请求数折线图](assets/number-play-requests.png){width="550" align="left"}

*播放请求数折线图*

### 操作影响：常规使用量度 {#impact-general-usage}

常规使用量度提供了在操作[评估期](/help/accountiq/product-concepts.md#evaluation-period-def)内操作区段中的平均设备、IP和位置数的概览。

#### 设备数量 {#devices}

此线形图表示在操作的评估期间每周平均[台设备](/help/accountiq/general-usage-reports.md#devices-week-account)。

![设备数量折线图](assets/number-devices.png){width="550" align="left"}

*设备数量折线图*

#### IP和位置数量 {#IPs-locations}

此折线图表示在操作的评估期内，每周平均[个IP地址](/help/accountiq/general-usage-reports.md#ip-week-account)和[个位置](/help/accountiq/general-usage-reports.md#locations-week-account)。

![IP数量和位置折线图](assets/number-ips-locations.png){width="550" align="left"}

*IP数量和位置折线图*

要关闭报告并返回主&#x200B;**操作**&#x200B;页面，请选择左侧面板中&#x200B;**操作**&#x200B;下的&#x200B;**操作**&#x200B;选项卡。

## 创建新操作 {#create-new-operation}

当您转到左侧面板中&#x200B;**操作**&#x200B;下的&#x200B;**操作**&#x200B;选项卡时，请选择&#x200B;**操作**&#x200B;页面顶部的&#x200B;**新建操作**。

要创建新操作，请按照以下部分中的说明进行操作：

* [操作详细信息](#operation-details)
* [区段](#segment)
* [操作](#action)
* [计划](#schedule)

### 操作详细信息 {#operation-details}

在此部分中，在&#x200B;**操作名称**&#x200B;中键入该操作的名称。

>[!TIP]
>
>描述&#x200B;**操作名称**&#x200B;中操作的目的或操作的性质，以便快速识别。 **添加描述和标记**&#x200B;的选项将在未来版本中提供。

![在操作详细信息中添加操作名称](assets/operation-details.png)

*添加操作名称*

### 区段 {#segment}

在此部分中，单击&#x200B;**选择区段**&#x200B;并选择要使用此操作的区段。 了解[如何选择区段](/help/accountiq/segments-timeinterval.md#segment-selection)。

选择区段后，使用 <img alt= "展开区段摘要" src="./assets/expand-segment-summary.svg" width="25">图标以查看详细的区段摘要。 阅读有关[区段摘要](segments-timeinterval.md#segment-summary)的更多信息。

![选择区段和时间间隔](assets/select-segment-timeinterval.png)

*选择区段和时间间隔*

>[!NOTE]
>
>在上一图像中显示的[视频类别](product-concepts.md#video-category-def)，如&#x200B;**MVPD**、**程序员**&#x200B;和&#x200B;**频道**，表示在Account IQ的TV Everywhere版本中使用的标签。 如果您以D2C服务身份登录，则这些标签会显示您公司的特定视频类别。

如果需要，请使用 <img alt= "编辑区段" src="./assets/edit-segment.svg" width="25">图标以编辑选定的区段或  <img alt= "创建新区段" src="./assets/create-new-segment.svg" width="25">图标以创建新区段。 有关更多详细信息，请参阅[创建新区段](work-with-segments.md#create-new-segment)或[编辑区段](work-with-segments.md#edit-segment)的说明。

>[!IMPORTANT]
>
>当前默认选择名为&#x200B;**[!UICONTROL Fixed number of accounts]**&#x200B;的&#x200B;**区段类型**。 选择&#x200B;**[!UICONTROL Variable number of accounts]**&#x200B;的选项将在即将发布的版本中提供。

选择&#x200B;**粒度和时间间隔**&#x200B;以监视特定期间的操作。 详细了解[如何选择粒度和时间间隔](/help/accountiq/segments-timeinterval.md#granularity-timeinterval)。

### 操作 {#action}

在此部分中，从下拉菜单中选择要针对所选区段执行的&#x200B;**操作**。

![选择操作类型](assets/apply-actions.png)

*选择操作类型*

有两个可用选项：

* 为与Account IQ集成的并发监控系统选择&#x200B;**CM策略**。

* 选择&#x200B;**外部操作**&#x200B;以创建并处理Account IQ外部且未与Account IQ系统集成的工作流。

>[!NOTE]
>
>外部操作可能并不总是与密码共享直接相关，但仍会影响密码共享，例如启动新季。

### 计划 {#schedule}

在此部分中，从日期选择器中选择&#x200B;**开始日期**&#x200B;和&#x200B;**结束日期**&#x200B;以设置操作的激活。

>[!IMPORTANT]
>
>目前，默认激活&#x200B;**开始日期**&#x200B;和&#x200B;**结束日期**&#x200B;设置为&#x200B;**日期**。 即将发布的版本中将提供用于选择&#x200B;**当满足条件时**&#x200B;和&#x200B;**手动**&#x200B;的选项。

>[!NOTE]
>
>确保开始日期和结束日期均与&#x200B;**步骤4**&#x200B;中选择的评估粒度一致。

* 如果已选择按周汇总粒度，请选择以周为单位的开始和结束日期（例如，第10周）。
* 如果已选择按月聚合粒度，请选择以月为单位的开始和结束日期。

![从日期选取器中选择开始日期和结束日期](assets/add-schedule.png)

*从日期选取器中选择开始日期和结束日期*

**A.**&#x200B;开始日期选取器&#x200B;**B.**&#x200B;结束日期选取器

>[!NOTE]
>
>**开始日期**&#x200B;必须晚于评估时段和当前日期，而&#x200B;**结束日期**&#x200B;必须晚于开始日期和当前日期，才能在未来的时段中计划和执行操作。

选择&#x200B;**操作**&#x200B;页面顶部的&#x200B;**保存操作**&#x200B;以处理新操作。

## 停止操作 {#stop-operation}

您只能停止当前处于&#x200B;**正在运行**&#x200B;状态的操作。 要停止现有操作，请执行以下步骤：

1. 导航到Account IQ应用程序左侧导航栏中&#x200B;**操作**&#x200B;下的&#x200B;**操作**&#x200B;选项卡。
1. 选择要停止的操作的&#x200B;**选项**&#x200B;菜单。

   ![选择选项菜单以停止操作](assets/stop-operation.png)

   *选择“选项”菜单以停止操作*

1. 选择&#x200B;**停止**。



