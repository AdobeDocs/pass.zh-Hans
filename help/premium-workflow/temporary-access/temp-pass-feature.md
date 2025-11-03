---
title: TempPass功能
description: TempPass功能
exl-id: 1df14090-8e71-4e3e-82d8-f441d07c6f64
source-git-commit: e4d243ebf293f3ecc38e532d77116c065a22ebd2
workflow-type: tm+mt
source-wordcount: '2203'
ht-degree: 0%

---

# TempPass功能 {#temp-pass-feature}

>[!IMPORTANT]
>
> 此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

TempPass是一项多功能功能，它使程序员能够为没有有效MVPD帐户凭据的用户提供对其受保护内容的临时访问权限。 无论是通过基本访问方案还是定向促销活动，它都是一个吸引查看者的有效工具。

TempPass是一个功能强大的解决方案，可帮助程序员：

* **吸引查看者：**&#x200B;提供优质内容体验以吸引新订阅者。
* **推动促销活动：**&#x200B;开展有针对性的促销活动以提高内容曝光率并提升品牌忠诚度。
* **保留控制：**&#x200B;根据业务目标管理访问期间、强制限制和重置访问权限。

TempPass功能通过在Adobe Pass身份验证服务器配置中引入伪MVPD（进一步命名为“Temp Pass”）来交付，作为与参与的程序员的集成。 TempPass功能有两种配置可用：

