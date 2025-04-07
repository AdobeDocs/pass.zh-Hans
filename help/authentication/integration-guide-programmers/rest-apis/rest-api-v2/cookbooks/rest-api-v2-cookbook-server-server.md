---
title: REST API V2指南（服务器到服务器）
description: REST API V2指南（服务器到服务器）
exl-id: 3160c03c-849d-4d39-95e5-9a9cbb46174d
source-git-commit: b753c6a6bdfd8767e86cbe27327752620158cdbb
workflow-type: tm+mt
source-wordcount: '2510'
ht-degree: 0%

---

# REST API V2指南（服务器到服务器） {#rest-api-v2-cookbook-server-to-server}

>[!IMPORTANT]
>
> 此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

>[!IMPORTANT]
>
> REST API V2实施受[限制机制](/help/authentication/integration-guide-programmers/throttling-mechanism.md)文档限制。

本文档面向将[Adobe Pass Authentication REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)集成到具有服务器到服务器(S2S)架构的流应用程序中的开发人员。

## 先决条件 {#prerequisites}

有关术语和定义，请参阅[REST API V2术语表](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md)文档。

有关强制要求和建议的做法，请参阅[REST API V2核对清单](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-checklist.md)文档。

有关常见问题，请参阅[REST API V2常见问题解答](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md)文档。

### 组件 {#components}

在开始使用之前，请确保您了解工作流中使用的以下组件和术语：

| 类型 | 组件 | 描述 |
|---------------------------|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Adobe基础架构 | Adobe Pass服务 | 与MVPD IdP和AuthZ服务集成，以提供身份验证和授权决策。 |
| 程序员基础架构 | 程序员服务 | 将流设备与Adobe Pass服务连接以获取经过身份验证的用户档案和授权决策。 |
| MVPD基础架构 | MVPD IdP服务 | MVPD端点负责基于凭据的身份验证，验证用户的身份。 |
|                           | MVPD AuthZ服务 | MVPD端点，用于根据用户订阅、家长控制和其他权利规则确定授权决策。 |
| 流设备 | 流应用程序 | 在用户流设备上运行并播放经过身份验证的视频内容的程序员应用程序。 |
|                           | （可选）身份验证模块 | 如果流设备具有User-Agent（例如，浏览器），则AuthN模块会处理MVPD IdP上的用户身份验证。 |
| （可选）身份验证设备 | AuthN应用程序 | 如果流设备缺少用户代理（例如，浏览器），则AuthN应用程序是通过Web浏览器从单独设备访问的程序员Web应用程序。 |

### 要求 {#requirements}

在服务器到服务器(S2S)实现中，流应用程序和程序员服务必须建立协议，使程序员服务能够：

* 代表流应用程序与Adobe Pass服务进行通信。

* 根据[AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)标头的要求，收集并传递流设备的唯一设备标识符。

* 根据[X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md)标头的要求，收集和传递准确的流设备信息，包括源端口和设备特定的详细信息。

* 收集并传递X-Forwarded-For标头所需的流设备IP地址。

* 将设备ID、客户端ID和客户端密钥等参数安全地存储在流应用程序或程序员服务中。

* 按照MVPD和集成应用程序（包括设备IP、源端口、设备特定信息、MRSS和可选标识符，如ECID）来格式化数据并发送数据。

* 维护并安全地管理与Adobe共享的证书，以传输加密的用户元数据。

* 在缓存时遵循身份验证配置文件和授权决策有效性，确保在收到通知时身份验证和授权状态失效。

* 将授权决策和相关说明返回到流应用程序。

### 环境 {#environments}

在进入工作流之前，请确保至少保持两个环境：生产和暂存。

**生产**

生产环境必须高度可用并适当扩展，才能处理大型或意外的流量尖峰，例如由体育赛事直播或突发新闻导致的流量尖峰。

* Adobe Pass服务在美国各地多个分散的数据中心中运行，以优化性能并最大程度地减少延迟。

   * 程序员服务应采用类似的基础架构策略，确保Adobe Pass提供低延迟的响应时间。

* 程序员必须提供其生产环境的公共IP范围。

   * 这些IP将添加到Adobe Pass基础架构中的允许列表。

