---
title: 使用iOS Authentication Access Enabler时Adobe Pass上的SSO
description: 使用iOS Authentication Access Enabler时Adobe Pass上的SSO
exl-id: 882f0abb-2e6e-461d-a375-3ab410991935
source-git-commit: 929d1cc2e0466155b29d1f905f2979c942c9ab8c
workflow-type: tm+mt
source-wordcount: '1121'
ht-degree: 0%

---

# 使用iOS Authentication Access Enabler时Adobe Pass上的SSO {#sso-on-ios-when-using-the-primetime-authentication-access-enabler}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

</br>

## 概述

由Adobe Pass身份验证提供支持的应用程序之间的单点登录(SSO)的工作方式有所不同，具体取决于底层操作系统。

使用Adobe Pass身份验证&#x200B;**Access Enabler**&#x200B;时，此文档地址iOS **上的** SSO。

**Access Enabler** **1.10**&#x200B;是Adobe Pass Authentication iOS本机SDK的最新版本。 Adobe强烈建议您迁移到此版本，而不是使用旧版本。 如果您使用的是旧版本的Access Enabler，则可以在[此处](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library)下载最新版本。

iOS上的SSO受以下条件限制：

- 应用必须使用相同的&#x200B;**令牌存储**（采用Access Enabler创建的自定义粘贴板的形式）。
- 应用必须生成相同的&#x200B;**设备ID** (Access Enabler根据MAC地址或IDFV计算设备ID，具体取决于操作系统版本)。

## 行为

SSO行为如下所示：

- **iOS 6及更低版本**： SSO在同一团队或其他团队开发的应用之间自动工作。 设备ID根据MAC地址进行计算（相同的值在所有应用程序中产生），并且存储区域对所有应用程序都是通用的(自定义粘贴板可在iOS 6及更低版本上的各个应用程序之间共享)。
   - **重要信息：**&#x200B;请注意，iOS SDK 1.9.4版本已[将最低iOS部署目标增加到iOS 7。](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library)
- **iOS 7及更高版本**： SSO将在以下条件下工作：

1. 使用相同的Apple分发配置文件或属于同一团队的配置文件发布应用程序。 这是应用程序在iOS 7和更高版本上共享自定义粘贴板的唯一方法。 在所有其他情况下，粘贴板是按应用程序沙箱的。 来自&#x200B;[*https://developer.apple.com/library/IOs/releasenotes/General/RN-iOSSDK-7.0/index.html*](https://developer.apple.com/library/ios/releasenotes/General/RN-iOSSDK-7.0/index.html)： \+\[`UIPasteboard pasteboardWithName:create:\`]和+\[`UIPasteboard pasteboardWithUniqueName`\]的给定名称现在是唯一的，仅允许同一应用程序组中的这些应用程序访问粘贴板。 如果开发人员尝试使用已存在的名称创建粘贴板，并且这些名称不属于同一应用程序包，则将获取他们自己的独特专用粘贴板。 请注意，这不会影响系统提供的粘贴板、“常规”和“查找”。

1. 应用程序具有相同的捆绑ID前缀（除最后一个组件之外的所有组件）。 只有共享同一捆绑ID前缀的应用程序才会计算相同的IDFV。 从&#x200B;[*https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIDevice\_Class/index.html\#//apple\_ref/occ/instp/UIDevice/identifierForVendor*](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIDevice_Class/index.html#//apple_ref/occ/instp/UIDevice/identifierForVendor)：在IOS 7上，捆绑包的所有组件（最后一个组件除外）都用于生成供应商ID。 如果捆绑ID只有一个组件，则使用整个捆绑ID。

现在我们重点讨论&#x200B;**&#39;iOS 7及更高版本&#39;**&#x200B;方案，因为它是真实用户最常使用的方案：

这两个条件（共享来自同一开发团队的用户档案并具有公共捆绑标识符前缀）是SSO的必需条件。

