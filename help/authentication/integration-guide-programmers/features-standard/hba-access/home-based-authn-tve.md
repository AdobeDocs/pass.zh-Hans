---
title: 适用于所有地区的电视的基于家庭的身份验证
description: 适用于所有地区的电视的基于家庭的身份验证
exl-id: abdc7724-4290-404a-8f93-953662cdc2bc
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1638'
ht-degree: 0%

---

# 适用于所有地区的电视的基于家庭的身份验证

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## 什么是基于主页的身份验证？ {#whatis-home-based-authn}

基于家庭身份的身份验证(HBA)是TV Everywhere的一项功能，它使付费电视订阅者在家时无需输入MVPD凭据即可在线查看电视内容，从而显着改善身份验证流的用户体验。

由开放式身份验证技术委员会(OATC)制定的基于主目录的身份验证定义：“主目录自动身份验证是MVPD/OVD使用主目录网络的特征（或主目录网络上的设备之间自动访问的标识符）来验证哪个订阅者帐户与该主目录网络相关联的过程，这样用户就无需在建立TVE会话时手动输入凭据即可访问TVE保护的内容。”



有关HBA和行业标准的详细信息，请阅读[OATC用例和要求](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/Defining%20TVE%20Home-Based%20Authentication%20(HBA)%20%20Use%20Cases%20and%20Requirements%20Recommended%20Practice%20Version%201_0%20FINAL%20DRAFT%20FOR%20BOARD%20APPROVAL.pdf){target=_blank}文档和&#x200B;**OATC HBA用户体验准则**。

>[!NOTE]
>
>某些HBA流是Premium Workflow包的一部分。 如果您有兴趣使用此功能，请联系您的Adobe Pass销售代表。

## 为什么HBA对您很重要 {#why-hba}

HBA非常重要，因为它实际上消除了您在家中已订阅了电缆的查看器的登录障碍。 此外，基于家庭的身份验证可显着提高查看者的参与度，并为您随时随地观看的电视内容提供更好的用户体验。

目前，几乎一半的登录尝试不成功。

一旦HBA被前5大MVPD之一激活，其身份验证转换率&#x200B;**就提高了40%**（从45%到63%）

![](../../../assets/authn-conv-pre-post.png)

此外，在下文中您还可以看到与不同MVPD集成的通道的登录转换率：已为其启用HBA的通道和没有HBA的通道。 使用HBA的转换率显着高于没有HBA的转换率。

![](../../../assets/hba-vs-non-hba.png)

在为与此MVPD集成的大多数频道启用HBA六个月后，我们注意到独特用户增加了82% （通过此MVPD访问TV Everywhere频道的用户数量几乎翻了一番）。

2w3相反，如以下图表所示，其他未启用HBA的MVPD在过去6个月中的独特用户数仅增加了26%。

![](../../../assets/unique-visitors-incr.png)

从启用HBA前6个月和后6个月收集的数据中，我们发现启用HBA的渠道的查看者参与度大幅增加。 实际上，来自已启用HBA的MVPD的用户所观看的内容通常比来自未启用HBA的MVPD的用户多30%。

![](../../../assets/user-engagement-increase.png)

## Adobe Pass身份验证HBA支持 {#auth-hba-support}

本节介绍Adobe Pass身份验证提供的HBA支持、Adobe Pass身份验证平台在HBA流中的行为，并提供对实施HBA有用的技术详细信息。

支持HBA的Adobe Pass身份验证功能

* 能够为HBA和非HBA身份验证设置不同的身份验证TTL（还需要MVPD支持）
* 能够在身份验证过期时自动选择MVPD（跳过MVPD选取器）。 这在HBA TTL较小的情况下特别有用。
* 能够向程序员公开身份验证是否为HBA（还需要MVPD支持）

### Adobe Pass身份验证平台上的HBA用户体验 {#hba-user-exp}

下表提供了有关启用HBA和不启用HBA时支持的平台的用户体验的信息：

