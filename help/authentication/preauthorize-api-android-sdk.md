---
title: 预先授权Android
description: 预先授权Android
exl-id: b5337595-135f-4981-a578-2da432f125d6
source-git-commit: 6c328eb2c635a1d76fc7dae8148a4de291c126e0
workflow-type: tm+mt
source-wordcount: '195'
ht-degree: 0%

---

# 预授权 {#preuthorize-android}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

</br>


应用程序需要使用预授权API方法以获得一个或多个资源的预授权决定。 应使用Preauthorize API请求进行UI提示和/或内容筛选。 在授予用户访问指定资源的权限之前，必须发出实际的授权API请求。



在出现意外错误（例如，网络问题、MVPD授权端点不可用等）时 在Adobe Pass身份验证服务处理预授权API请求时发生，作为预授权API响应结果的一部分，受影响资源将包含一个或多个分隔的错误信息。


## `public void preauthorize(PreauthorizeRequest request, AccessEnablerCallback<PreauthorizeResponse> callback);`


**描述：**

**可用性：** v3.6.0+

**参数：**

- *PreauthorizeRequest*：用于定义请求的生成器对象
- AccessEnablerCallback ：用于返回API响应的回调
- PreauthorizeResponse ：用于返回API的对象
响应内容


### 公共类PreauthorizeRequest {#androidpreauthorizerequest}

**类PreauthorizeRequest.Builder**

```java
    ///
    /// Sets the `List` of resources for which you want to obtain preauthorization decisions.
    ///
    /// Each element in the list must be a `String` representing either the resource ID value or the media RSS fragment which must be agreed with the MVPD.
    ///
    /// This function sets the information only in the context of current `Builder` object instance which is the receiver of this function call.
    ///
    /// To build an actual `PreauthorizeRequest` you can have a look at `Builder`'s function:
    /// ```
    /// public func build() -> PreauthorizeRequest
    /// ```
    ///
    /// - Parameter resources: The `List` of resources for which you want to obtain preauthorization decisions.
    ///
    /// - Returns: The reference to the same `Builder` object instance which is the receiver of the function call. It does this in order to allow the creation of function chaining.
    ///
```

**公共生成器setResources（List\&lt;String\>资源）**

```
    ///
    /// Sets the features which you want to have them disabled when obtaining preauthorization decisions.
    ///
    /// The list of available features are provided by `PreauthorizeRequest.Feature` enumeration.
    ///
    /// This function sets the information only in the context of current `Builder` object instance which is the receiver of this function call.
    ///
    /// To build an actual `PreauthorizeRequest` you can have a look at `Builder`'s function:
    /// ```
    /// public func build() -> PreauthorizeRequest
    /// ```
    ///
    /// - Parameter features: The set of features which you want to have them disabled.
    ///
    /// - Returns: The reference to the same `Builder` object instance which is the receiver of the function call. It does this in order to allow the creation of function chaining.
    ///
```


**公共生成器disableFeatures(Set\&lt;PreauthorizeRequest.Feature\>
功能)**

```
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
```

**公共PreauthorizeRequest生成()**

**枚举PreauthorizeRequest.Feature**

```java
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


### `abstract class AccessEnablerCallback<PreauthorizeResponse>` {#accessenablercallback}

```java
    /// Response callback called by the SDK when the preauthorize API request was fulfilled. The result is either a successful or an error result containing a status.

**public void onResponse(PreauthorizeResponse result)**

 

    /// Failure callback called by the SDK when the preauthorize API request could not be serviced. The result is a failure result containing a status. 

**public void onFailure(PreauthorizeResponse result)**
```



### 类PreauthorizeResponse {#preauthorizeresponse}

```java
    ///
    /// - Returns: Additional status (state) information in case of failure.
    ///   Might hold a `null` value.
    ///

**public [Status](#status) getStatus()**

 

    ///
    /// - Returns: The list of preauthorization decisions. One decision for each resource.
    ///            The list might be empty in case of failure.
    ///

**public List\<[Decision](#status)\> getDecisions()**
```


**类状态** {#status}

```java
///
/// - Returns: The HTTP response status code as documented in RFC 7231.
/// Might be 0 in case the \`Status\` comes from the SDK instead of Adobe Pass Authentication services.

///

**public int getStatus()**

    ///
    /// - Returns: The standard Adobe Pass Authentication services error code.
    ///            Might hold an empty string or a `null` value.
    ///

**public String getCode()**

    ///
    /// - Returns: The human readable message which can be displayed to the end user.
    ///            Might hold an empty string or a `null` value.
    ///

**public String getMessage()**

    ///
    /// - Returns: The detailed message which in some cases is provided by the MVPD authorization endpoints or by Programmer degradation rules.
    ///            Might hold an empty string or a `null` value.
    ///

**public String getDetails()**

    ///
    /// - Returns: The URL that links to more information about why this state/error occurred and possible solutions.
    ///            Might hold an empty string or a `null` value.
    ///

**public String getHelpUrl()**

    ///
    /// - Returns: The unique identifier for this response, which can be used when contacting support to identify specific issues in more complex scenarios.
    ///            Might hold an empty string or a `null` value.
    ///

**public String getTrace()**

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
    ///            Might hold an empty string or a `null` value.
    ///

**public String getAction()**
```

</br>

>**类决策** {#decision}

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

**public boolean isAuthorized()**

 

    ///
    /// This is a getter function.
    ///
    /// - Returns: Additional status (state) information in case some error has occurred.
    ///            Might hold a `null` value.
    ///

**public Status getError()**
```

</br>



示例：


```java
    // build request object 
    Preauthorize request = new PreauthorizeRequest.Builder()
                .setResources(resourcesList)
                .disableFeatures(PreauthorizeRequest.FEATURE.LOCAL_CACHE) // optional, use only to disable local cache for preauthorized resources
                .build();
    }
    
    // execute preauthorize call, passing a callback function to process the response
    accessEnabler.preauthorize(request, new AccessEnablerCallback<PreauthorizeResponse>() {
        @Override
        public void onResponse(PreauthorizeResponse result) {
            // Handle onResponse
        }
    
        @Override
        public void onFailure(PreauthorizeResponse result) {
            // Handle onFailure
        }
    });
```
