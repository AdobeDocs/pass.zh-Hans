---
title: iOS/tvOS API预授权
description: iOS/tvOS API预授权
exl-id: 79c596a4-0e38-4b6c-bb85-f97c6af45ed8
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '414'
ht-degree: 0%

---

# （旧版）预授权 {#preauthorize}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

>[!IMPORTANT]
>
> 确保随时了解汇总在[产品公告](/help/authentication/product-announcements.md)页中的最新Adobe Pass身份验证产品公告和停用时间表。

预授权API可用于获取一个或多个资源的预授权决策，这样应用程序就可以实现UI提示和/或内容过滤。

>[!IMPORTANT]
>
>在授予用户访问指定资源的权限之前，必须&#x200B;**使用授权API**。

如果Preauthorize API响应结果包含一个或多个具有被拒绝的预授权决定的资源，则可以为每个受影响的资源包含其他错误信息&#x200B;**（请参阅下面的注释）**。

>[!IMPORTANT]
>
>增强的错误报告功能（为被拒绝的预授权决策添加其他错误信息）在请求时可用，因为它必须在Adobe Pass身份验证配置端启用。

如果由于Adobe Pass身份验证SDK错误或发生Adobe Pass身份验证服务错误，导致预授权API请求无法提供服务，则无论上述配置如何，其他错误信息和任何资源都不会包含在预授权API响应结果中。

</br>

## `- (void) preauthorize:(nonnull PreauthorizeRequest *)request didCompleteWith:(nonnull AccessEnablerCallback<PreauthorizeResponse *> *)callback;`


**可用性：** v3.6.0+

**参数：**

- PreauthorizeRequest：用于传递API请求内容的请求对象；
- AccessEnablerCallback：用于返回API响应的回调对象；
- PreauthorizeResponse：用于返回API响应内容的响应对象；


</br>

## `class PreauthorizeRequest`{#androidpreauthorizerequest}

### **类PreauthorizeRequest.Builder**

```
    ///
    /// Sets the `List` of resources for which you want to obtain preauthorization decisions.
    ///
    /// Each element in the list must be a `String` representing either the resource ID value or the media RSS fragment which must be agreed with the MVPD.
    ///
    /// This function sets the information only in the context of current `Builder` object instance which is the receiver of this function call.
    ///
    /// To build an actual `PreauthorizeRequest` you can have a look at `Builder`'s function:
    ///
    /// ```
    /// public func build() -> PreauthorizeRequest
    /// ```
    ///
    /// - Parameter resources: The `List` of resources for which you want to obtain preauthorization decisions.
    ///
    /// - Returns: The reference to the same `Builder` object instance which is the receiver of the function call. It does this in order to allow the creation of function chaining.
    ///
    public func setResources(resources: [String]) -> PreauthorizeRequest.Builder

 

    ///
    /// Sets the features which you want to have them disabled when obtaining preauthorization decisions.
    ///
    /// The list of available features are provided by `PreauthorizeRequest.Feature` enumeration. All features are enabled by default.
    ///
    /// This function sets the information only in the context of current `Builder` object instance which is the receiver of this function call.
    ///
    /// To build an actual `PreauthorizeRequest` you can have a look at `Builder`'s function:
    ///
    /// ```
    /// public func build() -> PreauthorizeRequest
    /// ```
    ///
    /// - Parameter features: The set of features which you want to have them disabled.
    ///
    /// - Returns: The reference to the same `Builder` object instance which is the receiver of the function call. It does this in order to allow the creation of function chaining.
    ///
    public func disableFeatures(features: Set<PreauthorizeRequest.Feature>) -> PreauthorizeRequest.Builder

 

    ///
    /// Creates and retrieves the reference of a new `PreauthorizeRequest` object instance.
    ///
    /// This function instantiates a new `PreauthorizeRequest` object every time it is called.
    ///
    /// This function uses the values set in advance in the context of current `Builder` object instance which is the receiver of this function call.
    ///
    /// Bear in mind that this function does not produce any side effects, therefore it does not alter the state of the SDK or the state of the `Builder` object instance which is the receiver of this function call.
    ///
    /// It means that successive calls of this function for the same receiver will create different new `PreauthorizeRequest` object instances, but having the same information, in case the values set to the `Builder` where not modified between the calls.
    ///
    /// In case you do not need to update any of the provided information (resources, features, etc.) you may reuse the `PreauthorizeRequest` instance for multiple uses of the `preauthorize` API.
    ///
    /// - Returns: The reference to a new `PreauthorizeRequest` object instance.
    ///
    public func build() -> PreauthorizeRequest
