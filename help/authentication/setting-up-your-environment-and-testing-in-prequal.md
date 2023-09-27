---
title: 在资格预审中设置环境和测试
description: 在资格预审中设置环境和测试
exl-id: f822c0a1-045a-401f-a44f-742ed25bfcdc
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '473'
ht-degree: 0%

---

# 在预 Qual 中设置您的环境和测试{#setting-up-your-environment-and-testing-in-prequal}

>[!NOTE]
>
>此页面上的内容仅用于信息目的。 使用此 API 需要 Adobe Systems 的当前许可证。 不允许未经授权的使用。

此技术说明的目的是帮助我们的合作伙伴设置其环境，并开始测试在 Adobe Systems 预限定环境上部署的新版本。

由于存在两种版本风格： ***生产*** 和 ***暂存*** ，在此文档中，我们将在生产设置中聚焦，并指出，在进行暂存时，只有 url 不同。

步骤1和2在测试计算机上设置测试环境，步骤3是对基本流量进行验证，而步骤 4 &amp; 5 则演示了一些测试准则。

>[!IMPORTANT]
>
> 每次要更改测试环境（从暂存切换到生产用户档案或其他方式）时，执行步骤1和2非常重要。


## 步骤1. 将传递域解析为IP {#resolving-pass-domain-to-an-ip}

* 要查找可用于欺骗的负载平衡器IP，请运行以下命令：

* **在Windows上**

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
>从答案中排除的域名不相关，并且可能会因用户到用户而异。

>[!IMPORTANT]
>
> 这些 IP 地址将来可能会发生更改，对于不同地理区域中的用户而言，它们可能不是相同的。


## 步骤2。  哄骗预限定环境以进行生产 {#spoofing-the-prequalification-environment}

* *编辑 c:\\windows\\System32\\drivers\\etc\\hosts* 文件（在 windows 中）或 */etc/hosts* 文件（在 Macintosh/Linux/Android 上），并添加以下内容：

* 用户档案的欺骗性生产
   * 52.13.71.11 http://entitlement.auth.adobe.com，http://sp.auth.adobe.com，http://api.auth.adobe.com

**在Android上欺骗：** 要欺骗Android，您必须使用Android模拟器。

* 设置好欺骗后，您只需将常规URL用于生产和暂存配置文件即可： `http://sp.auth-staging.adobe.com` 和 `http://entitlement.auth-staging.adobe.com` 你真的会撞到 *资格预审环境/生产* 新版本编号。


## 步骤3.  验证您是否指向正确的环境 {#Verify-you-are-pointing-to-the-right-environment}

**这是一个简单的步骤：**

* 加载 [ 权利 prequal 环境 ](https://entitlement-prequal.auth.adobe.com/environment.html) 和 [ 权利 ](https://entitlement.auth.adobe.com/environment.html) 。 它们应返回相同的响应。


## 步骤4。  使用程序员的网站执行简单身份验证/授权流程 {#peform-a-simple-auth-flow}

* 此步骤需要程序员的网站地址和一些有效的 MVPD 凭据（经过身份验证和授权的用户）。

## 步骤5。  使用程序员的网站执行方案测试 {#perform-scenario-testing-using-programmer-website}

* 完成环境设置并确保基本身份验证授权流程正常工作后，您可以继续测试更复杂的方案。


## 步骤6.  使用API测试站点执行测试 {#perform-testing-using-api-testing-site}

* 如果您想更深入地测试Adobe Pass身份验证，我们建议您使用 [API测试站点](http://entitlement-prequal.auth.adobe.com/apitest/api.html).

有关API测试网站的更多详细信息，请访问 [如何使用Adobe API测试站点测试身份验证和授权流](/help/authentication/test-authn-authz-flows-using-adobes-api-test-site.md).
