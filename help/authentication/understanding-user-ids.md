---
title: 了解用户ID
description: 了解用户ID
exl-id: 813a8501-db72-4850-a387-c8db6120db80
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '602'
ht-degree: 0%

---

# 了解用户ID {#understanding-user-ids}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

从概念上讲，启动权利流的每个用户都与一个唯一的用户ID相关联。 但是，在权利流的过程中，这个用户ID可能会以不同的方式呈现，具体取决于您从哪个API获取该ID。

短媒体令牌中的`sessionGUID`是用户ID的安全形式，可通过`sendTrackingData()`调用获取。 在所有当前的集成中，这是一个跨时间和设备的永久性GUID。 GUID的源以来自MVPD的身份验证响应中的用户ID开头。 但是，某些MVPD将来可能会改变主意，并开始发送临时GUID。 如果程序员希望确保AuthN响应中的MVPD源用户ID是永久性的，他们应在与MVPD的协议中对其进行安排。

以下是在Adobe Pass身份验证API中表示用户ID的不同方式：

| 属性 | 用途 | 哈希 | 数字签名 | 描述 |
| --- | --- | --- | --- | --- |
| sendTrackingData() GUID属性 | 跟踪/分析 | 是 | 否 | - MVPD用户ID，按Adobe进行哈希处理。 用户ID无法追溯到MVPD的源中。</br> </br> — 此形式的ID未经过数字签名，因此对于防欺诈而言是不安全的。 但是，它足以用于分析。 </br> </br> — 此形式的用户ID是在Adobe Pass身份验证在AuthN/AuthZ流中生成的所有事件的客户端提供的。 |
| 短媒体令牌的sessionGUID属性 | 对并发使用情况的欺诈跟踪 | 是 | 是 |  — 这与通过sendTrackingData()发送的用户ID相同，但是，此ID经过数字签名以保护其完整性，并且足以用于欺诈跟踪。</br> </br> — 此视频将在使用我们的验证器库后在服务器端进行处理，并且可以在向客户端发布视频流之前分析欺诈模式。  这些任务中的任何一个都由程序员来完成。 |
| getMetadata() userID属性 | 帐户关联，与MVPD进行欺诈调查 | 否 | 否 |  — 此属性允许Adobe向程序员公开实际的源MVPD用户ID。</br> </br> — 在Adobe的配置中，可以将其设置为加密或不加密（取决于MVPD首选项）。 如果加密，将使用提供给Adobe的程序员证书中的公钥进行加密，这样客户端就不会清楚地看到该密钥。</br> </br> — 这将为程序员提供MVPD中的实际用户ID，因此可用于直接与MVPD进行帐户关联或欺诈调查。 |


**结论**

* 通常，MVPD会提供一个永久的唯一ID <u>，并在成功验证</u>时将其传递给Adobe。 它通常在所有网络中都是一致的。 Comcast MVPD例外，它为每个渠道提供不同的用户ID。

* MVPD用户ID不包含PII，它不是帐号。 它不需要以加密形式公开，因为我们已通过所有MVPD验证未发送任何PII。

用户ID的使用方式取决于用例：

* 如果您需要它来进行跟踪/分析，最切实可行的做法是从`sendTrackingData()`中获取它。
* 如果在服务器端需要用于流发布、欺诈或操作数据，则可以从Media Token验证器中获取它。
* 如果您需要它来关联帐户和进行更深入的欺诈，请咨询您的Adobe联系人以获取可用性。
