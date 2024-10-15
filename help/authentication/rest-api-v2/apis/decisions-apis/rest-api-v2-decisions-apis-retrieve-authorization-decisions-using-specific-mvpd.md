---
title: 使用特定的mvpd检索授权决策
description: REST API V2 — 使用特定mvpd检索授权决策
exl-id: e8889395-4434-4bec-a212-a8341bb9c310
source-git-commit: ca8eaff83411daab5f136f01394e1d425e66f393
workflow-type: tm+mt
source-wordcount: '907'
ht-degree: 1%

---

# 使用特定的mvpd检索授权决策 {#retrieve-authorization-decisions-using-specific-mvpd}

>[!IMPORTANT]
>
> 此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

>[!IMPORTANT]
>
> REST API V2实施受[限制机制](/help/authentication/throttling-mechanism.md)文档限制。

## 请求 {#request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">路径</td>
      <td>/api/v2/{serviceProvider}/decisions/authorize/{mvpd}</td>
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
      <td>在载入过程中与服务提供商关联的内部唯一标识符。</td>
      <td><i>必填</i></td>
   </tr>
    <tr>
      <td style="background-color: #DEEBFF;">mvpd</td>
      <td>在新用户引导过程中与身份提供商关联的内部唯一标识符。</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">正文参数</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">资源</td>
      <td>在可以播放它们之前需要MVPD决策的资源的列表。</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">标头</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">授权</td>
      <td><a href="../../appendix/headers/rest-api-v2-appendix-headers-authorization.md">授权</a>标头文档中描述了持有者令牌有效负载的生成。</td>
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
      <td><a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md">AP-Device-Identifier</a>标头文档中描述了设备标识符有效负载的生成。</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Device-Info</td>
      <td>
         <a href="../../appendix/headers/rest-api-v2-appendix-headers-x-device-info.md">X-Device-Info</a>标头文档中介绍了设备信息有效负载的生成。
         <br/><br/>
         强烈建议在应用程序的设备平台允许显式提供有效值时始终使用它。
         <br/><br/>
         提供该属性后，Adobe Pass身份验证后端将隐式地将显式设置的值与提取的值合并（默认情况下）。
         <br/><br/>
         如果未提供，Adobe Pass身份验证后端将隐式使用提取的值（默认情况下）。
      </td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Forwarded-For</td>
      <td>
         流设备的IP地址。
         <br/><br/>
         强烈建议始终将其用于服务器到服务器的实施，尤其是在由程序员服务而不是流设备进行调用时。
         <br/><br/>
         对于客户端到服务器实施，流设备的IP地址将隐式发送。
      </td>
      <td>可选</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Adobe-Subject-Token</td>
      <td>
        <a href="../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md">Platform-Subject-Token</a>标头文档介绍了为Platform Identity方法生成单点登录有效负载的过程。Adobe
        <br/><br/>
        有关使用平台标识启用单点登录的流的更多详细信息，请参阅<a href="../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md">使用平台标识流的单点登录</a>文档。
      </td>
      <td>可选</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-Token</td>
      <td>
        <a href="../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md">AD-Service-Token</a>标头文档介绍了服务令牌方法单点登录有效负载的生成。
        <br/><br/>
        有关使用服务令牌启用单点登录的流的更多详细信息，请参阅<a href="../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md">使用服务令牌流的单点登录</a>文档。
      </td>
      <td>可选</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP — 合作伙伴 — 框架 — 状态</td>
      <td>
        <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md">AP-Partner-Framework-Status</a>标头文档介绍了为Partner方法生成单一登录有效负载的过程。
        <br/><br/>
        有关使用合作伙伴启用单点登录流程的更多详细信息，请参阅<a href="../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md">使用合作伙伴流程进行单点登录</a>文档。</td>
      <td>可选</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-TempPass-Identity</td>
      <td><a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md">AP-TempPass-Identity</a>标头文档中描述了用户唯一标识符有效负载的生成。</td>
      <td>可选</td>
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

## 响应 {#response}

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
        响应正文包含一个决定列表，其中提供了更多信息。
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>错误请求</td>
      <td>
        请求无效，客户端需要更正请求并重试。 响应正文可能包含附在<a href="../../../enhanced-error-codes.md">增强型错误代码</a>文档中的错误信息。
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>未授权</td>
      <td>
        访问令牌无效，客户端需要获取新的访问令牌并重试。 有关更多详细信息，请参阅<a href="../../../dcr-api/dynamic-client-registration-overview.md">动态客户端注册概述</a>文档。
      </td>
   </tr>
   <tr>
      <td>405</td>
      <td>不允许使用该方法</td>
      <td>
        HTTP方法无效，客户端需要使用请求资源允许的HTTP方法并重试。 有关更多详细信息，请参阅<a href="#request">请求</a>部分。
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>内部服务器错误</td>
      <td>
        服务器端遇到问题。 响应正文可能包含遵守<a href="../../../enhanced-error-codes.md">增强型错误代码</a>文档的错误信息。
      </td>
   </tr>
