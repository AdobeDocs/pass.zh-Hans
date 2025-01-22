---
title: 如何使用Adobe API测试站点测试身份验证和授权流
description: 如何使用Adobe API测试站点测试身份验证和授权流
exl-id: 04af4aed-35e4-44cb-98ce-7643165a8869
source-git-commit: 811feba1f2476bdfacb20e332e33df7f7ae8ac00
workflow-type: tm+mt
source-wordcount: '380'
ht-degree: 0%

---

# （旧版）如何使用Adobe的API测试站点测试身份验证和授权流 {#How-to-test-auth-flows}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

>[!IMPORTANT]
>
> 确保随时了解汇总在[产品公告](/help/authentication/product-announcements.md)页中的最新Adobe Pass身份验证产品公告和停用时间表。

为了测试AuthN和AuthZ流，我们准备了一个&#x200B;**API测试站点**，可供您使用。 我们的支持团队将很乐意为您提供凭据。 您可以通过&#x200B;**tve-support@adobe.com**&#x200B;联系我们。


## 第一部分 {#part-I}

要针对版本环境进行测试，请直接跳至第二部分。  要在预认证环境中进行测试，请参阅[在预认证环境中设置环境和测试](/help/authentication/notes-technical/environments/setting-up-your-environment-and-testing-in-prequal.md)。

## 第二部分

完成第一部分后，请执行以下步骤：


1. 打开网页： [测试API](https://sp.auth-staging.adobe.com/apitest/api.html)。
1. 通过以下方式加载访问启用码：
   * 从下拉菜单中选取所需的AccessEnabler版本（v3或v4）、要从何处访问（暂存或生产）以及是否应处于调试模式
   * 输入要在其中测试是否使用v4的软件语句
   * 然后单击“**Load Access Enabler**”按钮。
1. 现在，将请求者ID值设置为“**requestorID**”，然后单击“setRequestor”按钮。
1. 之后，按“getAuthentication”按钮，等待显示选择器出现。
1. 从选取器中选择“**MVPD**”。
1. 在“**MVPD**”登录页面上输入您的凭据。
1. 重定向回之后，重做步骤1至3
1. 在“setAuthenticationStatus”上重新执行步骤3后，您应该会看到值“1”。 如果身份验证不起作用，将显示MVPD对话框。
1. 要测试授权，请在标记为“checkAuthorization”和“getAuthorization”的按钮右侧的输入字段中输入要授权的&#x200B;**资源**，然后单击“getAuthorization”按钮。
1. 因此，在“setToken” — \>“resource id”文本框中将显示资源，在“setToken” — \>“token”文本框中将显示shortAuthorizationToken，表示authZ成功。
1. 现在，您可以单击“注销”按钮以删除令牌。
