---
title: 按第二屏Web应用程序检索预授权资源列表
description: 按第二屏Web应用程序检索预授权资源列表
exl-id: 78eeaf24-4cc1-4523-8298-999c9effdb7a
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '254'
ht-degree: 0%

---

# 按第二屏Web应用程序检索预授权资源列表 {#retrieve-list-of-preauthorized-resources-by-second-screen-web-app}

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

## 描述 {#description}

请求Adobe Pass身份验证以获取预授权资源的列表。

有两组API：一组用于流应用程序或程序员服务，另一组用于第二屏幕Web应用程序。 本页介绍身份验证应用程序的API。


| 端点 | </br>调用者 | 输入   </br>参数 | HTTP </br>方法 | 响应 | HTTP </br>响应 |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/preauthorize/{注册码} | AuthN模块 | 1.注册码</br>    （路径组件）</br>2。  请求者（必需）</br>3。  资源列表（必需） | GET | 包含各个预授权决策或错误详细信息的XML或JSON。 请参阅下面的示例。 | 200 — 成功</br></br>400 — 错误请求</br></br>401 — 未授权</br></br>405 — 不允许的方法</br></br>412 — 前提条件失败</br></br>500 — 内部服务器错误 |



| 输入参数 | 描述 |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 注册码 | 用户在身份验证流程开始时提供的注册代码值。 |
| 请求者 | 此操作有效的程序员requestorId。 |
| 资源列表 | 一个字符串，其中包含以逗号分隔的resourceId列表，该resourceId列表标识用户可能可以访问并被MVPD授权端点识别的内容。 |


### 示例响应 {#sample-response}

**XML：**

```XML
HTTP/1.1 200 OK
Adobe-Request-Id : 7af28ec2-a068-45c2-8009-f5443049baf4`
Adobe-Response-Confidence : full
Content-Type: application/xml; charset=utf-8

<resources>
    <resource>
        <id>TestStream1</id>
        <authorized>true</authorized>
    </resource>
    <resource>
        <id>TestStream2</id>
        <authorized>false</authorized>  
        <error>
            <status>403</status>
            <code>authorization_denied_by_mvpd</code>
            <message>User not authorized</message>
            <details>Your subscription package does not include the "TestStream3" channel.</details>
            <helpUrl>https://experienceleague-review.corp.adobe.com/docs/primetime/authentication/auth-features/error-reportn/enhanced-error-codes.html#error-codes</helpUrl>
            <trace>0453f8c8-167a-4429-8784-cd32cfeaee58</trace>
            <action>none</action>
        </error>
    <resource>
</resources>
```

**JSON：**

```JSON
HTTP/1.1 200 OK
Adobe-Request-Id : 7af28ec2-a068-45c2-8009-f5443049baf4
Adobe-Response-Confidence : full
Content-Type: application/json; charset=utf-8
 
{
   "resources" : [
        {
            "id" : "TestStream1",
            "authorized" : true
        },
        {
            "id" : "TestStream3",
            "authorized" : false,
            "error" : {
               "status" : 403,
               "code" : "authorization_denied_by_mvpd",
               "message" : "User not authorized",
               "details" : "Your subscription package does not include the "TestStream3" channel.",
               "helpUrl" : "https://experienceleague-review.corp.adobe.com/docs/primetime/authentication/auth-features/error-reportn/enhanced-error-codes.html#error-codes",
               "trace" : "0453f8c8-167a-4429-8784-cd32cfeaee58",
               "action" : "none"
            }
        } 
    ]
}
```
