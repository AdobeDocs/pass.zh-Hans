---
title: Adobe Pass Concurrency Monitoring 2.5.0发行说明
description: Adobe Pass Concurrency Monitoring 2.5.0发行说明
exl-id: da392b18-a2aa-4f51-a75f-2c5b65b2b073
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '286'
ht-degree: 0%

---

# Adobe Pass Concurrency Monitoring 2.5.0发行说明 {#cm-250}

本页介绍了此版本的新增功能、更改和已知问题：

发行日期： 2016年4月21日

## 新增功能 {#new-features}

Concurrency Monitoring API v2.0现已正式推出。

V2版本统一了心率和查询调用，并在同时使用这两个API时大大简化了实施。



### 会话生命周期 {#session-lifecycle}

* 用于创建、保持活动状态并终止会话的纯写API — 即，不再有查询，在会话初始和心跳调用时都会返回决策
* 心跳间隔现在由服务器通过会话初始调用和保持活动调用中设置的Expires标头驱动。 这允许在应用程序中配置心跳间隔和一个较少硬编码的值。
* 快乐路径（用户和应用程序的合规行为）通过2XX状态代码“铺开”

### 错误处理 {#error-handling}

* 拒绝决策、缺少元数据或应用程序不当行为将标记为4XX响应（冲突、错误请求、未授权、消失等）

* 只要有意义，响应将包括：

   * 相关建议 — 失败的详细说明，提示用户。

   * 义务 — 应用程序必须执行的强制性操作(例如：刷新元数据、从Adobe Pass注销)。

### 元数据 {#metadata}

* 标准而不是自定义元数据 — 这允许API识别是否发送了错误的元数据或缺少规则所需的元数据。
* 现在，每个应用程序的必需属性均通过API调用提供，并且应由客户端缓存。
注释

>[!NOTE]
>
>V1版本继续可用。 如果客户需要使用心率以外的查询，则可以使用V1版本。




### 错误修复 {#bug-fixes}

不适用

### 已知问题 {#known-issues}

不适用
