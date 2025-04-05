---
title: REST API V2检查表
description: REST API V2检查表
source-git-commit: f0001d86f595040f4be74f357c95bd2919dadf15
workflow-type: tm+mt
source-wordcount: '2535'
ht-degree: 0%

---

# REST API V2检查表 {#rest-api-v2-checklist}

>[!IMPORTANT]
>
> 此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

本文档将强制要求和建议实践汇总到一个位置，以便程序员实施使用Adobe Pass身份验证[REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)的客户端应用程序。

在实施REST API V2时，必须遵循本文档作为验收标准的一部分，并且必须用作核对清单，以确保已执行所有必要步骤来成功集成。

## 强制性要求 {#mandatory-requirements}

### 1.登记阶段 {#mandatory-requirements-registration-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">要求</th>
      <th style="background-color: #EFF2F7;">风险</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>注册的应用程序范围</i></td>
      <td>使用具有REST API v2作用域的注册应用程序。</td>
      <td>可能会触发HTTP 401“未授权”错误响应，导致系统资源过载并增加延迟。</td>
   </tr>
    <tr>
      <td style="background-color: #DEEBFF;"><i>客户端凭据缓存</i></td>
      <td>将客户端凭据存储在永久存储中，并在每个访问令牌请求中重复使用它们。</td>
      <td>重新生成客户端凭据时，身份验证可能会丢失。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>访问令牌缓存</i></td>
      <td>将访问令牌存储在永久存储中并重复使用，直到其过期 — 不要为每个REST API v2调用请求新令牌。</td>
      <td>存在系统资源过载的风险，会增加延迟，并可能触发HTTP 429“请求过多”错误响应。</td>
   </tr>
</table>

### 2.配置阶段 {#mandatory-requirements-configuration-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">要求</th>
      <th style="background-color: #EFF2F7;">风险</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>配置检索</i></td>
      <td>仅在要求在身份验证阶段之前提示用户选择其MVPD（电视提供商）时检索配置响应。<br/><br/>在以下情况下，无需检索配置响应：<ul><li>用户已经过身份验证。</li><li>授予用户临时访问权限。</li><li>用户身份验证已过期，但可以提示用户确认他们仍然是之前选择的MVPD的订阅者。</li></ul></td>
      <td>存在系统资源过载和延迟增加的风险。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>TV提供程序选择缓存</i></td>
      <td>将用户的付费电视提供商(MVPD)选择存储在永久存储中，以将其用于所有后续阶段：<ul><li>从配置响应存储，用户选择MVPD“id”。</li><li>从配置响应存储中，用户选择MVPD “displayName”。</li><li>从配置响应存储，用户选择MVPD“logoUrl”。</li></ul></td>
      <td>存在系统资源过载和延迟增加的风险。</td>
   </tr>
</table>

