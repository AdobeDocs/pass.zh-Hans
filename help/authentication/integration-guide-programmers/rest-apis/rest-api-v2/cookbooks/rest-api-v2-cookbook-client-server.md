---
title: REST API V2指南（客户端到服务器）
description: REST API V2指南（客户端到服务器）
exl-id: 6a5a89d2-ea54-4f9c-9505-e575ced4301c
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '1833'
ht-degree: 0%

---

# REST API V2指南（客户端到服务器） {#rest-api-v2-cookbook-client-to-server}

>[!IMPORTANT]
>
> 此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

>[!IMPORTANT]
>
> REST API V2实施受[限制机制](/help/authentication/integration-guide-programmers/throttling-mechanism.md)文档限制。

本文档面向将[Adobe Pass Authentication REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)集成到具有客户端到服务器(C2S)架构的流应用程序中的开发人员。

## 先决条件 {#prerequisites}

有关术语和定义，请参阅[REST API V2术语表](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md)文档。

有关强制要求和建议的做法，请参阅[REST API V2核对清单](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-checklist.md)文档。

有关常见问题，请参阅[REST API V2常见问题解答](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md)文档。

## A.登记阶段 {#registration-phase}

注册阶段的目的是通过Dynamic Client Registration (DCR)过程注册针对Adobe Pass身份验证的流应用程序。

动态客户端注册(DCR)过程要求流应用程序获取一对客户端凭据，并检索访问令牌作为注册阶段的最终目标。

注册阶段是强制性的，但如果流应用程序具有一对缓存的客户端凭据和仍然有效的访问令牌，则该流应用程序可以跳过此阶段。

+++相关文章

**API：**

