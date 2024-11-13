---
title: 标头 — Adobe-Subject-Token
description: REST API V2 — 标头 — Adobe主题令牌
exl-id: 906d88f4-3b8f-491a-ab58-8e63d3b958d8
source-git-commit: e5ef8c0cba636ac4d2bda1abe0e121d0ecc1b795
workflow-type: tm+mt
source-wordcount: '166'
ht-degree: 1%

---

# 标头 — Adobe-Subject-Token {#header-adobe-subject-token}

>[!NOTE]
>
> 此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## 概述 {#overview}

<b>Request-Subject-Token</b>Adobe标头包含作为`JWS`或`JWE`的唯一平台标识符，该标识符是从在Adobe Pass身份验证系统外部运行的身份服务或库获得的。

此标头旨在用于利用Platform Identity方法启用单点登录(SSO)的流。

有关利用平台标识方法启用单点登录(SSO)流程的更多详细信息，请参阅[使用平台标识流程的单点登录](../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md)文档。

## 语法 {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>Adobe — 主题 — 令牌</b>： &lt;unique_platform_identifier&gt;</td>
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

* [Amazon SSO指南(REST API V2)](../../../single-sign-on/platform-single-sign-on/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v2.md)

## 示例 {#examples}

请参阅以下平台中描述的示例：

* [Amazon SSO指南(REST API V2)](../../../single-sign-on/platform-single-sign-on/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v2.md)
