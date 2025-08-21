---
title: 使用代码检索身份验证会话
description: REST API V2 — 使用代码检索身份验证会话
exl-id: 5cc209eb-ee6b-4bb9-9c04-3444408844b7
source-git-commit: 26245e019afac2c0844ed64b222208cc821f9c6c
workflow-type: tm+mt
source-wordcount: '441'
ht-degree: 2%

---

# 使用代码检索身份验证会话 {#retrieve-authentication-session-using-code}

>[!IMPORTANT]
>
> 此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

>[!IMPORTANT]
>
> REST API V2实施受[限制机制](/help/authentication/integration-guide-programmers/throttling-mechanism.md)文档限制。

>[!MORELIKETHIS]
>
> 确保也访问[REST API V2常见问题解答](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)。

## 请求 {#request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">路径</td>
      <td>/api/v2/{serviceProvider}/sessions/{code}</td>
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
      <td>在载入过程中与服务提供商关联的内部唯一标识符。</td>
      <td><i>必填</i></td>
   </tr>
    <tr>
      <td style="background-color: #DEEBFF;">代码</td>
      <td>在流设备上创建身份验证会话后获得的身份验证代码。</td>
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
      <td style="background-color: #DEEBFF;">AP-Visitor-Identifier</td>
      <td>
        <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-visitor-identifier.md">AP-Visitor-Identifier</a>标头文档中描述了访客标识符有效负载的生成。
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
        响应正文包含有关身份验证会话的信息。
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>错误请求</td>
      <td>
        请求无效，客户端需要更正请求并重试。 响应正文可能包含遵守<a href="../../../../features-standard/error-reporting/enhanced-error-codes.md">增强型错误代码</a>文档的错误信息。
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>未授权</td>
      <td>
        访问令牌无效，客户端需要获取新的访问令牌并重试。 有关更多详细信息，请参阅<a href="../../../rest-api-dcr/dynamic-client-registration-overview.md">动态客户端注册概述</a>文档。
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
        服务器端遇到问题。 响应正文可能包含遵守<a href="../../../../features-standard/error-reporting/enhanced-error-codes.md">增强型错误代码</a>文档的错误信息。
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
      <th style="background-color: #EFF2F7;">正文</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">参数</td>
      <td>
         具有以下属性的JSON对象：
         <ul>
            <li><b>existing</b><br/>已提供的现有参数。</li>
            <li><b>缺失</b><br/>缺少完成身份验证流程所需的参数。</li>
         </ul>
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
      <td>响应正文可能提供附加的错误信息，这些信息将遵循<a href="../../../../features-standard/error-reporting/enhanced-error-codes.md">增强型错误代码</a>文档。</td>
      <td><i>必填</i></td>
   </tr>
</table>

## 示例 {#samples}

### 1.检索身份验证会话，但不缺少参数

>[!BEGINTABS]

>[!TAB 请求]

```HTTPS
GET /api/v2/sessions/REF30/8BLW4RW HTTP/1.1
 
    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB 响应]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{        
    "parameters": {
        "existing": {
            "mvpd": "Cablevision",
            "domain": "adobe.com"
            "redirectUrl": "https://www.adobe.com"        
        }
}
```

>[!ENDTABS]

### 1.检索缺少参数的身份验证会话

>[!BEGINTABS]

>[!TAB 请求]

```HTTPS
GET /api/v2/sessions/REF30/8BLW4RW HTTP/1.1
 
    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB 响应]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{        
    "parameters": {
        "existing": {
            "mvpd": "Cablevision",
            "domain": "adobe.com"
        },
        "missing": ["redirectUrl"]
}
```

>[!ENDTABS]
