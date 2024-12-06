---
title: 删除注册记录
description: 删除注册资源
exl-id: 42707070-2e1f-4847-93fd-30025aef56c1
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '153'
ht-degree: 1%

---

# 删除注册记录 {#delete-registration-record}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

>[!NOTE]
>
> REST API实现受[限制机制](/help/authentication/integration-guide-programmers/throttling-mechanism.md)限制

## REST API端点 {#clientless-endpoints}

&lt;REGGIE_FQDN>：

* 生产 — [api.auth.adobe.com](http://api.auth.adobe.com/)
* 暂存 — [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>：

* 生产 — [api.auth.adobe.com](http://api.auth.adobe.com/)
* 暂存 — [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>


## 描述 {#delete-record}

删除注册代码记录并释放注册代码以供重用。

| 端点 | </br>调用者 | 输入   </br>参数 | HTTP </br>方法 | 响应 | HTTP </br>响应 |
| --- | --- | --- | --- | --- | --- |
| &lt;REGGIE_FQDN>/reggie/v1/{requestorId}/regcode/{registrationCode}</br></br>例如：</br></br>&lt;REGGIE_FQDN>/reggie/v1/regcode/ER45RTY | 流式处理应用程序</br></br>或</br></br>程序员服务 | 1.请求者ID </br>    （路径组件）</br>2。  注册码</br>    （路径组件） | DELETE | 无 | 204 |

{style="table-layout:auto"}

</br>

| 输入参数 | 描述 |
| --- | --- |
| 请求者 | 此操作有效的程序员requestorId。 |
| 注册码 | 将在流设备上显示的注册代码值（将输入到身份验证流程中）。 |

{style="table-layout:auto"}

</br>

### [返回REST API引用](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