* 程序员服务必须将DNS缓存限制在最多30秒，以允许动态重新路由，以防由于数据中心不可用而Adobe需要重定向流量。

**暂存**

暂存环境可以是最小的，但应通过包括所有关键系统组件和业务逻辑来镜像生产。

* 在部署到生产环境之前，它必须允许测试版本。

* 它在操作上应保持与生产相似，从而能够进行现实的测试。

* 理想情况下，暂存环境应连接到Adobe Pass测试环境，以便：

   * 允许程序员针对Adobe的基础设施进行测试。

   * 启用Adobe ，以便在必要时协助进行测试和故障排除。

## 工作流 {#workflow}

执行以下步骤，如下图所示。

![REST API V2指南（服务器到服务器）](/help/authentication/assets/rest-api-v2/cookbooks/rest-api-v2-cookbook-server-to-server-diagram.png)

*REST API V2指南（服务器到服务器）*

## A.登记阶段 {#registration-phase}

注册阶段的目的是通过Dynamic Client Registration (DCR)过程注册针对Adobe Pass身份验证的流应用程序。

动态客户端注册(DCR)过程要求流应用程序获取一对客户端凭据，并检索访问令牌作为注册阶段的最终目标。

注册阶段是强制性的，但如果流应用程序具有一对缓存的客户端凭据和仍然有效的访问令牌，则该流应用程序可以跳过此阶段。

+++相关文章

API：

