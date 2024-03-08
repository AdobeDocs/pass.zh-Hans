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

本文档地址 **iOS上的SSO**，使用Adobe Pass身份验证时 **访问启用码**.

**访问启用码** **1.10** 是Adobe Pass Authentication iOS本机SDK的最新版本。 Adobe强烈建议您迁移到此版本，而不是使用旧版本。 如果您使用的是旧版Access Enabler，则可以下载最新版本 [此处](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library).

iOS上的SSO受以下条件限制：

- 应用程序必须使用相同的 **令牌存储** （以Access Enabler创建的自定义粘贴板的形式）。
- 应用程序必须生成相同的 **设备ID** (Access Enabler根据MAC地址或IDFV（具体取决于操作系统版本）计算设备ID。)

## 行为

SSO行为如下所示：

- **iOS 6及更低版本**：SSO在同一团队或不同团队开发的应用程序之间自动工作。 设备ID根据MAC地址进行计算（相同的值在所有应用程序中产生），并且存储区域对所有应用程序都是通用的(自定义粘贴板可在iOS 6及更低版本上的各个应用程序之间共享)。
   - **重要提示：** 请注意，iOS SDK 1.9.4版本具有 [将iOS的最低部署目标提高到了iOS 7。](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library)
- **iOS 7及更高版本**：SSO将在以下条件下工作：

1. 使用相同的Apple分发配置文件或属于同一团队的配置文件发布应用程序。 这是应用程序在iOS 7和更高版本上共享自定义粘贴板的唯一方法。 在所有其他情况下，粘贴板是按应用程序沙箱的。 从 [*https://developer.apple.com/library/IOs/releasenotes/General/RN-iOSSDK-7.0/index.html*](https://developer.apple.com/library/ios/releasenotes/General/RN-iOSSDK-7.0/index.html)： \+\[`UIPasteboard pasteboardWithName:create:\`]和+\[`UIPasteboard pasteboardWithUniqueName`\]现在唯一指定名称，以便仅允许同一应用程序组中的这些应用程序访问粘贴板。 如果开发人员尝试使用已存在的名称创建粘贴板，并且这些名称不属于同一应用程序包，则将获取他们自己的独特专用粘贴板。 请注意，这不会影响系统提供的粘贴板、“常规”和“查找”。

1. 应用程序具有相同的捆绑ID前缀（除最后一个组件之外的所有组件）。 只有共享同一捆绑ID前缀的应用程序才会计算相同的IDFV。 从 [*https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIDevice\_Class/index.html\#//apple\_ref/occ/instp/UIDevice/identifierForVendor*](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIDevice_Class/index.html#//apple_ref/occ/instp/UIDevice/identifierForVendor)：在IOS 7上，捆绑包的所有组件（最后一个组件除外）都用于生成供应商ID。 如果捆绑ID只有一个组件，则使用整个捆绑ID。

现在，我们来关注一下 **&#39;iOS 7及更高版本&#39;** 场景，因为它是实际用户最常访问的：

这两个条件（共享来自同一开发团队的用户档案并具有公共捆绑标识符前缀）是SSO的必需条件。

以下是可能的组合及其产生的结果：

- **来自同一团队和同一捆绑ID前缀的配置文件**：应用程序将共享相同的粘贴板存储，并具有相同的设备ID (IDFV)。 用户只需要验证一次（在使用的第一个应用程序中），并且身份验证状态将在所有其他应用程序之间共享。 示例流程：

1. 用户打开应用程序A（带有捆绑包ID） *com.x.y.AppA*)，并且未进行身份验证
1. 用户在应用程序A中执行身份验证
1. 用户打开应用程序B（带有捆绑包ID） *com.x.y.AppB*)，并通过共享来自应用程序A的权利数据来自动进行身份验证（步骤2）
1. 用户打开应用程序A，并且仍旧进行身份验证（步骤2）



- **来自同一团队但包ID前缀不同的配置文件**：应用程序将共享相同的粘贴板存储，但具有不同的设备ID (IDFV)。 用户需要为每个应用程序验证一次。 示例流程：

1. 用户打开应用程序A（带有捆绑包ID） *com.x.y.AppA*)，并且未进行身份验证
1. 用户在应用程序A中执行身份验证
1. 用户打开应用程序B（带有捆绑包ID） *com.z.AppB*)，并且Access Enabler会检测第一个应用程序获得的令牌（因为存储是共享的），但是由于设备ID不同，因此它不会尝试通过SSO使用该令牌
1. 用户在应用程序B中执行身份验证
1. 用户打开应用程序A，并且仍旧进行身份验证（步骤2）



- **来自不同团队的用户档案（在此场景中，捆绑ID无关）**：应用程序将具有不同的粘贴板存储，并且它们之间将禁用SSO。 用户需要为每个应用程序验证一次，并且在应用程序之间切换时，身份验证会话将保持持久性。 示例流程：


1. 用户打开应用程序A且未进行身份验证
1. 用户在应用程序A中执行身份验证
1. 用户打开应用程序B且未进行身份验证
1. 用户在应用程序B中执行身份验证
1. 用户打开应用程序A并进行身份验证（步骤2）
1. 用户打开应用程序B并进行身份验证（步骤4）

**注意：** 请注意，通过安装应用程序时，上述SSO条件适用 **Apple App Store**. 如果应用程序部署在模拟器上（应用程序签名不适用）、使用Xcode安装或通过Ad Hoc配置文件分发，您可能会获得不同的结果。

**重要提示：** 注释(**关于AccessEnabler v1.8**)：上述第二种情况（来自同一团队但包ID前缀不同的用户档案）将会为的用户带来非常不愉快的用户体验。 **AccessEnabler v1.8** 跨由同一团队（媒体公司）开发的应用程序。 用户在来自同一媒体公司的应用程序之间过渡时将自动注销，因此，应用程序开发人员在决定捆绑包ID和分发配置文件时必须小心。 此案例的确切情况如下所示：

应用程序将共享相同的粘贴板存储，但具有不同的设备ID (IDFV)。 用户需要为每个应用程序验证一次， **但是在应用程序之间切换时，身份验证会话将被清除**. 示例流程：

1. 用户打开应用程序A（带有捆绑包ID） *com.x.y.AppA*)，并且未进行身份验证
1. 用户在应用程序A中执行身份验证
1. 用户打开应用程序B（带有捆绑包ID） *com.z.AppB*)，并且应用程序A创建的权利数据会被Access Enabler自动清除（安全机制，可检测应用程序B中当前计算的设备ID与权利令牌中存储的设备ID之间的冲突 — 由应用程序A创建）
1. 用户在应用程序B中执行身份验证
1. 用户打开应用程序A，应用程序B创建的权利数据会被Access Enabler自动清除（安全机制，可检测应用程序A中当前计算的设备ID与应用程序B创建的权利令牌中存储的设备ID之间的冲突）