### 3.身份验证阶段 {#mandatory-requirements-authentication-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">要求</th>
      <th style="background-color: #EFF2F7;">风险</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>轮询机制启动</i></td>
      <td>在以下条件下启动轮询机制： <br/><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md">在主（屏幕）应用程序内执行的身份验证</a></b><ul><li>当浏览器组件加载了会话端点请求中为“redirectUrl”参数指定的URL后，当用户到达最终目标页面时，主（流）应用程序应开始轮询。</li></ul><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md">在辅助（屏幕）应用程序内执行的身份验证</a></b><ul><li>主（流）应用程序应在用户启动身份验证过程后立即开始轮询 — 即在收到“会话”端点响应并向用户显示身份验证代码之后。</li></ul></td>
      <td>存在系统资源过载的风险，会增加延迟，并可能触发HTTP 429“请求过多”错误响应。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>轮询机制停止</i></td>
      <td>在以下条件下停止轮询机制： <br/><br/><b>成功身份验证</b><ul><li>成功检索用户的配置文件信息，确认其身份验证状态，因此不再需要轮询。</li></ul><br/><b>身份验证会话和代码过期</b><ul><li>身份验证会话和代码到期，用户必须重新启动身份验证过程，使用以前的身份验证代码的轮询应立即停止。</li></ul><br/><b>已生成新的身份验证代码</b><ul><li>如果用户请求新的身份验证代码，则现有会话将失效，使用以前的身份验证代码的轮询应立即停止。</li></ul></td>
      <td>存在系统资源过载的风险，会增加延迟，并可能触发HTTP 429“请求过多”错误响应。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>轮询机制配置</i></td>
      <td>在以下条件下配置轮询机制频率： <br/><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md">在主（屏幕）应用程序内执行的身份验证</a></b><ul><li>主（流）应用程序应每3-5秒轮询一次。</li></ul><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md">在辅助（屏幕）应用程序内执行的身份验证</a></b><ul><li>主（流）应用程序应每3-5秒轮询一次。</li></ul></td>
      <td>存在系统资源过载的风险，会增加延迟，并可能触发HTTP 429“请求过多”错误响应。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>配置文件缓存</i></td>
      <td>将部分用户配置文件信息存储在永久存储中，以提高性能并最大程度地减少不必要的REST API v2调用。<br/><br/>缓存应侧重于以下配置文件响应字段：<br/><br/><b>mvpd</b><ul><li>客户端应用程序可以使用此项来跟踪用户选择的电视提供商，并在预授权或授权阶段继续使用它。</li><li>当当前用户配置文件过期时，客户端应用程序可以使用记住的MVPD选择，只需请求用户进行确认。</li></ul><br/><b>属性</b><ul><li>用于根据不同的用户元数据键（例如，zip、maxRating等）个性化用户体验。</li><li>用户元数据在验证流程完成后变为可用，因此客户端应用程序无需查询单独的端点即可检索用户元数据信息，因为它已包含在配置文件信息中。</li><li>某些元数据属性可能会在授权阶段更新，具体取决于MVPD（例如Charter）和特定的元数据属性（例如householdID）。 因此，客户端应用程序可能需要在授权后再次查询Profiles API以检索最新的用户元数据。</li></ul></td>
      <td>存在系统资源过载的风险，会增加延迟，并可能触发HTTP 429“请求过多”错误响应。</td>
   </tr>
</table>

### 4. （可选）预授权阶段 {#mandatory-requirements-preauthorization-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">要求</th>
      <th style="background-color: #EFF2F7;">风险</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>预授权决策检索</i></td>
      <td>将预授权决策用于内容过滤，从不用于播放决策。</td>
      <td>违反程序员、MVPD和Adobe之间的合同协议的风险。<br/><br/>绕过我们的监控和警报系统的风险。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>预授权决策检索重试</i></td>
      <td>正确处理<a href="/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md">增强的错误代码</a>，并利用操作字段确定必要的修正步骤。<br/><br/>只有有限数量的增强型错误代码需要重试，而大多数需要操作字段中指定的替代分辨率。<br/><br/>请确保为检索预授权决策而实施的任何重试机制不会导致无休止循环，并且它会将重试限制为合理的数字（即2-3）。</td>
      <td>存在系统资源过载的风险，会增加延迟，并可能触发HTTP 429“请求过多”错误响应。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>预授权决策缓存</i></td>
      <td>缓存成功允许在内存中进行决策，以提高性能并最大程度地减少不必要的REST API v2调用，因为应用程序运行时订阅更新并不频繁。</td>
      <td>存在系统资源过载的风险，会增加延迟，并可能触发HTTP 429“请求过多”错误响应。</td>
   </tr>
</table>

### 5.授权阶段 {#mandatory-requirements-authorization-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">要求</th>
      <th style="background-color: #EFF2F7;">风险</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>授权决策检索</i></td>
      <td>在播放之前获取授权决策 — 无论是否存在预授权决策。<br/><br/>即使媒体令牌在播放期间过期，并允许流继续无中断运行，并在用户发出下次播放请求时请求包含（新增）媒体令牌的新授权决策，而不考虑它是针对同一资源还是其他资源。<br/><br/>长时间运行的实时流可以选择在视频操作后请求新的授权决策，例如在MRSS发生更改时暂停内容、启动商业中断或修改资产级配置。</td>
      <td>违反程序员、MVPD和Adobe之间的合同协议的风险。<br/><br/>绕过我们的监控和警报系统的风险。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>授权决策检索重试</i></td>
      <td>正确处理<a href="/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md">增强的错误代码</a>，并利用操作字段确定必要的修正步骤。<br/><br/>只有有限数量的增强型错误代码需要重试，而大多数需要操作字段中指定的替代分辨率。<br/><br/>请确保为检索授权决策而实施的任何重试机制不会导致无休止的循环，并且它将重试限制为合理的数字（即2-3）。</td>
      <td>存在系统资源过载的风险，会增加延迟，并可能触发HTTP 429“请求过多”错误响应。</td>
   </tr>
