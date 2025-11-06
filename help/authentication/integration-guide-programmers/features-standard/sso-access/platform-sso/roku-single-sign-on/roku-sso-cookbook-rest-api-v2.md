---
title: Roku SSO指南(REST API V2)
description: Roku SSO指南(REST API V2)
exl-id: 77b154bc-c09f-49d4-b1af-cc33bc6dd22b
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '568'
ht-degree: 0%

---

# Roku SSO指南(REST API V2) {#roku-sso-cookbook-rest-api-v2}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

Adobe Pass身份验证REST API V2支持在RokuOS上运行的客户端应用程序的最终用户的平台单点登录(SSO)。

此文档用作现有[REST API V2概述](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)的扩展，该视图提供了高级视图以及描述如何使用平台标识流[实施](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md)单点登录的文档。

## 使用平台标识流进行Roku单点登录 {#cookbook}

Adobe Pass身份验证可与Roku协作以改善登录用户体验，并促进电视订阅者在TV Everywhere应用程序中进行单点登录(SSO)。

### 先决条件 {#prerequisites}

在继续使用平台标识流进行Roku单点登录之前，请确保已启用Roku SSO。 默认情况下，Roku SSO处于启用状态，除非程序员或MVPD请求禁用SSO。

每个程序员都可以通过[Adobe Pass TVE功能板](https://experience.adobe.com/pass/authentication)，为特定集成启用或禁用Roku平台上的单点登录(SSO)。

### 工作流 {#workflow}

**客户端到服务器**

对于使用客户端到服务器体系结构来集成REST API V2的程序员应用程序，Roku SSO可无缝运行，无需进行任何修改。

RokuOS会自动将两个HTTP标头附加到发送到Adobe Pass身份验证端点的所有请求。

**服务器到服务器**

对于利用服务器到服务器体系结构来集成REST API V2的程序员应用程序，程序员必须与Roku团队协调，配置这些标头以将其包含在定向到其域的所有API流中。

要启用跨应用程序和跨设备SSO，应用程序传递时应使用Roku提供的订阅者ID，而不是设备ID。

有关更多详细信息，请参阅以下文档：

* [标头 — X-Roku-Reserved-Roku-Connect-Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-roku-reserved-roku-connect-token.md)
* [标头 — AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)

有关所需标头格式的特定详细信息，请联系您的Adobe代表。

### 常见问题解答 {#faqs}

* **SSO如何工作？**

  SSO将在与同一Roku用户关联的所有Roku设备上跨由Adobe Pass身份验证提供支持的所有程序员应用程序工作。 并非所有MVPD都允许Roku SSO。


* **身份验证TTL是否有任何更改？**

  第一个有效的身份验证令牌将用于执行SSO，在这种情况下，所有其他将通过SSO进行身份验证的应用程序将使用相同的TTL，直到它过期。 因此，当从一个应用程序导航到另一个应用程序时，第二个应用程序将共享验证第一个应用程序的TTL。


* **其他Adobe功能是否会与以前一样工作？**

  所有Adobe Pass身份验证功能都将像之前一样工作。


* **Roku平台上的SSO是否使程序员选择加入/选择退出过程受益？**

  这将是Adobe TVE仪表板中的配置更改。 每个程序员都可以在Roku平台上为特定集成启用或禁用SSO。


* **哪些是常见问题？**

  程序员应确保其当前基于Adobe REST API的实施不会妨碍Roku的平台SSO。

  请参阅下面的列表，其中列出了可能的问题以及应如何解决这些问题。

| 问题 | 可能的原因 | 可采用的解决方案 |
|--------------------------------------------------|----------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|
| 未向Adobe发送Roku SSO标头 | 在调用Adobe Pass身份验证域时使用HTTP而不是HTTPS | 使用HTTPS |
| SSO令牌未显示/未更新MVPD徽标 | UI依赖于本地存储 | 应用程序应在检查身份验证后更新UI（和本地存储，如果需要） |
| 注销（无AuthZ触发） | 应用程序设计 | 应更新应用程序，以免在后台执行注销 |
