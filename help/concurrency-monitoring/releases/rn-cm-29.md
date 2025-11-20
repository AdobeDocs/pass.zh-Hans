---
title: Adobe并发监控2.9发行说明
description: Adobe并发监控2.9发行说明
exl-id: fd793b1f-b704-492b-850c-dae6478b575a
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 1%

---

# 并发监控2.9发行说明 {#rn-cm29}

本页介绍了此版本的新增功能、更改和已知问题。

## 发行日期 {#release-date}

03/14/2019


## 发行版概述 {#release-overview}

* 从此版本开始，我们引入了一个用于了解并发使用的新报表：并发级别的直方图，其中显示：

* 在每个粒度间隔内达到每个并发级别的用户数（即有多少用户曾拥有2个并发流、3个并发流等）
* 每个并发级别的总持续时间（以分钟为单位，平均值可以通过简单地将该值除以上述计数来计算）
* 用户遇到每个并发级别的总次数，用于根据受影响的用户和汇总的用户体验来估计给定规则的影响
有关更多详细信息，请参阅[使用情况报表](/help/concurrency-monitoring/reports/cm-usage-reports.md)页面。

我们还改进了SQL注入保护并添加了多个错误修复。

## 已知问题 {#known-issues}

无。
