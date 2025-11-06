---
title: Android应用程序注册
description: Android应用程序注册
exl-id: 6238bd87-ac97-4a5c-9d92-3631f7b2d46a
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '609'
ht-degree: 0%

---

# （旧版）Android应用程序注册 {#android-application-registration}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

>[!IMPORTANT]
>
> 确保随时了解汇总在[产品公告](/help/authentication/product-announcements.md)页中的最新Adobe Pass身份验证产品公告和停用时间表。

## 简介 {#intro}

从3.0版本的Android AccessEnabler SDK开始，我们正在更改Adobe服务器的身份验证机制。 我们引入了软件语句字符串的概念，这种字符串可用于获取访问令牌，该令牌稍后将用于SDK对我们的服务器进行的所有调用，而不是使用公钥和密码系统来对requestorID进行签名。 除了软件声明之外，您还需要为应用程序创建深层链接。

有关详细信息，请参阅[动态客户端注册概述](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)。

## 什么是软件声明？ {#what}

软件语句是一个JWT令牌，其中包含有关应用程序的信息。 每个应用程序都应该有一个独特的软件声明，供我们的服务器用来识别Adobe系统中的应用程序。

初始化`AccessEnabler` SDK时需要传递软件语句。 它用于向Adobe注册应用程序。 注册后，SDK接收客户端ID和客户端密钥，客户端密钥用于获取访问令牌。 SDK对Adobe服务器进行的任何调用都需要有效的访问令牌。 SDK负责注册应用程序、获取和刷新访问令牌。

>[!NOTE]
>
>软件语句特定于应用程序，单个软件语句不能用于多个应用程序。 请注意，程序员级别的软件语句具有相同的限制，它们只能用于单个应用程序，无论是单通道还是多通道。

## 如何获取软件声明 {#how-to-get-ss}

以下是获取软件声明的方法。

### 如果您有权访问Adobe的TVE功能板

1. 打开浏览器并导航到[Adobe Pass TVE仪表板](https://experience.adobe.com/#/pass/authentication)。

1. 导航到&#x200B;**[!UICONTROL Channels]**&#x200B;部分，然后选择您的渠道。

1. 导航到&#x200B;**[!UICONTROL Registered Applications]**&#x200B;选项卡。

1. 单击&#x200B;**[!UICONTROL Add new application]**。

1. 命名应用程序并指定版本。

1. 选择应用程序可用的平台(在本例中为Android)。

1. 通过从已为程序员配置的域列表中进行选择，提供&#x200B;**[!UICONTROL Domain Name]**。

1. 将更改推送到服务器，然后导航回渠道的&#x200B;**[!UICONTROL Registered Applications]**&#x200B;选项卡。

   您应该会看到一个包含所有已注册应用程序的列表。 在您创建的应用程序上选择&#x200B;**[!UICONTROL Download]**。 您可能需要等待几分钟，软件声明才可供下载。

   下载文本文件。 将其内容用作软件声明。

有关详细信息，请参阅[动态客户端注册管理](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md#dynamic-client-registration-management)。

### 如果您无权访问Adobe的TVE功能板

向`tve-support@adobe.com`提交票证。 包括渠道、应用程序名称、版本和平台等必需信息。 我们的支持团队中的某个人将为您创建一份软件声明。

## 如何使用软件声明 {#how-to-use-ss}

获取软件语句后，需要将其作为参数在Access Enabler构造函数中传递。 我们建议将软件声明托管在远程位置。 这样，您就可以轻松地撤销和更改软件语句，而无需发布应用程序的新版本。

## 为您的应用程序创建并使用深层链接 {#create}

在Android上，使用与您创建软件语句时选择的域名相反的深层链接值

已创建的深层链接在Android设备上应具有唯一值。 当多个应用程序使用相同的深层链接值时，身份验证和注销流将干预。

## 如何使用软件声明和深层链接 {#use-both}

在应用程序的资源文件`strings.xml`中添加以下代码：

```JAVA
    <string name="software_statement">softwarestatement value</string>
    <string name="redirect_uri">com.domain_name</string>
```
