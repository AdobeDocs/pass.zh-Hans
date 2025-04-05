---
title: Amazon SSO指南(REST API V2)
description: Amazon SSO指南(REST API V2)
exl-id: 63e4fa63-8ca3-40eb-b49a-84dd75c2ca1d
source-git-commit: 640ba7073f7f4639f980f17f1a59c4468bfebcf4
workflow-type: tm+mt
source-wordcount: '567'
ht-degree: 0%

---

# Amazon SSO指南(REST API V2) {#amazon-sso-cookbook-rest-api-v2}

>[!IMPORTANT]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

Adobe Pass身份验证REST API V2支持在FireOS上运行的客户端应用程序的最终用户的平台单点登录(SSO)。

此文档用作现有[REST API V2概述](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)的扩展，该视图提供了高级视图以及描述如何使用平台标识流](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md)实施[单点登录的文档。

## 使用平台标识流的Amazon单点登录 {#cookbook}

Adobe Pass身份验证可与Amazon协作以改善登录用户体验，并促进电视订阅者在TV Everywhere应用程序中进行单点登录(SSO)。

### 先决条件 {#prerequisites}

Before proceeding with the Amazon single sign-on using platform identity flows, ensure the following prerequisites are met.

#### Integrate Amazon SSO SDK {#integrate-amazon-sso-sdk}

The streaming application must integrate the [Amazon SSO SDK](https://tve.zendesk.com/hc/en-us/article_attachments/360064368131/ottSSOTokenLib_v1.jar) library for Single Sign-On (SSO) into its build.

* 将最新的Amazon SSO SDK库下载并复制到与应用程序目录平行的`/SSOEnabler`文件夹中。

* 更新manifest和Gradle文件以使用Amazon SSO SDK库。

  **Manifest:**

  ```JAVA
  <uses-library android:name="com.amazon.ottssotokenlib" android:required="false">
  ```

  **Gradle:**

  Under repositories:

  ```JAVA
  flatDir {
      dirs '../SSOEnabler'
  }
  ```

  Under dependencies:

  ```JAVA
  provided fileTree(include: ['ottSSOTokenStub.jar'], dir: '../SSOEnabler')
  ```

#### Use Amazon SSO SDK {#use-amazon-sso-sdk}

流应用程序必须使用Amazon SSO SDK来获取SSO令牌（平台身份）有效负载。

Amazon SSO SDK提供同步和异步API来获取SSO令牌（平台身份）有效负载。

流应用程序可以根据其体系结构选择两个选项之一。

##### 异步API

* 获取`SSOEnabler`实例并设置`SSOEnablerCallback`：

  ```JAVA
  SSOEnabler ssoEnabler = SSOEnabler.getInstance(context);
  
  SSOEnablerCallback ssoEnablerCallback = new SSOEnablerCallbackImpl();
  ssoEnabler.setSSOTokenCallback(ssoEnablerCallback);
  ```

  这可以在流应用程序的初始化期间完成。

  ```JAVA
  public static abstract class SSOEnablerCallback
  {
          public abstract void getSSOTokenSuccess(Bundle result);
          public abstract void getSSOTokenFailure(Bundle result);
  }
  ```

  SSO令牌成功响应包将包含：
   * 作为`string`的SSO令牌，带有密钥“SSOToken”。

  <br/>

  SSO令牌失败响应包将包含：
   * 带有键“ErrorCode”的`int`形式的错误代码。
   * 带有键“ErrorDescription”的`string`的错误描述。

  <br/>

* Get the SSO token:

  ```JAVA
  Bundle getSSOTokenAsync(Void);
  ```

  This API will provide the response via callback set during the initialisation.

##### 同步API

* Get the `SSOEnabler` instance:

  ```JAVA
  SSOEnabler ssoEnabler = SSOEnabler.getInstance(context);
  ```

* 获取SSO标记：

  ```JAVA
  Bundle getSSOTokenSync(Void);
  ```

  此API将阻止调用方线程并使用结果包做出响应。 Since this is a synchronous call, be sure to not use it in your main thread.

  ```JAVA
  void setSSOTokenTimeout(long);
  ```

  This API will set the timeout value for the synchronous call. The default timeout value is 1 minute.

#### Fallback for Amazon SSO {#fallback-amazon-sso}

The streaming application must handle fallback scenarios from the Amazon SSO flow to the regular authentication flow.

Ensure that the streaming application is handling:

* 缺少应在Amazon设备上运行的Amazon配套应用程序。
   * 流应用程序可能在运行时在以下类`com.amazon.ottssotokenlib.SSOEnabler`上遇到`ClassNotFoundException`。

* 缺少应由上述API返回的SSO令牌（平台身份）有效负载。
   * 流应用程序可以联系Amazon和Adobe代表进行调查。

### 工作流 {#workflow}

The Amazon SSO token (platform identity) payload needs to be present on all HTTP requests made against Adobe Pass Authentication REST API V2 endpoints:

```
/api/v2/*
```

Adobe Pass Authentication REST API V2 supports the following methods to receive the SSO token (platform identity) payload which is a device-scoped or platform-scoped identifier:

* As a header named: `Adobe-Subject-Token`

>[!IMPORTANT]
> 
> For more details about `Adobe-Subject-Token` header, refer to the [Adobe-Subject-Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) documentation.

#### 示例

**Sending as a header**

```HTTPS
GET /api/v2/{serviceProvider}/sessions HTTP/1.1 
Host: sp-preprod.auth.adobe.com

Adobe-Subject-Token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA
```

>[!IMPORTANT]
>
> In case the `Adobe-Subject-Token` header value is missing or invalid, then Adobe Pass Authentication will service the requests without taking Single Sign-On into account.
