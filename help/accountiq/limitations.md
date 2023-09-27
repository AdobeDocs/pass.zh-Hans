---
title: 限制和已知问题
description: 产品中的已知问题。
exl-id: 08d65716-8b6a-4300-acda-fec63e1e6815
source-git-commit: 2ced89dc1f77d2c090b599c40e778f3054f1a8dd
workflow-type: tm+mt
source-wordcount: '415'
ht-degree: 0%

---

# 已知问题和限制 {#known-issues}

Adobe致力于通过其产品提供强大的功能以及无缝的用户体验。 Account IQ的当前版本（版本1.0）高度信任地向流提供商提供使用情况和订阅共享分析。 但是，即将发布的版本将解决以下限制。

* 在功能板或报告页面中定义同类群组时，当前无法添加量度，例如 **设备数量** 以优化区段。 此功能将在未来版本中提供。

* 在评估单个帐户的共享得分时，帐户IQ会采取一种保守的方法，使公司能够高度自信地对待共享。 但是，这种方法往往会低估在汇总多个帐户之间的共享总量。

* 此 [总体共享得分](/help/accountiq/dashboard.md#overall-sharing-score) 当前仅因子 [共享级别](/help/accountiq/dashboard.md#sharing-level) 和 [共享帐户的使用情况](/help/accountiq/dashboard.md#usage-from-shared-accounts). 未来的版本将考虑其他量度。

* 在功能板或报告页面中定义同类群组时，截至目前，MVPD和渠道的选择器缺乏搜索机制。

* 在功能板或报告页面中定义同类群组时，限制最多只能选择10个MVPD和程序员（或单个渠道）。

* 截至目前，导出帐户统计的选项仅限于导出1000个帐户。

* 要选择的选项 [区段类型](#segment-type) 定义操作时，仅限于 **固定帐户数**. 此 **帐户的可变数量** 选项将在即将发布的版本中提供。

* 左侧导航中的“基准”、“检测模型”、“区段”、“快照”和“规则”部分当前已禁用，并将在下一版本中提供。

* 创建时 [操作](/help/accountiq/operation-affecting-user-segment.md)，您只能识别两种 [操作](/help/accountiq/operation-affecting-user-segment.md) 截至现在 — 并发监视规则和外部操作。

* 目前，只能创建操作 [已计划](/help/accountiq/operation-affecting-user-segment.md#action). 未来的版本将允许您暂停、恢复和完全管理它们。

* 由于使用的数据集较为有限，因此隔离模式无法真正反映共享的数量。 因此，隔离模式下的MVPD无法与任何其他MVPD进行比较。 <!--do we need to separate out this limitation, which is from a different persona i.e. only for Programmer persona?-->

* 在定义新的 [区段](/help/accountiq/segments-timeframe.md) 对于操作，您可以添加量度。 但是，如果选择保存的区段，则无法添加更多量度来优化该区段。

* 粒度和时间范围选择器限制为一周或一个月，这意味着只能在一周或一个月内评估数据。

* 当前在粒度和时间范围选择器中禁用预定义的时间间隔，并且将在未来版本中提供。
