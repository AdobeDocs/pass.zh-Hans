---
title: 使用控制台应用程序日志调试AccessEnabler iOS/tvOS SDK
description: 使用控制台应用程序日志调试AccessEnabler iOS/tvOS SDK
exl-id: 0dad325e-db15-4ea0-a87a-75409eaf8d46
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '553'
ht-degree: 0%

---

# 使用控制台应用程序日志调试AccessEnabler iOS/tvOS SDK {#debugging-the-accessenabler-iostvos-sdk-using-console-app-logs}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。


## 概述

本文档旨在捕获和呈现AccessEnabler的iOS/tvOS SDK日志记录机制的演变，以及一些用于使用控制台应用程序日志调试AccessEnabler框架的有用详细信息。

## 日志记录机制状态

AccessEnabler iOS/tvOS日志记录机制的目的是发出有用的消息，用于排除使用AccessEnabler框架的应用程序可能因此而遇到的问题。

### AccessEnabler iOS/tvOS 3.5.0及更高版本

从AccessEnabler iOS/tvOS 3.5.0版本开始，日志记录机制引入以下改进作为更改：

* AccessEnabler框架使用Apple推荐的[OSLog](https://developer.apple.com/documentation/os/oslog)实现。

* AccessEnabler框架引入了根据子系统&#x200B;**com.adobe.pass.AccessEnabler**&#x200B;筛选控制台应用程序日志的功能。 SDK发出的所有消息均包含在com.adobe.pass.AccessEnabler中。

* AccessEnabler框架引入了根据任意（前缀）筛选控制台应用程序日志的功能： **[AccessEnabler]**。 SDK发出的所有消息都带有[AccessEnabler]前缀。

* AccessEnabler框架引入了根据类别&#x200B;**debug**、**error**&#x200B;以及以上两个条件中的任意一个筛选控制台应用程序日志的功能：子系统或任何（前缀）。

## 使用Console应用程序日志调试

根据调查的问题，您可能希望包含或排除AccessEnabler框架发出的日志记录消息，因此，您可以在下文中找到一些有用的详细信息，这些详细信息可能在调查期间和使用控制台应用程序日志时为您提供帮助。


### AccessEnabler iOS/tvOS 3.5.0及更高版本

#### 包括 {#including}

首先，为了能够查看AccessEnabler框架发出的任何日志记录消息，您&#x200B;**必须**&#x200B;在控制台应用程序的“操作”部分中选择“包括信息消息”和“包括调试消息”，如下图所示。

![](../assets/include-info-debug-msg.png)


为了能够调试AccessEnabler iOS/tvOS SDK和&#x200B;**查看** AccessEnabler框架日志，您可以：

* 在控制台应用程序中使用&#x200B;**子系统**&#x200B;选项进行搜索，该选项等于com.adobe.pass.AccessEnabler值，如下图所示。

![](../assets/subsys-console-app.png)

* 使用包含&#x200B;****
  [AccessEnabler]值，如下图所示。

![](../assets/any-optn-console-app.png)

除了上述两个标准之外，您还可以结合使用&#x200B;**Category**&#x200B;选项和&#x200B;**子系统**&#x200B;或&#x200B;**任何（前缀）**&#x200B;来显式搜索由AccessEnabler iOS/tvOS SDK发出的&#x200B;**调试**&#x200B;或&#x200B;**错误**&#x200B;级消息。

#### 不包括

为了能够更好地调试其他组件的功能和&#x200B;**排除** AccessEnabler框架日志，您可以：

* 在Console应用程序中使用&#x200B;**Subsystem**&#x200B;选项进行搜索，该选项不等于com.adobe.pass.AccessEnabler值。
* 使用不包含[AccessEnabler]值的&#x200B;**Any**&#x200B;选项在控制台应用程序中搜索。

## 报告问题

向Adobe Pass身份验证报告问题时，请考虑以下建议：

* 请尝试提供复制步骤。
* 请尝试提供出现问题的操作系统版本和设备型号。
* 请尝试提供遇到问题的AccessEnabler iOS/tvOS SDK的版本。
* 请尝试使用[包括](#including)部分中存在的两个选项之一来捕获并附加所有AccessEnabler iOS/tvOS SDK日志记录消息。
