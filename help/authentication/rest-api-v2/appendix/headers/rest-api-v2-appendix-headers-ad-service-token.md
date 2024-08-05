---
title: 标头 — AD-Service-Token
description: REST API V2 — 标头 — AD-Service-Token
source-git-commit: 150e064d0287eaac446c694fb5a2633f7ea4b797
workflow-type: tm+mt
source-wordcount: '252'
ht-degree: 1%

---


# 标头 — AD-Service-Token {#header-ad-service-token}

>[!NOTE]
>
> 此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## 概述 {#overview}

<b>AD-Service-Token</b>请求标头包含作为`JWS`的唯一用户标识符，该标识符从Adobe Pass身份验证系统外部运行的身份服务获取。

此标头旨在用于利用服务令牌方法启用单点登录(SSO)的流中。

有关利用服务令牌方法启用单点登录(SSO)流的更多详细信息，请参阅[使用服务令牌流的单点登录](../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md)文档。

## 语法 {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AD-Service-Token</b>： &lt;unique_user_identifier&gt;</td>
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

<b>唯一用户标识符</b>

JSON Web签名(`JWS`)是包含唯一用户标识符信息的已签名JSON Web令牌(`JWT`)。

`JWT`具有以下属性：

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">属性</th>
      <th style="background-color: #EFF2F7;">描述</th>
   </tr>
   <tr>
      <td>iss</td>
      <td>与为应用程序提供外部身份服务以实现单点登录(SSO)的实体关联的唯一标识符。</td>
   </tr>
   <tr>
      <td>sub</td>
      <td>外部标识服务返回的用户的唯一标识符。</td>
   </tr>
   <tr>
      <td>aud</td>
      <td>受众，应为“Adobe”。</td>
   </tr>
   <tr>
      <td>iat</td>
      <td>为当前JWT颁发的at时间戳。</td>
   </tr>
   <tr>
      <td>费用</td>
      <td>当前JWT的过期时间戳。</td>
   </tr>
</table>

`JWT`必须使用`SHA256withRSA`算法签名。

`JWT`必须使用私钥进行签名，私钥是一对RSA私钥的一部分，公钥由外部标识服务管理。

必须将该对的公钥移交给Adobe Pass身份验证，以便能够识别使用上述私钥签名的`JWT`令牌。

## 示例 {#examples}

```JSON
// JWT
// Header
// {
//  "alg": "RS256",
//  "kid": "qapEaY0hYNvphytwII3Sae_cAKyLS7GZOqtT_a4ajeo"
// }
// Payload data
// {
//  "sub": "Jane",
//  "name": "Jane Smith",
//  "iat": 1516239022,
//  "iss": "adobe",
//  "exp": 1720152820,
//  "aud": "adobe",
//  "jti": "3b2fb040-30a9-43d7-b647-d00ac495bab"
// }
 
// JWS
// eyJhbGciOiJSUzI1NiIsImtpZCI6InFhcEVhWTBoWU52cGh5dHdJSTNTYWVfY0FLeUxTN0daT3F0VF9hNGFqZW8ifQ.eyJzdWIiOiJKYW5lIiwibmFtZSI6IkphbmUgU21pdGgiLCJpYXQiOjE1MTYyMzkwMjIsImlzcyI6ImFkb2JlIiwiZXhwIjoxNzIwMTUyODIwLCJhdWQiOiJhZG9iZSIsImp0aSI6IjNiMmZiMDQwLTMwYTktNDNkNy1iNjQ3LWQwMGFjNDk1YmFiIn0.stHLZFh-635LDNjv9HRHzq912ICNCVGUS3f4RS_bAxpUiUSB6CShS2VvU4V-THEXj7d_zk1mxtPP0QM_pCrh4Vk2GaPRa856Bt_PhsfQY-_benDcB6MIoFX67qrREGncGiv7JEs3ksa-P1YvBYXolT7t52K093kFaQtICfB-aBa8danRZvUrJHjjFoILEpTbQuzxKRN6y36J3p1FZ-SfDuofHp3SnXDrWFRYyXYQnb9WFlhNBxR400-0vzTONZYd097WWy1shMw5V8TvIDvCDE5ifqk31gMdYga-N3JkcTA5QoW7Zl80UV7BhR5v14Va1IZLcbFra_UJdEzbBwW_nA

AD-Service-Token: eyJhbGciOiJSUzI1NiIsImtpZCI6InFhcEVhWTBoWU52cGh5dHdJSTNTYWVfY0FLeUxTN0daT3F0VF9hNGFqZW8ifQ.eyJzdWIiOiJKYW5lIiwibmFtZSI6IkphbmUgU21pdGgiLCJpYXQiOjE1MTYyMzkwMjIsImlzcyI6ImFkb2JlIiwiZXhwIjoxNzIwMTUyODIwLCJhdWQiOiJhZG9iZSIsImp0aSI6IjNiMmZiMDQwLTMwYTktNDNkNy1iNjQ3LWQwMGFjNDk1YmFiIn0.stHLZFh-635LDNjv9HRHzq912ICNCVGUS3f4RS_bAxpUiUSB6CShS2VvU4V-THEXj7d_zk1mxtPP0QM_pCrh4Vk2GaPRa856Bt_PhsfQY-_benDcB6MIoFX67qrREGncGiv7JEs3ksa-P1YvBYXolT7t52K093kFaQtICfB-aBa8danRZvUrJHjjFoILEpTbQuzxKRN6y36J3p1FZ-SfDuofHp3SnXDrWFRYyXYQnb9WFlhNBxR400-0vzTONZYd097WWy1shMw5V8TvIDvCDE5ifqk31gMdYga-N3JkcTA5QoW7Zl80UV7BhR5v14Va1IZLcbFra_UJdEzbBwW_nA
```
