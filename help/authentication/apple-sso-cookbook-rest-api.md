---
title: Apple SSO指南(REST API)
description: Apple SSO指南(REST API)
exl-id: cb27c4b7-bdb4-44a3-8f84-c522a953426f
source-git-commit: 1b8371a314488335c68c82882c930b7c19aa64ad
workflow-type: tm+mt
source-wordcount: '1344'
ht-degree: 0%

---

# Apple SSO指南(REST API) {#apple-sso-cookbook-rest-api}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## 简介 {#Introduction}

Adobe Pass身份验证REST API可以通过我们所说的Apple SSO工作流程，为在iOS、iPadOS或tvOS上运行的客户端应用程序的最终用户支持平台单点登录(SSO)身份验证。

请注意，此文档可用作现有REST API文档的扩展，该文档可在[此处](/help/authentication/rest-api-reference.md)找到。

## 指南 {#Cookbooks}

为了从Apple SSO用户体验中获益，一个应用程序需要集成由Apple开发的[视频订阅者帐户](https://developer.apple.com/documentation/videosubscriberaccount)框架，而对于Adobe Pass身份验证REST API通信，它将必须遵循下面介绍的提示顺序。

### 身份验证 {#Authentication}

- [是否存在有效的Adobe身份验证令牌？](#Is_there_a_valid_Adobe_authentication_token)
- [用户是否通过Platform SSO登录？](#Is_the_user_logged_in_via_Platform_SSO)
- [获取Adobe配置](#Fetch_Adobe_configuration)
- [使用Adobe配置启动平台SSO工作流](#Initiate_Platform_SSO_workflow_with_Adobe_config)
- [用户登录是否成功？](#Is_user_login_successful)
- [从Adobe获取所选MVPD的配置文件请求](#Obtain_a_profile_request_from_Adobe_for_the_selected_MVPD)
- [将Adobe请求转发给Platform SSO以获取配置文件](#Forward_the_Adobe_request_to_Platform_SSO_to_obtain_the_profile)
- [交换Adobe身份验证令牌的Platform SSO配置文件](#Exchange_the_Platform_SSO_profile_for_an_Adobe_authentication_token)
- [是否已成功生成Adobe令牌？](#Is_Adobe_token_generated_successfully)
- [启动第二个屏幕身份验证工作流](#Initiate_second_screen_authentication_workflow)
- [继续授权流程](#Proceed_with_authorization_flows)


![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/qu/platform-sso.jpeg)


#### 步骤：“是否存在有效的Adobe身份验证令牌？” {#Is_there_a_valid_Adobe_authentication_token}

>[!TIP]
>
> **<u>提示：</u>**&#x200B;通过[Adobe Pass身份验证](/help/authentication/check-authentication-token.md)服务的媒体实现此功能。


#### 步骤：“用户是否通过Platform SSO登录？” {#Is_the_user_logged_in_via_Platform_SSO}

>[!TIP]
>
> **<u>提示：</u>**&#x200B;通过[视频订阅者帐户](https://developer.apple.com/documentation/videosubscriberaccount)框架实现此功能。

- 应用程序必须检查[权限以访问](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)用户的订阅信息，并且只有在用户允许的情况下才继续。
- 应用程序必须提交[请求](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest)以获取订阅者帐户信息。
- 应用程序必须等待并处理[元数据](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata)信息。



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
                    
                    // This is going to make the Video Subscriber Account framework to refrain from prompting the user with the providers picker at this step. 
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
                            // Continue with the "Forward the Adobe request to Platform SSO to obtain the profile" step.
                            ...
                        } else {
                            // The user is not authenticated at platform level, continue with the "Fetch Adobe configuration" step.
                            ...
                        }
                    }
        
            // The user has not yet made a choice or does not allow the application to access subscription information.
            default:
                // Fallback to regular authentication workflow.
                ...
            }
}
...  
```

#### 步骤：“获取Adobe配置” {#Fetch_Adobe_configuration}

>[!TIP]
>
> **<u>提示：</u>**&#x200B;通过[Adobe Pass身份验证](/help/authentication/provide-mvpd-list.md)服务的媒体实现此功能。


>[!TIP]
>
> **<u>专业提示：</u>**&#x200B;请了解MVPD属性： *`enablePlatformServices`*、*`boardingStatus`*、*`displayInPlatformPicker`*、*`platformMappingId`*、*`requiredMetadataFields`*，并特别注意其他步骤中代码片段出现的注释。

#### 步骤“使用Adobe配置启动平台SSO工作流” {#Initiate_Platform_SSO_workflow_with_Adobe_config}

>[!TIP]
>
> **<u>提示：</u>**&#x200B;通过[视频订阅者帐户](https://developer.apple.com/documentation/videosubscriberaccount)框架实现此功能。

- 应用程序必须检查[权限以访问](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)用户的订阅信息，并且只有在用户允许的情况下才继续。
- 应用程序必须为VSAccountManager提供[委托](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate)。
- 应用程序必须提交[请求](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest)以获取订阅者帐户信息。
- 应用程序必须等待并处理[元数据](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata)信息。



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
                        
                        // This is going to make the Video Subscriber Account framework to prompt the user with the providers picker at this step. 
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
                                // Continue with the "Forward the Adobe request to Platform SSO to obtain the profile" step.
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
                                            // Continue with the "Initiate second screen authentcation workflow" step, but you can skip prompting the user with your MVPD picker and use the mvpd selection, therefore creating a better UX.
                                            ...
                                        } else {
                                            // Continue with the "Initiate second screen authentcation workflow" step.
                                            ...
                                        }
                                    } else {
                                        // Continue with the "Initiate second screen authentcation workflow" step.
                                        ...
                                    }
                                } else {
                                    // Continue with the "Initiate second screen authentcation workflow" step.
                                    ...
                                }
                            }
                        }
            
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                    // Fallback to regular authentication workflow.
                    ...
                }
    }
    ...
```

</br>

#### 步骤：“用户登录是否成功？” {#Is_user_login_successful}

>[!TIP]
>
> **<u>专业提示：</u>**&#x200B;请注意[“使用Adobe配置启动平台SSO工作流”](#Initiate_Platform_SSO_workflow_with_Adobe_config)步骤中的代码片段。 如果&#x200B;*`vsaMetadata!.accountProviderIdentifier`*&#x200B;包含有效值并且当前日期未传递&#x200B;*`vsaMetadata!.authenticationExpirationDate`*&#x200B;值，则用户登录成功。

#### 步骤“从Adobe获取所选MVPD的配置文件请求” {#Obtain_a_profile_request_from_Adobe_for_the_selected_MVPD}

>[!TIP]
>
> **<u>提示：</u>**&#x200B;通过Adobe Pass身份验证[配置文件请求](/help/authentication/retrieve-profilerequest.md)服务的媒体实现此功能。

>[!TIP]
>
> **<u>专业提示：</u>**&#x200B;请注意，从Adobe Pass身份验证配置中获得的提供程序标识符代表&#x200B;*`platformMappingId`*。 因此，应用程序必须通过Adobe Pass身份验证[提供MVPD列表](/help/authentication/provide-mvpd-list.md)服务的介质使用&#x200B;*`platformMappingId`*&#x200B;值确定MVPD ID属性值。

#### 步骤：“将Adobe请求转发给Platform SSO以获取配置文件” {#Forward_the_Adobe_request_to_Platform_SSO_to_obtain_the_profile}

>[!TIP]
>
> **<u>提示：</u>**&#x200B;通过[视频订阅者帐户](https://developer.apple.com/documentation/videosubscriberaccount)框架实现此功能。


- 应用程序必须检查[权限以访问](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)用户的订阅信息，并且只有在用户允许的情况下才继续。
- 应用程序必须提交[请求](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest)以获取订阅者帐户信息。
- 应用程序必须等待并处理[元数据](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata)信息。



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
                        
                        // This is going to make the Video Subscriber Account framework to refrain from prompting the user with the providers picker at this step. 
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
                                
                                // Continue with the "Exchange the Platform SSO profile for an Adobe authentication token" step.
                                ...
                            } else {
                                // Fallback to regular authentication workflow.
                                ...
                            }
                        }
                        
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                    // Fallback to regular authentication workflow.
                    ...
                }
    }
    ...
```

</br>

#### 步骤：“将Platform SSO配置文件交换为Adobe身份验证令牌” {#Exchange_the_Platform_SSO_profile_for_an_Adobe_authentication_token}

>[!TIP]
>
> **<u>提示：</u>**&#x200B;通过Adobe Pass身份验证[令牌交换](/help/authentication/token-exchange.md)服务实现此功能。


>[!TIP]
>
> **<u>专业提示：</u>**&#x200B;请注意[“将Adobe请求转发到Platform SSO以获取配置文件”](#Forward_the_Adobe_request_to_Platform_SSO_to_obtain_the_profile)步骤中的代码片段。 此&#x200B;*`vsaMetadata!.samlAttributeQueryResponse!`*&#x200B;表示需要在[令牌交换](/help/authentication/token-exchange.md)上传递并需要字符串操作和编码（*Base64*&#x200B;编码后和&#x200B;*URL*&#x200B;编码）的&#x200B;*`SAMLResponse`*，然后再进行调用。

</br>

#### 步骤：“是否已成功生成Adobe令牌？” {#Is_Adobe_token_generated_successfully}

>[!TIP]
>
> **<u>提示：</u>**&#x200B;通过媒介Adobe Pass身份验证[令牌交换](/help/authentication/token-exchange.md)成功响应实现此目的，响应将为&#x200B;*`204 No Content`*，指示令牌已成功创建并准备好用于授权流。

</br>

#### 步骤：“启动第二个屏幕身份验证工作流” {#Initiate_second_screen_authentication_workflow}

**重要提示：**“第二屏幕身份验证工作流”术语适用于AppleTV，而“第一屏幕身份验证工作流”/“常规身份验证工作流”术语更适用于iPhone和iPad。


>[!TIP]
>
> **<u>提示：</u>**&#x200B;通过Adobe Pass身份验证媒体实现此功能

[注册代码请求](/help/authentication/registration-code-request.md)、[启动身份验证](/help/authentication/initiate-authentication.md)和[REST API检索身份验证令牌](/help/authentication/retrieve-authentication-token.md)或[检查身份验证令牌](/help/authentication/check-authentication-token.md)服务。


>[!TIP]
>
> **<u>专业提示：</u>**&#x200B;请按照以下步骤实施tvOS。

- 应用程序必须[获取注册码](/help/authentication/registration-code-request.md)并在第一个设备（屏幕）上将其呈现给最终用户。
- 在获得注册码后，应用程序必须启动[轮询以确认第1台设备（屏幕）上的身份验证状态](/help/authentication/retrieve-authentication-token.md)。
- 使用注册码时，另一个应用程序必须在第二个设备（屏幕）上[启动身份验证](/help/authentication/initiate-authentication.md)。
- 生成身份验证令牌时，应用程序必须停止在第1台设备（屏幕）上的[轮询](/help/authentication/retrieve-authentication-token.md)。



>[!TIP]
>
> **<u>专业提示：</u>**&#x200B;请按照以下步骤实施iOS/iPadOS。

- 应用程序必须[获取不应在第1台设备（屏幕）上呈现给最终用户的注册码](/help/authentication/registration-code-request.md)。
- 应用程序必须在第一个设备（屏幕）上使用注册码和[WKWebView](https://developer.apple.com/documentation/webkit/wkwebview)或[SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller)组件[启动身份验证](/help/authentication/initiate-authentication.md)。
- 在[WKWebView](https://developer.apple.com/documentation/webkit/wkwebview)或[SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller)组件关闭后，应用程序必须启动[轮询以了解第一个设备（屏幕）上的身份验证状态](/help/authentication/retrieve-authentication-token.md)。
- 生成身份验证令牌时，应用程序必须停止在第1台设备（屏幕）上的[轮询](/help/authentication/retrieve-authentication-token.md)。

</br>

#### 步骤：“继续进行授权流” {#Proceed_with_authorization_flows}

>[!TIP]
>
> **<u>提示：</u>**&#x200B;通过Adobe Pass身份验证[启动授权](/help/authentication/initiate-authorization.md)和[获取短媒体令牌](/help/authentication/obtain-short-media-token.md)服务实现此功能。

</br>

### 注销 {#Logout}

[视频订阅者帐户](https://developer.apple.com/documentation/videosubscriberaccount)框架不提供API以编程方式注销已在设备系统级别登录其电视提供程序帐户的人员。 因此，要完全注销，最终用户必须从iOS/iPadOS上的&#x200B;*`Settings -> TV Provider`*&#x200B;或tvOS上的&#x200B;*`Settings -> Accounts -> TV Provider`*&#x200B;中显式注销。 用户将拥有的另一个选项是从特定应用程序设置部分（TV提供商访问）撤销访问用户订阅信息的权限。

>[!TIP]
>
> **<u>提示：</u>**&#x200B;通过Adobe Pass身份验证[用户元数据调用](/help/authentication/user-metadata.md)和[注销](/help/authentication/initiate-logout.md)服务的媒体实现此功能。


>[!TIP]
>
> **<u>专业提示：</u>**&#x200B;请按照以下步骤实施tvOS。


- 应用程序必须使用Adobe Pass身份验证服务中的&quot;*tokenSource&quot;* [user metadata](/help/authentication/user-metadata.md)，确定是否由于通过平台SSO登录而发生了身份验证。
- 如果&#x200B;*“tokenSource”*&#x200B;值等于“*Apple”，则应用程序必须指示/提示用户仅在tvOS **上**从&#x200B;*`Settings -> Accounts -> TV Provider`*显式注销。*
- 应用程序必须使用直接HTTP调用从Adobe Pass身份验证服务[启动注销](/help/authentication/initiate-logout.md)。 这无助于MVPD端的会话清理。



>[!TIP]
>
> **<u>专业提示：</u>**&#x200B;请按照以下步骤实施iOS/iPadOS。

- 应用程序必须使用Adobe Pass身份验证服务中的&quot;*tokenSource&quot;* [用户元数据](/help/authentication/user-metadata.md)，确定是否由于通过平台SSO登录而发生了身份验证。
- 如果&#x200B;*“tokenSource”*&#x200B;值等于&#x200B;*“Apple”*，则应用程序必须指示/提示用户仅在iOS/iPadOS **上**&#x200B;从&#x200B;*`Settings -> TV Provider`*&#x200B;显式注销。
- 应用程序必须使用[WKWebView](https://developer.apple.com/documentation/webkit/wkwebview)或[SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller)组件从Adobe Pass身份验证服务[启动注销](/help/authentication/initiate-logout.md)。 这将有助于MVPD端的会话清理。

<!--

## Resources

- [Apple SSO Overview](/help/authentication/apple-sso-overview.md)
- [REST API Overview](/help/authentication/rest-api-overview.md)
- [REST API Cookbook (Server-to-Server)](/help/authentication/rest-api-cookbook-servertoserver.md)
- [REST API Cookbook (Client-to-Server)](/help/authentication/rest-api-cookbook-clienttoserver.md)
- [REST API Reference](/help/authentication/rest-api-reference.md)
- [Apple Developer Documentation - Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount)
-->
