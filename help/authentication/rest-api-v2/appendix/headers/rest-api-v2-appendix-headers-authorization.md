---
title: 标头 — 授权
description: REST API V2 — 标头 — 授权
exl-id: 86917d7e-ffd9-4d34-8f9c-5a50083f85e6
source-git-commit: ca8eaff83411daab5f136f01394e1d425e66f393
workflow-type: tm+mt
source-wordcount: '118'
ht-degree: 1%

---


# 标头 — 授权 {#header-authorization}

>[!NOTE]
>
> 此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## 概述 {#overview}

<b>Authorization</b>请求标头包含客户端应用程序访问受Adobe Pass保护的API所需的`Bearer`访问令牌。

有关访问Adobe Pass保护的API的机制的更多详细信息，请参阅[动态客户端注册概述](../../../dcr-api/dynamic-client-registration-overview.md)文档。

## 语法 {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>授权</b>：持有者&lt;access_token&gt;</td>
   </tr>
   <tr>
      <td>标题类型</td>
      <td>请求标头</td>
   </tr>
   <tr>
      <td>标准</td>
      <td>是</td>
   </tr>
</table>

## 指令 {#directives}

<b>&lt;access_token></b>

访问令牌值是一个具有有限生存时间（例如，24小时）的不透明值，必须从Adobe Pass获取，如[检索访问令牌](../../../dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) API文档中所述。

## 示例 {#examples}

```JSON
Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiI0NmY0MGZiMy01NmJkLTQyYTktOTExYS02YmZmNmEyZmY0
                      MDciLCJuYmYiOjE3MjM1NjE4ODUsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpO
                      mNsaWVudDp2MiIsImV4cCI6MTcyMzU4MzQ4NSwiaWF0IjoxNzIzNTYxODg1fQ.aZUZqwN2fCqNXgX
                      SdKFG9_HcqHjha66G6HmsfTJYcZc12iuLxMu7TT7MbhWVz3kW1jRqgJv8PHhrFSBL5_dgJ1PRSuDg
                      97ZK1secgMKwk46vKZVdtx7LF5t3jGVzQTwN4RqChqyvkW2o67KxVk5xarwJtwB2fwhX_732CYDcv
                      1gWOTLx4xyH5IVvg-P_aImyveG0D-x65I2nOKXaROVvv-kYE6B9OQv_-JBGj72R_yS2AyJQC0R_im
                      0h5S4YvL-c2UZrYK7pvdZq-xAj0uW1wad7PLZjl8yL5CWUz9vzQk2Cmj8adsydjb0u0P3aFrJ0HE9
                      ISqtRvjf4plR1TGWgw6
```
