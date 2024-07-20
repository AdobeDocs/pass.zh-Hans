---
title: 避免在/authenticate请求中使用'&'reg_code
description: 避免在/authenticate请求中使用'&'reg_code
exl-id: c0ecb6f9-2167-498c-8a2d-a692425b31c5
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 0%

---

# 避免在/authenticate请求中使用&#39;&amp;&#39;reg_code {#clientless-avoid-using-reg_code-in-authenticate-request}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

</br>



## 问题

IE 9浏览器将“\&amp;reg”解释为特殊命令并将其转换为®。

## 说明

如果`/authenticate`请求由以下内容组成……


```
    <FQDN>authenticate? requestor_id=someRequestor&reg_code=EKAFMFI&domain_name=someRequestor.com&noflash=true&mso_id=someMvpd&redirect_url=someRequestor.redirect.url.html
```


...它将由IE浏览器进行如下解释，并将以以下格式发送到Adobe：


```
    <FQDN>authenticate?requestor_id=someRequestor&reg;_code=EKAFMFI&domain_name=someRequestor.com&noflash=true&mso_id=someMvpd&redirect_url=someRequestor.redirect.url.html
```


请求者\_id将解释为univision®\_code=EKAFMFI，因为没有“&amp;”，并且Adobe将找不到要与令牌关联的`regCode`参数。  有可能根本不会创建AuthN令牌，在这种情况下，`/checkauthn`调用将无法找到任何令牌。



## 解决方案

以下选项之一应该可以解决此问题：

1. 避免在其他查询字符串参数之间使用`&reg_code`参数。  请改为将其移动到请求URL中的第一个查询字符串参数，使请求URL如下所示：


       &lt;FQDN>身份验证？reg_code =EKAFMFI&amp;requestor_id=someRequestor&amp;domain_name=someRequestor.com&amp;noflash=true&amp;mso_id=someMvpd&amp;redirect_url=someRequestor.redirect.url.html
   

   这样，`&reg`参数将不会被错误解释。

1. 使用`&amp;reg_code`标准化`&reg_code`。

1. 如果AuthN令牌创建失败，Adobe可能会引入新功能，以便在响应身份验证调用时将错误代码发送回第2屏幕。
