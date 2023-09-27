---
title: 动态客户端注册管理
description: 动态客户端注册管理
exl-id: 2c3ebb0b-c814-4b9e-af57-ce1403651e9e
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1338'
ht-degree: 0%

---

# 动态客户端注册管理 {#dynamic-client-registration-management}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## 概述 {#overview}

随着广泛采用 [Android Chrome自定义选项卡](https://developer.chrome.com/multidevice/android/customtabs){target_blanck} 和 [Apple Safari视图控制器](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller){target_blanck} 在客户的应用程序中，我们正在更新Adobe Pass身份验证中的用户身份验证流程。 更具体地说，我们无法再达到保持状态以便验证MVPD订户的用户代理流可以在重定向之间被跟踪。 此操作以前是使用HTTP Cookie完成的。 此限制是开始将所有API迁移到OAuth 2.0的驱动程序 [RFC6749](https://tools.ietf.org/html/rfc6749){target_blanck}.

通过此更新，Adobe身份验证客户端成为OAuth 2.0客户端，并部署自定义OAuth 2.0授权服务器以满足Adobe Pass身份验证服务的需求。

为了使客户端应用程序能够利用OAuth 2.0授权，服务器必须动态注册以获取特定信息（客户端凭据）以便能够与其交互。 在注册过程中，客户端必须向客户端注册端点提供一组内置元数据。

此元数据作为软件语句传送，其中包含“software_id”，以允许我们的授权服务器使用相同的软件语句关联应用程序的不同实例。

A **软件声明** 是一个JSON Web令牌(JWT)，它将客户端软件的元数据值作为捆绑包进行声明。 作为客户端注册请求的一部分向授权服务器呈现时，必须使用JSON Web签名(JWS)对软件语句进行数字签名或MAC编辑。

您可以在官方文档中找到有关什么是软件语句及其工作方式的更详细说明 [RFC7591](https://tools.ietf.org/html/rfc7591).

软件语句应随应用程序一起部署在用户设备上。

在此更新之前，我们有两个机制允许应用程序执行对Adobe Pass身份验证的调用：

* 基于浏览器的客户端通过允许进行注册 [域列表](/help/authentication/programmer-overview.md#reg-and-init)
* 本机应用程序客户端(如iOS和Android应用程序)的注册方式为 **已签名的请求者** 机理


使用客户端注册授权机制，您必须将应用程序添加到TVE仪表板。

对于要开始实施新的Android SDK和即将发布的iOS SDK的客户，他们需要一份软件声明。 软件语句标识在TVE Dashboard中创建的应用程序。

按照以下部分中的步骤进行操作，在TVE功能板中创建已注册的应用程序。

## 创建注册的应用程序 {#create_app}

可通过两种方式在TVE功能板中创建已注册的应用程序：

* [程序员级别](#prog-level)  — 允许您创建已注册的应用程序，并将其链接到任何或所有程序员渠道。

* [渠道级别](#channel-level)  — 允许您创建仅永久链接到此渠道的已注册应用程序。

### 在程序员级别创建注册的应用程序 {#prog-level}

转到 **程序员** > **已注册的应用程序** 选项卡。

![](assets/reg-app-progr-level.png)

在“已注册的应用程序”选项卡中，单击 **添加新应用程序**. 在新窗口中填写必填字段。

如下图所示，您应填写的字段包括：

* **应用程序名称**  — 应用程序的名称

* **已分配给渠道**  — 渠道的名称，t</span>此应用程序链接到的目标。 下拉蒙版中的默认设置为 **所有渠道。** 该界面允许您选择一个通道或选择所有通道。

* **应用程序版本**  — 默认情况下，该参数设置为“1.0.0”，但我们强烈建议您使用自己的应用程序版本对其进行修改。 作为最佳实践，如果您决定更改应用程序的版本，请通过为其创建新的已注册应用程序来反映该版本。

* **应用程序平台**  — 要链接应用程序的平台。 您可以选择选择所有值或多个值。

* **域名**  — 要链接到的应用程序的域。 下拉列表中的域是从所有渠道中统一选择的所有域。 您可以选择从列表中选择多个域。 域的含义是重定向URL [RFC6749](https://tools.ietf.org/html/rfc6749). 在客户端注册过程中，客户端应用程序可请求允许使用重定向URL来最终确定身份验证流。 当客户端应用程序请求特定的重定向URL时，将针对与软件语句关联的此注册应用程序中列出的白名单域验证该URL。


![](assets/new-reg-app.png)


在用适当的值填写字段后，您必须单击“完成”才能将应用程序保存在配置中。

请注意 **没有用于修改已创建应用程序的选项**. 如果发现创建的某个内容不再满足要求，则需要创建一个新的注册应用程序，并将其与满足其要求的客户端应用程序一起使用。


### 在渠道级别注册新应用程序 {#channel-level}

要在渠道级别创建注册的应用程序，请导航到“渠道”菜单，然后选择要为其创建应用程序的渠道。 然后，在导航到“注册的应用程序”选项卡后，单击“添加新应用程序”按钮。

![](assets/reg-new-app-channel-level.png)

如下所示，与在程序员级别执行的相同操作相比，此处的略有不同之处在于“已分配渠道”下拉列表，该下拉列表未启用，因此没有选项将注册的应用程序绑定到当前渠道以外的其他渠道。

![](assets/new-reg-app-channel.png)

## 列出应用程序 {#list-reg-app}

创建已注册的应用程序后，可以获取软件语句，以将授权服务器作为请求的一部分呈现。

这可以通过导航到已注册应用程序所创建的程序员或渠道来完成，这些应用程序将列在报表中。

如下图所示，列表中的每个条目都将由其绑定的平台的名称、版本和符号进行标识。

![](assets/reg-app-list.png)

对于每个插件，您可以：

* [视图](#view)
* [下载软件声明](#download-statement)

### 查看已注册的应用程序 {#view}

在应用程序列表中，选择其中一个应用程序并单击“查看”按钮将显示创建应用程序时使用的详细信息。 如前所述，没有用于修改任何内容的选项。


![](assets/view-reg-app.png)


### 下载软件声明 {#download-statement}

单击需要软件语句的列表项上的“下载”按钮将生成文本文件。 此文件将包含类似于以下示例输出的内容。


![](assets/download-software-statement.png)

文件的名称通过以“software_statement”为前缀并添加当前时间戳进行唯一标识。

请注意，对于同一已注册应用程序，每次单击下载按钮时都会收到不同的软件语句，但这不会使先前为此应用程序获得的软件语句失效。 之所以发生这种情况，是因为它们是根据操作请求即时生成的。

有一个 **限制** 有关下载操作。 如果在创建注册的应用程序后不久通过单击“下载”按钮请求软件语句，但尚未保存该语句并且配置json未同步，则页面底部将显示以下错误消息。

![](assets/error-sw-statement-notready.png)

这会包装从核心收到的HTTP 404 Not Found错误代码，因为已注册应用程序的ID尚未传播，并且核心不知道它。

解决方案是在创建注册的应用程序后，最多等待2分钟以使配置同步。 发生这种情况后，将不再收到错误消息，并且包含软件语句的文本文件将可供下载。

有关端到端流程如何工作的详细信息，或者为了深入了解请求的执行方式以及预期的响应，请参阅以下相关信息中的链接，以及其他有用链接。

<!--
## Related Information {#related}

* [Dynamic Client Registration API](/help/authentication/dynamic-client-registration-api.md)
* [TVE Dashboard User Guide](/help/authentication/tve-dashboard-user-guide.md)
-->

## 功能演示 {#tutorial}

请观看 [此网络研讨会](https://my.adobeconnect.com/pzkp8ujrigg1/) 其中提供了这些功能的更多上下文，并包含有关如何使用TVE仪表板管理软件语句以及如何使用Adobe作为Android SDK的一部分提供的演示应用程序测试生成的语句的演示。
