---
title: 程序员
description: 了解TVE仪表板中的程序员及其配置。
exl-id: b450d7cc-d5b5-4454-8f95-8047856bfb98
source-git-commit: 5622cad15383560e19e8111f12a1460e9b118efe
workflow-type: tm+mt
source-wordcount: '1139'
ht-degree: 0%

---

# 程序员 {#programmers}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

TVE仪表板的&#x200B;**程序员**&#x200B;部分允许您查看和管理链接到帐户权利的[程序员](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#programmer)的设置。 您还可以根据需要[添加新程序员](#add-new-programmer)。

左侧面板中的&#x200B;**程序员**&#x200B;选项卡显示现有程序员的列表，其中包含以下详细信息：

* **程序员ID**：系统中的媒体公司标识符。
* **渠道**：链接到程序员的关联渠道数。

![现有程序员列表](../assets/tve-dashboard/new-tve-dashboard/programmers/programmers-list-view.png)

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

   ![程序员设置](../assets/tve-dashboard/new-tve-dashboard/programmers/programmer-tabs-view.png)

   *程序员设置*

>[!IMPORTANT]
>
> 查看[查看并推送更改](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md)以了解有关激活配置更改的详细信息。

### 渠道 {#channels}

此选项卡显示链接到当前程序员的渠道列表。 从此列表中选择特定渠道以访问[渠道](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md)部分中的详细信息。

要为所选程序员添加新频道，请从&#x200B;**可用频道**&#x200B;部分的右上角选择&#x200B;**添加新频道**。 了解[如何添加新渠道](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#add-new-channel)。

![添加新频道](../assets/tve-dashboard/new-tve-dashboard/programmers/programmer-add-new-channel-button.png)

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

   ![添加新证书](../assets/tve-dashboard/new-tve-dashboard/programmers/programmer-add-new-certificate-button.png)

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

   ![删除所选的证书](../assets/tve-dashboard/new-tve-dashboard/programmers/programmer-remove-certificate-button.png)

   *删除所选的证书*

1. 在&#x200B;**删除证书**&#x200B;对话框中选择&#x200B;**删除**。

已创建新的配置更改，可以随时更新服务器。 仅在&#x200B;**审阅和推送更改**&#x200B;后，证书才会从[可用证书](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md)部分中删除。

### 已注册的应用程序 {#registered-applications}

此选项卡显示已注册应用程序的列表。 有关已注册应用程序使用的更多详细信息，请参阅[动态客户端注册概述](../integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)文档。

您可以对已注册的应用程序执行以下操作：

* [添加新注册的应用程序](#add-registered-applications)
* [下载软件声明](#download-software-statement)

#### 添加新的已注册应用程序 {#add-registered-applications}

按照以下步骤添加新注册的应用程序。

1. 在&#x200B;**已注册的应用程序**&#x200B;部分的右上角选择&#x200B;**添加新应用程序**。

   ![添加新应用程序](../assets/tve-dashboard/new-tve-dashboard/programmers/programmer-add-new-application-button.png)

   *添加新应用程序*

1. 从&#x200B;**新建应用程序**&#x200B;对话框的下拉菜单中选择&#x200B;**分配给渠道**。

   >[!IMPORTANT]
   >
   > 建议创建具有更具体且更有限权限的已注册应用程序，以增强安全性并防止未经授权的访问。 因此，在创建已注册的应用程序时，请考虑对分配的`channel`使用更窄的选项。

1. 从下拉菜单中选择&#x200B;**平台**。

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

   ![下载软件语句](../assets/tve-dashboard/new-tve-dashboard/programmers/programmer-download-software-statement-button.png)

   *下载软件语句*


### 自定义架构 {#custom-schemes}

此选项卡显示自定义方案列表。 有关自定义方案使用的更多详细信息，请参阅[iOS/tvOS应用程序注册](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-application-registration.md)。

您可以对自定义方案进行以下更改：

* [生成新的自定义方案](#generate-custom-schemes)

#### 生成新的自定义方案 {#generate-custom-schemes}

按照以下步骤生成新的自定义方案。

1. 选择&#x200B;**生成新的自定义方案**。

   ![生成新的自定义方案](../assets/tve-dashboard/new-tve-dashboard/programmers/programmer-add-new-custom-scheme-button.png)

   *生成新的自定义方案*

已创建新的配置更改，可以随时更新服务器。 若要使用&#x200B;**自定义方案**&#x200B;部分中列出的新自定义方案，请继续[审核并推送更改](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md)流程。

## 添加新程序员 {#add-new-programmer}

按照以下步骤添加新程序员实体。

1. 在左侧面板中选择&#x200B;**程序员**&#x200B;选项卡。

1. 在&#x200B;**程序员**&#x200B;部分的右上角选择&#x200B;**添加新程序员**。

   ![添加新程序员](../assets/tve-dashboard/new-tve-dashboard/programmers/programmer-add-new-programmer-button.png)

   *添加新程序员*

1. 在&#x200B;**新建程序员**&#x200B;对话框的&#x200B;**程序员ID**&#x200B;中键入媒体公司标识符。

1. 在&#x200B;**显示名称**&#x200B;下键入要显示在控制台中的商业品牌名称。

1. 选择&#x200B;**添加程序员**。

已创建新的配置更改，可以随时更新服务器。 若要使用&#x200B;**程序员**&#x200B;部分中列出的新程序员，请继续[审阅和推送更改](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md)流程。
