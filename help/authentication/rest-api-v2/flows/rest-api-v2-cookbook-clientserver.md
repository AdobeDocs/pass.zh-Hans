---
title: REST API V2 — 指南 — 客户端/服务器实施步骤
description: REST API V2 — 指南 — 客户端/服务器实施步骤
source-git-commit: 5ba538bdb13d121ba27005df82d4ae604f912241
workflow-type: tm+mt
source-wordcount: '698'
ht-degree: 0%

---


# REST API V2 — 指南 — 客户端/服务器实施步骤 {#rest-api-v2-cookbook-clientserver}

>[!IMPORTANT]
>
> 此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

>[!IMPORTANT]
>
> REST API V2实施受[限制机制](/help/authentication/throttling-mechanism.md)文档限制。

## 在客户端应用程序中实施REST API V2的步骤 {#steps-to-implement-the-rest-api-v2-in-client-side-applications}

要实施Adobe Pass REST API V2，您需要遵循以下分阶段的步骤。

## A.登记阶段 {#registration-phase}

### 步骤1：注册应用程序 {#step-1-register-your-application}

应用程序要能够调用Adobe Pass REST API V2，它需要API安全层所需的访问令牌。
要获取访问令牌，应用程序需要执行如下所述的步骤：
[动态客户端注册](./dynamic-client-registration.md)

## B.认证阶段 {#authentication-phase}

### 步骤2：检查现有的已验证用户档案 {#step-2-check-for-existing-authenticated-profiles}

流式处理应用程序检查现有的已验证配置文件： <b>/api/v2/{serviceProvider}/配置文件</b><br>
（[检索已验证的配置文件](./apis/profiles-apis/rest-api-v2-retrieve-authenticated-profiles.md)）

* 如果未找到配置文件，则流应用程序将实施TempPass流
   * 按照有关如何实施[临时访问流](./temporary-access-flows/rest-api-v2-access-temporary-flows.md)的文档操作
* 如果未找到配置文件，则流应用程序将实施身份验证流程
   * 流式应用程序检索可用于serviceProvider的MVPD列表： <b>/api/v2/{serviceProvider}/configuration</b><br>
（[检索可用MVPD的列表](./apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)）
   * 流式应用程序可以对MVPD列表进行过滤，并且只显示预期的MVPD，同时隐藏其他MVPD（TempPass、测试MVPD、正在开发的MVPD等）
   * 流应用程序显示选择器，用户选择MVPD
   * 流式应用程序创建会话： <b>/api/v2/{serviceProvider}/sessions</b><br>
（[创建身份验证会话](./apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)）<br>
      * 返回用于身份验证的代码和URL
      * 如果找到配置文件，则流应用程序可能会继续到<a href="#preauthorization-phase">C。预授权阶段</a>或<a href="#authorization-phase">D。授权阶段</a>

### 步骤3：对用户进行身份验证 {#step-3-authenticate-the-user}

使用浏览器或基于Web的第二个屏幕的应用程序：

* 选项1. 流式应用程序可以打开浏览器或Web视图，加载URL以进行身份验证，并且用户登陆需要提交凭据的MVPD登录页面
   * 用户输入登录/密码，最终重定向显示成功页面
* 选项2。 流式应用程序不能打开浏览器而只显示代码。 <b>需要开发单独的Web应用程序</b>以要求用户输入代码、生成并打开URL： <b>/api/v2/authenticate/{serviceProvider}/{CODE}</b>
   * 用户输入登录/密码，最终重定向显示成功页面

### 步骤4：检查已验证的用户档案 {#step-4-check-for-authenticated-profiles}

流式应用程序检查是否使用MVPD进行身份验证，以便在浏览器或第二个屏幕中完成

* 建议在<b>/api/v2/{serviceProvider}/profiles/{mvpd}</b><br>上每15秒轮询一次
（[检索特定MVPD的已验证配置文件](.apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)）
   * 如果流应用程序中未选择MVPD，因为第二个屏幕应用程序中存在MVPD选取器，则应该使用代码<b>/api/v2/{serviceProvider}/profiles/code/{CODE}</b><br>进行轮询
（[检索特定代码的已验证配置文件](./apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)）
* 轮询不应超过30分钟，如果达到30分钟且流应用程序仍处于活动状态，则需要启动新会话，并返回新的代码和URL
* 身份验证完成后，返回值为200，配置文件已通过身份验证
* 流应用程序可以继续到<a href="#preauthorization-phase">C。预授权阶段</a>或<a href="#authorization-phase">D。授权阶段</a>

## C.预先授权阶段 {#preauthorization-phase}

### 步骤5：检查预授权的资源 {#step-5-check-for-preauthorized-resources}

流式应用程序准备显示可供经过身份验证的用户使用的视频，并且可以检查
对这些资源的访问权限。

* 步骤是可选的，如果应用程序希望过滤在经过身份验证的用户包中不可用的资源，请执行此步骤
* 调用<b>/api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}</b><br>
（[使用特定的MVPD检索预授权决策](.apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)）


## D.授权阶段 {#authorization-phase}

### 步骤6：检查授权资源 {#step-6-check-for-authorized-resources}

流应用程序准备播放用户选择的视频/资产/资源。

* 每个播放开始都需要执行步骤
* 调用<b>/api/v2/{serviceProvider}/decision/authorize/{mvpd}</b><br>
（[使用特定MVPD检索授权决策](.apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)）
   * decision = &#39;允许&#39; ，流设备开始流
   * decision = &#39;Deny&#39;，流设备会通知用户它无权访问该视频

## E.注销阶段 {#logout-phase}

### 步骤7：注销 {#step-7-logout}

流设备：用户希望从MVPD注销

* 调用<b>/api/v2/{serviceProvider}/logout/{mvpd}</b><br>
（[启动特定MVPD的注销](.apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)）
* 如果存在response actionType=&#39;interactive&#39;和url，请在浏览器/第二个屏幕中打开url以完成通过MVPD的注销

