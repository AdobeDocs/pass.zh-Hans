---
title: 媒体令牌
description: 媒体令牌
exl-id: 7e486d2c-e078-464d-90b1-14e2cfb4d20a
source-git-commit: e448427ae4a36c4c6cb9f9c1cb4d0cc5c6d564ed
workflow-type: tm+mt
source-wordcount: '653'
ht-degree: 0%

---

# 媒体令牌 {#media-tokens}

>[!IMPORTANT]
>
> 此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

媒体令牌是由Adobe Pass身份验证[REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)生成的令牌，它是授权决策的结果，该决策旨在提供对受保护内容（资源）的查看访问权限。 媒体令牌在发布时指定的有限且较短的时间范围（几分钟）内有效，这表示客户端应用程序必须验证和使用它的时长。

媒体令牌由以明文发送的基于公钥基础设施(PKI)的已签名字符串组成。 使用基于PKI的保护，使用由证书颁发机构(CA)颁发给Adobe的非对称密钥对令牌进行签名。

媒体令牌会传递到程序员，然后程序员可在启动视频流之前使用媒体令牌验证器验证它，以确保该资源访问的安全性。

媒体令牌验证器是由Adobe Pass身份验证分发的库，负责验证媒体令牌的真实性。

## 媒体令牌验证器 {#media-token-verifier}

Adobe Pass身份验证建议程序员将媒体令牌发送到集成媒体令牌验证器库的后端服务，以确保在启动视频流之前安全访问。 媒体令牌的生存时间(TTL)旨在解决令牌生成服务器和验证服务器之间的潜在时钟同步问题。

Adobe Pass身份验证强烈建议不要解析媒体令牌并直接提取其数据，因为不保证令牌格式，并且将来可能会更改。 媒体令牌验证器库应该是唯一用于分析令牌内容的工具。

可以从以下链接下载媒体令牌验证器库：

* https://tve.zendesk.com/hc/en-us/articles/204963159-Media-Token-Verifier-library

媒体令牌验证器库需要JDK版本1.5或更高版本，并且支持为签名算法(`SHA256WithRSA`)使用首选的Java加密扩展(JCE)提供程序。

由`mediatoken-verifier-VERSION.jar` Java存档表示的媒体令牌验证器库包括：

* 公钥Adobe。
* 令牌验证API (`ITokenVerifier.java`)。
* 引用实现(`com.adobe.entitlement.test.EntitlementVerifierTest.java`)。
* 依赖项和证书密钥库。

>[!IMPORTANT]
> 
> 包含的证书密钥库的默认密码为`123456`。

### 方法 {#methods}

`ITokenVerifier`类定义了以下方法：

