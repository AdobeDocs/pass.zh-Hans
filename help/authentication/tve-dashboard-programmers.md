---
title: 程序员
description: 了解TVE仪表板中的程序员及其配置。
source-git-commit: b81cc7498a8035f4c274ba25952dcd1dcd8d71f5
workflow-type: tm+mt
source-wordcount: '717'
ht-degree: 0%

---

# 程序员 {#programmers}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

此 **程序员** 区域的“ ”图标，用于查看和管理 [程序员](/help/authentication/glossary.md#programmer) 链接到您的帐户权利。 您还可以 [添加新程序员](#add-new-programmer) 根据您的要求。

此 **程序员** 选项卡在左侧面板中显示现有程序员的列表，其中包括以下详细信息：

* **程序员Id**：系统中的媒体公司标识符。
* **渠道**：链接到程序员的关联渠道数。

![现有程序员列表](assets/programmers-list.png)

*现有程序员列表*

键入程序员的姓名 **Search** ，了解更多有关程序员的知识。

## 管理程序员配置 {#manage-programmer-conf}

按照以下步骤管理特定程序员的各种设置。

1. 选择 **程序员** 选项卡。
1. 从列表中选择程序员。
1. 选择以下选项卡之一以查看和编辑所选程序员的相应设置：

   * [渠道](#channels)
   * [证书](#certificates)
   * [已注册的应用程序](#registered-applications)
   * [自定义架构](#custom-schemes)

   ![程序员设置](assets/programmer-settings.png)

   *程序员设置*

>[!IMPORTANT]
>
> 视图 [审核和推送更改](/help/authentication/tve-dashboard-review-push-changes.md) 有关激活配置更改的详细信息。

### 渠道 {#channels}

此选项卡显示链接到当前程序员的渠道列表。 从此列表中选择特定渠道以访问 [渠道](/help/authentication/tve-dashboard-channels.md) 部分。

要为所选程序员添加新渠道，请选择 **添加新渠道** 从的右上角 **可用渠道** 部分。 学习 [如何添加新渠道](/help/authentication/tve-dashboard-channels.md#add-new-channel).

![添加新渠道](assets/programmers-channels.png)

*添加新渠道*

### 证书 {#certificates}

此选项卡显示以下列表 [可用证书](#available-certificates) 在用户元数据加密流中使用。 它显示有关每个证书的详细信息，其中包括：

* 状态(是否启用 **用户元数据加密** 是否使用)
* 序列号
* 颁发者组织的名称
* 主题组织的名称
* 颁发日期
* 到期日期
* 用于加密用户元数据的下拉菜单(如果选择 **是**，则证书将加密敏感的用户信息，如邮政编码值)。

#### 可用的证书 {#available-certificates}

这些证书用作私钥或公钥，用于用户元数据加密。 与同一媒体公司关联的所有渠道都可以使用这些证书。

您可以对可用证书进行以下更改：

* [添加新证书](#add-new-certificate)
* [删除证书](#delete-certificate)

##### 添加新证书 {#add-new-certificate}

按照以下步骤添加新证书。

1. 选择 **添加新证书** 位于的右上角 **可用证书** 部分。

   ![添加新证书](assets/programmer-add-new-certificate.png)

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

   ![删除所选证书](assets/programmer-remove-certificate.png)

   *删除所选证书*

1. 选择 **删除** 在 **删除证书** 对话框。

已创建新的配置更改，可以随时更新服务器。 证书将从 **可用的证书** 部分仅晚于 [审阅和推送更改](/help/authentication/tve-dashboard-review-push-changes.md).

### 已注册的应用程序 {#registered-applications}

此选项卡提供应用程序注册的列表。 视图 [动态客户端注册管理](/help/authentication/dynamic-client-registration-management.md)，以了解更多信息。

### 自定义架构 {#custom-schemes}

此选项卡显示自定义方案列表。 视图 [iOS/tvOS应用程序注册](/help/authentication/iostvos-application-registration.md) 和 [动态客户端注册管理](/help/authentication/dynamic-client-registration-management.md)，以了解更多信息。

## 添加新程序员 {#add-new-programmer}

按照以下步骤添加新程序员实体。

1. 选择 **程序员** 选项卡。
1. 选择 **添加新程序员** 位于的右上角 **程序员** 部分。

   ![添加新程序员](assets/add-new-programmer.png)

   *添加新程序员*

1. 在中键入媒体公司标识符 **程序员Id** 在 **新程序员** 对话框。
1. 键入要在控制台中显示的商业品牌名称，位于 **显示名称**.
1. 选择 **添加程序员**.

已创建新的配置更改，可以随时更新服务器。 要使用 **程序员** 部分，继续执行 [审阅和推送更改](/help/authentication/tve-dashboard-review-push-changes.md) 流量。

