---
title: Apple SSO指南(REST API V1)
description: Apple SSO指南(REST API V1)
exl-id: 072a011f-e1bb-4d3e-bcb5-697f2d1739cc
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '1496'
ht-degree: 0%

---

# （旧版）Apple SSO指南(REST API V1) {#apple-sso-cookbook-rest-api-v1}

>[!IMPORTANT]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

>[!IMPORTANT]
>
> 确保随时了解汇总在[产品公告](/help/authentication/product-announcements.md)页中的最新Adobe Pass身份验证产品公告和停用时间表。

Adobe Pass身份验证REST API V1支持在iOS、iPadOS或tvOS上运行的客户端应用程序的最终用户的合作伙伴单点登录(SSO)。

此文档可用作现有REST API V1文档的扩展，该文档可在[此处](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)找到。

## 指南 {#apple-sso-cookbook-rest-api-v1-cookbook}

为了从Apple SSO用户体验中获益，应用程序需要集成由Apple开发的[视频订阅者帐户框架](https://developer.apple.com/documentation/videosubscriberaccount)，而对于Adobe Pass身份验证REST API V1通信，它需要遵循下面列出的步骤顺序。

### 权限 {#apple-sso-cookbook-rest-api-v1-permission}

>[!TIP]
>
> **<u>专业提示：</u>**&#x200B;流式应用程序必须请求访问保存在设备级别的用户订阅信息，用户必须授予应用程序继续操作的权限，类似于提供对设备摄像头或麦克风的访问权限。 必须使用Apple的[视频订阅者帐户框架](https://developer.apple.com/documentation/videosubscriberaccount)为每个应用程序请求此权限，设备将保存用户的选择。

>[!TIP]
>
> **<u>专业提示：</u>**&#x200B;我们建议通过说明Apple单点登录用户体验的好处，来激励拒绝授予访问订阅信息权限的用户，但请注意，用户可以通过转到应用程序设置（访问电视提供商权限）、转到iOS和iPadOS上的&#x200B;*`Settings -> TV Provider`*&#x200B;或tvOS上的&#x200B;*`Settings -> Accounts -> TV Provider`*&#x200B;来更改其决策。

>[!TIP]
>
> **<u>专业提示：</u>**&#x200B;我们建议在应用程序进入前台状态时请求用户的权限，因为在需要用户身份验证之前，应用程序可以随时检查[访问](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)用户订阅信息的权限。

### 身份验证 {#apple-sso-cookbook-rest-api-v1-authentication}

* [是否存在有效的Adobe身份验证令牌？](#step1)
* [用户是否通过合作伙伴SSO登录？](#step2)
* [获取Adobe配置](#step3)
* [使用Adobe配置启动合作伙伴SSO工作流](#step4)
* [用户登录是否成功？](#step5)
* [从Adobe获取所选MVPD的配置文件请求](#step6)
* [将Adobe请求转发给Partner SSO以获取配置文件](#step7)
* [交换Adobe身份验证令牌的合作伙伴SSO配置文件](#step8)
* [是否已成功生成Adobe令牌？](#step9)
* [启动常规身份验证工作流](#step10)
* [继续授权流程](#step11)

![](../../../assets/rest-api-v1/apple-sso-cookbook-rest-api-v1.png)

#### 步骤：“是否存在有效的Adobe身份验证令牌？” {#step1}

>[!TIP]
>
> **<u>提示：</u>**&#x200B;通过Adobe Pass身份验证[检查身份验证令牌](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md) API服务的媒体实现此功能。

#### 步骤：“用户是否通过合作伙伴SSO登录？” {#step2}

>[!TIP]
>
> **<u>提示：</u>**&#x200B;通过[视频订阅者帐户框架](https://developer.apple.com/documentation/videosubscriberaccount)的媒体实现此功能。

* 应用程序必须检查[权限以访问](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)用户的订阅信息，并且只有在用户允许的情况下才继续。
* 应用程序必须提交[请求](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest)以获取订阅者帐户信息。
* 应用程序必须等待并处理[元数据](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata)信息。

>[!TIP]
>
> **<u>专业提示：</u>**&#x200B;请遵循代码片段并特别注意这些备注。

```swift
...
let videoSubscriberAccountManager: VSAccountManager = VSAccountManager();

videoSubscriberAccountManager.checkAccessStatus(options: [VSCheckAccessOption.prompt: true]) { (accessStatus, error) -> Void in
            switch (accessStatus) {
            // The user allows the application to access subscription information.
            case VSAccountAccessStatus.granted:
                    // Construct the request for subscriber account information.
                    let vsaMetadataRequest: VSAccountMetadataRequest = VSAccountMetadataRequest();

                    // This is actually the SAML Issuer not the channel ID.
                    vsaMetadataRequest.channelIdentifier = "https://saml.sp.auth.adobe.com";
    
                    // This is the subscription account information needed at this step.
                    vsaMetadataRequest.includeAccountProviderIdentifier = true;
                    
                    // This is the subscription account information needed at this step.
                    vsaMetadataRequest.includeAuthenticationExpirationDate = true;
                    
                    // This is going to make the Video Subscriber Account Framework to refrain from prompting the user with the providers picker at this step. 
                    vsaMetadataRequest.isInterruptionAllowed = false;
                    
                    // Submit the request for subscriber account information - accountProviderIdentifier.
                    videoSubscriberAccountManager.enqueue(vsaMetadataRequest) { vsaMetadata, vsaError in        
                        if (vsaMetadata != nil && vsaMetadata!.accountProviderIdentifier != nil) {
                            // The vsaMetadata!.authenticationExpirationDate will contain the expiration date for current authentication session.
                            // The vsaMetadata!.authenticationExpirationDate should be compared against current date.
                            ...
                            // The vsaMetadata!.accountProviderIdentifier will contain the provider identifier as it is known for the platform configuration.
                            // The vsaMetadata!.accountProviderIdentifier represents the platformMappingId in terms of Adobe Pass Authentication configuration.
                            ...
                            // The application must determine the MVPD id property value based on the platformMappingId property value obtained above.
                            // The application must use the MVPD id further in its communication with Adobe Pass Authentication services.
                            ...
                            // Continue with the "Obtain a profile request from Adobe for the selected MVPD" step.
                            ...
                            // Continue with the "Forward the Adobe request to Partner SSO to obtain the profile" step.
                            ...
                        } else {
                            // The user is not authenticated at platform level, continue with the "Fetch Adobe configuration" step.
                            ...
                        }
                    }
        
            // The user has not yet made a choice or does not allow the application to access subscription information.
            default:
                // Continue with the "Initiate regular authentication workflow" step.
                ...
            }
}
...  
```

#### 步骤：“获取Adobe配置” {#step3}

>[!TIP]
>
> **<u>提示：</u>**&#x200B;通过Adobe Pass身份验证[提供MVPD列表](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md) API服务媒体实现此功能。

>[!TIP]
>
> **<u>专业提示：</u>**&#x200B;请了解MVPD属性： *`enablePlatformServices`*、*`boardingStatus`*、*`displayInPlatformPicker`*、*`platformMappingId`*、*`requiredMetadataFields`*，并特别注意其他步骤中代码片段出现的注释。

#### 步骤“使用Adobe配置启动Partner SSO工作流” {#step4}

>[!TIP]
>
> **<u>提示：</u>**&#x200B;通过[视频订阅者帐户框架](https://developer.apple.com/documentation/videosubscriberaccount)的媒体实现此功能。

* 应用程序必须检查[权限以访问](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)用户的订阅信息，并且只有在用户允许的情况下才继续。
* 应用程序必须为VSAccountManager提供[委托](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate)。
* 应用程序必须提交[请求](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest)以获取订阅者帐户信息。
* 应用程序必须等待并处理[元数据](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata)信息。

>[!TIP]
>
> **<u>专业提示：</u>**&#x200B;请遵循代码片段并特别注意这些备注。

```swift
    ...
    let videoSubscriberAccountManager: VSAccountManager = VSAccountManager();
    
    // This must be a class implementing the VSAccountManagerDelegate protocol.
    let videoSubscriberAccountManagerDelegate: VideoSubscriberAccountManagerDelegate = VideoSubscriberAccountManagerDelegate();
    
    videoSubscriberAccountManager.delegate = videoSubscriberAccountManagerDelegate;
    
    videoSubscriberAccountManager.checkAccessStatus(options: [VSCheckAccessOption.prompt: true]) { (accessStatus, error) -> Void in
                switch (accessStatus) {
                // The user allows the application to access subscription information.
                case VSAccountAccessStatus.granted:
                        // Construct the request for subscriber account information.
                        let vsaMetadataRequest: VSAccountMetadataRequest = VSAccountMetadataRequest();
    
                        // This is actually the SAML Issuer not the channel ID.
                        vsaMetadataRequest.channelIdentifier = "https://saml.sp.auth.adobe.com";
        
                        // This is the subscription account information needed at this step.
                        vsaMetadataRequest.includeAccountProviderIdentifier = true;
                        
                        // This is the subscription account information needed at this step.
                        vsaMetadataRequest.includeAuthenticationExpirationDate = true;
                        
                        // This is going to make the Video Subscriber Account Framework to prompt the user with the providers picker at this step. 
                        vsaMetadataRequest.isInterruptionAllowed = true;
                        
                        // This can be computed from the [Adobe Pass Authentication](/help/authentication/provide-mvpd-list.md) service response in order to filter the TV providers from the Apple picker.
                        vsaMetadataRequest.supportedAccountProviderIdentifiers = supportedAccountProviderIdentifiers;
    
                        // This can be computed from the [Adobe Pass Authentication](/help/authentication/provide-mvpd-list.md) service response in order to sort the TV providers from the Apple picker.
                        if #available(iOS 11.0, tvOS 11, *) {
                            vsaMetadataRequest.featuredAccountProviderIdentifiers = featuredAccountProviderIdentifiers;
                        }
                        
                        // Submit the request for subscriber account information - accountProviderIdentifier.
                        videoSubscriberAccountManager.enqueue(vsaMetadataRequest) { vsaMetadata, vsaError in                        
                            // This represents the checks for the "Is user login successful?" step.
                            if (vsaMetadata != nil && vsaMetadata!.accountProviderIdentifier != nil) {
                                // The vsaMetadata!.authenticationExpirationDate will contain the expiration date for current authentication session.
                                // The vsaMetadata!.authenticationExpirationDate should be compared against current date.
                                ...
                                // The vsaMetadata!.accountProviderIdentifier will contain the provider identifier as it is known for the platform configuration.
                                // The vsaMetadata!.accountProviderIdentifier represents the platformMappingId in terms of Adobe Pass Authentication configuration.
                                ...
                                // The application must determine the MVPD id property value based on the platformMappingId property value obtained above.
                                // The application must use the MVPD id further in its communication with Adobe Pass Authentication services.
                                ...
                                // Continue with the "Obtain a profile request from Adobe for the selected MVPD" step.
                                ...
                                // Continue with the "Forward the Adobe request to Partner SSO to obtain the profile" step.
                                ...
                            } else {
                                // The user is not authenticated at platform level.
                                if (vsaError != nil) {
                                    // The application can check to see if the user selected a provider which is present in Apple picker, but the provider is not onboarded in platform SSO.
                                    if let error: NSError = (vsaError! as NSError), error.code == 1, let appleMsoId = error.userInfo["VSErrorInfoKeyUnsupportedProviderIdentifier"] as! String? {
                                        var mvpd: Mvpd? = nil;
    
                                        // The requestor.mvpds must be computed during the "Fetch Adobe configuration" step. 
                                        for provider in requestor.mvpds {
                                            if provider.platformMappingId == appleMsoId {
                                                mvpd = provider;
                                                break;
                                            }
                                        }
                                        
                                        if mvpd != nil {
                                            // Continue with the "Initiate regular authentication workflow" step, but you can skip prompting the user with your MVPD picker and use the mvpd selection, therefore creating a better UX.
                                            ...
                                        } else {
                                            // Continue with the "Initiate regular authentication workflow" step.
                                            ...
                                        }
                                    } else {
                                        // Continue with the "Initiate regular authentication workflow" step.
                                        ...
                                    }
                                } else {
                                    // Continue with the "Initiate regular authentication workflow" step.
                                    ...
                                }
                            }
                        }
            
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                    // Continue with the "Initiate regular authentication workflow" step.
                    ...
                }
    }
    ...
```

#### 步骤：“用户登录是否成功？” {#step5}

>[!TIP]
>
> **<u>专业提示：</u>**&#x200B;请注意[“使用Adobe配置启动合作伙伴SSO工作流”](#step4)步骤中的代码片段。 如果&#x200B;*`vsaMetadata!.accountProviderIdentifier`*&#x200B;包含有效值并且当前日期未传递&#x200B;*`vsaMetadata!.authenticationExpirationDate`*&#x200B;值，则用户登录成功。

#### 步骤“从Adobe获取所选MVPD的配置文件请求” {#step6}

>[!TIP]
>
> **<u>提示：</u>**&#x200B;通过Adobe Pass身份验证[配置文件请求](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-profilerequest.md) API服务的媒体实现此功能。

>[!TIP]
>
> **<u>专业提示：</u>**&#x200B;请注意，从Adobe Pass身份验证配置中获得的提供程序标识符代表&#x200B;*`platformMappingId`*。 因此，应用程序必须使用&#x200B;*`platformMappingId`*&#x200B;值，通过MVPD身份验证[提供Adobe Pass列表](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md) API服务的介质来确定MVPD ID属性值。

#### 步骤：“将Adobe请求转发给Partner SSO以获取配置文件” {#step7}

>[!TIP]
>
> **<u>提示：</u>**&#x200B;通过[视频订阅者帐户框架](https://developer.apple.com/documentation/videosubscriberaccount)的媒体实现此功能。


* 应用程序必须检查[权限以访问](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)用户的订阅信息，并且只有在用户允许的情况下才继续。
* 应用程序必须提交[请求](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest)以获取订阅者帐户信息。
* 应用程序必须等待并处理[元数据](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata)信息。

>[!TIP]
>
> **<u>专业提示：</u>**&#x200B;请遵循代码片段并特别注意这些备注。

```swift
    ...
    let videoSubscriberAccountManager: VSAccountManager = VSAccountManager();
    
    videoSubscriberAccountManager.checkAccessStatus(options: [VSCheckAccessOption.prompt: true]) { (accessStatus, error) -> Void in
                switch (accessStatus) {
                // The user allows the application to access subscription information.
                case VSAccountAccessStatus.granted:
                        // Construct the request for subscriber account information.
                        let vsaMetadataRequest: VSAccountMetadataRequest = VSAccountMetadataRequest();
    
                        // This is actually the SAML Issuer not the channel ID.
                        vsaMetadataRequest.channelIdentifier = "https://saml.sp.auth.adobe.com";
        
                        // This is going to include subscription account information which should match the provider determined in a previous step.
                        vsaMetadataRequest.includeAccountProviderIdentifier = true;
                        
                        // This is going to include subscription account information which should match the provider determined in a previous step.
                        vsaMetadataRequest.includeAuthenticationExpirationDate = true;
                        
                        // This is going to make the Video Subscriber Account Framework to refrain from prompting the user with the providers picker at this step. 
                        vsaMetadataRequest.isInterruptionAllowed = false;
    
                        // This are the user metadata fields expected to be available on a successful login and are determined from the [Adobe Pass Authentication](/help/authentication/provide-mvpd-list.md) service. Look for the requiredMetadataFields associated with the provider determined in a previous step.
                        vsaMetadataRequest.attributeNames = requiredMetadataFields;
    
                        // This is the payload from [Adobe Pass Authentication](/help/authentication/retrieve-profilerequest.md) service.
                        vsaMetadataRequest.verificationToken = profileRequestPayload;
                        
                        // Submit the request for subscriber account information.
                        videoSubscriberAccountManager.enqueue(vsaMetadataRequest) { vsaMetadata, vsaError in
                            if (vsaMetadata != nil && vsaMetadata!.samlAttributeQueryResponse != nil) {
                                var samlResponse: String? = vsaMetadata!.samlAttributeQueryResponse!;
                                
                                // Remove new lines, new tabs and spaces.
                                samlResponse = samlResponse?.replacingOccurrences(of: "[ \\t]+", with: " ", options: String.CompareOptions.regularExpression);
                                samlResponse = samlResponse?.components(separatedBy: CharacterSet.newlines).joined(separator: "");
                                samlResponse = samlResponse?.trimmingCharacters(in: CharacterSet.whitespacesAndNewlines);
                                
                                // Base64 encode.
                                samlResponse = samlResponse?.data(using: .utf8)?.base64EncodedString(options: []);
                                
                                // URL encode. Please be aware not to double URL encode it further.
                                samlResponse = samlResponse?.addingPercentEncoding(withAllowedCharacters: CharacterSet.init(charactersIn: "!*'();:@&=+$,/?%#[]").inverted);
                                
                                // Continue with the "Exchange the Partner SSO profile for an Adobe authentication token" step.
                                ...
                            } else {
                                // Continue with the "Initiate regular authentication workflow" step.
                                ...
                            }
                        }
                        
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                    // Continue with the "Initiate regular authentication workflow" step.
                    ...
                }
    }
    ...
```

#### 步骤：“将Partner SSO配置文件交换为Adobe身份验证令牌” {#step8}

>[!TIP]
>
> **<u>提示：</u>**&#x200B;通过Adobe Pass身份验证[令牌交换](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/token-exchange.md) API服务的媒体实现此功能。

>[!TIP]
>
> **<u>专业提示：</u>**&#x200B;请注意[“将Adobe请求转发给合作伙伴SSO以获取配置文件”](#step7)步骤中的代码片段。 此&#x200B;*`vsaMetadata!.samlAttributeQueryResponse!`*&#x200B;表示需要在[令牌交换](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/token-exchange.md)上传递的&#x200B;*`SAMLResponse`*，它需要在调用之前进行字符串操作和编码（*Base64*&#x200B;编码和&#x200B;*URL*&#x200B;编码）。

#### 步骤：“是否已成功生成Adobe令牌？” {#step9}

>[!TIP]
>
> **<u>提示：</u>**&#x200B;通过Adobe Pass身份验证[令牌交换](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/token-exchange.md)成功响应的媒体实现此目的，响应将为&#x200B;*`204 No Content`*，这表示已成功创建令牌并准备好用于授权流。

#### 步骤：“启动常规身份验证工作流” {#step10}

>[!TIP]
>
> **<u>提示：</u>**&#x200B;通过Adobe Pass身份验证[注册码请求](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md)、[启动身份验证](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md)和[检索身份验证令牌](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md)或[检查身份验证令牌](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md) API服务实现此功能。

>[!TIP]
>
> **<u>专业提示：</u>**&#x200B;请按照以下步骤实施tvOS。

* 应用程序必须[获取注册码](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md)并在第一个设备（屏幕）上将其呈现给最终用户。
* 在获得注册码后，应用程序必须启动[轮询以确认第1台设备（屏幕）上的身份验证状态](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md)。
* 使用注册码时，另一个应用程序必须在第二个设备（屏幕）上[启动身份验证](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md)。
* 生成身份验证令牌时，应用程序必须停止在第1台设备（屏幕）上的[轮询](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md)。

>[!TIP]
>
> **<u>专业提示：</u>**&#x200B;请按照以下步骤实施iOS/iPadOS。

* 应用程序必须[获取不应在第1台设备（屏幕）上呈现给最终用户的注册码](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md)。
* 应用程序必须在第一个设备（屏幕）上使用注册码和[WKWebView](https://developer.apple.com/documentation/webkit/wkwebview)或[SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller)组件[启动身份验证](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md)。
* 在[WKWebView](https://developer.apple.com/documentation/webkit/wkwebview)或[SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller)组件关闭后，应用程序必须启动[轮询以了解第一个设备（屏幕）上的身份验证状态](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md)。
* 生成身份验证令牌时，应用程序必须停止在第1台设备（屏幕）上的[轮询](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md)。

#### 步骤：“继续进行授权流” {#step11}

>[!TIP]
>
> **<u>提示：</u>**&#x200B;通过Adobe Pass身份验证[启动授权](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authorization.md)和[获取短媒体令牌](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md) API服务的媒体实现此功能。

### 注销 {#apple-sso-cookbook-rest-api-v1-logout}

[视频订阅者帐户框架](https://developer.apple.com/documentation/videosubscriberaccount)不提供API以编程方式注销已在设备系统级别登录其电视提供程序帐户的人员。 因此，要完全注销，最终用户必须从iOS/iPadOS上的&#x200B;*`Settings -> TV Provider`*&#x200B;或tvOS上的&#x200B;*`Settings -> Accounts -> TV Provider`*&#x200B;中显式注销。 用户将拥有的另一个选项是从特定应用程序设置部分（TV提供商访问）撤销访问用户订阅信息的权限。

>[!TIP]
>
> **<u>提示：</u>**&#x200B;通过Adobe Pass身份验证[用户元数据调用](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md)和[注销](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md) API服务的媒体实现此功能。

>[!TIP]
>
> **<u>专业提示：</u>**&#x200B;请按照以下步骤实施tvOS。

* 应用程序必须使用Adobe Pass身份验证服务中的&quot;*tokenSource&quot;* [user metadata](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md)，确定是否由于通过合作伙伴SSO登录而发生了身份验证。
* 如果&#x200B;*“tokenSource”*&#x200B;值等于“*Apple”，则应用程序必须指示/提示用户仅在tvOS **上**从&#x200B;*`Settings -> Accounts -> TV Provider`*显式注销。*
* 应用程序必须使用直接HTTP调用从Adobe Pass身份验证服务[启动注销](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md)。 这将无助于MVPD端的会话清理。

>[!TIP]
>
> **<u>专业提示：</u>**&#x200B;请按照以下步骤实施iOS/iPadOS。

* 应用程序必须使用Adobe Pass身份验证服务中的&quot;*tokenSource&quot;* [user metadata](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md)，确定是否由于通过合作伙伴SSO登录而发生了身份验证。
* 如果&#x200B;*“tokenSource”*&#x200B;值等于&#x200B;*“Apple”*，则应用程序必须指示/提示用户仅在iOS/iPadOS **上**&#x200B;从&#x200B;*`Settings -> TV Provider`*&#x200B;显式注销。
* 应用程序必须使用[WKWebView](https://developer.apple.com/documentation/webkit/wkwebview)或[SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller)组件从Adobe Pass身份验证服务[启动注销](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md)。 这将有助于MVPD端的会话清理。
