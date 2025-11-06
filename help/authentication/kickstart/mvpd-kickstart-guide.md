---
title: MVPD快速入门指南
description: MVPD快速入门指南
exl-id: 6423cc9a-a45a-4cde-b562-4cb72c98e505
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '934'
ht-degree: 0%

---

# MVPD快速入门指南 {#mvpd-kickstart-guide}

>[!IMPORTANT]
>
> 此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

本Kickstart指南面向计划与Adobe® Pass Authentication集成的多渠道视频节目分发商(MVPD)。

本文档概述了确保顺利高效地启动集成流程的关键初始步骤。 它旨在明确我们的期望，并为我们如何与合作伙伴协作以实现成功集成提供指导。

Adobe提供了一系列资源来帮助您与Adobe Pass身份验证集成。 请参考&#x200B;**“您将提供”**&#x200B;和&#x200B;**“Adobe将提供”**&#x200B;以下各部分的提及内容。

>[!CAUTION]
>
> 每次用户启动授权流时，都会为他们分配一个不透明的唯一用户ID。 此ID源自MVPD，用于识别程序员应用程序中的用户。
>
> <br/>
>
> 用户ID不得包含任何个人身份识别信息(PII)或数据，这些信息或数据可以单独使用或与其他详细信息结合使用，以识别、联系或查找用户。

## 设置过程 {#setup-process}

设置过程涉及以下步骤：

![Adobe®通过身份验证集成进程](../assets/mvpd-int-lifecycle.png)

*Adobe®通过身份验证集成进程*

### 启动 {#kickoff}

**您将在启动阶段提供**：

* **显示名称**

  这是提示用户选择其付费电视提供商时显示在程序员网站或应用程序上的字符串。

* **徽标URL**

  这是一个文件，大小为112 x 33像素，其中包含提示用户选择其付费电视提供商时显示在程序员网站或应用程序上的徽标。

* **存留时间(TTL)**

  TTL是一个值，通常由MVPD在身份验证或授权过程中设置。 但是，Adobe可以覆盖这些TTL值，并根据程序员和MVPD商定的内容提供不同的值。

* **凭据集**

  这些凭据用于通过MVPD验证和授权用户，或仅验证用户。 通常，这些凭据由用户名和密码组成，必须同时为配置文件（暂存和生产）提供用户名和密码。

### 元数据交换(SAML) {#metadata-exchange-saml}

**Adobe将在元数据交换阶段提供**：

* **暂存环境元数据**

  可以从https://sp.auth-staging.adobe.com/sp/metadata中检索Adobe的SP元数据。

* **生产环境元数据**

  可以从https://sp.auth.adobe.com/sp/metadata中检索Adobe的SP元数据。

**您将在元数据交换阶段提供**：

* **暂存元数据**

  MVPD的暂存环境元数据。

* **生产元数据**

  MVPD的生产环境元数据。

### 连接性 {#connectivity}

**您将提供**&#x200B;一种从Adobe允许列表IP的方法，因为Adobe Pass身份验证要求防火墙允许通过端口80和443的流量在身份验证和授权过程中启用对受限资源的访问。

**您将在暂存配置文件中提供**&#x200B;部署以测试连接。

### 开发 {#development}

**Adobe将提供**&#x200B;个工程时间来与MVPD密切合作，以确保正确建立技术集成。 此过程包括根据MVPD的特定要求定制自定义代码。

### 暂存部署 {#deployment-staging}

**Adobe将为**&#x200B;内部版本提供必需的代码更新，这些代码更新将首先部署在PRE-QUAL暂存环境中。 在此阶段中，还将执行必要的配置更改以将MVPD与`TestDistributors`服务提供商集成以进行测试。

**您和Adobe将提供**&#x200B;质量保证(QA)时间，以确保在PRE-QUAL暂存环境中成功测试集成。 在此阶段之后，MVPD将移至发行版暂存环境，以供使用实际的程序员进一步测试。

### 在生产环境中部署 {#deployment-production}

**您将在生产配置文件中提供**&#x200B;部署以测试连接。

**Adobe将为**&#x200B;生成提供所需的代码更新，这些更新将部署在PRE-QUAL生产环境中。

**您和Adobe将提供**&#x200B;质量保证(QA)时间，以确保使用生产配置文件成功测试集成。 如果此时一切正常，Adobe可以将集成移至可供所有用户使用的发行版生产环境（“实时”）。

>[!IMPORTANT]
>
> 一旦集成在RELEASE生产环境中上线，保持最佳客户体验就变得至关重要。 为了有效解决服务器停机情况，MVPD必须向Adobe提供详细的升级过程文档以便管理此类问题。
>
> 作为回报，Adobe将确保MVPD接收最新版本的Adobe Pass身份验证升级过程，以简化问题解决方案。

## 对环境的访问权限 {#access-environments}

**Adobe将为开发过程的不同阶段提供**&#x200B;对环境的访问权限：

* **资格预审（资格预审）**

  PRE-QUAL环境托管下一个发行候选版本，并用作新合作伙伴的初始集成平台。 在迁移到RELEASE环境之前，合作伙伴有时间在PRE-QUAL中测试其集成。

* **版本（版本）**

  RELEASE环境托管当前（稳定的）生产版本。

有关如何使用这些环境的更多信息，请参阅[了解Adobe环境](/help/authentication/notes-technical/environments/understanding-the-adobe-environments.md)文档。

>[!IMPORTANT]
> 
> 在已建立的更改请求流程之后，必须通过Adobe代表明确请求对这些环境进行配置更改。

## 访问客户支持 {#access-customer-support}

**Adobe将通过** Zendesk[提供](https://tve.zendesk.com/home)访问我们的客户支持系统的权限。 要访问Zendesk，您必须在https://tve.zendesk.com/home上注册并创建一个帐户。

Adobe Pass身份验证团队可用于解答我们在集成过程中可能遇到的任何问题或技术问题。 请通过[tve-support@adobe.com](mailto:tve-support@adobe.com)联系我们。

## 文档访问权限 {#access-documentation}

**Adobe将通过** Adobe Experience League[提供](https://experienceleague.adobe.com/en/docs/pass/authentication/home)对公共文档的访问权限。

Adobe Pass身份验证团队提供了[MVPD集成指南](/help/authentication/integration-guide-mvpds/mvpd-integration-guide-overview.md)部分下可用功能和工作流的综合文档。 有关每个主题的详细信息的链接，请参阅本节下的目录。

## 访问测试工具 {#access-testing-tool}

**Adobe将通过** Adobe Developer[网站提供](https://developer.adobe.com/adobe-pass/)对我们API探索工具的访问权限。
