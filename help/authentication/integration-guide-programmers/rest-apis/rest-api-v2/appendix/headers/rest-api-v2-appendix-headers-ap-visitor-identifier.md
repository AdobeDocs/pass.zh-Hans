---
title: 标头 — AP-Visitor-Identifier
description: REST API V2 — 标头 — AP-Visitor-Identifier
exl-id: b4f8e2a1-9c7d-4e3a-8f5b-2d1c6e9a4b7f
source-git-commit: 26245e019afac2c0844ed64b222208cc821f9c6c
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 2%

---


# 标头 — AP-Visitor-Identifier {#header-ap-visitor-identifier}

>[!NOTE]
>
> 此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## 概述 {#overview}

<b>AP-Visitor-Identifier</b>请求标头包含客户端应用程序在所有Adobe Experience Cloud解决方案中唯一标识访客所需的`ECID`。

有关在Adobe Pass身份验证中使用ECID的更多详细信息，请参阅[在Adobe Pass身份验证中使用Experience Cloud ID](../../../../features-premium/analytics/exp-cloud-id-authn.md)文档。

## 语法 {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP访客标识符</b>： &lt;visitor_identifier&gt;</td>
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

## 示例 {#examples}

```JSON
AP-Visitor-Identifier: "THE_ECID_VALUE"
```
