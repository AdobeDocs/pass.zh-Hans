---
title: 关于Adobe Pass身份验证
description: 关于Adobe Pass身份验证
exl-id: 5edeaccb-f9fa-4395-83b4-706c518d5a03
source-git-commit: 07bb12f7983f39b58e1b9795fdaa1bec4f68e674
workflow-type: tm+mt
source-wordcount: '1832'
ht-degree: 0%

---

# 关于Adobe®通过身份验证 {#about-adobe-pass-authentication}

>[!IMPORTANT]
>
> 此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## 关于所有地方的电视 {#about-tv-everywhere}

当今的电视观众期待随时随地无缝访问付费电视内容。 随着互联网连接设备的兴起，受众在越来越多的平台上消费内容，包括：

* 笔记本电脑
* 平板电脑
* 智能手机
* 网站
* 联合应用程序
* 游戏控制台
* 机顶盒
* 智能电视

TV Everywhere是一项行业计划，旨在确保付费电视订阅者能够通过家庭内外多种设备访问他们已付费的内容。

虽然传统（线性）电视仍然很强大，但视频消费增长最快的领域是时移内容、在线流媒体和替代屏幕。 这种转变扰乱了视频分发市场，使TV Everywhere成为协调&#x200B;**程序员、付费电视提供商和订阅者利益的关键解决方案。**

### 各处电视的目标 {#goals-tv-everywhere}

**技术目标**

* 支持付费电视客户在所有设备和平台上无缝访问其订阅的内容。

**业务目标**

* 保持和加强现有客户关系，同时创造新的机会。
* 使程序员和内容所有者能够接触到更广泛的受众，并最大化高级内容的价值。
* 通过与查看者直接在线互动来扩展品牌。

### 各地电视的挑战 {#challenges-tv-everywhere}

随处可见的电视机会带来了巨大的挑战，权利是最关键的。 在查看者能够访问订阅内容之前，系统必须验证其权限。

关键问题包括：

* 用户是否与付费电视提供商存在有效订阅？
* 他们的订阅是否包含请求的内容？

对于程序员和内容所有者来说，确定权利尤其具有挑战性，因为付费电视操作员可以控制客户数据和访问权限。

除了应享权利之外，还出现了一些技术和集成挑战，包括：

* 制定可确保无缝访问的多设备策略。
* 管理程序员和付费电视提供商之间的复杂关系。
* 防止欺骗性访问并强制实施服务条款。
* 跨网站和应用程序提供一致、用户友好的身份验证体验。
* 保持快速上市时间以与关联协议保持一致。
* 控制与多个集成相关的成本。

这些挑战使得程序员和多个付费电视身份验证系统之间的直接集成高度占用资源，需要时间和技术专业知识。

为了克服这些障碍，**Adobe® Pass Authentication**&#x200B;简化并简化了授权验证，从而能够无缝、安全地访问TV Everywhere内容。

## Adobe Pass身份验证简介 {#introduction-adobe-pass-authentication}

Adobe Pass身份验证安全地协调程序员和付费电视提供商之间的授权交易，确保适当的客户可以轻松访问适当的内容。

![](../assets/programmers-connect-authn.png)

*通过Adobe Pass身份验证连接的一些程序员和付费电视提供商*

**受益于Adobe Pass身份验证的人员**

* **程序员**

  ，这些用户希望轻松与付费电视提供商（也称为MVPD，即多频道视频节目分发商）集成，以吸引最广泛的受众并优化收入。

* **付费电视提供商(MVPD)**

  寻求通过单一集成与多个内容所有者（程序员）建立联系的客户，通过促进在线访问订阅内容来提高客户满意度。

* **付费电视客户**

  想要随时随地通过任何设备观看已付费内容的用户。

**程序员**

希望最大限度地扩大受众范围和收入的程序员可以：

* 轻松与顶级付费电视提供商集成，无需管理多个直接连接。
* 通过确保所有主要提供商和平台之间的身份验证来扩展其受众。
* 通过安全身份验证保护高级内容，限制对授权用户和设备的访问。
* 启用单点登录(SSO)以改善跨应用程序和网站的用户体验。

**付费电视提供商(MVPD)**

付费电视提供商可通过以下方式改善客户体验并简化操作：

