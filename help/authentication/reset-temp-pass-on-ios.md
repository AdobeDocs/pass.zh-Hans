---
title: 在iOS上重置临时传递
description: 在iOS上重置临时传递
exl-id: 53a22fae-192c-4b4c-9d63-fd9a2d960923
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '717'
ht-degree: 0%

---

# 在iOS上重置临时传递 {#reset-temp-pass-on-ios}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

</br>

iOS演示应用程序包括一个用于重置Temp Pass TTL的专用屏幕。 重置操作需要以下信息：

- **环境：** 指定将接收重置临时传递网络调用的付费 — 电视传递服务器终结点Adobe。 可能的值： **先决条件** (*mgmt-prequal.auth-staging.adobe.com*)， **版本** (*mgmt.auth.adobe.com*)或 **自定义** (保留供Adobe内部测试使用)。
- **OAuth2持有者令牌：** OAuth2令牌是授权程序员进行Adobe付费电视身份验证所必需的。 此类令牌可从专用付费电视身份验证OAuth2端点(例如， *curl -u &quot;\&lt;consumer _key=&quot;&quot;>：\&lt;consumer _secret=&quot;&quot; _key=&quot;&quot;>*&quot; *&quot;https://mgmt.auth.adobe.com/oauth2/permanent\_accesstoken？grant\_type=client\_credentials&quot;*)。
- **请求者ID：** 当前程序员的唯一ID 从演示应用程序的主屏幕（请求者字段）中读取此值。
- **临时传递ID：** 临时传递MVPD的唯一ID。
- **设备ID：** 演示应用程序计算的经过哈希处理的设备ID。
- **通用键：** 某些Temp Pass MVPD（即下一个可扩展的Temp Pass功能）支持用于重置Temp Pass的通用键（与设备ID一起）。

上述所有参数(除 *通用键*)为必填项。 以下是演示应用程序将执行的参数和相关网络调用的示例（示例采用*curl *命令的形式）：

- **环境：** 发行版本(*mgmt.auth.adobe.com*)
- **OAuth2持有者令牌：** H4j7cF3GtJX81BrsgDa10GwSizVz
- **程序员ID：** 参照
- **临时传递ID：** TempPassREF
- **设备ID：** f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991
- **通用键：** null（未提供值）

```curl
curl -X DELETE -H "Authorization:Bearer* *H4j7cF3GtJX81BrsgDa10GwSizVz" "https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset?device_id=f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991&requestor_id=REF&mvpd_id=TempPassREF"
```

将向发出DELETEHTTP请求 **/reset** 端点，传递 *Oauth2持有者令牌* 在Authorization标头和 *设备ID*， *请求者ID* 和 *临时传递ID (MVPD ID)* 作为参数。

如果程序员为 *通用键*，则会执行另一个HTTP调用(此次 **/reset/general** 端点)，传递 *通用键* 内部 *键* 请求参数。

例如，设置 *通用键* 对电子邮件地址哈希（用于支持此类功能的临时传递MVPD）将产生以下HTTP调用(电子邮件为 `user@domain.com` 其SHA-256哈希为 `f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`)：

```curl
curl -X DELETE -H "Authorization:Bearer H4j7cF3GtJX81BrsgDa10GwSizVz"
"https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```

需要强调的是，在演示应用程序中重置Temp Pass对同一设备上的程序员应用程序可能没有相同的效果。 这是由于设备ID（由演示应用程序和AccessEnabler计算）对于设备上的所有应用程序可能不同所致：

- iOS 6及更低版本：设备ID是使用MAC地址计算的（每个应用程序都具有唯一性），因此在演示应用程序中重置Temp Pass将会在设备上的所有其他程序员应用程序中重置该ID。

- iOS 7及更高版本：设备ID是根据IDFV（供应商ID）值计算的，该值对于具有相同捆绑ID前缀的所有应用程序（即除最后一个组件以外的所有组件）都是唯一的。 由于演示应用程序和程序员应用程序具有不同的捆绑包ID，因此重置演示应用程序中的Temp Pass不会对程序员应用程序产生影响。

最后一个用例(iOS 7及更高版本)是最常见的，因此我们来看看在这种情况下，程序员如何为其应用程序重置Temp Pass。 有几个选项：

1. 将代码从演示应用程序移植到程序员应用程序。 此 *TempPassResetViewController* 和 *DeviceIdDemoApp* 类包含用于重置Temp Pass的核心逻辑，可以轻松地对其进行修改并将其包含在程序员应用程序中。

1. 执行HTTP请求以重置Temp Pass *curl*. device\_Id参数可以通过计算程序员应用程序的IDFV并在其上应用SHA-256哈希来获取(示例代码位于 *DeviceIdDemoApp* 类)。

1. 只需将程序员应用程序的哈希IDFV指定为 *通用键*. 这将导致两个网络调用：一个用于为演示应用程序重置Temp Pass（与程序员无关），另一个用于为程序员应用程序重置Temp Pass。

以上所有选项都类似，取决于实施的难易程度，由程序员来选择。
