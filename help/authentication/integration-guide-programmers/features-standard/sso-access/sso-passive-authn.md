---
title: 通过被动身份验证进行SSO
description: 通过被动身份验证进行SSO
exl-id: ce45899f-6e94-4bb0-a2c1-51f03bd66d8d
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '778'
ht-degree: 0%

---

# 通过被动身份验证进行SSO

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。


## 简介

本文档的范围是介绍被动身份验证流程的实现，以及它如何与我们的标准单点登录方法配合使用。

## 用例

Adobe Pass身份验证在应用程序/站点之间启用单点登录(SSO)。 用户使用其MVPD凭据登录后，Adobe Pass身份验证会生成一个表示MVPD身份验证会话的安全令牌，并使用设备ID将该令牌绑定到用户的设备。 Adobe Pass身份验证将令牌/设备ID存储在服务器或设备上。

只要令牌仍然有效，用户就会直接显示为已通过身份验证。 这样，用户就可以更少地输入其凭据，同时确保事务安全。



此处详述的业务用例是一项非常具体的要求：必须针对每个访问的站点至少对用户进行一次身份验证。 这使得MVPD能够应用与身份验证会话相关联的业务规则，这些规则可能因每个网络而异。 它与当前的TVE承诺冲突，当前TVE承诺用户只需登录一次，并且将在Adobe Pass身份验证生态系统所包含的所有网站上进行身份验证。



为了维护业务规则同时保持良好的用户体验， MVPD不要求用户手动提供凭据信息。 我们可以从之前设置的会话Cookie中获益，并尝试使用被动流进行自动重新身份验证；从用户的角度来看，他将显示为自动登录。



为了解决这些问题，我们实现了两种不同的功能：每个网络的身份验证和被动身份验证支持。 MVPD将支持SAML被动authN，在这种情况下，如果IdP上存在authN会话，则无论该会话是在哪个网站上创建的，MVPD都只是重新验证用户。



## 每个网络的身份验证

此功能可确保MVPD会收到每个访问站点的身份验证请求。 此功能意味着Adobe Pass身份验证令牌将绑定到请求者ID，因此仅适用于请求它的网络。 因此，一旦用户在网站“A”上进行身份验证，并随后访问网站“B”，则需要对其进行身份验证。



请注意，由于在MVPD IdP上用户已经过身份验证，因此将不需要他提供登录信息，而是只需将浏览器从站点“B”重定向到MVPD IdP，然后再重定向回来。 如果同一用户返回，则仍将在网站“A”上进行身份验证。



以下流程描述了“每个网络的基本身份验证”功能：





## 被动身份验证

这样做的目的是使重新身份验证过程在后台进行，而无需完全的浏览器重定向和显示选取器。 因此，从站点A移动到站点B的用户将会自动进行身份验证。



从UX的角度来看，此流与使用常规MVPD执行的流之间没有区别。 用户看到的是，在因访问站点A而输入凭据后，它将在站点B上自动进行身份验证。



为了使此流程可行，MVPD将需要支持被动身份验证，以便在MVPD不再具有会话的情况下，隐藏的iframe不会在登录页面上“卡住”。 这是通过标准的“isPassive”属性来完成的。



下图描述了改进的流程和“后台”被动身份验证：





SAML请求示例
以下是被动authN流的SAML请求示例：


```xml
<saml2p:AuthnRequest xmlns:saml2p="urn:oasis:names:tc:SAML:2.0:protocol"
                     AssertionConsumerServiceURL="https://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer"
                     Destination="https://mvpd_idp_url"
                     ForceAuthn="false"
                     ID="_15056686-399c-4528-b21a-4a9542cfc8ec"
                     IsPassive="true"
                     IssueInstant="2014-11-03T14:18:12.394Z"
                     ProtocolBinding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST"
                     Version="2.0"
                     >
    <saml2:Issuer xmlns:saml2="urn:oasis:names:tc:SAML:2.0:assertion">https://saml.sp.auth.adobe.com </saml2:Issuer>
    <saml2p:Extensions>
        <thrpty:RespondTo xmlns:thrpty="urn:oasis:names:tc:SAML:protocol:ext:third-party">https://saml.sp.auth.adobe.com</thrpty:RespondTo>
    </saml2p:Extensions>
    <saml2p:NameIDPolicy AllowCreate="true"
                         Format="urn:oasis:names:tc:SAML:2.0:nameid-format:transient"
                         SPNameQualifier="https://saml.sp.auth.adobe.com"
                         />
</saml2p:AuthnRequest>
```

## 业务规则

MVPD具有特定的SSO范围域限制。 例如，某些MVPD只允许使用某些域（对于同一媒体公司使用SSO，但不允许跨公司使用）。
某些MVPD可能需要实施不同的身份验证规则。 例如，MVPD在不同的网络中可能有不同的身份验证TTL。 此外，MVPD可能会为某些网络启用基于家庭的身份验证，但不会为其他网络启用基于家庭的身份验证（此处强烈演示了家长控制用例）。


这些业务要求应牢记，主要用例是成功使用用户的MVPD登录后，不应要求用户再次登录。

通过使用带有被动身份验证标志的每个网络来进行身份验证，可以做到这一点。



## 已知限制

iOS — 由于iOS本地存储的性质，SSO流程不适用于由不同供应商开发的应用程序的iOS。 有关iOS 8及更高版本中SSO的更多信息，请参阅此技术说明。


<!--
>[!RELATEDINFORMATION]
>* Single Sign-On on iOS
>* SSO on iOS when using the Adobe Pass Authentication Access Enabler
-->