| 用户流量 — 平台类型 | swf、iOS、Android |
|---|---|
| 启用HBA | 用户在家时，会自动进行身份验证。 HBA AuthN令牌过期后，将自动对用户进行重新身份验证。 |
| 不使用HBA | 要求用户选择其MVPD并输入其凭据，即使他们位于主目录。AuthN令牌过期后，用户必须再次输入其凭据。 |

| 用户流量 — 平台类型 | js， Windows（本机） |
|---|---|
| 启用HBA | 用户在家时，会自动进行身份验证。 HBA AuthN令牌过期后，用户必须从选取器中重新选择其MVPD，并将自动进行身份验证。 |
| 不使用HBA | 系统会要求用户选择其MVPD并输入其凭据，即使他们在家也是如此。 AuthN令牌过期后，用户必须再次输入其凭据。 |

| 用户流量 — 平台类型 | 无客户端REST API（第二个屏幕身份验证） |
|---|---|
| 启用HBA | 当用户在家并使用无客户端REST API应用程序时，他们在输入注册码并选择其MVPD后在第二个屏幕设备上自动进行身份验证。 HBA AuthN令牌过期后，将自动对用户进行重新身份验证（在第二个屏幕设备上）。 |
| 不使用HBA | 系统会要求用户选择其MVPD并输入其凭据，即使他们在家也是如此。 AuthN令牌过期后，用户必须再次输入其凭据。 |

### 实施HBA的技术详细信息 {#tech-details-hba}

#### OAuth 2.0协议 {#oauth-2-protocol}

在与OAuth 2.0身份验证协议集成的MVPD的HBA流中，MVPD发出刷新令牌，Adobe发出HBA身份验证令牌：

* 刷新令牌的TTL由MVPD的业务需求决定。
* HBA身份验证令牌TTL **必须小于或等于**&#x200B;刷新令牌TTL。


*OAuth 2.0协议的HBA身份验证流描述*


| 用户操作 | 系统操作 |
|---|---|
| 用户导航到程序员网站。 当尝试播放视频时，将显示MVPD选取器。 用户选择其MVPD并单击登录。 | 进行背景检查。 MVPD应用其规则集进行用户检测(例如，将用户的IP地址映射到分发服务器提供的调制解调器或宽带连接的机顶盒的MAC地址)。 |
| 此时会显示一个持续约3秒的屏幕。 可以显示插播式页面，通知用户正在使用其MVPD帐户自动登录他/她。 | <ol><li>安装在程序员端的AccessEnabler将身份验证请求（作为HTTP请求）发送到Adobe Pass身份验证端点。</li><li>Adobe Pass身份验证端点将请求重定向到MVPD身份验证端点。 <br />**注意：**&#x200B;请求包含`hba_flag`参数（尝试HBA = true），该参数表示MVPD应尝试HBA身份验证。</li><li>MVPD身份验证端点向Adobe Pass身份验证端点发送授权代码。</li><li>Adobe Pass身份验证使用授权代码从MVPD令牌端点请求刷新令牌和访问令牌。</li><li>MVPD在`id_token`中发送身份验证决策和`hba_status` (true/false)参数。</li><li>发送对MVPD用户配置文件端点的调用，以公开用户元数据](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata-feature.md#obtaining)中的[hba_status密钥。</li><li>MVPD将刷新令牌TTL设置为MVPD同意的值，Adobe将AuthN令牌TTL设置为小于或等于刷新令牌的值。</li></ol> |
| 用户已经过身份验证，现在可以浏览授权的TV Everywhere内容。 | 身份验证令牌将传递到用户，用户现在可以成功浏览程序员的网站。 |

#### SAML协议 {#saml-protocol}

SAML身份验证协议的HBA身份验证流的说明

