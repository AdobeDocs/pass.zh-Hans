---
title: Amazon SSO指南(REST API V1)
description: Amazon SSO指南(REST API V1)
exl-id: 4c65eae7-81c1-4926-9202-a36fd13af6ec
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '591'
ht-degree: 0%

---

# （旧版）Amazon SSO指南(REST API V1) {#amazon-sso-cookbook-rest-api-v1}

>[!IMPORTANT]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

Adobe Pass身份验证REST API V1支持在FireOS上运行的客户端应用程序的最终用户的平台单点登录(SSO)。

此文档用作现有[REST API V1概述](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-overview.md)的扩展，该视图提供了高级视图。

## 使用平台标识流的Amazon单点登录 {#cookbook}

### 先决条件 {#prerequisites}

在继续使用平台标识流进行Amazon单点登录之前，请确保满足以下先决条件。

#### 集成Amazon SSO SDK {#integrate-amazon-sso-sdk}

流应用程序必须将用于单点登录(SSO)的[Amazon SSO SDK](https://tve.zendesk.com/hc/en-us/article_attachments/360064368131/ottSSOTokenLib_v1.jar)库集成到其内部版本中。

* 将最新的Amazon SSO SDK库下载并复制到与应用程序目录平行的`/SSOEnabler`文件夹中。

* 更新manifest和Gradle文件以使用Amazon SSO SDK库。

  **清单：**

  ```JAVA
  <uses-library android:name="com.amazon.ottssotokenlib" android:required="false">
  ```

  **Gradle：**

  在存储库下：

  ```JAVA
  flatDir {
      dirs '../SSOEnabler'
  }
  ```

  在依赖项下：

  ```JAVA
  provided fileTree(include: ['ottSSOTokenStub.jar'], dir: '../SSOEnabler')
  ```

#### 使用Amazon SSO SDK {#use-amazon-sso-sdk}

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

* 获取SSO标记：

  ```JAVA
  Bundle getSSOTokenAsync(Void);
  ```

  此API将在初始化期间通过回调集提供响应。

##### 同步API

* 获取`SSOEnabler`实例：

  ```JAVA
  SSOEnabler ssoEnabler = SSOEnabler.getInstance(context);
  ```

* 获取SSO标记：

  ```JAVA
  Bundle getSSOTokenSync(Void);
  ```

  此API将阻止调用方线程并使用结果包做出响应。 由于这是同步调用，请确保不要在主线程中使用它。

  ```JAVA
  void setSSOTokenTimeout(long);
  ```

  此API将设置同步调用的超时值。 默认超时值为1分钟。

#### Amazon SSO的回退 {#fallback-amazon-sso}

流应用程序必须处理从Amazon SSO流到常规身份验证流的回退方案。

确保流应用程序正在处理：

* 缺少应在Amazon设备上运行的Amazon配套应用程序。
   * 流应用程序可能在运行时在以下类`com.amazon.ottssotokenlib.SSOEnabler`上遇到`ClassNotFoundException`。

* 缺少应由上述API返回的SSO令牌（平台身份）有效负载。
   * 流媒体应用程序可以联系Amazon和Adobe代表进行调查。

### 工作流 {#workflow}

需要在Amazon身份验证端点发起的所有HTTP请求上都存在Adobe Pass SSO令牌（平台身份）有效负载：

```
/adobe-services/*
/reggie/*
/api/*
```

>[!IMPORTANT]
> 
> 流应用程序可能会在`/authenticate`调用中跳过发送Amazon SSO令牌（平台身份）有效负载，因为它是在`/regcode`调用中提供的。

Adobe Pass身份验证支持以下方法接收SSO令牌（平台身份）有效负载，该有效负载是设备范围或平台范围的标识符：

* 作为名为`Adobe-Subject-Token`的标头
* 作为查询参数，名为： `ast`
* 作为名为`ast`的post参数

>[!IMPORTANT]
>
> 如果作为查询参数发送，则整个URL可能会变得非常长并被拒绝。
>
> 如果作为查询/发布参数发送，则在生成请求签名时必须包含该参数。

#### 示例

**作为标头发送**

```HTTPS
GET /api/v1/config/{requestorId} HTTP/1.1 
Host: sp-preprod.auth.adobe.com

Adobe-Subject-Token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA
```

**作为查询参数发送**

```HTTPS
GET /api/v1/config/{requestorId}?ast=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA HTTP/1.1
Host: sp.auth.adobe.com
```

**作为post参数发送**

```HTTPS
POST /api/v1/config/{requestorId}?ast=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.Jl\_BFhN\_h\_NCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA HTTP/1.1
Host: sp.auth.adobe.com 
Content-Type: multipart/form-data;
```

>[!IMPORTANT]
>
> 如果`Adobe-Subject-Token`标头或`ast`参数值缺失或无效，则Adobe Pass身份验证将无需考虑单点登录即可为请求提供服务。
