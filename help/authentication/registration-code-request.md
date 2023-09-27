---
title: 注册页面
description: 注册页面
exl-id: 581b8e2e-7420-4511-88b9-f2cd43a41e10
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '487'
ht-degree: 0%

---

# 注册页面 {#registration-page}

## REST API端点 {#clientless-endpoints}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

&lt;reggie_fqdn>：

* 生产 —  [api.auth.adobe.com](http://api.auth.adobe.com/)
* 暂存 —  [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;sp_fqdn>：

* 生产 —  [api.auth.adobe.com](http://api.auth.adobe.com/)
* 暂存 —  [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## 描述 {#create-reg-code-svc}

返回随机生成的注册代码和登录页面URI。

| 端点 | 已调用  </br>按 | 输入   </br>参数 | HTTP  </br>方法 | 响应 | HTTP  </br>响应 |
| --- | --- | --- | --- | --- | --- |
| &lt;reggie_fqdn>/reggie/v1/{requestor}/regcode</br>例如：</br>REGGIE_FQDN/reggie/v1/sampleRequestorId/regcode | 流应用程序</br>或</br>程序员服务 | 1.请求人  </br>    （路径组件）</br>2.  deviceId (Hashed)   </br>    （必需）</br>3.  device_info/X-Device-Info（必需）</br>4.  mvpd（可选）</br>5.  ttl（可选）</br>6.  _设备类型_</br> 7.  _设备用户_ （已弃用）</br>8.  _appId_ （已弃用） | POST | 包含注册码和信息的XML或JSON，如果失败，则显示错误详细信息。 请参阅下面的架构和示例。 | 201 |

{style="table-layout:auto"}

| 输入参数 | 描述 |
| --- | --- |
| 请求者 | 此操作有效的程序员requestorId。 |
| deviceId | 设备ID字节。 |
| device_info/</br>X-Device-Info | 流设备信息。</br>**注意**：可以将此device_info作为URL参数进行传递，但由于此参数的潜在大小以及GETURL长度的限制，它应当作为X-Device-Info在http标头中传递。 </br>欲知详情，请参阅 [传递设备和连接信息](/help/authentication/passing-client-information-device-connection-and-application.md). |
| mvpd | 此操作有效的MVPD ID。 |
| ttl | 此正则码应在秒内保持多长时间。</br>**注意**：允许用于ttl的最大值为36000秒（10小时）。 较高的值会导致400 HTTP响应（错误请求）。 如果 `ttl` 留空，则Adobe Pass身份验证将默认值为30分钟。 |
| _设备类型_ | 设备类型（例如Roku、PC）。</br>如果此参数设置正确，则ESM提供的量度 [按设备类型细分](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) 使用无客户端时，以便可以执行不同类型的分析，例如Roku、AppleTV和Xbox。</br>看， [在通过的量度中使用无客户端设备类型参数的好处&#x200B;](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br>**注意**：device_info将替换此参数。 |
| _设备用户_ | 设备用户标识符。 |
| _appId_ | 应用程序id/名称。 </br>**注意**：device_info将替换此参数。 |

{style="table-layout:auto"}


>[!CAUTION]
>
>**流设备IP地址**
></br>
>对于客户端到服务器实施，流设备IP地址将随此调用隐式发送。  对于服务器到服务器实施，其中 **regcode** 调用是程序员服务而不是流设备，以下标头需要传递流设备IP地址：
>
>
>```
>X-Forwarded-For : <streaming_device_ip> 
>```
>
>位置 `<streaming\_device\_ip>` 是流设备公共IP地址。
></br></br>
>示例：</br>
>
>```
>POST /reggie/v1/{req_id}/regcode HTTP/1.1</br>X-Forwarded-For:203.45.101.20
>```
>
</br>

### 响应XML架构 {#xml-schema}


#### 注册码XSD {#registration-code-xsd}

```XML
    <?xml version="1.0" encoding="UTF-8"?>
    <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="model.mvc.reggie.pass.adobe.com"
            targetNamespace="model.mvc.reggie.pass.adobe.com"
            attributeFormDefault="unqualified"
            elementFormDefault="unqualified">
        <xs:element name="regcode">
            <xs:complexType>
                <xs:all>
                    <xs:element name="id" type="xs:string" />
                    <xs:element name="code" type="xs:string" />
                    <xs:element name="requestor" type="xs:string" minOccurs="1" maxOccurs="1"/>
                    <xs:element name="mvpd" type="xs:string" minOccurs="1" maxOccurs="1"/
                    <xs:element name="generated" type="xs:long" />
                    <xs:element name="expires" type="xs:long" />
                    <xs:element name="info" type="infoType" maxOccurs="1"/>
                </xs:all>
            </xs:complexType>
        </xs:element>
        <xs:complexType name="infoType">
            <xs:all>
                <xs:element name="deviceId" type="xs:base64Binary" minOccurs="1" maxOccurs="1"/>
                <xs:element name="deviceType" type="xs:string" minOccurs="0" maxOccurs="1"/>
                <xs:element name="deviceUser" type="xs:string" minOccurs="0" maxOccurs="1"/>
                <xs:element name="appId" type="xs:string" minOccurs="0" maxOccurs="1"/>
                <xs:element name="appVersion" type="xs:string" minOccurs="0" maxOccurs="1"/>
                <xs:element name="registrationURL" type="xs:anyURI" minOccurs="0" maxOccurs="1"/>
            </xs:all>
        </xs:complexType>
    </xs:schema>
```

</br>

| 元素名称 | 描述 |
| --------------- | ------------------------------------------------------------------------------------ |
| id | 注册代码服务生成的UUID |
| 代码 | 注册代码服务生成的注册代码 |
| 请求者 | 请求者ID |
| mvpd | Mvpd ID |
| 已生成 | 注册码创建时间戳（以自1970年1月1日GMT以来的毫秒为单位） |
| 过期 | 注册代码过期的时间戳（以自1970年1月1日GMT以来的毫秒为单位） |
| deviceId | 唯一设备ID（或XSTS令牌） |
| 设备类型 | 设备类型 |
| 设备用户 | 用户已登录到设备 |
| appId | 应用程序ID |
| appVersion | 应用程序版本 |
| 注册URL | 要向最终用户显示的登录Web应用程序的URL |

{style="table-layout:auto"}


</br>

### 错误消息XSD  {#error-message}

```XML
    <?xml version="1.0" encoding="UTF-8"?>
    <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="rest.pass.adobe.com"
               targetNamespace="rest.pass.adobe.com"
               attributeFormDefault="unqualified"
               elementFormDefault="unqualified">
        <xs:element name="error">
            <xs:complexType>
                <xs:all>
                    <xs:element name="status" type="xs:int" minOccurs="1" maxOccurs="1"/>
                    <xs:element name="message" type="xs:string" minOccurs="1" maxOccurs="1"/>
                    <xs:element name="details" type="xs:string" minOccurs="0" maxOccurs="1"/>
                </xs:all>
            </xs:complexType>
        </xs:element>
    </xs:schema>
```


### 示例响应 {#sample-response}

**XML：**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <ns2:regcode xmlns:ns2="model.mvc.reggie.pass.adobe.com">
        <id>678f9fea-a1cafec8-1ff0-4a26-8564-f6cd020acf13</id>
        <code>TJJCFK</code>
        <requestor>sampleRequestorId</requestor>
        <mvpd>sampleMvpdId</mvpd>
        <generated>1348039846647</generated>
        <expires>1348043446647</expires>
        <info>
            <deviceId>dGhpc0lkQUR1bW15RGV2aWNlSWQ=</deviceId>
            <deviceType>xbox</deviceType>
            <deviceUser>JD</deviceUser>
            <appId>2345</appId>
            <appVersion>2.0</appVersion>
            <registrationURL>http://loginwebapp.com</registrationURL>
        </info>
    </ns2:regcode>
```

**JSON：**

```JSON
    {
        "id": "678f9fea-9d364b29-246c-488f-b97e-298566d1b9e0",
        "code": "D4BDU2W",
        "requestor": "sampleRequestorId",
        "mvpd": "sampleMvpdId",
        "generated": 1348039555877,
        "expires": 1348043155877,
        "info": {
            "deviceId": "dGhpc0l.kQUR1bW15RGV2.aWNlSWQ=",
            "deviceType": "xboxOne",
            "deviceUser": "JD",
            "appId": "2345",
            "appVersion": "2.0",
            "registrationURL": "http://loginwebapp.com"
        }
    }
```
