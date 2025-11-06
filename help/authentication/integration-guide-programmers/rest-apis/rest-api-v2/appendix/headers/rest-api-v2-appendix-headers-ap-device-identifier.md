---
title: 标头 — AP-Device-Identifier
description: REST API V2 — 标头 — AP-Device-Identifier
exl-id: 90a5882b-2e6d-4e67-994a-050465cac6c6
source-git-commit: 81d3c3835d2e97e28c2ddb9c72d1a048a25ad433
workflow-type: tm+mt
source-wordcount: '485'
ht-degree: 0%

---

# 标头 — AP-Device-Identifier {#header-ap-device-identifier}

>[!NOTE]
>
> 此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## 概述 {#overview}

<b>AP-Device-Identifier</b>请求标头包含由客户端应用程序创建的流设备标识符。

## 语法 {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP设备标识符</b>： &lt;类型&gt; &lt;标识符&gt;</td>
   </tr>
   <tr>
      <td>标题类型</td>
      <td>请求标头</td>
   </tr>
   <tr>
      <td>标准</td>
      <td>否</td>
   </tr>
</table>

## 指令 {#directives}

<b>&lt;类型></b>

设备标识符类型。

只有一种受支持的类型，如下所示。

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">类型</th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td>指纹</td>
      <td>
            设备标识符由客户端应用程序为每个设备创建和管理的稳定且唯一的标识符组成。
            <br/>
            客户端应用程序应将设备标识符缓存在永久存储中，因为丢失或更改它会使身份验证失效。 客户端应用程序应防止由于用户操作（如应用程序卸载、重新安装或升级）而导致值更改。
      </td>
   </tr>
</table>


<b>&lt;标识符></b>

设备标识符的`Base64-encoded`值。

## 示例 {#example}

```JSON
// device identifier
// ba23d141-d715-561c-94f4-e9e4c966b1eb

// Base64-encoded
// YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi

AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
```

## 指南 {#cookbooks}

>[!IMPORTANT]
>
> 提供文档资源以供参考。
>
> 文档资源不是详尽的，可能需要额外的修改才能在您的项目中正常工作。
> 
> 无论实际实施如何，`AP-Device-Identifier`标头都必须包含一个格式化的值，如[指令](#directives)部分中所述。

### 浏览器 {#browsers}

要为浏览器中运行的设备生成`AP-Device-Identifier`标头，您的客户端应用程序需要根据可用数据（如浏览器、设备或特定于用户的数据）计算稳定且唯一的标识符。

_(*)建议集成提供浏览器或设备指纹识别机制的库或服务。_

### 移动设备 {#mobile-devices}

#### iOS和iPadOS {#ios-ipados}

要为运行`AP-Device-Identifier`iOS或iPadOS[的设备生成](https://developer.apple.com/documentation/ios-ipados-release-notes)标头，您可以参考以下文档：

* [identifierForVendor](https://developer.apple.com/documentation/uikit/uidevice/1620059-identifierforvendor)的Apple开发人员文档。

_(*)建议对OS提供的值应用SHA-256哈希函数。_

#### Android {#android}

要为运行`AP-Device-Identifier`Android[的设备生成](https://developer.android.com/about/versions)标头，您可以参考以下文档：

* [ANDROID_ID](https://developer.android.com/reference/android/provider/Settings.Secure#ANDROID_ID)的Android开发人员文档。

_(*)建议对OS提供的值应用SHA-256哈希函数。_

### TV连接的设备 {#tv-connected-devices}

#### tvOS {#tvos}

要为运行`AP-Device-Identifier`tvOS[的设备生成](https://developer.apple.com/documentation/tvos-release-notes)标头，您可以参考以下文档：

* [identifierForVendor](https://developer.apple.com/documentation/uikit/uidevice/1620059-identifierforvendor)的Apple开发人员文档。

_(*)建议对OS提供的值应用SHA-256哈希函数。_

#### Fire操作系统 {#fireos}

要为运行`AP-Device-Identifier`Fire OS[的设备生成](https://developer.amazon.com/docs/fire-tv/fire-os-overview.html)标头，您可以参考以下文档：

* [ANDROID_ID](https://developer.android.com/reference/android/provider/Settings.Secure#ANDROID_ID)的Android开发人员文档。

_(*)建议对OS提供的值应用SHA-256哈希函数。_

#### Roku OS {#rokuos}

要为运行`AP-Device-Identifier`Roku OS[的设备生成](https://developer.roku.com/docs/developer-program/release-notes/roku-os-release-notes.md)标头，您可以参考以下文档：

* [GetChannelClientId](https://developer.roku.com/docs/references/brightscript/interfaces/ifdeviceinfo.md#getchannelclientid-as-string)的Roku开发人员文档。

_(*)建议对OS提供的值应用SHA-256哈希函数。_

### 其他  {#others}

对于文档中未涵盖的设备平台，设备标识符应链接到任何可用的硬件标识，通常在设备的硬件手册中指定。

如果没有可用的硬件标识符，则应该使用基于客户端应用程序属性的唯一生成的标识符，并将其缓存在永久存储中。
