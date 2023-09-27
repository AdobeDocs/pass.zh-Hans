---
title: Account IQ术语表
description: 产品术语词汇表。
exl-id: 2ee54442-9538-4c30-b999-265310b3935f
source-git-commit: d543bbe972944ad83f4cb28c8a17ea6e10f66975
workflow-type: tm+mt
source-wordcount: '1383'
ht-degree: 0%

---

# 产品概念和术语表 {#glossary}

请参阅以下产品术语及其定义。

## [!UICONTROL Accounts Sharing Probability] {#account-sharing-probability-def}

带有图表的仪表板面板，它将当前区段共享得分分为“非常低”、“低”、“中”、“高”和“非常高”共享范围类别。

## [!UICONTROL Action] {#action-def}

与相关的直接或间接事件 [操作](#operation-def) ，这会影响相关操作区段（或同类群组）的特性（例如，共享得分或使用中的设备数量）。

## [!UICONTROL Aggregated sharing score] {#sharing-probability-level-def}

带有图表的仪表板面板，可将当前区段共享得分分为“非常低”、“低”、“中等”、“高”和“非常高”共享范围类别，以及每个类别占区段流处理总量的百分比。

## [!UICONTROL AuthN] {#authn-def}

身份验证，或身份验证尝试的次数。 身份验证尝试是一个过程，没有当前有效身份验证状态的用户将被重定向到他们选择的MVPD，在那里，他们向MVPD标识自己 — 通常使用用户名和密码。

## [!UICONTROL AuthN OK] {#authn-ok-def}

成功验证的次数。 当用户标识由MVPD确认并导致用户被重定向回程序员应用程序或站点时，就会发生成功的身份验证。

## [!UICONTROL AuthZ] {#authz-def}

授权，或授权请求的次数。 授权请求是程序员通过Adobe从MVPD请求权限以开始流式传输用户请求内容的过程。 MVPD通常基于与用户的MVPD订阅相关联的内容权限来授予请求（例如，与内容相关联的频道是否在用户的订阅中）。 有些授权请求响应是通过Adobe缓存的，这允许Adobe立即响应，而无需将请求传递到MVPD。

## [!UICONTROL AuthZ OK] {#authz-ok-def}

成功的授权数。

## [!UICONTROL Channel] {#channel-def}

渠道（也称为“属性”）是与主题相关的视频内容源。 传统上，表示来自MVPD的不同、数字可寻址连续视频馈送。 该渠道直接映射到可供订阅者通过其机顶盒(STB)访问的内容渠道。

## [!UICONTROL Cluster] {#cluster-def}

群集是位置和设备的集合。 群集是通过在设备之间查找公共位置创建的。 在公共位置看到的设备将被视为属于同一群集。 两个设备可以在同一群集中，即使它们没有公共位置，但可以通过其他设备的位置进行连接。

### [!UICONTROL Mobile cluster] {#mobile-cluster-def}

没有静态设备的群集。

### [!UICONTROL Static cluster] {#static-cluster-def}

至少具有一个静态设备的群集。

## [!UICONTROL Concurrency] {#consurrency-def}

并发由同时播放或时间非常接近的两个（或多个）流定义，因此它们之间的间隔不能通过以正常速度行驶来调整。
并行使用量是使用2个不同群集之间的最大速度（英里/小时）计算的。 如果用户的速度在小于124英里的距离上大于124米/小时，或者如果用户的速度在大于124英里的距离上大于400米/小时，则被视为同时使用。 计算来自不同群集的位置之间的距离。 同一群集中允许并发使用。

## [!UICONTROL Device] {#device-def}

一种数字视频硬件产品，可播放TV Everywhere内容，并受Adobe Pass支持。 例如，智能手机、笔记本电脑和台式计算机、游戏机和智能电视。

## [!UICONTROL Geographical Span] {#geographical-span-def}

一组位置中最远点之间的距离。

## [!UICONTROL Granularity] {#granularity-def}

对于时间范围，是指期间的大小；例如一周或一个月。

## [!UICONTROL Industry Average Index] {#industry-avg-index-def}

在选定的时间范围内为所有程序员和MVPD的每个风险指数（帐户、使用情况、总体）计算的值。

## [!UICONTROL IP] {#ip-def}

Internet服务提供商分配给设备的Internet协议地址。 例如，有线服务提供商，以及小区服务提供商。

## [!UICONTROL Isolation Mode] {#isolation-mode-def}

一种共享分析类型，在此类分析中，对帐户的评估仅限于在选定区段中直接发生在程序员身上的事件。  通常，会评估所有帐户事件，从而提供更加准确的共享估计。  某些MVPD数据的结构仅允许隔离模式分析。

## [!UICONTROL Location] {#location-def}

地球上独一无二的点。 它还称为特定播放请求的地理位置，使用Pass数据检测，精度为1000mx1000m（1平方公里）。

<!-- ### Home location {#home-location-def}

the precision is 0.01 ~ 2000mx2000m (4 square km) - at this moment the home location is detected using an ML algo, based on data provided by two mvpds. The probability is ~ 80%. We are not using the zip code for the majority of the users. Currently, this information is not used in assessing the sharing probability. -->

## [!UICONTROL Media Company] {#media-company-def}

Media Company是一家拥有一组媒体网络的公司。

## [!UICONTROL Metric] {#metric}

量度是订阅者帐户的属性（例如，订阅者帐户的MVPD、程序员和他们流传输内容的渠道、他们使用的设备数量）。

## [!UICONTROL Mobile device] {#mobile-device-def}

具有高移动性的设备。 例如，手机和平板电脑。

## [!UICONTROL MVPD] {#mvpd-def}

MVPD （也称为分销商）是媒体公司视频内容的汇总、转销商和分销商。

## 操作 {#operation-def}

操作是为跟踪特定操作效果而创建的记录 [操作](#action-def) 关联的区段上删除。 例如，可以对段标识的帐户允许的并发流数量进行限制。

## [!UICONTROL Overall sharing score] {#overall-sharing-score}

一个值，可帮助用户了解程序员属性或MVPD订阅者共享密码的程度，并让他们有一种紧迫感需要采取行动。

<!--**Aggregated Risk Index**
Also known as Risk Index and Sharing Risk Index, it is a value that helps users understand the magnitude of password sharing on Programmer properties or by MVPD subscribers and provides them a sense of urgency to act upon it.-->

<!--**Risk Index - Overall**
A value computed as an average of "Risk Index - Accounts" and the "Risk Index - Usage". Overall Sharing Risk Index-->

## [!UICONTROL Play Request] {#play-requests-def}

客户端应用程序或站点发出的请求，用于Adobe请求媒体令牌以记录和保护流启动。

## [!UICONTROL Programmer] {#programmer-def}

Programmer，也称为Network，是拥有和管理一个或多个渠道的大公司（公司）的子公司。

## [!UICONTROL requestorID] {#requestorid-def}

媒体公司用于标识自己或MVPD子公司的ID。  根据程序员实施，这可以映射到媒体公司、程序员或渠道。  媒体公司用于标识自己或MVPD子公司的最常见ID。  根据程序员实施，这可以映射到媒体公司、程序员或渠道。  传统上，这会映射到渠道。  通过创建诸如MML（三月疯狂Live）之类的伪通道以及采取技术性措施解决MVPD驱动的数据限制，请求者ID开始与媒体公司建立更多的关联。

## [!UICONTROL resourceID] {#resource-id-def}

最终用户请求的内容。  传统上，这已标识与用户请求的内容关联的渠道。  系统增强功能允许该ID代表特定节目（例如具有特定评级），该ID继续标识相关联频道。

## [!UICONTROL Risk Index - Usage] {#risk-index-usage}

该值也称为“共享帐户的使用情况”，它根据每个帐户发出的播放请求数量并按照每个帐户的共享概率进行加权计算得出的值。 也称为“共享帐户风险指数的使用情况”。

## [!UICONTROL Segment] {#segmet-def}

区段是一组符合由选定量度指定的用户定义条件的帐户（例如“已观看渠道X、Y或Z的MVPD A、B、C、D或E的用户”）。

## [!UICONTROL Sharing level] {#sharing-level-def}

风险指数，也称为帐户或共享帐户风险指数，它基于一组选定MVPD中每个帐户计算的平均共享概率来计算，该组选定MVPD在选定的时间范围内从某个选定程序员渠道进行流式处理。

## [!UICONTROL Static device] {#static-device-def}

移动性非常低的设备。 例如，游戏机、机顶盒和电视机。

## [!UICONTROL Time frame] {#time-frame-def}

该时段也称为“时段”或“时隙”，包含用户界面中显示的播放请求活动和从开始到结束的表格。

## [!UICONTROL Top MVPDs in segment] {#top-mvpds-def}

所选区段中的前MVPD（最多10个），以共享级别、共享帐户的使用情况或总体共享分数进行衡量。

## [!UICONTROL Trend] {#trend-def}

当前时段和上一时段之间关联量度的百分比差异（例如，播放请求总数的百分比）。

## [!UICONTROL Unique Subscribers] {#unique-subscriber-def}

在给定时间段内与程序员TV Everywhere涉及Adobe Pass的应用程序或网站进行交互的唯一MVPD帐户数。  该交互包括程序员应用程序或网站上导致调用Adobe Pass服务的任何活动。 例如，检查authN或authZ状态、身份验证和授权。 如果程序员使用的AdobeTempPass（即免费预览）是区段的一部分，则独特订阅者的总数还将包括独特设备的数量。

## [!UICONTROL Usage] {#usage-defs}

### [!UICONTROL Avid user] {#avid-user-def}

每月超过37个播放请求。

### [!UICONTROL Infrequent user] {#infrequent-users-def}

每月少于9个播放请求。

### [!UICONTROL Regular user] {#regular-user-def}

每月9到37个播放请求。

## [!UICONTROL Usage Pattern] {#usage-patern-def}

应用于帐户的一组有限的类别标签之一，最能体现帐户用户在社会群体或行为方面的特征（例如，小家庭、旅行者或通勤者、社交分享等）。

## [!UICONTROL Zip Code] {#zip-code-def}

与位于美国境内的位置关联的美国邮政编码
