---
title: 限制
description: 了解帐户IQ中面向程序员的限制和隔离模式MVPD 。
exl-id: 08d65716-8b6a-4300-acda-fec63e1e6815
source-git-commit: 791d661e1495bdb6fe4eb25efbefeecd813f0f3a
workflow-type: tm+mt
source-wordcount: '273'
ht-degree: 0%

---

# 限制 {#limitations}

D2C和TV Everywhere版本的Account IQ为流媒体提供商提供使用和订阅共享分析。 但是，当前版本1.3存在一些限制，将在未来版本中予以解决。

* 此 [总体共享得分](/help/accountiq/data-panels.md#overall-sharing-score) 当前包括 [共享级别](/help/accountiq/data-panels.md#sharing-level) 和 [共享帐户的使用情况](/help/accountiq/data-panels.md#usage-from-shared-accounts). 未来版本将包含更多量度。

* 在功能板或使用模式中定义区段时， [区段中的视频类别](/help/accountiq/data-panels.md#video-categories-segment)， [按渠道和MVPD共享得分](/help/accountiq/data-panels.md#sharin-score-by-channels-and-mvpds)、和 [视频类别的使用模式分发](/help/accountiq/usage-patterns.md#usage-pattern-dis-video-categories) 报表最多只能显示20个的数据 [视频类别](product-concepts.md#video-category-def). 包含超过20个视频类别的区段将不会在这些报表中显示数据。

* 目前，导出帐户统计信息的选项仅限于导出1000个帐户。

* 定义操作时，要选择的选项 [区段类型](/help/accountiq/operations.md#segment) 限制为 **固定帐户数**. 此 **帐户的可变数量** 选项将在未来版本中提供。

* 此 **基准测试**， **检测模型**， **操作**、和 **设置** 左侧导航中的部分当前已禁用，并将在未来版本中提供。

* 创建操作时，只能应用两种 [操作](/help/accountiq/operations.md#action)  — 并发监视规则和外部操作。

* 目前，您只能 [创建](/help/accountiq/operations.md#create-new-operation) 和 [计划](/help/accountiq/operations.md#schedule) 操作。 在未来的版本中，您可以暂停、继续并完全管理这些页面。

* 选择“粒度”和“时间间隔”时，您一次只能分析一周或一个月的数据。

* 不能将隔离模式MVPD与任何其他MVPD一起添加到段定义中。 某些MVPD无法唯一识别多个程序员渠道中的订户。 因此，这些MVPD对于TV Everywhere程序员是分开处理的。



