---
title: 渠道
description: 了解TVE仪表板中的渠道及其各种配置。
exl-id: bbddeccb-6b6f-4a8f-87ab-d4af538eee1d
source-git-commit: c2dcea9e4170a3e10654bcd3f8d2f5cdb82c9603
workflow-type: tm+mt
source-wordcount: '1140'
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
* **集成**：与[MVPD](/help/authentication/glossary.md#mvpd)建立的连接数。

![现有渠道列表](assets/channels-list.png)

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

   ![渠道设置](assets/channel-settings.png)

   *渠道设置*

>[!IMPORTANT]
>
> 查看[查看并推送更改](/help/authentication/tve-dashboard-review-push-changes.md)以了解有关激活配置更改的详细信息。

### 常规设置 {#general-settings}

此选项卡显示&#x200B;**渠道信息**&#x200B;和&#x200B;**分析配置**。

#### 渠道信息 {#channel-information}

在此部分中，可以编辑以下详细信息：

* **显示名称**：用于商业目的的渠道的品牌名称。

* **默认重定向URL**：用于身份验证和注销的备份重定向URL。

* **错误报告**：选择&#x200B;**是**&#x200B;时，Adobe Pass SDK将错误报告发送到Adobe Pass后端以进行Analytics。

![编辑频道信息](assets/channel-information.png)

*编辑频道信息*

#### Analytics配置 {#analytics-configuration}

在此部分中，您可以配置将Adobe Pass身份验证事件转发到Adobe Analytics。

要启用&#x200B;**Analytics配置**，请联系您的技术客户经理(TAM)，了解有关设置报表包ID (RSID)的详细信息。

![启用Analytics配置](assets/channel-analytical-conf.png)

*启用Analytics配置*

选择&#x200B;**添加新的Analytics配置**&#x200B;以添加多个配置。

已创建新的配置更改，可以随时更新服务器。 要使用&#x200B;**Analytics配置**&#x200B;部分中的新Analytics配置，请继续[审核和推送更改](/help/authentication/tve-dashboard-review-push-changes.md)流程。

### 集成 {#integrations}

此选项卡显示当前所选渠道与MVPD之间可用集成的列表。 该列表会显示每个集成及其状态，指示集成是否已启用。 从此列表中选择特定集成以访问[集成](tve-dashboard-integrations.md)部分中的详细信息。

![可用集成列表](assets/channel-integrations.png)

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

   ![添加新证书](assets/add-new-certificate.png)

   *添加新证书*

1. 将证书的公钥粘贴到&#x200B;**新建证书**&#x200B;对话框中。
1. 选择&#x200B;**添加证书**。

   已创建新的配置更改，可以随时更新服务器。 若要使用&#x200B;**可用证书**&#x200B;部分中列出的新证书，请继续[审阅和推送更改](/help/authentication/tve-dashboard-review-push-changes.md)流程。

1. 在&#x200B;**可用证书**&#x200B;的列表中找到新证书。

   >[!IMPORTANT]
   >
   > 确保您的系统为最新版本，并且已准备好使用新证书。

1. 从&#x200B;**用于加密用户元数据**&#x200B;下拉菜单中选择&#x200B;**是**&#x200B;以激活新证书。

##### 删除证书 {#delete-certificate}

按照以下步骤删除证书。

1. 将鼠标悬停在要从&#x200B;**可用证书**&#x200B;列表中删除的证书上。
1. 选择&#x200B;**删除**。

   ![删除所选的证书](assets/channel-delete-certificate.png)

   *删除所选的证书*

1. 从&#x200B;**删除活动证书**&#x200B;对话框中选择&#x200B;**删除**。

已创建新的配置更改，可以随时更新服务器。 仅在[审阅和推送更改](/help/authentication/tve-dashboard-review-push-changes.md)后，证书才会从&#x200B;**可用证书**&#x200B;部分中删除。

#### 继承的可用证书 {#inherited-avail-certificates}

媒体公司在其自己的级别定义这些证书。 与同一媒体公司关联的所有渠道都可以使用这些证书。

![继承的可用证书](assets/inherited-available-certificates.png)

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

   ![添加新域](assets/add-new-domain.png)

   *添加新域*

1. 在&#x200B;**新建域**&#x200B;对话框中键入您的域名称。

1. 选择&#x200B;**添加域**&#x200B;为所选渠道添加新域。

已创建新的配置更改，可以随时更新服务器。 若要使用&#x200B;**可用域**&#x200B;部分中列出的新域，请继续[审阅和推送更改](/help/authentication/tve-dashboard-review-push-changes.md)流程。

#### 删除域 {#delete-domain}

按照以下步骤删除域。

1. 将鼠标悬停在要从&#x200B;**可用域**&#x200B;列表中删除的域上。
1. 选择&#x200B;**删除**。

   ![删除选定的域](assets/remove-domain.png)

   *删除选定的域*

1. 在&#x200B;**删除域**&#x200B;对话框中选择&#x200B;**删除**。

已创建新的配置更改，可以随时更新服务器。 只有在[审阅并推送更改](/help/authentication/tve-dashboard-review-push-changes.md)后，才会从&#x200B;**可用域**&#x200B;部分删除该域。

所选域不再可用。 因此，与此域关联的应用程序将无法访问Adobe Pass身份验证服务。

### 已注册的应用程序 {#registered-applications}

此选项卡提供应用程序注册的列表。 查看[Dynamic client注册管理](/help/authentication/dynamic-client-registration-management.md)以了解详细信息。

### 自定义架构 {#custom-schemes}

此选项卡显示自定义方案列表。 有关详细信息，请查看[iOS/tvOS应用程序注册](/help/authentication/iostvos-application-registration.md)和[动态客户端注册管理](/help/authentication/dynamic-client-registration-management.md)。

## 添加新渠道 {#add-new-channel}

按照以下步骤添加新渠道。

1. 在左侧面板中选择&#x200B;**渠道**&#x200B;选项卡。
1. 选择&#x200B;**渠道**&#x200B;部分的右上角的&#x200B;**添加新渠道**。

   ![添加新频道](assets/add-new-channel.png)

   *添加新频道*

1. 从&#x200B;**新建渠道**&#x200B;对话框的下拉菜单中选择&#x200B;**程序员ID**。

1. 在&#x200B;**渠道ID**&#x200B;中键入唯一标识符。
1. 在&#x200B;**显示名称**&#x200B;中键入用于商业目的的渠道的品牌名称。
1. 选择&#x200B;**添加频道**。

已创建新的配置更改，可以随时更新服务器。 若要使用&#x200B;**渠道**&#x200B;部分中列出的新渠道，请继续[审核和推送更改](/help/authentication/tve-dashboard-review-push-changes.md)流程。