```


## **枚举PreauthorizeRequest.Feature**

```
    ///
    /// This feature controls whether to use the information from the AccessEnabler SDK cache or to bypass it and
    /// rely on Adobe Pass server information via a network call.
    ///
    LOCAL_CACHE

    ///
    /// This feature controls whether to use the information from the Adobe Pass server cache or to bypass it and
    /// rely on MVPD server information via a network call.
    ///
    REMOTE_CACHE
```

</br>

## `interface AccessEnablerCallback<PreauthorizeResponse>` {#accessenablercallback}

```
    /// Response callback called by the SDK when the preauthorize API request was fulfilled. The result is either a successful or an error result containing a status.
    public func onResponse(result: PreauthorizeResponse)


    /// Failure callback called by the SDK when the preauthorize API request could not be serviced. The result is a failure result containing a status. 
    public func onFailure(result: PreauthorizeResponse)
```

</br>


## `class PreauthorizeResponse` {#preauthorizeresponse}

```
    ///
    /// - Returns: Additional status (state) information in case of error or failure.
    ///   Might hold a `nil` value.
    ///
    public Status getStatus()

    ///
    /// - Returns: The list of preauthorization decisions. One decision for each resource.
    ///            The list might be empty in case of error or failure.
    ///
    public List<Decision> getDecisions()
```

### 示例：

本节重点介绍一些可能的PreauthorizeResponse对象的JSON结构。

>[!IMPORTANT]
>
>以下示例提供的JSON仅可通过本文档中显示的模型类访问。 除非通过公共方法，否则您将无法访问此类JSON的属性。

>[!IMPORTANT]
>
>通过[高级错误报告](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)中记录了通过增强错误报告功能的介质检索到的可能的其他错误列表。

#### 成功

所有请求的资源都有一个积极的预授权决定

```JSON
    {
        "resources": [
            {
                "id": "resource1",
                "authorized": true 
            },
            {
                "id": "resource2",
                "authorized": true 
            },
            {
                "id": "resource3",
                "authorized": true 
            }
        ]
    }
```


一个或多个资源具有被拒绝的预授权决定，并且未在Adobe Pass身份验证配置中启用增强的错误报告功能

```JSON
    {
        "resources": [
            {
                "id": "resource1",
                "authorized": true 
            },
            {
                "id": "resource2",
                "authorized": false,
                 
            },
            {
                "id": "resource3",
                "authorized": true 
            }
        ]
    }
```


一个或多个资源具有被拒绝的预授权决定，并在Adobe Pass身份验证配置中启用了增强的错误报告功能

```JSON
    {
        "resources": [
            {
                "id": "resource1",
                "authorized": true 
            },
            {
                "id": "resource2",
                "authorized": false,
                "error" : {
                   "status" : 403,
                   "code" : "authorization_denied_by_mvpd",
                   "message" : "User not authorized",
                   "details" : "Your subscription package does not include the "TestStream3" channel.",
                   "helpUrl" : "https://experienceleague.adobe.com/docs/primetime/authentication/auth-features/error-reportn/enhanced-error-codes.html?lang=zh-Hans",
                   "trace" : "0453f8c8-167a-4429-8784-cd32cfeaee58",
                   "action" : "none"
                }
            },
            {
                "id": "resource3",
                "authorized": true 
            }
        ]
    }
```


#### 错误



Adobe Pass身份验证服务在为Preauthorize API请求提供服务时遇到错误

```JSON
    {
        "resources": [],
        "status": {
            "status": 400,
            "code" : "bad_request",
            "message": "Missing required parameter : deviceId",
            "details": "",
            "helpUrl" : "https://experienceleague.adobe.com/docs/primetime/authentication/auth-features/error-reportn/enhanced-error-codes.html?lang=zh-Hans",
            "trace" : "9f115e1c-0158-4a41-8805-9f68923f3646",
            "action" : "none"
        }
    }
