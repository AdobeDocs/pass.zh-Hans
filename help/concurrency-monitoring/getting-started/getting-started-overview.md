---
title: 开始使用并发监视
description: 了解并发监控的基础知识以及如何开始使用集成
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 0%

---


# 开始使用并发监视 {#getting-started-overview}

欢迎使用并发监控！ 本指南将帮助您了解基础知识，并让您的集成快速启动和运行。

## 并发监控解决了问题 {#problem-solved}

在当今的流媒体环境中，用户希望跨多个设备和应用程序访问内容。 但是，这给内容提供商带来了挑战：

- **内容许可协议**&#x200B;通常会限制同步流的数量
- **带宽成本**&#x200B;会随着过多的并发使用而增加
- 当太多流降低性能时，**用户体验**&#x200B;会受损
- **收入保护**&#x200B;需要防止帐户共享滥用

并发监控为这些难题提供了集中的解决方案。


## 并发监测的工作原理

### 核心功能

并发监视作为&#x200B;**会话管理服务**&#x200B;运行，该服务：

1. **实时跟踪所有应用程序中的活动流会话**
2. **根据当前使用模式评估业务策略**
3. 违反策略时&#x200B;**强制限制**
4. 发生冲突时&#x200B;**提供解决选项**

## 不同利益相关者获得的优势

### 内容提供商（程序员）

- **保护内容权限**&#x200B;并遵守许可协议
- **通过防止过度使用来优化带宽成本**
- **改进用户体验**，对限制有明确的反馈
- **通过详细报告获得使用情况分析**

### 身份提供程序(MVPD)

- 在所有内容合作伙伴中&#x200B;**强制执行订阅者协议**
- 多个应用程序的&#x200B;**集中式策略管理**
- **实时监视**&#x200B;使用模式
- **可扩展的体系结构**&#x200B;处理数百万个会话

### 最终用户

- **清楚了解**&#x200B;其使用限制
- 所有应用程序中的&#x200B;**一致体验**
- **达到限制时解决冲突的选项**
- **关于访问受限制原因的透明反馈**


## 快速入门路径 {#quick-start-path}

1. **查看关键概念** — 了解[会话、策略和元数据](key-concepts.md)
2. **选择策略** — 审阅[LIFO与FIFO策略](../use-cases/lifo-fifo-strategies.md)
3. **开始实施** — 遵循[API引用](../api/api-reference-overview.md)

## 准备好集成了吗？ {#ready-to-integrate}

- [API参考](../api/api-reference-overview.md)
- [用例](../use-cases/cm-use-cases.md)


## 服务注册 {#service-registration}

要开始使用并发监视，请与我们的[支持团队](mailto:tve-support@adobe.com)联系并提供以下信息：

1. **公司名称**&#x200B;和联系人详细信息
2. 您要与并发监视集成的&#x200B;**应用程序**。 对于每个应用程序，请提供：
   1. 应用程序名称
   2. 应用程序平台
3. **集成合作伙伴**(如果您是应另一方、程序员或MVPD的要求订阅并发监视)


## 需要帮助？ {#need-help}

- **API资源管理器** — 在[Swagger UI](https://streams-stage.adobeprimetime.com/swagger-ui/index.html)处以交互方式测试API
- **关键术语和定义** - [词汇表](../cm-glossary.md)
- **如何获取帮助？** - [支持过程](../support/cm-escalation-procedures.md)
- **支持** — 联系[tve-support@adobe.com](mailto:tve-support@adobe.com)
