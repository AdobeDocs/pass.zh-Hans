---
title: 用户元数据功能
description: 用户元数据功能
exl-id: 9fd68885-7b3a-4af0-a090-6f1f16efd2a1
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1649'
ht-degree: 0%

---

# 用户元数据 {#user-metadata}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

</br>
</br>

## 介绍 {#intro}

用户元数据功能使程序员能够访问由MVPD维护的不同类型的用户特定数据。  用户元数据类型包括邮政编码、家长评级、用户ID等。  *用户* 元数据是以前可用的的扩展 *静态* 元数据（身份验证令牌TTL、授权令牌TTL和设备ID）。


用户元数据关键点：

- 在身份验证和授权流期间传递到程序员应用程序
- 值将保存在令牌中
- 如果不同的MVPD以不同的格式提供数据，则可以规范化值
- 可以使用程序员的密钥（如邮政编码）对某些参数进行加密
- 通过配置更改，可按Adobe提供特定值

## 获取用户元数据 {#obtaining}

用户元数据可通过AccessEnabler供程序员使用 `getMetadata()` 函数并通过 `/usermetadata` 无客户端API中的端点。  有关使用的详细信息，请参阅平台API文档 `getMetadata()` 及其相应的回调 `setMetadataStatus()` （或用于无客户端API中使用的端点和参数）。

程序员通过为要获取的元数据类型提供键来获取元数据： `getMetadata(key)`.

元数据返回如下： `setMetadataStatus(key, encrypted, data)`：

| 参数 | 类型 | 描述 |
| ----------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `key` | 字符串 | 指定请求的元数据的类型 |
| `encrypted` | 布尔型 | 指示“value”是否已加密的布尔标记。 如果为“true”，则“value”实际上将是实际值的JSON Web加密表示形式。 |
| `data` | 对象 | 包含元数据表示形式的JSON对象 |



数据参数的结构，值在类型之间变化：

