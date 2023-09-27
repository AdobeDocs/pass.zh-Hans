---
title: 如何使用Adobe API测试站点测试身份验证和授权流
description: 如何使用Adobe API测试站点测试身份验证和授权流
exl-id: 04af4aed-35e4-44cb-98ce-7643165a8869
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '318'
ht-degree: 0%

---

# 如何使用Adobe API测试站点测试身份验证和授权流 {#How-to-test-auth-flows}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

为了测试AuthN和AuthZ流，我们准备了一个 **API测试站点** 你可以随意选择。 我们的支持团队将很乐意为您提供凭据。 您可以通过联系我们： **support@tve.zendesk.com**.


## 第一部分 {#part-I}

要针对版本环境进行测试，请直接跳至第二部分。  要在资格预审环境中进行测试，请参阅 [在资格预审中设置环境和测试](/help/authentication/setting-up-your-environment-and-testing-in-prequal.md).

## 第二部分

完成第一部分后，请执行以下步骤：


1. 打开网页： [测试API](https://sp.auth-staging.adobe.com/apitest/api.html).
1. 从以下URL加载访问启用码：
   * [用于暂存的Access Enabler javascript](https://entitlement.auth-staging.adobe.com/entitlement/js/AccessEnabler.js).
   * 或者
   * [用于生产的Access Enabler javascript](https://entitlement.auth.adobe.com/entitlement/js/AccessEnabler.js).
   * 然后单击“**加载访问启用码**”按钮。
1. 现在，将请求者ID值设置为&quot;**请求者ID**”并单击“setRequestor”按钮。
1. 之后，按“getAuthentication”按钮，等待显示选择器出现。
1. 选择&quot;**MVPD**”来自选取器。
1. 在“”中输入您的凭据&#x200B;**MVPD**”登录页面。
1. 重定向回之后，重做步骤1至3
1. 在“setAuthenticationStatus”上重新执行步骤3后，您应该会看到值“1”。 如果身份验证不起作用，则会显示MVPD对话框。
1. 要测试授权，请在资源输入字段中输入&quot;**请求者ID**”并单击“getAuthorization”按钮。
1. 因此，在“setToken” — \>“resource id”文本框中将显示资源，在“setToken” — \>“token”文本框中将显示shortAuthorizationToken，表示authZ成功。
1. 现在，您可以单击“注销”按钮以删除令牌。
