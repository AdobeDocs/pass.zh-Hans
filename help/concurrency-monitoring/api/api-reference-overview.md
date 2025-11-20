---
title: API参考概述
description: 并发监控API的完整参考，包括端点、身份验证和响应格式
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '438'
ht-degree: 2%

---


# API参考概述 {#api-reference-overview}

并发监控API提供了一个RESTful接口，用于管理流会话和强制实施并发使用策略。 此参考提供了有关所有端点、身份验证方法、请求/响应格式和错误处理的完整文档。

## API基本URL

### 生产环境

```
https://streams.adobeprimetime.com/v2/
```

### 暂存环境

```
https://streams-stage.adobeprimetime.com/v2/
```

**注意：**&#x200B;始终使用暂存环境进行开发和测试。 生产凭据仅在成功暂存集成后提供。

## 身份验证

所有API调用都需要使用应用程序凭据进行HTTP基本身份验证：

- **用户名：**&#x200B;您的应用程序ID(由Adobe提供)
- **密码：**&#x200B;空字符串

### 身份验证标头示例

```bash
curl -u "<your-app-id>:" https://streams-stage.adobeprimetime.com/v2/sessions

For an application with id "demo-app" the authentication header would be exactly as shown below, including the quotes and colon:
curl -u "demo-app:" https://streams-stage.adobeprimetime.com/v2/sessions
```

## 响应格式标准

### 成功响应

所有成功的响应都遵循此结构：

```json
{
  "status": "success",
  "data": {
    // Response-specific data
  },
  "timestamp": "2024-01-15T10:30:00Z"
}
```

### 错误响应

所有错误响应都遵循此结构：

```json
{
  "associatedAdvice": [
    {
      "policyName": "string",
      "ruleName": "string",
      "scope": {},
      "attribute": "string",
      "threshold": 0,
      "conflicts": [
        {}
      ]
    }
  ],
  "obligations": [
    {
      "namespace": "string",
      "action": "string",
      "arguments": [
        "string"
      ]
    }
  ]
}
```

### 评估结果格式

在评估政策（特别是针对409个冲突）时，响应包括评估结果：

```json
{
  "evaluationResult": {
    "decision": "DENY",
    "obligations": [
      {
        "id": "obligation-id",
        "fulfillOn": "DENY",
        "attributes": {
          "attribute1": "value1"
        }
      }
    ],
    "associatedAdvice": [
      {
        "id": "advice-id",
        "adviceType": "rule-violation",
        "attributes": {
          "rule": "rule-name",
          "threshold": 3,
          "current": 4,
          "conflicts": [
            {
              "sessionId": "session-123",
              "terminationCode": "term-456",
              "metadata": {
                "deviceId": "device-789",
                "channel": "Channel1"
              }
            }
          ]
        }
      }
    ]
  }
}
```

## 常见HTTP状态代码

| 代码 | 描述 | 返回时 |
|------|----------------------|------------------------------------------------|
| 200 | 确定 | 成功的GET请求 |
| 202 | 创建/接受 | 会话创建/心率记录成功 |
| 400 | 错误请求 | 参数无效或缺少必填字段 |
| 401 | 未授权 | 身份验证无效或缺少身份验证 |
| 403 | 禁止 | 权限不足 |
| 404 | 未找到会话ID | CM服务未生成会话ID |
| 409 | 冲突 | 策略违规（已达到并发限制） |
| 410 | 不存在 | 会话已过期或已终止 |
| 429 | 请求过多 | 超出速率限制 |

## 参数传递方法

### 路径参数

作为URL路径一部分的必需参数：

1. `{idp}` — 身份提供程序标识符
2. `{subject}` — 用户标识符(通常来自Adobe Pass)
3. `{sessionId}` — 会话标识符（在Location标头中返回）

### 其他参数

可选参数在URL中传递：

```bash
GET /sessions/{idp}/{subject}?platform=test
```

### 表单数据(POST/PUT)

请求正文中的元数据和会话数据：

```bash
POST /sessions/{idp}/{subject}
Content-Type: application/x-www-form-urlencoded

channel=Channel1&deviceId=device-123&contentType=live
```

### 标头

在HTTP标头中传递的特殊参数：

```bash
X-Terminate: termination-code-123
X-Client-Version: 1.0.0
```

## 错误处理最佳实践

### 409冲突处理

当您收到409冲突响应时：

1. **分析评估结果**&#x200B;以了解策略冲突
2. **从**&#x200B;提取冲突信息`associatedAdvice`
3. **根据您的后进先出/先进先出策略向用户显示选项**
4. 如果实现LIFO行为，则&#x200B;**使用终止代码**

### 410处理完毕

当您收到“410不存在”响应时：

1. **检查响应是否包含正文** — 指示远程终止
2. **分析建议**&#x200B;以了解会话终止的原因
3. **更新UI**&#x200B;以反映会话终止
4. **正常处理** — 会话可能已自然超时
5. **启动新会话** — 如果认为适当，启动新会话

### 速率限制

当您收到429的请求过多时：

1. **将呼叫频率**&#x200B;限制为每分钟最多200个请求，这是CM接受的最大级别
2. **按所需的间隔**&#x200B;发送心率，即每分钟发送一次。

## 测试工具

### 交互式API资源管理器

使用我们的[Swagger UI](https://streams-stage.adobeprimetime.com/swagger-ui/index.html)进行交互式测试：

1. 在右上角输入您的应用程序ID
2. 单击“浏览”以设置身份验证
3. 使用实际参数测试端点
4. 查看请求/响应示例
