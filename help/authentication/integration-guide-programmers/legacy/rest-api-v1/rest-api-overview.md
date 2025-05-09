---
title: REST API概述
description: Rest API概述
exl-id: 5533d852-f644-417e-bf80-6f7aa1edd6b2
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '1635'
ht-degree: 0%

---

# （旧版）REST API概述 {#rest-api-overview}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

>[!IMPORTANT]
>
> 确保随时了解汇总在[产品公告](/help/authentication/product-announcements.md)页中的最新Adobe Pass身份验证产品公告和停用时间表。

## 概述 {#over}

Adobe Pass身份验证REST API允许直接访问TV Everywhere (TVE)身份验证和授权服务。 此API支持两种主要体系结构：服务器到服务器或连接的设备（如游戏机、智能电视、机顶盒等）应用程序，这些应用程序没有Web浏览功能。

### 节流机构

Adobe Pass身份验证REST API受[限制机制](/help/authentication/integration-guide-programmers/throttling-mechanism.md)控制。


### 服务器到服务器

服务器到服务器解决方案涉及与程序员服务集成的程序员客户端应用程序，这些服务与Adobe Pass身份验证服务连接以实现TVE流。 这种方法将大多数TVE实施从客户端转移到服务器，在服务器中，可以构建和维护单个统一的授权模块。 客户端应用程序的主要剩余职责是管理用于用户身份验证的Web视图。



### 连接的设备

Connected Devices应用程序通过REST API直接与Adobe Pass身份验证进行通信，以执行配置、注册、身份验证状态检查和授权流，而身份验证流需要第二个屏幕（浏览器）应用程序。 因此，不会使用本机SDK。



### 其他体系结构

除了基于REST API的两个主要体系结构（服务器到服务器和直接客户端解决方案）之外，还有其它体系结构。  其中主要的是SDK架构，该架构使用称为Access Enabler的客户端组件，Adobe Pass身份验证为程序员提供。  该应用程序使用Access Enabler API处理启动、身份验证、授权和注销。  程序员的应用程序与Adobe Pass身份验证服务器之间的所有通信都通过Access Enabler进行。  其他类型的Access Enabler可用于以下平台：JavaScript、iOS、tvOS、Android和FireTV。

虽然可以直接在支持服务器到服务器解决方案之外的本机SDK的客户端平台上使用REST API，但不建议使用此方法。



## REST API优缺点 {#ProsAndCons}

创建Adobe Pass身份验证REST API是为了为没有Web浏览功能或永久存储的设备提供随时随地电视(TVE)解决方案。 REST API支持所有身份验证和授权流，但它缺少本机SDK组件。 由Adobe Pass身份验证提供和维护的SDK附带开箱即用的功能，这些功能实施业务规则，在具有REST API的情况下，必须由程序员实施和维护。 在下面的“程序员职责”表中，我们描述了当前REST API的局限性，这些问题需要程序员来解决。



### 服务器到服务器与基于客户端的优点和缺点

服务器到服务器体系结构提供了一种将大多数身份验证和授权相关逻辑整合到单个逻辑单元或实现中的方法。  这种方法有优缺点。  其优点包括：

* 身份验证和授权业务逻辑的单一实施。
* 无需在每个受支持的平台上使用该平台本机工具实施该逻辑。
* 能够更新功能，而无需使用客户端的所有相关要求（例如应用商店更新）更新客户端。
* **更轻松**&#x200B;扩展和自定义authN和authZ功能（例如添加D2C）。
* 直接管理相关流量，以便更好地控制、质量和监控。



同样，缺点列在程序员的职责中，但包括以下内容：

* 对于不具有Platform SSO的平台，必须为每个客户端实施SSO。
* 如果需要，程序员必须实施特定于MVPD的逻辑。
* 所有使用REST API的平台共享单个配置来控制属性，如身份验证TTL。



### 连接的设备

对于大多数连接的设备，由于SDK不可用，因此必须以某种方式使用REST API。 连接的设备将直接使用REST API，或与使用REST API的服务器到服务器解决方案集成。

## 程序员职责 {#programmer-responsibilities}

以下内容适用于服务器到服务器和连接的设备应用程序。