* 通过单个集成与多个内容所有者连接。
* 跨设备和平台提供贴牌、无缝的观看体验。
* 确保安全身份验证，以防止未经授权的访问，并管理每个家庭的并发流。

**付费电视客户**

订户可随时随地通过电视收看自己已付费的内容。

### 与Adobe Pass身份验证集成 {#integrating-adobe-pass-authentication}

无论您是付费电视提供商还是程序员，与Adobe Pass身份验证集成都需要主动参与。 下面是这两个角色的过程的高度概述。

#### 程序员集成流程 {#programmer-integration-process}

正式启动集成后，可提供其他指导，但流程通常涉及以下步骤：

**先决条件**

* 内容管理系统(CMS)。
* 一种内容传送机制，其可以包括第三方内容传送网络(CDN)。
* 在网站或独立应用程序中嵌入了媒体播放器的现有在线视频平台。

**集成任务**

* 集成Adobe Pass身份验证[REST API DCR](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)。
* 集成Adobe Pass身份验证[REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)。
* 集成Adobe Pass身份验证[媒体令牌验证器](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier)。
* 开发用于身份验证、授权和注销工作流程的用户界面。

有关程序员集成过程的更多详细信息，请参阅[程序员kickstart指南](/help/authentication/kickstart/programmer-kickstart-guide.md)和[程序员集成指南](/help/authentication/integration-guide-programmers/programmer-integration-guide-overview.md)文档。

#### 付费电视提供商集成流程 {#pay-tv-provider-integration-process}

正式启动集成后，可提供其他指导，但流程通常涉及以下步骤：

**先决条件**

* 签署Adobe Pass身份验证保密协议(NDA)。
* 向Adobe Pass身份验证工程团队提供有关身份验证和授权系统的规范。 为了实现最简单的集成，建议使用基于SAML的身份验证提供程序(IdP)和基于SOAP的授权系统。

**集成任务**

* 建立与Adobe Pass身份验证服务器的连接。
* 完成暂存版本并确保质量保证。
* 完成生产版本并确保质量保证。

付费电视提供商可免费使用标准集成，包括文档和基本电子邮件支持。 但是，需要大量帮助或加快时间安排的提供商可能会收取支持费用，或者选择与经验丰富的第三方合作伙伴（如Synacor）合作。

Adobe Pass身份验证可以通过两种关键方式有效支持特定于付费电视提供商的业务逻辑：

* 对于自包含且由付费电视提供商在收到授权请求时应用的业务逻辑，Adobe会提供支持实施所需的数据（例如唯一设备ID、IP地址）。
* 对于需要用户干预的业务逻辑或Adobe的特定处理，可以为每个付费电视提供商维护自定义属性。 这些配置可能包括在身份验证过程中的特定点触发的预定义工作流。

有关付费电视提供商集成流程的更多详细信息，请参阅[MVPD kickstart指南](/help/authentication/kickstart/mvpd-kickstart-guide.md)和[MVPD集成指南](/help/authentication/integration-guide-mvpds/mvpd-integration-guide-overview.md)文档。

### 权利流 {#entitlement-flow}

Adobe Pass Authentication充当代理，通过为双方提供安全一致的界面，促进程序员和MVPD之间的权利流。

对于程序员，Adobe Pass身份验证将API作为&#x200B;**Standard**&#x200B;或&#x200B;**Premium**&#x200B;层的一部分提供：

