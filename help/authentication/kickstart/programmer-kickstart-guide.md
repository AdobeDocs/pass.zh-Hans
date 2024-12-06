---
title: 程序员kickstart指南
description: 程序员kickstart指南
exl-id: 0aecdb81-9b97-4475-b0b0-654d916b2374
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '972'
ht-degree: 0%

---

# 程序员kickstart指南 {#programmer-kickstart-guide}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## 简介 {#prog-kickstart-guide-intro}

欢迎使用Adobe Pass Authentication for TV Everywhere。 我们期待着与您合作。

>[!NOTE]
>
>这是面向程序员（内容提供商）的Kickstart指南。 如果您使用多通道视频编程分发服务器(MVPD)，请务必查看[MVPD快速入门指南](/help/authentication/kickstart/mvpd-kickstart-guide.md)。


Adobe Pass身份验证联系人：

* 支持 — 针对所有问题、事件或功能请求，`tve-support@adobe.com`
* 到项目启动时，会向项目分配一个支持联系人。

以下信息概述了一些重要的首要步骤，以便我们开始一个扎实而有效的计划。 我们的目标是提供有关我们将如何与合作伙伴合作来实现集成的解释和预期。 请注意下面每个项目的“您将提供”/“Adobe将提供”部分。 在我们完成项目时，这些内容以核对清单或指南的方式列出。

本文档假定程序员已注册与选定的MVPD合作伙伴合作。

## 发布计划 {#release-schedule}

我们规划了Adobe冲刺开发周期，以便您能够了解排程发布的时间以及如何通过我们的开发系统推广每个版本。

在每次冲刺完成后，它可用于QE，或在UAT服务器上看到新的实施。

大约每6周会向Adobe资格预审服务器发布一次。 （这是我们在执行最终定量宽松的同时保留提议的下一版本的服务器。 ） 这些内部版本将包含自上次放置以来在冲刺中完成的所有工作。 目前，合作伙伴可以利用2周的QE窗口测试这一版本。

假定在前两个星期的测试时段中没有出现任何严重问题，则该版本将提升至实时生产。 这意味着该集成将在Adobe发布环境中可用，但合作伙伴会在发布时进行选择。

<!--For the latest release schedule information, see the Release Calendar.-->

## 支持文档 {#supp-doc}

Adobe将提供：

* 部署指南： **`https://tve.zendesk.com/entries/498741-tve-deployment-guide`**
* 访问我们的Zendesk客户支持系统。 您还可以在这里找到有关某些流程的示例、信息和视频教程。 若要在Zendesk上访问此文档以及在其中发布的其他文档，您必须在`https://tve.zendesk.com/home`注册并创建一个帐户。 您可以注册的用户数量没有限制。  您可以查看和共享任何已归档票证的评论。 所有支持问题都应发送至`tve-support@adobe.com`。
* [程序员集成指南](/help/authentication/integration-guide-programmers/programmer-integration-guide-overview.md)
* 媒体令牌验证器库： `https://tve.zendesk.com/entries/471323-media-token-validator-library`。

## 测试环境设置 {#test-env-setup}

Adobe将首先为您设置Adobe测试站点，在该站点中，Adobe充当MVPD用于测试。 然后，您的团队可以设置一个调用AdobeAPI的测试网站。 使用默认的MVPD选择器，并选择“Adobe”作为idP。

您将提供：

1. 请求者ID。 这是一个字符串，它将唯一标识向Adobe Pass身份验证发出请求的网站或应用程序的品牌。 字符串本身是任意的，但需要在Adobe和程序员之间达成一致
1. 渠道信息。 这是一组字符串，用于标识请求者ID所请求的内容渠道。 在许多情况下，渠道和请求者ID是相同的。 但是，您可能具有多个可通过同一ID请求的内容渠道。 频道名称字符串应该与有线电视频道相对应。 某些MVPD将通过AuthN和/或AuthZ协议验证此值。
1. 域名（允许用于该请求者ID）。 这将是一个实际域名列表，Adobe将列出这些域名以接受请求者ID。 这可以确保只有您批准的域才能访问使用您的元数据的Adobe Pass身份验证。 注意：对生产有效的域名在测试/暂存时可能不同，应该提供并识别这两个域名。

Adobe将设置帐户，Adobe将提供：

* 用于访问测试站点的登录名和密码

## 使用MVPD设置 {#setup-mvpd}

本节介绍从Adobe测试站点迁移以使用MVPD时需要执行的操作。

您将提供（通过MVPD）：

* **两组凭据**：
   * AuthN + AuthZ ：经过身份验证和授权的用户的登录/密码
   * AuthN + Non-AuthZ ：已验证但未授权的用户的登录/密码
* **资源ID**。 这是一个特定的内容标识符，将通过AuthZ协议通过MVPD进行验证。 这可以在渠道、节目、集或资产级别；它应与您的MVPD商定。

Adobe Pass身份验证支持基于MRSS的元数据架构，这意味着资源ID可以根据需要而特定，并且可以包含对于特定MVPD唯一的标识符。

**新MVPD集成**：请务必记住，您选择的MVPD在完成任何集成时都起着不可或缺的作用。 Adobe需要根据每个MVPD的规范为其编写代码。 在这些步骤完成之前，您将无法从对话框中选择该MVPD，也无法完成产品测试。 Adobe需要提前安排此工作，以便与下一个可用冲刺相匹配。 （有关当前计划信息，请参阅发布日历。）

**现有MVPD集成**：如果您选择的MVPD已使用Adobe进行设置，那么连接步骤应该简单得多（更快），并且通常可以通过配置更改实现连接。

>[!NOTE]
>
>MVPD仍必须启用程序员，并签署任何相关的商业交易。

**QE与MVPD**：所有集成都将涉及联合QE，由于最终用户最终是MVPD的客户，因此许多人在推送“实时”之前设置了测试周期。 由于这涉及MVPD资源的调度，因此这是一个潜在的延迟区域。

<!--
>[RELATEDINFORMATION]
>[MVPD Kickstart Guide](help\authentication\mvpd-kickstart-guide.md)
-->
