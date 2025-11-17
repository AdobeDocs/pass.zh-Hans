---
title: ESM仪表板
description: 了解如何使用ESM Dashboard监控MVPD合作伙伴的权利和事件数据。
source-git-commit: 53ebbd82fc160f68fccdddb18cf98e249ad6ecce
workflow-type: tm+mt
source-wordcount: '472'
ht-degree: 1%

---


# ESM仪表板 {#esm-dashboard}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

ESM Dashboard提供了权限和事件数据的统一视图，可帮助您跨MVPD合作伙伴监控性能、识别异常并了解用户访问模式。 本指南介绍如何使用功能板的过滤器、解释报表，并了解可配置时间间隔内的关键量度。

![ESM仪表板视图](../assets/tve-dashboard/new-tve-dashboard/esm/esm-full-page.png)

## 用例 {#use-cases}

- 可视化每个平台或MVPD的趋势
- 比较MVPD性能
- 了解每个应用程序的客户使用情况

有关ESM数据和事件的更多详细信息可在[授权服务监视概述](https://experienceleague.adobe.com/en/docs/pass/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview)中找到。

## 报告 {#reports}

可以使用以下报告：

### 播放请求 {#play-requests}

显示所选时间间隔内的播放请求数。

**图形样式** — 折线。

### 身份验证转换 {#authentication-conversion}

显示成功的身份验证事件与身份验证尝试总次数之间的比率。

**图形样式** — 水平条形图

### 授权转换 {#authorization-conversion}

显示成功的身份验证事件与身份验证尝试总次数之间的比率。

**图形样式** — 水平条形图

### 授权延迟 {#authorization-latency}

显示MVPD响应的平均延迟（以毫秒为单位）。

**图形样式** — 折线。

### MVPD使用情况 {#mvpd-usage}

显示每个MVPD独特用户数的比较。

**图形样式** — 栈叠的区域。

### 成功的身份验证 {#successful-authentications}

显示所选时间间隔内的成功验证总数。

**图形样式** — 折线。

### 成功的授权 {#successful-authorizations}

显示所选时间间隔内的成功授权(MVPD的“允许”响应)总数。

**图形样式** — 折线。

## 操作 {#actions}

### 查看方式 {#view-by}

对于每个图表，都有一个“查看依据”下拉列表，您可以从中选择要显示的确切数据。

- **聚合** — 显示整体数据
- **MVPD/渠道/平台** — 概述了所选的特定过滤器

### 下载 {#download}

您可以下载原始数据：

- **以CSV格式下载图表数据** — 下载特定图表的数据
- **以CSV格式下载所有数据** — 下载所有图形中的所有ESM数据

## 过滤器 {#filters}

使用过滤器缩小数据集并集中分析。 可以使用以下过滤器：

- **渠道**：包括所有可用渠道（品牌）
- **MVPD**：关注一个或多个提供商
- **平台**： Web、移动设备、连接的电视或设备系列

要添加新筛选器，请选择“添加筛选器”按钮。

在“数据集筛选器”页面中，您可以拖放所需的筛选器。

![ESM仪表板筛选器](../assets/tve-dashboard/new-tve-dashboard/esm/filters-modal.png)

对于每个部分，您可以单独删除过滤器或清除整个选择。

### 过滤器提示 {#filter-tips}

- 合并多个过滤器以隔离同类群组(例如，移动平台上的一个用于渠道的MVPD)。
- 切入之前，请勿添加筛选器以缩小和建立基线。

## 时间间隔 {#time-intervals}

控制分析窗口和粒度。

![ESM仪表板时间间隔](../assets/tve-dashboard/new-tve-dashboard/esm/date-picker.png)

### 日期范围 {#date-range}

**预设**：今天、本周、过去7天、本月、过去30天、过去3个月、过去6个月、过去12个月

**自定义**：选择所需的时间间隔

### 粒度 {#granularity}

每日/每月