* 标准Adobe Pass身份验证API：
   * [REST API DCR](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
   * [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)

* Premium Adobe Pass身份验证API：
   * [重置临时传递API](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#reset-tempass-api-access)
      * [TempPass功能](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md)
   * [降级API](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md#degradation-api-access)
      * [退化特征](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md)
   * [授权服务监控API](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-api.md)

有关权利流程的更多详细信息，请参阅[程序员集成指南](/help/authentication/integration-guide-programmers/programmer-integration-guide-overview.md#entitlement-flow)文档。

#### 了解权利 {#understanding-entitlements}

Adobe Pass身份验证解决方案围绕着权限创建来考虑的，权限是在成功完成身份验证和授权工作流后生成的特定数据段。 这些权限授予对受保护内容的访问权限，但具有有限的生命周期。 权利到期后，必须通过重新启动身份验证或授权过程来续订权利。

有关授权的更多详细信息，请参阅以下文档：

* **配置文件**

  在成功进行身份验证后，Adobe Pass身份验证会创建一个已身份验证配置文件（“长期”），该配置文件与请求应用程序、设备和服务提供商标识符（请求者标识符）相关联。

* **[用户元数据](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)**

  在成功进行身份验证后（在某些情况下，在身份验证之后也是如此），Adobe Pass身份验证将从MVPD接收用户元数据，这些元数据可能会向请求应用程序公开这些元数据。

* **[决策](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md)**

  成功授权后，Adobe Pass身份验证会创建一个与请求应用程序、设备、服务提供商标识符（请求者标识符）和特定受保护资源（资源标识符）关联的授权决策（“长期”）。

* **[媒体令牌](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md)**

  成功授权后，Adobe Pass身份验证会创建一个与成功的播放请求关联的媒体令牌（“短暂的”），并为减少欺诈（例如，流翻录）的行业最佳实践提供支持。

用户档案和决策的生存时间(“TTL”)值是根据程序员和付费电视提供商之间的协议设置的，这些提供商就最适合所有相关人员的价值达成了共识。

#### 构建用户界面 {#building-user-interface}

程序员负责为其网站或应用程序内的授权工作流设计和实现用户界面(UI)，而某些元素（如登录流程）则由付费电视提供商处理。

程序员至少必须：

* **实现提供程序选择接口**
   * 允许新用户识别其付费电视提供商并首次登录。
   * 有些付费电视提供商会将用户重定向到外部登录页面，而另一些提供商则需要在iframe内登录。 程序员必须实施回调函数才能在需要时生成iframe。

* **管理支持的付费电视提供商列表**
   * 确保用户只能通过批准的提供商访问内容。

* **指示身份验证状态**
   * 显示用户在应用程序或网站中经过身份验证的时间。

* **识别受保护的资源**
   * 明确指示哪些内容在查看之前需要授权。
   * 在授予访问权限后，更新UI以反映成功的授权。

## 常见问题解答 {#faqs}

**哪里都有电视？**

TV Everywhere是一项行业计划，允许Pay TV客户通过各种互联网连接设备访问他们已订阅的优质内容。 这包括个人计算机、平板电脑、智能手机、游戏机、机顶盒和智能电视。 主要挑战是确保无缝且用户友好的身份验证过程，使客户能够访问其订阅内容而无需多次登录或技术障碍。

**什么是Adobe Pass身份验证，它如何支持所有地方的电视节目？**

Adobe Pass身份验证通过简单高效地安全验证用户访问内容的权限，让TV Everywhere变得生动有趣。 它是一项托管服务，可根据程序员和付费电视提供商设置的业务规则促进快速后端集成。 这将缩短所有利益相关者的上市时间，提供更加安全的环境以最大程度地减少欺诈，并提供更好的用户体验，在多个平台上可访问更多电视内容。

**如何传递Adobe Pass身份验证？**

Adobe Pass身份验证作为软件即服务(SaaS)解决方案提供。 此方法可确保最终用户、程序员和付费电视提供商之间的安全通信，以进行内容权利验证。

**什么因素使Adobe Pass身份验证不同于其他TV Everywhere解决方案？**

与其他解决方案相比，Adobe Pass身份验证具有以下几个优势：

* **无缝单点登录(SSO)** — 与各个提供商的直接集成不同，Adobe Pass身份验证在用户在不同网站和应用之间移动时提供持久登录体验。
* **广泛的市场渗透** — 一旦程序员与Adobe Pass身份验证集成，他们将立即获得覆盖超过90%美国家庭的付费电视运营商的访问权限。
* **与Adobe的生态系统集成** — 与其他Adobe解决方案无缝协作，以实现内容交付、保护和盈利，包括Adobe Analytics。

**Adobe Pass身份验证的安全程度如何？**

安全是重中之重。 Adobe Pass身份验证通过绑定对用户设备的访问权限，确保只有经过授权的用户才能访问高级内容。 它还提供了用于限制每个家庭并发流、会话或设备数的选项。

**Adobe Pass身份验证支持哪些设备？**

Adobe Pass身份验证设计用于各种设备，例如基于Web的设备、移动设备或连接电视的设备。

**Adobe Pass身份验证对最终用户是否造成任何损失？**

不适用。 Adobe Pass身份验证对最终用户是免费的。
