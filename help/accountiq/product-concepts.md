---
title: Account IQ术语表
description: 产品术语词汇表。
exl-id: 2ee54442-9538-4c30-b999-265310b3935f
source-git-commit: cfcaa00ab05c99a64bcb0edfe5af60845a6769a9
workflow-type: tm+mt
source-wordcount: '1330'
ht-degree: 0%

---

# 产品概念和术语表 {#glossary}

## D2C和电视上的通用术语

以下产品术语及其定义对于Account IQ](versions-aiq.md)的所有[版本都是通用的。

### [!UICONTROL Accounts Sharing Probability] {#account-sharing-probability-def}

一个仪表板面板，上面有图表，将当前区段的共享得分分为非常低、低、中、高和非常高的共享范围类别。

### [!UICONTROL Action] {#action-def}

与[操作](#operation-def)关联的直接或间接事件，它会影响相关操作区段的特性（例如，共享得分或正在使用的设备数）。

### [!UICONTROL Aggregated sharing score] {#sharing-probability-level-def}

一个仪表板面板，上面有图表，将当前区段的共享得分分为“非常低”、“低”、“中等”、“高”和“非常高”共享范围类别，以及各个类别占区段的总流处理量的百分比。

### [!UICONTROL AuthN] {#authn-def}

身份验证尝试的次数。 身份验证尝试是指用户尝试使用D2C服务或MVPD登录的过程。 对于TV Everywhere用户，用户将被重定向到他们选择的MVPD，在那里他们向MVPD标识自己 — 通常使用用户名和密码。

### [!UICONTROL AuthN OK] {#authn-ok-def}

成功验证的次数。 当D2C服务或MVPD确认用户的身份时，就会发生成功的身份验证。 对于TV Everywhere用户，这会导致用户被重定向回程序员应用程序或网站。

### [!UICONTROL Cluster] {#cluster-def}

群集是位置和设备的集合。 群集是通过在设备之间查找公共位置创建的。 在公共位置看到的设备将被视为属于同一群集。 两台设备可以在同一群集中，即使它们没有公共位置，但可以通过其他设备的位置进行连接。

#### [!UICONTROL Mobile cluster] {#mobile-cluster-def}

没有静态设备的群集。

#### [!UICONTROL Static cluster] {#static-cluster-def}

至少具有一个静态设备的群集。

### [!UICONTROL Concurrency] {#consurrency-def}

并发由同时播放或时间非常接近的两个（或多个）流定义，因此它们之间的间隔不能通过以正常速度行驶来调整。
并行使用量是使用2个不同群集之间的最大速度（英里/小时）计算的。 如果用户的速度在小于124英里的距离上大于124米/小时，或者如果用户的速度在大于124英里的距离上大于400米/小时，则被视为同时使用。 计算来自不同群集的位置之间的距离。 同一群集中允许并发使用。

### [!UICONTROL Device] {#device-def}

一种能够播放上流内容的数字视频硬件产品。 例如，智能手机、笔记本电脑和台式计算机、游戏机和智能电视。

### [!UICONTROL Evaluation period] {#evaluation-period-def}

评估期是从与操作关联的操作开始到操作或其测量结束之间的时间。

### [!UICONTROL Geographical Span] {#geographical-span-def}

一组位置中最远点之间的距离。

### [!UICONTROL Granularity] {#granularity-def}

在引用时间间隔时，期间的大小；如一周或一个月。

### [!UICONTROL IP] {#ip-def}

Internet服务提供商分配给设备的Internet协议地址。 例如，有线服务提供商，以及小区服务提供商。

### [!UICONTROL Location] {#location-def}

地球上独一无二的点。 它也被称为特定播放请求的地理位置，精度为1000m x 1000m（1平方公里）。

### [!UICONTROL Media Company] {#media-company-def}

Media Company是一家拥有一组媒体网络的公司。

### [!UICONTROL Metric] {#metric}

量度是订阅者帐户的属性（例如，订阅者帐户的MVPD、程序员和他们流传输内容的渠道，以及他们使用的设备数量）。

### [!UICONTROL Mobile device] {#mobile-device-def}

具有高移动性的设备。 例如，手机和平板电脑。

### 操作 {#operation-def}

操作是为跟踪特定[操作](#action-def)对关联区段的影响而创建的记录。 例如，可以对段标识的帐户允许的并发流数量进行限制。

### [!UICONTROL Overall sharing score] {#overall-sharing-score}

一个价值，可帮助用户了解密码共享的规模，并为其提供采取相应措施的紧迫感。

### [!UICONTROL Play Request] {#play-requests-def}

等同于流开始。 此事件标记用户流内容的开始。

### [!UICONTROL Risk Index-Usage] {#risk-index-usage}

该值也称为“共享帐户的使用情况”，它根据每个帐户发出的播放请求数量并按照每个帐户的共享概率进行加权计算得出的值。 也称为“共享帐户风险指数的使用情况”。

### [!UICONTROL Segment] {#segmet-def}

区段是一组符合由选定指标指定的自定义条件的帐户。 例如，“区域A、B、C、D或E中的用户拥有三个以上的设备”。

### [!UICONTROL Sharing level] {#sharing-level-def}

风险指数，也称为帐户或共享帐户风险指数，它是根据在所选时间间隔内至少流式传输一次的当前区段中每个帐户计算的平均共享概率计算得出的值。

### [!UICONTROL Static device] {#static-device-def}

移动性非常低的设备。 例如，游戏机、机顶盒和电视机。

### [!UICONTROL Time interval] {#time-interval-def}

也称为时间段，它是包含用户界面中显示的播放请求活动和从头到尾的表的时间窗口。

### [!UICONTROL Trend] {#trend-def}

当前时段和上一时段之间关联量度的百分比差异（例如，播放请求总数的百分比）。

### [!UICONTROL Unique Subscribers] {#unique-subscriber-def}

在给定时段内至少流式处理一次的唯一帐户数。

### [!UICONTROL Usage] {#usage-defs}

#### [!UICONTROL Avid user] {#avid-user-def}

每月超过37个播放请求。

#### [!UICONTROL Infrequent user] {#infrequent-users-def}

每月少于9个播放请求。

#### [!UICONTROL Regular user] {#regular-user-def}

每月9到37个播放请求。

### [!UICONTROL Usage Pattern] {#usage-patern-def}

应用于帐户的一组有限的类别标签之一，最能体现帐户用户在社会群体或行为方面的特征（例如，小家庭、旅行者或通勤者、社交分享等）。

### [!UICONTROL Video category] {#video-category-def}

#### [!UICONTROL For D2C service] {#d2c-service}

视频类别是限定视频性质的特定标签。 例如，源的区域、内容类型（如VOD或直播）、事件或特定标签（如团队）。

#### [!UICONTROL For TV Everywhere] {#tv-everywhere}

视频类别由程序员、频道以及与流关联的MVPD的组合定义。

### [!UICONTROL Zip Code] {#zip-code-def}

与位于美国境内的位置关联的美国邮政编码
<!--calculated metrics-->


## TV Everywhere特定术语

### [!UICONTROL AuthZ] {#authz-def}

授权，或授权请求的次数。 授权请求是程序员通过Adobe从MVPD请求权限以开始流式传输用户请求内容的过程。 MVPD通常基于与用户的MVPD订阅相关联的内容权限来授予请求（例如，与内容相关联的频道是否在用户的订阅中）。 有些授权请求响应是通过Adobe缓存的，这允许Adobe立即响应，而无需将请求传递到MVPD。

### [!UICONTROL AuthZ OK] {#authz-ok-def}

成功的授权数。

### [!UICONTROL Channel] {#channel-def}

渠道（也称为“属性”）是与主题相关的视频内容源。 传统上，表示来自MVPD的不同、数字可寻址连续视频馈送。 该渠道直接映射到可供订阅者通过其机顶盒(STB)访问的内容渠道。

### [!UICONTROL Industry Average Index] {#industry-avg-index-def}

在所选时间间隔内为所有程序员和MVPD的每个风险指数（帐户、使用情况、总体）计算的值。

### [!UICONTROL Isolation Mode] {#isolation-mode-def}

一种共享分析类型，在此类分析中，对帐户的评估仅限于在选定区段中直接发生在程序员身上的事件。  通常，会评估所有帐户事件，从而提供更加准确的共享估计。  某些MVPD数据的结构仅允许隔离模式分析。

### [!UICONTROL MVPD] {#mvpd-def}

MVPD （也称为分销商）是媒体公司视频内容的汇总、转销商和分销商。

### [!UICONTROL Programmer] {#programmer-def}

Programmer，也称为Network，是拥有和管理一个或多个渠道的大公司（公司）的子公司。

### [!UICONTROL requestorID] {#requestorid-def}

媒体公司用于标识自己或MVPD子公司的ID。  根据程序员实施，这可以映射到媒体公司、程序员或渠道。 传统上，这会映射到渠道。  随着MML（三月疯狂Live）等伪渠道的创建以及技术驱动的行动旨在解决MVPD驱动的数据限制，请求者ID开始与媒体公司建立更多关联。

### [!UICONTROL resourceID] {#resource-id-def}

最终用户请求的内容。 传统上，这已标识与用户请求的内容关联的渠道。  系统增强功能允许该ID代表特定节目（例如具有特定评级），该ID继续标识相关联频道。


