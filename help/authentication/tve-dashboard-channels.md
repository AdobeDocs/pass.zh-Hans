---
title: 渠道
description: 了解TVE仪表板中的渠道及其各种配置。
source-git-commit: b81cc7498a8035f4c274ba25952dcd1dcd8d71f5
workflow-type: tm+mt
source-wordcount: '1140'
ht-degree: 0%

---


# 渠道 {#channels}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

此 **渠道** 通过TVE仪表板的部分，您可以查看和管理与特定程序员关联的渠道设置。 您还可以 [添加新渠道](#add-new-channel) 根据您的要求。

此 **渠道** 选项卡在左侧面板中显示一个链接渠道列表，其中包含以下详细信息：

* **显示名称**：用于商业目的的渠道的品牌名称。
* **渠道ID**：唯一标识符，也称为请求者ID。
* **集成**：与建立的连接数 [MVPDs](/help/authentication/glossary.md#mvpd).

![现有渠道的列表](assets/channels-list.png)

*现有渠道的列表*

在中键入渠道的名称 **Search** 栏了解渠道的更多信息。

## 管理渠道配置 {#manage-channel-conf}

按照步骤管理特定渠道的各种设置。

1. 选择 **渠道** 选项卡。
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
> 视图 [审核和推送更改](/help/authentication/tve-dashboard-review-push-changes.md) 有关激活配置更改的详细信息。

### 常规设置 {#general-settings}

此选项卡将显示 **渠道信息** 和 **Analytics配置**.

#### 渠道信息 {#channel-information}

在此部分中，可以编辑以下详细信息：

* **显示名称**：用于商业目的的渠道的品牌名称。

* **默认重定向URL**：用于身份验证和注销的备份重定向URL。

* **错误报告**：在选择时 **是**，Adobe Pass SDK会将错误报告发送到Adobe Pass后端以进行Analytics。

![编辑渠道信息](assets/channel-information.png)

*编辑渠道信息*

#### Analytics配置 {#analytics-configuration}

在此部分中，您可以配置将Adobe Pass身份验证事件转发到Adobe Analytics。

要启用 **Analytics配置**，请联系您的技术客户经理(TAM) ，以了解有关设置报表包ID (RSID)的更多详细信息。

![启用Analytics配置](assets/channel-analytical-conf.png)

*启用Analytics配置*

选择 **添加新的Analytics配置** 以添加多个配置。

已创建新的配置更改，可以随时更新服务器。 要使用新的分析配置，请访问 **Analytics配置** 部分，继续执行 [审阅和推送更改](/help/authentication/tve-dashboard-review-push-changes.md) 流量。

### 集成 {#integrations}

此选项卡显示当前所选渠道与MVPD之间可用集成的列表。 该列表会显示每个集成及其状态，指示集成是否已启用。 从此列表中选择特定集成以访问 [集成](tve-dashboard-integrations.md) 部分。

![可用集成列表](assets/channel-integrations.png)

*可用集成列表*

### 证书 {#certificates}

此选项卡显示以下列表 [可用证书](#available-certificates) 和 [继承的可用证书](#inherited-avail-certificates) 在用户元数据加密流中使用。 它显示有关每个证书的详细信息，其中包括：

* 状态(是否启用 **用户元数据加密** 是否使用)
* 序列号
* 颁发者组织的名称
* 主题组织的名称
* 颁发日期
* 到期日期
* 用于加密用户元数据的下拉菜单(如果选择 **是**，则证书加密敏感的用户信息，如邮政编码值)。

#### 可用的证书 {#available-certificates}

这些证书用作私钥或公钥，用于用户元数据加密。
您可以在可用证书部分下进行以下更改：

* [添加新证书](#add-new-certificate)
* [删除证书](#delete-certificate)

##### 添加新证书 {#add-new-certificate}

要添加新证书，请执行以下步骤：

1. 选择 **添加新证书** 在顶部 **可用证书** 部分。

   ![添加新证书](assets/add-new-certificate.png)

   *添加新证书*

1. 将证书的公钥粘贴到 **新建证书** 对话框。
1. 选择 **添加证书**.

   已创建新的配置更改，可以随时更新服务器。 要使用 **可用证书** 部分，继续执行 [审阅和推送更改](/help/authentication/tve-dashboard-review-push-changes.md) 流量。

1. 在列表中找到新证书 **可用证书**.

   >[!IMPORTANT]
   >
   > 确保您的系统为最新版本，并且已准备好使用新证书。

1. 选择 **是** 从 **用于加密用户元数据** 下拉菜单以激活新证书。

##### 删除证书 {#delete-certificate}

按照以下步骤删除证书。

1. 将鼠标悬停在要从列表中删除的证书上 **可用的证书**.
1. 选择 **移除**.

   ![删除所选证书](assets/channel-delete-certificate.png)

   *删除所选证书*

1. 选择 **删除** 从 **删除活动证书** 对话框。

已创建新的配置更改，可以随时更新服务器。 证书将从 **可用的证书** 部分仅晚于 [审阅和推送更改](/help/authentication/tve-dashboard-review-push-changes.md).

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

1. 选择 **添加新域** 位于的右上角 **可用域** 部分。

   ![添加新域](assets/add-new-domain.png)

   *添加新域*

1. 在中键入您的域的名称 **新域** 对话框。

1. 选择 **添加域** ，以便为所选渠道添加新域。

已创建新的配置更改，可以随时更新服务器。 要使用 **可用域** 部分，继续执行 [审阅和推送更改](/help/authentication/tve-dashboard-review-push-changes.md) 流量。

#### 删除域 {#delete-domain}

按照以下步骤删除域。

1. 将鼠标悬停在要从列表中删除的域上 **可用域**.
1. 选择 **移除**.

   ![删除所选域](assets/remove-domain.png)

   *删除所选域*

1. 选择 **删除** 在 **删除域** 对话框。

已创建新的配置更改，可以随时更新服务器。 域将从 **可用域** 部分仅晚于 [审阅和推送更改](/help/authentication/tve-dashboard-review-push-changes.md).

所选域不再可用。 因此，与此域关联的应用程序将无法访问Adobe Pass身份验证服务。

### 已注册的应用程序 {#registered-applications}

此选项卡提供应用程序注册的列表。 视图 [动态客户端注册管理](/help/authentication/dynamic-client-registration-management.md) 以了解更多信息。

### 自定义架构 {#custom-schemes}

此选项卡显示自定义方案列表。 视图 [iOS/tvOS应用程序注册](/help/authentication/iostvos-application-registration.md) 和 [动态客户端注册管理](/help/authentication/dynamic-client-registration-management.md) 以了解更多信息。

## 添加新渠道 {#add-new-channel}

按照以下步骤添加新渠道。

1. 选择 **渠道** 选项卡。
1. 选择 **添加新渠道** 位于的右上角 **渠道** 部分。

   ![添加新渠道](assets/add-new-channel.png)

   *添加新渠道*

1. 选择 **程序员Id** 从的下拉菜单中 **新建渠道** 对话框。

1. 在中键入唯一标识符 **渠道ID**.
1. 在中键入用于商业目的的渠道的品牌名称 **显示名称**.
1. 选择 **添加渠道**.

已创建新的配置更改，可以随时更新服务器。 要使用中列出的新渠道，请执行以下操作 **渠道** 部分，继续执行 [审阅和推送更改](/help/authentication/tve-dashboard-review-push-changes.md) 流量。

