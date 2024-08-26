---
title: Amazon FireOS应用程序注册
description: Amazon FireOS应用程序注册
exl-id: 650fd4a2-dfc3-4c74-9b5b-6bea832a28ca
source-git-commit: 3cff9d143eedb35155aa06c72d53b951b2d08d39
workflow-type: tm+mt
source-wordcount: '515'
ht-degree: 0%

---

# Amazon FireOS应用程序注册 {#amazon-fireos-application-registration}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

</br>

## 简介 {#intro}

从FireOS AccessEnabler SDK版本3.0开始，我们正在更改Adobe服务器的身份验证机制。 我们引入了“软件语句”字符串的概念，而不是使用公钥和密码系统对requestorID进行签名，该字符串可用于获取访问令牌，该令牌稍后将用于SDK对我们的服务器进行的所有调用。 除了软件声明之外，您还需要为应用程序创建深层链接。

有关详细信息，请参阅[动态客户端注册概述](./dcr-api/dynamic-client-registration-overview.md)。

## 什么是软件声明？ {#what}

软件语句是一个JWT令牌，其中包含有关应用程序的信息。 每个应用程序都应该有一个独一无二的软件声明，供我们的服务器用来识别Adobe系统中的应用程序。 初始化AccessEnabler SDK时需要传递软件语句，并且软件语句将用于向Adobe注册应用程序。 注册后，SDK将接收客户端ID和用于获取访问令牌的客户端密码。 SDK对我们的服务器进行的任何调用都需要有效的访问令牌。 SDK负责注册应用程序、获取和刷新访问令牌。

**注意：**&#x200B;软件语句特定于应用程序，单个软件语句不能用于多个应用程序。 请注意，这同样适用于提供对多个渠道访问权限的应用程序。

## 如何获取软件声明？ {#how-to}

### 如果您有权访问Adobe的TVE仪表板：

1. 打开浏览器并导航到`https://console.auth.adobe.com`。

1. 导航到&#x200B;**[!UICONTROL Channels]**&#x200B;部分，然后选择您的渠道。

1. 导航到&#x200B;**[!UICONTROL Registered Applications]**&#x200B;选项卡。

1. 单击&#x200B;**[!UICONTROL Add new application]**。

1. 提供应用程序的名称和版本，并选择可在其中使用该应用程序的平台(例如Android)。

1. 通过从已为程序员配置的域列表中进行选择，提供&#x200B;**[!UICONTROL Domain Name]**。

1. 将更改推送到服务器，然后导航回渠道的&#x200B;**[!UICONTROL Registered Applications]**&#x200B;选项卡。

   您应该会看到一个包含所有已注册应用程序的列表。

1. 在刚刚创建的应用程序上单击&#x200B;**[!UICONTROL Download]**。

   您可能需要等待几分钟，软件语句才可供下载。

   下载文本文件。 将其内容用作软件声明。

有关详细信息，请参阅[动态客户端注册管理](./dcr-api/dynamic-client-registration-overview.md#dynamic-client-registration-management)。

### 如果您无权访问Adobe的TVE功能板：

向[tve-support@adobe.com](mailto:tve-support@adobe.com)提交票证。 包括所有必需的信息（包括渠道、应用程序名称、版本和平台），我们的支持团队将为您创建一份软件声明。

## 如何使用软件声明 {#use}

获取软件语句后，需要将其作为参数在Access Enabler构造函数中传递。 Adobe建议将软件语句托管在远程位置。 这样，您就可以轻松地撤销和更改软件语句，而无需发布应用程序的新版本。

## 如何使用软件声明 {#use-both}

在应用程序的资源文件`strings.xml`中添加以下代码：

```XML
<string name="software_statement">softwarestatement value</string>
```
