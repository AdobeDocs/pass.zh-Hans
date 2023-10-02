---
product: adobe primetime
audience: end-user
feature: Authentication
user-guide-title: Adobe Pass 身份验证
user-guide-description: Adobe Pass 身份验证是一个适用于 TV Everywhere 的授权解决方案，它提供一个模块化框架，以供确定请求访问资源的人员是否有权访问该资源。
source-git-commit: fd65582522d6b40e5797dff192e2340c5575ef3b
workflow-type: tm+mt
source-wordcount: '776'
ht-degree: 4%

---


# Adobe Pass 身份验证 帮助 {#authentication}

+ [Adobe Pass身份验证概述](home.md)
+ Adobe Pass身份验证概念 {#authentication-concepts}
   + [技术文件](technical-paper.md)
   + [面向程序员的概述](programmer-overview.md)
   + [MVPD概述](mvpd-overview.md)
+ 快速入门指南 {#kickstart-guides}
   + [程序员kickstart指南](programmer-kickstart-guide.md)
   + [MVPD快速入门指南](mvpd-kickstart-guide.md)
+ 程序员集成指南 {#programmer-integration-guide}
   + [程序员集成指南概述](programmer-integration-guide-overview.md)
   + [程序员权利流](entitlement-flow.md)
   + [程序员用例](programmer-use-cases.md)
   + [传递客户端信息（设备、连接和应用程序）](passing-client-information-device-connection-and-application.md)
   + REST API {#restapi}
      + [REST API概述](rest-api-overview.md)
      + [REST API指南（服务器到服务器）](rest-api-cookbook-servertoserver.md)
      + [REST API指南（客户端到服务器）](rest-api-cookbook-clienttoserver.md)
      + Rest API参考 {#rest-api-reference}
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
   + AccessEnabler SDK {#accessenabler-sdk}
      + JavaScript SDK {#javascriptsdk}
         + [JavaScript SDK概述](javascript-sdk-overview.md)
         + [JavaScript SDK指南](javascript-sdk-cookbook.md)
         + [JavaScript SDK API参考](javascript-sdk-api-reference.md)
         + 准则 {#js-sdk-guidelines}
            + [无刷新登录和注销](refreshless-login-and-logout.md)
         + JavaScript API {#js-api}
            + [预授权](js-preauthorize.md)
      + iOS/tvOS SDK {#ios-sdk}
         + [iOS/tvOS SDK概述](iostvos-sdk-overview.md)
         + [iOS/tvOS SDK指南](iostvos-sdk-cookbook.md)
         + [iOS/tvOS SDK API参考](iostvos-sdk-api-reference.md)
         + 准则 {#ios-tvos-sdk-guidelines}
            + [iOS/tvOS应用程序注册](iostvos-application-registration.md)
            + 迁移准则 {#migration-guidelines}
               + [iOS/tvOS v3.x迁移指南](iostvos-v3x-migration-guide.md)
         + iOS/tvOS API {#ios-tvos-api}
            + [预授权](preauthorize.md)
      + Android SDK {#androidsdk}
         + [Android SDK概述](android-sdk-overview.md)
         + [Android SDK指南](android-sdk-cookbook.md)
         + [Android SDK API参考](android-sdk-api-reference.md)
         + 准则 {#androidguidelines}
            + [Android应用程序注册](android-application-registration.md)
            + [带有动态客户端注册的Android SDK](android-sdk-with-dynamic-client-registration.md)
         + Android API{#androidapi}
            + [预授权](preauthorize-android.md)
      + Amazon FireOS SDK {#fireossdk}
         + [Amazon FireOS SSO — 程序员启动指南](amazon-firetv-sso-programmer-kickoff-guide.md)
         + [使用无客户端API指南的Amazon FireOS SSO](amazon-fireos-sso-using-clientless-api-cookbook.md)
         + [Amazon FireOS技术概述](amazon-fireos-technical-overview.md)
         + [Amazon FireOS集成指南](amazon-fireos-integration-cookbook.md)
         + [Amazon FireOS API参考](amazon-fireos-native-client-api-reference.md)
         + [Amazon FireOS应用程序注册](amazon-fireos-application-registration.md)
         + [带有动态客户端注册的FireOS SDK](fireos-sdk-with-dynamic-client-registration.md)
   + 平台SSO {#platform-sso}
      + APPLE SSO {#apple-sso}
         + [Apple SSO概述](apple-sso-overview.md)
         + [Apple SSO指南(REST API)](apple-sso-cookbook-rest-api.md)
         + [Apple SSO指南(iOS/tvOS SDK)](apple-sso-cookbook-iostvos-sdk.md)
      + Roku SSO {#roku-sso}
         + [Roku SSO](roku-sso-overview.md)
   + 内容元数据 {#content-metadata}
      + [标识受保护的资源](identify-protected-resources.md)
   + Content server集成 {#content-serv-int}
      + [如何集成媒体令牌验证器](media-token-verifier-int.md)
   + 附录 {#appendices}
      + [调试提示](appendix-b-debugging-tips.md)
+ MVPD集成指南 {#mvpd-int-guide}
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
+ Adobe Pass身份验证功能 {#auth-features}
   + Adobe Analytics集成 {#analytics-int}
      + [将Adobe Pass身份验证服务器端数据集成到Adobe Analytics](integrate-authn-servr-data-analytics.md)
      + [在Adobe Pass身份验证中使用Experience CloudID](exp-cloud-id-authn.md)
   + 授权服务监控 {#entitlement-service-monitoring}
      + [授权服务监控概述](entitlement-service-monitoring-overview.md)
      + [授权服务监控API](entitlement-service-monitoring-api.md)
      + [服务器端量度](understanding-serverside-metrics.md)
   + 临时传递 {#temp-pass}
      + [临时传递](temp-pass.md)
      + [促销临时通票](promotional-temp-pass.md)
   + 单点登录 {#sso}
      + [单点登录支持](sso-support.md)
      + [通过被动身份验证的SSO](sso-passive-authn.md)
   + 基于主目录的身份验证 {#home-based-auth}
      + [适用于所有地区的电视的家庭身份验证](home-based-authn-tve.md)
      + [MVPD的HBA状态](hba-status-mvpds.md)
   + 用户元数据 {#user-metadat}
      + [用户元数据](user-metadata-feature.md)
   + [印前检查授权](preflight-authz.md)
   + 错误报告 {#error-reportn}
      + [错误报告](error-reporting.md)
      + [增强的错误代码](enhanced-error-codes.md)
   + 客户端注册 {#client-regn}
      + [动态客户端注册](dynamic-client-registration.md)
      + [动态客户端注册API](dynamic-client-registration-api.md)
      + [动态客户端注册管理](dynamic-client-registration-management.md)
   + 降解服务 {#degrn-service}
      + [降级API概述](degradation-api-overview.md)
   + 隐私就绪 {#privacy-readiness}
      + [隐私支持概述](privacy-supp-overview.md)
      + [如何提出隐私请求](make-privacy-req.md)
+ 提示和疑难解答 {#tips-troubleshoot}
   + [在选择对话框中允许MVPD](allow-mvpd-selectn-dialog.md)
   + [阻止MVPD显示“选择”对话框](prevent-mvpd-selectn-dialog.md)
+ 支持 {#support}
   + [上报程序](escalation-procedures.md)
   + [监控Adobe PassAdobePayTV Pass](monitoring-adobe-pay-tv-pass.md)
   + [最低系统要求](minimum-system-requirements.md)
+ 发行说明 {#release-notes}
   + [Adobe Pass Authentication 2.67发行说明](auth-rn-267.md)
   + [Adobe Pass Authentication 2.66发行说明](auth-rn-266.md)
   + [Adobe Pass Authentication 2.65.1发行说明](auth-rn-2651.md)
   + [Adobe Pass Authentication 2.65发行说明](auth-rn-265.md)
   + [Adobe Pass Authentication 2.64.1发行说明](auth-rn-2641.md)
   + [Adobe Pass Authentication 2.64发行说明](auth-rn-264.md)
   + [Adobe Pass Authentication 2.63发行说明](auth-rn-263.md)
   + [Adobe Pass Authentication 2.62.1发行说明](auth-rn-2621.md)
   + [Adobe Pass Authentication iOS / tvOS 3.7.0发行说明](authn-rn-ios-tvos-370.md)
   + [Adobe Pass Authentication iOS / tvOS 3.8.1发行说明](authn-rn-ios-tvos-381.md)
   + [Adobe Pass Authentication iOS / tvOS 3.8.2发行说明](authn-rn-ios-tvos-382.md)
   + [Adobe Pass Authentication Android 3.7.3发行说明](authn-rn-android-373.md)
+ 技术说明 {#tech-notes}
   + Adobe Pass身份验证SDK {#primetime-authentication-sdks}
      + [证书常见问题解答](certificates-qa.md)
      + JavaScript SDK {#javascript}
         + [适用于Safari浏览器的JS SDK限制](js-sdk-limitations-for-safari-browser.md)
         + [Cookie更新 — SameSite和Secure标记](cookies-updates--samesite-and-secure-flags.md)
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
   + 通过身份验证环境 {#primetime-authentication-environments}
      + [了解Adobe环境](understanding-the-adobe-environments.md)
      + [在资格预审中设置环境和测试](setting-up-your-environment-and-testing-in-prequal.md)
      + [如何使用AdobeAPI测试站点测试身份验证和授权流](test-authn-authz-flows-using-adobes-api-test-site.md)
   + 无客户端API {#clientless-api}
      + [无客户端API实施 — 错误代码/包含可能原因/原因的消息](clientless-api-implementation-error-codes--messages-with-probable-reason--cause.md)
      + [缺少设备ID时的无客户端API流](clientless-api-flow-in-the-absence-of-device-id.md)
      + [无客户端：避免在/authenticate请求中使用&#39;&amp;&#39;reg_code](clientless-avoid-using-reg-code-in-authenticate-request.md)
      + [为Xbox 360和XboxOne无客户端程序员启用Adobe Pass授权服务](enabling-primetime-entitlement-services-for-a-programmer-on-xbox-360-and-xboxone-clientless-solution.md)
      + [无客户端设备类型和量度](benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)
   + 用户体验 {#user-exp}
      + [如何将MVPD登录页面从iFrame迁移到弹出窗口](migr-mvpd-login-iframe-popup.md)
      + [印前检查功能：如何启用、排除故障或确定问题](preflight-feature.md)
   + 工具和实用程序 {#tools-and-utilities}
      + [使用Charles代理](using-charles-proxy.md)
   + 概念 {#concepts}
      + [了解用户ID](understanding-user-ids.md)
+ [TVE仪表板用户指南](tve-dashboard-user-guide.md)
+ [术语表](glossary.md)
