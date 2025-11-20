---
title: Adobe Pass并发监控2.3.2发行说明
description: Adobe Pass并发监控2.3.2发行说明
exl-id: 3996da45-498c-482a-b374-3cda1c5df2f7
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 1%

---

# Adobe Pass并发监控2.3.2发行说明 {#cm-232}

本页介绍了此版本的新增功能、更改和已知问题：

发行日期： 2015年12月11日

## 新增功能和改进功能 {#new-features}

* 使用情况报表中提供了新的细分。 如果与并发监控集成的应用程序发送自定义元数据，则新的划分将可用。
   * 应用程序 — 调用URL中报告的应用程序ID
   * mvpd — 在调用URL中报告的MVPD
   * 渠道 — 自定义元数据渠道
   * platform — 自定义元数据应用程序平台
* 使用情况报告中提供了与&#x200B;**流持续时间**&#x200B;相关的新量度。 新量度可用于创建流持续时间的直方图。 以下时间间隔（以分钟为单位）当前可用：
   * duration_0-15
   * duration_15-30
   * duration_30-60
   * duration_60-120
   * duration_over-120

## 错误修复 {#bug-fixes}

不适用

## 已知问题 {#known-issues}

不适用