以下是可能的组合及其产生的结果：

- **来自相同团队和相同捆绑ID前缀的配置文件**：应用将共享相同的粘贴板存储，并具有相同的设备ID (IDFV)。 用户只需要验证一次（在使用的第一个应用程序中），并且身份验证状态将在所有其他应用程序之间共享。 示例流程：

1. 用户打开应用程序A（捆绑包ID为&#x200B;*com.x.y.AppA*）且未经过身份验证
1. 用户在应用程序A中执行身份验证
1. 用户打开应用程序B（包ID为&#x200B;*com.x.y.AppB*），并通过共享来自应用程序的权利数据自动进行身份验证
A（步骤2）
1. 用户打开应用程序A，并且仍旧进行身份验证（步骤2）



- **来自同一团队但包ID前缀不同的配置文件**：应用将共享相同的粘贴板存储，但设备ID (IDFV)不同。 用户需要为每个应用程序验证一次。 示例流程：

1. 用户打开应用程序A（捆绑包ID为&#x200B;*com.x.y.AppA*）且未经过身份验证
1. 用户在应用程序A中执行身份验证
1. 用户打开应用程序B（捆绑ID为&#x200B;*com.z.AppB*），Access Enabler检测到第一个应用程序获得的令牌（因为存储已共享），但不会尝试通过SSO使用它，因为设备ID不同
1. 用户在应用程序B中执行身份验证
1. 用户打开应用程序A，并且仍旧进行身份验证（步骤2）



- **来自不同团队的个人资料（此场景中捆绑包ID无关）**：应用程序将具有不同的粘贴板存储，并且它们之间将禁用SSO。 用户需要为每个应用程序验证一次，并且在应用程序之间切换时，身份验证会话将保持持久性。 示例流程：


1. 用户打开应用程序A且未进行身份验证
1. 用户在应用程序A中执行身份验证
1. 用户打开应用程序B且未进行身份验证
1. 用户在应用程序B中执行身份验证
1. 用户打开应用程序A并进行身份验证（步骤2）
1. 用户打开应用程序B并进行身份验证（步骤4）

**注意：**&#x200B;请注意，通过&#x200B;**Apple App Store**&#x200B;安装应用程序时，上述SSO条件适用。 如果应用程序部署在模拟器上（应用程序签名不适用）、使用Xcode安装或通过Ad Hoc配置文件分发，您可能会获得不同的结果。

**重要信息：**&#x200B;注意（**关于AccessEnabler v1.8**）：上面描述的第二种情况（来自同一团队但包ID前缀不同的配置文件）将在由同一团队（媒体公司）开发的应用程序上为&#x200B;**AccessEnabler v1.8**&#x200B;的用户创建非常不愉快的用户体验。 用户在来自同一媒体公司的应用程序之间过渡时将自动注销，因此，应用程序开发人员在决定捆绑包ID和分发配置文件时必须小心。 此案例的确切情况如下所示：

应用程序将共享相同的粘贴板存储，但具有不同的设备ID (IDFV)。 用户需要为每个应用验证一次，**，但在应用之间切换时将擦除身份验证会话**。 示例流程：

1. 用户打开应用程序A（捆绑包ID为&#x200B;*com.x.y.AppA*）且未经过身份验证
1. 用户在应用程序A中执行身份验证
1. 用户打开应用程序B（捆绑ID为&#x200B;*com.z.AppB*），并且应用程序A创建的权利数据由Access Enabler自动清除（安全机制，可检测应用程序B中当前计算的设备ID与应用程序A创建的权利令牌中存储的设备ID之间的冲突）
1. 用户在应用程序B中执行身份验证
1. 用户打开应用程序A，应用程序B创建的权利数据会被Access Enabler自动清除（安全机制，可检测应用程序A中当前计算的设备ID与应用程序B创建的权利令牌中存储的设备ID之间的冲突）
