---
title: 促销临时通票
description: 促销临时通票
exl-id: 705c1ba9-0430-4e3b-add1-d9e4da3f82d1
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1510'
ht-degree: 0%

---

# 促销临时通票 {#promotional-temp-pass}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## 功能摘要 {#feature-summary}

促销临时密码允许程序员为没有使用MVPD的帐户凭据的用户提供对其受保护内容的临时访问权限。

促销临时通行证设计用于运行促销活动，其中，在将有效标识信息（例如电子邮件地址）提供给程序员之后，用户能够在预定义的时间段&#x200B;**内使用**&#x200B;个预定义数量的不同VOD标题。

>[!IMPORTANT]
>
>Adobe不存储任何个人身份信息(PII)。 因此，程序员必须对Adobe Pass身份验证API上唯一用户提供的信息设置哈希。

提升临时传递是基于[临时传递](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md)功能构建的，这意味着它包括所有临时传递功能。

一旦超过最大预定义VOD标题数或预定义的时段，该用户将无法访问同一设备上的内容，也无法使用相同的用户标识符信息（例如，电子邮件地址）来访问内容，直到从Adobe Pass身份验证服务器中清除授权令牌为止。

>[!NOTE]
>
>临时传递是Premium工作流包的一部分。 如果您有兴趣使用此功能，请联系您的Adobe Pass销售代表。

## 临时通过和促销临时通过比较 {#tp-ptp-comparison}

| 临时传递 | 促销临时通票 |
|----------------------------------|----------------------------------------------------------------------------------------|
| 访问内容 <ul><li>基于时间</li></ul> | 访问内容 <ul><li>基于时间</li><li>基于资源数量</li></ul> |
| 访问安全性基于 <ul><li>设备Id</li></ul> | 安全性基于 <ul><li>设备Id</li><li>散列提供的用户标识符信息（例如，电子邮件）</li></ul> |
| 客户端错误API可用 | 客户端错误API可用 |
| 重置/清除可用 | 重置/清除可用 |

## 功能详细信息 {#feature-details}

此功能使用户在程序员的应用程序中提供了唯一信息（如电子邮件地址）之后，可以从特定设备（电话和平板电脑）访问促销内容。

程序员将在身份验证和授权API上提供用户PII上的哈希。 此哈希将与设备ID一起用于生成唯一密钥以标识用户和设备。

Adobe Pass身份验证将根据设备ID和用户提供的信息并按照以下逻辑确定用户是处于新试用还是现有试用中：

* 新哈希覆盖用户提供的信息（例如，电子邮件），新设备ID =>新试用版
* 现有散列覆盖用户提供的信息（例如，电子邮件），新设备ID =>现有试用(具有现有散列覆盖用户提供的信息（例如，电子邮件）)
* 新哈希覆盖用户提供的信息（例如，电子邮件），现有设备ID =>现有试用版（具有现有设备ID）
* 现有散列覆盖用户提供的信息（例如，电子邮件），现有设备ID =>现有试用版

>[!NOTE]
>对用户提供的信息的验证和散列由程序员处理，而不是Adobe。

**可根据以下属性配置提升临时传递功能：**

* 用户提供的信息键（例如，电子邮件）
* 用户有权使用的资源数
* TTL — 用户有权使用配置资源数的时间间隔

### 用户元数据 {#user-metadata}

为了方便程序员应用程序的实施，以下&#x200B;**用户元数据信息在Promotional Temp Pass上显示**，其中包含相应的密钥(要激活密钥，请联系tve-support@adobe.com)：

* **remaining_resources**：当前用户有权使用的剩余资源数
* **used_assets**：当前用户已使用的资源列表
* **expiration_date**：当前用户的过期日期

### 如何计算观看时间？ {#compute-viewing-time}

临时传递保持有效的时间与用户在程序员应用程序上查看内容所花费的时间无关。 在通过“提升临时传递”对初始用户请求授权时，通过将初始当前请求时间添加到由程序员指定的TTL（持续时间时间间隔）来计算到期时间。

### 身份验证和授权 {#authn-authz}

对于“提升临时传递”流程，身份验证和授权不会与实际MVPD通信，**因此，只要满足以下所有条件，所有授权请求都将成功**：

* 授权令牌对指定资源有效
* **used_assets**&#x200B;的数目低于程序员设置的限制
* **expiration_date**&#x200B;值晚于当前日期。

### 预检行为 {#preflight-beh}

当对促销临时通过MVPD发出预检或预授权请求时，返回的相应预检响应将包含预检成功时来自预检请求的完整资源列表。

其背后的逻辑是：“促销临时通过”授权条件基于时间和资源编号限制，而不是基于特定资源。 更具体地说，只要满足时间限制并且没有超过资源限制，将被调用的资源将被授权。

### SSO {#sso}

促销临时传递的实例未启用SSO，该实例配置为启用“每个请求者的身份验证”。 这意味着，当用户从应用程序A切换到另一个应用程序B（两者都与同一促销临时密码集成）时，用户将不会自动登录。

### 注销 {#logout}

注销时会删除设备上的所有令牌。 因此，从“促销临时传递”切换到用户选择的常规MVPD不应依赖此实施。 建议使用`setSelectedProvider(null)`函数以清除应用程序状态，然后重新启动身份验证流程，这样用户体验会更好。