* [检索客户端凭据](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [检索访问令牌](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)

**流：**

* [动态客户端注册流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

**常见问题解答：**

* [注册阶段常见问题解答](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md)

+++

### 步骤1：注册应用程序 {#step-1-register-your-application}

* **检索客户端凭据：**&#x200B;流应用程序通过调用&#x200B;[**/o/client/register**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)&#x200B;终结点来检索客户端凭据。

   * 流应用程序必须存储客户端凭据，并在需要检索访问令牌时无限期使用这些凭据。


* **检索访问令牌：**&#x200B;流应用程序通过调用&#x200B;[**/o/client/token**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)&#x200B;终结点来检索访问令牌。

   * 流应用程序必须存储并使用访问令牌，直到它过期，然后丢弃它并获取一个新的访问令牌。

## B.认证阶段 {#authentication-phase}

验证阶段的目的是为流应用程序提供验证用户身份和获取用户元数据信息的能力。

当流应用程序需要播放内容时，身份验证阶段是预授权阶段或授权阶段的先决步骤。

+++相关文章

**API**

* [创建身份验证会话](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [恢复身份验证会话](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [检索身份验证会话](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)
* [在用户代理中执行身份验证](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)
* [检索用户档案](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [检索特定mvpd的配置文件](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [检索特定代码的配置文件](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

**流**

* [在主应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [在辅助应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
* [基本配置文件在主要应用程序中执行](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [基本配置文件在辅助应用程序中执行](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

**常见问题解答**

* [身份验证阶段常见问题解答](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)

+++

### 步骤2：检查现有的已验证用户档案 {#step-2-check-for-existing-authenticated-profiles}

* **检索配置文件：**&#x200B;流应用程序通过调用&#x200B;[**/api/v2/{serviceProvider}/profiles**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)&#x200B;终结点来检查现有配置文件。


* **方案1：**&#x200B;存在配置文件，流应用程序可以进入[预授权阶段](#preauthorization-phase)或[授权阶段](#authorization-phase)。


* **方案2：**&#x200B;没有现有的配置文件，流应用程序可以继续执行下一步以[对用户进行身份验证](#step-3-authenticate-the-user)。


* **方案3：**&#x200B;没有现有的配置文件，流应用程序可以继续通过[TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md)功能为用户提供临时访问权限。

   * 此方案超出了此文档的范围，有关详细信息，请参阅[临时访问流](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)文档。

### 步骤3：对用户进行身份验证 {#step-3-authenticate-the-user}

* **检索配置：**&#x200B;流应用程序通过调用&#x200B;[**/api/v2/{serviceProvider}/配置**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)&#x200B;终结点来检索可用MVPD的列表。

   * 流应用程序可以实施自定义过滤机制来根据配置响应细化MVPD列表，仅显示预期的提供者，同时隐藏其他提供者（例如，正在开发的MVPD、测试MVPD、TempPass）。 这可以确保在选择电视提供商时，向用户呈现精选内容。


* **创建身份验证会话：**&#x200B;流应用程序通过调用&#x200B;[**/api/v2/{serviceProvider}/sessions**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)&#x200B;终结点来启动身份验证会话。


* **方案1：**&#x200B;流式应用程序可以打开浏览器或Webview，因此必须加载身份验证`url`。

   * 用户在MVPD登录页面中提交用户名和密码。 成功验证后，最终重定向会显示成功页面。


* **方案2：**&#x200B;流式应用程序无法打开浏览器，因此必须显示身份验证`code`。 需要单独的Web应用程序来提示用户输入`code`、构造身份验证`url`并打开： [**/api/v2/authenticate/{serviceProvider}/{code}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)。

   * 用户在MVPD登录页面中提交用户名和密码。 成功验证后，最终重定向会显示成功页面。

### 步骤4：检查已验证的用户档案 {#step-4-check-for-authenticated-profiles}

* **检索特定代码的配置文件：**&#x200B;流应用程序必须使用`code`实施轮询机制，以通过调用&#x200B;[**/api/v2/{serviceProvider}/profiles/code/{code}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)终结点来检查是否成功生成并保存了配置文件。

   * 流应用程序必须在以下条件下&#x200B;**启动轮询**&#x200B;机制：

      * **在主（屏幕）应用程序内执行的身份验证：**&#x200B;当浏览器组件加载`redirectUrl`会话[终结点请求中为](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)参数指定的URL后，主（流）应用程序应在用户到达最终目标页面时开始轮询。

      * **在辅助（屏幕）应用程序内执行的身份验证：**&#x200B;主（流）应用程序应在用户启动身份验证过程后立即开始轮询 — 在收到[会话](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)终结点响应并向用户显示身份验证代码之后。

   * 流应用程序必须在以下条件下&#x200B;**停止轮询**&#x200B;机制：

      * **身份验证成功：**&#x200B;已成功检索用户的配置文件信息，确认其身份验证状态。 此时，不再需要轮询。

      * **身份验证会话和代码过期：**&#x200B;身份验证会话和代码过期，如`notAfter`会话[终结点响应中的](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)时间戳（如30分钟）所指示。 如果发生这种情况，用户必须重新启动身份验证过程，使用以前的身份验证代码的轮询应立即停止。

      * **生成的新身份验证代码：**&#x200B;如果用户请求在主（屏幕）设备上生成新的身份验证代码，则现有会话不再有效，使用以前的身份验证代码的轮询应立即停止。

   * 流应用程序必须在以下条件下&#x200B;**配置轮询**&#x200B;机制频率：

      * **在主（屏幕）应用程序内执行的身份验证：**&#x200B;主（流）应用程序应每3-5秒或更长时间轮询一次。

      * **在辅助（屏幕）应用程序内执行的身份验证：**&#x200B;主（流）应用程序应每3-5秒或更长时间轮询一次。

   * 流应用程序应该将部分用户配置文件信息缓存在永久性存储中，以避免不必要的请求并改善用户体验。

## C.（可选）预授权阶段 {#preauthorization-phase}

预授权阶段的目的是为流式应用程序提供从用户有权访问的目录呈现资源子集的能力。

当用户首次打开流应用程序或导航到新部分时，预授权阶段可以增强用户体验。

预授权阶段不是强制性的，如果流应用程序希望呈现资源目录而不是首先根据用户权利筛选资源，则可以跳过此阶段。

+++相关文章

**API**

* [检索预授权决策](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)

**流**

* [在主应用程序中执行的基本预授权流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

**常见问题解答**

* [预授权阶段常见问题解答](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#preauthorization-phase-faqs-general)

+++

### 步骤5：检查预授权的资源 {#step-5-check-for-preauthorized-resources}

* **检索预授权决策：**&#x200B;流应用程序通过调用&#x200B;[**/api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)终结点来检索资源列表的预授权决策。

   * 流应用程序不需要将预授权决策存储在永久存储中。 但是，建议将允许决策缓存到内存中以改进用户体验。 这有助于避免对已预授权的资源进行不必要的调用，从而减少延迟并提高性能。

   * 流应用程序可以通过检查包含在来自Decisions Preauthorize端点的响应中的[错误代码和消息](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)来确定拒绝预授权决策的原因。 这些详细信息可为insight提供预授权请求被拒绝的特定原因，帮助通知用户体验或在应用程序中触发任何必要的处理。 请确保在预授权决策被拒绝时，为检索预授权决策而实施的任何重试机制都不会导致无限循环。 考虑将重试限制为合理数字，并通过向用户提供明确的反馈，谨慎处理拒绝请求。

   * 由于MVPD施加的条件，流应用程序可以在单个API请求中获取对有限数量的资源的预授权决策，通常最多可达5个。 在通过Adobe Pass [TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)与MVPD达成一致后，您的组织管理员或代表您行事的Adobe Pass身份验证代表可以查看和更改资源的最大数量。


## D.授权阶段 {#authorization-phase}

授权阶段的目的是为流应用程序提供在MVPD中验证用户请求的资源后，播放这些资源的功能。

授权阶段是强制性的，如果流应用程序希望播放用户请求的资源，则无法跳过此阶段，因为它需要在释放流之前与MVPD验证用户是否有权使用流。

+++相关文章

**API**

* [检索授权决策](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

**流**

* [在主应用程序中执行的基本授权流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

**常见问题解答**

* [授权阶段常见问题解答](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authorization-phase-faqs-general)

+++

### 步骤6：检查授权资源 {#step-6-check-for-authorized-resources}

* **检索授权决定：**&#x200B;流应用程序通过调用&#x200B;[**/api/v2/{serviceProvider}/decision/authorize/{mvpd}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)终结点检索特定资源的授权决定。

   * 不需要流式应用程序将授权决策存储在永久存储中。

   * 流应用程序可以通过检查包含在来自Decisions Authorize端点的响应中的[错误代码和消息](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)来确定拒绝授权决策的原因。 这些详细信息可为insight提供授权请求被拒绝的具体原因，有助于告知用户体验或在应用程序中触发任何必要的处理。 请确保在授权决策被拒绝时，为检索授权决策而实施的任何重试机制都不会导致无限循环。 考虑将重试限制为合理数字，并通过向用户提供明确的反馈，谨慎处理拒绝请求。

   * 当流正在主动播放时，不需要流应用程序刷新过期的媒体令牌。 如果媒体令牌在播放期间过期，则应该允许流继续而不会中断。 但是，下次用户尝试播放资源时，客户端必须请求新的授权决定，并获取新的媒体令牌。

   * 由于MVPD施加的条件，流应用程序可以在单个API请求中获取对有限数量的资源的授权决策，通常最多可达1。

## E.注销阶段 {#logout-phase}

注销阶段的目的是为流应用程序提供在用户请求时在Adobe Pass身份验证中终止用户的已验证配置文件的功能。

注销阶段是强制性的，流应用程序必须为用户提供注销功能。

+++相关文章

**API**

* [启动特定mvpd的注销](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)

**流**

* [在主应用程序中执行的基本注销流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)

**常见问题解答**

* [注销阶段常见问题解答](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#logout-phase-faqs-general)

+++

### 步骤7：注销 {#step-7-logout}

* **启动Adobe Pass注销：**&#x200B;流应用程序通过调用&#x200B;[**/api/v2/{serviceProvider}/logout/{mvpd}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)终结点来启动注销流程。

   * 流应用程序必须按照注销终结点响应的`actionName`和`actionType`属性中提供的说明进行操作，以确保正确完成注销过程。

      * 如果响应中的`actionType`属性设置为“interactive”：

         * **方案1：**&#x200B;流应用程序可以打开浏览器或Web视图，因此必须加载注销`url`。

         * **方案2：**&#x200B;流应用程序无法打开浏览器，因此注销过程可以停止，因为MVPD会话未保留在流设备浏览器缓存中。
