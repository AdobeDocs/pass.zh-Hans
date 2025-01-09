---
title: Dynamic Client Registration (DCR)术语表
description: Dynamic Client Registration (DCR)术语表
source-git-commit: 5622cad15383560e19e8111f12a1460e9b118efe
workflow-type: tm+mt
source-wordcount: '299'
ht-degree: 0%

---

# Dynamic Client Registration (DCR)术语表 {#rest-api-dcr-glossary}

>[!IMPORTANT]
>
> 此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

本文档提供了集成Adobe Pass身份验证动态客户端注册(DCR)时使用的术语的定义。

>[!MORELIKETHIS]
> 
> * [REST API v2术语表](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md)

## 术语表 {#glossary-terms}

### A {#a}

#### 访问令牌 {#access-token}

访问令牌是Adobe Pass身份验证为[动态客户端注册(DCR)](#dcr)进程生成的令牌，该进程旨在确保访问受保护的API。

### C {#c}

#### 客户端凭据 {#client-credentials}

客户端凭据是在[动态客户端注册(DCR)](#dcr)过程中生成的一组唯一值，旨在用于获取[访问令牌](#access-token)。

#### 自定义方案 {#custom-scheme}

自定义方案是引用[程序员](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#programmer)应用程序的唯一值，可以从Adobe Pass [TVE仪表板](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)生成和下载该应用程序，其目的是在iOS设备上运行的应用程序中用作最终重定向。

### 日期{#d}

#### DCR {#dcr}

动态客户端注册(DCR)是由[RFC 7591](https://datatracker.ietf.org/doc/html/rfc7591)定义的授权机制，它基于[RFC 6749](https://datatracker.ietf.org/doc/html/rfc6749)描述的OAuth 2.0授权框架。

DCR作为Adobe Pass身份验证服务交付给[程序员](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#programmer)，该服务可进一步启用对受保护API的访问。

有关详细信息，请参阅[动态客户端注册概述](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)文档。

### R {#r}

#### 已注册的应用程序 {#registered-application}

注册的应用程序是一个Adobe Pass身份验证概念，它存储有关[程序员](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#programmer)应用程序的信息，该应用程序需要继续执行[动态客户端注册(DCR)](#dcr)进程。

### S {#s}

#### 软件声明 {#software-statement}

软件语句是可以从Adobe Pass [TVE仪表板](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)下载的JSON Web令牌(JWT)，其目的是作为[动态客户端注册(DCR)](#dcr)进程的一部分使用。