### 提升临时传递流程图 {#promo-tempass-flowdia}

![促销临时传递流程图](../../../assets/promo-temp-pass-flow.png)

*图：促销临时传递流程*

## 实施促销临时通行 {#impl-promo-tempass}

促销临时传递需要以下客户端功能：

* **用户标识符信息，例如电子邮件地址传播**（发送验证和授权流中的用户电子邮件地址）。 Adobe Pass身份验证需要电子邮件来绑定身份验证和授权令牌（与`device_ID`的情况类似，在所有调用中都是必需的）。
* **强制身份验证** — 允许程序员在用户已进行身份验证后强制身份验证流程。 每次启动应用程序时强制用户元数据刷新（用户元数据键&#x200B;**used_assets**&#x200B;包含可用资源数）需要此功能。 由于用户可以在多个设备上登录，因此应用程序启动期间设备上存在的用户元数据不可靠，我们需要对其进行更新，以反映该特定用户的当前状态（由电子邮件地址标识）。


>[!IMPORTANT]
>仅可在iOS和Android上执行强制身份验证。
>Adobe Pass身份验证没有内置机制可在X分钟后停止免费流式传输。 用户使用Y空闲资源后，Adobe Pass身份验证将停止发出&#x200B;**授权**&#x200B;和&#x200B;**短媒体**&#x200B;令牌。 一旦促销临时密码过期，将由程序员限制访问。

## 安全性 {#security}

>[!IMPORTANT]
>Adobe不存储任何个人身份信息(PII)。 因此，程序员必须对Adobe Pass身份验证API上唯一用户提供的信息设置哈希。

用户标识符信息的&#x200B;**散列处理**

Adobe建议在将数据发送到Adobe之前对数据使用&#x200B;**SHA-2**&#x200B;系列或其特定的&#x200B;**SHA-256**、**SHA-512**&#x200B;功能。

例如，**&quot;user@domain.com&quot;**&#x200B;上的&#x200B;**SHA-256**&#x200B;是&#x200B;**&quot;f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&quot;**。

## 重置或清除促销临时传递 {#reset-promo-tempass}

某些业务规则需要定期清除“促销临时通行”。 为此，Adobe Pass身份验证为程序员提供了&#x200B;*公共* Web API，如下所述：

| `DELETE https://mgmt.auth.adobe.com/reset-tempass/v2/reset` |
|----|
| <ul><li>协议： **https**</li><li>主机：<ul><li>发行版本：**mgmt.auth.adobe.com**</li><li>前qual： **mgmt-prequal.auth.adobe.com**</li></ul></li><li>路径： **/reset-tempass/v2/reset**</li><li>查询参数： **device_id=all&amp;requestor_id=THE_REQUESTOR_ID&amp;mvpd_id=THE_TEMPASS_MVPD_ID**</li><li>标头： ApiKey： **1232293681726481**</li> <li>响应：<ul><li>成功： **HTTP 204**</li><li>失败： **HTTP 400**&#x200B;不正确的请求，**HTTP 401**（如果未指定ApiKey），**HTTP 403**（如果ApiKey无效）</li></ul></li></ul> |

除了清除Temp Pass的要求之外， Promotional Temp Pass在验证和授权清除时使用以&#x200B;**generic_data**&#x200B;发送的用户标识符信息的哈希。

将发送哈希，而不是发送整个JSON：

```cURL
$ curl -X DELETE -H "Authorization:Bearer H4j7cF3GtJX81BrsgDa10GwSizVz" "https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=FlexibleTempPass"
```

### 支持的客户端 {#supported-clients}

| Adobe Pass身份验证客户端 | 促销临时通票 | 重置工具 | 支持专用响应代码/客户端错误 |
|:--------------------------------------:|:---------------------:|:----------:|:-----------------------------------------------:|
| JS访问启用程序 | 是 | 是 | 是（从v 3.0.0开始） |
| Native Client iOS | 是 | 是 | 是（从v 1.10开始） |
| Native Client Android | 是 | 是 | 是 |
| 无客户端API | 是 | 是 | 否 |


## 限制 {#limitations}

本节介绍适用于当前实施的促销临时传递的限制。

### 无客户端 {#lim-clientless}

**没有唯一设备ID的智能设备**

并非所有智能设备应用程序都能提供唯一设备ID。 在缺少设备标识的情况下，Adobe Pass身份验证可以使用设备注册代码服务生成的UUID作为Adobe的唯一设备ID。 这意味着当用户注销时，身份验证和授权令牌将被删除。 一旦用户再次尝试进行身份验证，这次使用不同的用户信息（例如，电子邮件）用户将能够再次授权。 Adobe建议添加不允许用户“欺骗”系统的UI流程，并添加逻辑以确定是新用户请求试用还是现有试用。

**正在重置/清除临时密码**

在Xbox360和Xbox One的特定情况下，无客户端程序的重置临时密码不可用，因为这些平台需要额外的设备ID解析，在“重置临时密码”工具中则不可用。

<!--
>[!RELATEDINFORMATION]
>
>* [Preflight Authorization](/help/authentication/preflight-authz.md)
-->