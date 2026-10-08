---
title: 实施模型
description: 实施模型
exl-id: 3bcb63ba-9b4a-4df4-8d24-e520b8830a10
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '63'
ht-degree: 0%
---
# 实施模型 {#imp-models}

## 服务器端策略 {#ss-policies}

该模型利用CM作为策略决策点，将访问决策委托给服务。

由于客户端不应就所应用的策略做出任何假设，因此实施需要在心跳响应回放期间定期检查会话初始化决策。