| **功能** | **负责处理功能** | **当前无客户端API的限制以及与本机SDK的差异的说明** |
| --- | --- | --- |
| 每个平台应用的配置设置 | Adobe | 在所有平台(包括iOS和Android等移动设备)上使用REST API的一个&#x200B;**主要限制**&#x200B;是，与我们的TVE仪表板配置工具中的REST API对应的配置设置适用于所有设备(即使有一个iOS设备运行在REST API之上实现的本机应用程序)。 此限制&#x200B;**可能会破坏**&#x200B;与MVPD之间约定的TTL和约定平台设置 — 如果每个平台的TTL和约定设置不同。 [1](#1) |
| 单点登录 | 程序员 | 使用REST API时，仅在支持平台SSO的平台(例如Apple、Roku、Amazon)上提供SSO，而使用REST API时，无法为其他平台保证SSO。 SDK以跨站点/应用程序方式缓存数据。 这意味着用户在网站/应用程序上登录一次，并且已在参与网站中登录，无需任何用户交互。 [2](#2) |
| 单次注销 | 程序员 | 在本机SDK SSO方案中，从参与的一个应用程序中注销将会从所有位置注销用户。 在当前REST API上，我们不支持SLO，从某个应用程序注销将只为该特定应用程序注销用户。 |
| 缓存 | 程序员 | REST API实施必须实施自己的缓存机制来处理业务认可的数据项。 SDK在考虑到各种业务规则的情况下自动缓存各种数据项。 例如，使用与身份验证令牌相同的TTL来缓存用户元数据，而某些项目可以通过编程方式从缓存中排除（预检）。 |
| 详细的错误报告机制 | 程序员 | REST API主要依赖于HTTP错误代码来报告应用程序错误，而SDK具有详细的错误报告机制，可帮助应用程序开发人员更好地了解所发生的情况。 |
| 应用程序错误恢复（出错时重试、循环检测等） | 程序员 | REST API实施需要构建自己的应用程序恢复系统，而基于SDK的实施可以从SDK错误恢复系统中受益：通过重新尝试具有就地逻辑的特定网络调用来防止“循环”，从而从暂时性网络错误中恢复。 |
| 每个请求者的身份验证 | 程序员 | 有些MVPD出于业务原因或技术挑战需要针对每个站点/应用程序进行身份验证。 SDK会根据TVE功能板配置自动强制实施此功能。 REST API实施者必须自己实施此功能，以免违反业务协议，或能够完成针对这些MVPD的授权，这些MVPD根据应用程序限定其身份验证数据。 |
| 被动身份验证 | 程序员 | 某些MVPD支持“被动”身份验证，用户不需要输入凭据，并且会自动尝试对用户进行身份验证。 这对于要求具有“每个请求者的身份验证”的MVPD特别有用。 在这种情况下，SDK支持的UX是无缝的，用户在一个应用程序中仅验证一次，而SDK对生态系统中的其他应用程序执行“被动”身份验证。 用户看不到其他被动调用，他只需要完成身份验证即可，而MVPD则实现了为每个应用程序设置单独身份验证会话的目标。 |
| 隐式和统一的设备检测 | 程序员 | SDK会自动检测设备类型并以标准方式发送该信息。 REST API实施会根据实施者而易于发送不同的信息，从而跨站点倾斜/破坏业务规则和统计数据。 **程序员需要确保在他们的每个应用程序上向我们发送正确的设备信息**。 对于服务器到服务器实施，除非执行其他步骤，否则REST API无法正确识别最终用户的IP地址。 IP地址在某些使用案例（如防欺诈或HBA）中非常重要。 |
| 防篡改临时传递 | 程序员 | 强制实施TempPass基于稳定的设备ID。 SDK使用硬件信息/指纹技术来识别设备，这种机制不是公开的，因此更加安全和无冲突。 对于REST API实施，程序员需要实施自己的设备识别/指纹算法。 |
| 设备绑定 | 程序员 | 通过SDK的应用程序在设备上生成的令牌不可移植，使得恶意用户难以共享其令牌并提供对其他用户的访问权限。 这是基于与“防篡改临时密码”相同的设备ID机制。 对于REST API，令牌将保留在云中，这意味着具有相同deviceID进行相同调用的两个设备将获得相同的响应/访问权限。 程序员需要确保他们有用于生成/分配设备ID的强大、安全和无冲突的机制。 |
| 可预测且经过认证的功能集 | 程序员 | 每个SDK中的现有功能集都是一致的、可预测的，并且已经过全面认证和测试。 某些业务要求是通过SDK强制执行的，这使得程序员在使用REST API时违反合同的风险很大。 虽然REST API可能更灵活，但Adobe可确保SDK实施实施TVE Dashboard中的业务决策和设置。 在无客户端情况下，每个程序员都实施其自己的业务规则子集，这些业务规则在给定时间点套件其应用程序。 |


1. 作为我们新的One API计划的一部分，我们计划修复此限制，并能够基于设备识别为每个平台应用规则。

2. Adobe将继续与所有主要平台合作，以实施可与我们的REST API一起使用的平台SSO。 我们的“一个API”计划将在使用本机SDK实施的应用程序与使用REST API实施的应用程序之间提供SSO支持。

## 最低设备要求 {#min_reqs}

要使用Adobe Pass身份验证REST API，设备必须满足或超过[Adobe Pass身份验证平台/设备/工具要求文档](#general_clientless_reqs)的REST API部分中列出的最低技术要求。
