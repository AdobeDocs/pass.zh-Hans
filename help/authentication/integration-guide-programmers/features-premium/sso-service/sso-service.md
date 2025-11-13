---
title: Adobe单点登录服务
description: 了解Adobe Pass SSO服务，该服务可跨多个设备和应用程序实现无缝身份验证。
exl-id: a1ff85d4-f7d2-4dea-b82f-d29730d9012f
source-git-commit: 151c64276377be5ef21bca4c0d3eaa04ac3da495
workflow-type: tm+mt
source-wordcount: '3615'
ht-degree: 2%

---


# Adobe单点登录服务 {#sso-service}

本文档介绍了Adobe单点登录服务的用例、端点和API。

**当前修订版 — 1.0.0**

## 范围 {#scope}

Adobe Pass SSO服务支持跨多个设备和应用程序的无缝身份验证，在保持安全性和合规性标准的同时提供统一的用户体验。 此服务可满足当今多设备流式处理环境中对跨平台身份验证日益增长的需求。

## 概述 {#overview}

### 内容 {#what-it-is}

全面的单点登录解决方案，允许用户只进行一次身份验证，并以知情的方式管理向其他设备的身份验证传输

### 身份验证的当前挑战 {#current-challenges}

* 用户必须使用他订阅的每个流服务进行身份验证
* 用户必须在每个设备或应用程序上单独进行身份验证
* 由于在某些平台上难以在身份验证流程中输入密码，这可能会导致丢弃率增加

### Adobe Pass SSO服务的机会 {#opportunity}

* 越来越多的流媒体服务需要自己的身份验证
* 对无缝跨设备体验的需求日益增长
* 每户流设备数量不断增加
* 每户需要统一身份验证

### 业务优势 {#business-benefits}

#### 内容提供商 {#content-providers}

* **用户参与度提高** — 无缝体验会导致会话时间延长
* **摩擦减少** — 较低的身份验证障碍会增加内容消耗
* **改进的保留率** — 更好的用户体验减少了流失
* **成本降低** — 与身份验证问题相关的支持票证减少

#### 最终用户 {#end-users}

* **无缝体验** — 身份验证一次，随时随地访问
* **节省时间** — 没有重复的登录过程
* **设备灵活性** — 在设备之间切换而不中断
* **一致的体验** — 跨所有平台的统一身份验证

#### IdP（ MVPD 、电信公司等） {#idps}

* 可以使用经过身份验证的SSO向MVPD通知其他设备
* **用户满意度提高** — 身份验证体验提高
* **支持负载减少** — 与身份验证相关的支持调用减少
* **竞争优势** — 比竞争对手的用户体验更好

## 用例 {#use-cases}

### 标识映射 {#identity-mapping}

该服务构建了在同一应用程序中链接D2C和TVE帐户的功能。

更多流媒体服务正在构建捆绑包以出售给第三方(MVPD/虚拟MVPD/Telco等)。 用户最终可能必须在同一应用程序中处理多个帐户。 要创建无缝身份验证体验，服务需要桥接这些帐户以简化登录体验。

用户将使用其D2C帐户进行身份验证，然后使用其他帐户(例如 MVPD)。 在我们的服务中，这些帐户将被关联，这意味着以后，不同设备上的后续身份验证只能通过D2C帐户进行。

### 跨设备SSO {#cross-device-sso}

在连接电视的设备上进行的身份验证可能比在手机上更麻烦。 良好的用户体验应该是在手机上进行身份验证，然后将该身份验证传递到智能电视。

## 关键组件 {#key-components}

* **服务令牌API** — 安全管理单点登录机制的密钥组件
* **列表API** — 应用程序可以帮助用户了解其生态系统中的设备列表
* **链接API** — 应用程序使用户能够在其生态系统中添加其他设备
* **取消链接API** — 应用程序使用户能够删除其生态系统中的设备

## 用例详细信息 {#use-cases-detailed}

### D2C-TVE SSO {#d2c-tve-sso}

此用例允许在一台设备上的D2C和TVE(MVPD)凭据之间进行链接，并在同一应用程序内的其他设备上使用该链接配置文件。

![D2C-TVE SSO流](../../../assets/sso_service_d2c_1.png)

![跨设备SSO流程](../../../assets/sso_service_d2c_2.png)

### 跨设备SSO {#cross-device-sso-detailed}

通过此使用案例，已在一台设备上经过身份验证的用户可以在其他设备上安装的同一D2C或TVE应用程序(实施Adobe Pass REST V2进行身份验证和授权所需的应用程序)上重复使用经过身份验证的配置文件。

