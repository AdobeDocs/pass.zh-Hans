---
title: 在资格预审中设置环境和测试
description: 在资格预审中设置环境和测试
exl-id: f822c0a1-045a-401f-a44f-742ed25bfcdc
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '477'
ht-degree: 0%

---

# 在资格预审中设置环境和测试{#setting-up-your-environment-and-testing-in-prequal}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此 API 需要 Adobe 的最新许可证。 不得未经授权使用。

本技术说明旨在帮助我们的合作伙伴设置其环境并开始测试在 Adobe 资格预审环境中部署的新版本。

由于有两种构建风格：生产和&#x200B;******&#x200B;暂存&#x200B;***，***&#x200B;因此在本文档中，我们将重点介绍生产设置，并提到暂存的所有步骤都是相同的，只是 URL 不同。

步骤1和2是在其中一台测试机器上设置测试环境，步骤3是对基本流程的验证，步骤4和5提供了一些测试指南。

>[!IMPORTANT]
>
> 每次要更改测试环境（从暂存配置文件切换到生产配置文件或相反）时，执行步骤 1 和 2 非常重要


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

* **在 Linux/Mac 上**

```sh
    $ dig sp-prequal.auth.adobe.com
    
    ;; ANSWER SECTION:
    ...
    ............ 60 IN A      52.13.71.11
    ............ 60 IN A      54.184.208.150
```

```Choose any IP from **A records (**e.g `52.13.71.11)```

>[!NOTE]
>
>从答案中排除的域，因为它们不相关，并且可能因用户而异。

>[!IMPORTANT]
>
> 这些 IP 地址将来可能会更改，对于不同地理区域的用户，它们可能并不相同。


## 第 2 步。  欺骗资格预审环境以进行生产 {#spoofing-the-prequalification-environment}

* *编辑 c：\\windows\\System32\\drivers\\etc\\hosts* 文件（在 Windows 中）或 */etc/hosts* 文件（在 Macintosh/Linux/Android 上），然后添加以下内容：

* 恶搞制作配置文件
   * 52.13.71.11 http://entitlement.auth.adobe.com、http://sp.auth.adobe.com http://api.auth.adobe.com

**在Android上欺骗：**&#x200B;为了欺骗Android，您必须使用Android模拟器。

* 一旦设置好欺骗，您只需将常规URL用于生产和暂存配置文件即可： (即`http://sp.auth-staging.adobe.com`和`http://entitlement.auth-staging.adobe.com`，您实际上将点击*新内部版本的&#x200B;*资格预审环境/生产*。


## 步骤3.  验证您是否指向正确的环境 {#Verify-you-are-pointing-to-the-right-environment}

**这是一个简单的步骤：**

* 加载[权利 环境和](https://entitlement-prequal.auth.adobe.com/environment.html)[权利](https://entitlement.auth.adobe.com/environment.html)。它们应返回相同的响应。


## 步骤 4.  使用程序员的网站执行简单的身份验证/授权流程 {#peform-a-simple-auth-flow}

* 此步骤需要程序员的网站地址和一些有效的 MVPD 凭据（经过身份验证和授权的用户）。

## 步骤 5.  使用程序员的网站执行场景测试 {#perform-scenario-testing-using-programmer-website}

* 完成环境设置并确保基本身份验证-授权流正常工作后，可以继续测试更复杂的方案。


## 步骤6.  使用API测试站点执行测试 {#perform-testing-using-api-testing-site}

* 如果您想更深入地测试Adobe Pass身份验证，我们建议您使用[API测试站点](http://entitlement-prequal.auth.adobe.com/apitest/api.html)。

您可以在[找到有关API测试站点的更多详细信息。如何使用Adobe的API测试站点](/help/authentication/test-authn-authz-flows-using-adobes-api-test-site.md)测试身份验证和授权流。
