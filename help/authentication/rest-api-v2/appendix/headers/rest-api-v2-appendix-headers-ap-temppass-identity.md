---
title: 标头 — AP-TempPass-Identity
description: REST API V2 — 标头 — AP-TempPass-Identity
source-git-commit: 150e064d0287eaac446c694fb5a2633f7ea4b797
workflow-type: tm+mt
source-wordcount: '84'
ht-degree: 2%

---


# 标头 — AP-TempPass-Identity {#header-ap-temppass-identity}

>[!NOTE]
>
> 此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## 概述 {#overview}

<b>AP-TempPass-Identity</b>请求标头包含用于实现提升TempPass的用户标识信息。

## 语法 {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP-TempPass-Identity</b>： &lt;用户标识信息&gt;</td>
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

<b>&lt;用户标识信息></b>

与需要向其授予促销临时访问权限的最终用户相关联的用户标识信息上的`Base64-encoded`值。

## 示例 {#examples}

```JSON
// Identity
// {"email": "example@domain.com"}

// Base64-encoded
// eyJlbWFpbCI6ICJleGFtcGxlQGRvbWFpbi5jb20ifQ==

AP-TempPass-Identity: eyJlbWFpbCI6ICJleGFtcGxlQGRvbWFpbi5jb20ifQ==
```
