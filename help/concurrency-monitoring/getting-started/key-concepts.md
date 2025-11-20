---
title: 重要概念
description: 了解并发监控的基本概念，包括会话、策略、元数据等
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '697'
ht-degree: 0%

---


# 重要概念 {#key-concepts}

了解并发监控的核心概念对于成功实施至关重要。 本指南介绍构建基块以及它们如何协同工作。

## 核心概念 {#core-concepts}

### 会话

**会话**&#x200B;表示活动视频流实例。 可将其视为允许用户观看内容的“票证”。

#### 会话生命周期

1. **创建** — 用户开始观看内容时
2. **活动** — 用户正在观看时（由心率维护）
3. **终止** — 用户停止观看或会话过期时

#### 会话属性

```json
{
  "sessionId": "unique-session-identifier",
  "idp": "identity-provider",
  "subject": "user-identifier",
  "startTime": "2024-01-15T10:30:00Z",
  "expiresAt": "2024-01-15T10:40:00Z",
  "metadata": {
    "deviceId": "device-123",
    "channel": "Channel1",
    "contentType": "live"
  }
}
```

### 支持

**策略**&#x200B;是定义并发流限制的业务规则。 它们决定用户何时可以启动新流。

#### 策略组件

- **规则** — 特定限制和条件
- **元数据要求** — 需要哪些数据
- **评估逻辑** — 如何应用策略

#### 示例策略

```json
{
  "name": "basic_stream_limit",
  "description": "Maximum 3 concurrent streams per user",
  "rules": [
    {
      "type": "maxstreams",
      "limit": 3,
      "message": "You have reached your maximum of 3 concurrent streams"
    }
  ]
}
```

### 元数据

**元数据**&#x200B;提供有关每个会话的上下文。 其中包括有关设备、内容、用户和应用程序的信息。

#### 元数据类别

| 类别 | 示例 | 用途 |
|-----------------|------------------------------------------|---------------------------------|
| **设备** | `deviceId`，`deviceType`，`osName` | 识别设备并对其进行分类 |
| **内容** | `channel`，`contentType`，`assetId` | 描述正在观看的内容 |
| **用户** | `subject`，`subscriptionTier` | 用户特定信息 |
| **应用程序** | `applicationName`，`applicationPlatform` | 特定于应用程序的详细信息 |
| **位置** | `country`，`hba` | 地理和网络信息 |

#### 必需与可选元数据

- **必需的元数据** — 必须提供用于创建会话
- **可选元数据** — 可以为增强策略提供
- **标准元数据** — 所有应用程序均可用的预定义字段
- **自定义元数据** — 您定义的特定于应用程序的字段

## 身份和身份验证 {#identity-and-authentication}

### 身份提供程序(IdP)

**身份提供程序**&#x200B;是对用户进行身份验证并提供其身份信息的服务。 在Adobe Pass集成中，这通常是MVPD。

#### IdP示例

- 有线电视公司（Comcast、Charter等）
- 卫星供应商(DirecTV， Dish)
- 流媒体服务(Netflix， Hulu)
- 移动运营商(Verizon、AT&amp;T)

### 主题

**subject**&#x200B;是身份提供程序中用户的唯一标识符。 这通常是用户的帐户ID或订阅者ID。

## 会话管理 {#session-management}

### 心率

**心率**&#x200B;是使会话保持活动状态的定期API调用。 他们告诉系统“此用户仍在观看”。

#### 心率要求

- **频率** — 每60秒（推荐）
- **目的** — 保持会话的活动状态，更新元数据
- **终止** — 心跳停止时会话过期


### 会话终止

可以通过多种方式终止会话：

1. **用户停止观看** — 应用程序调用DELETE终结点
2. **会话过期** — 在超时内未收到任何心率
3. **策略冲突** — 新会话终止旧会话(FIFO)
4. **远程终止** — 另一设备终止了会话

## 策略评估 {#policy-evaluation}

### 策略的工作原理

1. **用户请求新流**
2. **系统根据当前使用情况评估策略**
3. **决策已做出** — 允许或拒绝
4. **已发送响应** — 成功或冲突信息

## 冲突解决 {#conflict-resolution}

### 什么是冲突？

当用户尝试启动新流但超出其限制时，会发生&#x200B;**冲突**。

### 冲突响应

当发生冲突时，系统返回包含详细信息的409冲突响应：

```json
{
  "status": "error",
  "error": {
    "code": "POLICY_VIOLATION",
    "message": "Concurrent usage limit exceeded"
  },
  "evaluationResult": {
    "decision": "DENY",
    "associatedAdvice": [
      {
        "adviceType": "rule-violation",
        "attributes": {
          "rule": "max_streams",
          "threshold": 3,
          "current": 4,
          "conflicts": [
            {
              "sessionId": "session-123",
              "terminationCode": "term-456",
              "metadata": {...}
            }
          ]
        }
      }
    ]
  }
}
```

### 解决策略

#### 后进先出

- **旧会话受保护**
- **新会话被阻止**
- **更简单的UI**
- **更适合扩展查看**


#### 先进先出

- **新会话终止现有会话**
- **用户选择要停止的会话**
- **需要更复杂的UI**
- **更适合内容切换**

## 应用程序和租户 {#applications-and-tenants}

### 应用程序

**应用程序**&#x200B;是与并发监视集成的软件程序。 每个应用程序都有一个唯一的ID，并且可以拥有自己的策略。

#### 应用程序示例

- 移动应用程序(iOS/Android)
- Web应用程序
- 智能电视应用程序
- 机顶盒应用程序

### 租户

**租户**&#x200B;是拥有一个或多个应用程序的组织。 租户可以跨应用程序共享策略。

#### 租户结构

```
Tenant: "Streaming Company"
├── Application: "Mobile App"
├── Application: "Web App"
└── Application: "TV App"
```

## 环境 {#environments}

### 暂存环境

- **目的** — 开发和测试
- **URL** - `https://streams-stage.adobeprimetime.com/v2/`
- **凭据** — 测试应用程序标识
- **数据** — 仅测试数据

### 生产环境

- **用途** — 实时用户流量
- **URL** - `https://streams.adobeprimetime.com/v2/`
- **凭据** — 生产应用程序标识
- **数据** — 真实用户数据

## 集成流 {#integration-flow}

### 基本流量

1. **用户身份验证** — 使用Adobe Pass进行身份验证
2. **会话创建** — 使用用户元数据创建CM会话
3. **心跳管理** — 发送定期心跳
4. **会话终止** — 用户停止观看时终止

### 错误处理

1. **404未找到** — 处理会话ID不是CM服务生成的请求
2. **409冲突** — 处理策略违规
3. **410不存在** — 处理会话终止
4. **网络错误** — 处理连接问题
5. **身份验证错误** — 处理凭据问题


## 常用术语 {#common-terminology}

| 术语 | 定义 |
|-----------------|--------------------------------------------|
| **会话** | 活动视频流实例 |
| **策略** | 定义使用限制的业务规则 |
| **元数据** | 有关会话的上下文信息 |
| **IDP** | 身份提供者（身份验证服务） |
| **主题** | IDP中的用户标识符 |
| **心率** | 定期调用以保持会话活动 |
| **冲突** | 启动新流时违反策略 |
| **后进先出** | 先进先出冲突解决 |
| **先进先出** | 先进先出冲突解决 |
| **租户** | 拥有应用程序的组织 |
| **应用程序** | 使用CM的软件程序 |

