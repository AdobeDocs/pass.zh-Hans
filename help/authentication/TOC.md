---
product: adobe primetime
audience: end-user
feature: Authentication
user-guide-title: Adobe Pass 身份验证
user-guide-description: Adobe Pass 身份验证是一个适用于 TV Everywhere 的授权解决方案，它提供一个模块化框架，以供确定请求访问资源的人员是否有权访问该资源。
source-git-commit: ffedb5db269644c8d9c81480d27dff43bd4eb5d6
workflow-type: tm+mt
source-wordcount: '1240'
ht-degree: 2%

---


# Adobe Pass身份验证帮助 {#authentication}

+ [Adobe Pass 身份验证](home.md)
+ [产品公告](product-announcements.md)
+ 产品版本{#product-releases}
   + 2024 {#2024}
      + [Adobe Pass Authentication 3.0.3发行说明](notes-releases/auth-rn-303.md)
      + [Adobe Pass Authentication 3.0发行说明](notes-releases/auth-rn-300.md)
      + [Adobe Pass Authentication 2.70发行说明](notes-releases/auth-rn-270.md)
      + [Adobe Pass Authentication 2.69发行说明](notes-releases/auth-rn-269.md)
      + [Adobe Pass Authentication JavaScript 4.7.0发行说明](notes-releases/authn-rn-javascript-470.md)
      + [Adobe Pass Authentication iOS / tvOS 3.9.2发行说明](notes-releases/authn-rn-ios-tvos-392.md)
      + [Adobe Pass Authentication iOS / tvOS 3.8.4发行说明](notes-releases/authn-rn-ios-tvos-384.md)
   + 2023 {#2023}
      + [Adobe Pass Authentication 2.68发行说明](notes-releases/auth-rn-268.md)
      + [Adobe Pass Authentication 2.67发行说明](notes-releases/auth-rn-267.md)
      + [Adobe Pass Authentication 2.66发行说明](notes-releases/auth-rn-266.md)
      + [Adobe Pass Authentication 2.65.1发行说明](notes-releases/auth-rn-2651.md)
      + [Adobe Pass Authentication 2.65发行说明](notes-releases/auth-rn-265.md)
      + [Adobe Pass Authentication 2.64.1发行说明](notes-releases/auth-rn-2641.md)
      + [Adobe Pass Authentication iOS / tvOS 3.8.3发行说明](notes-releases/authn-rn-ios-tvos-383.md)
      + [Adobe Pass Authentication iOS / tvOS 3.8.2发行说明](notes-releases/authn-rn-ios-tvos-382.md)
      + [Adobe Pass Authentication iOS / tvOS 3.8.1发行说明](notes-releases/authn-rn-ios-tvos-381.md)
      + [Adobe Pass Authentication Android 3.7.3发行说明](notes-releases/authn-rn-android-373.md)
   + 2022 {#2022}
      + [Adobe Pass Authentication 2.64发行说明](notes-releases/auth-rn-264.md)
      + [Adobe Pass Authentication 2.63发行说明](notes-releases/auth-rn-263.md)
      + [Adobe Pass Authentication 2.62.1发行说明](notes-releases/auth-rn-2621.md)
      + [Adobe Pass Authentication JavaScript 4.6.0发行说明](notes-releases/authn-rn-javascript-460.md)
   + 2021 {#2021}
      + [Adobe Pass Authentication JavaScript 4.4.0发行说明](notes-releases/authn-rn-javascript-440.md)
      + [Adobe Pass Authentication iOS / tvOS 3.7.0发行说明](notes-releases/authn-rn-ios-tvos-370.md)
+ 启动{#kickstart}
   + [技术文件](kickstart/technical-paper.md)
   + [程序员概述](kickstart/programmer-overview.md)
   + [MVPD概述](kickstart/mvpd-overview.md)
   + [程序员kickstart指南](kickstart/programmer-kickstart-guide.md)
   + [MVPD快速入门指南](kickstart/mvpd-kickstart-guide.md)
   + [支持过程常见问题解答](kickstart/support-procedures-faqs.md)
+ 程序员集成指南{#integration-guide-programmers}
   + REST API {#rest-apis}
      + REST API DCR {#rest-api-dcr}
         + [Dynamic Client注册概述](integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
         + [Dynamic Client注册术语表](integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-glossary.md)
         + [动态客户端注册常见问题解答](integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md)
         + API {#rest-api-dcr-apis}
            + [检索客户端凭据](integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
            + [检索访问令牌](integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)
         + 流{#rest-api-dcr-flows}
            + [动态客户端注册流程](integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)
      + REST API V2 {#rest-api-v2}
         + [REST API V2概述](integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)
         + [REST API V2术语表](integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md)
         + [REST API V2常见问题解答](integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md)
         + API {#rest-api-v2-apis}
            + [REST API V2 API概述](integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)
            + 配置{#rest-api-v2-configuration-apis}
               + [检索特定服务提供商的配置](integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)
            + 会话{#rest-api-v2-sessions-apis}
               + [创建身份验证会话](integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
               + [恢复身份验证会话](integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
               + [检索身份验证会话](integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)
               + [在用户代理中执行身份验证](integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)
            + 配置文件{#rest-api-v2-profiles-apis}
               + [检索用户档案](integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
               + [检索特定mvpd的配置文件](integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
               + [检索特定代码的配置文件](integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
            + 决策{#rest-api-v2-decisions-apis}
               + [使用特定的mvpd检索授权决策](integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
               + [使用特定的mvpd检索预授权决策](integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
            + 注销{#rest-api-v2-logout-apis}
               + [启动特定mvpd的注销](integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)
            + 合作伙伴单点登录{#rest-api-v2-partner-single-sign-on-apis}
               + [检索合作伙伴身份验证请求](integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md)
               + [使用合作伙伴身份验证响应检索配置文件](integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md)
         + 流{#rest-api-v2-flows}
            + [REST API V2流概述](integration-guide-programmers/rest-apis/rest-api-v2/flows/rest-api-v2-flows-overview.md)
            + 基本访问流{#rest-api-v2-basic-access-flows}
               + [基本配置文件在主要应用程序中执行](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
               + [基本配置文件在辅助应用程序中执行](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)
               + [在主应用程序中执行的基本身份验证流程](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
               + [在辅助应用程序中执行的基本身份验证流程](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
               + [在主应用程序中执行的基本授权流程](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)
               + [在主应用程序中执行的基本预授权流程](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)
               + [在主应用程序中执行的基本注销流程](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)
            + 已降级的访问流{#rest-api-v2-degraded-access-flows}
               + [降级访问流](integration-guide-programmers/rest-apis/rest-api-v2/flows/degraded-access-flows/rest-api-v2-access-degraded-flows.md)
            + 临时访问流{#rest-api-v2-temporary-access-flows}
               + [临时访问流](integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)
            + 单点登录访问流程{#rest-api-v2-single-sign-on-access-flows}
               + [使用合作伙伴流程进行单点登录](integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md)
               + [使用平台身份流进行单点登录](integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md)
               + [使用服务令牌流进行单点登录](integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md)
               + [单个注销流程](integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-logout-flow.md)
         + 指南{#rest-api-v2-cookbooks}
            + [REST API V2指南（客户端到服务器）](integration-guide-programmers/rest-apis/rest-api-v2/cookbooks/rest-api-v2-cookbook-client-server.md)
            + [REST API V2指南（服务器到服务器）](integration-guide-programmers/rest-apis/rest-api-v2/cookbooks/rest-api-v2-cookbook-server-server.md)
         + 附录{#rest-api-v2-appendix}
            + 标头{#rest-api-v2-appendix-headers}
               + [标头 — 授权](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md)
               + [标头 — AP-Device-Identifier](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)
               + [标头 — X-Device-Info](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md)
               + [标头 — AD-Service-Token](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md)
               + [标头 — Adobe-Subject-Token](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md)
               + [标头 — AP-Partner-Framework-Status](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md)
               + [标头 — AP-TempPass-Identity](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md)
   + 标准功能{#standard-features}
      + 权利{#entitlements}
         + [决策](integration-guide-programmers/features-standard/entitlements/decisions.md)
         + [媒体令牌](integration-guide-programmers/features-standard/entitlements/media-tokens.md)
         + [用户元数据](integration-guide-programmers/features-standard/entitlements/user-metadata.md)
      + 报告{#error-reporting}时出错
         + [增强的错误代码](integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)
      + 单点登录访问{#sso-access}
         + 合作伙伴单点登录{#partner-sso}
            + Apple单点登录{#apple-sso}
               + [Apple SSO概述](integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-overview.md)
               + [Apple SSO指南(REST API V2)](integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-cookbook-rest-api-v2.md)
         + 平台单点登录{#platform-sso}
            + Amazon单点登录{#amazon-sso}
               + [Amazon SSO指南(REST API V2)](integration-guide-programmers/features-standard/sso-access/platform-sso/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v2.md)
            + Roku单点登录{#roku-sso}
               + [Roku SSO概述](integration-guide-programmers/features-standard/sso-access/platform-sso/roku-single-sign-on/roku-sso-overview.md)
      + 基于主页的身份验证访问{#hba-access}
         + [基于家庭的身份验证(HBA)](integration-guide-programmers/features-standard/hba-access/home-based-authentication.md)
      + 隐私支持{#privacy-support}
         + [隐私支持概述](integration-guide-programmers/features-premium/privacy-support/privacy-supp-overview.md)
         + [如何提出隐私请求](integration-guide-programmers/features-premium/privacy-support/make-privacy-req.md)
   + 高级功能{#features-premium}
      + 临时访问{#temporary-access}
         + [TempPass功能](integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md)
      + 已降级访问{#degraded-access}
         + [退化特征](integration-guide-programmers/features-premium/degraded-access/degradation-feature.md)
      + ESM {#esm}
         + [授权服务监控概述](integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md)
         + [授权服务监控API](integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-api.md)
         + [服务器端量度](integration-guide-programmers/features-premium/esm/understanding-serverside-metrics.md)
      + 分析{#analytics}
         + [将Adobe Pass身份验证服务器端数据集成到Adobe Analytics](integration-guide-programmers/features-premium/analytics/integrate-authn-servr-data-analytics.md)
         + [在Adobe Pass身份验证中使用Experience CloudID](integration-guide-programmers/features-premium/analytics/exp-cloud-id-authn.md)
   + 旧版{#legacy}
      + （旧版） REST API V1 {#rest-api-v1}
         + [（旧版）REST API V1概述](integration-guide-programmers/legacy/rest-api-v1/rest-api-overview.md)
         + [（旧版）REST API V1参考](integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
         + （旧版） API {#rest-api-v1-apis}
            + [（旧版）注册码请求](integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md)
            + [（旧版）退货注册记录](integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md)
            + [（旧版）删除注册记录](integration-guide-programmers/legacy/rest-api-v1/apis/delete-registration-record.md)
            + [（旧版）提供MVPD列表](integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md)
            + [（旧版）启动身份验证](integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md)
            + [（旧版）检查身份验证令牌](integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md)
            + [（旧版）检索身份验证令牌](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md)
            + [（旧版）启动授权](integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authorization.md)
            + [（旧版）检索授权令牌](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authorization-token.md)
            + [（旧版）获取短媒体令牌](integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md)
            + [（旧版）按第二屏Web应用程序检查身份验证流程](integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-flow-by-second-screen-web-app.md)
            + [（旧版）检索预授权资源的列表](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources.md)
            + [（旧版）按第二屏Web应用程序检索预授权资源列表](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md)
            + [（旧版）启动注销](integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md)
            + [（旧版）用户元数据](integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md)
            + [（旧版）Retrieve profile-request](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-profilerequest.md)
            + [（旧版）令牌交换](integration-guide-programmers/legacy/rest-api-v1/apis/token-exchange.md)
            + [（旧版）临时通行证和促销临时通行证的免费预览](integration-guide-programmers/legacy/rest-api-v1/apis/free-preview-for-temp-pass-and-promotional-temp-pass.md)
         + （旧版）指南{#rest-api-v1-cookbooks}
            + [（旧版）REST API V1指南（客户端到服务器）](integration-guide-programmers/legacy/rest-api-v1/cookbooks/rest-api-cookbook-clienttoserver.md)
            + [（旧版）REST API V1指南（服务器到服务器）](integration-guide-programmers/legacy/rest-api-v1/cookbooks/rest-api-cookbook-servertoserver.md)
      + （旧版） SDK {#sdks}
         + （旧版）JavaScript SDK {#javascript-sdk}
            + [（旧版）JavaScript SDK概述](integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-overview.md)
            + [（旧版）JavaScript SDK指南](integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-cookbook.md)
            + [（旧版）JavaScript SDK API参考](integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md)
            + [（旧版）JavaScript SDK API预授权](integration-guide-programmers/legacy/sdks/javascript-sdk/preauthorize-api-javascript-sdk.md)
            + （旧版）准则{#javascript-sdk-guidelines}
               + [（旧版）无刷新登录和注销](integration-guide-programmers/legacy/sdks/javascript-sdk/refreshless-login-and-logout.md)
         + （旧版）iOS/tvOS SDK {#ios-tvos-sdk}
            + [（旧版）iOS/tvOS SDK概述](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-overview.md)
            + [（旧版）iOS/tvOS SDK指南](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-cookbook.md)
            + [（旧版）iOS/tvOS SDK API参考](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)
            + [（旧版）iOS/tvOS SDK API预授权](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/preauthorize-api-ios-tvos-sdk.md)
            + （旧版）准则{#ios-tvos-sdk-guidelines}
               + [（旧版）iOS/tvOS应用程序注册](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-application-registration.md)
               + [（旧版） iOS/tvOS v3.x迁移指南](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-v3x-migration-guide.md)
               + [（旧版）iOS/tvOS存储完整性检查](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-storage-integrity-checks.md)
         + （旧版）Android SDK {#android-sdk}
            + [（旧版）Android SDK概述](integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-overview.md)
            + [（旧版）Android SDK指南](integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-cookbook.md)
            + [（旧版）Android SDK API参考](integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md)
            + [（旧版）Android SDK API预授权](integration-guide-programmers/legacy/sdks/android-sdk/preauthorize-api-android-sdk.md)
            + （旧版）准则{#android-sdk-guidelines}
               + [（旧版）Android应用程序注册](integration-guide-programmers/legacy/sdks/android-sdk/android-application-registration.md)
               + [（旧版）Android SDK，带有动态客户端注册功能](integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-with-dynamic-client-registration.md)
         + （旧版）FireOS SDK {#fireos-sdk}
            + [（旧版）Amazon FireOS技术概述](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-technical-overview.md)
            + [（旧版）Amazon FireOS集成指南](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-integration-cookbook.md)
            + [（旧版）Amazon FireOS API参考](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md)
            + [（旧版）Amazon FireOS应用程序注册](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-application-registration.md)
            + [（旧版）带有Dynamic Client注册的FireOS SDK](integration-guide-programmers/legacy/sdks/fireos-sdk/fireos-sdk-with-dynamic-client-registration.md)
            + [（旧版）Amazon FireOS SSO — 程序员启动指南](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-firetv-sso-programmer-kickoff-guide.md)
      + （旧版）客户端信息{#client-information}
         + [（旧版）传递客户端信息（设备、连接和应用程序）](integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md)
      + （旧版）错误报告{#error-reporting}
         + [（旧版）错误报告](integration-guide-programmers/legacy/error-reporting/error-reporting.md)
      + （旧版）单点登录访问{#sso-access}
         + [（旧版）单点登录支持](integration-guide-programmers/legacy/sso-access/sso-support.md)
         + [（旧版）通过被动身份验证的SSO](integration-guide-programmers/legacy/sso-access/sso-passive-authn.md)
         + [（旧版）Amazon SSO指南(REST API V1)](integration-guide-programmers/legacy/sso-access/amazon-sso-cookbook-rest-api-v1.md)
         + [（旧版）Apple SSO指南(REST API V1)](integration-guide-programmers/legacy/sso-access/apple-sso-cookbook-rest-api-v1.md)
         + [（旧版）Apple SSO指南(iOS/tvOS SDK)](integration-guide-programmers/legacy/sso-access/apple-sso-cookbook-iostvos-sdk.md)
      + （旧版） TVE仪表板{#tve-dashboard}
         + [（旧版）TVE功能板用户指南](integration-guide-programmers/legacy/tve-dashboard/tve-dashboard-user-guide.md)
      + （旧版）技术说明{#tech-notes}
         + （旧版） REST API V1 {#rest-api-v1}
            + [（旧版）无客户端API实施 — 错误代码/包含可能原因/原因的消息](integration-guide-programmers/legacy/notes-technical/clientless-api-implementation-error-codes-messages-with-probable-reason-cause.md)
            + [（旧版）缺少设备ID时的无客户端API流](integration-guide-programmers/legacy/notes-technical/clientless-api-flow-in-the-absence-of-device-id.md)
            + [（旧版）无客户端：避免在/authenticate请求中使用&#39;&amp;&#39;reg_code](integration-guide-programmers/legacy/notes-technical/clientless-avoid-using-reg-code-in-authenticate-request.md)
            + [（旧版）在Xbox 360和XboxOne上为程序员启用Adobe Pass授权服务（无客户端）](integration-guide-programmers/legacy/notes-technical/enabling-primetime-entitlement-services-for-a-programmer-on-xbox-360-and-xboxone-clientless-solution.md)
            + [（旧版）无客户端设备类型和量度](integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)
         + （旧版） SDK {#sdks}
            + [（旧版）证书常见问题解答](integration-guide-programmers/legacy/notes-technical/certificates-qa.md)
            + [（旧版）了解用户ID](integration-guide-programmers/legacy/notes-technical/understanding-user-ids.md)
            + （旧版）JavaScript SDK {#javascript-sdk}
               + [（旧版）跟踪预防评估 — Apple Safari](integration-guide-programmers/legacy/notes-technical/tracking-prevention-assessment-apple-safari.md)
               + [（旧版）跟踪预防评估 — Google Chrome](integration-guide-programmers/legacy/notes-technical/tracking-prevention-assessment-google-chrome.md)
               + [（旧版）Cookie更新 — SameSite和Secure标记](integration-guide-programmers/legacy/notes-technical/cookies-updates-samesite-and-secure-flags.md)
               + [（旧版）调试提示](integration-guide-programmers/legacy/notes-technical/appendix-b-debugging-tips.md)
            + （旧版）Android SDK {#android-sdk}
               + [（旧版）Android 10应用程序上的Access Enabler Android SDK单点登录(SSO)](integration-guide-programmers/legacy/notes-technical/access-enabler-android-sdk-single-signon-sso-on-android-10-devices.md)
               + [（旧版）Adobe Pass身份验证和Android 6“Marshmallow”新权限模型](integration-guide-programmers/legacy/notes-technical/adobe-primetime-authentication-and-the-android-6-marshmallow-new-permissions-model.md)
            + （旧版）iOS/tvOS SDK {#ios-tvos-sdk}
               + [（旧版）iOS SDK 3.1+上的WKWebView支持](integration-guide-programmers/legacy/notes-technical/wkwebview-support-on-ios-sdk-31.md)
               + [（旧版）iOS SDK 3.2+上的SFSafariViewController支持](integration-guide-programmers/legacy/notes-technical/sfsafariviewcontroller-support-on-ios-sdk-32.md)
               + [（旧版）使用iOS Authentication Access Enabler时Adobe Pass上的SSO](integration-guide-programmers/legacy/notes-technical/sso-on-ios-when-using-the-primetime-authentication-access-enabler.md)
               + [（旧版）iOS身份验证错误 — 找不到adobepass.ios.app](integration-guide-programmers/legacy/notes-technical/ios-authentication-error-adobepassiosapp-cannot-be-found.md)
               + [（旧版）在iOS上重置临时传递](integration-guide-programmers/legacy/notes-technical/reset-temp-pass-on-ios.md)
               + [（旧版）使用控制台应用程序日志调试AccessEnabler iOS/tvOS SDK](integration-guide-programmers/legacy/notes-technical/debugging-the-accessenabler-iostvos-sdk-using-console-app-logs.md)
               + [（旧版） AccessEnabler iOS/tvOS 3.7.0升级路径](integration-guide-programmers/legacy/notes-technical/accessenabler-iostvos-370-upgrade-path.md)
         + （旧版）用户体验{#user-experience}
            + [（旧版）如何将MVPD登录页面从iFrame迁移到弹出窗口](integration-guide-programmers/legacy/notes-technical/migr-mvpd-login-iframe-popup.md)
            + [（旧版）印前检查功能：如何启用、排除或确定问题](integration-guide-programmers/legacy/notes-technical/preflight-feature.md)
            + [（旧版）在选择对话框中允许MVPD](integration-guide-programmers/legacy/notes-technical/allow-mvpd-selectn-dialog.md)
            + [（旧版）阻止MVPD显示选择对话框](integration-guide-programmers/legacy/notes-technical/prevent-mvpd-selectn-dialog.md)
         + （旧版）疑难解答{#troubleshooting}
            + [（旧版）使用Charles代理](integration-guide-programmers/legacy/notes-technical/using-charles-proxy.md)
            + [（旧版）监控Adobe PassAdobePayTV密码](integration-guide-programmers/legacy/notes-technical/monitoring-adobe-pay-tv-pass.md)
            + [（旧版）如何使用AdobeAPI测试站点测试身份验证和授权流](integration-guide-programmers/legacy/notes-technical/test-authn-authz-flows-using-adobes-api-test-site.md)
   + [程序员集成指南概述](integration-guide-programmers/programmer-integration-guide-overview.md)
   + [程序员权利流](integration-guide-programmers/entitlement-flow.md)
   + [程序员用例](integration-guide-programmers/programmer-use-cases.md)
   + [节流机构](integration-guide-programmers/throttling-mechanism.md)
   + [最低系统要求](integration-guide-programmers/minimum-system-requirements.md)
+ MVPD的集成指南{#integration-guide-mvpds}
   + [集成功能](integration-guide-mvpds/mvpd-integr-features.md)
   + [身份验证](integration-guide-mvpds/authn-usecase.md)
   + [使用OAuth 2.0协议进行身份验证](integration-guide-mvpds/authn-oauth2-protocol.md)
   + [授权](integration-guide-mvpds/authz-usecase.md)
   + [印前检查授权](integration-guide-mvpds/mvpd-preflight-authz.md)
   + [MVPD注销](integration-guide-mvpds/usecase-mvpd-logout.md)
   + [内容元数据交换](integration-guide-mvpds/mvpd-content-metadata-exchange.md)
   + [用户元数据交换](integration-guide-mvpds/mvpd-user-metadata-exchng.md)
   + [代理MVPD Web服务](integration-guide-mvpds/proxy-mvpd-webserv.md)
   + [代理MVPD SAML集成](integration-guide-mvpds/proxy-mvpd-saml-int.md)
   + [服务提供商范围](integration-guide-mvpds/serv-provider-scoping.md)
   + [MVPD允许IP地址](integration-guide-mvpds/mvpd-listing-ip-addres.md)
+ TVE仪表板{#user-guide-tve-dashboard}用户指南
   + [TVE仪表板概述](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md)
   + [环境](/help/authentication/user-guide-tve-dashboard/tve-dashboard-environments.md)
   + [审核和推送更改](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md)
   + [仪表板](/help/authentication/user-guide-tve-dashboard/tve-dashboard-home.md)
   + [渠道](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md)
   + [程序员](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md)
   + [MVPDs](/help/authentication/user-guide-tve-dashboard/tve-dashboard-mvpds.md)
   + [集成](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md)
   + [报告](/help/authentication/user-guide-tve-dashboard/tve-dashboard-reports.md)
   + [更改日志](/help/authentication/user-guide-tve-dashboard/tve-dashboard-changes-log.md)
+ 技术说明{#tech-notes}
   + 环境{#environments}
      + [了解Adobe环境](notes-technical/environments/understanding-the-adobe-environments.md)
      + [在资格预审中设置环境和测试](notes-technical/environments/setting-up-your-environment-and-testing-in-prequal.md)
