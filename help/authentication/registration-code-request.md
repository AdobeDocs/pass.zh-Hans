---
title: 注册页面
description: 注册页面
exl-id: 581b8e2e-7420-4511-88b9-f2cd43a41e10
source-git-commit: 3c44f1dfbbb5b9ec31f13e267dc691e14dd2ddfa
workflow-type: tm+mt
source-wordcount: '493'
ht-degree: 0%

---

# 注册页面 {#registration-page}

## REST API端点 {#clientless-endpoints}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

>[!NOTE]
>
> REST API实现受[限制机制](/help/authentication/throttling-mechanism.md)限制

&lt;REGGIE_FQDN>：

* 生产 — [api.auth.adobe.com](http://api.auth.adobe.com/)
* 暂存 — [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>：

* 生产 — [api.auth.adobe.com](http://api.auth.adobe.com/)
* 暂存 — [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

<br>

## 描述 {#create-reg-code-svc}

返回随机生成的注册代码和登录页面URI。

| 端点 | <br>调用者 | 输入   <br>参数 | HTTP <br>方法 | 响应 | HTTP <br>响应 |
| --- | --- | --- | --- | --- | --- |
| &lt;REGGIE_FQDN>/reggie/v1/{requestor}/regcode<br>例如：<br>REGGIE_FQDN/reggie/v1/sampleRequestorId/regcode | 流式处理应用程序<br>或<br>程序员服务 | 1.请求者<br>    （路径组件）<br>2。  deviceId (Hashed)   <br>    （必需）<br>3。  device_info/X-Device-Info （必需）<br>4。  mvpd （可选）<br>5。  ttl （可选）<br> | POST | 包含注册码和信息的XML或JSON，如果失败，则显示错误详细信息。 请参阅下面的示例。 | 201 |

{style="table-layout:auto"}

| 输入参数 | 类型 | 描述 |
| --- |------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 授权 | 标头<br>值：持有者&lt;access_token> | DCR访问令牌 |
| Accept | 标头<br>值： application/json | 指示客户端应该能够理解的内容类型 |
| 请求者 | 查询参数 | 此操作有效的程序员requestorId。 |
| deviceId | 查询参数 | 设备ID字节。 |
| 设备信息/<br>X — 设备信息 | device_info：正文<br> X-Device-Info：标头 | 流设备信息。<br>**注意**：可以将此device_info作为URL参数传递，但由于此参数的潜在大小以及GETURL的长度限制，它应作为X-Device-Info传递到http标头。 <br>在[传递设备和连接信息](/help/authentication/passing-client-information-device-connection-and-application.md)中查看完整的详细信息。 |
| mvpd | 查询参数 | 此操作有效的MVPD ID。 |
| ttl | 查询参数 | 此正则码应在秒内保持多长时间。<br>**注意**： ttl允许的最大值为36000秒（10小时）。 较高的值会导致400 HTTP响应（错误请求）。 如果`ttl`留空，则Adobe Pass身份验证将默认值设置为30分钟。 |
| _deviceType_ | 查询参数 | 已弃用，不应使用。 |
| _设备用户_ | 查询参数 | 已弃用，不应使用。 |
| _appId_ | 查询参数 | 已弃用，不应使用。 |

{style="table-layout:auto"}

>[!CAUTION]
>
>**流设备IP地址**
><br>
>对于客户端到服务器实施，流设备IP地址将随此调用隐式发送。  对于服务器到服务器实施，如果将&#x200B;**regcode**&#x200B;调用设置为程序员服务而不是流设备，则需要以下标头来传递流设备IP地址：
>
>
>```
>X-Forwarded-For : <streaming_device_ip> 
>```
>
>其中`<streaming\_device\_ip>`是流设备公共IP地址。
><br><br>
>示例：<br>
>
>```
>POST /reggie/v1/{req_id}/regcode HTTP/1.1<br>X-Forwarded-For:203.45.101.20
>```
>
<br>

### 响应JSON


#### 注册码JSON示例

```JSON
{
  "id": "ef5a79e8-7c8a-41d6-a45a-e378c6c7c8b5",
  "code": "IYQD5JQ",
  "requestor": "sampleRequestorId",
  "mvpd": "sampleMvpdId",
  "generated": 1704963921144,
  "expires": 1704965721144,
  "info": {
    "deviceId": "c28tZGV2aWQtMDAz",
    "deviceInfo": "eyJ0eXBlIjoiU2V0VG9wQm94IiwibW9kZWwiOiJBRlRNTSIsInZlcnNpb24iOnsibWFqb3IiOjAsIm1pbm9yIjowLCJwYXRjaCI6MCwicHJvZmlsZSI6IiJ9LCJoYXJkd2FyZSI6eyJuYW1lIjoiQUZUTU0iLCJ2ZW5kb3IiOiJVbmtub3duIiwidmVyc2lvbiI6eyJtYWpvciI6MCwibWlub3IiOjAsInBhdGNoIjowLCJwcm9maWxlIjoiIn0sIm1hbnVmYWN0dXJlciI6IlJva3UifSwib3BlcmF0aW5nU3lzdGVtIjp7Im5hbWUiOiJBbmRyb2lkIiwiZmFtaWx5IjoiQW5kcm9pZCIsInZlbmRvciI6IkFtYXpvbiIsInZlcnNpb24iOnsibWFqb3IiOjcsIm1pbm9yIjoxLCJwYXRjaCI6MiwicHJvZmlsZSI6IiJ9fSwiYnJvd3NlciI6eyJuYW1lIjoiQ2hyb21lIiwidmVuZG9yIjoiR29vZ2xlIiwidmVyc2lvbiI6eyJtYWpvciI6MTEyLCJtaW5vciI6MCwicGF0Y2giOjU2MTUsInByb2ZpbGUiOiIifSwidXNlckFnZW50IjoiTW96aWxsYS81LjAgKExpbnV4OyBBbmRyb2lkIDcuMS4yOyBBRlRNTSBCdWlsZC9OUzYyOTc7IHd2KSBBcHBsZVdlYktpdC81MzcuMzYgKEtIVE1MLCBsaWtlIEdlY2tvKSBWZXJzaW9uLzQuMCBDaHJvbWUvMTEyLjAuNTYxNS4xOTcgTW9iaWxlIFNhZmFyaS81MzcuMzYgQWRvYmVQYXNzTmF0aXZlRmlyZVRWLzMuMC44Iiwib3JpZ2luYWxVc2VyQWdlbnQiOiJNb3ppbGxhLzUuMCAoTGludXg7IEFuZHJvaWQgNy4xLjI7IEFGVE1NIEJ1aWxkL05TNjI5Nzsgd3YpIEFwcGxlV2ViS2l0LzUzNy4zNiAoS0hUTUwsIGxpa2UgR2Vja28pIFZlcnNpb24vNC4wIENocm9tZS8xMTIuMC41NjE1LjE5NyBNb2JpbGUgU2FmYXJpLzUzNy4zNiBBZG9iZVBhc3NOYXRpdmVGaXJlVFYvMy4wLjgifSwiZGlzcGxheSI6eyJ3aWR0aCI6MCwiaGVpZ2h0IjowLCJwcGkiOjAsIm5hbWUiOiJESVNQTEFZIiwidmVuZG9yIjpudWxsLCJ2ZXJzaW9uIjpudWxsLCJkaWFnb25hbFNpemUiOm51bGx9LCJhcHBsaWNhdGlvbklkIjpudWxsLCJjb25uZWN0aW9uIjp7ImlwQWRkcmVzcyI6IjE5My4xMDUuMTQwLjEzMSIsInBvcnQiOiI5OTM0Iiwic2VjdXJlIjpmYWxzZSwidHlwZSI6bnVsbH19",
    "userAgent": "Mozilla/5.0 (Linux; Android 7.1.2; AFTMM Build/NS6297; wv) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/112.0.5615.197 Mobile Safari/537.36 AdobePassNativeFireTV/3.0.8",
    "originalUserAgent": "Mozilla/5.0 (Linux; Android 7.1.2; AFTMM Build/NS6297; wv) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/112.0.5615.197 Mobile Safari/537.36 AdobePassNativeFireTV/3.0.8",
    "authorizationType": "OAUTH2",
    "sourceApplicationInformation": {
      "id": "14138364-application-id",
      "name": "application name",
      "version": "1.0.0"
    }
  }
}
```

<br>

| 元素名称 | 描述 |
|-----------------------------------|------------------------------------------------------------------------------------------------------------------|
| id | 注册代码服务生成的UUID |
| 代码 | 注册代码服务生成的注册代码 |
| 请求者 | 请求者ID |
| mvpd | Mvpd ID |
| 已生成 | 注册码创建时间戳（以自1970年1月1日GMT以来的毫秒为单位） |
| 过期 | 注册代码过期的时间戳（以自1970年1月1日GMT以来的毫秒为单位） |
| deviceId | Base64唯一设备ID |
| info：deviceId | Base64设备类型 |
| info：deviceInfo | Base64规范化的设备信息基于从User-Agent、X-Device-Info或device_info接收的信息 |
| info：userAgent | 应用程序发送的用户代理 |
| info：originalUserAgent | 应用程序发送的用户代理 |
| info：authorizationType | 使用DCR的调用的OAUTH2 |
| info：sourceApplicationInformation | DCR中配置的应用程序信息 |

{style="table-layout:auto"}


<br>

### 错误消息JSON响应示例(#error-sample-response)

```JSON
{
  "status": 400,
  "message": "Required '<>' is not present"
}
```