## 如何在D2C-TVE应用程序中集成Adobe SSO服务 {#integration}

### 步骤1 — 获取通用标识符 {#step-1}

要集成Adobe SSO服务，应用程序实施应建立一个唯一的永久性ID，用作X-SSO-ID中的通用标识符SSO属性。 这可以通过使用D2C服务对用户进行身份验证并保留有关此身份验证的属性来获得。

### 步骤2 — 获取服务令牌 {#step-2}

在POST /serviceToken端点的X-SSO-ID中使用公共标识符将检索具有以下有效负载的已签名JWT：

```json
{
  "iss": "ssoservicetoken",
  "sub": "unique_common_identifier",
  "nbf": 1758093558,
  "exp": 1758097158,
  "iat": 1758093558
}
```

服务令牌具有“iat”（颁发时间）和“exp”（过期时间），服务令牌在该时间间隔内有效。 如果过期，则可以使用服务令牌过期的GET /serviceToken端点获取新的JWT。

### 步骤3 — 将Adobe Pass REST API V2与TVE MVPD一起进行身份验证 {#step-3}

应使用服务令牌实施Adobe Pass的身份验证： [REST API V2 — 单点登录服务令牌流程](https://experienceleague.adobe.com/zh-hans/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-flows/rest-api-v2-single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows)

### 步骤4 — 链接其他设备 {#step-4}

从经过身份验证的应用程序中，可以使用/link API创建要在其他设备上使用的链接

```json
{
    "status": "CREATED",
    "code": "228128",
    "notBefore": 1758094617220,
    "notAfter": 1758098217220
}
```

“代码”是一个短暂的链接代码，它是6位数序列的形式，用户将在辅助设备上的未经身份验证的应用程序中引入

### 步骤5 — 检索单点登录身份验证 {#step-5}

在不同的设备上，一旦用户引入了代码，应用程序就可以：

* 从D2C服务检索身份
* 使用REST V2 API从Adobe Pass检索MVPD配置文件

在获得初始身份验证期间，MVPD配置文件将通过SSO生效。

如果MVPD使配置文件失效或用户选择从MVPD注销，使用Adobe Pass REST API V2的应用程序将不再有配置文件记录，并且应要求用户再次通过MVPD进行身份验证。

### 步骤6 — 管理其他设备上的单点登录 {#step-6}

应用程序可以使用/list API获取与同一通用标识符关联的所有其他设备的信息。

在任何时候，都可以通过取消设备与/unlink API的链接将其从单点登录中删除。

## API {#apis}

### 服务令牌API {#service-token-api}

#### 描述 {#service-token-description}

服务令牌API可用于请求和管理可在多个应用程序或设备之间启用单点登录(SSO)功能的服务令牌。 这些服务令牌可识别经过身份验证的用户档案（SSO用户档案），对于建立和维护SSO连接至关重要。

>[!WARNING]
>
>服务令牌包含敏感的身份验证信息。 应用程序必须安全地处理这些令牌，并且绝不会将其暴露给不受信任的方面。

服务令牌API提供两个主要端点：

* **POST /api/{serviceProvider}/serviceToken** — 获取新创建的JWS服务令牌
* **GET /api/{serviceProvider}/serviceToken** — 刷新现有的JWS服务令牌

如果由于Adobe Pass身份验证服务错误而无法服务服务令牌API请求，则其他错误信息将作为API响应的一部分包含。

#### POST - serviceToken {#post-service-token}

##### 请求 {#post-service-token-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">路径</td>
      <td>/api/{serviceProvider}/serviceToken</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">方法</td>
      <td>POST</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">路径参数</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>正在为其请求令牌的服务提供程序标识符。</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">标头</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">授权</td>
      <td><a href="https://experienceleague.adobe.com/zh-hans/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization">授权</a>标头文档中描述了持有者令牌有效负载的生成。</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP设备标识符</td>
      <td>
         <a href="https://experienceleague.adobe.com/zh-hans/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier">AP-Device-Identifier</a>标头文档中描述了设备标识符有效负载的生成。
         <br/><br/>
         如果未提供X-SSO-ID，则此标识符用作默认的SSO标识符。
      </td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Device-Info</td>
      <td>
         <a href="https://experienceleague.adobe.com/zh-hans/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-x-device-info">X-Device-Info</a>标头文档中指定的设备信息。
         <br/><br/>
         <b>强烈建议</b>在应用程序的设备平台允许显式提供有效值时使用。
         <br/><br/>
         Adobe Pass身份验证后端会将显式设置的值与隐式提取的值合并。 如果未提供，则将使用默认的提取值。
      </td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-SSO链接</td>
      <td>
         将此请求与现有的已验证配置文件关联的链接代码。 提供后，响应将包含用于SSO的服务令牌以及生成链接代码的配置文件。
         <br/><br/>
         当辅助应用程序或设备想要从主应用程序或设备连接到已验证的配置文件时，通常使用此选项。
      </td>
      <td>如果未提供x-sso-id，则此为必填字段</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-SSO-ID</td>
      <td>
         应用程序请求作为SSO基础的通用标识符。
         <br/><br/>
         如果提供，此标识符将用于在设备和/或应用程序之间建立通用SSO配置文件。
      </td>
      <td>如果未提供x-sso-link，则此为必填字段</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Accept</td>
      <td>
         客户端应用程序接受的媒体类型。
         <br/><br/>
         如果指定，则必须是application/json。
      </td>
      <td>可选</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">User-Agent</td>
      <td>客户端应用程序的用户代理。</td>
      <td>可选</td>
   </tr>
</table>

##### 响应 {#post-service-token-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">代码</th>
      <th style="background-color: #EFF2F7;">文本</th>
      <th style="background-color: #EFF2F7;">描述</th>
   </tr>
   <tr>
      <td>201</td>
      <td>已创建</td>
      <td>
        服务令牌已成功生成并在响应正文中返回。
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>错误请求</td>
      <td>
        请求无效，客户端需要更正请求并重试。 响应正文可能包含遵守<a href="https://experienceleague.adobe.com/zh-hans/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">增强型错误代码</a>文档的错误信息。
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>未授权</td>
      <td>
        访问令牌无效，客户端需要获取新的访问令牌并重试。 有关更多详细信息，请参阅<a href="https://experienceleague.adobe.com/zh-hans/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview">动态客户端注册概述</a>文档。
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>内部服务器错误</td>
      <td>
        服务器端遇到问题。 响应正文可能包含遵守<a href="https://experienceleague.adobe.com/zh-hans/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">增强型错误代码</a>文档的错误信息。
      </td>
   </tr>
</table>

###### 成功 {#success-post-service-token}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">标头</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">状态</td>
      <td>201</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">正文</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">状态</td>
      <td>HTTP状态（例如“CREATED”）</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">jws</td>
      <td>包含服务令牌的Base64编码的JSON Web签名(JWS)。 此令牌可用于后续API调用，以识别经过身份验证的配置文件并启用SSO功能。</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notBefore</td>
      <td>纪元毫秒，或出错时为0</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notAfter</td>
      <td>纪元毫秒，或出错时为0</td>
      <td><i>必填</i></td>
   </tr>
</table>

###### 错误 {#error-post-service-token}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">标头</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">状态</td>
      <td>400， 401， 500</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">正文</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>响应正文可能提供附加的错误信息，这些信息将遵循<a href="https://experienceleague.adobe.com/zh-hans/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">增强型错误代码</a>文档。</td>
      <td><i>必填</i></td>
   </tr>
</table>

## 示例 {#samples-post-service-token}

### 1.请求新的服务令牌（使用SSO ID）

>[!BEGINTABS]

>[!TAB 请求]

```HTTPS
POST /api/{serviceProvider}/serviceToken HTTP/1.1

    Authorization: Bearer <access_token>
    X-SSO-ID: <sso_id>
    AP-Device-Identifier: fingerprint <base64_device_id>
    X-Device-Info: <base64_device_info>
    Accept: application/json
```

>[!TAB 响应]

```HTTPS
HTTP/1.1 201 Created

Content-Type: application/json

{
  "status": "CREATED",
  "serviceToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "notBefore": 1710000000000,
  "notAfter": 1710003600000
}
```

>[!ENDTABS]

### 2.请求新的服务令牌（使用SSO链接）

>[!BEGINTABS]

>[!TAB 请求]

```HTTPS
POST /api/{serviceProvider}/serviceToken HTTP/1.1

    Authorization: Bearer <access_token>
    X-SSO-LINK: <link_code>
    AP-Device-Identifier: fingerprint <base64_device_id>
    X-Device-Info: <base64_device_info>
    User-Agent: <user_agent>
    Accept: application/json
```

>[!TAB 响应]

```HTTPS
HTTP/1.1 201 Created

Content-Type: application/json

{
  "status": "CREATED",
  "serviceToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "notBefore": 1710000000000,
  "notAfter": 1710003600000
}
```

>[!ENDTABS]

#### GET - serviceToken {#get-service-token}

##### 请求 {#get-service-token-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">路径</td>
      <td>/api/{serviceProvider}/serviceToken</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">方法</td>
      <td>GET</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">路径参数</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>正在为其请求令牌的服务提供程序标识符。</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">标头</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">授权</td>
      <td><a href="https://experienceleague.adobe.com/zh-hans/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization">授权</a>标头文档中描述了持有者令牌有效负载的生成。</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-Token</td>
      <td>
         以前获取的需要刷新的服务令牌。
         <br/><br/>
         此令牌必须有效或最近已过期才能进行刷新。
      </td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Accept</td>
      <td>
         客户端应用程序接受的媒体类型。
         <br/><br/>
         如果指定，则必须是application/json。
      </td>
      <td>可选</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">User-Agent</td>
      <td>客户端应用程序的用户代理。</td>
      <td>可选</td>
   </tr>
</table>

##### 响应 {#get-service-token-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">代码</th>
      <th style="background-color: #EFF2F7;">文本</th>
      <th style="background-color: #EFF2F7;">描述</th>
   </tr>
   <tr>
      <td>200</td>
      <td>确定</td>
      <td>
        服务令牌已成功刷新并在响应正文中返回。
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>错误请求</td>
      <td>
        请求无效，客户端需要更正请求并重试。 响应正文可能包含遵守<a href="https://experienceleague.adobe.com/zh-hans/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">增强型错误代码</a>文档的错误信息。
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>未授权</td>
      <td>
        访问令牌或服务令牌无效，客户端需要获取新的访问令牌或服务令牌，然后重试。 有关更多详细信息，请参阅<a href="https://experienceleague.adobe.com/zh-hans/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview">动态客户端注册概述</a>文档。
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>内部服务器错误</td>
      <td>
        服务器端遇到问题。 响应正文可能包含遵守<a href="https://experienceleague.adobe.com/zh-hans/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">增强型错误代码</a>文档的错误信息。
      </td>
   </tr>
</table>

###### 成功 {#success-get-service-token}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">标头</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">状态</td>
      <td>200</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">正文</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">状态</td>
      <td>HTTP状态（例如“OK”）</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">jws</td>
      <td>包含刷新的服务令牌的Base64编码JSON Web签名(JWS)。 此令牌可用于后续API调用，以识别经过身份验证的配置文件并启用SSO功能。</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notBefore</td>
      <td>纪元毫秒，或出错时为0</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notAfter</td>
      <td>纪元毫秒，或出错时为0</td>
      <td><i>必填</i></td>
   </tr>
</table>

###### 错误 {#error-get-service-token}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">标头</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">状态</td>
      <td>400， 401， 500</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">正文</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>响应正文可能提供附加的错误信息，这些信息将遵循<a href="https://experienceleague.adobe.com/zh-hans/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">增强型错误代码</a>文档。</td>
      <td><i>必填</i></td>
   </tr>
</table>

## 示例 {#samples-get-service-token}

### 1.请求刷新服务令牌

>[!BEGINTABS]

>[!TAB 请求]

```HTTPS
GET /api/{serviceProvider}/serviceToken HTTP/1.1

    Authorization: Bearer <access_token>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
```

>[!TAB 响应]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "status": "OK",
  "serviceToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "notBefore": 1710000000000,
  "notAfter": 1710003600000
}
```

>[!ENDTABS]

### 链接API {#link-api}

#### 描述 {#link-description}

Link API可用于请求可以在多个应用程序或设备之间启用单点登录(SSO)的链接代码（或QR代码）。 此链接代码允许用户将新的应用程序或设备连接到现有的已验证配置文件（SSO配置文件），从而提供跨应用程序或设备的无缝SSO体验。

链接API要求在AD-Service-Token标头中提供有效的服务令牌。

生成的链接代码通常显示给主应用程序或设备上的用户，并在辅助应用程序或设备上输入以建立SSO连接。 链接代码的有效期有限（通常为5-30分钟），仅供一次性使用。

如果由于Adobe Pass身份验证服务错误而无法服务链接API请求，则其他错误信息将包含在链接API响应结果中。

#### POST — 链接 {#post-link}

##### 请求 {#post-link-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">路径</td>
      <td>/api/{serviceProvider}/link</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">方法</td>
      <td>POST</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">路径参数</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>正在为其请求令牌的服务提供程序标识符。</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">标头</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">授权</td>
      <td><a href="https://experienceleague.adobe.com/zh-hans/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization">授权</a>标头文档中描述了持有者令牌有效负载的生成。</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP设备标识符</td>
      <td><a href="https://experienceleague.adobe.com/zh-hans/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier">AP-Device-Identifier</a>标头文档中描述了设备标识符有效负载的生成。</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-Token</td>
      <td>
         服务令牌API文档对服务令牌的生成进行了描述。
         <br/><br/>
         此服务令牌标识要为其生成链接代码的已验证配置文件。
      </td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Accept</td>
      <td>
         客户端应用程序接受的媒体类型。
         <br/><br/>
         如果指定，则必须是application/json。
      </td>
      <td>可选</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">User-Agent</td>
      <td>客户端应用程序的用户代理。</td>
      <td>可选</td>
   </tr>
</table>

##### 响应 {#post-link-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">代码</th>
      <th style="background-color: #EFF2F7;">文本</th>
      <th style="background-color: #EFF2F7;">描述</th>
   </tr>
   <tr>
      <td>201</td>
      <td>已创建</td>
      <td>
        已成功生成链接代码并在响应正文中返回。
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>错误请求</td>
      <td>
        请求无效，客户端需要更正请求并重试。 响应正文可能包含遵守<a href="https://experienceleague.adobe.com/zh-hans/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">增强型错误代码</a>文档的错误信息。
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>未授权</td>
      <td>
        访问令牌无效，客户端需要获取新的访问令牌并重试。 有关更多详细信息，请参阅<a href="https://experienceleague.adobe.com/zh-hans/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview">动态客户端注册概述</a>文档。
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>内部服务器错误</td>
      <td>
        服务器端遇到问题。 响应正文可能包含遵守<a href="https://experienceleague.adobe.com/zh-hans/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">增强型错误代码</a>文档的错误信息。
      </td>
   </tr>
</table>

###### 成功 {#success-post-link}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">标头</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">状态</td>
      <td>201</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">正文</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">状态</td>
      <td>HTTP状态（例如“CREATED”）</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">链接</td>
      <td>可用于辅助应用程序或设备以建立SSO连接的短数字或字母数字代码。</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notBefore</td>
      <td>链接代码生效时的时间戳（以自纪元以来的毫秒数为单位）。</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notAfter</td>
      <td>链接代码过期的时间戳（以自纪元以来的毫秒数为单位）。</td>
      <td><i>必填</i></td>
   </tr>
</table>

###### 错误 {#error-post-link}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">标头</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">状态</td>
      <td>400， 401， 500</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">正文</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>响应正文可能提供附加的错误信息，这些信息将遵循<a href="https://experienceleague.adobe.com/zh-hans/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">增强型错误代码</a>文档。</td>
      <td><i>必填</i></td>
   </tr>
</table>

## 示例 {#samples-post-link}

### 1.请求现有已验证配置文件的链接代码

>[!BEGINTABS]

>[!TAB 请求]

```HTTPS
POST /api/{serviceProvider}/link HTTP/1.1

    Authorization: Bearer <access_token>
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
```

>[!TAB 响应]

```HTTPS
HTTP/1.1 201 Created