* [检索客户端凭据](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [检索访问令牌](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)

流量：

* [动态客户端注册流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

常见问题解答：

* [注册阶段常见问题解答](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md)

+++

### 步骤1：注册应用程序 {#step-1-register-your-application}

* 检索客户端凭据：程序员服务通过调用&#x200B;[**/o/client/register**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)&#x200B;终结点来检索客户端凭据。

   * 程序员服务或程序员应用程序必须存储客户端凭据，并在需要检索访问令牌时无限期使用这些凭据。


* 检索访问令牌：程序员服务通过调用&#x200B;[**/o/client/token**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)&#x200B;终结点检索访问令牌。

   * 程序员服务或程序员应用程序必须存储和使用访问令牌，直到它过期，然后丢弃它并获取新的访问令牌。

## B.认证阶段 {#authentication-phase}

验证阶段的目的是为流应用程序提供验证用户身份和获取用户元数据信息的能力。

当流应用程序需要播放内容时，身份验证阶段是预授权阶段或授权阶段的先决步骤。

+++相关文章

API

* [创建身份验证会话](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [恢复身份验证会话](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [检索身份验证会话](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)
* [在用户代理中执行身份验证](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)
* [检索用户档案](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [检索特定mvpd的配置文件](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [检索特定代码的配置文件](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

流

* [在主应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [在辅助应用程序中执行的基本身份验证流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
* [基本配置文件在主要应用程序中执行](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [基本配置文件在辅助应用程序中执行](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

常见问题解答

* [身份验证阶段常见问题解答](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)

+++

### 步骤2：检查现有的已验证用户档案 {#step-2-check-for-existing-authenticated-profiles}

* **检索配置文件：**&#x200B;程序员服务通过调用&#x200B;[**/api/v2/{serviceProvider}/profiles**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)&#x200B;终结点，代表流式处理应用程序检查现有配置文件。


* **方案1：**&#x200B;存在配置文件，程序员服务可以进入[预授权阶段](#preauthorization-phase)或[授权阶段](#authorization-phase)。


* **方案2：**&#x200B;没有现有的配置文件，程序员服务可能会继续执行下一步以[对用户进行身份验证](#step-3-authenticate-the-user)。


* **方案3：**&#x200B;没有现有的配置文件，程序员服务可以继续通过[TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md)功能为用户提供临时访问权限。

   * 此方案超出了此文档的范围，有关详细信息，请参阅[临时访问流](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)文档。

### 步骤3：对用户进行身份验证 {#step-3-authenticate-the-user}

* **检索配置：**&#x200B;程序员服务通过调用&#x200B;[**/api/v2/{serviceProvider}/configuration**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)&#x200B;终结点检索可用MVPD的列表。

   * 程序员服务可以实施自定义筛选机制，以根据配置响应细化MVPD列表，从而使流应用程序仅显示预期的提供程序，而隐藏其他提供程序（例如，正在开发的MVPD、测试MVPD、TempPass）。 这可以确保在选择电视提供商时，向用户呈现精选内容。


* **创建身份验证会话：**&#x200B;程序员服务通过调用&#x200B;[**/api/v2/{serviceProvider}/sessions**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)&#x200B;终结点来启动身份验证会话。

   * 程序员服务必须将`code`和`url`返回到流应用程序。


* **方案1：**&#x200B;流式应用程序可以打开浏览器或Web视图，因此必须加载身份验证`url`。

   * 用户在MVPD登录页面中提交用户名和密码。 成功验证后，最终重定向会显示成功页面。


* **方案2：**&#x200B;流式应用程序无法打开浏览器，因此必须显示身份验证`code`。 需要单独的Web应用程序来提示用户输入`code`、构造身份验证`url`并打开： [**/api/v2/authenticate/{serviceProvider}/{code}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)。

   * 用户在MVPD登录页面中提交用户名和密码。 成功验证后，最终重定向会显示成功页面。

### 步骤4：检查已验证的用户档案 {#step-4-check-for-authenticated-profiles}

* **检索特定代码的配置文件：**&#x200B;程序员服务必须使用`code`实施轮询机制，通过调用&#x200B;[**/api/v2/{serviceProvider}/profiles/code/{code}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)&#x200B;终结点来检查是否成功生成并保存了配置文件。

   * 程序员服务必须在以下条件下&#x200B;**启动轮询**&#x200B;机制：

      * **在主（屏幕）应用程序内执行的身份验证：**&#x200B;当浏览器组件加载[会话](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)终结点请求中为`redirectUrl`参数指定的URL后，程序员服务应在用户到达最终目标页面时开始轮询。

      * **在辅助（屏幕）应用程序内执行的身份验证：**&#x200B;程序员服务应用程序应在用户启动身份验证过程后立即开始轮询 — 在收到[会话](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)终结点响应并向用户显示身份验证代码之后。

   * 程序员服务必须在以下条件下&#x200B;**停止轮询**&#x200B;机制：

      * **身份验证成功：**&#x200B;已成功检索用户的配置文件信息，确认其身份验证状态。 此时，不再需要轮询。

      * **身份验证会话和代码过期：**&#x200B;身份验证会话和代码过期，如[会话](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)终结点响应中的`notAfter`时间戳（如30分钟）所指示。 如果发生这种情况，用户必须重新启动身份验证过程，使用以前的身份验证代码的轮询应立即停止。

      * **生成的新身份验证代码：**&#x200B;如果用户请求在主（屏幕）设备上生成新的身份验证代码，则现有会话不再有效，使用以前的身份验证代码的轮询应立即停止。

   * 程序员服务必须在以下条件下&#x200B;**配置轮询**&#x200B;机制频率：

      * **在主（屏幕）应用程序内执行的身份验证：**&#x200B;程序员服务应每3-5秒或更长时间轮询一次。

      * **在辅助（屏幕）应用程序内执行的身份验证：**&#x200B;程序员服务应每3-5秒或更长时间轮询一次。

   * 程序员服务应将用户的部分配置文件信息缓存在永久性存储中，以避免不必要的请求并改善用户体验。

## C.（可选）预授权阶段 {#preauthorization-phase}

预授权阶段的目的是为流式应用程序提供从用户有权访问的目录呈现资源子集的能力。

当用户首次打开流应用程序或导航到新部分时，预授权阶段可以增强用户体验。

预授权阶段不是强制性的，如果流应用程序希望呈现资源目录而不是首先根据用户权利筛选资源，则可以跳过此阶段。

+++相关文章

API

* [检索预授权决策](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)

流

* [在主应用程序中执行的基本预授权流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

常见问题解答

* [预授权阶段常见问题解答](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#preauthorization-phase-faqs-general)

+++

### 步骤5：检查预授权的资源 {#step-5-check-for-preauthorized-resources}

* **检索预授权决策：**&#x200B;程序员服务通过调用&#x200B;[**/api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)&#x200B;终结点来检索资源列表的预授权决策。

   * 程序员服务必须将允许列表和拒绝预授权决策传递给流应用程序。

   * 将预授权决策存储在永久存储中不需要程序员服务。 但是，建议将允许决策缓存到内存中以改进用户体验。 这有助于避免对已预授权的资源进行不必要的调用，从而减少延迟并提高性能。

   * 通过检查决策预授权终结点响应中包含的[错误代码和消息](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)，程序员服务可以确定拒绝预授权决策的原因。 这些详细信息可让您深入了解预授权请求被拒绝的具体原因，从而帮助告知用户体验或在应用程序中触发任何必要的处理。 请确保在预授权决策被拒绝时，为检索预授权决策而实施的任何重试机制都不会导致无限循环。 考虑将重试限制为合理数字，并通过向用户提供明确的反馈，谨慎处理拒绝请求。

   * 由于MVPD施加的条件，程序员服务可以在单个API请求中为有限数量的资源获取预授权决策，通常最多可达5个。 在通过Adobe Pass [TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)与MVPD达成一致后，您的组织管理员或代表您行事的Adobe Pass身份验证代表可以查看和更改资源的最大数量。

## D.授权阶段 {#authorization-phase}

授权阶段的目的是为流应用程序提供在MVPD中验证用户请求的资源后，播放这些资源的功能。

授权阶段是强制性的，如果流应用程序希望播放用户请求的资源，则无法跳过此阶段，因为它需要在释放流之前与MVPD验证用户是否有权使用流。

+++相关文章

API

* [检索授权决策](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

流

* [在主应用程序中执行的基本授权流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

常见问题解答

* [授权阶段常见问题解答](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authorization-phase-faqs-general)

+++

### 步骤6：检查授权资源 {#step-6-check-for-authorized-resources}

* **检索授权决定：**&#x200B;程序员服务通过调用&#x200B;[**/api/v2/{serviceProvider}/decision/authorize/{mvpd}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)&#x200B;终结点，检索流应用程序传递的特定资源的授权决定。

   * 将授权决策存储在永久存储中不需要程序员服务。

   * 通过检查决策授权终结点响应中包含的[错误代码和消息](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)，程序员服务可以确定拒绝授权决策的原因。 这些详细信息可为insight提供授权请求被拒绝的具体原因，有助于告知用户体验或在流应用程序中触发任何必要的处理。 请确保在授权决策被拒绝时，为检索授权决策而实施的任何重试机制都不会导致无限循环。 考虑将重试限制为合理数字，并通过向用户提供明确的反馈，谨慎处理拒绝请求。

   * 程序员服务可以评估其他业务规则，并向流应用程序返回适当的授权决定。

   * 当流正在播放时，不需要程序员服务来刷新过期的媒体令牌。 如果媒体令牌在播放期间过期，则应该允许流继续而不会中断。 但是，下次用户尝试播放资源时，客户端必须请求新的授权决定，并获取新的媒体令牌。

   * 由于MVPD施加的条件，程序员服务可以在单个API请求中为有限数量的资源获取授权决策，通常最多可达1。

## E.注销阶段 {#logout-phase}

注销阶段的目的是为流应用程序提供在用户请求时在Adobe Pass身份验证中终止用户的已验证配置文件的功能。

注销阶段是强制性的，流应用程序必须为用户提供注销功能。

+++相关文章

API

* [启动特定mvpd的注销](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)

流

* [在主应用程序中执行的基本注销流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)

常见问题解答

* [注销阶段常见问题解答](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#logout-phase-faqs-general)

+++

### 步骤7：注销 {#step-7-logout}

* 启动Adobe Pass注销：程序员服务通过调用[/api/v2/{serviceProvider}/logout/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)终结点来启动流应用程序请求的注销流程。

   * 程序员服务可清除其存储的有关已验证用户的任何信息。

   * 程序员服务必须按照注销终结点响应的`actionName`和`actionType`属性中提供的说明进行操作，以确保正确完成注销过程。

      * 如果响应中的`actionType`属性设置为“interactive”，则程序员服务必须将`url`属性值返回到流应用程序。

         * **方案1：**&#x200B;流应用程序可以打开浏览器或Web视图，因此必须加载注销`url`。

         * **场景2：**&#x200B;流应用程序无法打开浏览器，因此注销过程可以停止，因为MVPD会话未保留在流设备浏览器缓存中。