| 用户操作 | 系统操作 |
|---|---|
| 用户导航到程序员网站。 当尝试播放视频时，将显示MVPD选取器。 用户选择其MVPD并单击登录。 | 进行背景检查。 MVPD应用其规则集进行用户检测(例如，将用户的IP地址映射到分发服务器提供的调制解调器或宽带连接的机顶盒的MAC地址)。 |
| 此时会显示一个持续约3秒的屏幕。 可以显示插播式页面，通知用户正在使用其MVPD帐户自动登录他/她。 | <ol><li>安装在程序员端的AccessEnabler将身份验证请求（作为HTTP请求）发送到Adobe Pass身份验证端点。</li><li>Adobe Pass身份验证端点将请求重定向到MVPD身份验证端点。</li><li>MVPD应以SAML响应的形式发送身份验证决定，该响应应包含HBA标志： hba_status (true/false)。</li><li>发送对MVPD用户配置文件端点的调用，以公开用户元数据](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata-feature.md#obtaining)中的[hba_status密钥。</li></ol> |
| 用户已经过身份验证，现在可以浏览授权的TV Everywhere内容。 | 身份验证令牌将传递到用户，用户现在可以成功浏览程序员的网站。 |


## 如何激活HBA {#how-to-activate-hba}

* **OAuth协议：**
   * 有关启用HBA的信息，请参阅[Adobe Pass TVE仪表板用户指南](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md)
* **SAML协议：**基于Home的身份验证已在MVPD端激活。 程序员或Adobe无需执行任何操作。
有关支持基于家庭身份验证的MVPD的详细信息，请参阅MVPD的[HBA状态](/help/authentication/integration-guide-programmers/features-standard/hba-access/hba-status-mvpds.md)。

## 常见问题解答 {#faqs}


**问题：**&#x200B;为什么使用SAML和OAuth2协议分离基于主页的身份验证？

**回答：**&#x200B;这两个协议的HBA流不同。 从程序员的角度来看，无需采取措施来确保为SAML MVPD启用HBA，而对于OAuth2 MVPD，可以在Adobe Pass TVE仪表板中打开或关闭HBA。



**问题：**&#x200B;启用HBA时，用户第一次进行身份验证时是否需要填写用户名和密码？

**答案：**&#x200B;否，不需要用户名和密码。



**问题：**&#x200B;如何强制执行家长监护？

**答案1：** Adobe可以禁用HBA以便与需要家长控制审批的渠道集成。

**回答2：** Adobe正在与OATC合作编写UX文档，该文档建议如何使用家长控制设置HBA体验。



**问题：**&#x200B;支持HBA的提供程序的HBA TTL窗口是否比常规身份验证的更短？

**回答：** TTL设置是可配置的。 我们建议为HBA身份验证令牌设置较短的TTL，以防止错误处理。


## 有用信息 {#useful-info}

* [即时访问(HBA) Recommendations](http://www.ctamtve.com/instantaccess){target=_blank} — 由CTAM提供
* [在程序员应用程序上实现HBA的示例](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/HBA_Flow_Sample.pdf?dc=201604222139-1346){target=_blank} — 按Adobe
  <!--* [Home Based Authentication User Experience Guidelines for TV Everywhere](http://oatc.us/Standards/DownloadRecommendedPractices.aspx){target=_blank} - by OATC-->
* [OATC基于Home的身份验证用例和要求](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/Defining%20TVE%20Home-Based%20Authentication%20(HBA)%20%20Use%20Cases%20and%20Requirements%20Recommended%20Practice%20Version%201_0%20FINAL%20DRAFT%20FOR%20BOARD%20APPROVAL.pdf){target=_blank}
* [基于主目录的身份验证信息图](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/AdobeNewsletterHBA.pdf?dc=201604260953-2640){target=_blank} — 按Adobe
* [使用OAuth 2.0协议进行身份验证](/help/authentication/integration-guide-mvpds/authn-oauth2-protocol.md)
* [使用SAML MVPD进行身份验证](/help/authentication/integration-guide-mvpds/authn-usecase.md)
* [Adobe Pass TVE功能板用户指南](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md)
* [hba_status用户元数据](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata-feature.md#obtaining)