Content-Type: application/json

{            
   "status": "CREATED",
   "code": "123456",
   "notBefore": 1623840000000,
   "notAfter": 1623842700000
}
```

>[!ENDTABS]

### 取消API链接 {#unlink-api}

#### 描述 {#unlink-description}

取消链接API可用于请求从已验证的配置文件（SSO配置文件）中删除一个或多个设备。 此API允许用户将设备从其SSO设置断开连接，从而控制哪些设备有权访问其经过身份验证的配置文件。

>[!WARNING]
>
>取消链接API要求在AD-Service-Token标头中提供有效的服务令牌。

如果由于Adobe Pass身份验证服务错误而无法服务取消链接API请求，则其他错误信息将包含在取消链接API响应结果中。

#### 发布 — 取消链接 {#post-unlink}

##### 请求 {#post-unlink-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">路径</td>
      <td>/api/{serviceProvider}/unlink</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">方法</td>
      <td>POST</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">路径参数</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>服务提供商标识符。</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">正文参数</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">设备</td>
      <td>
         要取消链接的设备标识符数组。
         <br/><br/>
         示例：<br/><code>["deviceid1", "deviceid2", "deviceid3"]</code>
      </td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">标头</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">授权</td>
      <td><a href="https://experienceleague.adobe.com/zh-hans/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization">授权</a>标头文档中描述了持有者令牌有效负载的生成。</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>
         所发送资源的接受媒体类型。
         <br/><br/>
         它必须是application/json。
      </td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP设备标识符</td>
      <td><a href="https://experienceleague.adobe.com/zh-hans/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier">AP-Device-Identifier</a>标头文档中描述了设备标识符有效负载的生成。</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-Token</td>
      <td>
         服务令牌API文档对服务令牌的生成进行了描述。
         <br/><br/>
         此服务令牌标识要取消其设备链接的已验证配置文件。
      </td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Accept</td>
      <td>
         客户端应用程序接受的媒体类型。
         <br/><br/>
         如果指定，则必须是application/json。
      </td>
      <td>可选</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">User-Agent</td>
      <td>客户端应用程序的用户代理。</td>
      <td>可选</td>
   </tr>
</table>

##### 响应 {#post-unlink-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">代码</th>
      <th style="background-color: #EFF2F7;">文本</th>
      <th style="background-color: #EFF2F7;">描述</th>
   </tr>
   <tr>
      <td>200</td>
      <td>确定</td>
      <td>
        已成功从SSO设置中取消请求设备的链接。
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>错误请求</td>
      <td>
        请求无效，客户端需要更正请求并重试。 响应正文可能包含遵守<a href="https://experienceleague.adobe.com/zh-hans/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">增强型错误代码</a>文档的错误信息。
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>未授权</td>
      <td>
        访问令牌无效，客户端需要获取新的访问令牌并重试。 有关更多详细信息，请参阅<a href="https://experienceleague.adobe.com/zh-hans/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview">动态客户端注册概述</a>文档。
      </td>
   </tr>
   <tr>
      <td>405</td>
      <td>不允许使用该方法</td>
      <td>
        HTTP方法无效，客户端需要使用请求资源允许的HTTP方法并重试。
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>内部服务器错误</td>
      <td>
        服务器端遇到问题。 响应正文可能包含遵守<a href="https://experienceleague.adobe.com/zh-hans/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">增强型错误代码</a>文档的错误信息。
      </td>
   </tr>
</table>

###### 成功 {#success-post-unlink}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">标头</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">状态</td>
      <td>200</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">正文</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">状态</td>
      <td>有关操作结果的信息：“确定”</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">unlinkedDevices</td>
      <td>
         已成功取消链接的设备列表。
         <br/><br/>
         示例：<br/><code>["deviceid1", "deviceid2", "deviceid3"]</code>
      </td>
      <td><i>必填</i></td>
   </tr>
</table>

###### 错误 {#error-post-unlink}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">标头</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">状态</td>
      <td>400， 401， 405， 500</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">正文</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>响应正文可能提供附加的错误信息，这些信息将遵循<a href="https://experienceleague.adobe.com/zh-hans/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">增强型错误代码</a>文档。</td>
      <td><i>必填</i></td>
   </tr>
</table>

## 示例 {#samples-post-unlink}

### 1.请求从SSO配置文件中取消设备链接

>[!BEGINTABS]

>[!TAB 请求]

```HTTPS
POST /api/{serviceProvider}/unlink HTTP/1.1

    Authorization: Bearer <access_token>
    Content-Type: application/json
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
    
