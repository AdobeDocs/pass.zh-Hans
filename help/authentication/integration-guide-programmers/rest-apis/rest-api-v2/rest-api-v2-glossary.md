---
title: REST API V2术语表
description: REST API V2术语表
exl-id: 8b3bd2de-1ff8-4c57-b18d-27ecdf2b0de2
source-git-commit: e448427ae4a36c4c6cb9f9c1cb4d0cc5c6d564ed
workflow-type: tm+mt
source-wordcount: '1742'
ht-degree: 0%

---

# REST API V2术语表 {#rest-api-v2-glossary}

>[!IMPORTANT]
>
> 此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

本文档提供集成Adobe Pass身份验证REST API V2时使用的术语的定义。

>[!MORELIKETHIS]
>
> * [动态客户端注册(DCR)术语表](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-glossary.md)

## 术语表 {#glossary-terms}

### A {#a}

#### 身份验证 {#authentication}

身份验证是一个进程，它允许用户在[MVPD](#programmer)验证用户订阅后，向[程序员](#resource)证明其身份，以获得对受保护内容（[资源](#mvpd)）的访问权限。

#### 身份验证代码 {#code}

身份验证代码是一个Adobe Pass身份验证概念，它存储当用户启动[身份验证](#authentication)进程时生成的唯一值，并在身份验证进程完成之前唯一标识用户的[身份验证会话](#session)。

身份验证代码可由[主要（程序员）应用程序](#primary-application)或[辅助（程序员）应用程序](#secondary-application)使用，以便完成[身份验证](#authentication)进程、检索有关[身份验证会话](#session)的信息，或访问用户[配置文件](#profile)。

与以前术语使用的注册码同义。

#### 身份验证会话 {#session}

身份验证会话是一个Adobe Pass身份验证概念，用于存储有关从[程序员](#programmer)应用程序启动（或继续）的用户身份验证过程的信息，并且由[身份验证代码](#code)唯一标识。

身份验证会话还可以指示[程序员](#programmer)应用程序继续进行[授权](#authorization)进程，作为[权利](#entitlement)流程的下一个阶段，以防用户已经过身份验证。

#### 授权 {#authorization}

授权是一个进程，它允许用户在通过[MVPD](#resource)验证用户权限后，基于拥有的[MVPD](#programmer)订阅，从[程序员](#mvpd)目录访问受保护的内容（[资源](#mvpd)）。

### C {#c}

#### 配置 {#configuration}

该配置是一个Adobe Pass身份验证概念，它存储有关[程序员](#programmer)和[MVPD](#mvpd)集成设置的信息，在[身份验证](#authentication)过程中询问用户从活动集成列表中选择其[电视提供程序](#tv-provider)时可以使用。

### D {#d}

#### 决策 {#decision}

决策是一个Adobe Pass身份验证概念，用于存储有关[MVPD](#mvpd) [授权](#authorization)或[预授权](#preauthorization)进程查询的信息，以允许或拒绝用户访问[程序员](#programmer)受保护的内容。

#### 降解 {#degradation}

降级是一种Adobe Pass身份验证功能，它允许用户访问受保护的内容，即使其[MVPD](#mvpd)遇到服务中断也是如此。

有关详细信息，请参阅[降级功能](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md)文档。

#### 设备ID {#device-id}

设备ID是绑定到用户设备的唯一标识符，必须由[程序员](#programmer)应用程序在[权利](#entitlement)流的所有阶段提供。

### E {#e}

#### 权利 {#entitlement}

权利是一个Adobe Pass身份验证概念，它整合了帮助用户通过不同阶段的可用流程和功能，以访问受保护的内容，这些阶段包括[身份验证](#authentication)、[预授权](#preauthorization)、[授权](#authorization)，最后是[注销](#logout)。

#### 增强的错误代码 {#enhanced-error-code}

增强的错误代码是一个Adobe Pass身份验证概念，它提供有关处理请求时发生的错误的其他信息。

有关详细信息，请参阅[增强型错误代码](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)文档。

### H {#h}

#### HBA {#hba}

基于家庭的身份验证(HBA)是一个过程，通过该过程，消费者将被自动授予访问连接到其家庭网络（属于订阅合同内的位置）的选定设备上的[TV Everywhere (TVE)](#tve)内容的权限。

### I {#i}

#### 身份提供者 {#identity-provider}

身份提供程序是一家公司，在[TV Everywhere (TVE)](#tve)的上下文中通过有线电视、卫星或基于互联网的服务向消费者提供身份服务。

与[MVPD](#mvpd)和[电视提供程序](#tv-provider)同义。

### L {#l}

#### 注销 {#logout}

注销是一个进程，它允许用户在Adobe Pass身份验证中终止其经过身份验证的[配置文件](#profile)，并更新[程序员](#programmer)应用程序以反映用户的状态。

### M {#m}

#### 媒体令牌 {#media-token}

媒体令牌是Adobe Pass身份验证生成的令牌，它是授权[决定](#decision)的结果，该授权旨在提供对受保护内容的访问权限。

媒体令牌已传递给[程序员](#programmer)，程序员随后验证该令牌以确保该[资源](#resource)的访问安全。

与使用短授权令牌的前一术语同义。

#### 媒体令牌验证器 {#media-token-verifier}

媒体令牌验证器是由Adobe Pass身份验证分发的库，负责验证[媒体令牌](#media-token)的真实性。

有关详细信息，请参阅[媒体令牌验证器](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier)文档。

#### MVPD {#mvpd}

多频道视频节目分销商(MVPD)是一家通过有线电视、卫星电视或互联网服务为消费者提供电视服务的公司。

MVPD由MVPD和Adobe在新用户引导过程中定义的唯一值来标识。

与[电视提供程序](#tv-provider)和[身份提供程序](#identity-provider)同义。

### P {#p}

#### 合作伙伴 {#partner}

合作伙伴是向[程序员](#programmer)提供服务或框架以启用单点登录用户体验的公司。

合作伙伴由唯一值（例如“apple”）标识，该唯一值在合作伙伴和Adobe之间的载入流程中定义。

#### 预授权 {#preauthorization}

预授权是一个进程，允许用户在[MVPD](#resource)验证用户权限后，从[程序员](#programmer)目录预览他们有权访问的[资源](#mvpd)的子集。

与[Preflight](#preflight)同义。

#### Preflight {#preflight}

预检是允许用户在[MVPD](#resource)验证用户权限后，从[程序员](#programmer)目录预览他们有权访问的[资源](#mvpd)的子集的进程。

与[预授权](#preauthorization)同义。

#### 主要（程序员）应用程序 {#primary-application}

主应用程序引用了一个启动[身份验证](#programmer)的[程序员](#authentication)应用程序，但该应用程序可能无法通过使用[用户代理](#user-agent)导航到[MVPD](#mvpd)登录页来完成该进程。

#### 个人资料 {#profile}

配置文件是一个Adobe Pass身份验证概念，用于存储有关用户身份验证开始日期和结束日期、[用户的元数据](#user-metadata)的信息以及指示获取身份验证方法的其他字段（例如，“常规”、“已降级”、“临时”、“单点登录”等）。

与以前术语使用的身份验证令牌同义。

#### 程序员 {#programmer}

The Programmer是一家通过各种平台拥有的渠道（品牌）向消费者提供内容的公司。

在与Adobe Pass身份验证的集成中，程序员将多个拥有的渠道（品牌）分组为[服务提供商](#service-provider)。

#### 代理MVPD {#proxy-mvpd}

代理MVPD是一家为其他MVPD提供身份服务并直接与Adobe Pass身份验证集成的公司。

#### 代理的MVPD {#proxied-mvpd}

代理的MVPD是一家与Adobe Pass身份验证没有直接集成，但通过[代理MVPD](#proxy-mvpd)进行集成的公司。

#### 平台标识 {#platform-identity}

平台标识是由绑定到用户设备的服务或框架（库）生成的唯一平台标识符有效负荷，提供给[程序员](#programmer)以启用单点登录用户体验。

有关详细信息，请参阅[使用平台标识流的单点登录](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md)文档。

### R {#r}

#### 资源 {#resource}

资源是用户尝试从[程序员](#programmer)目录访问的受保护内容。

资源由程序员和MVPD之间商定的唯一值标识。

有关详细信息，请参阅[受保护的资源](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#protected-resources)文档。

### S {#s}

#### SAML {#saml}

安全断言标记语言(SAML)是一种开放标准，用于在各方之间交换身份验证和授权数据，特别是[身份提供程序](#identity-provider)和[服务提供程序](#sp)之间的身份验证和授权数据。

#### 辅助（程序员）应用程序 {#secondary-application}

辅助应用程序是指能够使用[用户代理](#programmer)导航到[MVPD](#authentication)登录页来完成[身份验证](#user-agent)进程的[程序员](#mvpd)应用程序。

辅助应用程序可以在与主应用程序相同的设备上运行，或者在其他（辅助）设备上运行，在这种情况下，登录体验称为“第二屏幕验证”用户体验。

#### 服务令牌 {#service-token}

服务令牌是由绑定到用户的服务或框架（库）生成的唯一用户标识符，提供给[程序员](#programmer)以启用单点登录用户体验。

有关详细信息，请参阅[使用服务令牌流的单点登录](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md)文档。

#### 服务提供商 {#service-provider}

服务提供商是[程序员](#programmer)拥有的渠道（品牌）。

服务提供商由程序员和Adobe在新用户引导过程中定义的唯一值来标识。

与使用请求者ID的前一个术语同义。

#### SLO {#slo}

单一登出(SLO)是一个允许用户从属于[单一登入(SSO)](#sso)的所有应用程序登出的过程。

#### SP {#sp}

服务提供商(SP)是指在与[MVPD](#programmer)的集成中，Adobe Pass身份验证代表[程序员](#mvpd)发挥的作用。

#### SSO {#sso}

单点登录(SSO)是一个进程，允许用户验证一次，并获取对多个[程序员](#programmer)应用程序的访问权限，而无需为每个应用程序验证。

### T {#t}

#### TempPass基本 {#temp-pass-basic}

基本的TempPass是Adobe Pass身份验证功能，它允许用户在有限的时间内访问受保护的内容，而无需使用[MVPD](#mvpd)进行身份验证。

有关详细信息，请参阅[基本TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#basic-temp-pass)文档。

#### TempPass提升 {#temp-pass-promotional}

提升TempPass是一种Adobe Pass身份验证功能，它允许用户在最大数量的资源和有限的时间内访问受保护的内容，而无需使用[MVPD](#mvpd)进行身份验证。

有关详细信息，请参阅[促销临时传递](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#promotional-temp-pass)文档。

#### TTL {#ttl}

生存时间(TTL)是一个值，它指示基础实体有效的时间量。

可以为[访问令牌](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-glossary.md#access-token)、[配置文件](#profile)、授权[决策](#decision)或[媒体令牌](#media-token)提及TTL。

#### TVE {#tve}

TV Everywhere (TVE)是一个产业细分市场，它允许消费者在多台设备（如智能手机、平板电脑、笔记本电脑等）上观看他们最喜爱的电视节目、电影和其他内容。

#### 动态仪表板 {#tve-dashboard}

TV Everywhere (TVE) Dashboard是提供给[程序员](#programmer)的Adobe Pass身份验证工具，用于管理其配置和数据。

有关详细信息，请参阅[TVE仪表板用户指南](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md)文档。

#### 电视提供商 {#tv-provider}

电视提供商是一家公司，通过有线电视、卫星电视或互联网服务为消费者提供电视服务。

根据电视提供商和Adobe在新用户引导过程中定义的唯一值，来识别电视提供商。

与[MVPD](#mvpd)和[身份提供程序](#identity-provider)同义。

### U {#u}

#### 用户代理 {#user-agent}

用户代理是指能够导航Web并呈现[MVPD](#mvpd)登录页面的浏览器或类似组件（特定于平台）。

#### 用户 ID {#user-id}

用户ID是绑定到用户的唯一标识符，源自[MVPD](#mvpd)身份验证过程。

#### 用户元数据 {#user-metadata}

用户元数据是指由[MVPD](#mvpd)维护并由Adobe Pass身份验证作为[配置文件](#profile)的一部分提供的用户特定属性（例如，邮政编码、家长评级、用户ID等）。

有关详细信息，请参阅[用户元数据](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)文档。

### V {#v}

#### VSA {#vsa}

视频订阅者帐户(VSA)是提供给[程序员](#programmer)的Apple开发的框架，用于启用单点登录用户体验。

有关详细信息，请参阅[视频订阅者帐户框架](https://developer.apple.com/documentation/videosubscriberaccount)和[使用合作伙伴流的单点登录](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md)文档。
