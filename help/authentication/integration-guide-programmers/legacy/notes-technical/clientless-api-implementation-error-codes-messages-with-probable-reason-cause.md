---
title: 无客户端 API 实现 - 具有可能原因/原因的错误代码/消息
description: 无客户端 API 实现 - 具有可能原因/原因的错误代码/消息
exl-id: 616e35fc-9b72-422b-9a05-e6248bd52490
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 0%

---

# （旧版）无客户端 API 实现 - 具有可能原因/原因的错误代码/消息 {#clientless-api-implementation--error-codes-messages-with-probable-reason-cause}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此 API 需要 Adobe 的最新许可证。 不允许未经授权使用。

>[!IMPORTANT]
>
> 确保您随时了解“产品公告[&#128279;](/help/authentication/product-announcements.md)”页面中汇总的最新 Adobe Pass 身份验证产品公告和停用时间表。

</br>


## 错误：未授权

### 原因：

1. 开机自检中缺少授权标头
1. 授权标头问题 - 检查请求时间是否以毫秒为单位。

## 错误：身份验证期间出现 SC 400

### 原因：

1. 服务器找不到注册码，该注册码是为特定请求者和环境创建的。
1. 您可能会遇到跨域脚本问题
1. 应将正确的欺骗添加到 /etc/hosts 文件中

## 错误： 400错误请求

### 原因：

1. POST/GET 的 URL 格式不正确
1. SAMLAssertionParserException – 无法在 Adobe 端解密加密的 SAML 断言

## 错误：403 禁止访问

### 原因：

1. 快速请求太多 - API 管理的一项功能，用于防止 DoS 攻击。
2. 如果使用 prequal 环境，请添加欺骗，否则请确保已从 /etc/hosts 文件中删除欺骗

## 错误：无法登录到 MVPD 页面

### 原因：

1. 用户名和密码不匹配
2. 登录可能已被禁用
3. 检查登录名是用于生产还是暂存


<!--

## Related Information

- [Clientless API Reference](/help/authentication/rest-api-reference.md)

-->