* 用于验证媒体令牌的`isValid()`方法。 它接受一个参数[资源标识符](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#resource-identifier)。 如果提供的资源标识符为`null`，则该方法将仅验证媒体令牌的真实性和有效期。

  `isValid()`方法返回以下状态值之一：

  | VALID_TOKEN | 令牌验证成功 |
  |----------------------|-------------------------------------------|
  | INVALID_TOKEN_FORMAT | 令牌格式无效 |
  | INVALID_SIGNATURE | 无法验证令牌真实性 |
  | TOKEN_EXPIRED | 令牌TTL无效 |
  | INVALID_RESOURCE_ID | 令牌对给定资源无效 |
  | ERROR_UNKNOWN | 尚未验证令牌 |

* `getResourceID()`方法用于检索与媒体令牌关联的资源标识符，并将其与授权决策响应返回的标识符进行比较。

* 用于检索颁发媒体令牌的时间的`getTimeIssued()`方法。

* `getTimeToLive()`方法用于检索媒体令牌的TTL。

* `getUserSessionGUID()`方法用于检索由MVPD设置的匿名化GUID。

* `getMvpdId()`方法，用于检索对用户进行身份验证的MVPD的标识符。

* `getProxyMvpdId()`方法，用于检索对用户进行身份验证的代理MVPD的标识符。

### 示例 {#sample}

媒体令牌验证器存档包含引用实现(`com.adobe.entitlement.test.EntitlementVerifierTest.java`)和用测试类调用API的示例。 此示例(`com.adobe.entitlement.text.EntitlementVerifierTest.java`)说明了媒体令牌验证器库与媒体服务器的集成。

```JAVA
package com.adobe.entitlement.test;

import com.adobe.entitlement.verifier.CryptoDataHolder;
import com.adobe.entitlement.verifier.ITokenVerifier;
import com.adobe.entitlement.verifier.ITokenVerifierFactory;
import com.adobe.entitlement.verifier.SimpleTokenPKISignatureVerifierFactory;
import com.adobe.tve.crypto.SignatureVerificationCredential; 
import java.io.InputStream; 

public class EntitlementVerifierTest { 
    String mRequestorID = null;
    String mTokenToVerify = null;
    String mPathToCertificate = null;
    String mKeystoreType = null;
    String mKeystorePasswd = null;
    String mResourceID = null;

    public static void main(String[] args) { 
        if (args == null || args.length < 2 ) {
            System.out.println("Incorrect args: Usage: EntitlementVerifierTest requestorID tokenToVerify [resourceID]");
            return;
        } 
        String requestorID = args[0];
        String tokenToVerify = args[1];
        String pathToCertificate = "media_token_keystore.jks"; // the default keystore provided in the entitlement jar 
        String keystoreType = "jks";
        String keystorePasswd = "123456"; // password for the default keystore 
        if (requestorID == null || tokenToVerify == null) {
            System.out.println("One or more arguments is null");
            return;
        } 
        System.out.println("RequestorID: " + requestorID);
        System.out.println("token: " + tokenToVerify);
        System.out.println("cert: " + pathToCertificate);
        System.out.println("keystoretype: " + keystoreType);
        System.out.println("keystore passwd: " + keystorePasswd);
        String resourceID = null;
        if (args.length > 2) {
            resourceID = args[2];
        }
        System.out.println("Resource ID: " + resourceID);
        EntitlementVerifierTest verifier = new EntitlementVerifierTest(requestorID,
            tokenToVerify, pathToCertificate, keystoreType, keystorePasswd, resourceID);
        verifier.verifyToken();
    } 

    protected EntitlementVerifierTest(String inRequestorID,
                                      String inTokenToVerify,
                                      String inPathToCertificate,
                                      String inKeystoreType,
                                      String inKeystorePasswd, String inResourceID) {
        mRequestorID = inRequestorID;
        mTokenToVerify = inTokenToVerify;
        mPathToCertificate = inPathToCertificate;
        mKeystoreType = inKeystoreType;
        mKeystorePasswd = inKeystorePasswd;
        mResourceID = inResourceID;
    } 

    protected void verifyToken() {
        // It is expected that the SignatureVerificationCredential and 
        // CryptoDataHolder could be created at Init time in a web application 
        // and be reused for all token verifications. 
        CryptoDataHolder cryptoData = createCryptoDataHolder(mPathToCertificate, mKeystoreType, mKeystorePasswd);
        ITokenVerifierFactory tokenVerifierFactory = new SimpleTokenPKISignatureVerifierFactory();
        ITokenVerifier tokenVerifier = tokenVerifierFactory.getInstance(mRequestorID, mTokenToVerify, cryptoData);
        ITokenVerifier.eReturnValue status = tokenVerifier.isValid(mResourceID);
        System.out.println("Is token Valid? : " + status.toString());
        System.out.println("Token User ID: " + tokenVerifier.getUserSessionGUID());
        System.out.println("Token was generated at: " + tokenVerifier.getTimeIssued());

        System.out.println("Token Mvpd ID: " + tokenVerifier.getMvpdId());
        System.out.println("Token Proxy Mvpd ID: " + tokenVerifier.getProxyMvpdId());
    } 
    
    protected CryptoDataHolder createCryptoDataHolder(String pathToCertificate,
                                                      String keystoreType, String keystorePasswd) {
        SignatureVerificationCredential verificationCredential =
            readShortTokenVerificationCredential(pathToCertificate, keystoreType, keystorePasswd);
        CryptoDataHolder cryptoData = new CryptoDataHolder();
        cryptoData.setCertificateInfo(verificationCredential);
        return cryptoData;
    } 
    
    protected SignatureVerificationCredential readShortTokenVerificationCredential(String keystoreFile,
                                                                                   String keystoreType,
                                                                                   String keystorePasswd) {
        SignatureVerificationCredential cred = null; 
        if (keystoreFile != null){
            try {
                // load the keystore file 
                ClassLoader loader = EntitlementVerifierTest.class.getClassLoader();
                InputStream certInputStream =  loader.getResourceAsStream(keystoreFile);
                if (certInputStream != null) {
                    cred = new SignatureVerificationCredential(certInputStream, keystorePasswd, keystoreType);          
                }
            }
            catch (Exception e) {
                System.out.println("Error creating short token server credentials: " + e.getMessage());
            }
        }
        if (cred == null) {
            System.out.println("Error creating short token server credentials");
        } 
        return cred;
    } 
}
```

## REST API V2 {#rest-api-v2}

可以使用以下API检索媒体令牌：

* [使用特定的mvpd检索授权决策](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

请参阅上述API的&#x200B;**响应**&#x200B;和&#x200B;**示例**&#x200B;部分，以了解授权决策和媒体令牌的结构。

有关如何以及何时集成上述API的更多详细信息，请参阅以下文档：

* [在主应用程序中执行的基本授权流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

>[!IMPORTANT]
>
> 客户端应用程序必须将返回的`token`中的`serializedToken`值传递给[媒体令牌验证器](#media-token-verifier)进行验证。
