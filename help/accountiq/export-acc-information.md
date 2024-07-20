---
title: 导出共享得分较高的帐户的信息
description: 导出共享得分较高的帐户的信息。
exl-id: df41ddd2-fde3-4861-abd4-6e32f0be9ea5
source-git-commit: 88b11527b2a432c2cd27bf9e29fd286969036eb0
workflow-type: tm+mt
source-wordcount: '676'
ht-degree: 1%

---

# 导出共享得分较高的帐户的信息 {#export-account-info-high-score}

[!UICONTROL Account IQ]允许您根据前1000个订阅者帐户的[共享概率](/help/accountiq/product-concepts.md#account-sharing-probability-def)导出其帐户共享详细信息。 您可以在[共享帐户报表](/help/accountiq/shared-acc-reports.md)页面上导出当前[区段](/help/accountiq/product-concepts.md#segment-def)和[指定时间间隔](/help/accountiq/product-concepts.md#time-interval-def)的帐户共享信息。

按照以下步骤导出特定段的订阅者帐户的帐户共享信息。

1. 使用您的凭据登录。
1. 导航到&#x200B;**报表**&#x200B;部分下的&#x200B;**共享帐户**&#x200B;选项卡。
1. 从区段和时间间隔面板中选择所需的区段和时间间隔。 了解[如何选择区段和时间间隔](segments-timeinterval.md)。

   如果需要，请参阅[创建区段](work-with-segments.md#create-new-segment)或[编辑区段](work-with-segments.md#edit-segment)的说明。

1. 选择位于区段和时间间隔面板右上角的&#x200B;**[!UICONTROL Export top 1000 accounts]**。

   ![导出前1000个帐户](assets/export-top-1000-accounts.png)

   *选择“导出前1000个帐户”选项*

该文件将自动以.csv格式下载到您的本地计算机。

此文件包含前1000个帐户的数据，该数据基于当前段中的订阅者帐户的共享概率，按递减顺序排列。

以下是导出的.csv文件示例。

![已导出.csv文件中的数据](assets/exported-csv.png)

*已导出.csv文件中的数据*

## 导出的报告中的列 {#columns-in-export}

**周/月**

区段选择器中&#x200B;**[!UICONTROL Granularity and Time Interval]**&#x200B;选项内选定的周或月。

**MVPD**

如果您是程序员，列会显示与其订阅帐户的分发商。

>[!NOTE]
>
> **MVPD**&#x200B;列仅适用于TV Everywhere版本。

**订阅者ID**

特定帐户的唯一标识符。

**最小设备数**

用户从中主动流式传输内容的最小设备数。

>[!NOTE]
>
>流内容的实际设备数大于为特定帐户指定的最小设备数。

**最小人员数**

使用这些设备积极流式处理内容的最小个人数。

>[!NOTE]
>
>流内容的实际个人数大于分配给特定帐户的最小人员数。

**[!UICONTROL # IPs]**

从中对内容进行流式传输的IP地址数。

**[!UICONTROL # Locations]**

对内容进行流式处理的位置数（基于邮政编码）。

**[!UICONTROL # Cities]**

流活动发生的城市数。

**[!UICONTROL # States]**

已发生流活动的状态的数量。

**[!UICONTROL # Clusters]**

已进行流处理的非重复[群集](/help/accountiq/product-concepts.md#cluster-def)的数量。

**[!UICONTROL Geographic span (miles)]**

与帐户关联的流位置之间的最大距离。

**[!UICONTROL # AuthN OK]**

指定时间段内用户使用该帐户登录的次数。

>[!NOTE]
>
> 某些D2C服务可能未看到&#x200B;**[!UICONTROL # AuthN OK]**&#x200B;数据，因为它可能未包含在其公司的数据中。

**[!UICONTROL # AuthZ OK]**

MVPD授权流或授予该帐户内容访问权限的次数。

>[!NOTE]
>
>**[!UICONTROL # AuthZ OK]**&#x200B;不可用于D2C服务。

>[!NOTE]
>
>对于所有位置的电视，**[!UICONTROL # AuthZ OK]**&#x200B;与&#x200B;**[#个播放请求](/help/accountiq/product-concepts.md##play-requests-def)**&#x200B;的数量相关。 它始终小于&#x200B;**[!UICONTROL # Play Requests]**，因为Adobe通常会从MVPD缓存授权约24小时。


**[!UICONTROL # Play Requests]**

在指定时间段内发生的实际流数量。

>[!NOTE]
>
>[#播放请求](/help/accountiq/product-concepts.md##play-requests-def)列在TV Everywhere MVPD版本不可用。

**[!UICONTROL # Channels]**

帐户在指定时段内观看的渠道总数。

>[!NOTE]
>
> 对于D2C服务&#x200B;**[!UICONTROL # Channels]**，等同于&#x200B;**[!UICONTROL # Video categories]**&#x200B;的数量。

>[!NOTE]
>
>对于TV Everywhere，其中包含可能不属于登录程序员的频道。 帐户的此号码包括指定时间段内访问的渠道和其他渠道。


**使用模式**

这些列中的值用作标识符，对应于我们用于对所有用户帐户进行分类的14种模式之一。

<table>
    <tbody>
      <tr>
        <th style="width:10%">ID</th>
        <th style="width:30%">使用模式</th>
      </tr>
      <tr>
        <td>1</td>
        <td>普通用户</td>
      </tr>
      <tr>
        <td>2</td>
        <td>旅行者或通勤者</td>
      </tr>
      <tr>
        <td>3</td>
        <td>大家庭</td>
      </tr>
      <tr>
        <td>4</td>
        <td>亲朋好友</td>
      </tr>
      </tr>
         <td>5和8</td>
         <td>社交组共享</td>
      </tr>
      </tr>
         <td>6</td>
         <td>一大群朋友</td>
      </tr>
      </tr>
         <td>7</td>
         <td>并发流</td>
      </tr>
      </tr>
         <td>9</td>
         <td>社区共享</td>
      </tr>
      </tr>
         <td>10和11</td>
         <td>不确定行为</td>
      </tr>
      </tr>
         <td>12</td>
         <td>小型家庭</td>
      </tr>
      </tr>
         <td>13</td>
         <td>第二个主页 </td>
      </tr>
      </tr>
         <td>14</td>
         <td>使用异常</td>
      </tr>
    </tbody>
  </table>

*使用模式的导出的.csv映射中的使用模式标识符*

**共享概率**

特定帐户共享其凭据的可能性。

>[!NOTE]
>
> 所选区段中所有帐户的平均共享概率用于计算[平均共享得分](/help/accountiq/data-panels.md#aggregated-sharing)的[共享级别](/help/accountiq/data-panels.md#sharing-level)。
