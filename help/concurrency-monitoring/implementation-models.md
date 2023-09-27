---
title: 实施模型
description: 实施模型
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '62'
ht-degree: 0%

---


# 实施模型 {#imp-models}

## 服务器端策略 {#ss-policies}

该模型利用CM作为策略决策点，将访问决策委托给服务。

由于客户端不应就所应用的策略做出任何假设，因此实施需要在心跳响应回放期间定期检查会话初始化决策。
