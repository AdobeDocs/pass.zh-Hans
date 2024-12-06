---
title: TVE功能板集成
description: 了解您的渠道和MVPD之间的集成以及如何管理集成。
exl-id: 0add340b-120c-4e82-8e3c-6c190d77cf7e
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '2093'
ht-degree: 0%

---

# 集成

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

TVE仪表板的&#x200B;**集成**&#x200B;部分允许您查看和管理渠道与MVPD之间的集成设置。 您还可以[根据需要创建新的集成](#create-new-integration)。

左侧面板中的&#x200B;**集成**&#x200B;选项卡显示现有集成的列表，详细信息如下：

* 指示集成当前是活动还是不活动的状态
* 将特定渠道与相应的MVPD关联的集成
* 具有渠道ID的渠道名称
* MVPD显示名称和MVPD ID

![现有集成的列表](../assets/tve-dashboard/new-tve-dashboard/integrations/integrations-list.png)

*现有集成的列表*

在列表上方的&#x200B;**搜索**&#x200B;栏中键入渠道或MVPD的名称，了解有关集成的详细信息。

## 管理集成配置 {#manage-integration-conf}

请按照以下步骤管理特定集成。

1. 在左侧面板中选择&#x200B;**集成**&#x200B;选项卡。
1. 从提供的列表中选择集成，以查看和编辑以下部分中的各种设置：

   * [端点选择](#endpoint-selection)
   * [平台设置](#platform-settings)
   * [用户元数据](#user-metadata)

>[!IMPORTANT]
>
> 查看[查看并推送更改](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md)以了解有关激活配置更改的详细信息。

### 端点选择 {#endpoint-selection}

此部分允许您从各自的下拉菜单中选择用于身份验证、授权和注销流的MVPD端点。

身份验证、授权和注销流的![端点](../assets/tve-dashboard/new-tve-dashboard/integrations/integration-endpoint-selection-panel-view.png)

身份验证、授权和注销流的&#x200B;*端点*

>[!NOTE]
>
>MVPD可以为每个流提供一个或多个端点。 在集成新通道时，MVPD必须为每个流指定其首选端点。

>[!IMPORTANT]
>
>对端点进行的任何更改都将影响集成的整体行为。 只有在收到MVPD的确认后，才应实施这些更改。

### 平台设置 {#platform-settings}

此部分允许您查看和编辑所有[平台](/help/authentication/user-guide-tve-dashboard/tve-dashboard-reports.md#platforms)上的集成设置。 您可以根据各个平台更改这些设置。 例如，您可以调整Android上的授权TTL持续时间，同时保留另一个平台的默认值。

平台设置中的每个属性都继承了MVPD设置的默认值，但如有必要，可以调整该值。

>[!IMPORTANT]
>
>要确定为平台设置中的每个属性设置的值，需要与MVPD达成协议。

>[!IMPORTANT]
>
> 设置继承遵循从MVPD设置（最常用）、MVPD端点、集成、平台类别和platform（包含最具体的值）开始的链。

**平台设置**&#x200B;用于覆盖继承链中每个级别的设置。 链中的可用级别分组如下：

* **Default for All**：如果未定义特定平台值，则为适用于所有平台的属性设置值，而不管程序员的实施如何。

* **台式机设备**：设置适用于所有台式机和笔记本电脑的属性值，不考虑编程方法（JS SDK或REST API）。

* **移动设备**：设置适用于所有移动设备的属性值，包括&#x200B;**iOS**、**Android**&#x200B;和其他设备，而不管编程方法如何（SDK或REST API）。

* **TV连接设备**：设置适用于所有电视连接设备的属性值，包括&#x200B;**tvOS**、**Roku**、**FireTV**&#x200B;和其他设备，不考虑编程方法（SDK或REST API）。

* **未识别的设备**：设置适用于当前机制无法准确识别平台的所有设备的属性值。 在这种情况下，应用MVPD定义的最严格的规则。

  ![平台及其设备的类别](../assets/tve-dashboard/new-tve-dashboard/integrations/integration-platform-settings-menu.png)

  *平台及其设备的类别*

选择 位于每个属性右侧的<img alt= "继承链图标" src="../assets/tve-dashboard/new-tve-dashboard/integrations/integration-platform-settings-inheritance-chain-icon.svg" width="25">图标可浏览用于上述每个继承级别的属性。

#### 最常用的业务流程 {#most-used-flows}

**平台设置**&#x200B;部分提供用于不同业务流程的一系列属性。 实际属性可能因特定集成中选择的MVPD而异。 以下是最常用的流：

跨所有平台的&#x200B;**AuthN TTL和AuthZ TTL**

>[!IMPORTANT]
>
>身份验证(AuthN) TTL和授权(AuthZ) TTL值必须与MVPD设置一致。

按照以下步骤更改特定集成在所有平台上的身份验证和授权TTL。

1. 在左侧面板中选择&#x200B;**集成**&#x200B;选项卡。

1. 选择要为其更改身份验证N TTL和身份验证Z TTL值的集成。

1. 导航到&#x200B;**平台设置**&#x200B;部分。

1. 在&#x200B;**平台设置**&#x200B;下选择&#x200B;**所有**&#x200B;选项卡的默认值。

   >[!NOTE]
   >
   >如果要更改平台类别或特定平台的&#x200B;**AuthN TTL**&#x200B;和&#x200B;**AuthZ TTL**&#x200B;的持续时间，请相应地选择平台。

   ![更改所有平台上的AuthN TTL AuthZ TTL持续时间](../assets/tve-dashboard/new-tve-dashboard/integrations/integration-platform-settings-authn-ttl-authz-ttl-properties.png)

   *更改所有平台上的AuthN TTL AuthZ TTL持续时间*

   **A.** AuthN TTL属性&#x200B;**B.** AuthZ TTL属性

1. 选择向上和向下箭头可调整&#x200B;**AuthN TTL**&#x200B;和&#x200B;**AuthZ TTL**&#x200B;属性中天数、小时数、分钟数和秒数的持续时间。

只有在[查看和推送更改](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md)之后，才会更新所有平台上的&#x200B;**AuthN TTL**&#x200B;和&#x200B;**AuthZ TTL**&#x200B;的持续时间。

**启用平台SSO**

>[!IMPORTANT]
>
>**在&#x200B;*iOS、tvOS、Roku和FireTV*平台上专门支持启用单点登录**&#x200B;属性。 它仅适用于与支持这些平台单点登录的MVPD的集成。

按照以下步骤为特定集成和平台启用或禁用SSO。

1. 在左侧面板中选择&#x200B;**集成**&#x200B;选项卡。

1. 选择要为其启用或禁用单点登录的集成。

1. 导航到&#x200B;**平台设置**&#x200B;部分。

1. 选择要在&#x200B;**平台设置**&#x200B;下启用单点登录的特定平台或平台类别。

   ![为特定平台启用单点登录](../assets/tve-dashboard/new-tve-dashboard/integrations/integration-platform-settings-single-sign-on-properties.png)

   *为特定平台启用单点登录*

   **A.**&#x200B;单点登录属性&#x200B;**B.**&#x200B;强制平台权限属性

1. 从&#x200B;**启用单点登录**&#x200B;下拉菜单中选择&#x200B;**是**&#x200B;启用或&#x200B;**否**&#x200B;禁用。

1. 从&#x200B;**强制平台权限**&#x200B;下拉菜单中选择&#x200B;**是**&#x200B;启用或&#x200B;**否**&#x200B;禁用。

   **强制平台权限**&#x200B;属性控制是否遵循用户对其电视提供商订阅&#x200B;**允许**&#x200B;或&#x200B;**拒绝**&#x200B;平台访问的决定。

   例如，如果同时启用了&#x200B;**启用单点登录**&#x200B;和&#x200B;**强制平台权限**，并且用户选择拒绝平台访问其电视提供商订阅，则相应的应用程序（频道）将无法使用由其他应用程序（频道）获得的Adobe Pass身份验证令牌。

仅在[查看和推送更改](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md)之后，才能启用或禁用所选平台的&#x200B;**单点登录**&#x200B;属性。

**启用基于主目录的身份验证**

按照以下步骤为基于OAuth2的MVPD启用或禁用基于主目录的身份验证。

1. 在左侧面板中选择&#x200B;**集成**&#x200B;选项卡。

1. 选择要为其启用或禁用基于主目录的身份验证的集成。

1. 导航到&#x200B;**平台设置**&#x200B;部分。

1. 选择要在&#x200B;**平台设置**&#x200B;下启用基于主页的身份验证的特定平台或平台类别。

   ![为特定平台启用基于主目录的身份验证](../assets/tve-dashboard/new-tve-dashboard/integrations/integration-platform-settings-attempt-hba-properties.png)

   *为特定平台启用基于主目录的身份验证*

   **A.**&#x200B;尝试HBA属性&#x200B;**B.** HBA AuthN TTL属性

1. 从&#x200B;**尝试HBA**&#x200B;下拉菜单中选择&#x200B;**是**&#x200B;启用和&#x200B;**否**&#x200B;禁用。

>[!IMPORTANT]
>
>应避免更改&#x200B;**HBA AuthN TTL**&#x200B;属性的持续时间。 它可能会导致授权过程中出现意外故障。

只有在[查看和推送更改](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md)之后，才能启用或禁用特定MVPD的&#x200B;**尝试HBA**&#x200B;属性。

#### 添加更多属性 {#add-more-properties}

**添加更多属性**&#x200B;允许灵活地为集成包含其他特定属性，尤其是不太常见的流。

您可以添加以下属性：

* 对于所有平台，选择左侧所有&#x200B;**选项卡的**&#x200B;默认值。
* 对于平台类别，请选择左侧的&#x200B;**桌面设备**、**移动设备**&#x200B;或&#x200B;**TV连接设备**&#x200B;选项卡。
* 对于特定设备，请选择左侧的&#x200B;**iOS**、**Android**、**tvOS**、**Roku**&#x200B;或&#x200B;**FireTV**&#x200B;选项卡。

以下是可以通过添加这些属性启用的不同流的一些示例：

**更改预授权资源的数量**

默认情况下，大多数MVPD支持最多使用5个资源ID的预检authZ调用。
但是，如果MVPD同意提高此限制，则可以导航到**添加更多属性**，然后从选项菜单中选择&#x200B;**预检最大资源**。

**预检最大资源**&#x200B;将添加新的属性，可以指定与MVPD商定的限制。

![添加预检最大资源属性](../assets/tve-dashboard/new-tve-dashboard/integrations/integration-platform-settings-preflight-max-resources-properties.png)

*添加预检最大资源属性*

**预检最大资源**&#x200B;属性仅在[审核和推送更改](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md)之后添加。

**更改MVPD显示名称或徽标URL**

对于不想构建其MVPD选取器而是依赖所提供的配置的程序员应用程序，您可以导航到&#x200B;**添加更多属性**&#x200B;并选择&#x200B;**显示名称**&#x200B;或&#x200B;**徽标URL**&#x200B;以从选项菜单为每个MVPD添加所需的显示名称或徽标URL。

这些属性的不同值可用于相同的MVPD，具体取决于设备平台和所需的用户体验。

![添加显示名称或徽标URL属性](../assets/tve-dashboard/new-tve-dashboard/integrations/integration-platform-settings-display-name-logo-url-properties.png)

*添加显示名称或徽标URL属性*

**显示名称**&#x200B;或&#x200B;**徽标URL**&#x200B;属性仅在[审阅和推送更改](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md)之后添加。

**在应用程序（渠道）切换时请求新的身份验证流程**

如果要在用户在不同应用程序之间切换时强制进行新身份验证。 在这种情况下，您可以导航到&#x200B;**添加更多属性**，选择&#x200B;**每个聚合器的身份验证**&#x200B;属性。

为每个聚合器&#x200B;**添加**&#x200B;身份验证会有效地中断相应渠道的单点登录。

![为每个聚合器属性添加身份验证](../assets/tve-dashboard/new-tve-dashboard/integrations/integration-platform-settings-auth-per-aggregator-properties.png)

*为每个聚合器属性添加身份验证*

仅在[审阅和推送更改](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md)之后才会添加每个聚合器&#x200B;**的身份验证**&#x200B;属性。

添加后，选择&#x200B;**是**&#x200B;为选定的集成启用每个聚合器的&#x200B;**身份验证**&#x200B;属性。

#### 删除属性 {#delete-properties}

选择 每个属性右侧的<img alt= "“删除属性”按钮" src="../assets/tve-dashboard/new-tve-dashboard/integrations/integration-platform-settings-delete-property-icon.svg" width="25">图标用于删除不再需要的属性。

>[!NOTE]
>
>无法删除某些属性，因为它们是所选MVPD的必需要求。

只有在[审阅和推送更改](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md)后，属性才会从&#x200B;**平台设置**&#x200B;部分中删除。

### 用户元数据 {#user-metadata}

此部分允许您更新MVPD共享的每个用户元数据参数的设置。

>[!NOTE]
>
>每个MVPD可以共享不同的参数。 有关特定MVPD可以共享的参数的更多信息，请联系您的Adobe代表。

用户元数据部分显示以下列：

**密钥**：表示要在API中用于提取值的实际用户元数据参数。

**描述**：提供每个用户元数据参数的简短描述。

**Encrypted**：此列允许您通过分别从下拉菜单中选择&#x200B;**是**&#x200B;或&#x200B;**否**&#x200B;来启用或禁用API中的参数。 选择&#x200B;**是**&#x200B;表示将在API中加密参数值。 使用由&#x200B;**用户元数据**&#x200B;作用域定义的证书执行加密。

>[!TIP]
>
>
> 始终确保&#x200B;**ZIP**&#x200B;参数已加密。

了解有关[程序员](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#available-certificates)和[渠道](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#available-certificates)部分中的可用证书的详细信息。

**已启用**：此列允许您通过从下拉菜单中分别选择&#x200B;**是**&#x200B;或&#x200B;**否**&#x200B;来启用或禁用API中的参数。

![可用于用户元数据的参数](../assets/tve-dashboard/new-tve-dashboard/integrations/integration-user-metadata-panel-view.png)

*可用于用户元数据的参数*

## 创建新集成 {#create-new-integration}

要在当前设置中创建与新MVPD的新集成，请执行以下步骤：

1. 在左侧面板中选择&#x200B;**集成**&#x200B;选项卡。

1. 选择&#x200B;**集成**&#x200B;部分的右上角的&#x200B;**新建集成**。

   ![创建新集成](../assets/tve-dashboard/new-tve-dashboard/integrations/integration-create-new-integration-button.png)

   *创建新集成*

   将显示以下部分：

   **选择通道和MVPD**

   从&#x200B;**选择渠道**&#x200B;下拉菜单中选择一个&#x200B;**渠道**&#x200B;以添加新集成。 选择渠道后，从&#x200B;**选择MVPD**&#x200B;下拉菜单中选择要与所选渠道集成所需的&#x200B;**MVPD**。

   ![选择通道和MVPD](../assets/tve-dashboard/new-tve-dashboard/integrations/integration-new-integration-select-channel-and-mvpd-panel-view.png)

   *选择通道和MVPD*

   **选择端点**

   选择所需的MVPD后，**选择终结点**&#x200B;部分将预填充为该特定MVPD配置的默认终结点。

   >[!IMPORTANT]
   >
   >不更改任何流中的默认端点，除非MVPD特别说明。

   ![选择终结点](../assets/tve-dashboard/new-tve-dashboard/integrations/integration-new-integration-select-endpoints-panel-view.png)

   *选择端点*

   **其他信息**

   此部分包括需要在&#x200B;**选择通道和MVPD**&#x200B;部分中为所选MVPD配置的各种属性。

   >[!NOTE]
   >
   > 根据&#x200B;**选择通道和MVPD**&#x200B;部分中选择的MVPD，实际属性可能会有所不同。

   例如，您可以在下图的MVPD登录页面上编辑&#x200B;**AuthN TTL**&#x200B;或&#x200B;**合作伙伴ID** （渠道ID）以用于品牌联合。

   ![编辑其他信息](../assets/tve-dashboard/new-tve-dashboard/integrations/integration-new-integration-additional-information-panel-view.png)

   *编辑其他信息*

   选择&#x200B;**新建集成**&#x200B;部分的右上角的&#x200B;**保存集成**。

只有在[审核并推送更改](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md)之后才会创建新集成。


## 禁用集成 {#disable-integration}

要禁用集成，请执行以下步骤：

1. 在左侧面板中选择&#x200B;**集成**&#x200B;选项卡。

1. 选择要禁用的集成。

1. 禁用所选集成右上角的切换。

   ![禁用集成](../assets/tve-dashboard/new-tve-dashboard/integrations/integration-enabled-disabled-button.png)

   *禁用集成*

只有在[审阅并推送更改](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md)后，才会禁用该集成。

禁用集成后，最终用户将无法使用特定的MVPD进行身份验证或授权。
