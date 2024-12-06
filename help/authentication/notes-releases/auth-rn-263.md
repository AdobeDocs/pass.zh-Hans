---
title: Adobe Pass Authentication 2.63发行说明
description: Adobe Pass Authentication 2.63发行说明
exl-id: 40987328-6d41-4948-aa4a-bab31f98a18a
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '304'
ht-degree: 0%

---

# Adobe Pass Authentication 2.63发行说明 {#pt-authn-263-rn}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

本页介绍了此版本的新增功能、更改和已知问题：

## 服务器端和Web客户端 {#server-side-web-clients-263}

* [内部版本号](#build-number)
* [新增功能](#new-features)

### 内部版本号 {#build-number-263}

Adobe Pass身份验证： adobe-pass-**2.63**
发行日期： **09/20/2022 - 09/22/2022**

### 新增功能 {#new-features-263}

#### 改进平台识别机制 {#pf-identification-mech}

* 从此版本开始，我们改进了用于识别设备的机制，不再依赖于客户端实施。 在平台级别应用业务规则时，这将提供更准确的粒度，并更好地了解ESM报表中的流量值。

* 随后将发布新的ESM版本，其中新增和改进的报告将展示与平台相关的字段。

* 有关计划变更的更多详细信息，请联系您的TAM。

#### MVPD自降解 {#mvpd-self-degradation}

此功能使MVPD能够在高流量情况下临时绕过自己的身份验证和授权端点，前提是这些端点各自的负载过高。


#### 在授权调用的标头中添加代理的ID {#add-proxied-id}

此功能在授权调用的标头中添加Synacor代理的MVPD的ID。 这使Synacor能够为每个代理的个人(例如 路由到不同的域（根据代理的MVPD）。


#### 动态仪表板 {#tve-dashboard}

在此版本中，我们修复了配置报告中未正确计算在MVPD级别设置的authN或authZ TTL的问题。


#### JavaScript SDK 4.6.0 {#js-sdk}

* 删除了`eval`函数的使用，从而使SDK符合内容安全策略。
* 修复了合作伙伴应用程序明确清除浏览器的本地存储时，身份验证流无法成功完成的问题。
