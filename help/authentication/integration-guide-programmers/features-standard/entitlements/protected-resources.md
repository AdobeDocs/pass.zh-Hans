---
title: 受保护的资源
description: 受保护的资源
source-git-commit: dbca6c630fcbfcc5b50ccb34f6193a35888490a3
workflow-type: tm+mt
source-wordcount: '317'
ht-degree: 0%

---

# 受保护的资源 {#protected-resources}

>[!IMPORTANT]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

受保护的资源表示用户可以流式传输的内容，并通过通过MVPD与参与程序员之间的协议建立的唯一值来标识。

受保护的资源遵循分层树结构，每个级别都为内容授权提供了更大的粒度：

* 网络
   * 渠道
      * 显示
         * 集
            * 资产

## 受保护的资源标识符 {#identifiers}

资源唯一标识符可以有两种格式：

* 简单的字符串格式，如渠道（品牌）的唯一标识符。
* 一种媒体RSS (MRSS)格式，包含标题、评级和家长控制元数据等附加信息。

在简单资源标识符的情况下，例如“REF30”（假定表示信道），可以将其转换为RSS资源标识符，如下所示：

```RSS
    <rss version="2.0"> 
        <channel>
            <title>REF30</title>
        </channel>
    </rss>
```

在更复杂的资源标识符的情况下，RSS资源标识符可以包括以下附加的评级信息：

```RSS
    <rss version="2.0" xmlns:media="http://search.yahoo.com/mrss/"> 
        <channel>
            <title>REF30</title>
            <media:rating scheme="urn:mpaa">pg</media:rating>
        </channel>
    </rss>
```

## REST API V2 {#rest-api-v2}

检索预授权和授权决策的Adobe Pass身份验证API要求客户端应用程序将受保护资源的标识符作为参数包含在内。

可以使用以下API检索预授权和授权决策：

* [使用特定的mvpd检索预授权决策](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
* [使用特定的mvpd检索授权决策](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

请参阅上述API的&#x200B;**请求**&#x200B;和&#x200B;**示例**&#x200B;部分，了解提供受保护资源的唯一标识符所需的格式。

唯一标识符对Adobe Pass身份验证是不透明的，因为它们直接传递到MVPD。 如果MVPD无法识别或分析资源标识符，则会向Adobe Pass身份验证返回错误，然后使用[增强型错误代码](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)将错误转发回客户端应用程序。

有关如何以及何时集成上述API的更多详细信息，请参阅以下文档：

* [在主应用程序中执行的基本预授权流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)
* [在主应用程序中执行的基本授权流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)
