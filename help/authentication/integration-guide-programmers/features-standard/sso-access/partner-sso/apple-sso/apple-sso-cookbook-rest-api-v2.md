---
title: Apple SSO指南(REST API V2)
description: Apple SSO指南(REST API V2)
exl-id: 81476312-9ba4-47a0-a4f7-9a557608cfd6
source-git-commit: 63ffde4a32f003d7232d2c79ed6878ca59748f74
workflow-type: tm+mt
source-wordcount: '3857'
ht-degree: 0%

---

# Apple SSO指南(REST API V2) {#apple-sso-cookbook-rest-api-v2}

>[!IMPORTANT]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

Adobe Pass身份验证REST API V2支持在iOS、iPadOS或tvOS上运行的客户端应用程序的最终用户的合作伙伴单点登录(SSO)。

此文档用作现有[REST API V2概述](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)的扩展，该视图提供了高级视图以及描述如何使用合作伙伴流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md)实施[单点登录的文档。

## 使用合作伙伴流程进行Apple单点登录 {#cookbook}

### 先决条件 {#prerequisites}

在继续使用合作伙伴流程进行Apple单点登录之前，请确保满足以下先决条件：

* 流应用程序必须收集`X-Device-Info`和/或`User-Agent`标头所需的所有必要数据，以便Adobe Pass身份验证后端能够识别设备平台及其功能。 有关`X-Device-Info`标头的更多详细信息，请参阅[X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md)文档。

* 流应用程序必须请求访问保存在设备级别的用户订阅信息，用户必须授予应用程序继续操作的权限，类似于提供对设备摄像头或麦克风的访问权限。 必须使用Apple的[视频订阅者帐户框架](https://developer.apple.com/documentation/videosubscriberaccount)为每个应用程序请求此权限，设备将保存用户的选择。

  我们建议通过说明Apple单点登录用户体验的好处，来激励拒绝授予访问订阅信息权限的用户，但请注意，用户可以通过转到应用程序设置（访问电视提供商权限）、转到iOS和iPadOS上的&#x200B;*`Settings -> TV Provider`*&#x200B;或tvOS上的&#x200B;*`Settings -> Accounts -> TV Provider`*&#x200B;来更改其决策。

  当应用程序进入前台状态时，流应用程序可以请求用户的权限，因为应用程序可以在要求用户身份验证之前随时检查[访问](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)用户订阅信息的权限。

>[!IMPORTANT]
>
> 假设
>
> <br/>
>
> * 流应用程序已完成适用于程序员的[入门先决条件](/help/authentication/integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-overview.md#apple-sso-prerequisites-programmer)，该先决条件是启用Apple单点登录用户体验所必需的。

### 工作流 {#workflow}

执行给定步骤以使用合作伙伴流程实施Apple单点登录，如下图所示。

使用合作伙伴流程![Apple单点登录](/help/authentication/assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-apple-single-sign-on-using-partner-flows.png)

使用合作伙伴流程&#x200B;*Apple单点登录*

+++A.登记阶段

1. **检索客户端凭据：**&#x200B;流应用程序通过调用客户端注册终结点，收集检索客户端凭据所需的所有数据。

   >[!IMPORTANT]
   >
   > 有关以下内容的详细信息，请参阅[检索客户端凭据](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md#request) API文档：
   >
   > * 所有&#x200B;_必需的_&#x200B;参数，如`software_statement`
   > * 所有&#x200B;_必需的_&#x200B;标头，如`Content-Type`、`X-Device-Info`
   > * 所有&#x200B;_可选_&#x200B;参数和标头

1. **返回客户端凭据：**&#x200B;客户端注册终结点响应包含有关与接收的参数和标头关联的客户端凭据的信息。

   >[!IMPORTANT]
   >
   > 有关客户端凭据响应中提供的信息的详细信息，请参阅[检索客户端凭据](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md#success) API文档。
   >
   > <br/>
   >
   > Client Register验证请求数据，以确保满足基本条件：
   >
   > * _必需_&#x200B;参数和标头必须有效。
   >
   > <br/>
   >
   > 如果验证失败，将生成错误响应，提供附加信息以遵守[检索客户端凭据](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md#error) API文档。

   >[!TIP]
   >
   > 必须缓存并无限期使用客户端凭据。

1. **检索访问令牌：**&#x200B;流应用程序通过调用客户端令牌终结点来收集检索访问令牌所需的所有数据。

   >[!IMPORTANT]
   >
   > 有关以下内容的详细信息，请参阅[检索访问令牌](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md#request) API文档：
   >
   > * 所有&#x200B;_必需的_&#x200B;参数，如`client_id`、`client_secret`和`grant_type`
   > * 所有&#x200B;_必需的_&#x200B;标头，如`Content-Type`、`X-Device-Info`
   > * 所有&#x200B;_可选_&#x200B;参数和标头

1. **返回访问令牌：**&#x200B;客户端令牌终结点响应包含有关与收到的参数和标头关联的访问令牌的信息。

   >[!IMPORTANT]
   >
   > 有关访问令牌响应中提供的信息的详细信息，请参阅[检索访问令牌](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md#success) API文档。
   >
   > <br/>
   >
   > 客户端令牌验证请求数据，以确保满足基本条件：
   >
   > * _必需_&#x200B;参数和标头必须有效。
   >
   > <br/>
   >
   > 如果验证失败，将生成错误响应，提供附加信息以遵守[检索访问令牌](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md#error) API文档。

   >[!TIP]
   >
   > 访问令牌必须仅在指定的时间内（例如，24小时存留期）缓存和使用。 过期后，流应用程序必须请求新的访问令牌。

+++

+++B.检查身份验证阶段

1. **检索合作伙伴框架状态：**&#x200B;流应用程序调用由Apple开发的[视频订阅者帐户框架](https://developer.apple.com/documentation/videosubscriberaccount)，以获取用户权限和提供程序信息。

   >[!IMPORTANT]
   >
   > 有关以下内容的详细信息，请参阅[视频订阅者帐户框架](https://developer.apple.com/documentation/videosubscriberaccount)文档：
   >
   > <br/>
   >
   > * 流应用程序必须检查[访问](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)用户订阅信息的权限，并且只有在用户允许的情况下才继续。
   > * 流应用程序必须为`VSAccountManager`提供[代理](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate)。
   > * 流应用程序必须提交订阅者帐户信息的[请求](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest)。
   > * 流应用程序必须等待并处理[元数据](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata)信息。
   >
   > <br/>
   >
   > 流应用程序必须确保它为`VSAccountMetadataRequest`对象中的[`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed)属性指定了等于`false`的布尔值，以指示在此阶段不能中断用户。

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
                            // Continue with the "Retrieve profiles" step.
                            ...
                        } else {
                            // The user is not authenticated at platform level, continue with the "Retrieve profiles" step.
                            ...
                        }
                    }
   
            // The user has not yet made a choice or does not allow the application to access subscription information.
            default:
                // Continue with the "Retrieve profiles" step.
                ...
            }
   }
   ...
   ```

1. **返回合作伙伴框架状态信息：**&#x200B;流式应用程序验证响应数据以确保满足基本条件：
   * 已授予用户权限访问状态。
   * 用户提供程序映射标识符存在且有效。
   * 用户提供程序配置文件的到期日期（如果可用）有效。

1. **检索用户档案：**&#x200B;流式应用程序通过向“用户档案”端点发送请求，收集检索所有用户档案信息所需的所有数据。

   >[!IMPORTANT]
   >
   > 有关以下内容的详细信息，请参阅[检索配置文件](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md#Request) API文档：
   >
   > * 所有&#x200B;_必需的_&#x200B;参数，如`serviceProvider`
   > * 所有&#x200B;_必需的_&#x200B;标头，如`Authorization`、`AP-Device-Identifier`和`AP-Partner-Framework-Status`
   > * 所有&#x200B;_可选_&#x200B;参数和标头
   >
   > <br/>
   >
   > 流应用程序必须确保它包含合作伙伴框架状态的有效值，以便检索到的响应可以包含“appleSSO”类型配置文件。
   >
   > <br/>
   >
   > 有关`AP-Partner-Framework-Status`标头的更多详细信息，请参阅[AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md)文档。

1. **返回有关找到的配置文件的信息：**&#x200B;配置文件终结点响应包含有关找到的与收到的参数和标头关联的配置文件的信息。

1. **选择一个配置文件并继续决策流：**&#x200B;如果配置文件端点响应包含配置文件，则流应用程序将使用其内部逻辑（最终通过与最终用户交互）选择一个可用的配置文件以继续后续决策流。

1. **继续执行合作伙伴身份验证流程：**&#x200B;如果Profiles终结点响应不包含配置文件，则流式应用程序将继续执行合作伙伴身份验证流程。

+++

+++C.合作伙伴身份验证阶段

1. **检索配置：**&#x200B;流应用程序通过向Configuration端点发送请求，收集所有必要的数据以检索具有活动集成的MVPD列表。

   >[!IMPORTANT]
   >
   > 有关以下内容的详细信息，请参阅[检索特定服务提供商](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md#Request) API的配置：
   >
   > * 所有&#x200B;_必需的_&#x200B;参数，如`serviceProvider`
   > * 所有&#x200B;_必需的_&#x200B;标头，如`Authorization`、`AP-Device-Identifier`和`X-Device-Info`
   > * 所有&#x200B;_可选_&#x200B;参数和标头

1. **返回配置：**&#x200B;配置终结点响应包含有关与服务提供程序具有活动集成的MVPD的信息。

   >[!IMPORTANT]
   >
   > 有关配置响应中提供的信息的详细信息，请参阅特定服务提供商](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md#Response) API的[检索配置。
   >
   > <br/>
   >
   > 配置端点验证请求数据，以确保满足基本条件：
   >
   > * _必需_&#x200B;参数和标头必须有效。
   >
   > <br/>
   >
   > 如果验证失败，将生成错误响应，提供附加信息以遵守[增强型错误代码](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)文档。

   >[!IMPORTANT]
   >
   > 流应用程序在继续下一步操作时，必须确保它处理为每个MVPD提供的以下详细信息：
   >
   > * `enablePlatformServices`：指示MVPD当前是否支持Apple单点登录。
   > * `displayInPlatformPicker`：指示MVPD是否可以显示在Apple选取器中。
   > * `boardingStatus`：指示MVPD是否已载入Apple单点登录。

1. **检索合作伙伴框架状态：**&#x200B;流应用程序调用由Apple开发的[视频订阅者帐户框架](https://developer.apple.com/documentation/videosubscriberaccount)，以获取用户权限和提供程序信息。

   >[!IMPORTANT]
   >
   > 有关以下内容的详细信息，请参阅[视频订阅者帐户框架](https://developer.apple.com/documentation/videosubscriberaccount)文档：
   >
   > <br/>
   >
   > * 流应用程序必须检查[访问](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)用户订阅信息的权限，并且只有在用户允许的情况下才继续。
   > * 流应用程序必须为`VSAccountManager`提供[代理](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate)。
   > * 流应用程序必须提交订阅者帐户信息的[请求](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest)。
   > * 流应用程序必须等待并处理[元数据](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata)信息。
   >
   > <br/>
   >
   > 流应用程序必须确保它为`VSAccountMetadataRequest`对象中的[`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed)属性指定了等于`true`的布尔值，以指示在此阶段可以中断用户选择电视提供程序。

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
   
                        // This can be computed from the Configuration service response in order to filter the TV providers from the Apple picker.
                        vsaMetadataRequest.supportedAccountProviderIdentifiers = supportedAccountProviderIdentifiers;
   
                        // This can be computed from the Configuration service response in order to sort the TV providers from the Apple picker.
                        if #available(iOS 11.0, tvOS 11, *) {
                            vsaMetadataRequest.featuredAccountProviderIdentifiers = featuredAccountProviderIdentifiers;
                        }
   
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
                                // Continue with the "Retrieve partner authentication request" step.
                                ...
                            } else {
                                // The user is not authenticated at platform level.
                                if (vsaError != nil) {
                                    // The application can check to see if the user selected a provider which is present in Apple picker, but the provider is not onboarded in platform SSO.
                                    if let error: NSError = (vsaError! as NSError), error.code == 1, let appleMsoId = error.userInfo["VSErrorInfoKeyUnsupportedProviderIdentifier"] as! String? {
                                        var mvpd: Mvpd? = nil;
   
                                        // The requestor.mvpds must be computed during the "Return configuration" step. 
                                        for provider in requestor.mvpds {
                                            if provider.platformMappingId == appleMsoId {
                                                mvpd = provider;
                                                break;
                                            }
                                        }
   
                                        if mvpd != nil {
                                            // Continue with the "Proceed with basic authentication flow" step, but you can skip prompting the user with your MVPD picker and use the mvpd selection, therefore creating a better UX.
                                            ...
                                        } else {
                                            // Continue with the "Proceed with basic authentication flow" step.
                                            ...
                                        }
                                    } else {
                                        // Continue with the "Proceed with basic authentication flow" step.
                                        ...
                                    }
                                } else {
                                    // Continue with the "Proceed with basic authentication flow" step.
                                    ...
                                }
                            }
                        }
   
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                    // Continue with the "Proceed with basic authentication flow" step.
                    ...
                }
    }
    ...
   ```

1. **返回合作伙伴框架状态信息：**&#x200B;流式应用程序验证响应数据以确保满足基本条件：
   * 已授予用户权限访问状态。
   * 用户提供程序映射标识符存在且有效。
   * 用户提供程序配置文件的到期日期（如果可用）有效。

1. **检索合作伙伴身份验证请求：**&#x200B;流应用程序通过调用会话合作伙伴终结点，收集启动身份验证会话所需的所有数据。

   >[!IMPORTANT]
   >
   > 有关以下内容的详细信息，请参阅[检索合作伙伴身份验证请求](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md#Request) API文档：
   >
   > * 所有&#x200B;_必需的_&#x200B;参数，如`serviceProvider`和`partner`
   > * 所有&#x200B;_必需的_&#x200B;标头，如`Authorization`、`AP-Device-Identifier`、`Content-Type`、`X-Device-Info`和`AP-Partner-Framework-Status`
   > * 所有&#x200B;_可选_&#x200B;标头和参数
   >
   > <br/>
   >
   > 流应用程序必须确保它包含合作伙伴框架状态的有效值，以便检索到的响应可能包含合作伙伴身份验证请求（SAML请求）。
   >
   > <br/>
   >
   > 有关`AP-Partner-Framework-Status`标头的更多详细信息，请参阅[AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md)文档。

1. **指示下一个操作：**&#x200B;会话合作伙伴终结点响应包含指导流应用程序执行下一个操作所需的数据。

   >[!IMPORTANT]
   >
   > 有关会话响应中提供的信息的详细信息，请参阅[检索合作伙伴身份验证请求](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md#Response) API文档。
   >
   > <br/>
   >
   > 会话合作伙伴端点验证请求数据以确保满足基本条件：
   >
   > * _必需_&#x200B;参数和标头必须有效。
   > * 提供的`serviceProvider`和`mvpd`之间的集成必须处于活动状态。
   >
   > <br/>
   >
   > 如果基本验证失败，将生成错误响应，提供附加信息，这些信息将遵循[增强型错误代码](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)文档。
   >
   > <br/>
   >
   > 会话合作伙伴端点验证请求数据，以确保满足合作伙伴单点登录条件：
   >
   >  * Adobe Pass服务器中的合作伙伴单点登录配置必须有效且已启用。
   >  * 通过[AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md)标头接收的合作伙伴框架状态有效负载必须有效。
   >
   > <br/>
   >
   > 如果合作伙伴单点登录验证失败，则响应将默认使用基本身份验证流程。

1. **继续决策流：**&#x200B;会话合作伙伴终结点响应包含以下数据：
   * `actionName`属性设置为“授权”。
   * `actionType`属性设置为“直接”。

   如果Adobe Pass后端标识有效的配置文件，则流应用程序无需使用选定的MVPD重新进行身份验证，因为已存在可用于后续决策流的配置文件。

1. **继续基本身份验证流程：**&#x200B;会话合作伙伴终结点响应包含以下数据：
   * `actionName`属性设置为“身份验证”或“恢复”。
   * `actionType`属性设置为“interactive”或“direct”。

   如果Adobe Pass后端未识别有效的配置文件，并且合作伙伴单点登录验证失败，则Adobe Pass服务器将回退到基本身份验证流程。

   有关基本身份验证流程的更多详细信息，请参阅以下文档：
   * [在主应用程序中执行身份验证](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
   * [使用预选的mvpd在辅助应用程序中执行身份验证](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [无需预选mvpd就可在辅助应用程序中执行身份验证](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

1. **继续使用合作伙伴身份验证响应流创建和检索配置文件：**&#x200B;会话合作伙伴终结点响应包含以下数据：
   * `actionName`属性设置为“partner_profile”。
   * `actionType`属性设置为“直接”。
   * `authenticationRequest - type`属性包括合作伙伴框架用于MVPD登录的安全协议（当前仅设置为SAML）。
   * `authenticationRequest - request`属性包含传递到合作伙伴框架的SAML请求。
   * `authenticationRequest - attributesNames`属性包含传递到合作伙伴框架的SAML属性。

   如果Adobe Pass后端未识别有效的配置文件，并且合作伙伴单点登录验证通过，则流应用程序会收到一个包含操作和数据的Response，以便传递到Partner Framework，从而开始使用MVPD的身份验证流程。

1. **使用合作伙伴框架完成MVPD身份验证：**&#x200B;将上一步骤中获得的合作伙伴身份验证请求（SAML请求）转发到[视频订阅者帐户框架](https://developer.apple.com/documentation/videosubscriberaccount)。 如果身份验证流程成功，则与MVPD的[视频订阅者帐户框架](https://developer.apple.com/documentation/videosubscriberaccount)交互将生成合作伙伴身份验证响应（SAML响应），该响应将随合作伙伴框架状态信息一起返回。

   >[!IMPORTANT]
   >
   > 有关以下内容的详细信息，请参阅[视频订阅者帐户框架](https://developer.apple.com/documentation/videosubscriberaccount)文档：
   >
   > <br/>
   >
   > * 流应用程序必须检查[访问](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)用户订阅信息的权限，并且只有在用户允许的情况下才继续。
   > * 流应用程序必须为`VSAccountManager`提供[代理](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate)。
   > * 流应用程序必须提交订阅者帐户信息的[请求](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest)，并且必须包含上一步中获得的合作伙伴身份验证请求（SAML请求）。
   > * 流应用程序必须等待并处理[元数据](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata)信息。
   >
   > <br/>
   >
   > 流应用程序必须确保它为`VSAccountMetadataRequest`对象中的[`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed)属性指定了等于`true`的布尔值，以指示在此阶段可以中断用户以向所选电视提供程序进行身份验证。

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
   
                        // This are the user metadata fields expected to be available on a successful login and are determined from the Sessions SSO service. Look for the authenticationRequest > attributesNames associated with the provider determined in a previous step.
                        vsaMetadataRequest.attributeNames = attributesNames;
   
                        // This is the authenticationRequest > request field from Sessions SSO service.
                        vsaMetadataRequest.verificationToken = authenticationRequestPayload;
   
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
   
                                // Continue with the "Create and retrieve profile using partner authentication response" step.
                                ...
                            } else {
                                // Continue with the "Proceed with basic authentication flow" step.
                                ...
                            }
                        }
   
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                    // Continue with the "Proceed with basic authentication flow" step.
                    ...
                }
    }
    ...
   ```

1. **返回合作伙伴身份验证响应：**&#x200B;流式应用程序验证响应数据以确保满足基本条件：
   * 已授予用户权限访问状态。
   * 用户提供程序映射标识符存在且有效。
   * 用户提供程序配置文件的到期日期（如果可用）有效。
   * 合作伙伴身份验证响应（SAML响应）存在且有效。

1. **使用合作伙伴身份验证响应创建和检索配置文件：**&#x200B;流应用程序通过调用Profiles Partner终结点来收集创建和检索配置文件所需的所有数据。

   >[!IMPORTANT]
   >
   > 有关以下内容的详细信息，请参阅[使用合作伙伴身份验证响应创建和检索配置文件](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md#Request) API文档：
   >
   > * 所有&#x200B;_必需的_&#x200B;参数，如`serviceProvider`、`partner`和`SAMLResponse`
   > * 所有&#x200B;_必需的_&#x200B;标头，如`Authorization`、`AP-Device-Identifier`、`Content-Type`、`X-Device-Info`和`AP-Partner-Framework-Status`
   > * 所有&#x200B;_可选_&#x200B;标头和参数
   >
   > <br/>
   >
   > 流应用程序必须确保它包含合作伙伴框架状态和合作伙伴身份验证响应（SAML响应）的有效值，以便检索到的响应可以包含“appleSSO”类型配置文件。
   >
   > <br/>
   >
   > 有关`AP-Partner-Framework-Status`标头的更多详细信息，请参阅[AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md)文档。

1. **返回有关合作伙伴配置文件的信息：** Profiles终结点响应包含有关合作伙伴配置文件的信息，包括设置为“appleSSO”的属性`type`。

   >[!IMPORTANT]
   >
   > 有关配置文件响应中提供的信息的详细信息，请参阅[使用合作伙伴身份验证响应创建和检索配置文件](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md#Response) API文档。
   >
   > <br/>
   >
   > Profiles Partner端点验证请求数据，以确保满足基本条件：
   >
   > * _必需_&#x200B;参数和标头必须有效。
   > * 提供的`serviceProvider`和`mvpd`之间的集成必须处于活动状态。
   >
   > <br/>
   >
   > 如果验证失败，将生成错误响应，提供附加信息以遵守[增强型错误代码](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)文档。
   >
   > <br/>
   >
   > Profiles Partner端点验证请求数据，以确保满足合作伙伴单点登录条件：
   >
   >  * Adobe Pass服务器中的合作伙伴单点登录配置必须有效且已启用。
   >  * 通过[AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md)标头接收的合作伙伴框架状态有效负载必须有效。
   >
   > <br/>
   >
   > 如果合作伙伴单一登录验证失败，则响应将默认使用基本用户档案检索流程。

1. **继续决策流：**&#x200B;流式应用程序可以继续后续决策流。

+++

+++ D.决定阶段

1. **检索合作伙伴框架状态：**&#x200B;流应用程序调用由Apple开发的[视频订阅者帐户框架](https://developer.apple.com/documentation/videosubscriberaccount)，以获取用户权限和提供程序信息。

   >[!IMPORTANT]
   > 
   > 如果所选的用户配置文件类型不是“appleSSO”，则流应用程序可以跳过此步骤。

   >[!IMPORTANT]
   >
   > 有关以下内容的详细信息，请参阅[视频订阅者帐户框架](https://developer.apple.com/documentation/videosubscriberaccount)文档：
   >
   > <br/>
   >
   > * 流应用程序必须检查[访问](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)用户订阅信息的权限，并且只有在用户允许的情况下才继续。
   > * 流应用程序必须为`VSAccountManager`提供[代理](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate)。
   > * 流应用程序必须提交订阅者帐户信息的[请求](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest)。
   > * 流应用程序必须等待并处理[元数据](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata)信息。
   >
   > <br/>
   >
   > 流应用程序必须确保它为`VSAccountMetadataRequest`对象中的[`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed)属性指定了等于`false`的布尔值，以指示在此阶段不能中断用户。

   >[!TIP]
   >
   > 流应用程序可以使用合作伙伴框架状态信息的缓存值，建议在应用程序从后台状态转换为前台状态时刷新该值。 在这种情况下，流应用程序必须确保它缓存并仅使用合作伙伴框架状态的有效值，如“返回合作伙伴框架状态信息”步骤所述。

1. **返回合作伙伴框架状态信息：**&#x200B;流式应用程序验证响应数据以确保满足基本条件：
   * 已授予用户权限访问状态。
   * 用户提供程序映射标识符存在且有效。
   * 用户提供程序配置文件的到期日期有效。

   >[!IMPORTANT]
   >
   > 如果所选的用户配置文件类型不是“appleSSO”，则流应用程序可以跳过此步骤。

1. **检索预授权决策：**&#x200B;流应用程序通过调用Decisions Preauthorize终结点，收集所有必要的数据以获取资源列表的预授权决策。

   >[!IMPORTANT]
   >
   > 有关以下各项的详细信息，请参阅使用特定mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md#request) API检索[预授权决策：
   >
   > * 所有&#x200B;_必需的_&#x200B;参数，如`serviceProvider`、`mvpd`和`resources`
   > * 所有&#x200B;_必需的_&#x200B;标头，如`Authorization`和`AP-Device-Identifier`
   > * 所有&#x200B;_可选_&#x200B;参数和标头
   >
   > <br/>
   >
   > 如果所选配置文件是“appleSSO”类型配置文件，则流应用程序在进一步发出请求之前，必须确保它包含合作伙伴框架状态的有效值。 但是，如果所选的用户配置文件类型不是“appleSSO”，则可以跳过此步骤。
   >
   > <br/>
   >
   > 有关`AP-Partner-Framework-Status`标头的更多详细信息，请参阅[AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md)文档。

1. **返回预授权决策：**&#x200B;决策预授权终结点响应包含每个资源的`Permit`或`Deny`决策：
   * `Permit`决策意味着资源可播放。 响应不包含媒体令牌，因为不得使用预授权流播放资源。
   * `Deny`决策意味着资源不可播放。 响应包含错误有效负载，该有效负载遵守[增强型错误代码](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)文档。

   >[!IMPORTANT]
   >
   > 有关决策响应中提供的信息的详细信息，请参阅使用特定mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md#response) API检索[预授权决策。
   >
   > <br/>
   >
   > Decisions Preauthorize端点验证请求数据以确保满足基本条件：
   >
   > * _必需_&#x200B;参数和标头必须有效。
   > * 提供的`serviceProvider`和`mvpd`之间的集成必须处于活动状态。
   >
   > <br/>
   >
   > 如果验证失败，将生成错误响应，提供附加信息以遵守[增强型错误代码](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)文档。

1. **检索合作伙伴框架状态：**&#x200B;流应用程序调用由Apple开发的[视频订阅者帐户框架](https://developer.apple.com/documentation/videosubscriberaccount)，以获取用户权限和提供程序信息。

   >[!IMPORTANT]
   >
   > 如果所选的用户配置文件类型不是“appleSSO”，则流应用程序可以跳过此步骤。

   >[!IMPORTANT]
   >
   > 有关以下内容的详细信息，请参阅[视频订阅者帐户框架](https://developer.apple.com/documentation/videosubscriberaccount)文档：
   >
   > <br/>
   >
   > * 流应用程序必须检查[访问](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)用户订阅信息的权限，并且只有在用户允许的情况下才继续。
   > * 流应用程序必须为`VSAccountManager`提供[代理](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate)。
   > * 流应用程序必须提交订阅者帐户信息的[请求](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest)。
   > * 流应用程序必须等待并处理[元数据](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata)信息。
   >
   > <br/>
   >
   > 流应用程序必须确保它为`VSAccountMetadataRequest`对象中的[`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed)属性指定了等于`false`的布尔值，以指示在此阶段不能中断用户。

   >[!TIP]
   >
   > 流应用程序可以使用合作伙伴框架状态信息的缓存值，建议在应用程序从后台状态转换为前台状态时刷新该值。 在这种情况下，流应用程序必须确保它缓存并仅使用合作伙伴框架状态的有效值，如“返回合作伙伴框架状态信息”步骤所述。

1. **返回合作伙伴框架状态信息：**&#x200B;流式应用程序验证响应数据以确保满足基本条件：
   * 已授予用户权限访问状态。
   * 用户提供程序映射标识符存在且有效。
   * 用户提供程序配置文件的到期日期有效。

   >[!IMPORTANT]
   >
   > 如果所选的用户配置文件类型不是“appleSSO”，则流应用程序可以跳过此步骤。

1. **检索授权决定：**&#x200B;流应用程序通过调用Decisions Authorize终结点，收集所有必需的数据以获取特定资源的授权决定。

   >[!IMPORTANT]
   >
   > 有关以下各项的详细信息，请参阅使用特定mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md#request) API检索[授权决策：
   >
   > * 所有&#x200B;_必需的_&#x200B;参数，如`serviceProvider`、`mvpd`和`resources`
   > * 所有&#x200B;_必需的_&#x200B;标头，如`Authorization`和`AP-Device-Identifier`
   > * 所有&#x200B;_可选_&#x200B;参数和标头
   >
   > <br/>
   >
   > 如果所选配置文件是“appleSSO”类型配置文件，则流应用程序在进一步发出请求之前，必须确保它包含合作伙伴框架状态的有效值。 但是，如果所选的用户配置文件类型不是“appleSSO”，则可以跳过此步骤。
   >
   > <br/>
   >
   > 有关`AP-Partner-Framework-Status`标头的更多详细信息，请参阅[AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md)文档。

1. **返回授权决定：** Decisions Authorize终结点响应包含特定资源的`Permit`或`Deny`决定：
   * `Permit`决策意味着资源可播放。 响应包括媒体令牌。
   * `Deny`决策意味着资源不可播放。 响应包含错误有效负载，该有效负载遵守[增强型错误代码](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)文档。

   >[!IMPORTANT]
   >
   > 有关决策响应中提供的信息的详细信息，请参阅使用特定mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md#response) API检索[授权决策。
   >
   > <br/>
   >
   > Decisions Authorize端点验证请求数据以确保满足基本条件：
   >
   > * _必需_&#x200B;参数和标头必须有效。
   > * 提供的`serviceProvider`和`mvpd`之间的集成必须处于活动状态。
   >
   > <br/>
   >
   > 如果验证失败，将生成错误响应，提供附加信息以遵守[增强型错误代码](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)文档。

+++

+++ D.注销阶段

1. **启动Adobe Pass注销：**&#x200B;流应用程序通过调用Adobe Pass注销终结点来收集启动注销流所需的所有数据。

   >[!IMPORTANT]
   >
   > 有关以下内容的详细信息，请参阅特定mvpd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md#request) API的[Initiate注销：
   >
   > * 所有&#x200B;_必需的_&#x200B;参数，如`serviceProvider`、`mvpd`和`redirectUrl`
   > * 所有&#x200B;_必需的_&#x200B;标头，如`Authorization`、`AP-Device-Identifier`
   > * 所有&#x200B;_可选_&#x200B;参数和标头

1. **指示下一个操作：** Adobe Pass注销终结点响应包含指导流应用程序执行下一个操作所需的数据：
   * 缺少`url`属性，因为用户需要与合作伙伴（系统）级别交互以完成注销流程。
   * `actionName`属性设置为“partner_logout”。
   * `actionType`属性设置为“partner_interactive”。

   >[!IMPORTANT]
   >
   > 当删除的用户配置文件类型为“appleSSO”时，流应用程序必须提示用户在合作伙伴级别完成注销过程，如`actionName`和`actionType`属性所指定。

   >[!IMPORTANT]
   >
   > 有关注销响应中提供的信息的详细信息，请参阅特定mvpd ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md#response) API的[启动注销。
   >
   > <br/>
   >
   > Adobe Pass注销端点验证请求数据以确保满足基本条件：
   >
   > * _必需_&#x200B;参数和标头必须有效。
   > * 提供的`serviceProvider`和`mvpd`之间的集成必须处于活动状态。
   >
   > <br/>
   >
   > 如果验证失败，将生成错误响应，提供附加信息以遵守[增强型错误代码](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)文档。

+++
