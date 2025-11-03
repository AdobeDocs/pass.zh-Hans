---
title: 渠道
description: 了解TVE仪表板中的渠道及其各种配置。
exl-id: bbddeccb-6b6f-4a8f-87ab-d4af538eee1d
source-git-commit: 913b2127d2189bec1a7e6e197944f1512b764893
workflow-type: tm+mt
source-wordcount: '1556'
ht-degree: 0%

---

# 渠道 {#channels}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

TVE仪表板的&#x200B;**渠道**&#x200B;部分允许您查看和管理与特定程序员关联的渠道设置。 您还可以[根据需要添加新渠道](#add-new-channel)。

左侧面板中的&#x200B;**渠道**&#x200B;选项卡显示链接渠道的列表，其中包含以下详细信息：

* **显示名称**：用于商业目的的渠道的品牌名称。
* **渠道ID**：唯一标识符，也称为请求者ID。
* **集成**：与[MVPD](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#mvpd)建立的连接数。

![现有渠道列表](/help/authentication/assets/tve-dashboard/new-tve-dashboard/channels/channels-list-view.png)

*现有渠道列表*

在列表上方的&#x200B;**搜索**&#x200B;栏中键入该渠道的名称，了解有关该渠道的更多信息。

## 管理渠道配置 {#manage-channel-conf}

按照步骤管理特定渠道的各种设置。

1. 在左侧面板中选择&#x200B;**渠道**&#x200B;选项卡。

1. 从可用列表中选择渠道。

1. 选择以下选项卡之一以查看和编辑所选渠道的相应设置：

   * [常规设置](#general-settings)
   * [集成](#integrations)
   * [证书](#certificates)
   * [域](#domains)
   * [已注册的应用程序](#registered-applications)
   * [自定义架构](#custom-schemes)

   ![渠道设置](/help/authentication/assets/tve-dashboard/new-tve-dashboard/channels/channel-tabs-view.png)

   *渠道设置*

>[!IMPORTANT]
>
> 查看[查看并推送更改](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md)以了解有关激活配置更改的详细信息。

### 常规设置 {#general-settings}

此选项卡显示&#x200B;**渠道信息**&#x200B;和&#x200B;**分析配置**。

#### 渠道信息 {#channel-information}

在此部分中，可以编辑以下详细信息：

* **显示名称**：用于商业目的的渠道的品牌名称。

* **默认重定向URL**：用于身份验证和注销的备份重定向URL。

* **错误报告**：选择&#x200B;**是**&#x200B;时，Adobe Pass SDK将错误报告发送到Adobe Pass后端以进行Analytics。

![编辑频道信息](/help/authentication/assets/tve-dashboard/new-tve-dashboard/channels/channel-general-settings-tab-view.png)

*编辑频道信息*

#### Analytics配置 {#analytics-configuration}

在此部分中，您可以配置将Adobe Pass身份验证事件转发到Adobe Analytics。

要启用&#x200B;**Analytics配置**，请联系您的技术客户经理(TAM)，了解有关设置报表包ID (RSID)的详细信息。

![启用Analytics配置](/help/authentication/assets/tve-dashboard/new-tve-dashboard/channels/channel-add-new-analytics-configuration-button.png)

*启用Analytics配置*

选择&#x200B;**添加新的Analytics配置**&#x200B;以添加多个配置。

已创建新的配置更改，可以随时更新服务器。 要使用&#x200B;**Analytics配置**&#x200B;部分中的新Analytics配置，请继续[审核和推送更改](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md)流程。

### 集成 {#integrations}

此选项卡显示当前所选渠道与MVPD之间可用集成的列表。 该列表会显示每个集成及其状态，指示集成是否已启用。 从此列表中选择特定集成以访问[集成](tve-dashboard-integrations.md)部分中的详细信息。

![可用集成列表](/help/authentication/assets/tve-dashboard/new-tve-dashboard/channels/channel-integrations-tab-view.png)

*可用集成列表*

### 证书 {#certificates}

此选项卡显示用户元数据加密流中使用的[可用证书](#available-certificates)和[继承的可用证书](#inherited-avail-certificates)的列表。 它显示有关每个证书的详细信息，其中包括：

* 状态（是否启用&#x200B;**用户元数据加密**&#x200B;用法）
* 序列号
* 颁发者组织的名称
* 主题组织的名称
* 颁发日期
* 到期日期
* 用于加密用户元数据的下拉菜单（如果选择&#x200B;**是**，证书将加密敏感的用户信息，如邮政编码值）。

#### 可用的证书 {#available-certificates}

这些证书用作私钥或公钥，用于用户元数据加密。
您可以在可用证书部分下进行以下更改：

* [添加新证书](#add-new-certificate)
* [删除证书](#delete-certificate)

##### 添加新证书 {#add-new-certificate}

要添加新证书，请执行以下步骤：

1. 在&#x200B;**可用证书**&#x200B;部分的顶部选择&#x200B;**添加新证书**。

   ![添加新证书](/help/authentication/assets/tve-dashboard/new-tve-dashboard/channels/channel-add-new-certificate-button.png)

   *添加新证书*

1. 将证书的公钥粘贴到&#x200B;**新建证书**&#x200B;对话框中。

1. 选择&#x200B;**添加证书**。

1. 在&#x200B;**可用证书**&#x200B;的列表中找到新证书。

   >[!IMPORTANT]
   >
   > 确保您的系统为最新版本，并且已准备好使用新证书。

1. 从&#x200B;**用于加密用户元数据**&#x200B;下拉菜单中选择&#x200B;**是**&#x200B;以激活新证书。

已创建新的配置更改，可以随时更新服务器。 若要使用&#x200B;**可用证书**&#x200B;部分中列出的新证书，请继续[审阅和推送更改](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md)流程。

##### 删除证书 {#delete-certificate}

按照以下步骤删除证书。

1. 将鼠标悬停在要从&#x200B;**可用证书**&#x200B;列表中删除的证书上。

1. 选择&#x200B;**删除**。

   ![删除所选的证书](/help/authentication/assets/tve-dashboard/new-tve-dashboard/channels/channel-delete-certificate-button.png)

   *删除所选的证书*

1. 从&#x200B;**删除活动证书**&#x200B;对话框中选择&#x200B;**删除**。

已创建新的配置更改，可以随时更新服务器。 仅在&#x200B;**审阅和推送更改**&#x200B;后，证书才会从[可用证书](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md)部分中删除。

#### 继承的可用证书 {#inherited-avail-certificates}

媒体公司在其自己的级别定义这些证书。 与同一媒体公司关联的所有渠道都可以使用这些证书。

![继承的可用证书](/help/authentication/assets/tve-dashboard/new-tve-dashboard/channels/channel-inherited-available-certificates-panel-view.png)

*继承的可用证书*

### 域 {#domains}

此选项卡显示可用域的列表，各个渠道可通过这些域与Adobe Pass身份验证进行通信。

您可以对域进行以下更改：

* [添加新域](#add-domains)
* [删除域](#delete-domain)

>[!TIP]
>
> 如果列表中存在更一般的域，请避免添加新子域。

#### 添加新域 {#add-domains}

按照以下步骤添加域。

1. 选择&#x200B;**可用域**&#x200B;部分的右上角的&#x200B;**添加新域**。

   ![添加新域](/help/authentication/assets/tve-dashboard/new-tve-dashboard/channels/channel-add-new-domain-button.png)

   *添加新域*

1. 在&#x200B;**新建域**&#x200B;对话框中键入您的域名称。

1. 选择&#x200B;**添加域**&#x200B;为所选渠道添加新域。

已创建新的配置更改，可以随时更新服务器。 若要使用&#x200B;**可用域**&#x200B;部分中列出的新域，请继续[审阅和推送更改](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md)流程。

#### 删除域 {#delete-domain}

按照以下步骤删除域。

1. 将鼠标悬停在要从&#x200B;**可用域**&#x200B;列表中删除的域上。

1. 选择&#x200B;**删除**。

   ![删除选定的域](/help/authentication/assets/tve-dashboard/new-tve-dashboard/channels/channel-remove-domain-button.png)

   *删除选定的域*

1. 在&#x200B;**删除域**&#x200B;对话框中选择&#x200B;**删除**。

已创建新的配置更改，可以随时更新服务器。 只有在&#x200B;**审阅并推送更改**&#x200B;后，才会从[可用域](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md)部分删除该域。

所选域不再可用。 因此，与此域关联的应用程序将无法访问Adobe Pass身份验证服务。

### 已注册的应用程序 {#registered-applications}

此选项卡显示已注册应用程序的列表。 有关已注册应用程序使用的更多详细信息，请参阅[动态客户端注册概述](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)文档。

您可以对已注册的应用程序执行以下操作：

* [添加新注册的应用程序](#add-registered-applications)
* [下载软件声明](#download-software-statement)

#### 添加新的已注册应用程序 {#add-registered-applications}

按照以下步骤添加新注册的应用程序。

1. 在&#x200B;**已注册的应用程序**&#x200B;部分的右上角选择&#x200B;**添加新应用程序**。

   ![添加新应用程序](/help/authentication/assets/tve-dashboard/new-tve-dashboard/channels/channel-add-new-application-button.png)

   *添加新应用程序*

1. 从&#x200B;**新建应用程序**&#x200B;对话框的下拉菜单中选择&#x200B;**平台**。

   >[!IMPORTANT]
   >
   > 建议创建具有更具体且更有限权限的已注册应用程序，以增强安全性并防止未经授权的访问。 因此，在创建已注册的应用程序时，请考虑对分配的`platforms`使用更窄的选项。

1. 从下拉菜单中选择&#x200B;**域**。

   >[!IMPORTANT]
   >
   > 在客户端注册过程中，客户端应用程序可请求允许使用重定向URL来最终确定身份验证流。 当客户端应用程序使用特定的重定向URL时，将针对在此选择中选择的`domains`验证该URL。

1. 键入应用程序的&#x200B;**名称**。

1. 键入应用程序的&#x200B;**版本**。

   >[!IMPORTANT]
   >
   > 建议为客户端应用程序的每次重大更新创建新的注册应用程序，以管理其生命周期和使用情况。 如有必要，请通过我们的[Zendesk](https://adobeprimetime.zendesk.com)创建一个票证，然后要求您的技术客户经理(TAM)撤销已注册的应用程序，以阻止特定客户端应用程序版本的功能。

1. 从下拉菜单中选择&#x200B;**Type**&#x200B;值“直接”。

1. 选择&#x200B;**添加应用程序**。

已创建新的配置更改，可以随时更新服务器。 若要使用&#x200B;**已注册应用程序**&#x200B;部分中列出的新已注册应用程序，请继续[审阅和推送更改](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md)流程。

#### Download软件声明 {#download-software-statement}

按照以下步骤下载软件语句。

1. 将鼠标悬停在要从&#x200B;**注册应用程序**&#x200B;列表中下载软件语句的注册应用程序上。

1. 选择&#x200B;**下载**。

   ![下载软件语句](/help/authentication/assets/tve-dashboard/new-tve-dashboard/channels/channel-download-software-statement-button.png)

   *下载软件语句*

### 自定义架构 {#custom-schemes}

此选项卡显示自定义方案列表。 有关自定义方案使用的更多详细信息，请参阅[iOS/tvOS应用程序注册](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-application-registration.md)。

您可以对自定义方案进行以下更改：

* [生成新的自定义方案](#generate-custom-schemes)

#### 生成新的自定义方案 {#generate-custom-schemes}

按照以下步骤生成新的自定义方案。

1. 选择&#x200B;**生成新的自定义方案**。

   ![生成新的自定义方案](/help/authentication/assets/tve-dashboard/new-tve-dashboard/channels/channel-add-new-custom-scheme-button.png)

   *生成新的自定义方案*

已创建新的配置更改，可以随时更新服务器。 若要使用&#x200B;**自定义方案**&#x200B;部分中列出的新自定义方案，请继续[审核并推送更改](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md)流程。

#### 继承的自定义架构 {#inherited-custom-schemes}

媒体公司根据自己的级别定义这些自定义方案。 与同一媒体公司关联的所有渠道都可以使用这些自定义方案。

![继承的自定义方案](/help/authentication/assets/tve-dashboard/new-tve-dashboard/channels/channel-inherited-custom-schemes-panel-view.png)

*继承的自定义方案*

## 添加新渠道 {#add-new-channel}

按照以下步骤添加新渠道。

1. 在左侧面板中选择&#x200B;**渠道**&#x200B;选项卡。

1. 选择&#x200B;**渠道**&#x200B;部分的右上角的&#x200B;**添加新渠道**。

   ![添加新频道](/help/authentication/assets/tve-dashboard/new-tve-dashboard/channels/channel-add-new-channel-button.png)

   *添加新频道*

1. 从&#x200B;**新建渠道**&#x200B;对话框的下拉菜单中选择&#x200B;**程序员ID**。

1. 在&#x200B;**渠道ID**&#x200B;中键入唯一标识符。

1. 在&#x200B;**显示名称**&#x200B;中键入用于商业目的的渠道的品牌名称。

1. 选择&#x200B;**添加频道**。

已创建新的配置更改，可以随时更新服务器。 若要使用&#x200B;**渠道**&#x200B;部分中列出的新渠道，请继续[审核和推送更改](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md)流程。
