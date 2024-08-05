---
title: 使用代码检索身份验证会话
description: REST API V2 — 使用代码检索身份验证会话
source-git-commit: c3aa2a24b242669ce0818b95ec34de2adec8001b
workflow-type: tm+mt
source-wordcount: '398'
ht-degree: 2%

---


# 使用代码检索身份验证会话 {#retrieve-authentication-session-using-code}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## 请求 {#request}

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
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
      <th style="background-color: #EFF2F7; width: 15%;">路径参数</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
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
      <th style="background-color: #EFF2F7; width: 15%;">标头</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">授权</td>
      <td><a href="../../../dynamic-client-registration-api.md">动态客户端注册</a>文档中描述了持有者令牌有效负载的生成。</td>
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

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 10%;">代码</th>
      <th style="background-color: #EFF2F7; width: 20%;">文本</th>
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
        请求无效，客户端需要更正请求并重试。 响应正文可能包含遵守<a href="../../../enhanced-error-codes.md">增强型错误代码</a>文档的错误信息。
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>未授权</td>
      <td>
        访问令牌无效，客户端需要获取新的访问令牌并重试。 有关更多详细信息，请参阅<a href="../../../dynamic-client-registration-api.md">动态客户端注册</a>文档。
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

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">标头</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">状态</td>
      <td>200</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">正文</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">参数</td>
      <td>
         具有以下属性的JSON对象：
         <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 20%;">属性</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7; width: 15%;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">现有</td>
               <td>已提供的现有参数。</td>
               <td><i>必填</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">缺失</td>
               <td>需要提供以便完成身份验证流程的缺失参数。</td>
               <td><i>必填</i></td>
            </tr>
         </table>
      </td>
      <td><i>必填</i></td>
</table>

### 错误 {#error}

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">标头</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
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
      <th style="background-color: #EFF2F7; width: 15%;">正文</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">错误</td>
      <td>该错误提供了附加信息，这些信息将遵守<a href="../../../enhanced-error-codes.md">增强型错误代码</a>文档。</td>
      <td><i>必填</i></td>
   </tr>
</table>

## 示例 {#samples}

### 1.使用代码检索现有身份验证会话的信息

>[!BEGINTABS]

>[!TAB 请求]

```JSON
GET /api/v2/sessions/REF30/8BLW4RW
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
Accept: application/json
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
```

>[!TAB 响应]

```JSON
HTTP/1.1 200 OK
 
"parameters" : {
    "existing" : {           
         "mvpd" : "Cablevision",
         "domain" : "adobe.com"
    },
    "missing" : ["redirectUrl"]
}
```

>[!ENDTABS]