* [基于时间的访问的Basic TempPass](#basic-temp-pass)。
* [促销临时传递](#promotional-temp-pass)，用于灵活地访问促销活动。

>[!IMPORTANT]
>
> TempPass功能是一项高级功能，需要Adobe的当前许可证。

下表简要比较了基本和促销临时传递功能：

| **功能** | **基本TempPass** | **促销临时传递** |
|-------------------------------|------------------------------|-------------------------------------------------------------------------------------------------|
| **访问内容** | <ul><li>基于时间</li></ul> | <ul><li>基于时间</li><li>限制为最大资源数</li></ul> |
| **访问基于**&#x200B;的安全性 | <ul><li>设备ID</li></ul> | <ul><li>设备ID</li><li>提供的用户标识符信息（例如电子邮件）的哈希</li></ul> |
| **增强的错误代码** | 可用 | 可用 |
| **TempPass重置功能** | 可用 | 可用 |

>[!IMPORTANT]
> 
> Adobe Pass身份验证不包括内置机制，用于在分配的时间（X分钟）过后自动停止正在进行的流。 当TempPass在正在进行的流中过期时，程序员有责任实施访问限制。

无论您是提供内容库的偷窥功能，还是促销选框事件，TempPass都能为您提供相应的工具来扩展受众，同时保持对访问权限的控制。

## 基本临时传递 {#basic-temp-pass}

基本的TempPass功能允许程序员根据各种情况提供对内容的有时间限制的访问：

* **简短预览：**&#x200B;提供简短预览，例如10分钟的每日访问时间，以吸引潜在的订阅者。
* **基于事件的访问：**&#x200B;为重大事件（如4小时会话）启用更长的访问权限。
* **组合访问：**&#x200B;混合和匹配持续时间，例如初始的延长查看时间后是几天内的较短的每日预览。

某些事件可能要求分阶段免费访问内容，如初始的延长的免费访问时段（例如，4小时），然后是较短的每日免费访问间隔（例如，每天10分钟）。 要实施此方案，程序员必须与其Adobe代表协调以配置两个根据其需求定制的TempPass MVPD。

例如，要提供初始的4小时免费会话以及随后每日10分钟的免费会话，Adobe可以为程序员配置：

* **TempPass1**：已配置4小时的生存时间(TTL)，以涵盖初始的免费访问期限。
* **TempPass2**：为后续的每日免费访问间隔配置了10分钟的生存时间(TTL)。

为确保日常访问正常运行，必须在每天00:00小时为所有设备重置TempPass2。

### 功能详细信息 {#basic-temp-pass-feature-details}

**配置参数：**

* **TTL（生存时间）：**&#x200B;程序员可以指定访问的持续时间。 无论实际查看时间如何，此基于时钟的TTL都将过期。

**用户标识：**

基本的TempPass功能使用设备标识符作为用户标识参数。

下表可帮助您了解用户识别参数如何影响用户试用体验：

| 设备标识符 | 结果 |
|-------------------|----------------|
| 新建 | 新试用版 |
| 现有 | 现有试用版 |

**查看时间计算：**

TTL表示从初始授权请求时间到到期时间的持续时间，与查看内容所花费的实际时间无关。 每个未来请求根据存储的到期时间检查当前服务器时间以授权访问。

**身份验证：**

Basic TempPass不需要身份验证，允许您直接进入授权步骤。

**授权：**

由于不会与实际MVPD进行交互，因此在TempPass有效的情况下，基本“Temp Pass”MVPD将授权任何资源。 在授权成功的情况下，媒体令牌验证器库仍然适用于验证媒体令牌并确保在启动内容播放之前进行资源验证。

授权决策基于用户标识参数和配置的TTL。 要成功获得对资源的授权，有效的请求必须满足以下条件：

* **未使用的持续时间：**&#x200B;通过将初始授权请求时间（保存在我们的数据库中）添加到配置的TTL来计算过期时间。 将当前服务器时间与此过期时间进行比较，以确定TempPass是否仍然有效。

如果用户超过配置的TTL，他们将无法在同一设备上查看内容，除非重置其TempPass。

**预授权：**

当对Basic &quot;Temp Pass&quot; MVPD发出预授权请求时，响应将返回请求中已成功预授权的完整资源列表。 鉴于授权条件基于时间限制而不是特定资源，此行为反映了授权逻辑。 只要时间限制有效，就会授权请求的资源。

**注销：**

Basic TempPass不需要注销，因此您可以使用实际用户MVPD直接切换到身份验证步骤。

**跟踪数据和分析：**

在基本的TempPass流程中，跟踪数据使用设备标识符的哈希版本，其中MVPD标识符设置为“Temp Pass”。 程序员应在其Analytics实施中将TempPass量度与标准MVPD量度区分开来。

## 促销临时传递 {#promotional-temp-pass}

促销TempPass功能扩展了基本TempPass的功能，专门用于运行促销活动。 通过此功能，程序员可在收集有效用户标识（如电子邮件地址）后的指定时间段内访问预定义的数量的VOD标题，从而吸引用户参与。

促销TempPass包含基本TempPass的所有功能，增加了以下功能的灵活性：

* 定义在促销期间可访问的VOD标题的最大数量。
* 设置促销访问有效的时段。

一旦用户超过预定义的访问限制(VOD标题的数量或持续时间)，他们将无法再在同一设备上或使用同一用户标识符查看内容，除非重置他们的TempPass。

### 功能详细信息 {#promotional-temp-pass-feature-details}

**配置参数：**

* **用户信息密钥：**&#x200B;用于传递用户提供的标识符的密钥，如电子邮件地址（即，密钥是电子邮件）。
* **资源数：**&#x200B;定义用户可以访问的VOD标题数。
* **TTL （生存时间）：**&#x200B;用户可以使用允许的资源的持续时间。

**用户标识：**

促销临时传递功能使用用户提供的标识符的哈希作为用户标识参数，而不是设备标识符。

>[!IMPORTANT]
>
> 用户提供标识符的验证和哈希处理由程序员而非Adobe管理。 Adobe不存储任何个人身份信息(PII)。 因此，在与Adobe Pass身份验证API交互时，程序员负责生成并提交用户提供的唯一标识符的哈希值。

Adobe建议在将数据发送到Adobe之前，对数据使用&#x200B;**SHA-2**&#x200B;系列或其特定的&#x200B;**SHA-256**、**SHA-512**&#x200B;功能。 例如，**&quot;user@domain.com&quot;**&#x200B;上的&#x200B;**SHA-256**&#x200B;是&#x200B;**&quot;f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&quot;**。

下表可帮助您了解用户识别参数如何影响用户试用体验：

| 用户提供的标识符哈希 | 设备标识符 | 结果 |
|-------------------------------|-------------------|---------------------------------------------------------|
| 新建 | 新建 | 新试用版 |
| 现有 | 新建 | 现有试用（基于用户提供的标识符哈希） |
| 新建 | 现有 | 现有试用版（基于设备标识符） |
| 现有 | 现有 | 现有试用版 |

**查看时间计算：**

TTL表示从初始授权请求时间到到期时间的持续时间，与查看内容所花费的实际时间无关。 每个未来请求根据存储的到期时间检查当前服务器时间以授权访问。

**身份验证：**

Promotional TempPass不需要身份验证，允许您直接进入授权步骤。

为了支持程序员应用程序的实施，Promotional TempPass公开了以下用户元数据信息，可通过相应的键访问：

* **`remaining_resources`**：指示用户仍有权使用的资源数。
* **`used_assets`**：提供用户已使用的资源列表。
* **`expiration_date`**：显示用户促销临时传递的到期日期。

**授权：**

由于无法与实际MVPD交互，因此，促销“临时传递”MVPD将授权任何资源，因为TempPass有效。 在授权成功的情况下，媒体令牌验证器库仍然适用于验证媒体令牌并确保在启动内容播放之前进行资源验证。

授权决策基于用户标识参数、配置的资源数和TTL。 要成功获得对资源的授权，有效的请求必须满足以下条件：

* **未使用的持续时间：**&#x200B;通过将初始授权请求时间（保存在我们的数据库中）添加到配置的TTL来计算过期时间。 将当前服务器时间与此过期时间进行比较，以确定TempPass是否仍然有效。
* **未使用的资源：**&#x200B;已跟踪使用的资源数（保存在我们的数据库中）。 消耗的资源数与配置的资源数进行比较，以确定TempPass是否仍然有效。

如果用户超过了配置的TTL或资源数，他们将无法在同一设备上查看内容，也无法使用相同的用户提供的标识符查看内容，除非重置了用户的TempPass。

**预授权：**

当对促销“临时通过”MVPD发出预授权请求时，响应将返回请求中已成功预授权的完整资源列表。 此行为反映了授权逻辑，因为授权条件基于时间限制和访问的资源总数，而不是特定资源。 只要时间限制有效并且未超过资源限制，就会授权请求的资源。

**注销：**

Promotional TempPass不需要注销，因此您可以使用实际用户MVPD直接切换到身份验证步骤。

**跟踪数据和分析：**

在提升TempPass流程期间，跟踪数据使用设备标识符的哈希版本，其中MVPD标识符设置为“Temp Pass”。 程序员应在其Analytics实施中将TempPass量度与标准MVPD量度区分开来。

## 重置TempPass API访问 {#reset-tempass-api-access}

在访问Reset TempPass API之前，您必须完成Dynamic Client Registration (DCR)进程中的所需步骤。 此强制流程可确保您具有与Reset TempPass API交互所需的访问令牌。

有关全面说明，请参阅[动态客户端注册概述](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)文档。

## 重置TempPass API -DELETE/reset-tempass/v3/reset {#reset-tempass-v3-reset}

为了重置某个设备或所有设备的特定TempPass，Adobe Pass身份验证为程序员提供了一个同时适用于基本和提升TempPass的API。

### 请求 {#reset-tempass-v3-reset-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">主机</td>
      <td>mgmt.auth.adobe.com</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">路径</td>
      <td>/reset-tempass/v3/reset</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">方法</td>
      <td>DELETE</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">查询参数</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">requestor_id</td>
      <td>在载入过程中与服务提供商关联的内部唯一标识符。</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">mvpd_id</td>
      <td>在载入流程中与TempPass关联的内部唯一标识符。</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">device_id</td>
      <td>
            此重置操作有效的设备标识符。
            <br/><br/>
            如果未提供值，则重置操作将应用于所有设备。
      </td>
      <td>可选</td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">标头</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">授权</td>
      <td><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md">Retrieve access token</a>文档中介绍了持有者令牌有效负载的生成。</td>
      <td><i>必填</i></td>
   </tr>
</table>

### 响应 {#reset-tempass-v3-reset-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">代码</th>
      <th style="background-color: #EFF2F7;">文本</th>
      <th style="background-color: #EFF2F7;">描述</th>
   </tr>
   <tr>
      <td>204</td>
      <td>无内容</td>
      <td>
        重置成功。
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>错误请求</td>
      <td>
        请求无效，客户端需要更正请求并重试。
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>未授权</td>
      <td>
        访问令牌无效，客户端需要获取新的访问令牌并重试。 有关更多详细信息，请参阅<a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md">动态客户端注册概述</a>文档。
      </td>
   </tr>
   <tr>
      <td>403</td>
      <td>禁止</td>
      <td>
        访问令牌无效，客户端需要获取新的客户端凭据和新的访问令牌，然后重试。 有关更多详细信息，请参阅<a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md">动态客户端注册概述</a>文档。
      </td>
   </tr>
</table>

### 示例 {#reset-tempass-v3-reset-samples}

#### 为特定设备重置TempPass {#reset-tempass-v3-reset-specific-device}

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?requestor_id=REF30&mvpd_id=TempPass&device_id=ba23d141-d715-561c-94f4-e9e4c966b1eb"
```

#### 重置所有设备的TempPass {#reset-tempass-v3-reset-all-devices}

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?requestor_id=REF30&mvpd_id=TempPass&device_id=all"
```

## 重置TempPass API -DELETE/reset-tempass/v3/reset/generic {#reset-tempass-v3-reset-generic}

为了重置通用密钥（用户提供的标识符哈希）或所有密钥的特定TempPass，Adobe Pass身份验证为程序员提供了一个可用于提升TempPass的API。

### 请求 {#reset-tempass-v3-reset-generic-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">主机</td>
      <td>mgmt.auth.adobe.com</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">路径</td>
      <td>/reset-tempass/v3/reset/generic</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">方法</td>
      <td>DELETE</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">查询参数</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">requestor_id</td>
      <td>在载入过程中与服务提供商关联的内部唯一标识符。</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">mvpd_id</td>
      <td>在载入流程中与TempPass关联的内部唯一标识符。</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">键</td>
      <td>
            此重置操作有效的用户提供的标识符哈希。
            <br/><br/>
            如果未提供值，则重置操作将应用于所有用户。
      </td>
      <td>可选</td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">标头</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">授权</td>
      <td><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md">Retrieve access token</a>文档中介绍了持有者令牌有效负载的生成。</td>
      <td><i>必填</i></td>
   </tr>
</table>

### 响应 {#reset-tempass-v3-reset-generic-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">代码</th>
      <th style="background-color: #EFF2F7;">文本</th>
      <th style="background-color: #EFF2F7;">描述</th>
   </tr>
   <tr>
      <td>204</td>
      <td>无内容</td>
      <td>
        重置成功。
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>错误请求</td>
      <td>
        请求无效，客户端需要更正请求并重试。
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>未授权</td>
      <td>
        访问令牌无效，客户端需要获取新的访问令牌并重试。 有关更多详细信息，请参阅<a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md">动态客户端注册概述</a>文档。
      </td>
   </tr>
   <tr>
      <td>403</td>
      <td>禁止</td>
      <td>
        访问令牌无效，客户端需要获取新的客户端凭据和新的访问令牌，然后重试。 有关更多详细信息，请参阅<a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md">动态客户端注册概述</a>文档。
      </td>
   </tr>
</table>

### 示例 {#reset-tempass-v3-reset-generic-samples}

#### 为特定密钥重置TempPass {#reset-tempass-v3-reset-specific-key}

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?requestor_id=REF30&mvpd_id=TempPass&key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7"
```

#### 重置所有密钥的TempPass {#reset-tempass-v3-reset-all-keys}

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?requestor_id=REF30&mvpd_id=TempPass&key=all"
```

## REST API V2 {#rest-api-v2}

使用TempPass功能需要实施代码更新，以修改您的TV Everywhere (TVE)应用程序与Adobe Pass身份验证[REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)的交互方式。

有关这些更新和相关工作流的全面指南，请参阅[临时访问流](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)文档。
