---
title: 数据保留策略
description: 数据保留策略
exl-id: aa7d2d5e-9a8b-404b-874c-9e5923417784
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '298'
ht-degree: 1%

---

# 数据保留策略 {#data-retention-policy}

>[!WARNING]
>
>**注意：**&#x200B;此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。


## 简介 {#introduction}

Adobe作为您的数据处理者，必须采取适当措施协助其客户实现个人访问、删除和其他请求。 应用适当、安全和及时的删除策略是履行此义务的重要部分。

## 定义 {#definitions}

数据保留策略确定Adobe存储客户数据的时间。 并发监视的默认数据保留策略为&#x200B;**25个月**。

| 数据保留期 | 数据保留期是默认的数据保留期（25个月）。 |
|---|---|
| **数据保留窗口** | 数据保留窗口定义可以查看和报告完整数据的参数。 数据保留窗口按以下方式确定：<br/> *开始日期* =当前日期 — 数据保留期&#x200B;<br/>*结束日期* =当前日期 |

## 数据收集 {#data-collection}

*点击流数据*&#x200B;表示客户在会话心率上共享的数据（例如，subjectID、mvpdName和元数据）。 所有自定义元数据字段在[标准元数据属性](/help/concurrency-monitoring/technical/standard-metadata-attributes.md)中引用。

## 客户类型 {#customer-types}

### 当前客户 {#current-customers}

除非客户购买数据保留扩展，否则并发监视将符合以下客户数据保留要求：

* 由并发监视收集的&#x200B;*点击流数据*&#x200B;必须从收集日期起不晚于&#x200B;**25个月**&#x200B;删除。

### 已终止的客户 {#terminated-customers}

已终止的客户是已结束与Adobe的关系且不再使用并发监控的客户。

* 并发监控收集的&#x200B;*点击流数据*&#x200B;必须在客户合同终止日期的&#x200B;**6个月**&#x200B;内删除。

## 数据删除 {#data-deletion}

Adobe保留删除超出数据保留期日期的数据的权利，且不提供恢复选项。 应每月滚动删除当前客户的数据。
