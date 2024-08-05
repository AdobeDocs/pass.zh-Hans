---
title: 标头 — AP-Device-Identifier
description: REST API V2 — 标头 — AP-Device-Identifier
source-git-commit: 150e064d0287eaac446c694fb5a2633f7ea4b797
workflow-type: tm+mt
source-wordcount: '103'
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
      <td>设备标识符由不透明标识符组成，由客户端应用程序创建。</td>
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