{
  "devices": [
    "deviceid1",
    "deviceid2",
    "deviceid3"
  ]
}
```

>[!TAB 响应]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "status": "OK",
  "unlinkedDevices": [
    "deviceid1",
    "deviceid2",
    "deviceid3"
  ]
}
```

>[!ENDTABS]

### 2.请求取消设备链接并部分成功

>[!BEGINTABS]

>[!TAB 请求]

```HTTPS
POST /api/{serviceProvider}/unlink HTTP/1.1

    Authorization: Bearer <access_token>
    Content-Type: application/json
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
    
{
  "devices": [
    "deviceid1",
    "unknowndevice",
    "deviceid3"
  ]
}
```

>[!TAB 响应]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "status": "OK",
  "unlinkedDevices": [
    "deviceid1",
    "deviceid3"
  ]
}
```

>[!ENDTABS]

### 列表API {#list-api}

#### 描述 {#list-description}

List API可用于获取当前连接到已验证配置文件（SSO配置文件）的设备列表。 此API允许用户和应用程序查看哪些设备属于其SSO设置的一部分，从而为其跨多个设备的经过身份验证的体验提供可见性和管理功能。

>[!WARNING]
>
>列表API要求在AD-Service-Token标头中提供有效的服务令牌。

List API返回与已验证配置文件（SSO配置文件）中的每个设备相关的详细信息，包括设备标识符和元数据，可帮助用户识别其设备。 此信息可用于帮助用户就哪些设备应保留在其SSO设置中做出明智的决策。

如果由于Adobe Pass身份验证服务错误而无法服务列表API请求，则其他错误信息将包含在列表API响应结果中。

#### GET — 列表 {#get-list}

##### 请求 {#get-list-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">路径</td>
      <td>/api/{serviceProvider}/list</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">方法</td>
      <td>GET</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">路径参数</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>服务提供商标识符。</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">标头</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">授权</td>
      <td><a href="https://experienceleague.adobe.com/zh-hans/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization">授权</a>标头文档中描述了持有者令牌有效负载的生成。</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP设备标识符</td>
      <td><a href="https://experienceleague.adobe.com/zh-hans/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier">AP-Device-Identifier</a>标头文档中描述了设备标识符有效负载的生成。</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-Token</td>
      <td>
         服务令牌API文档对服务令牌的生成进行了描述。
         <br/><br/>
         此服务令牌标识要检索其设备列表的已验证配置文件。
      </td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Accept</td>
      <td>
         客户端应用程序接受的媒体类型。
         <br/><br/>
         如果指定，则必须是application/json。
      </td>
      <td>可选</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">User-Agent</td>
      <td>客户端应用程序的用户代理。</td>
      <td>可选</td>
   </tr>
</table>

##### 响应 {#get-list-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">代码</th>
      <th style="background-color: #EFF2F7;">文本</th>
      <th style="background-color: #EFF2F7;">描述</th>
   </tr>
   <tr>
      <td>200</td>
      <td>确定</td>
      <td>
        已成功检索SSO设置中的设备列表，并在响应正文中返回该列表。
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>错误请求</td>
      <td>
        请求无效，客户端需要更正请求并重试。 响应正文可能包含遵守<a href="https://experienceleague.adobe.com/zh-hans/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">增强型错误代码</a>文档的错误信息。
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>未授权</td>
      <td>
        访问令牌无效，客户端需要获取新的访问令牌并重试。 有关更多详细信息，请参阅<a href="https://experienceleague.adobe.com/zh-hans/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview">动态客户端注册概述</a>文档。
      </td>
   </tr>
   <tr>
      <td>405</td>
      <td>不允许使用该方法</td>
      <td>
        HTTP方法无效，客户端需要使用请求资源允许的HTTP方法并重试。
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>内部服务器错误</td>
      <td>
        服务器端遇到问题。 响应正文可能包含遵守<a href="https://experienceleague.adobe.com/zh-hans/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">增强型错误代码</a>文档的错误信息。
      </td>
   </tr>
</table>

###### 成功 {#success-get-list}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">标头</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">状态</td>
      <td>200</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">正文</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">设备</td>
      <td>
         JSON包含键、值对的映射。
         <br/><br/>
         <b>密钥：</b> deviceId - <a href="https://experienceleague.adobe.com/zh-hans/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier">AP-Device-Identifier</a>标头文档中所述的设备标识符有效负载
         <br/><br/>
         <b>值：</b>属性 — JSON包含设备元数据属性映射，包括：
         <ul>
            <li>设备类型</li>
            <li>平台</li>
            <li>用户代理</li>
            <li>有助于识别设备的其他相关元数据</li>
         </ul>
         属性的值可以是简单的（字符串、整数、布尔值等）
      </td>
      <td><i>必填</i></td>
   </tr>
</table>

###### 错误 {#error-get-list}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">标头</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">状态</td>
      <td>400， 401， 405， 500</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">正文</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>响应正文可能提供附加的错误信息，这些信息将遵循<a href="https://experienceleague.adobe.com/zh-hans/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">增强型错误代码</a>文档。</td>
      <td><i>必填</i></td>
   </tr>
</table>

## 示例 {#samples-get-list}

### 1.在SSO配置文件中列出设备的请求

>[!BEGINTABS]

>[!TAB 请求]

```HTTPS
GET /api/{serviceProvider}/list HTTP/1.1

    Authorization: Bearer <access_token>
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
```

>[!TAB 响应]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "devices": {
    "deviceid1": {
      "deviceType": "smartTV",
      "model": "Samsung",
      "os": "Tizen",
      "osVersion": "5.0",
      "lastSeen": 1623840000000,
      "type": "regular"
    },
    "deviceid2": {
      "deviceType": "mobile",
      "model": "iPhone",
      "os": "iOS",
      "osVersion": "14.5",
      "lastSeen": 1623830000000,
      "type": "sso"
    },
    "deviceid3": {
      "deviceType": "tablet",
      "model": "iPad",
      "os": "iPadOS",
      "osVersion": "14.5",
      "lastSeen": 1623820000000,
      "type": "sso"
    }
  }
}
```

