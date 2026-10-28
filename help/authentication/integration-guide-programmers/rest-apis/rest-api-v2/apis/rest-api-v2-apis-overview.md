---
title: REST API V2 API概述
description: REST API V2 API概述
exl-id: 62b48bf0-d200-4949-b268-8f8ea2daabfa
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 297578d3b311fbc2a5702f1bcc947e94463fb884
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 0%
---
# REST API V2 API概述 {#rest-api-v2-apis-overview}

>[!IMPORTANT]
>
> 确保随时了解汇总在[产品公告](/help/authentication/product-announcements.md)页中的最新Adobe Pass身份验证产品公告和停用时间表。

要开始配置，请访问以下列表中您感兴趣的API的公共文档。

## 基本URL {#rest-api-v2-apis-base-urls}

请使用下面特定于环境的基本URL来处理REST API V2请求。 将每个API记录的相对`path`附加到此基本URL。

某些API响应会返回交互式URL，流设备必须在浏览器或用户代理中打开这些URL。 示例包括[Initiate logout为特定mvpd](logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)返回的`url`属性，以及[在用户代理](sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)中执行身份验证返回的`Location`重定向。

这些交互式URL使用相同的Adobe Pass服务提供程序(SP)主机。 完全按照返回内容打开一个绝对的交互式URL。 如果Adobe Pass返回以`/adobe-services/`开头的相对交互式URL，请针对目标环境的SP主机解析该URL：

| 环境 | REST API V2和交互式基本URL |
| --- |--------------------------------------|
| 生产 | `https://sp.auth.adobe.com` |
| 暂存 | `https://sp.auth-staging.adobe.com` |

## 配置 {#rest-api-v2-apis-configuration}

* [检索特定服务提供商的配置](configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)

## 会话 {#rest-api-v2-apis-sessions}

* [创建身份验证会话](sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [恢复身份验证会话](sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [检索身份验证会话](sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)
* [在用户代理中执行身份验证](sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)

## 轮廓 {#rest-api-v2-apis-profiles}

* [检索用户档案](profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [检索特定mvpd的配置文件](profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [检索特定代码的配置文件](profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

## 决策 {#rest-api-v2-apis-decisions}

* [使用特定的mvpd检索授权决策](decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [使用特定的mvpd检索预授权决策](decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)

## 注销 {#rest-api-v2-apis-logout}

* [启动特定mvpd的注销](logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)

## 合作伙伴单点登录 {#rest-api-v2-apis-partner-single-sign-on}

* [检索合作伙伴身份验证请求](partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md)
* [使用合作伙伴身份验证响应创建和检索配置文件](partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md)

## 想要尝试REST API V2吗？

您现在可以通过[Adobe Developer](https://developer.adobe.com/adobe-pass/)网站中的产品专用页面浏览REST API V2。
