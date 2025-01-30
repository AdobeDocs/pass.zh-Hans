---
title: 决策
description: 决策
source-git-commit: e448427ae4a36c4c6cb9f9c1cb4d0cc5c6d564ed
workflow-type: tm+mt
source-wordcount: '886'
ht-degree: 0%

---

# 决策 {#decisions}

>[!IMPORTANT]
>
> 此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

决策由Adobe Pass身份验证[REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)根据用户的MVPD授权或预授权查询生成，用于确定是否授予或拒绝对[受保护内容](#protected-resources)的访问。

根据调用的API，提供两种类型的决策：

* [预授权决策](#preauthorization-decisions)是信息性决策。
* [授权决定](#authorization-decisions)是授权决定。

## 预授权决策 {#preauthorization-decisions}

预授权决定是一个信息性决定，它允许通知客户端应用程序MVPD是否允许或拒绝用户访问[保护的资源](#protected-resources)。

预授权（预检授权）的目的是使应用程序能够显示有关用户可能有资格查看的内容的准确信息。 这是通过使用指示符（例如锁定或未锁定图标）增强用户界面来反映访问状态来实现的。

>[!IMPORTANT]
>
> 不得以权威方式使用预授权决定来播放资源，因为这是[授权决定](#authorization-decisions)的目的。

预授权API的使用不是强制性的，如果客户端应用程序希望在不进行任何过滤的情况下呈现资源目录，则可以跳过此行为。

如果客户端应用程序希望使用此功能，请务必注意，每个API请求只能为有限数量的资源（通常最多5个）获取预授权决策。

>[!IMPORTANT]
> 
> 只有在与MVPD和Adobe Pass身份验证代表达成协议后，才能增加资源的最大数量。 一旦达成一致，您组织中的管理员或代表您行事的Adobe Pass身份验证代表可以通过Adobe Pass TVE仪表板实施更改。
> 
> 有关更多详细信息，请参阅[TVE仪表板集成用户指南](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#add-more-properties)文档。

MVPD可能支持通过各种机制进行预授权，每种机制对性能以及单个API请求可以处理的最大资源数都有不同的影响。

有关支持预授权的现有机制的更多详细信息，请参阅[MVPD预检授权](/help/authentication/integration-guide-mvpds/mvpd-preflight-authz.md)文档。

>[!IMPORTANT]
>
> 对于没有完全预检授权支持的MVPD，必须事先与MVPD和Adobe Pass身份验证代表商定使用预授权，因为这可能会导致性能问题和响应速度变慢。

## 授权决策 {#authorization-decisions}

授权决定是一个授权决定，它允许客户端应用程序符合MVPD允许或拒绝用户访问[保护的资源](#protected-resources)的决定。

授权的目的是使应用程序能够在与MVPD进行权限验证并从Adobe Pass身份验证接收媒体令牌之后播放用户请求的资源。

>[!IMPORTANT]
> 
> Adobe Pass身份验证建议程序员使用媒体令牌验证器库来验证授权决策中包含的媒体令牌，确保在启动视频流之前安全访问。
> 
> 有关更多详细信息，请参阅[媒体令牌](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md)文档。

授权API是强制性的，如果客户端应用程序要播放用户请求的资源，则无法跳过此阶段，因为它需要在释放流之前与MVPD验证用户是否有权使用授权API。

需要注意的是，每个API请求只能为有限数量的资源（通常为1）获取授权决策。

>[!IMPORTANT]
>
> 只有在与MVPD和Adobe Pass身份验证代表达成协议后，才能增加资源的最大数量。

## 受保护的资源 {#protected-resources}

受保护的资源是指可流化的内容，由MVPD与参与程序员之间协议定义的唯一值标识。

受保护的资源遵循分层树结构，每个级别都为内容授权提供了更大的粒度：

* 网络
   * 渠道
      * 显示
         * 集
            * 资产

>[!IMPORTANT]
>
> 预授权（预检授权）侧重于标识符具有简单字符串或MRSS格式的通道级资源。
> 
> 我们不建议在预授权的情况下使用标识符包含`CDATA`部分的资源，因为它们主要用于MRSS定义的资产级资源。

### 资源标识符 {#resource-identifier}

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

唯一标识符对于Adobe Pass身份验证来说主要是不透明的，但是，可以根据MVPD的功能和要求应用转换器。 如果MVPD无法识别或分析资源标识符，则会向Adobe Pass身份验证返回错误，该身份验证随后使用[增强型错误代码](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)将错误转发到客户端应用程序。

## REST API V2 {#rest-api-v2}

可以使用以下API检索预授权决策：

* [使用特定的mvpd检索预授权决策](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)

可以使用以下API检索授权决策：

* [使用特定的mvpd检索授权决策](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

请参阅上述API的&#x200B;**响应**&#x200B;和&#x200B;**示例**&#x200B;部分，了解预授权和授权决策的结构。

有关如何以及何时集成上述API的更多详细信息，请参阅以下文档：

* [在主应用程序中执行的基本预授权流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)
* [在主应用程序中执行的基本授权流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)
