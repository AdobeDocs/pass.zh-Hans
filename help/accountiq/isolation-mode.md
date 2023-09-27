---
title: 以隔离模式查看报告
description: 以隔离模式查看Xfinity的报告。
exl-id: e7cf24c5-9bfa-48f6-b5c8-20443a976891
source-git-commit: d543bbe972944ad83f4cb28c8a17ea6e10f66975
workflow-type: tm+mt
source-wordcount: '459'
ht-degree: 0%

---

# 以隔离模式查看共享报告 {#report-isolation-mode}

在隔离模式中，MVPD（例如Xfinity）始终识别跨设备的订户，但根据其交互的程序员以不同的方式识别其订户。 而在标准模式中，MVPD一致地识别跨设备的订户，而不管程序员是谁。

例如，在下图中，如果隔离模式MVPD的订户B（如Xfinity）使用同一设备访问由两个不同程序员提供的内容，则MVPD将不同的标识符与两个不同的访问尝试相关联。 因此，对于这些程序员（图中的L和M）和Account IQ来说，似乎有两个不同的订阅者访问内容。 但是，对于标准MVPD，如果订户B访问由两个不同的程序员提供的内容，则MVPD将关联用于两次访问尝试的单个访问标识符。 隔离模式中的MVPD（例如Xfinity）不能一致地识别订户，即使订户跨不同的程序员使用相同的设备。

![](assets/isolation-diff-new.png)

*图：隔离模式MVPD标识四个不同的订户，而不是两个*

为了管理数据失真（由于根据访问不同程序员将同一用户识别为不同用户），隔离模式将有关程序员报告的活动限制为仅在该程序员的应用程序上报告的活动。 例如，对于上图中的隔离模式，程序员L仅根据身份W和Y的活动查看数据，而忽略身份X和Z。

>[!IMPORTANT]
>
> 缺点是，由于与L以外的任何程序员的活动，程序员L不能共享收集到的关于订阅者A和B的信息。

在隔离模式下，为获得共享分数和所有相关量度所做的所有计算仅使用从属于所选程序员和渠道的应用程序流式传输的设备活动来进行。
共享得分和概率仅使用从当前所选渠道开始的流计算。

要在隔离模式下查看度量，请执行以下操作：

1. 选择 **[!UICONTROL isolation mode]** 从 **[!UICONTROL MVPDs in segment]** 下拉选项，然后选择 **[!UICONTROL Apply Selection]**.

   ![](assets/xfinity-in-segment.gif)

   *图：隔离模式下的MVPD选择*

1. 从中选择所需的渠道 **[!UICONTROL Channels in segment]** 下拉选项，然后选择 **[!UICONTROL Apply Selection]**.

   另外，选择 [时间范围](/help/accountiq/product-concepts.md#granularity-def).

   >[!IMPORTANT]
   >
   >由于在对所有程序员的应用程序进行流式传输时，帐户共享更相关，因此，在隔离模式下，您将会看到较低的共享分数，以及量度中的一些变化。

   ![](assets/aggregate-sharing-isolation.png)

   *图：隔离模式下的共享概率表*

   请注意，上述测量结果显示，所有账户中只有6%被共享；只有8%的内容被这8%的人使用。 因此，这些通道可以将其隔离模式下的得分与其他MVPD上的得分进行比较。 因此，使用隔离模式获取的信息应解释为与其他数据不同。
