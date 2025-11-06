---
title: 在资格预审中设置环境和测试
description: 在资格预审中设置环境和测试
exl-id: f822c0a1-045a-401f-a44f-742ed25bfcdc
source-git-commit: ca95bc45027410becf8987154c7c9f8bb8c2d5f8
workflow-type: tm+mt
source-wordcount: '476'
ht-degree: 0%

---

# 在资格预审中设置环境和测试{#setting-up-your-environment-and-testing-in-prequal}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

本技术说明旨在帮助我们的合作伙伴设置其环境，并开始测试在Adobe资格预审环境中部署的新内部版本。

由于存在两种生成风格： ***生产***&#x200B;和&#x200B;***暂存***，在本文档中，我们将重点介绍生产设置，并提及所有步骤对于暂存都是相同的，只有URL不同。

步骤1和2是在其中一台测试计算机上设置测试环境，步骤3是对基本流程的验证，步骤4和5介绍了一些测试准则。

>[!IMPORTANT]
>
> 每次要更改测试环境（从暂存切换到生产配置文件或其他方式）时执行步骤1和2非常重要


## 步骤1. 将传递域解析为IP {#resolving-pass-domain-to-an-ip}

* 要查找可用于欺骗的负载平衡器IP，请运行以下命令：

* Windows上的&#x200B;****

  ```cmd
  C:\>nslookup sp-prequal.auth.adobe.com
  ...
  Addresses:  52.13.71.11
              54.184.208.150
  ```

```Choose any IP from **addresses** section (e.g. `52.13.71.11)```

```cmd
C:\>nslookup entitlement-prequal.auth.adobe.com 
...
Addresses:  52.26.79.43
            54.190.212.171
```

```Choose any IP from **addresses** section (e.g. `54.190.212.171)```


* 在Linux/Mac上&#x200B;****

```sh
    $ dig sp-prequal.auth.adobe.com
    
    ;; ANSWER SECTION:
    ...
    ............ 60 IN A      52.13.71.11
    ............ 60 IN A      54.184.208.150
```

```Choose any IP from **A records (**e.g `52.13.71.11)```

```sh
    $ dig entitlement-prequal.auth.adobe.com
    
    ;; ANSWER SECTION:
    ...
    ............ 60 IN A      52.26.79.43
    ............ 60 IN A      54.190.212.171
```

```Choose any IP from **A records (**e.g `54.190.212.171)```

>[!NOTE]
>
>从应答中排除的域与此无关，并且可能因用户而异。

>[!IMPORTANT]
>
> 这些IP地址在未来可能会发生更改，并且对于不同地理区域的用户来说，它们可能会不同。


## 步骤2.  伪造要生产的预鉴定环境 {#spoofing-the-prequalification-environment}

* 编辑&#x200B;*c：\\windows\\System32\\drivers\\etc\\hosts*&#x200B;文件（在Windows中）或&#x200B;*/etc/hosts*&#x200B;文件(在Macintosh/Linux/Android上)并添加以下内容：

* 伪造生产配置文件
   * 52.13.71.11 sp.auth.adobe.com api.auth.adobe.com
   * 54.190.212.171 entitlement.auth.adobe.com

**在Android上欺骗：**&#x200B;为了欺骗Android，您必须使用Android模拟器。

* 一旦设置好欺骗，您只需将常规URL用于生产和暂存配置文件即可： (即`http://sp.auth-staging.adobe.com`和`http://entitlement.auth-staging.adobe.com`，您实际上将点击*新内部版本的&#x200B;*资格预审环境/生产*。


## 步骤3.  验证您是否指向正确的环境 {#Verify-you-are-pointing-to-the-right-environment}

**这是一个简单的步骤：**

* 加载[授权前置环境](https://entitlement-prequal.auth.adobe.com/environment.html)和[授权](https://entitlement.auth.adobe.com/environment.html)。 它们应返回相同的响应。


## 步骤4.  使用程序员的网站执行简单的身份验证/授权流程 {#peform-a-simple-auth-flow}

* 此步骤需要程序员的网站地址和一些有效的MVPD凭据（经过身份验证和授权的用户）。

## 步骤5.  使用程序员的网站执行场景测试 {#perform-scenario-testing-using-programmer-website}

* 完成环境设置并确保基本身份验证授权流程正常工作后，您可以继续测试更复杂的场景。


## 步骤6.  使用API测试站点执行测试 {#perform-testing-using-api-testing-site}

* 如果您想更深入地测试Adobe Pass身份验证，我们建议您使用[API测试站点](http://entitlement-prequal.auth.adobe.com/apitest/api.html)。

您可以在[找到有关API测试站点的更多详细信息。如何使用Adobe的API测试站点](/help/authentication/integration-guide-programmers/legacy/notes-technical/test-authn-authz-flows-using-adobes-api-test-site.md)测试身份验证和授权流。