| 键 | 值类型 | 示例 | 描述 |
| --- | --- | --- | --- |
| `zip` | JSON数组 | \[&quot;77754&quot;, &quot;12345&quot;\] | 邮政编码 |
| `householdID` | JSON字符串 | “1o7241p” | 家庭标识符。 如果MVPD不支持子帐户，则它与 `userID` |
| `maxRating` | JSON对象 | { MPAA：“NR”， <br>  VCHIP： &quot;X&quot;，  <br>  URL： &quot;http://manage.my/parental&quot; } | 用户的最大家长评级 |
| `userID` | JSON字符串 | “1o7241p” | 用户标识符。 如果MVPD支持子帐户，并且用户不是主帐户， `userID` 将不同 `householdID`. |
| `channelID` | JSON数组 | \[&quot;channel-1&quot;， &quot;channel-2&quot; \] | 用户有权查看的渠道列表 |
| `is_hoh` | JSON字符串 | &quot;1&quot; | 标识用户是否为户主的标志 |
| `encryptedZip` | JSON字符串 | &quot;&quot; | Comcast会公开加密的邮政编码 |
| `typeID` | JSON字符串 | 主要 | 标识用户帐户是否为主/辅助帐户的标志 |
| `primaryOID` | JSON字符串 | “uuidd1e19ec9-012c-124f-b520-acaf118d16a0” | 家庭标识符。 如果 `typeID` 为Primary，将包含 `userID` |
| hba状态 | 布尔型 | &quot;true&quot; &quot;false&quot; | 一个布尔标记，指示是否为特定集成启用基于主页的身份验证 |
| allowMirroring | 布尔型 | &quot;true&quot; &quot;false&quot; | 指示此设备是否允许屏幕镜像 |

>[!NOTE]
>
> **注意：** 如果数据参数已加密，通常情况如下 **zip密钥**，则元数据键的表示形式将为JSON字符串，而不是数组或对象。


**重要提示：** 程序员可用的实际用户元数据取决于MVPD提供的内容。  在生产环境中提供敏感用户元数据（如邮政编码）之前，必须与MVPD签署法律协议。

</br>


| 名称 | 详细信息 | 需要加密 | 评论 |
| --- | --- | --- | --- |
| 用户标识 | 由MVPD提供 | 否 | 此值随后将通过Adobe进行哈希处理，并显示在media令牌和sendTrackingData()回调中。<br><br>此例中的哈希处理是出于历史原因<br><br>此ID可以是家庭ID或子帐户ID。 通常不指定，而是仅与当时使用的登录关联的ID（可以是主要帐户或子帐户） |
| 上游用户Id | 由MVPD提供，仅用于并发监控流 | 否 | 在MVPD和程序员网站和应用程序中强制实施并发限制时，可使用此值。 <br><br>ID也可以包含并发监视策略<br><br>对于大多数MVPD，此值等于用户ID |
| 家庭用户ID | 由MVPD提供，主要用于家长控制流 | 否 | 允许程序员了解家庭与子帐户使用情况的ID。<br><br>如果真实评级不可用，它有时会被用作家长监控的替代项（如果用户使用他们可以查看的家庭帐户登录，否则不会显示分级内容）<br><br>在MVPD中，其表示方式存在很大差异 — 家庭用户ID、户主ID、户主标志等。 |
| 户主 | 指示当前帐户是否为户主的标志 | 否 | 请参阅上文 |
| 类型ID/主ID | 家庭帐户标识符 | 否 | AT&amp;T针对户主的特定指标。<br><br>类型ID =标识用户帐户是否为主/辅助帐户的标志<br><br>主OID =家庭标识符。 如果TypeID为Primary，则将包含userID的值 |
| 最大评级 | 当前帐户允许的最大评级 | 否 | 允许程序员过滤掉不适合帐户的内容。<br><br>具有MPAA或VCHIP评级 |
| 渠道排列 | 用户包中可用的渠道列表 | 否 | 用于从聚合多个网络的入口中快速允许/删除各种通道</br></br> *请注意，印前检查授权通常允许此用法更灵活，应改用* <br><br>OLCA规范允许将此作为AuthN响应中的AttributeStatement |
| HBA状态 | 指示是否已通过HBA进行身份验证 | 否 |     |
| 邮政编码 | 用户的计费邮政编码 | 是 | 用于广播或体育活动<br><br>还可以随AuthZ响应一起提供快速更新<br><br>敏感数据，需要MVPD法律协议 |
| 加密的邮政编码 | 用户的计费邮政编码(Comcast) | 是 | 如上所示，但由Comcast加密 |
| 语言 | 用户语言设置 | 否 | 用于根据用户的偏好显示消息 |
| 允许镜像 | 指示此设备是否允许屏幕镜像 | 否 |     |



## 可用元数据 {#available_metadata}

下表列出了Adobe Pass身份验证生态系统中用户元数据的当前状态：


|     | **法律&#x200B;**<br><br>**协议&#x200B;**<br><br>**已签名（仅限zip）** | **用户标识&#x200B;**<br><br>**在AuthN** | **邮政编码&#x200B;**<br><br>**在AuthN/Z** | **评级&#x200B;**<br><br>**在AuthN/Z** | **家庭&#x200B;**<br><br>**AuthN/Z上的ID** | **AuthN上的渠道ID** | **AuthN上的户主** | **AuthN的类型ID** | **AuthN上的主OID** | 语言 | 上游用户标识 **在AuthN** | HBA状态 | 网络 | inHome | 允许在AuthZ上镜像 | **注释** |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **正式名称** | 不适用 | `userID` | `zip` | `maxRating` | `householdID` | `channelID` | is_hoh | typeID | primaryOID | 语言 | 上游用户ID | hba状态 | onNet | inHome | allowMirroring | 1.对于AuthN — 必须更改OiosamlMetadataParser，以便所有解析器都启用此新属性 <br>2.  对于AuthZ — 没有通用的方式，因为授权实施特定于MVPD |
| **需要加密** | 不适用 | **否** | **是** | **否** | **否** | **否** | **否** | **否** | **否** | **否** | **否** | **否** | **否** | **否** | **否** |     |
| **敏感** | 不适用 | **否** | **是** | **否** | **否** | **否** | **否** | **否** | **否** | **否** | **否** | **否** | **否** | **否** |     |     |
| **AdobeIdP** | **是** | **是** | **是（仅限AuthN）** | **是（仅限AuthN）** | **是（仅限AuthN）** | **是** | **是** | **是** | **是** | **否** | **是** | **否** | **否** | **否** | **否** | 无需法律协议，可以启用。 |
| **Synacor** | **是** | **是** | **是（仅限AuthN）** | **是（仅限AuthN）** | **是（仅限AuthN）** | **是** | **是** | **否** | **否** | **否** | **是** | **否** | **否** | **否** | **否** | **法律协议未涵盖所有代理的MVPD。**   <br>这是对Synacor的通用支持 — 可能未汇总到其所有MVPD。 |
| 碟子 | **否** | **是** | **是（仅限AuthN）** | **是（仅限AuthN）** | **是（仅限AuthN）** | **是** | **否** | **否** | **否** | **否** | **是** | **否** | **否** | **否** | **否** | 与所有Synacor MVPD共享同一列表，外加upstreamUserID。 |
| Comcast | **否** | **是** | **否** | **是（仅限AuthZ）** | **是（仅限AuthZ）** | **否** | **否** | **否** | **否** | **否** | **是** | **是** | **否** | **否** | **否** |     |
| **AT&amp;T** | **是** | **是** | **是（仅限AuthN）** | **否** | **是（仅限AuthN）** | **否** | **否** | **是** | **是** | **否** | **是** | **否** | **否** | **否** | **否** | 签署了法律协议。 |
| **Cablevision** | **是** | **是** | **是（仅限AuthN）** | **否** | **否** | **是** | **否** | **否** | **否** | **否** | **是** | **否** | **否** | **否** | **否** | 签署了法律协议。 |
| **宏达国际电子** | **否** | **是** | **否** | **否** | **否** | **是** | **否** | **否** | **否** | **否** | **是** | **否** | **否** | **否** | **否** |     |
| **代理马西隆** | **是** | **是** | **是（仅限AuthN）** | **否** | **是（仅限AuthN）** | **否** | **否** | **否** | **否** | **否** | **是** | **否** | **否** | **否** | **否** | 签署了法律协议。 |
| **代理Clearleap** | **是** | **是** | **是（仅限AuthN）** | **是（仅限AuthZ）** | **否** | **否** | **否** | **否** | **否** | **是** | **是** | **否** | **否** | **否** | **否** | 签署了法律协议。 |
| 罗杰斯 | **否** | **是** | **否** | **否** | **否** | **否** | **否** | **否** | **否** | **否** | **是** | **否** | **否** | **否** | **否** |     |
| RCN | **是** | **是** | **是（仅限AuthN）** | **是（仅限AuthN）** | **是（仅限AuthN）** | **否** | **否** | **否** | **否** | **否** | **是** | **否** | **否** | **否** | **否** |     |
| Charter | **是** | **是** | **是（仅限AuthN）** | **是（仅限AuthN）** | **是（仅限AuthN）** | **否** | **否** | **否** | **否** | **否** | **是** | **否** | **否** | **否** | **否** |     |
| 威里宗 | **否** | **是** | **是（仅限AuthN）** | **否** | **否** | **否** | **否** | **否** | **否** | **否** | **是** | **是** | **否** | **否** | **否** |     |
| 东链 | **否** | **是** | **是（仅限AuthN）** | **是（仅限AuthN）** | **是（仅限AuthN）** | **是** | **否** | **否** | **否** | **否** | **是** | **否** | **否** | **否** | **否** |     |
| 代理GLDS | **否** | **是** | **是（仅限AuthN）** | **否** | **否** | **否** | **否** | **否** | **否** | **否** | **是** | **否** | **否** | **否** | **否** |     |
| DTV | **是** | **是** | **是（仅限AuthN）** | **否** | **否** | **否** | **否** | **否** | **否** | **否** | **是** | **否** | **否** | **否** | **否** |     |
| COX | **否** | **是** | **是（仅限AuthN）** | **否** | **否** | **否** | **否** | **否** | **否** | **否** | **是** | **否** | **否** | **否** | **否** |     |
| Cogeco | **否** | **是** | **是（仅限AuthN）** | **否** | **是（仅限AuthN）** | **否** | **否** | **否** | **否** | **否** | **是** | **否** | **否** | **否** | **否** |     |
| Videotron | **否** | **是** | **是（仅限AuthN）** | **否** | **是*** | **否** | **否** | **否** | **否** | **否** | **是** | **否** | **否** | **否** | **否** | 显示与userID值相同的家庭ID |
| 频谱 | **是** | **是** | **是（仅限AuthN）** | **是（仅限AuthN）** | **是（仅限AuthN）** | **否** | **否** | **否** | **否** | **否** | **是** | **是** | **否** | **否** | **是** |     |
| **所有其他&#x200B;**<br><br>**MVPDs** | **否** | **是** | **否** | **否** | **否** | **否** | **否** | **否** | **否** | **否** | **是** | **否** | **否** | **否** | **否** | **尚无法律协议，敏感元数据不可用于生产。**  <br>对于所有MVPD，用户ID可用，无需额外工作。 |


随着新类型可用并添加到Adobe Pass身份验证系统中，用户元数据类型的列表将会展开。

## 代码示例 {#code_samples}

- [代码示例1](#code_sample1)
- [代码示例2 （模拟getMetadata）](#code_sample2)


### 代码示例1 {#code_sample1}

```
    // Assuming a reference to the AccessEnabler has been previously obtained and stored in the "ae" variable
     
    ae.setRequestor("SITE");
    ae.checkAuthentication();
     
    function setAuthenticationStatus(status, reason) {
      if(status ==1) {
        // User is authenticated, request metadata
        ae.getMetadata("zip");
        ae.getMetadata("maxRating");
      } else {
        ...
      }
    }
     
    // Implement the setMetadataStatus() callback
    function setMetadataStatus(key, encrypted, data) {
      if(encrypted) {
        // The metadata value is encrypted
        // Needs to be decrypted by the programmer
         data = decrypt(data);
      }
      alert(key + "=" + data);
    }
```


### 代码示例2 （模拟getMetadata） {#code_sample2}

```
    // Assuming a reference to the AccessEnabler has been
    //   previously obtained and stored in the "ae" variable
     
    // Mock the getMetadata() method
    var aeMock = {
        getMetadata: function(key) {
          var data = null;
          // Set mock data based on the received key,
          // according to the format in the spec
          switch(key) {
            case 'zip': 
              data = [ "1235", "23456" ];
              break;
            case 'maxRating': 
              data = { "MPAA": "PG-13", "VCHIP": "TV-14" };
              break;
            default:
              break;
          }
          // Call the metadata status just like AccessEnabler does
          setMetadataStatus(key, false, data);
        }
     }
     
    ae.setRequestor("SITE");
    ae.checkAuthentication();
     
    function setAuthenticationStatus(status, reason) {
      if (status == 1) {
        // User is authenticated, request metadata using mock object
        aeMock.getMetadata("zip");
        aeMock.getMetadata("maxRating");
      } else {
        ...
      }
    }
     
    // Implement the  setMetadataStatus() callback
    function setMetadataStatus(key, encrypted, data) {
      if (encrypted) {
        // The metadata value is encrypted, so it
        //   needs to be decrypted by the programmer
         data = decrypt(data);
      }
      alert(key + "=" + data);
    }
```

<!---

For details on your particular platform, or to gain some insight into how User Metadata is processed on the MVPD side, see the appropriate link in Related Information below.  

## Related Information {#related}

- ActionScript - [getMetadata()](#getMeta), [setMetadataStatus()](#setMetaData)
- JavaScript - [getMetadata()](#getMeta), [setMetadataStatus()](#setMetaData)
- iOS - [getMetadata()](#getMeta), [setMetadataStatus()](#setMetaStatus)
- Android - [getMetadata()](#getMetadata), [setMetadataStatus()](#setMetadaStatus)
- Clientless - [AuthN Metadata](#authn_metadata)
- [MVPD Integration Guide: User Metadata Exchange](/help/authentication/mvpd-user-metadata-exchng.md)
-->
