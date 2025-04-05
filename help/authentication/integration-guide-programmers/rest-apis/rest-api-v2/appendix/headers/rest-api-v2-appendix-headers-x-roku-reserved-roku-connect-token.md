---
title: 标头 — X-Roku-Reserved-Roku-Connect-Token
description: REST API V2 — 标头 — X-Roku-Reserved-Roku-Connect-Token
source-git-commit: 640ba7073f7f4639f980f17f1a59c4468bfebcf4
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 0%

---

# 标头 — X-Roku-Reserved-Roku-Connect-Token {#header-x-roku-reserved-roku-connect-token}

>[!NOTE]
>
> 此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## 概述 {#overview}

<b>X-Roku-Reserved-Roku-Connect-Token</b>请求标头包含唯一的平台标识符，即`JWS`或`JWE`，该标识符从Adobe Pass身份验证系统外部运行的身份服务或库获取。

此标头旨在用于利用Platform Identity方法启用单点登录(SSO)的流。

有关利用平台标识方法启用单点登录(SSO)流程的更多详细信息，请参阅[使用平台标识流程的单点登录](../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md)文档。

## 语法 {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>X-Roku-Reserved-Roku-Connect-Token</b>： &lt;唯一平台标识符&gt;</td>
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

<b>unique_platform_identifier</b>

JSON Web签名(`JWS`)或JSON Web加密(`JWE`)是包含唯一平台标识符信息的已签名或加密JSON Web令牌(`JWT`)。

这适用于以下平台：

* [Roku SSO指南(REST API V2)](../../../../features-standard/sso-access/platform-sso/roku-single-sign-on/roku-sso-cookbook-rest-api-v2.md)
