---
title: 自定义元数据
description: 自定义元数据
exl-id: 0cfd1158-8c6c-47c2-b838-5490ff4bf0ce
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '367'
ht-degree: 0%

---

# 自定义元数据 {#cm}

>[!NOTE]
>
> 此页面已弃用，因为它适用于不再建议用于新集成的API早期版本

该服务允许客户端在查询和决策中同时使用标准和自定义字段。 标准字段始终可用于任何流（因为创建流或服务器生成流需要标准字段）：

* 应用程序
* mvpd
* accountId
* streamId
* streamstart
* 发起者


自定义字段是在流初始化时作为表单或查询字符串参数传入的任何键/值对。 之后，可以在以下场景中使用标准和自定义字段（有关详细信息，请参阅下面有关API资源的实际文档）：

* 在获取帐户的流列表（/streams资源）时请求额外的字段（通过字段查询字符串参数）
* 通过指定要作为分组依据的维度（ /activity资源）来划分帐户活动
* 根据字段值或基数定义服务器端策略（本示例使用伪SQL来明确说明）：
* 将策略配置为仅适用于特定字段值(例如，专用的iOS策略：其中osType为“iOS”)
* 限制给定字段的非重复值的数量(例如，不超过X个非重复设备：HAVING DISTINCT COUNT(deviceId) >= 2)
* 限制每个字段值的活动流数量(例如，对于单个设备类型，不超过X个活动流：GROUP BY deviceType HAVING COUNT(streamId) >= 3)


根据发送的这些键/值，可以建立不同的规则。 大致可以是：

1. 客户决定要发送参数组，该参数组的值将为“SPORTS”和“KIDS”。
1. 然后，应用程序需要执行此操作：
   * 对于体育频道，在流初始化时，应用程序将发送&#x200B;***type=SPORTS***&#x200B;作为查询参数
   * 对于具有子级相关内容的渠道，在流初始化时，应用程序将发送&#x200B;***type=KIDS***&#x200B;作为查询参数
1. 然后可以定义这样的策略：
   * `GROUP by type HAVING COUNT(streamID) < 4) IF type=KIDS`
   * `GROUP by type HAVING COUNT(streamID) < 2) IF type=SPORTS`
1. 这基本上意味着当用户观看体育时，他/她无法在超过1台设备上执行此操作，但是，当用户观看儿童内容时，最多允许在3台设备上查看。
