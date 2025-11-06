---
title: 启动身份验证
description: 启动身份验证
exl-id: 55dddd29-68d6-4aae-8744-307fea285e29
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '306'
ht-degree: 0%

---

# （旧版）启动身份验证 {#initiate-authentication}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

>[!IMPORTANT]
>
> 确保随时了解汇总在[产品公告](/help/authentication/product-announcements.md)页中的最新Adobe Pass身份验证产品公告和停用时间表。

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


## 描述 {#description}

通过通知MVPD选择事件来启动身份验证过程。 在Adobe Pass身份验证数据库上创建记录，在从MVPD收到成功响应时进行协调。



| 端点 | </br>调用者 | 输入   </br>参数 | HTTP </br>方法 | 响应 | HTTP </br>响应 |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/身份验证 | AuthN模块 | &#x200B;1. requestor_id （必需）</br>2。  mso_id （必需）</br>3。  reg_code （必需）</br>4。  domain_name （必需）</br>5。  noflash=true - </br>    （必需，剩余参数）</br>6。  no_iframe=true （必需，剩余参数）</br>7。  额外参数（可选）</br>8。  redirect_url（必需） | GET | 登录Web应用程序将被重定向到MVPD登录页面。 | 302（完全重定向实施） |

{style="table-layout:auto"}


| 输入参数 | 描述 |
| --- | --- |
| requestor_id | 此操作有效的程序员请求者。 |
| mso_id | 此操作对其有效的MVPD ID。 |
| reg_code | Reggie服务生成的注册码。 |
| 域名 | 原始域。 |
| redirect_url | 身份验证完成后的登录Webapp重定向url。 |

{style="table-layout:auto"}

</br>

>[!IMPORTANT]
> 
>**重要提示：必需的参数 —**&#x200B;无论客户端实现如何，上述所有参数都是必需的。
>
>
>示例：
>
>```
>domain_name=loginwebapp.com
>mso_id=sampleMvpdId
>reg_code=RO0885W
>requestor_id=sampleRequestorId
>noflash=true
>redirect_url=http://loginwebapp.com
>```

>[!IMPORTANT]
> 
>**重要信息：可选参数**
>
>调用可能还包含可选参数，用于启用其他功能，例如：
>
> * generic\_data — 允许使用[促销临时传递](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#promotional-temp-pass)
>
>```JSON
>Example:
>   generic_data=("email":"email@domain.com")
>```


### **备注** {#notes}

* `domain_name`参数的值必须设置为在Adobe Pass身份验证中注册的域名之一。

* [避免在/authenticate请求中使用“&amp;”reg\_code（技术说明）](/help/authentication/integration-guide-programmers/legacy/notes-technical/clientless-avoid-using-reg-code-in-authenticate-request.md)

* `redirect_url`参数必须是顺序中的最后一个参数

* `redirect_url`参数的值必须为URL编码
