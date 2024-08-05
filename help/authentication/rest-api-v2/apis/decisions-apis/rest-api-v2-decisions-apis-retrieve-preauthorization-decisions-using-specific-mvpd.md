---
title: 使用特定的mvpd检索预授权决策
description: REST API V2 — 使用特定的mvpd检索预授权决策
source-git-commit: c3aa2a24b242669ce0818b95ec34de2adec8001b
workflow-type: tm+mt
source-wordcount: '802'
ht-degree: 1%

---


# 使用特定的mvpd检索预授权决策 {#retrieve-preauthorization-decisions-using-specific-mvpd}

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
      <td>/api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">方法</td>
      <td>POST</td>
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
      <td style="background-color: #DEEBFF;">mvpd</td>
      <td>在新用户引导过程中与身份提供商关联的内部唯一标识符。</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">正文参数</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">资源</td>
      <td>需要MVPD决策才能显示的资源列表。</td>
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
      <td><a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md">AP-Device-Identifier</a>文档中介绍了设备标识符有效负载的生成。</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Device-Info</td>
      <td>
         <a href="../../appendix/headers/rest-api-v2-appendix-headers-x-device-info.md">X-Device-Info</a>文档中介绍了设备信息有效负载的生成。
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
        <a href="../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md">Platform-Subject-Token</a>文档中介绍了为Platform Identity方法生成单点登录有效负载的过程。Adobe
        <br/><br/>
        有关使用平台标识启用单点登录的流的更多详细信息，请参阅<a href="../../flows/single-sign-on-flows/rest-api-v2-single-sign-on-platform-identity-flows.md">使用平台标识流的单点登录</a>文档。
      </td>
      <td>可选</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-Token</td>
      <td>
        <a href="../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md">AD-Service-Token</a>文档中介绍了服务令牌方法单点登录有效负载的生成。
        <br/><br/>
        有关使用服务令牌启用单点登录的流的更多详细信息，请参阅<a href="../../flows/single-sign-on-flows/rest-api-v2-single-sign-on-service-token-flows.md">使用服务令牌流的单点登录</a>文档。
      </td>
      <td>可选</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP — 合作伙伴 — 框架 — 状态</td>
      <td>
        <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md">AP-Partner-Framework-Status</a>文档中介绍了为Partner方法生成单一登录有效负载的过程。
        <br/><br/>
        有关使用合作伙伴启用单点登录流程的更多详细信息，请参阅<a href="../../flows/single-sign-on-flows/rest-api-v2-single-sign-on-partner-flows.md">使用合作伙伴流程进行单点登录</a>文档。</td>
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
        响应正文包含一个决定列表，其中提供了更多信息。
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
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">正文</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">决策</td>
      <td>
         JSON包含元素列表，每个元素均具有以下属性：
         <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 20%;">属性</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7; width: 15%;"></th>
            </tr>
            <tr>
                <td style="background-color: #DEEBFF;">资源</td>
                <td>为其返回预授权决策的资源标识符。</td>
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
                  有关决策源的信息：
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 30%;">值</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">mvpd</td>
                        <td>决策由MVPD预授权端点发出。</td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">降解</td>
                        <td>由于访问降级而发布决策。</td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">临时传递</td>
                        <td>决策因临时访问而发布。</td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">虚拟</td>
                        <td>作为虚拟预授权功能的结果来发布决策。</td>
                     </tr>
                  </table>
               <td><i>必填</i></td>
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

### 1.使用常规mvpd检索预授权决策

>[!BEGINTABS]

>[!TAB 请求]

```JSON
POST /api/v2/REF30/decisions/preauthorize/Cablevision

Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
Accept: application/json
Content-Type: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:       

{
    "resources": ["resource1", "resource2", "resource3"]
}
```

>[!TAB 响应 — 允许和拒绝决策]

```JSON
HTTP/1.1 200 OK 
Content-Type: application/json  

{
   "decisions": [
      {
         "resource": "resource1 ",
         "serviceProvider": "REF30",
         "mvpd": "Cablevision",
         "source": "mvpd",
         "authorized": true
      },
      {
         "resource": "resource2",
         "serviceProvider": "REF30",
         "mvpd": "Cablevision",
         "source": "mvpd",
         "authorized": true
      },
      {
         "resource": "resource3",
         "serviceProvider": "REF30",
         "mvpd": "Cablevision",
         "source": "mvpd",
         "authorized": false,
         "error": {
            "status": 202,
            "code": "preauthorization_denied_by_mvpd",
            "message": "The MVPD has returned a \"Deny\" decision when requesting pre-authorization for the specified resource.",
            "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
            "action": "none"
         }
      }
   ]
}
```

>[!ENDTABS]

### 2.使用降级的mvpd检索预授权决策

>[!BEGINTABS]

>[!TAB 请求]

```JSON
POST /api/v2/REF30/decisions/preauthorize/degradedMvpd

Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
Accept: application/json
Content-Type: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:

{
    "resources": ["REF30", "apasstest1"]
}
```

>[!TAB 响应 — AuthNAll降级]

```JSON
HTTP/1.1 200 OK 
Content-Type: application/json
 
{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "REF30",
            "mvpd": "degradedMvpd",
            "source": "degradation",
            "authorized": true
        }
        {
            "resource": "apasstest1",
            "serviceProvider": "REF30",
            "mvpd": "degradedMvpd",
            "source": "degradation",
            "authorized": true
        }
    ]
}
```

**注意：**&#x200B;在这种情况下，AuthZAll规则具有“渠道”：“apasstest1”

>[!TAB 响应 — AuthZAll降级]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8 
 
{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "REF30",
            "mvpd": "degradedMvpd",
            "source": "degradation",
            "authorized": true
        }
        {
            "resource": "apasstest1",
            "serviceProvider": "REF30",
            "mvpd": "degradedMvpd",
            "source": "degradation",
            "authorized": true
        }
    ]
}
```

**注意：**&#x200B;在这种情况下，AuthZAll规则具有“渠道”：“apasstest1”

>[!TAB 响应 — AuthZNone降级]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8 
 
{
    "decisions": [
        {
            "resource": "REF30",
            "serviceProvider": "REF30",
            "mvpd": "degradedMvpd",
            "authorized": false,
            "error": {
                "status": 200,
                "code": "authorization_denied_by_degradation_rule",
                "message": "The integration has an AuthZNone rule applied for the requested resources",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "none"
            }
        }
        {
            "resource": "apasstest1",
            "serviceProvider": "REF30",
            "mvpd": "degradedMvpd",
            "authorized": false,
            "error": {
                "status": 200,
                "code": "authorization_denied_by_degradation_rule",
                "message": "The integration has an AuthZNone rule applied for the requested resources",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "none"
            }
        }
 
    ]
}
```

**注意：**&#x200B;在这种情况下，AuthZNone规则具有“channel”：“apasstest1”

>[!TAB 响应 — 已过期的降级规则]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8     
 
{
    "decisions": [
        {
            "authorized": false,
            "error": {
                "status": 200,
                "code": "authorization_denied_by_degradation_configuration_change",
                "message": "AuthXAll degradation configuration changed, please try again!",
                "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
                "action": "none"
            }
        }
    ]
}
```

>[!ENDTABS]
