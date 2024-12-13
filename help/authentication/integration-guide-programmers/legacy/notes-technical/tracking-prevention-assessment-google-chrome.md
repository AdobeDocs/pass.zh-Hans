---
title: 跟踪预防评估Google Chrome
description: 跟踪预防评估Google Chrome
exl-id: f3d552da-2fd7-4ac8-9f82-876625af5d47
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '673'
ht-degree: 0%

---

# （旧版）跟踪预防评估 — Google Chrome {#tracking-prevention-assessment-google-chrome}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

>[!IMPORTANT]
>
> 确保随时了解汇总在[产品公告](/help/authentication/product-announcements.md)页中的最新Adobe Pass身份验证产品公告和停用时间表。

## 概述

本文档汇总了有用的资源，并对Google Chrome作为其逐步淘汰第三方Cookie计划的一部分而计划的即将进行的更改进行评估。

评估针对运行在Google Chrome浏览器上、且使用Adobe Pass Access Enabler JavaScript SDK v4与Adobe Pass Authentication后端服务集成的TV Everywhere (TVE)应用程序。

## 公共资源

请参阅下面的Google开发人员网站及其官方博客汇总的资源列表，我们建议我们的客户参考这些资源：

* [在Chrome中逐步停用第三方Cookie的下一步骤](https://blog.google/products/chrome/privacy-sandbox-tracking-protection/)
* [隐私沙盒开发人员文档](https://developers.google.com/privacy-sandbox)
* [为第三方Cookie限制做准备](https://developers.google.com/privacy-sandbox/3pcd)
* [准备第三方Cookie逐步淘汰](https://developers.google.com/privacy-sandbox/3pcd/prepare/prepare-for-phaseout)
* [准备结束第三方Cookie](https://developers.google.com/privacy-sandbox/blog/cookie-countdown-2023oct)
* [默认情况下限制了1%的Chrome用户的第三方Cookie](https://developers.google.com/privacy-sandbox/blog/cookie-countdown-2024jan)

## 时间线

简单地说，Google Chrome已开始测试[跟踪保护](https://privacysandbox.com/)，这是一项新功能，可限制影响所有第三方Cookie的跨站点跟踪。

最初，这是从2024年初开始的，影响到大约1%的用户，其（暂定）计划从2024年第三季度开始，将这一影响扩展到最多100%的用户。

## 评估

Google通过以下链接发布了一份文档，汇总了其推荐的行动手册，为第三方Cookie逐步淘汰做准备： https://developers.google.com/privacy-sandbox/3pcd/prepare/prepare-for-phaseout。

我们遵守了本行动手册，以评估在Google Chrome浏览器上运行的、使用Adobe Pass Access Enabler JavaScript SDK v4与Adobe Pass Authentication后端服务集成的TV Everywhere (TVE)应用程序。

### 结论

根据我们的测试，模拟Google Chrome即将进行的更新，主要TVE业务流程&#x200B;**将继续按预期运行**。

但是，必须认识到Google更广泛的战略，该战略不仅涉及停止使用第三方Cookie，还涉及对第三方存储进行分区。

因此，Chrome用户将会遇到单点登录(SSO)、单点注销(SLO)和被动身份验证功能中断的情况，从而需要为他们使用的每个TVE应用程序执行单独的登录/注销操作（与当前在Safari上的体验保持一致）。

## 要求自我评估

我们敦促客户尽早主动进行类似的评估，以查明潜在问题，并熟悉修订后的Google Chrome用户体验。

此评估应同时包含第一方服务和第三方服务，尤其是与Adobe Pass Access Enabler JavaScript SDK v4集成相关的服务。

如果您遇到任何与TVE业务流程相关的困难，包括身份验证、预授权、授权、用户元数据或注销，我们邀请您通过Zendesk票证向我们的客户关怀团队提交报告。

如需制定自我评估计划的帮助，请参阅以下部分。

### 审核Cookie的使用

从Chrome 118开始，[DevTools Issues](https://developer.chrome.com/docs/devtools/issues/)选项卡通过以下消息突出显示可能受影响的Cookie：`Cookie sent in cross-site context will be blocked in future Chrome versions`。

标记为第三方使用的Cookie可以通过其`SameSite=None`属性值进行标识。

请访问以下链接以了解更多信息： https://developers.google.com/privacy-sandbox/3pcd/prepare/audit-cookies

### 测试损坏

为了测试是否损坏，请使用`--test-third-party-cookie-phaseout`命令行标志启动Chrome，或者从Chrome 118中启用`chrome://flags/`中的`#test-third-party-cookie-phaseout`。

这将设置Google Chrome以阻止第三方Cookie并确保未来功能处于活动状态，以便最好地模拟逐步停用后的状态。

值得深入了解以下Chrome标记的技术规范：

* `#test-third-party-cookie-phaseout`
* `#third-party-storage-partitioning`

请访问以下链接以了解更多信息： https://developers.google.com/privacy-sandbox/3pcd/prepare/test-for-breakage

## 其他浏览器

### Firefox

Firefox几年前推出了名为`Enhanced Tracking Protection`的机制。

请参阅下面来自Firefox的一些有用资源：

* https://support.mozilla.org/en-US/kb/enhanced-tracking-protection-firefox-desktop
* https://support.mozilla.org/en-US/kb/enhanced-tracking-protection-firefox-android

### Safari

几年前，Safari推出其名为`Intelligent Tracking Prevention`的机制。

请参阅下面来自Safari的一些有用资源：

* https://webkit.org/blog/9521/intelligent-tracking-prevention-2-3/
* https://webkit.org/blog/8828/intelligent-tracking-prevention-2-2/
* https://webkit.org/blog/8311/intelligent-tracking-prevention-2-0/
* https://webkit.org/blog/8142/intelligent-tracking-prevention-1-1/
* https://webkit.org/blog/7675/intelligent-tracking-prevention/

请参阅下面来自Adobe Pass的一些有用资源：

* [跟踪预防评估 — Apple Safari](tracking-prevention-assessment-apple-safari.md)
