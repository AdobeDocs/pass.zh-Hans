---
title: 返回注册记录
description: 返回注册记录
exl-id: 7b9e63a2-59b6-4123-a19b-ee1f021219ea
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '252'
ht-degree: 2%

---

# 返回注册记录 {#return-registration-record}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

>[!NOTE]
>
> REST API实现受[限制机制](/help/authentication/integration-guide-programmers/throttling-mechanism.md)限制

## REST API端点 {#clientless-endpoints}

`<REGGIE_FQDN>`：

* 生产 — [api.auth.adobe.com](http://api.auth.adobe.com/)
* 暂存 — [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

`<SP_FQDN>`：

* 生产 — [api.auth.adobe.com](http://api.auth.adobe.com/)
* 暂存 — [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)




## 描述 {#description}

返回包含注册码UUID、注册码和经过哈希处理的设备ID的注册码记录。






| 端点 | </br>调用者 | 输入   </br>参数 | HTTP </br>方法 | 响应 | HTTP </br>响应 |
| --- | --- | --- | --- | --- | --- |
| `<REGGIE_FQDN>`；/reggie/v1/`{requestorId}`/regcode/`{registrationCode}`<p>例如：<p>`<REGGIE_FQDN>`/reggie/v1/sampleRequestorId/regcode/TJCFK？format=xml | 流式处理应用程序</br></br>或</br></br>程序员服务 | 1.请求者</br>    （路径组件）</br>2。  注册码</br>    （路径组件） | GET | 包含注册代码和信息的XML或JSON。 请参阅下面的架构和示例。 | 200 |

{style="table-layout:auto"}




| 输入参数 | 描述 |
| --- | --- |
| 请求者 | 此操作有效的程序员requestorId。 |
| 注册码 | 将在流设备上显示的注册代码值（将输入到身份验证流程中）。 |




## 响应XML架构 {#response-xml-schema}

### 注册码XSD

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

| 元素名称 | 描述 |
| --- | --- |
| id | 注册代码服务生成的UUID |
| 代码 | 注册代码服务生成的注册代码 |
| 请求者 | 请求者ID |
| mvpd | MVPD ID |
| 已生成 | 注册码创建时间戳（以自1970年1月1日GMT以来的毫秒为单位） |
| 过期 | 注册代码过期的时间戳（以自1970年1月1日GMT以来的毫秒为单位） |
| deviceId | 唯一设备ID（或XSTS令牌） |
| 设备类型 | 设备类型 |
| 设备用户 | 用户已登录到设备 |
| appId | 应用程序ID |
| appVersion | 应用程序版本 |
| 注册URL | 要向最终用户显示的登录Web应用程序的URL |

{style="table-layout:auto"}

### 示例响应 {#sample-response}

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
