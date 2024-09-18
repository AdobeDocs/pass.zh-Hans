---
title: TVE仪表板环境
description: 了解TVE仪表板中不同环境的使用和工作。
exl-id: 591becb8-2f6c-46e0-b108-c64e6df69f89
source-git-commit: acff285f7db1bdd32d5da3e01a770d9581d3ba75
workflow-type: tm+mt
source-wordcount: '346'
ht-degree: 0%

---

# 环境 {#environments}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

TVE功能板提供了各种自定义环境，以便在Adobe Pass身份验证中实现特定目的。 有两个主要环境：

* **先决条件**：资格预审环境用作测试场，用于在部署到生产环境之前准备和测试新内部版本。

* **版本**：版本环境承载已完成的和已测试的内部版本以用于生产。

在每个环境中，有两个不同的配置文件：

* **暂存**：暂存配置文件连接到MVPD的暂存服务器，以便在集成上线之前进行测试和验证。

* **生产**：生产配置文件连接到MVPD的实际生产活动的生产配置文件。

## 用例

TVE Dashboard中的环境可在整个应用程序生命周期中提供各种用例。 通过这些环境，您可以：

### 前期暂存

* 使用MVPD的暂存端点验证Adobe Pass身份验证服务器的新未发布功能。
* 主要由Adobe Pass身份验证产品团队用于添加和验证新的MVPD集成。

### 前期生产

* 使用MVPD的生产端点验证Adobe Pass身份验证服务器的新未发布功能或配置。
* 使用MVPD的生产端点验证每个渠道的新应用程序版本。
* 在将每个配置更改推送到生产环境之前对其进行验证。

### 发布暂存

* 使用MVPD的暂存端点验证每个渠道的新应用程序版本。
* 在此环境中执行性能或容量测试。

### 发布生产

* 表示向所有最终用户公开的最新Adobe Pass版本号的实时环境。
* 保持代码和配置的稳定性，并立即反映最终用户应用程序中的配置更改。

## 切换环境 {#switch-environments}

按照以下步骤在Adobe Pass身份验证TVE功能板环境之间进行切换。

1. 使用程序员凭据登录。

1. 从左侧面板顶部的&#x200B;**环境**&#x200B;下拉菜单中选择所需的暂存或生产环境。

   ![TVE仪表板环境下拉列表](../../assets/tve-dashboard/new-tve-dashboard/dashboard/dashboard-environment-menu.png)

   *Adobe Pass身份验证TVE仪表板环境下拉菜单*

>[!NOTE]
>
> 根据您的设置，每个环境中的配置可能有所不同。