</table>

### 6.注销阶段 {#mandatory-requirements-logout-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">要求</th>
      <th style="background-color: #EFF2F7;">风险</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>注销支持</i></td>
      <td>实施注销API以允许用户手动注销，终止其已验证的配置文件，并遵循为每个已删除的配置文件指定的REST API v2操作名称：<ul><li>对于支持注销端点的MVPD，客户端应用程序需要导航到用户代理中提供的“url”。</li><li>对于“appleSSO”类型配置文件，客户端应用程序需要引导用户也从合作伙伴级别(Apple的系统设置)注销。</li></ul></td>
      <td>由于缺少对客户端应用程序端的支持，导致客户端应用程序出现故障的风险。</td>
   </tr>
</table>

### 7.参数和标头 {#mandatory-requirements-parameters-headers}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">要求</th>
      <th style="background-color: #EFF2F7;">风险</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>发送授权标头</i></td>
      <td>为每个REST API v2请求发送<a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md">授权</a>标头。</td>
      <td>可能会触发HTTP 401“未授权”错误响应，导致系统资源过载并增加延迟。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>发送AP设备标识符标头</i></td>
      <td>为每个REST API v2请求发送<a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md">AP-Device-Identifier</a>标头。<br/><br/>即使请求来自代表设备的服务器，AP-Device-Identifier标头值也必须反映实际的流设备标识符。</td>
      <td>触发HTTP 400“错误请求”错误响应、系统资源过载和延迟增加的风险。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>发送X-Device-Info头</i></td>
      <td>为每个REST API v2请求发送<a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md">X-Device-Info</a>标头。<br/><br/>即使请求来自代表设备的服务器，X-Device-Info标头值也必须反映实际的流设备信息。</td>
      <td>风险被分类为源自未知平台并被视为不安全，因而受到更严格的规则的约束，例如较短的身份验证TTL。<br/><br/>此外，某些字段，如流式设备connectionIp和connectionPort，对于频谱的Home Base Authentication等功能是必需的。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>稳定设备标识符</i></td>
      <td>计算并存储<a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md">AP-Device-Identifier</a>标头的稳定设备标识符，该标识符不会随着更新或重新启动而更改。<br/><br/>对于没有硬件标识符的平台，从应用程序属性生成唯一标识符并将其保留。</td>
      <td>当设备标识符更改时，存在身份验证丢失的风险。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>遵循API引用</i></td>
      <td>确保只发送REST API v2预期的参数和标头。</td>
      <td>触发HTTP 400“错误请求”错误响应、系统资源过载和延迟增加的风险。</td>
   </tr>
</table>

### 8.错误处理 {#mandatory-requirements-error-handling}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">要求</th>
      <th style="background-color: #EFF2F7;">风险</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>增强的错误代码处理支持</i></td>
      <td>正确处理<a href="/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md">增强的错误代码</a>，并利用操作字段确定必要的修正步骤。<br/><br/>只有有限数量的增强型错误代码需要重试，而大多数需要操作字段中指定的替代分辨率。<br/><br/>如果在启动应用程序之前的开发阶段进行了正确处理，<a href="/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2">增强型错误代码 — REST API V2</a>文档中列出的大多数增强型错误代码都可以完全阻止。</td>
      <td>存在系统资源过载的风险，会增加延迟，并可能触发HTTP 429“请求过多”错误响应。<br/><br/>由于缺少处理增强的错误代码而导致客户端应用程序故障的风险 — 导致错误消息不明确、用户指导不正确或回退行为不正确。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>HTTP错误处理支持</i></td>
      <td>区分处理HTTP错误响应（例如，400、401、403、404、405、500）和包含增强错误代码有效负载的成功响应（例如，200、201），如上所述。<br/><br/>只有有限数量的HTTP错误代码需要重试，而大多数需要替代解决方案。<br/><br/>如果在启动应用程序之前的开发阶段处理得当，大部分HTTP错误响应都可以完全阻止。</td>
      <td>存在系统资源过载的风险，会增加延迟，并可能触发HTTP 429“请求过多”错误响应。<br/><br/>由于缺少处理增强的错误代码而导致客户端应用程序故障的风险 — 导致错误消息不明确、用户指导不正确或回退行为不正确。</td>
   </tr>
