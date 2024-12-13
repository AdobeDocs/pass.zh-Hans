---
title: 在iOS上重置临时传递
description: 在iOS上重置临时传递
exl-id: 53a22fae-192c-4b4c-9d63-fd9a2d960923
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '721'
ht-degree: 0%

---

# （旧版）在iOS上重置临时传递 {#reset-temp-pass-on-ios}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

>[!IMPORTANT]
>
> 确保随时了解汇总在[产品公告](/help/authentication/product-announcements.md)页中的最新Adobe Pass身份验证产品公告和停用时间表。

</br>

iOS演示应用程序包括一个用于重置Temp Pass TTL的专用屏幕。 重置操作需要以下信息：

- **环境：**&#x200B;指定将接收重置临时传递网络调用的Adobe付费电视传递服务器终结点。 可能的值： **Prequal** (*mgmt-prequal.auth-staging.adobe.com*)、**Release** (*mgmt.auth.adobe.com*)或&#x200B;**Custom** (保留用于Adobe内部测试)。
- **OAuth2持有者令牌：** OAuth2令牌是授权Adobe付费电视身份验证的程序员所必需的。 此类令牌可从专用付费电视身份验证OAuth2终结点获取(例如&#x200B;*curl -u &quot;\&lt;consumer\_key\>：\&lt;consumer\_secret\_key\>*&quot; *&quot;https://mgmt.auth.adobe.com/oauth2/permanent\_accesstoken？grant\_type=client\_credentials&quot;*)。
- **请求者ID：**&#x200B;当前程序员的唯一ID。 从演示应用程序的主屏幕（请求者字段）中读取此值。
- **临时传递ID：**&#x200B;临时传递MVPD的唯一ID。
- **设备ID：**&#x200B;演示应用程序计算的经过哈希处理的设备ID。
- **通用键：**&#x200B;某些Temp Pass MVPD（即下一个可扩展的Temp Pass功能）支持用于重置Temp Pass的通用键（与设备ID一起）。

上述所有参数（除&#x200B;*泛型键*&#x200B;之外）都是必需的。 以下是演示应用程序将执行的参数和相关网络调用的示例（示例采用*curl *命令的形式）：

- **环境：**&#x200B;版本(*mgmt.auth.adobe.com*)
- **OAuth2持有者令牌：** H4j7cF3GtJX81BrsgDa10GwSizVz
- **程序员ID：**&#x200B;引用
- **临时传递ID：**&#x200B;临时传递引用
- **设备ID：** f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991
- **泛型键：** null （未提供值）

```curl
curl -X DELETE -H "Authorization:Bearer* *H4j7cF3GtJX81BrsgDa10GwSizVz" "https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset?device_id=f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991&requestor_id=REF&mvpd_id=TempPassREF"
```

将向&#x200B;**/reset**&#x200B;端点发出DELETE的HTTP请求，在授权标头中传递&#x200B;*OAuth2持有者令牌*，并将&#x200B;*设备ID*、*请求者ID*&#x200B;和&#x200B;*临时传递ID (MVPD ID)*&#x200B;作为参数。

如果程序员为&#x200B;*泛型键*&#x200B;提供值，则将执行另一个HTTP调用（这次是对&#x200B;**/reset/泛型**&#x200B;终结点），在&#x200B;*键*&#x200B;请求参数中传递&#x200B;*泛型键*。

例如，将&#x200B;*通用键*设置为电子邮件地址哈希(对于
临时传递支持此类功能的MVPD将产生
HTTP调用后(电子邮件为`user@domain.com`其SHA-256
哈希为`f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`)：

```curl
curl -X DELETE -H "Authorization:Bearer H4j7cF3GtJX81BrsgDa10GwSizVz"
"https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```

需要强调的是，在演示应用程序中重置Temp Pass对同一设备上的程序员应用程序可能没有相同的效果。 这是由于设备ID（由演示应用程序和AccessEnabler计算）对于设备上的所有应用程序可能不同所致：

- iOS 6及更低版本：设备ID是使用MAC地址计算的（每个应用程序都具有唯一性），因此在演示应用程序中重置Temp Pass将会在设备上的所有其他程序员应用程序中重置该ID。

- iOS 7及更高版本：设备ID是根据IDFV（供应商ID）值计算的，该值对于具有相同捆绑ID前缀的所有应用程序（即除最后一个组件以外的所有组件）都是唯一的。 由于演示应用程序和程序员应用程序具有不同的捆绑包ID，因此重置演示应用程序中的Temp Pass不会对程序员应用程序产生影响。

最后一个用例(iOS 7及更高版本)是最常见的，因此我们来看看在这种情况下，程序员如何为其应用程序重置Temp Pass。 有几个选项：

1. 将代码从演示应用程序移植到程序员应用程序。 *TempPassResetViewController*&#x200B;和&#x200B;*DeviceIdDemoApp*&#x200B;类包含用于重置Temp Pass的核心逻辑，可以轻松地对其进行修改并将其包含在程序员应用程序中。

1. 执行HTTP请求以使用&#x200B;*curl*&#x200B;重置临时传递。 通过计算程序员应用程序的IDFV并对其应用SHA-256哈希（*DeviceIdDemoApp*&#x200B;类中的示例代码），可以获取device\_Id参数。

1. 只需将程序员应用程序的哈希IDFV指定为&#x200B;*通用键*，即可从演示应用程序执行重置。 这将导致两个网络调用：一个用于为演示应用程序重置Temp Pass（与程序员无关），另一个用于为程序员应用程序重置Temp Pass。

以上所有选项都类似，取决于实施的难易程度，由程序员来选择。
