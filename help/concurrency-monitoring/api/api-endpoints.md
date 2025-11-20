---
title: API端点
description: 并发监控API的完整列表
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
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
