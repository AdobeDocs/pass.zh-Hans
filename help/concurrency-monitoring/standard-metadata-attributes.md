---
title: 标准元数据属性
description: 标准元数据属性
exl-id: 99ffa98c-213f-47a5-a6e7-fbacb77875d0
source-git-commit: ae2e61152695b738b0bb08d1dcd81417f3bbdfb5
workflow-type: tm+mt
source-wordcount: '1053'
ht-degree: 0%

---

# 标准元数据属性 {#std-metadata-attributes}

本页旨在提供并发监控服务可以处理的元数据属性的详尽列表，这些属性可用作可以实施的策略的基础。 标准元数据属性可按如下方式进行分类：

* 设计包括的属性（在每次会话初始化调用时发送，因为URL路径中需要这些属性）。 如果没有这些值，则无法执行有效的调用。
* 元数据属性：在会话初始化调用期间需要作为表单数据传递的值（如果后端策略需要其值）。

## 设计所需的属性 {#attr-req-by-design}

并发监视API强制客户端将以下值作为任何有效初始化调用的一部分发送： [会话启动调用](/help/concurrency-monitoring/restrict-concurr-usage-mult-apps.md#api-calls-descr)。

| 字段名称 | 示例值 | 在何处使用 | 获取自 |
|---------------|-----------------------------|----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| applicationId | 75b4-431b-adb2-eb6b9e546013 | 授权标头 | 集成中的Zendesk票证 |
| mvpdName | Sample_MVPD | URI路径 | 用户选择MVPD时，从配置端点进行Adobe Pass身份验证 |
| accountId | 12345 | URI路径 | 用户登录[用户Metadata upstreamUserID - Adobe Pass身份验证](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)后的Adobe Pass身份验证upstreamUserID元数据 |


## 元数据属性 {#metadata-attr}

程序员和MVPD可以使用下表中的字段创建将在并发监控中实施的策略。

使用[API v2.0](http://docs.adobeptime.io/cm-api-v2/)时，如果定义的策略需要这些属性中的任何一个，则没有该属性的会话初始化尝试将导致400错误请求。


| 实体 | 属性名称 | 数据类型 | 描述 | 外部参考（例如：EIDR、OATC） | 示例值 | 验证规则 |
|---------------|-------------------|-----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| 媒体公司 | 程序员姓名 | 字符串 | 程序员的姓名 |                                                   | 程序员X |                                                                                   |
| 资源 | 渠道 | 字符串 | 电视频道 |                                                   | ChannelY |                                                                                   |
|                 | 资产ID | 字符串 | 要为此内容显示的“友好”或消费者可读的标题 | [EIDR 2.0数据字段引用](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/EIDR_2_0_Data_Fields.pdf){target=_blank} | 本胡尔 |                                                                                   |
|                 | type | 明细列表 | 描述TveItem表示的内容的一般类型的值。 枚举值包括：电影broadcastEpisodeBroadcastEpisode音乐Video awards节目剪辑音乐会新闻活动体育活动预告片 | [OATC元数据馈送建议做法](https://userfiles-kb.s3.amazonaws.com/userfiles/258/326/ckfinder/files/OATC%20Metadata%20Feed%201_0d_1%20OATC%20BOARD%20APPROVED%20FOR%20RELEASE%20%281%29.pdf?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIMM7Q2VAGHGVAOHA%2F20230803%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20230803T144225Z&X-Amz-SignedHeaders=host&X-Amz-Expires=1200&X-Amz-Signature=e61658133a4875ff48757b1a3bafb7627054ba6fc75c134a3dea9fa8022b45fa){target=_blank} | broadcastEpisode | 该字段必须对应于枚举中的项之一 |
|                 | 内容类型 | 字符串 | 此字段确定请求的内容是实时内容还是VOD内容 | 不适用 | 实时，vod | live或vod |
|                 | 流派 | 字符串 | 流式传输内容的流派。 描述常规编程类型 | [OATC元数据馈送建议](https://userfiles-kb.s3.amazonaws.com/userfiles/258/326/ckfinder/files/OATC%20Metadata%20Feed%201_0d_1%20OATC%20BOARD%20APPROVED%20FOR%20RELEASE%20%281%29.pdf?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIMM7Q2VAGHGVAOHA%2F20230803%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20230803T144225Z&X-Amz-SignedHeaders=host&X-Amz-Expires=1200&X-Amz-Signature=e61658133a4875ff48757b1a3bafb7627054ba6fc75c134a3dea9fa8022b45fa){target=_blank}实践 | 喜剧 | 有效的流派类型 |
|                 | 持续时间 | 数字 | 媒体项目的持续时间（以秒为单位） | [OATC元数据馈送建议做法](https://userfiles-kb.s3.amazonaws.com/userfiles/258/326/ckfinder/files/OATC%20Metadata%20Feed%201_0d_1%20OATC%20BOARD%20APPROVED%20FOR%20RELEASE%20%281%29.pdf?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIMM7Q2VAGHGVAOHA%2F20230803%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20230803T144225Z&X-Amz-SignedHeaders=host&X-Amz-Expires=1200&X-Amz-Signature=e61658133a4875ff48757b1a3bafb7627054ba6fc75c134a3dea9fa8022b45fa){target=_blank} | 1800 | 编号规则 |
| 设备/浏览器 | deviceId | 字符串 | 唯一设备标识符。 | [Device Atlas属性](https://deviceatlas.com/device-data/properties){target=_blank} | 2b6f0cc904d137be2e1730235f5664094b831186 |                                                                                   |
|                 | 设备名称 | 字符串 | 此设备的友好名称。 |                                                   | 乔的iPad |                                                                                   |
|                 | 营销名称 | 字符串 | 设备的营销名称（或客户友好名称） | [Device Atlas属性](https://deviceatlas.com/device-data/properties){target=_blank} | iPhone 6s | 有效的营销名称 |
|                 | 移动设备 | 布尔型 | 如果设备用于移动使用，则为true | [Device Atlas属性](https://deviceatlas.com/device-data/properties){target=_blank} | true， false | true， false |
|                 | 设备型号 | 字符串 | 设备、浏览器或其他组件的型号名称 | [Device Atlas属性](https://deviceatlas.com/device-data/properties){target=_blank} | 平板电脑，手机，xbox。 机顶盒 | 有效设备型号名称 |
|                 | 操作系统名称 | 字符串 | 设备正在运行的操作系统 | [Device Atlas — 操作系统预定义的属性值](https://deviceatlas.com/device-data/explorer/#defined_property_values/877430/4121272){target=_blank} | Android、Windows 10、OS X、Linux、其他注意：您需要使用Device Atlas中的用户名和密码登录才能查看属性值 | 预期值是Device Atlas预定义属性中的值之一 |
|                 | browserName | 字符串 | 设备上的浏览器的名称或类型 | [Device Atlas — 浏览器预定义的属性值](https://deviceatlas.com/device-data/explorer/#defined_property_values/7/2705619){target=_blank} | 设备上浏览器的名称或类型。  注意：您需要使用Device Atlas中的用户名和密码登录才能查看属性值 | 预期值是Device Atlas预定义属性中的值之一 |
|                 | browserVersion | 字符串 | 设备上的浏览器版本 | [Device Atlas属性](https://deviceatlas.com/device-data/properties){target=_blank} | 设备上的浏览器版本 |                                                                                   |
| 应用程序 | 应用程序名称 | 字符串 | 应用程序的“用户友好”或用户可读的名称 | 不适用 | Sample_Application |                                                                                   |
|                 | applicationId | 字符串 | 唯一标识客户端应用程序的应用程序ID。 | 不适用 | de305d54-75b4-431b-adb2-eb6b9e546013 |                                                                                   |
|                 | applicationplatform | 字符串 | 应用程序的本机平台 | 不适用 | ios、android |                                                                                   |
|                 | applicationversion | 字符串 | 此值可用于分析目的 | 不适用 | 1.0、2.0 |                                                                                   |
| 主题 | accountId | 字符串 | 并发监控主体的帐户ID(在MVPD范围内) | 不适用 | test-account |                                                                                   |
|                 | contracttype | 字符串 | 高级，基本。 客户可以将其添加为自定义元数据，并在其自己的领域中使用它 | 不适用 | 高级，基本 |                                                                                   |
| 用户 | name | 字符串 | 某些MVPD提供与播放内容的特定用户相关的信息。 | 不适用 |                                                                                                                                                         |                                                                                   |
|                 | hba | 布尔型 | 标识用户是否尝试从其家乡启动流 | 不适用 | true， false | true或false |
| 位置 | 大陆 | 字符串 | 发送播放请求的deviceID所在的大陆 | 不适用 | 北美 | 有效的大陆名称 |
|                 | 国家/地区 | 字符串 | 发送播放请求的deviceID所在的国家/地区 | 不适用 | 美国 | 有效的国家/地区名称 |
|                 | state | 字符串 | 发送播放请求的设备ID源自的状态 | 不适用 | CA | 有效的状态名称 |
|                 | 城市 | 字符串 | 发送播放请求的设备ID所在的城市 | 不适用 | 库比蒂诺 | 有效的城市名称 |
|                 | 邮政编码 | 数字 | 发送播放请求的deviceID所在的邮政编码 | 不适用 | 95014 | 有效的邮政编码 |
| 流 | streamId | 字符串 | 由CM服务生成，不在客户控制中。 在定义maxstreams类型的规则时隐式使用。 | 不适用 | 不适用 | 不适用 |
|                 | streamCDN | 字符串 | 指示从中获取流的CDN | 不适用 | 不适用 | 不适用 |

## 使用元数据属性创建策略的示例 {#examples-metadata-attr}

标准元数据字段可用于根据其字段值定义服务器端策略：

* 您可以将策略配置为仅应用于特定字段值(例如，专用iOS策略：其中`osType`为`iOS`)
* 您可以限制给定字段的不同值的数量。 以下是一些示例：
   * 不多于X个不同的设备： `HAVING DISTINCT COUNT(deviceId) <= 2`
   * 不多于X个不同的邮政编码： `HAVING DISTINCT COUNT(zipcode) <= 3`
* 您可以限制每个字段值的活动流数量。 以下是一些示例：
   * 单个设备类型不超过X个活动流： `GROUP BY deviceType HAVING COUNT(streamId) <= 3`
   * 实时内容流的活动流不超过X个： `SELECT COUNT(streamId) AS streamCount WHERE contentType='live' HAVING streamCount <= 3`

通过[在Zendesk](mailto:tve-support@adobe.com)中创建票证联系并发监控团队，并指示要实施的策略。

您可以在以下页面中找到策略和集成Cookbook的更多示例：

* [策略决策点](/help/concurrency-monitoring/cm-policy-decision-point.md)
* [API控制台 — Adobe并发监视](http://docs.adobeptime.io/cm-api-v2/)
