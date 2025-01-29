---
title: 用户元数据
description: 用户元数据
exl-id: 9fd68885-7b3a-4af0-a090-6f1f16efd2a1
source-git-commit: ae2e61152695b738b0bb08d1dcd81417f3bbdfb5
workflow-type: tm+mt
source-wordcount: '1793'
ht-degree: 0%

---

# 用户元数据 {#user-metadata}

>[!IMPORTANT]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

用户元数据是指由MVPD维护并通过Adobe Pass身份验证[REST API V2](#apis)提供给程序员的用户特定的[属性](#attributes)（例如邮政编码、家长评级、用户ID等）。

用户元数据可用于增强用户的个性化，但也可用于分析。 例如，程序员可能使用用户的邮政编码来传送本地化的新闻或天气更新，或实施家长监控。

当MVPD以不同格式提供数据时，Adobe Pass身份验证会标准化用户元数据值。 此外，对于某些属性（例如，邮政编码），可以使用程序员的证书[加密](#encryption)。

Adobe Pass身份验证使程序员能够查看在其MVPD集成中提供的用户元数据，并[通过[Adobe Pass TVE仪表板](https://experience.adobe.com/#/pass/authentication)管理这些元数据](#management)。

## 用户元数据属性 {#attributes}

下表列出了一些可供程序员使用的用户元数据属性：

| 键 | 类型 | 示例 | 需要加密 | 描述 | 详细信息 |
|------------------|---------|--------------------------------------------------------------|---------------------|------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `userID` | 字符串 | “1o7241p” | 否 | 帐户标识符。 | 属性值可以是家庭标识符或子帐户标识符。 如果MVPD支持子帐户并且当前用户不是主帐户所有者，则`userID`值将与`householdID`值不同。 |
| `upstreamUserID` | 字符串 | “1o7241p” | 否 | 用于并发监控的帐户标识符。 | 属性值可用于在MVPD和程序员网站及应用程序中强制实施并发限制。 对于大多数MVPD，`upstreamUserID`值与`userID`值相同。 |
| `householdID` | 字符串 | “1o7241p” | 否 | 用于家长控制的帐户标识符。 | 属性值可用于区分家庭和子帐户使用情况。 有时，如果无法提供真实评级，则将其用作家长控制的替代项；如果用户使用家庭帐户登录，则可以观看分级内容，否则，不会显示分级内容。 在MVPD中，此值表示方式存在很大差异（例如，家庭用户ID、户主ID、家庭负责人标志等），如果MVPD不支持子帐户，则它将与`userID`相同。 |
| `primaryOID` | 字符串 | “uuidd1e19ec9-012c-124f-b520-acaf118d16a0” | 否 | 帐户标识符。 | 该属性特定于AT&amp;T。当`typeID`值设置为“Primary”时，`primaryOID`值与`userID`值相同。 |
| `typeID` | 字符串 | &quot;Primary&quot; | 否 | 指示当前用户是主帐户持有人还是辅助帐户持有人的属性。 | 该属性特定于AT&amp;T。当`typeID`值设置为“Primary”时，`primaryOID`值与`userID`值相同。 |
| `is_hoh` | 字符串 | &quot;1&quot; | 否 | 指示当前用户是否为户主的属性。 | 该属性特定于Synacor。 |
| `hba_status` | 布尔型 | &quot;true&quot; | 否 | 指示当前用户是否通过HBA进行身份验证的属性。 |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| `allowMirroring` | 布尔型 | &quot;true&quot; | 否 | 指示当前设备是否可以镜像屏幕的属性。 | 该属性特定于频谱。 |
| `zip` | 数组 | \[&quot;77754&quot;， &quot;12345&quot;\] | 是 | 用户的邮政编码。 | 属性值可用于投放本地化的新闻、天气更新或体育赛事。 `zip`值表示需要与MVPD签订法律协议的敏感数据。 加密后，`zip`密钥的表示形式将为`String`，而不是`Array`。 |
| `encryptedZip` | 字符串 | “” | 是 | 用户的加密邮政编码。 | 该属性专用于Comcast。 |
| `channelID` | 数组 | \[&quot;channel-1&quot;， &quot;channel-2&quot;\] | 否 | 用户有权查看的渠道列表。 | 属性值可用于从聚合多个网络的门户过滤各种渠道。 我们建议使用[预授权API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)而不是此用户元数据属性来过滤掉用户不可用的渠道。 |
| `maxRating` | 对象 | { MPAA： &quot;NR&quot;， VCHIP： &quot;X&quot;， URL： &quot;http://manage.my/parental&quot; } | 否 | 当前用户的最大家长分级。 | 属性值可用于根据“MPAA”或“VCHIP”等级筛选不适合于当前用户的内容。 |
| `language` | 字符串 | &quot;English&quot; | 否 | 语言设置。 | 属性值可用于根据用户的语言偏好显示消息。 |

对于程序员可用的用户元数据属性取决于MVPD提供的内容。 下表列出了各种MVPD提供的属性：

|                         | **已签署法律协议（仅限zip）** | **用户ID位于AuthN** | **AuthN的上游用户ID** | **家庭ID位于AuthN/Z** | **AuthN**&#x200B;上的主OID | **AuthN的类型ID** | **户主在AuthN** | **HBA状态** | **允许在AuthZ上镜像** | AuthN/Z上的&#x200B;**邮政编码** | **通道ID位于AuthN** | **AuthN/Z评分** | **语言** | **onNet** | **inHome** | **备注** |
|-------------------------|---------------------------------------|----------------------|-------------------------------|-----------------------------|--------------------------|----------------------|--------------------------------|----------------|------------------------------|-------------------------|-------------------------|-----------------------|--------------|-----------|------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| **正式名称** | 不适用 | `userID` | `upstreamUserID` | `householdID` | `primaryOID` | `typeID` | `is_hoh` | `hba_status` | `allowMirroring` | `zip` | `channelID` | `maxRating` | `language` | `onNet` | `inHome` |                                                                                                                                           |
| **需要加密** | 不适用 | **否** | **否** | **否** | **否** | **否** | **否** | **否** | **否** | **是** | **否** | **否** | **否** | **否** | **否** |                                                                                                                                           |
| **敏感** | 不适用 | **否** | **否** | **否** | **否** | **否** | **否** | **否** | **否** | **是** | **否** | **否** | **否** | **否** | **否** |                                                                                                                                           |
| AdobeIdP | **是** | **是** | **是** | **是（仅限AuthN）** | **是** | **是** | **是** | **否** | **否** | **是（仅限AuthN）** | **是** | **是（仅限AuthN）** | **否** | **否** | **否** | 不需要法律协议。 |
| Synacor | **是** | **是** | **是** | **是（仅限AuthN）** | **否** | **否** | **是** | **否** | **否** | **是（仅限AuthN）** | **是** | **是（仅限AuthN）** | **否** | **否** | **否** | 法律协议未涵盖所有代理的MVPD。 这是对Synacor的通用支持，可能不会汇总到其所有MVPD。 |
| 碟子 | **否** | **是** | **是** | **是（仅限AuthN）** | **否** | **否** | **否** | **否** | **否** | **是（仅限AuthN）** | **是** | **是（仅限AuthN）** | **否** | **否** | **否** | 它与所有Synacor MVPD共享相同的列表，加上`upstreamUserID`。 |
| Comcast | **否** | **是** | **是** | **是（仅限AuthZ）** | **否** | **否** | **否** | **是** | **否** | **否** | **否** | **是（仅限AuthZ）** | **否** | **否** | **否** |                                                                                                                                           |
| AT&amp;T | **是** | **是** | **是** | **是（仅限AuthN）** | **是** | **是** | **否** | **否** | **否** | **是（仅限AuthN）** | **否** | **否** | **否** | **否** | **否** | 签署了法律协议。 |
| DTV | **是** | **是** | **是** | **否** | **否** | **否** | **否** | **否** | **否** | **是（仅限AuthN）** | **否** | **否** | **否** | **否** | **否** |                                                                                                                                           |
| COX | **否** | **是** | **是** | **否** | **否** | **否** | **否** | **否** | **否** | **是（仅限AuthN）** | **否** | **否** | **否** | **否** | **否** |                                                                                                                                           |
| Cablevision | **是** | **是** | **是** | **否** | **否** | **否** | **否** | **否** | **否** | **是（仅限AuthN）** | **是** | **否** | **否** | **否** | **否** | 签署了法律协议。 |
| 频谱 | **是** | **是** | **是** | **是（仅限AuthN）** | **否** | **否** | **否** | **是** | **是** | **是（仅限AuthN）** | **否** | **是（仅限AuthN）** | **否** | **否** | **否** |                                                                                                                                           |
| Charter | **是** | **是** | **是** | **是（仅限AuthN）** | **否** | **否** | **否** | **否** | **否** | **是（仅限AuthN）** | **否** | **是（仅限AuthN）** | **否** | **否** | **否** |                                                                                                                                           |
| 威里宗 | **否** | **是** | **是** | **否** | **否** | **否** | **否** | **是** | **否** | **是（仅限AuthN）** | **否** | **否** | **否** | **否** | **否** |                                                                                                                                           |
| 宏达国际电子 | **否** | **是** | **是** | **否** | **否** | **否** | **否** | **否** | **否** | **否** | **是** | **否** | **否** | **否** | **否** |                                                                                                                                           |
| 罗杰斯 | **否** | **是** | **是** | **否** | **否** | **否** | **否** | **否** | **否** | **否** | **否** | **否** | **否** | **否** | **否** |                                                                                                                                           |
| RCN | **是** | **是** | **是** | **是（仅限AuthN）** | **否** | **否** | **否** | **否** | **否** | **是（仅限AuthN）** | **否** | **是（仅限AuthN）** | **否** | **否** | **否** |                                                                                                                                           |
| 东链 | **否** | **是** | **是** | **是（仅限AuthN）** | **否** | **否** | **否** | **否** | **否** | **是（仅限AuthN）** | **是** | **是（仅限AuthN）** | **否** | **否** | **否** |                                                                                                                                           |
| Cogeco | **否** | **是** | **是** | **是（仅限AuthN）** | **否** | **否** | **否** | **否** | **否** | **是（仅限AuthN）** | **否** | **否** | **否** | **否** | **否** |                                                                                                                                           |
| Videotron | **否** | **是** | **是** | **是*** | **否** | **否** | **否** | **否** | **否** | **是（仅限AuthN）** | **否** | **否** | **否** | **否** | **否** | 它公开与`userID`具有相同值的`householdID`。 |
| 代理马西隆 | **是** | **是** | **是** | **是（仅限AuthN）** | **否** | **否** | **否** | **否** | **否** | **是（仅限AuthN）** | **否** | **否** | **否** | **否** | **否** | 签署了法律协议。 |
| 代理Clearleap | **是** | **是** | **是** | **否** | **否** | **否** | **否** | **否** | **否** | **是（仅限AuthN）** | **否** | **是（仅限AuthZ）** | **是** | **否** | **否** | 签署了法律协议。 |
| 代理GLDS | **否** | **是** | **是** | **否** | **否** | **否** | **否** | **否** | **否** | **是（仅限AuthN）** | **否** | **否** | **否** | **否** | **否** |                                                                                                                                           |
| 其他MVPD | **否** | **是** | **是** | **否** | **否** | **否** | **否** | **否** | **否** | **否** | **否** | **否** | **否** | **否** | **否** | 尚无法律协议，敏感元数据不可用于生产。 对于所有MVPD，`userID`均可用，无需额外工作。 |

>[!IMPORTANT]
>
> 在提供敏感用户元数据（例如，邮政编码）之前，必须与MVPD签署法律协议。

## 用户元数据加密 {#encryption}

若要加密和解密用户元数据属性，程序员需要生成证书（公钥/私钥对）并[通过[Adobe Pass TVE仪表板](https://experience.adobe.com/#/pass/authentication)自行配置](#management)证书或与Adobe Pass身份验证代表共享公钥。

请按照以下步骤操作，以确保正确生成并配置证书：

1. 下载并安装OpenSSL工具包(http://www.openssl.org)。

1. 生成证书签名请求(CSR)：

   * 生成密钥对。 在命令终端上，运行以下命令：

     ```bash
     openssl genrsa -des3 -out mycompany-license.key 2048
     ```

   * 生成CSR。 在命令终端上，运行以下命令：

     ```bash
     openssl req -new -key mycompany-license.key -out mycompany-license.csr -batch
     ```

     系统将提示您输入私钥的密码。

   * 创建私钥和密码的备份副本。 示例CSR：

     ```
     -----BEGIN CERTIFICATE REQUEST-----
     MIIBnTCCAQYCAQAwXTELMAkGA1UEBhMCU0cxETAPBgNVBAoTCE0yQ3J5cHRvMRIw
     EAYDVQQDEwlsb2NhbGhvc3QxJzAlBgkqhkiG9w0BCQEWGGFkbWluQHNlcnZlci5l
     eGFtcGxlLmRvbTCBnzANBgkqhkiG9w0BAQEFAAOBjQAwgYkCgYEAr1nYY1Qrll1r
     uB/FqlCRrr5nvupdIN+3wF7q915tvEQoc74bnu6b8IbbGRMhzdzmvQ4SzFfVEAuM
     MuTHeybPq5th7YDrTNizKKxOBnqE2KYuX9X22A1Kh49soJJFg6kPb9MUgiZBiMlv
     tb7K3CHfgw5WagWnLl8Lb+ccvKZZl+8CAwEAAaAAMA0GCSqGSIb3DQEBBAUAA4GB
     AHpoRp5YS55CZpy+wdigQEwjL/wSluvo+WjtpvP0YoBMJu4VMKeZi405R7o8oEwi
     PdlrrliKNknFmHKIaCKTLRcU59ScA6ADEIWUzqmUzP5Cs6jrSRo3NKfg1bd09D1K
     9rsQkRc9Urv9mRBIsredGnYECNeRaK5R1yzpOowninXC
     -----END CERTIFICATE REQUEST-----
     ```

1. 将CSR发送给证书颁发机构(CA)（例如Verisign）。

1. CA将以.p7b格式（PKCS#7，加密消息语法标准）向您发送证书。

1. 部署.p7b证书。 使用私钥将PKCS#7 (.p7b)文件转换为PKCS#12 （ PFX文件、个人信息交换语法标准），并生成PEM文件（连接的证书容器文件）：

   * 将PKCS#7文件转换为临时PEM文件。 在命令行上，运行以下命令：

     ```
     openssl pkcs7 -in mycompany-license.p7b -inform DER -out mycompany-license-temp.pem -outform PEM -print_certs
     ```

   * 将临时PEM文件转换为PFX文件。 在命令行上，运行以下命令：

     ```
     openssl pkcs12 -export -inkey mycompany-license.key -in mycompany-license-temp.pem -out mycompany-license.pfx -passin pass:private_key_password -passout pass:pfx_password
     ```

   * 将临时PEM文件转换为最终PEM文件。 在命令行上，运行以下命令：

     ```
     openssl x509 -in mycompany-license-temp.pem -inform PEM -out mycompany-license.pem -outform PEM
     ```

1. 使用PEM文件通过[Adobe Pass TVE仪表板](https://experience.adobe.com/#/pass/authentication)配置](#management)证书或将PEM文件发送到Adobe Pass身份验证代表。[

   * 有关如何通过[Adobe Pass TVE仪表板](https://experience.adobe.com/#/pass/authentication)管理证书的更多详细信息，请参阅下一节。

   * Adobe Pass身份验证支持主证书和备份证书。 如果主证书以任何方式遭到破坏，您可以撤销该证书，然后切换到辅助证书。 这将确保在证书之间平稳过渡，对客户的影响最小。

## 用户元数据管理 {#management}

>[!IMPORTANT]
>
> 如果您无权访问Adobe Pass TVE仪表板，请通过我们的[Zendesk](https://adobeprimetime.zendesk.com)创建票证，并请求技术客户经理(TAM)为您进行适当的更改。

Adobe Pass TVE Dashboard是一款用于Adobe Pass身份验证客户（程序员）管理其配置和数据的工具。 此自助仪表板启用了[Adobe Pass TVE仪表板用户指南](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md)文档中描述的一系列功能。

要查看和管理MVPD提供的用户元数据属性，请按照[TVE集成功能板用户指南](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#user-metadata)文档中的步骤操作。

要查看和管理用于加密用户元数据属性的证书，请按照[面向程序员的TVE仪表板用户指南](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#certificates)中的步骤或面向渠道的[TVE仪表板用户指南](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#certificates)文档中的步骤操作。

## REST API V2 {#rest-api-v2}

可以使用以下API检索用户元数据属性：

* [检索用户档案](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [检索特定mvpd的配置文件](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [检索特定代码的配置文件](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

请参阅上述API的&#x200B;**响应**&#x200B;和&#x200B;**示例**&#x200B;部分，了解用户元数据属性的结构。

有关如何以及何时集成上述API的更多详细信息，请参阅以下文档：

* [基本配置文件在主要应用程序中执行](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [基本配置文件在辅助应用程序中执行](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)
