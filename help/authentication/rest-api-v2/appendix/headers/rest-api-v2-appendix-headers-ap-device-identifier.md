---
title: 标头 — AP-Device-Identifier
description: REST API V2 — 标头 — AP-Device-Identifier
exl-id: 90a5882b-2e6d-4e67-994a-050465cac6c6
source-git-commit: 8f4fb5d6cc8b45b300010438c56d4af2e8fc0a76
workflow-type: tm+mt
source-wordcount: '413'
ht-degree: 0%

---

# 标头 — AP-Device-Identifier {#header-ap-device-identifier}

>[!NOTE]
>
> 此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## 概述 {#overview}

<b>AP-Device-Identifier</b>请求标头包含由客户端应用程序创建的流设备标识符。

## 语法 {#syntax}

<table>
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

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">类型</th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td>指纹</td>
      <td>
            设备标识符由客户端应用程序创建和管理的一个稳定的唯一标识符组成。
            <br/>
            客户端应用程序必须阻止由于用户操作（如应用程序卸载、重新安装或升级）引起的值更改。
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

要为运行[iOS或iPadOS](https://developer.apple.com/documentation/ios-ipados-release-notes)的设备生成`AP-Device-Identifier`标头，您可以参考以下文档：

* [identifierForVendor](https://developer.apple.com/documentation/uikit/uidevice/1620059-identifierforvendor)的Apple开发人员文档。

_(*)建议对OS提供的值应用SHA-256哈希函数。_

#### Android {#android}

要为运行[Android](https://developer.android.com/about/versions)的设备生成`AP-Device-Identifier`标头，您可以参考以下文档：

* [ANDROID_ID](https://developer.android.com/reference/android/provider/Settings.Secure#ANDROID_ID)的Android开发人员文档。

_(*)建议对OS提供的值应用SHA-256哈希函数。_

### TV连接的设备 {#tv-connected-devices}

#### tvOS {#tvos}

要为运行[tvOS](https://developer.apple.com/documentation/tvos-release-notes)的设备生成`AP-Device-Identifier`标头，您可以参考以下文档：

* [identifierForVendor](https://developer.apple.com/documentation/uikit/uidevice/1620059-identifierforvendor)的Apple开发人员文档。

_(*)建议对OS提供的值应用SHA-256哈希函数。_

#### Fire操作系统 {#fireos}

要为运行[Fire OS](https://developer.amazon.com/docs/fire-tv/fire-os-overview.html)的设备生成`AP-Device-Identifier`标头，您可以参考以下文档：

* [ANDROID_ID](https://developer.android.com/reference/android/provider/Settings.Secure#ANDROID_ID)的Android开发人员文档。

_(*)建议对OS提供的值应用SHA-256哈希函数。_

#### Roku OS {#rokuos}

要为运行[Roku OS](https://developer.roku.com/docs/developer-program/release-notes/roku-os-release-notes.md)的设备生成`AP-Device-Identifier`标头，您可以参考以下文档：

* [GetChannelClientId](https://developer.roku.com/docs/references/brightscript/interfaces/ifdeviceinfo.md#getchannelclientid-as-string)的Roku开发人员文档。

_(*)建议对OS提供的值应用SHA-256哈希函数。_