```


#### 失败

Adobe Pass身份验证SDK在为Preauthorize API请求提供服务时命中错误

```JSON
    {
        "status": 0,
        "code": "requestor_not_configured",
        "message": "The requestor is not yet configured which is a prerequisite for using any API apart from the setRequestor API.",
        "action": "retry"
    }
    
    {
        "status": 0,
        "code": "authentication_session_expired",
        "message": "The current authentication session has expired. The user must re-authenticate with a supported MVPD in order to continue.",
        "action": "authentication"
    }
    
    {
        "status": 0,
        "code": "authentication_session_missing",
        "message": "The authentication session associated with this request could not be retrieved. The user must re-authenticate with a supported MVPD in order to continue.",
        "action": "authentication"
    }
    
    {
        "status": 0,
        "code": "access_token_unavailable",
        "message": "The request failed due to an unexpected error while retrieving the access token. Please check the validity of your software statement and custom scheme.",
        "action": "none"
    }
    
    {
        "status": 0,
        "code": "server_response_format_unknown",
        "message": "The request failed due to an unknown server response format. Please capture the device console logs and contact support.",
        "action": "none"
    }
    
    {
        "status": 0,
        "code": "network_error",
        "message": "The request failed due to a network error. If the issue persists over several retries, please capture the device console logs and contact support.",
        "action": "none"
    }
```

</br>

## **类状态** {#status}

```
    ///
    /// - Returns: The HTTP response status code as documented in RFC 7231.
    ///            Might be 0 in case the `Status` comes from the SDK instead of Adobe Pass Authentication services.
    ///
    public int getStatus()

    ///
    /// - Returns: The standard Adobe Pass Authentication services error code.
    ///            Might hold an empty string or a `nil` value.
    ///
    public String getCode()

    ///
    /// - Returns: The human readable message which can be displayed to the end user.
    ///            Might hold an empty string or a `nil` value.
    ///
    public String getMessage()

    ///
    /// - Returns: The detailed message which in some cases is provided by the MVPD authorization endpoints or by Programmer degradation rules.
    ///            Might hold an empty string or a `nil` value.
    ///
    public String getDetails()

    ///
    /// - Returns: The URL that links to more information about why this state/error occurred and possible solutions.
    ///            Might hold an empty string or a `nil` value.
    ///
    public String getHelpUrl()

    ///
    /// - Returns: The unique identifier for this response, which can be used when contacting support to identify specific issues in more complex scenarios.
    ///            Might hold an empty string or a `nil` value.
    ///
    public String getTrace()

    ///
    /// - Returns: The recommended action to remediate the situation.
    ///             - none: Unfortunately there is no predefined action to remediate this issue. This might indicate an improper invocation of the public API
    ///             - configuration: A configuration change is needed through TVE dashboard or by contacting support.
    ///             - application-registration: The application must register itself again.
    ///             - authentication: The user must authenticate or re-authenticate.
    ///             - authorization: The user must obtain authorization for the specific resource.
    ///             - degradation: Some form of degradation should be applied.
    ///             - retry: Retrying the request might solve the issue
    ///             - retry-after: Retrying the request after the indicated period of time might solve the issue.
    ///            Might hold an empty string or a `nil` value.
    ///
    public String getAction()
```

<br>

## **类决策** {#decision}

```
    ///
    /// This is a getter function.
    ///
    /// - Returns: The resource id for which the decision was obtained.
    ///
    public Status getId()

    ///
    /// This is a getter function.
    ///
    /// - Returns: The value of the flag indicating if the decision is successful or not.
    ///
    public boolean isAuthorized()

    ///
    /// This is a getter function.
    ///
    /// - Returns: Additional status (state) information in case some error has occurred.
    ///            Might hold a `nil` value.
    ///
    public Status getError()
```

</br>


## **示例代码** {#sample}

```
let resources: [String] = ["resource_1", "resource_2", "resource_3"];

let disabledFeatures: Set<PreauthorizationRequest.Feature> = [PreauthorizationRequest.Feature.LOCAL_CACHE];

// Build the Preauthorization API request using the builder from PreauthorizationRequest class`

let request: PreauthorizationRequest = PreauthorizationRequest.Builder()

                  .setResources(resources: resources)


                  .disableFeatures(features: disabledFeatures)  // It is **optional** to disable features. If not used all features are enabled by default.

                  .build();

// Build the AccessEnablerCallback by providing the constructor two callbacks for onResponse and onFailure handling  
func onResponseCallback(result: PreauthorizeResponse) -> Void {  //
TODO };

func onFailureCallback(result: PreauthorizeResponse) -> Void {

// TODO

};

let callback: AccessEnablerCallback<PreauthorizeResponse> = AccessEnablerCallback<PreauthorizeResponse>(onResponse: onResponseCallback, onFailure: onFailureCallback);

// Use the preauthorize API
accessEnabler.preauthorize(request, callback);
```
