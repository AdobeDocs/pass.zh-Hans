---
title: 如何在并发监控中区分VOD和实时内容
description: 如何在并发监控中区分VOD和实时内容
exl-id: 51ba686a-7c1f-4403-9e8e-cd247bf9e345
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 0%

---

# 方法：在并发监控中区分VOD和实时内容 {#dist-vod-live}

**问：**&#x200B;并发监控服务能否区分正在播放的内容类型（实时内容与点播视频）？



**A：**&#x200B;并发监视无法直接区分实时内容与点播视频(VOD)。 视频播放器必须知道正在播放的内容类型，并在[会话初始化调用](/help/concurrency-monitoring/cm-api-overview.md#session-initial)期间发送此信息（并发监视所需）。 常规工作流如下所示：

1. 并发监控客户定义了一组他们希望对其实施规则的元数据（例如content-type=live|vod、device-type=mobile|console|desktop）。
1. 并发监控团队会实施所需的策略。 示例：
   1. 如果content-type=live，max streams=3，则为最新胜利
   1. 如果content-type=vod，max streams=1，则为最新入选次数

1. 当最终用户播放内容时，视频播放器必须发送作为策略的一部分建立的这些元数据字段的值。

1. 并发监控服务将根据定义的策略和收到的值，发出决策（播放/停止）。

1. 视频播放器必须遵循决策才能使系统正常工作。



## 相关信息 {#related-info-vod-live-dist}

* [并发监控标准元数据属性](/help/concurrency-monitoring/standard-metadata-attributes.md)
* [并发监控API概述](/help/concurrency-monitoring/cm-api-overview.md)
