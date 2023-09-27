---
title: Clientless API 实施-错误代码/具有可能原因/原因的消息
description: Clientless API 实施-错误代码/具有可能原因/原因的消息
exl-id: 616e35fc-9b72-422b-9a05-e6248bd52490
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 0%

---

# Clientless API 实施-错误代码/具有可能原因/原因的消息 {#clientless-api-implementation--error-codes-messages-with-probable-reason-cause}

>[!NOTE]
>
>此页面上的内容仅用于信息目的。 使用此 API 需要 Adobe Systems 的当前许可证。 不允许未经授权使用。

</br>


## 错误：未授权

### 原因：

1. POST 中缺少授权标头
1. 授权标头问题-如果请求时间为毫秒，则检查。

## 错误：身份验证期间的 SC 400

### 原因：

1. 服务器找不到为特定请求程序和环境创建的注册代码。
1. 您可能会遇到跨域脚本问题
1. 应将适当的欺骗添加到/etc/hosts 文件

## 错误：400错误请求

### 原因：

1. POST/GET 的 url 格式不正确
1. SAMLAssertionParserException –无法在 Adobe Systems 的结尾解密加密的 SAML 声明

## 错误：403禁止

### 原因：

1. 快速请求太多--API 管理的一项功能可防止 DoS 攻击。
2. 如果使用 prequal 环境则添加电子欺骗，否则请确保已从/etc/hosts 文件中删除了欺骗

## 错误：无法登录到 MVPD 页面

### 原因：

1. 用户名和密码不匹配
2. 可能已禁用登录
3. 检查登录是用于生产还是暂存


<!--

## Related Information

- [Clientless API Reference](/help/authentication/rest-api-reference.md)

-->
