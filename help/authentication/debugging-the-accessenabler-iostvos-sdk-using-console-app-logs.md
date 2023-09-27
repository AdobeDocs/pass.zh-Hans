---
title: 使用控制台应用程序日志调试AccessEnabler iOS/tvOS SDK
description: 使用控制台应用程序日志调试AccessEnabler iOS/tvOS SDK
exl-id: 0dad325e-db15-4ea0-a87a-75409eaf8d46
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '551'
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

* AccessEnabler框架使用Apple推荐的 [OSLog](https://developer.apple.com/documentation/os/oslog) 实现。

* AccessEnabler框架引入了根据子系统筛选Console应用程序日志的功能： **com.adobe.pass.AccessEnabler**. SDK发出的所有消息均包含在com.adobe.pass.AccessEnabler中。

* AccessEnabler框架引入了根据任意（前缀）筛选控制台应用程序日志的功能： **[AccessEnabler]**. SDK发出的所有消息都以为前缀 [AccessEnabler].

* AccessEnabler框架引入了根据类别筛选Console应用程序日志的功能： **调试**， **错误** 以及以上两个标准中的任何一个：子系统或任何（前缀）。

## 使用Console应用程序日志调试

根据调查的问题，您可能希望包含或排除AccessEnabler框架发出的日志记录消息，因此，您可以在下文中找到一些有用的详细信息，这些详细信息可能在调查期间和使用控制台应用程序日志时为您提供帮助。


### AccessEnabler iOS/tvOS 3.5.0及更高版本

#### 包括 {#including}

首先，为了能够查看AccessEnabler框架发出的任何日志记录消息，您 **必须** 在Console应用程序的“操作”部分中选择“包括信息消息”和“包括调试消息”，如下图所示。

![](assets/include-info-debug-msg.png)


为了能够调试AccessEnabler iOS/tvOS SDK和 **请参阅** AccessEnabler框架日志可以：

* 在Console应用程序中使用搜索 **子系统** 选项等于com.adobe.pass.AccessEnabler值，如下图所示。

![](assets/subsys-console-app.png)

* 在Console应用程序中使用搜索 **任何** 选项包含
  [AccessEnabler] 值，如下图所示。

![](assets/any-optn-console-app.png)

除了上述两个标准之外，您还可以使用 **类别** 结合使用选项 **子系统** 或 **任意（前缀）** 以明确搜索 **调试** 或 **错误** 级别AccessEnabler iOS/tvOS SDK发出的消息。

#### 不包括

为了能够更好地调试其他组件和 **排除** AccessEnabler框架日志可以：

* 在Console应用程序中使用搜索 **子系统** 不等于com.adobe.pass.AccessEnabler值的选项。
* 在Console应用程序中使用搜索 **任何** 选项，但不包含 [AccessEnabler] 值。

## 报告问题

向Adobe Pass身份验证报告问题时，请考虑以下建议：

* 请尝试提供复制步骤。
* 请尝试提供出现问题的操作系统版本和设备型号。
* 请尝试提供遇到问题的AccessEnabler iOS/tvOS SDK的版本。
* 请尝试使用以下两个选项之一捕获并附加所有AccessEnabler iOS/tvOS SDK日志消息： [包括](#including) 部分。