</table>

### 成功 {#success}

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
      <td style="background-color: #DEEBFF;">决策</td>
      <td>
         JSON包含元素列表，每个元素均具有以下属性：
         <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">属性</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
                <td style="background-color: #DEEBFF;">资源</td>
                <td>为其返回授权决策的资源标识符。</td>
                <td><i>必填</i></td>
            </tr>
            <tr>
                <td style="background-color: #DEEBFF;">serviceProvider</td>
                <td>在载入过程中与服务提供商关联的内部唯一标识符。</td>
                <td><i>必填</i></td>
            </tr>
            <tr>
                <td style="background-color: #DEEBFF;">mvpd</td>
                <td>在新用户引导过程中与身份提供商关联的内部唯一标识符。</td>
                <td><i>必填</i></td>
            </tr>
            <tr>
                <td style="background-color: #DEEBFF;">authorized</td>
                <td>资源的决策状态，可以为“true”或“false”。</td>
                <td><i>必填</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">源</td>
               <td>
                  有关决策源的信息。
                  <br/><br/>
                  可能的值包括：
                  <ul>
                    <li><b>mvpd</b><br/>决策由MVPD授权终结点发出。</li>
                    <li><b>降级</b><br/>决策因访问降级而发出。</li>
                    <li><b>temppass</b><br/>决策作为临时访问的结果发布。</li>
                    <li><b>dummy</b><br/>决策是由虚拟授权功能发出的。</li>
                  </ul>
               <td><i>必填</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">令牌</td>
               <td>
                  有关媒体令牌的信息。
                  <br/><br/>
                  具有以下属性的JSON对象：
                  <ul>
                    <li><b>notBefore</b><br/>媒体令牌失效之前的时间戳。</li>
                    <li><b>notAfter</b><br/>时间戳，在此时间戳之后媒体令牌无效。</li>
                    <li><b>serializedToken</b><br/>Base64编码的媒体令牌。</li>
                  </ul>
               <td>可选</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">notBefore</td>
               <td>决策无效之前的时间戳。</td>
               <td>可选</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">notAfter</td>
               <td>超过该时间戳决策无效。</td>
               <td>可选</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">错误</td>
               <td>该错误提供了有关遵守<a href="../../../enhanced-error-codes.md">增强型错误代码</a>文档的“拒绝”决定的其他信息。</td>
               <td>可选</td>
            </tr>
         </table>
      </td>
      <td><i>必填</i></td>
</table>

### 错误 {#error}

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
      <td>响应正文可能提供附加的错误信息，这些信息将遵循<a href="../../../enhanced-error-codes.md">增强型错误代码</a>文档。</td>
      <td><i>必填</i></td>
   </tr>
</table>

## 示例 {#samples}

### 1.使用特定的mvpd检索授权决策

>[!BEGINTABS]

>[!TAB 请求]

```HTTPS
POST /api/v2/REF30/decisions/authorize/Cablevision HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Content-Type: application/json
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
        
Body:

{
    "resources": ["REF30"]
}
```

>[!TAB 响应]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "REF30",
            "mvpd": "Cablevision",
            "source": "mvpd",
            "authorized": true,
            "token": {
                "notBefore": 1697094207324,
                "notAfter": 1697094802367,
                "serializedToken": "PHNpZ25hdHVyZUluZm8..."
            },
            "notBefore": 1697094207324,
            "notAfter": 1697098802367
        }
    ]
}
```

>[!ENDTABS]

### 2.在应用降级的情况下，使用特定的mvpd检索授权决策

>[!BEGINTABS]

>[!TAB 请求]

```HTTPS
POST /api/v2/REF30/decisions/authorize/${degradedMvpd} HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Content-Type: application/json
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
        
Body:

{
    "resources": ["REF30", "apasstest1"]
}
```

>[!TAB 响应 — AuthNAll降级]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "REF30",
            "mvpd": "${degradedMvpd}",
            "source": "degradation",
            "authorized": true,
            "token": {
                "notBefore": 1697543318183,
                "notAfter": 1697543918183,
                "serializedToken": "PHNpZ25hdHVyZUluZm8+..."
            },
            "notBefore": 1697543318183,
            "notAfter": 1697549918183
        }
        {
            "resource": "apasstest1",
            "serviceProvider": "REF30",
            "mvpd": "${degradedMvpd}",
            "source": "degradation",
            "authorized": true,
            "token": {
                "notBefore": 1697543318183,
                "notAfter": 1697543918183,
                "serializedToken": "TYGjZ33jjLPi78yuX99+..."
            },
            "notBefore": 1697543318183,
            "notAfter": 1697549918183
        }
    ]
}
```