</table>

### 9.测试 {#mandatory-requirements-testing}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">要求</th>
      <th style="background-color: #EFF2F7;">风险</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>生命周期测试</i></td>
      <td>使用官方的Adobe Pass身份验证非生产环境开发和测试应用程序：<ul><li>前期生产</li><li>Release-Staging</li></ul><br/>在启动到发布生产之前，在这些环境中执行彻底的质量保证(QA)。<br/><br/>客户端应用程序必须先在非生产环境中完成端到端验证，然后才能进入发布生产环境。</td>
      <td>带有严重和主要缺陷的风险启动。<br/><br/>缺少简短而有效的调试路径可能会阻碍Adobe支持和工程部门快速干预。</td>
   </tr>
</table>

## 推荐做法 {#recommended-practices}

### 1.登记阶段 {#recommended-practices-registration-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">实践</th>
      <th style="background-color: #EFF2F7;">风险</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>访问令牌验证</i></td>
      <td>主动检查访问令牌的有效性并在过期时刷新它。<br/><br/>在重试原始请求之前，请确保用于处理HTTP 401“未授权”错误的任何重试机制首先刷新访问令牌。</td>
      <td>可能会触发HTTP 401“未授权”错误响应，导致系统资源过载并增加延迟。</td>
   </tr>
</table>

### 2.配置阶段 {#recommended-practices-configuration-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">实践</th>
      <th style="background-color: #EFF2F7;">风险</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>配置缓存</i></td>
      <td>将配置响应存储在内存或永久存储中较短时间（例如3-5分钟）以提高性能并最大程度地减少不必要的REST API v2调用。</td>
      <td>存在系统资源过载和延迟增加的风险。</td>
   </tr>
</table>

### 3.身份验证阶段 {#recommended-practices-authentication-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">实践</th>
      <th style="background-color: #EFF2F7;">风险</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>验证代码验证（第2屏验证）</i></td>
      <td>在以下条件下，在调用/api/v2/authenticate API之前，验证通过辅助(2)应用程序（屏幕）上的用户输入提交的身份验证代码：<br/><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md#perform-authentication-within-secondary-application-with-preselected-mvpd">在辅助（屏幕）应用程序内使用预先选择的mvpd执行的身份验证</a></b><ul><li>利用<a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md">恢复身份验证会话</a> - POST /api/v2/{serviceProvider}/sessions/{code}</li></ul><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md#perform-authentication-within-secondary-application-without-preselected-mvpd">在辅助（屏幕）应用程序中执行的身份验证，但未预先选择mvpd</a></b><ul><li>使用<a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md">检索身份验证会话</a> - GET /api/v2/{serviceProvider}/sessions/{code}</li></ul><br/>如果键入的身份验证代码错误或身份验证会话过期，客户端应用程序将收到错误。</td>
      <td>在身份验证期间可能会出现各种错误响应和工作流问题。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>支持多个配置文件</i></td>
      <td>通过提示用户选择配置文件或应用自定义逻辑（例如自动选择有效期最长的配置文件），确保客户端应用程序可以处理多个配置文件。</td>
      <td>由于缺少对客户端应用程序端的支持，导致客户端应用程序出现故障的风险。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>（可选）非基本流支持</i></td>
      <td>支持非基本流程，以防客户应用程序业务需要：<ul><li>降级访问流（高级功能）</li><li>临时访问流程（高级功能）</li><li>单点登录访问流程（标准功能）</li></ul></td>
      <td>存在造成用户体验不理想的风险。</td>
   </tr>
</table>

### 4. （可选）预授权阶段 {#recommended-practices-preauthorization-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">实践</th>
      <th style="background-color: #EFF2F7;">风险</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>用户体验</i></td>
      <td>使用MVPD或Adobe通过增强错误代码提供的消息传递功能，在预授权决策被拒绝时显示清晰的用户反馈。</td>
      <td>存在造成用户体验不理想的风险。</td>
   </tr>
</table>

