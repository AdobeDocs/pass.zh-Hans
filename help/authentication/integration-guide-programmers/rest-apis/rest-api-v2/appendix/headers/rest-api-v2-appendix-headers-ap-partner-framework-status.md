---
title: 标头 — AP-Partner-Framework-Status
description: REST API V2 — 标头 — AP-Partner-Framework-Status
exl-id: f589d948-e23e-43d4-81c2-8db0e7a40e93
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '388'
ht-degree: 0%

---

# 标头 — AP-Partner-Framework-Status {#header-ap-partner-framework-status}

>[!NOTE]
>
> 此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## 概述 {#overview}

<b>AP-Partner-Framework-Status</b>请求标头包含从合作伙伴框架中获取的状态信息，以实现单点登录(SSO)。

## 语法 {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP-Partner-Framework-Status</b>： &lt;partner_framework_status_information&gt;</td>
   </tr>
   <tr>
      <td>标题类型</td>
      <td>请求标头</td>
   </tr>
   <tr>
      <td>标准</td>
      <td>否</td>
   </tr>
</table>

## 指令 {#directives}

<b>&lt;partner_framework_status_information></b>

包含以下属性的JSON元素的`Base64-encoded`值：

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">属性</th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td>frameworkPermissionInfo</td>
      <td>
         这是必需属性。
         <br/><br/>
         合作伙伴框架返回并由应用程序处理的用户权限状态信息。
         <br/><br/>
         这是一个具有以下属性的JSON元素：
         <br/>
         <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 15%;">属性</th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td>访问状态</td>
               <td>
                  这是必需属性。
                  <br/><br/>
                  这是具有以下可能值的枚举：
                  <br/>
                  <ul>
                     <li>granted — 用户允许应用程序访问订阅信息。</li>
                     <li>拒绝 — 用户拒绝应用程序访问订阅信息。</li>
                     <li>挂起 — 用户尚未选择允许应用程序访问订阅信息。</li>
                     <li>notDetermined — 不允许应用程序访问订阅信息。</li>
                  </ul>
               </td>
            </tr>
            <tr>
               <td>错误</td>
               <td>
                  这是一个可选属性。
                  <br/><br/>
                  如果在查询用户权限状态信息时触发了合作伙伴框架错误，则可以使用此信息来传递合作伙伴框架错误。
                  <br/><br/>
                  这是一个具有以下属性的JSON元素：
                  <br/>
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 15%;">属性</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td>代码</td>
                        <td>一个字符串，可唯一标识合作伙伴框架定义的错误。</td>
                     </tr>
                     <tr>
                        <td>message</td>
                        <td>一个字符串，其中包含合作伙伴框架定义的错误描述。</td>
                     </tr>
                  </table>
               </td>
            </tr>
         </table>
      </td>
   </tr>
   <tr>
      <td>frameworkProviderInfo</td>
      <td>
         这是必需属性。
         <br/><br/>
         合作伙伴框架返回并由应用程序处理的提供程序登录状态信息。
         <br/><br/>
         这是一个具有以下属性的JSON元素：
         <br/>
         <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 15%;">属性</th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td>id</td>
               <td>
                  这是必需属性。
                  <br/><br/>
                  这是mappingId，用于标识在合作伙伴框架级别的身份验证流程中使用的MVPD。
               </td>
            </tr>
            <tr>
               <td>过期日期</td>
               <td>
                  这是必需属性。
                  <br/><br/>
                  这是经过身份验证的用户配置文件的过期日期，以防用户已在合作伙伴框架级别使用支持的MVPD成功登录。
               </td>
            </tr>
            <tr>
               <td>错误</td>
               <td>
                  这是一个可选属性。
                  <br/><br/>
                  如果在查询提供程序登录状态信息时触发了合作伙伴框架错误，则可以使用此信息来传递合作伙伴框架错误。
                  <br/><br/>
                  这是一个具有以下属性的JSON元素：
                  <br/>
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 15%;">属性</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td>代码</td>
                        <td>一个字符串，可唯一标识合作伙伴框架定义的错误。</td>
                     </tr>
                     <tr>
                        <td>message</td>
                        <td>一个字符串，其中包含合作伙伴框架定义的错误描述。</td>
                     </tr>
                  </table>
               </td>
            </tr>
         </table>
      </td>
   </tr>
</table>

## 示例 {#examples}

```JSON
// Partner framework status information
// {
//    "frameworkPermissionInfo": {
//        "accessStatus": "....",
//        "error": {
//            "code" : "....",
//            "message" : "...."
//        }
//     },
//    "frameworkProviderInfo" : {
//        "id" : "....",
//        "expirationDate" : "....",
//        "error" : {
//            "code" : "...",
//            "message" : "....."
//        }
//     }
// }  
 
// Base64-encoded
// ewogICAgImZyYW1ld29ya1Blcm1pc3Npb25JbmZvIjogewogICAgICAgICJhY2Nlc3NTdGF0dXMiOiAiLi4uLiIsCiAgICAgICAg
// ImVycm9yIjogewogICAgICAgICAgICAiY29kZSIgOiAiLi4uLiIsCiAgICAgICAgICAgICJtZXNzYWdlIiA6ICIuLi4uIgogICAg
// ICAgIH0KICAgIH0sCiAgICAiZnJhbWV3b3JrUHJvdmlkZXJJbmZvIiA6IHsKICAgICAgICAiaWQiIDogIi4uLi4iLAogICAgICAg
// ICJleHBpcmF0aW9uRGF0ZSIgOiAiLi4uLiIsCiAgICAgICAgImVycm9yIiA6IHsKICAgICAgICAgICAgImNvZGUiIDogIi4uLiIs
// CiAgICAgICAgICAgICJtZXNzYWdlIiA6ICIuLi4uLiIKICAgICAgICB9CiAgICB9Cn0gIA==
 
AP-Partner-Framework-Status: ewogICAgImZyYW1ld29ya1Blcm1pc3Npb25JbmZvIjogewogICAgICAgICJhY2Nlc3NTdGF0dXMiOiAiLi4uLiIsCiAgICAgICAgImVycm9yIjogewogICAgICAgICAgICAiY29kZSIgOiAiLi4uLiIsCiAgICAgICAgICAgICJtZXNzYWdlIiA6ICIuLi4uIgogICAgICAgIH0KICAgIH0sCiAgICAiZnJhbWV3b3JrUHJvdmlkZXJJbmZvIiA6IHsKICAgICAgICAiaWQiIDogIi4uLi4iLAogICAgICAgICJleHBpcmF0aW9uRGF0ZSIgOiAiLi4uLiIsCiAgICAgICAgImVycm9yIiA6IHsKICAgICAgICAgICAgImNvZGUiIDogIi4uLiIsCiAgICAgICAgICAgICJtZXNzYWdlIiA6ICIuLi4uLiIKICAgICAgICB9CiAgICB9Cn0gIA==
```
