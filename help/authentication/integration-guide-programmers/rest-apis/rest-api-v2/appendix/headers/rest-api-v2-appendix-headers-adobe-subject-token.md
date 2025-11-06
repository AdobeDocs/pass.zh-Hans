---
title: 标头 — Adobe-Subject-Token
description: REST API V2 — 标头 — Adobe-Subject-Token
exl-id: 906d88f4-3b8f-491a-ab58-8e63d3b958d8
source-git-commit: 81d3c3835d2e97e28c2ddb9c72d1a048a25ad433
workflow-type: tm+mt
source-wordcount: '166'
ht-degree: 1%

---

# 标头 — Adobe-Subject-Token {#header-adobe-subject-token}

>[!NOTE]
>
> 此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## 概述 {#overview}

<b>Adobe-Subject-Token</b>请求标头包含作为`JWS`或`JWE`的唯一平台标识符，该标识符是从在Adobe Pass身份验证系统之外运行的身份服务或库获得的。

此标头旨在用于利用Platform Identity方法启用单点登录(SSO)的流。

有关利用平台标识方法启用单点登录(SSO)流程的更多详细信息，请参阅[使用平台标识流程的单点登录](../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md)文档。

## 语法 {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>Adobe-Subject-Token</b>： &lt;唯一平台标识符&gt;</td>
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

* [Amazon SSO指南(REST API V2)](../../../../features-standard/sso-access/platform-sso/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v2.md)

## 示例 {#examples}

请参阅以下平台中描述的示例：

* [Amazon SSO指南(REST API V2)](../../../../features-standard/sso-access/platform-sso/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v2.md)
