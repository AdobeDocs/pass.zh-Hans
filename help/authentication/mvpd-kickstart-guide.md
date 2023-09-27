---
title: MVPD直接集成计划
description: MVPD直接集成计划
exl-id: 6423cc9a-a45a-4cde-b562-4cb72c98e505
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1067'
ht-degree: 0%

---

# MVPD快速入门指南：MVPD直接集成计划 {#mvpd-dir-int-plan}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## 介绍 {#mvpd-kickstart-intro}

欢迎使用Adobe Pass Authentication for TV Everywhere。  我们期待着与您合作。

>[!NOTE]
>
>这是多频道视频节目分发商(MVPD)的快速入门指南。 如果您是程序员（内容提供商），请参见 [程序员Kickstart指南](/help/authentication/programmer-kickstart-guide.md).

您可以随时通过Zendesk上的Adobe Pass身份验证票证系统提供支持。 在这里，您还可以找到用于我们流程的示例、文档和视频教程。 要使用 [Zendesk](https://adobeprimetime.zendesk.com/)，您必须在https://tve.zendesk.com/home上注册并创建一个帐户。 您可以注册的用户数量以及在已归档票证上可查看或发表评论的用户数量没有限制。 所有支持问题都应发送至：tve-support，网址为adobe.com

**团队联系人**：

**支持**  — 对于所有问题、事件或功能请求 **tve-support@adobe.com**.

## 1.启动会议 {#kickoff-meetings}

这些会议的范围是Adobe和多边残疾人发展组织开始技术讨论。 在这个流程中，文档需要由双方共享。 接下来，Adobe需要在我们的票证系统(https://tve.zendesk.com/)中打开票证来跟踪集成的状态。

## 2.功能 {#features}

Adobe将设置每周状态调用，以讨论和跟踪集成的总体计划、步骤、时间线和实施详细信息。 在此阶段，Adobe将检查MVPD的规范。 其结果应该是详细说明了MVPD所需的所有功能的规范页面。 MVPD将向Adobe发送一份规范文档，详细介绍MVPD的身份验证/授权实现。

要澄清的项目，请参阅 [MVPD集成功能](/help/authentication/mvpd-integr-features.md).

有几个设置需要在此时详细描述：

* **MVPD的徽标URL**  — 这是一个文件，其尺寸为：112 x 33像素。 当用户单击“登录”按钮选择他们的付费电视提供商时，程序员会在他们的网站上显示徽标。
* **TTL（存留期）值** - TTL通常由MVPD在身份验证/授权过程中设置。 但是，Adobe可以覆盖这些TTL值，并根据程序员和MVPD商定的内容提供不同的值。
* **显示名称**  — 当用户单击“登录”按钮选择他们的付费电视提供商时，程序员会在他们的网站上显示此信息。
* **测试凭据**  — 两个用户档案（暂存和生产）都应具有测试凭据列表。

>[!IMPORTANT]
>
>每次用户启动授权流时，他都会与单个、不透明、唯一的用户ID相关联。  用户ID用于标识程序员应用程序的用户，但源自MVPD。

>[!CAUTION]
>
>每个MVPD的重要注意事项：用户ID不应包含任何PII（个人身份信息）、可单独使用或与其他信息一起用于识别、联系或查找用户的信息。

## 2.元数据交换 {#metadata-ex}

双方需要交换涉及的所有环境（生产、暂存等）的元数据。

* **Adobe**
   * 对于暂存环境，可以从以下位置检索Adobe的SP元数据： [身份验证暂存sp元数据](https://sp.auth-staging.adobe.com/sp/metadata)
   * 对于生产环境，可以从以下位置检索Adobe的SP元数据： [身份验证生产sp元数据](https://sp.auth.adobe.com/sp/metadata)

* **MVPD**
   * 添加元数据（暂存/生产）。

## 4.允许IP列表 {#allow-ip-list}

以下IP应在MVPD的防火墙中列入白名单。 请联系Adobe以获取IP列表。

* Adobe Pass身份验证要求在端口80和443上打开防火墙，以允许访问受限资源。

* MVPD需要为身份验证和授权服务器添加IP地址列表（如果是这种情况）。

## 5.发展 {#deve}

开发阶段的持续时间将在审查规格并考虑到双方签署的项目后确定。 Adobe需要为授权部分编写自定义代码。

>[!NOTE]
>
>请注意，授权是按资源执行的。 授权交易通常使用从程序员站点传递的ID字符串执行，该ID字符串表示用户请求授权的渠道。 此资源ID在程序员和MVPD之间建立，并且可以根据需要精确显示。

## 6.Adobe环境 {#adobe-env}

Adobe为开发过程的不同阶段提供不同的环境：

* **资格预审** （预定）：预定环境包含下一个候选版本。 在将集成升级到发布环境之前，Adobe最初在此环境中集成新合作伙伴。 合作伙伴有两周时间可以在质量前环境中进行测试，并且必须明确请求对质量前配置进行任何更改(有关更改请求流程的详细信息，请与您的Adobe代表联系)。 错误修复会在此环境中触发新部署。
* **版本** （版本）：在此处将Adobe的当前生产版本部署到实时环境中。

有关如何使用Adobe环境的更多信息，请参阅 [了解Adobe环境](/help/authentication/understanding-the-adobe-environments.md)

## 7.暂存部署 {#stag-env}

根据从MVPD收到的元数据，Adobe将在Adobe Pass身份验证系统中创建和配置新的MVPD。 这将部署在Adobe的预测试环境中，并将使用我们的测试程序员(TestDistributors)进行配置。

MVPD需要在其QA/暂存/测试环境中执行相同的部署。

## 8.测试和故障排除 {#tes-troubleshoot}

在此阶段，Adobe和MVPD测试并排除集成故障。 为了帮助测试集成，Adobe Pass身份验证团队可以使用Adobe的API测试站点。 要了解有关使用Adobe API测试站点的更多信息，请参阅 [使用AdobeAPI测试站点测试身份验证和授权流](/help/authentication/test-authn-authz-flows-using-adobes-api-test-site.md).

测试和故障排除成功完成后，会在Adobe的发布暂存环境中启用集成。 此时，Adobe可以将MVPD与实际的程序员集成。

## 9.生产部署 {#prod-dep}

* MVPD需要首先在生产配置文件中部署，以测试连接。

* Adobe部署在前期生产中。

* 现在，所有各方都可以在生产用户档案中进行测试。

* 如果此时一切正常，Adobe可以将集成移动到发布生产环境（“实时”），该环境可供所有用户使用。

## 10.上报程序 {#esc-proc}

一旦集成在生产环境中投入使用，提供最佳客户体验就变得至关重要。 为了确保服务器停机问题时获得最佳响应，MVPD必须提供上报过程文档，以记录提请Adobe注意的问题。

反过来，Adobe会为MVPD提供最新的Adobe Pass身份验证升级流程。


<!--- [!RELATEDINFORMATION]
>
>* [Programmer Kickstart Guide](/help/authentication/programmer-kickstart-guide.md)
>* [MVPD Integration Guide](/help/authentication/mvpd-integr-features.md)
-->
