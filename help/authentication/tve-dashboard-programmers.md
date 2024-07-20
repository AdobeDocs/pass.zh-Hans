---
title: 程序员
description: 了解TVE仪表板中的程序员及其配置。
exl-id: b450d7cc-d5b5-4454-8f95-8047856bfb98
source-git-commit: c2dcea9e4170a3e10654bcd3f8d2f5cdb82c9603
workflow-type: tm+mt
source-wordcount: '717'
ht-degree: 0%

---

# 程序员 {#programmers}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

TVE仪表板的&#x200B;**程序员**&#x200B;部分允许您查看和管理链接到帐户权利的[程序员](/help/authentication/glossary.md#programmer)的设置。 您还可以根据需要[添加新程序员](#add-new-programmer)。

左侧面板中的&#x200B;**程序员**&#x200B;选项卡显示现有程序员的列表，其中包含以下详细信息：

* **程序员ID**：系统中的媒体公司标识符。
* **渠道**：链接到程序员的关联渠道数。

![现有程序员列表](assets/programmers-list.png)

*现有程序员列表*

在列表上方的&#x200B;**Search**&#x200B;栏中键入程序员的姓名，了解有关程序员的更多信息。

## 管理程序员配置 {#manage-programmer-conf}

按照以下步骤管理特定程序员的各种设置。

1. 在左侧面板中选择&#x200B;**程序员**&#x200B;选项卡。
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
> 查看[查看并推送更改](/help/authentication/tve-dashboard-review-push-changes.md)以了解有关激活配置更改的详细信息。

### 渠道 {#channels}

此选项卡显示链接到当前程序员的渠道列表。 从此列表中选择特定渠道以访问[渠道](/help/authentication/tve-dashboard-channels.md)部分中的详细信息。

要为所选程序员添加新频道，请从&#x200B;**可用频道**&#x200B;部分的右上角选择&#x200B;**添加新频道**。 了解[如何添加新渠道](/help/authentication/tve-dashboard-channels.md#add-new-channel)。

![添加新频道](assets/programmers-channels.png)

*添加新频道*

### 证书 {#certificates}

此选项卡显示用户元数据加密流中使用的[可用证书](#available-certificates)的列表。 它显示有关每个证书的详细信息，其中包括：

* 状态（是否启用&#x200B;**用户元数据加密**&#x200B;用法）
* 序列号
* 颁发者组织的名称
* 主题组织的名称
* 颁发日期
* 到期日期
* 用于加密用户元数据的下拉菜单（如果选择&#x200B;**是**，证书将加密敏感的用户信息，如邮政编码值）。

#### 可用的证书 {#available-certificates}

这些证书用作私钥或公钥，用于用户元数据加密。 与同一媒体公司关联的所有渠道都可以使用这些证书。

您可以对可用证书进行以下更改：

* [添加新证书](#add-new-certificate)
* [删除证书](#delete-certificate)

##### 添加新证书 {#add-new-certificate}

按照以下步骤添加新证书。

1. 在&#x200B;**可用证书**&#x200B;部分的右上角选择&#x200B;**添加新证书**。

   ![添加新证书](assets/programmer-add-new-certificate.png)

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

   ![删除所选的证书](assets/programmer-remove-certificate.png)

   *删除所选的证书*

1. 在&#x200B;**删除证书**&#x200B;对话框中选择&#x200B;**删除**。

已创建新的配置更改，可以随时更新服务器。 仅在[审阅和推送更改](/help/authentication/tve-dashboard-review-push-changes.md)后，证书才会从&#x200B;**可用证书**&#x200B;部分中删除。

### 已注册的应用程序 {#registered-applications}

此选项卡提供应用程序注册的列表。 查看[动态客户端注册管理](/help/authentication/dynamic-client-registration-management.md)，以了解详细信息。

### 自定义架构 {#custom-schemes}

此选项卡显示自定义方案列表。 查看[iOS/tvOS应用程序注册](/help/authentication/iostvos-application-registration.md)和[动态客户端注册管理](/help/authentication/dynamic-client-registration-management.md)，以了解详细信息。

## 添加新程序员 {#add-new-programmer}

按照以下步骤添加新程序员实体。

1. 在左侧面板中选择&#x200B;**程序员**&#x200B;选项卡。
1. 在&#x200B;**程序员**&#x200B;部分的右上角选择&#x200B;**添加新程序员**。

   ![添加新程序员](assets/add-new-programmer.png)

   *添加新程序员*

1. 在&#x200B;**新建程序员**&#x200B;对话框的&#x200B;**程序员ID**&#x200B;中键入媒体公司标识符。
1. 在&#x200B;**显示名称**&#x200B;下键入要显示在控制台中的商业品牌名称。
1. 选择&#x200B;**添加程序员**。

已创建新的配置更改，可以随时更新服务器。 若要使用&#x200B;**程序员**&#x200B;部分中列出的新程序员，请继续[审阅和推送更改](/help/authentication/tve-dashboard-review-push-changes.md)流程。