>[!TAB 响应 — AuthZAll降级]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "REF30",
            "mvpd": "${degradedMvpd}",
            "source": "degradation",
            "authorized": true,
            "token": {
                "notBefore": 1697543318183,
                "notAfter": 1697543918183,
                "serializedToken": "PHNpZ25hdHVyZUluZm8+..."
            }
        }
        {
            "resource": "apasstest1",
            "serviceProvider": "REF30",
            "mvpd": "${degradedMvpd}",
            "source": "degradation",
            "authorized": true,
            "token": {
                "notBefore": 1697543318183,
                "notAfter": 1697543918183,
                "serializedToken": "TYGjZ33jjLPi78yuX99+..."
            }
        }
    ]
}
```

>[!TAB 响应 — AuthZNone降级]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "REF30",
            "mvpd": "${degradedMvpd}",
            "source": "degradation",
            "authorized": false,
            "error": {
                "status": 403,
                "code": "authorization_denied_by_degradation_rule",
                "message": "The integration has an AuthZNone rule applied for the requested resources",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "none"
            }
        }
        {
            "resource": "apasstest1",
            "serviceProvider": "REF30",
            "mvpd": "${degradedMvpd}",
            "source": "degradation",
            "authorized": false,
            "error": {
                "status": 403,
                "code": "authorization_denied_by_degradation_rule",
                "message": "The integration has an AuthZNone rule applied for the requested resources",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "none"
            }
        }
    ]
}
```

>[!ENDTABS]

### 3.使用基本TempPass检索授权决策

>[!BEGINTABS]

>[!TAB 请求]

```HTTPS
POST /api/v2/apasstest1/decisions/authorize/TempPass_TEST40 HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Content-Type: application/json
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)

Body:

{
    "resources": ["REF30"]
}
```

>[!TAB 响应 — 可用]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "apasstest1",
            "mvpd": "TempPass_TEST40",
            "source": "temppass",
            "authorized": true,
            "token": {
                "notBefore": 1697094207324,
                "notAfter": 1697094802367,
                "serializedToken": "PHNpZ25hdHVyZUluZm8..."
            },
            "notBefore": 1697094207324,
            "notAfter": 1697594802367
        }
    ]
}
```

>[!TAB 响应 — 超出持续时间限制]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "apasstest1",
            "mvpd": "TempPass_TEST40",
            "source": "temppass",
            "authorized": false,
            "error": {
                "status": 403,
                "code": "temporary_access_duration_limit_exceeded",
                "message": "The temporary access duration limit has been exceeded.",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "authentication"
            }
        }
    ]
}
```

>[!TAB 响应 — 配置无效]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "apasstest1",
            "mvpd": "TempPass_TEST40",
            "source": "temppass",
            "authorized": false,
            "error": {
                "status": 500,
                "code": "invalid_configuration_temporary_access",
                "message": "The temporary access configuration is invalid.",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "configuration"
            }
        }
    ]
}
```

>[!ENDTABS]

### 4.使用促销临时传递检索授权决策

>[!BEGINTABS]

>[!TAB 请求]

```HTTPS
POST /api/v2/apasstest1/decisions/authorize/flexibleTempPass HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Content-Type: application/json
    AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    AP-TempPass-Identity: eyJlbWFpbCI6ImZvb0BiYXIuY29tIn0=
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)

Body:

{
    "resources": ["REF30"]
}
```

>[!TAB 响应 — 可用]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "apasstest1",
            "mvpd": "flexibleTempPass",
            "source": "temppass",
            "authorized": true,
            "token": {
                "notBefore": 1697543318183,
                "notAfter": 1697543918183,
                "serializedToken": "PHNpZ25hdHVyZUluZm8+..."
            },
            "notBefore": 1697543318183,
            "notAfter": 1697843918183
        }
    ]
}
```

>[!TAB 响应 — 超出持续时间限制]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "apasstest1",
            "mvpd": "flexibleTempPass",
            "source": "temppass",
            "authorized": false,
            "error": {
                "status": 403,
                "code": "temporary_access_duration_limit_exceeded",
                "message": "The temporary access duration limit has been exceeded.",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "authentication"
            }
        }
    ]
}
```

>[!TAB 响应 — 超出资源限制]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "apasstest1",
            "mvpd": "flexibleTempPass",
            "source": "temppass",
            "authorized": false,
            "error": {
                "status": 403,
                "code": "temporary_access_resources_limit_exceeded",
                "message": "The temporary access resources limit has been exceeded.",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "authentication"
            }
        }
    ]
}
```

>[!TAB 响应 — 配置无效]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "apasstest1",
            "mvpd": "flexibleTempPass",
            "source": "temppass",
            "authorized": false,
            "error": {
                "status": 500,
                "code": "invalid_configuration_temporary_access",
                "message": "The temporary access configuration is invalid.",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "configuration"
            }
        }
    ]
}
```

>[!TAB 响应 — 标识无效]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "apasstest1",
            "mvpd": "flexibleTempPass",
            "source": "temppass",
            "authorized": false,
            "error": {
                "status": 400,
                "code": "invalid_header_identity_for_temporary_access",
                "message": "The identity for temporary access header value is missing or invalid.",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "none"
            }
        }
    ]
}
```

>[!ENDTABS]