>[!ENDTABS]

### 2.请求列出没有链接设备的设备

>[!BEGINTABS]

>[!TAB 请求]

```HTTPS
GET /api/{serviceProvider}/list HTTP/1.1

    Authorization: Bearer <access_token>
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
```

>[!TAB 响应]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "devices": {}
}
```

>[!ENDTABS]

## 错误代码 {#error-codes}

### 错误响应结构 {#error-structure}

所有错误响应都包含以下字段：

| 字段 | 类型 | 描述 |
|:---|:---|:---|
| 状态 | 整数 | HTTP状态代码（400、401、500等） |
| 代码 | 字符串 | 机器可读的错误代码 |
| message | 字符串 | 易于用户识别的描述 |
| 操作 | 字符串 | 客户端的建议操作 |
| helpUrl | 字符串 | 文档链接 |
| trace | 字符串 | 关联的唯一请求ID (UUID) |

**错误响应示例**

```json
{
  "status": "BAD_REQUEST",
  "error": {
    "status": 400,
    "code": "header_missing",
    "message": "Required header is missing",
    "action": "check_headers",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html?lang=zh-Hans",
    "trace": "a1b2c3d4-e5f6-7890-abcd-ef1234567890"
  }
}
```

**成功响应示例**

```json
{
  "status": "OK",
  "serviceToken": "eyJhbGciOiJIUzI1NiIs...",
  "notBefore": 1697500800000,
  "notAfter": 1697587200000
}
```

>[!NOTE]
>
>成功响应不包含错误字段。 错误响应不包含成功字段。

### 错误代码目录 {#error-catalog}

#### 一般错误

| 代码 | 状态 | message | 操作 |
|:---|:---|:---|:---|
| token_valid | 400 | 提供的令牌无效 | get_new_token |
| token_expired | 401 | 令牌已过期 | get_new_token |
| header_missing | 400 | 缺少所需的标头 | check_headers |
| 未授权 | 401 | 未经授权的访问 | 无 |
| internal_error | 500 | 发生内部错误 | 无 |

#### 验证错误

| 代码 | 状态 | message | 操作 |
|:---|:---|:---|:---|
| request_null | 400 | 请求对象不能为空 | 无 |

#### POST ServiceToken错误

| 代码 | 状态 | message | 操作 |
|:---|:---|:---|:---|
| header_missing | 400 | post请求需要x-sso-id或x-sso-link标头 | check_headers |
| header_missing | 400 | POST请求需要AP-Device-Identifier标头 | check_headers |

#### GET ServiceToken错误

| 代码 | 状态 | message | 操作 |
|:---|:---|:---|:---|
| header_missing | 400 | GET请求需要AD-Service-Token标头 | check_headers |
| header_invalid | 401 | AD-Service-Token中的JWT签名无效 | get_new_token |
| header_invalid | 401 | 验证JWT签名时出错 | get_new_token |
| header_invalid | 401 | AD服务令牌中的JWT主题(sub)缺失或为空 | get_new_token |
| header_invalid | 401 | 提取JWT主题时出错 | get_new_token |

#### 链接验证错误

| 代码 | 状态 | message | 操作 |
|:---|:---|:---|:---|
| header_missing | 401 | 链接请求需要AD-Service-Token标头 | check_headers |
| header_invalid | 401 | AD-Service-Token中的JWT签名无效 | get_new_token |
| header_invalid | 401 | 验证JWT签名时出错 | get_new_token |

#### 取消链接验证错误

| 代码 | 状态 | message | 操作 |
|:---|:---|:---|:---|
| header_missing | 401 | 取消链接请求需要AD-Service-Token标头 | check_headers |
| header_invalid | 401 | AD-Service-Token中的JWT签名无效 | get_new_token |
| header_invalid | 401 | 验证JWT签名时出错 | get_new_token |
| header_invalid | 401 | AD服务令牌中的JWT主题(sub)缺失或为空 | get_new_token |
| request_valid | 400 | 设备列表不能为null或空 | check_request_body |

#### 列出验证错误

| 代码 | 状态 | message | 操作 |
|:---|:---|:---|:---|
| header_missing | 401 | 列表请求需要AD-Service-Token标头 | check_headers |
| header_invalid | 401 | AD-Service-Token中的JWT签名无效 | get_new_token |
| header_invalid | 401 | 验证JWT签名时出错 | get_new_token |
| header_invalid | 401 | AD服务令牌中的JWT主题(sub)缺失或为空 | get_new_token |
| header_invalid | 401 | 提取JWT主题时出错 | get_new_token |

