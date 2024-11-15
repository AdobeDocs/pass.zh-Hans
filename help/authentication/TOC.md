---
product: adobe primetime
audience: end-user
feature: Authentication
user-guide-title: Adobe Pass 身份验证
user-guide-description: Adobe Pass 身份验证是一个适用于 TV Everywhere 的授权解决方案，它提供一个模块化框架，以供确定请求访问资源的人员是否有权访问该资源。
source-git-commit: 1370554c66116a357970fb05c046608e261f0ed3
workflow-type: tm+mt
source-wordcount: '1214'
ht-degree: 2%

---


# Adobe Pass身份验证帮助 {#authentication}

+ [Adobe Pass身份验证概述](home.md)
+ Adobe Pass身份验证概念{#authentication-concepts}
   + [技术文件](technical-paper.md)
   + [面向程序员的概述](programmer-overview.md)
   + [MVPD概述](mvpd-overview.md)
+ 快速入门指南{#kickstart-guides}
   + [程序员kickstart指南](programmer-kickstart-guide.md)
   + [MVPD快速入门指南](mvpd-kickstart-guide.md)
+ 程序员集成指南{#programmer-integration-guide}
   + [程序员集成指南概述](programmer-integration-guide-overview.md)
   + [程序员权利流](entitlement-flow.md)
   + [程序员用例](programmer-use-cases.md)
   + [传递客户端信息（设备、连接和应用程序）](passing-client-information-device-connection-and-application.md)
   + [节流机构](throttling-mechanism.md)
   + REST API V1 {#rest-api-v1}
      + [REST API概述](rest-api-overview.md)
      + [REST API指南（服务器到服务器）](rest-api-cookbook-servertoserver.md)
      + [REST API指南（客户端到服务器）](rest-api-cookbook-clienttoserver.md)
      + Rest API引用{#rest-api-reference}
         + [REST API参考](rest-api-reference.md)
         + [注册码请求](registration-code-request.md)
         + [返回注册记录](return-registration-record.md)
         + [删除注册记录](delete-registration-record.md)
         + [提供MVPD列表](provide-mvpd-list.md)
         + [启动身份验证](initiate-authentication.md)
         + [检查身份验证令牌](check-authentication-token.md)
         + [检索身份验证令牌](retrieve-authentication-token.md)
         + [启动授权](initiate-authorization.md)
         + [检索授权令牌](retrieve-authorization-token.md)
         + [获取短媒体令牌](obtain-short-media-token.md)
         + [通过第二屏幕Web应用程序检查身份验证流程](check-authentication-flow-by-second-screen-web-app.md)
         + [检索预授权资源的列表](retrieve-list-of-preauthorized-resources.md)
         + [按第二屏Web应用程序检索预授权资源列表](retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md)
         + [启动注销](initiate-logout.md)
         + [用户元数据](user-metadata.md)
         + [Retrieve profile-request](retrieve-profilerequest.md)
         + [令牌交换](token-exchange.md)
         + [临时通行证和促销临时通行证的免费预览](free-preview-for-temp-pass-and-promotional-temp-pass.md)
   + REST API V2 {#rest-api-v2}
      + [REST API V2概述](./rest-api-v2/rest-api-v2-overview.md)
      + [REST API V2术语表](./rest-api-v2/rest-api-v2-glossary.md)
      + [REST API V2常见问题解答](./rest-api-v2/rest-api-v2-faqs.md)
      + API {#rest-api-v2-apis}
         + [REST API V2 API概述](rest-api-v2/apis/rest-api-v2-apis-overview.md)
         + 配置{#rest-api-v2-configuration-apis}
            + [检索特定服务提供商的配置](rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)
         + 会话{#rest-api-v2-sessions-apis}
            + [创建身份验证会话](rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
            + [恢复身份验证会话](rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
            + [检索身份验证会话](rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)
            + [在用户代理中执行身份验证](rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)
         + 配置文件{#rest-api-v2-profiles-apis}
            + [检索用户档案](rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
            + [检索特定mvpd的配置文件](rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
            + [检索特定代码的配置文件](rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
         + 决策{#rest-api-v2-decisions-apis}
            + [使用特定的mvpd检索授权决策](rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
            + [使用特定的mvpd检索预授权决策](rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
         + 注销{#rest-api-v2-logout-apis}
            + [启动特定mvpd的注销](rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)
         + 合作伙伴单点登录{#rest-api-v2-partner-single-sign-on-apis}
            + [检索合作伙伴身份验证请求](rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md)
            + [使用合作伙伴身份验证响应检索配置文件](rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md)
      + 流{#rest-api-v2-flows}
         + [REST API V2流概述](rest-api-v2/flows/rest-api-v2-flows-overview.md)
         + 基本访问流{#rest-api-v2-basic-access-flows}
            + [基本配置文件在主要应用程序中执行](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
            + [基本配置文件在辅助应用程序中执行](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)
            + [在主应用程序中执行的基本身份验证流程](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
            + [在辅助应用程序中执行的基本身份验证流程](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
            + [在主应用程序中执行的基本授权流程](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)
            + [在主应用程序中执行的基本预授权流程](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)
            + [在主应用程序中执行的基本注销流程](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)
         + 已降级的访问流{#rest-api-v2-degraded-access-flows}
            + [降级访问流](rest-api-v2/flows/degraded-access-flows/rest-api-v2-access-degraded-flows.md)
         + 临时访问流{#rest-api-v2-temporary-access-flows}
            + [临时访问流](rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)
         + 单点登录访问流程{#rest-api-v2-single-sign-on-access-flows}
            + [使用合作伙伴流程进行单点登录](rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md)
            + [使用平台身份流进行单点登录](rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md)
            + [使用服务令牌流进行单点登录](rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md)
            + [单个注销流程](rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-logout-flow.md)
      + 指南{#rest-api-v2-cookbooks}
         + [REST API V2指南（客户端到服务器）](rest-api-v2/cookbooks/rest-api-v2-cookbook-client-server.md)
         + [REST API V2指南（服务器到服务器）](rest-api-v2/cookbooks/rest-api-v2-cookbook-server-server.md)
      + 附录{#rest-api-v2-appendix}
         + 标头{#rest-api-v2-appendix-headers}
            + [标头 — 授权](rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md)
            + [标头 — AP-Device-Identifier](rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)
            + [标头 — X-Device-Info](rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md)
            + [标头 — AD-Service-Token](rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md)
            + [标头 — Adobe-Subject-Token](rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md)
            + [标头 — AP-Partner-Framework-Status](rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md)
            + [标头 — AP-TempPass-Identity](rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md)
   + AccessEnabler SDK {#accessenabler-sdk}
      + JavaScript SDK {#javascriptsdk}
         + [JavaScript SDK概述](javascript-sdk-overview.md)
         + [JavaScript SDK指南](javascript-sdk-cookbook.md)
         + [JavaScript SDK API参考](javascript-sdk-api-reference.md)
         + 准则{#js-sdk-guidelines}
            + [无刷新登录和注销](refreshless-login-and-logout.md)
         + JavaScript API {#javascript-sdk-api}
            + [预授权](preauthorize-api-javascript-sdk.md)
      + iOS/tvOS SDK {#ios-sdk}
         + [iOS/tvOS SDK概述](iostvos-sdk-overview.md)
         + [iOS/tvOS SDK指南](iostvos-sdk-cookbook.md)
         + [iOS/tvOS SDK API参考](iostvos-sdk-api-reference.md)
         + 准则{#ios-tvos-sdk-guidelines}
            + [iOS/tvOS应用程序注册](iostvos-application-registration.md)
            + 迁移准则{#migration-guidelines}
               + [iOS/tvOS v3.x迁移指南](iostvos-v3x-migration-guide.md)
            + [iOS/tvOS存储完整性检查](iostvos-sdk-storage-integrity-checks.md)
         + iOS/tvOS API {#ios-tvos-sdk-api}
            + [预授权](preauthorize-api-ios-tvos-sdk.md)
      + Android SDK {#androidsdk}
         + [Android SDK概述](android-sdk-overview.md)
         + [Android SDK指南](android-sdk-cookbook.md)
         + [Android SDK API参考](android-sdk-api-reference.md)
         + 准则{#androidguidelines}
            + [Android应用程序注册](android-application-registration.md)
            + [带有动态客户端注册功能的Android SDK](android-sdk-with-dynamic-client-registration.md)
         + Android API{#android-sdk-api}
            + [预授权](preauthorize-api-android-sdk.md)
      + Amazon FireOS SDK {#fireossdk}
         + [Amazon FireOS技术概述](amazon-fireos-technical-overview.md)
         + [Amazon FireOS集成指南](amazon-fireos-integration-cookbook.md)
         + [Amazon FireOS API参考](amazon-fireos-native-client-api-reference.md)
         + [Amazon FireOS应用程序注册](amazon-fireos-application-registration.md)
         + [带有动态客户端注册的FireOS SDK](fireos-sdk-with-dynamic-client-registration.md)
         + [Amazon FireOS SSO — 程序员启动指南](amazon-firetv-sso-programmer-kickoff-guide.md)
   + 单点登录{#sso}
      + 合作伙伴单点登录{#partner-sso}
         + Apple单点登录{#apple-sso}
            + [Apple SSO概述](single-sign-on/partner-single-sign-on/apple-single-sign-on/apple-sso-overview.md)
            + [Apple SSO指南(REST API V2)](single-sign-on/partner-single-sign-on/apple-single-sign-on/apple-sso-cookbook-rest-api-v2.md)
            + [Apple SSO指南(REST API V1)](single-sign-on/partner-single-sign-on/apple-single-sign-on/apple-sso-cookbook-rest-api-v1.md)
            + [Apple SSO指南(iOS/tvOS SDK)](single-sign-on/partner-single-sign-on/apple-single-sign-on/apple-sso-cookbook-iostvos-sdk.md)
      + 平台单点登录{#platform-sso}
         + Amazon单点登录{#amazon-sso}
            + [Amazon SSO指南(REST API V2)](single-sign-on/platform-single-sign-on/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v2.md)
            + [Amazon SSO指南(REST API V1)](single-sign-on/platform-single-sign-on/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v1.md)
         + Roku单点登录{#roku-sso}
            + [Roku SSO概述](single-sign-on/platform-single-sign-on/roku-single-sign-on/roku-sso-overview.md)
   + 内容元数据{#content-metadata}
      + [标识受保护的资源](identify-protected-resources.md)
   + 内容服务器集成{#content-serv-int}
      + [如何集成媒体令牌验证器](media-token-verifier-int.md)
   + 附录{#appendices}
      + [调试提示](appendix-b-debugging-tips.md)
+ MVPD集成指南{#mvpd-int-guide}
   + [集成功能](mvpd-integr-features.md)
   + [身份验证](authn-usecase.md)
   + [使用OAuth 2.0协议进行身份验证](authn-oauth2-protocol.md)
   + [授权](authz-usecase.md)
   + [印前检查授权](mvpd-preflight-authz.md)
   + [MVPD注销](usecase-mvpd-logout.md)
   + [内容元数据交换](mvpd-content-metadata-exchange.md)
   + [用户元数据交换](mvpd-user-metadata-exchng.md)
   + [代理MVPD Web服务](proxy-mvpd-webserv.md)
   + [代理MVPD SAML集成](proxy-mvpd-saml-int.md)
   + [服务提供商范围](serv-provider-scoping.md)
   + [MVPD允许IP地址](mvpd-listing-ip-addres.md)
+ Adobe Pass身份验证功能{#auth-features}
   + Adobe Analytics集成{#analytics-int}
      + [将Adobe Pass身份验证服务器端数据集成到Adobe Analytics](integrate-authn-servr-data-analytics.md)
      + [在Adobe Pass身份验证中使用Experience CloudID](exp-cloud-id-authn.md)
   + 授权服务监控{#entitlement-service-monitoring}
      + [授权服务监控概述](entitlement-service-monitoring-overview.md)
      + [授权服务监控API](entitlement-service-monitoring-api.md)
      + [服务器端量度](understanding-serverside-metrics.md)
   + 临时传递{#temp-pass}
      + [临时传递](temp-pass.md)
      + [促销临时通票](promotional-temp-pass.md)
      + [重置临时传递](reset-temp-pass.md)
   + 单点登录{#sso}
      + [单点登录支持](sso-support.md)
      + [通过被动身份验证的SSO](sso-passive-authn.md)
   + 基于主页的身份验证{#home-based-auth}
      + [适用于所有地区的电视的家庭身份验证](home-based-authn-tve.md)
      + [MVPD的HBA状态](hba-status-mvpds.md)
   + 用户元数据{#user-metadat}
      + [用户元数据](user-metadata-feature.md)
      + [用于加密的用户元数据证书](user-metadata-certificate.md)
   + [印前检查授权](preflight-authz.md)
   + 报告{#error-reportn}时出错
      + [错误报告](error-reporting.md)
      + [增强的错误代码](enhanced-error-codes.md)
   + 客户端注册{#dcr-api}
      + [动态客户端注册概述](./dcr-api/dynamic-client-registration-overview.md)
      + API {#dcr-api-apis}
         + [检索客户端凭据](./dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
         + [检索访问令牌](./dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md)
      + 流{#dcr-api-flows}
         + [动态客户端注册流程](./dcr-api/flows/dynamic-client-registration-flow.md)
   + 降级服务{#degrn-service}
      + [降级API概述](degradation-api-overview.md)
   + 隐私就绪{#privacy-readiness}
      + [隐私支持概述](privacy-supp-overview.md)
      + [如何提出隐私请求](make-privacy-req.md)
+ 提示和疑难解答{#tips-troubleshoot}
   + [在选择对话框中允许MVPD](allow-mvpd-selectn-dialog.md)
   + [阻止MVPD显示“选择”对话框](prevent-mvpd-selectn-dialog.md)
+ 支持{#support}
   + [上报程序](escalation-procedures.md)
   + [监控Adobe PassAdobePayTV Pass](monitoring-adobe-pay-tv-pass.md)
   + [最低系统要求](minimum-system-requirements.md)
+ 发行说明{#release-notes}
   + [Adobe Pass Authentication 3.0.3发行说明](auth-rn-303.md)
   + [Adobe Pass Authentication 3.0发行说明](auth-rn-300.md)
   + [Adobe Pass Authentication 2.70发行说明](auth-rn-270.md)
   + [Adobe Pass Authentication 2.69发行说明](auth-rn-269.md)
   + [Adobe Pass Authentication 2.68发行说明](auth-rn-268.md)
   + [Adobe Pass Authentication 2.67发行说明](auth-rn-267.md)
   + [Adobe Pass Authentication 2.66发行说明](auth-rn-266.md)
   + [Adobe Pass Authentication 2.65.1发行说明](auth-rn-2651.md)
   + [Adobe Pass Authentication 2.65发行说明](auth-rn-265.md)
   + [Adobe Pass Authentication 2.64.1发行说明](auth-rn-2641.md)
   + [Adobe Pass Authentication 2.64发行说明](auth-rn-264.md)
   + [Adobe Pass Authentication 2.63发行说明](auth-rn-263.md)
   + [Adobe Pass Authentication 2.62.1发行说明](auth-rn-2621.md)
   + JavaScript SDK发行说明{#release-notes-javascript}
      + [Adobe Pass Authentication JavaScript 4.7.0发行说明](authn-rn-javascript-470.md)
      + [Adobe Pass Authentication JavaScript 4.6.0发行说明](authn-rn-javascript-460.md)
      + [Adobe Pass Authentication JavaScript 4.4.0发行说明](authn-rn-javascript-440.md)
      + [Adobe Pass Authentication JavaScript 4.2.0发行说明](authn-rn-javascript-420.md)
      + [Adobe Pass Authentication JavaScript 4.1.1发行说明](authn-rn-javascript-411.md)
      + [Adobe Pass Authentication JavaScript 4.1.0发行说明](authn-rn-javascript-410.md)
      + [Adobe Pass Authentication JavaScript 4.0.0发行说明](authn-rn-javascript-400.md)
      + [Adobe Pass Authentication JavaScript 3.5.0发行说明](authn-rn-javascript-350.md)
   + iOS/tvOS SDK发行说明{#release-notes-ios}
      + [Adobe Pass Authentication iOS / tvOS 3.9.2发行说明](authn-rn-ios-tvos-392.md)
      + [Adobe Pass Authentication iOS / tvOS 3.8.4发行说明](authn-rn-ios-tvos-384.md)
      + [Adobe Pass Authentication iOS / tvOS 3.8.3发行说明](authn-rn-ios-tvos-383.md)
      + [Adobe Pass Authentication iOS / tvOS 3.8.2发行说明](authn-rn-ios-tvos-382.md)
      + [Adobe Pass Authentication iOS / tvOS 3.8.1发行说明](authn-rn-ios-tvos-381.md)
      + [Adobe Pass Authentication iOS / tvOS 3.7.0发行说明](authn-rn-ios-tvos-370.md)
   + Android SDK发行说明{#release-notes-android}
      + [Adobe Pass Authentication Android 3.7.3发行说明](authn-rn-android-373.md)
+ 技术说明{#tech-notes}
   + Adobe Pass身份验证SDK {#primetime-authentication-sdks}
      + [证书常见问题解答](certificates-qa.md)
      + JavaScript SDK {#javascript}
         + [跟踪预防评估 — Apple Safari](tracking-prevention-assessment-apple-safari.md)
         + [跟踪预防评估 — Google Chrome](tracking-prevention-assessment-google-chrome.md)
         + [Cookie更新 — SameSite和Secure标记](cookies-updates-samesite-and-secure-flags.md)
      + Android SDK {#android}
         + [Android 10应用程序上的Access Enabler Android SDK单点登录(SSO)](access-enabler-android-sdk-single-signon-sso-on-android-10-devices.md)
         + [Adobe Pass身份验证和Android 6“Marshmallow”新权限模型](adobe-primetime-authentication-and-the-android-6-marshmallow-new-permissions-model.md)
      + iOS/tvOS SDK {#iostvos}
         + [iOS SDK 3.1+上的WKWebView支持](wkwebview-support-on-ios-sdk-31.md)
         + [iOS SDK 3.2+上的SFSafariViewController支持](sfsafariviewcontroller-support-on-ios-sdk-32.md)
         + [使用iOS Authentication Access Enabler时Adobe Pass上的SSO](sso-on-ios-when-using-the-primetime-authentication-access-enabler.md)
         + [iOS身份验证错误 — 找不到adobepass.ios.app](ios-authentication-error-adobepassiosapp-cannot-be-found.md)
         + [在iOS上重置临时传递](reset-temp-pass-on-ios.md)
         + [使用控制台应用程序日志调试AccessEnabler iOS/tvOS SDK](debugging-the-accessenabler-iostvos-sdk-using-console-app-logs.md)
         + [AccessEnabler iOS/tvOS 3.7.0升级路径](accessenabler-iostvos-370-upgrade-path.md)
   + 传递身份验证环境{#primetime-authentication-environments}
      + [了解Adobe环境](understanding-the-adobe-environments.md)
      + [在资格预审中设置环境和测试](setting-up-your-environment-and-testing-in-prequal.md)
      + [如何使用AdobeAPI测试站点测试身份验证和授权流](test-authn-authz-flows-using-adobes-api-test-site.md)
   + 无客户端API {#clientless-api}
      + [无客户端API实施 — 错误代码/包含可能原因/原因的消息](clientless-api-implementation-error-codes-messages-with-probable-reason-cause.md)
      + [缺少设备ID时的无客户端API流](clientless-api-flow-in-the-absence-of-device-id.md)
      + [无客户端：避免在/authenticate请求中使用&#39;&amp;&#39;reg_code](clientless-avoid-using-reg-code-in-authenticate-request.md)
      + [为Xbox 360和XboxOne无客户端程序员启用Adobe Pass授权服务](enabling-primetime-entitlement-services-for-a-programmer-on-xbox-360-and-xboxone-clientless-solution.md)
      + [无客户端设备类型和量度](benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)
   + 用户体验{#user-exp}
      + [如何将MVPD登录页面从iFrame迁移到弹出窗口](migr-mvpd-login-iframe-popup.md)
      + [印前检查功能：如何启用、排除故障或确定问题](preflight-feature.md)
   + 工具和实用工具{#tools-and-utilities}
      + [使用Charles代理](using-charles-proxy.md)
   + 概念{#concepts}
      + [了解用户ID](understanding-user-ids.md)
+ [TVE仪表板用户指南](tve-dashboard/old-tve-dashboard/tve-dashboard-user-guide.md)
+ 新TVE仪表板用户指南{#user-guide}
   + [TVE仪表板概述](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-overview.md)
   + [环境](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-environments.md)
   + [审核和推送更改](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-review-push-changes.md)
   + [仪表板](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-home.md)
   + [渠道](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-channels.md)
   + [程序员](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-programmers.md)
   + [MVPDs](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-mvpds.md)
   + [集成](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-integrations.md)
   + [报告](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-reports.md)
   + [更改日志](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-changes-log.md)
+ [术语表](glossary.md)