### 5.授权阶段 {#recommended-practices-authorization-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">实践</th>
      <th style="background-color: #EFF2F7;">风险</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>媒体令牌验证</i></td>
      <td>使用<a href="/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier">媒体令牌验证器</a>库验证媒体令牌。</td>
      <td>冒着串流欺诈等欺诈方案的风险。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>用户体验</i></td>
      <td>使用MVPD或Adobe通过增强错误代码提供的消息传送功能，在授权决策被拒绝时显示清晰的用户反馈。</td>
      <td>存在造成用户体验不理想的风险。</td>
   </tr>
</table>

### 6.注销阶段 {#recommended-practices-logout-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">实践</th>
      <th style="background-color: #EFF2F7;">风险</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>用户体验</i></td>
      <td>避免在预授权或授权被拒绝等情况下自动（以编程方式）调用注销API，因为注销API只应在响应直接用户请求时调用。</td>
      <td>风险使用户误认为身份验证失败。</td>
   </tr>
</table>

### 7.参数和标头 {#recommended-practices-parameters-headers}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">实践</th>
      <th style="background-color: #EFF2F7;">风险</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>重用代码</i></td>
      <td>重用REST API v1中的代码计算设备标识符和设备信息，但请确保只发送REST API v2预期的参数和标头。<br/><br/>重用REST API v1中的代码来调用DCR API以检索访问令牌。</td>
      <td>-</td>
   </tr>
</table>

### 8.测试 {#recommended-practices-testing}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">实践</th>
      <th style="background-color: #EFF2F7;">风险</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>测试覆盖率</i></td>
      <td>确保跨设备和平台测试以下基本流：<br/><br/><b>身份验证流</b><ul><li>主应用程序（屏幕）身份验证方案</li><li>辅助应用程序（屏幕）身份验证方案</li></ul><br/><b>（可选）预授权流程</b><ul><li>测试允许决策方案</li><li>测试拒绝决策方案</li></ul><br/><b>授权流</b><ul><li>测试允许决策方案</li><li>测试拒绝决策方案</li></ul><br/><b>注销流</b><br/><br/>此外，测试其他访问流（如果适用）：<br/><br/><ul><li>降级访问流（高级功能）</li><li>临时访问流程（高级功能）</li><li>单点登录访问流程（标准功能）</li></ul><br/>涵盖MVPD的顶部集成（涵盖最广泛使用的提供程序）。</td>
      <td>在生产过程中存在无法预见的故障风险，尤其是在测试不太频繁的平台上或极少数流量出现负值情况时。</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>测试工具</i></td>
      <td>使用<a href="https://developer.adobe.com/adobe-pass/">Adobe Developer</a>网站。</td>
      <td>-</td>
   </tr>
</table>

## 摘要 {#summary}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">阶段</th>
      <th style="background-color: #EFF2F7;">必需</th>
      <th style="background-color: #EFF2F7;">（强烈）推荐</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>注册</i></td>
      <td>缓存客户端凭据<br/><br/>缓存访问令牌</td>
      <td>验证和刷新访问令牌</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>配置</i></td>
      <td>最大程度地减少配置响应检索</td>
      <td>缓存配置响应</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>身份验证</i></td>
      <td>轮询机制微调<br/><br/>缓存部分配置文件</td>
      <td>支持多个配置文件<br/><br/>支持降级功能（如果业务需要）<br/><br/>支持TempPass功能（如果业务需要）<br/><br/>支持单点登录功能（如果业务需要）</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>预授权</i></td>
      <td>缓存允许预授权决定<br/><br/>重试机制微调</td>
      <td>增强用户体验，为被拒绝的预授权决策使用错误代码</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>授权</i></td>
      <td>当用户请求播放<br/><br/>重试机制微调时检索授权决策</td>
      <td>使用被拒绝的授权决定<br/><br/>媒体令牌验证的错误代码增强用户体验</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>注销</i></td>
      <td>实施注销API以允许用户手动注销</td>
      <td>避免自动调用注销API</td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">必需</th>
      <th style="background-color: #EFF2F7;">（强烈）推荐</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>参数和标头</i></td>
      <td>遵循必需的标头规范</td>
      <td>重用REST API v1中的代码</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>错误处理</i></td>
      <td>实施增强的错误处理<br/><br/>实施HTTP错误处理</td>
      <td>-</td>
   </tr>
</table>
