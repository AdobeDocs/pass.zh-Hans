---
title: API端点
description: 并发监控API的完整列表
exl-id: e8a9dfd2-cd16-4971-b9bc-9646987dd3ce
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '54'
ht-degree: 3%
---
# API端点

## 核心会话管理

| 端点 | 方法 | 描述 |
|---------------------------------------|--------|---------------------------------------|
| `/sessions/{idp}/{subject}` | POST | 创建新的流会话 |
| `/sessions/{idp}/{subject}/{session}` | POST | 发送心跳以保持会话活动 |
| `/sessions/{idp}/{subject}/{session}` | DELETE | 终止会话 |
| `/runningStreams/{idp}/{subject}` | GET | 获取主题的所有活动会话 |

## 元数据管理

| 端点 | 方法 | 描述 |
|-------------|--------|----------------------------------------------|
| `/metadata` | GET | 获取应用程序的必需元数据字段 |
