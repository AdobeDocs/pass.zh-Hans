---
title: 预授权
description: JavaScript预授权
exl-id: b7493ca6-1862-4cea-a11e-a634c935c86e
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '1466'
ht-degree: 0%

---

# （旧版）预授权 {#js-preauthorize}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## 概述 {#preauth-overview}

应用程序使用预授权API方法获取一个或多个资源的预授权决策。 应使用Preauthorize API请求进行UI提示和/或内容筛选。 在允许用户访问指定的资源之前，必须发出实际的授权API请求。

如果Adobe Pass身份验证服务处理预授权API请求时发生了意外错误(例如，网络问题和MVPD授权端点不可用)，则作为预授权API响应结果的一部分，将为受影响的资源包含一个或多个单独的错误信息。

### public preauthorize(request： PreauthorizeRequest， callback： AccessEnablerCallback&lt;any>)： void {#preauth-method}

**描述：**&#x200B;应用程序使用此方法从Adobe Pass身份验证服务获取经过身份验证用户的预授权（信息性）决策，以查看特定的受保护资源，主要目的是装饰应用程序的UI（例如，使用锁定和解锁图标指示访问状态）。

**可用性：** v4.4.0+

**参数：**

* `PreauthorizeRequest`：用于定义请求的生成器对象
* `AccessEnablerCallback`：用于返回API响应的回调
* `PreauthorizeResponse`：用于返回API响应内容的对象

### 类PreauthorizeRequestBuilder {#preath-req-builder-class}

#### setResources(resources： string[])： PreauthorizeRequestBuilder {#set-res-preath-req-buildr}

* 设置要获取预授权决策的资源的列表。
* 必须为使用预授权API设置此变量。
* 列表中的每个元素都必须是一个字符串，表示资源ID值或必须与MVPD一致的媒体RSS片段。
* 此方法仅在当前`PreauthorizeRequestBuilder`对象实例的上下文中设置信息，该实例是此方法调用的接收方。

* 要构建实际的`PreauthorizeRequest`，您可以查看`PreauthorizeRequestBuilder`的方法：

```JavaScript
  build(): PreauthorizeRequest
```

* `@param {string[]}`资源。 要获取预授权决策的资源的列表。
* `@returns {PreauthorizeRequestBuilder}`对同一`PreauthorizeRequestBuilder`对象实例的引用，该对象实例是方法调用的接收方。
* 这样做是为了允许创建方法链接。

#### disableFeatures(...features： string[])： PreauthorizeRequestBuilder {#disabl-featres-preauth-req-buildr}

* 设置要在获取预授权决策时禁用这些功能的功能。
* 此函数仅在当前`PreauthorizeRequestBuilder`对象实例的上下文中设置信息，该实例是此函数调用的接收方。
* 要构建实际的`PreauthorizeRequest`，您可以查看`PreauthorizeRequestBuilder`的函数：

```JavaScript
public func build() -> PreauthorizeRequest
```

* `@param {string[]}`功能。 要禁用这些功能的功能集。
* `@returns`对同一`PreauthorizeRequestBuilder`对象实例的引用，该对象实例是函数调用的接收方。
* 这样做是为了允许创建功能链接。

#### build()： PreauthorizeRequest {#preauth-req}

* 创建并检索新`PreauthorizeRequest`对象实例的引用。
* 此方法在每次调用时都会实例化新的`PreauthorizeRequest`对象。
* 此方法使用当前`PreauthorizeRequestBuilder`对象实例（此方法调用的接收方）的上下文中预先设置的值。
* 请记住，这个方法不会产生任何副作用，
* 因此，它不会更改SDK的状态或`PreauthorizeRequestBuilder`对象实例（此方法调用的接收方）的状态。
* 这意味着对同一接收器的后续此方法调用将创建不同的新`PreauthorizeRequest`对象实例，但具有相同的信息，以防值设置为`PreauthorizeRequestBuilder`，且未在调用之间修改。
* 如果您不需要更新任何提供的信息（资源和缓存），则可以将PreauthorizeRequest实例重用于预授权API的多个用途。
* `@returns {PreauthorizeRequest}`

### 接口AccessEnablerCallback&lt;T> {#interface-access-enablr-callback}

#### onResponse（结果： T）； {#on-response-result}

* 完成预授权API请求后，SDK调用的响应回调。
* 结果为成功或包含状态的错误结果。
* `@param {T} result`

#### onFailure（结果： T）； {#on-failure-result}

* 当预授权API请求无法得到服务时，SDK调用的失败回调。
* 结果是一个包含状态的失败结果。
* `@param {T} result`

### 类PreauthorizeResponse {#preauth-response-class}

#### 公共地位：地位； {#public-status}

* 返回：失败时的其他状态（状态）信息。
* 可能包含`null`值。

#### 公共决策：决策[]；{#public-decisions}

* 返回：预授权决策的列表。 每个资源一个决策。
* 如果失败，列表可能为空。

### 类状态 {#class-status}

#### 公共地位：号码； {#public-status-numbr}

* RFC 7231中记录的HTTP响应状态代码。
* 如果`Status`来自SDK而不是Adobe Pass身份验证服务，则可能为0。

#### 公共代码：号码； {#public-code-numbr}

* 标准Adobe Pass身份验证服务错误代码。
* 可能包含空字符串或`null`值。

#### 公共消息：字符串； {#public-msg-string}

* 在某些情况下由MVPD授权端点或程序员降级规则提供的详细消息。
* 可能包含空字符串或`null`值。

#### 公共详细信息：字符串； {#public-details-strng}

* 保存详细消息，在某些情况下，该消息由MVPD授权端点或程序员降级规则提供。
* 可能包含空字符串或`null`值。


#### public helpUrl： string； {#public-help-url-string}

* 此URL链接到有关出现此状态/错误的原因和可能解决方案的更多信息。
* 可能包含空字符串或`null`值。

#### 公共跟踪：字符串； {#public-trace-string}

* 此响应的唯一标识符，在联系支持人员以识别更复杂场景中的特定问题时，可以使用此标识符。
* 可能包含空字符串或`null`值。

#### 公共行动：字符串； {#public-action-string}

* 建议采取的补救措施。
   * **无**：很遗憾，没有预定义操作可修复此问题。 这可能表明对公共API的调用不正确
   * **配置**：需要通过TVE仪表板或联系支持部门更改配置。
   * **application-registration**：应用程序必须重新注册自身。
   * **身份验证**：用户必须验证或重新验证。
   * **授权**：用户必须获得特定资源的授权。
   * **降级**：应该应用某种形式的降级。
   * **重试**：重试请求可能会解决此问题
   * ****&#x200B;后重试：在指定的时间段后重试请求可能会解决此问题。
* 可能包含空字符串或`null`值。

### 类别决策 {#class-decision}

#### 公共id：字符串； {#public-id-string}

* 为其获取决策的资源ID。

#### 公共授权：布尔型； {#public-auth-boolean}

* 指示决策是否成功的标志值。

#### 公共错误：状态； {#public-error-status}

* 发生某些错误时的其他状态（状态）信息。 可能包含`null`值。

## 客户端实施示例 {#client-imp-example}

```JavaScript
let accessEnablerApi = new window.AccessEnabler.AccessEnabler("software statement");
let accessEnablerModels = window.AccessEnabler.models;



// Build request
let requestBuilder = new accessEnablerModels.PreauthorizeRequest.getBuilder();
let request = requestBuilder
    .setResources(["RES01", "RES02", "RES03"])
    .disableFeatures("LOCAL_CACHE")
    .build();



// Create callback
let callback = {
    onResponse(response) {
        // Handle onResponse
    },
    onFailure(response) {
        // Handle onFailure
    }
};

// Invoke call
accessEnablerApi.preauthorize(request, callback);
```


## 方案示例 {#scenario-examples}

### 方案1：所有请求的资源均已获得授权 {#all-req-res-auth}

<table>
<thead>
  <tr>
    <th>增强的错误代码标志</th>
    <th>响应</th>
  </tr>
</thead>
<tbody>
<tr>
    <td>已禁用</td>
    <td>

```JavaScript
        {
    "decisions": [
        {
        "id": "RES01",
        "authorized": true
        },
        {
        "id": "RES02",
        "authorized": true
        },
        {
        "id": "RES03",
        "authorized": true
        }
    ]
    }    
```

</td>
  </tr>
</tbody>


### 方案2：一些请求的资源已获得授权。 {#sm-req-res-auth}

<table>
<thead>
  <tr>
    <th>增强的错误代码标志</th>
    <th>响应</th>
  </tr>
</thead>
<tbody>
<tr>
    <td>已禁用</td>
    <td>

    ``JavaScript
    
    {
    `decisions`： [
    {
    `id`： &quot;RES01&quot;，
    `authorized&quot;： true
    }，
    {
    `id`： &quot;RES02&quot;，
    `authorized&quot;： false
    }，
    {
    `id`： &quot;RES03&quot;，
    `authorized&quot;： true
    }
    ]
    }
    
    `

</td>
  </tr>

<tr>
    <td>已启用</td>
    <td>

    ``JavaScript
    {
    `decisions`： [
    {
    `id`： &quot;RES01&quot;，
    `authorized&quot;： true
    }，
    {
    `id`： &quot;RES02&quot;，
    `authorized&quot;： false，
    `error&quot;： {
    `status&quot;： 403，
    `code&quot;： &quot;preauthorization_denied_by_mvpd&quot;，
    `message&quot;： &quot;MVPD在请求预授权时返回了\&quot;拒绝决定指定的资源。”，
    “helpUrl”： &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;，
    &quot;action&quot;： &quot;none&quot;
    }
    }，
    {
    &quot;id&quot;： &quot;RES03&quot;，
    &quot;authorized&quot;： true
    }，
    ]
    }
    
    &quot;&#39;

</td>
  </tr>
</tbody>


### 方案3：所请求的资源均未获得授权。 {#none-req-res-auth}

<table>
<thead>
  <tr>
    <th>增强的错误代码标志</th>
    <th>响应</th>
  </tr>
</thead>
<tbody>
<tr>
    <td>已禁用</td>
    <td>

    ``JavaScript
    
    {
    `decisions`： [
    {
    `id`： &quot;RES01&quot;，
    `authorized&quot;： false
    }，
    {
    `id`： &quot;RES02&quot;，
    `authorized&quot;： false
    }，
    {
    `id`： &quot;RES03&quot;，
    `authorized&quot;： false
    }
    ]
    }
    
    `

</td>
  </tr>

<tr>
    <td>已启用</td>
    <td>

    ``JavaScript
    
    {
    `decisions`： [
    {
    `id`： &quot;RES01&quot;，
    `authorized&quot;： false，
    `error&quot;： {
    `status&quot;： 403，
    `code&quot;： &quot;preauthorization_denied_by_mvpd&quot;，
    `message&quot;： &quot;MVPD在请求对指定资源的预授权时返回了\&quot;Deny\&quot;决定。&quot;，
    `helpUrl`： &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;，
    `action&quot;： &quot;none&quot;
    }
    }，
    {
    &quot;id&quot;： &quot;RES02&quot;，
    &quot;authorized&quot;： false，
    &quot;error&quot;： {
    &quot;status&quot;： 403，
    &quot;code&quot;： &quot;preauthorization_denied_by_mvpd&quot;，
    &quot;message&quot;： &quot;MVPD在请求指定资源的预授权时返回了\&quot;Deny\&quot;决定。&quot;，
    &quot;helpUrl&quot;： &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;，
    &quot;action&quot;： &quot;none&quot;
    }
    }，
    {
    &quot;id&quot;： &quot;RES03&quot;，
    &quot;authorized&quot;： false，
    &quot;error&quot;： {
    &quot;status&quot;： 403，
    &quot;code&quot;： &quot;maximum_execution_time_exceeded&quot;，
    &quot;message&quot;： &quot;请求未在允许的最长时间内完成。 重试请求可能会解决此问题。”，
    “helpUrl”：“https://experienceleague.adobe.com/docs/primetime/authentication/home.html”，
    “action”：“retry”
    }
    }
    ]
    
    
    ”&#39;

</td>
  </tr>
</tbody>


### 场景4：错误的客户端请求 — 未指定资源。 {#bad-cl-req-no-res-sp}

<table>
<thead>
  <tr>
    <th>增强的错误代码标志</th>
    <th>响应</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>已禁用/已启用</td>
    <td>

    &quot;&#39;JavaScript
    {
    &quot;状态&quot;： {
    &quot;状态&quot;： 400，
    &quot;代码&quot;： &quot;internal_error&quot;，
    &quot;message&quot;： &quot;由于内部错误，请求失败。&quot;，
    &quot;details&quot;： &quot;Required String[]参数&quot;resource&quot;不存在&quot;，
    &quot;helpUrl&quot;： &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;，
    &quot;action&quot;： &quot;none&quot;
    }，
    &quot;decisions&quot;： []
    }
    &quot;

</td>
  </tr>
</tbody>
</table>

### 场景5：错误的客户端请求 — 指定的资源为空。 {#bad-cl-req-empt-res-sp}

<table>
<thead>
  <tr>
    <th>增强的错误代码标志</th>
    <th>响应</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>已禁用/已启用</td>
    <td>

    ``JavaScript
    {
    `状态&quot;： {
    `状态&quot;： 412，
    `代码&quot;： &quot;missing_resource&quot;，
    `message&quot;： &quot;缺少资源参数&quot;，
    `helpUrl&quot;： &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;，
    `action&quot;： &quot;none&quot;
    }，
    `decisions&quot;： []
    }
    ``&#39;

</td>
  </tr>
</tbody>
</table>

### 场景6：网络错误。 {#ntwrk-error}

<table>
<thead>
  <tr>
    <th>增强的错误代码标志</th>
    <th>响应</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>已启用</td>
    <td>

    ``JavaScript
    {
    `decisions`： [
    {
    `id`： &quot;RES01&quot;，
    `authorized&quot;： false，
    `error&quot;： {
    `status&quot;： 403，
    `code&quot;： &quot;network_received_error&quot;，
    `message&quot;： &quot;从关联的合作伙伴服务检索响应时出现读取错误。 重试请求可能会解决此问题。&quot;，
    &quot;helpUrl&quot;： &quot;https://experienceleague.adobe.com/docs/primetime/authentication/home.html&quot;，
    &quot;action&quot;： &quot;retry&quot;
    }
    }，
    {
    &quot;id&quot;： &quot;RES02&quot;，
    &quot;authorized&quot;： false，
    &quot;error&quot;： {
    &quot;status&quot;： 403，
    &quot;code&quot;： &quot;network_received_error&quot;，
    &quot;message&quot;： &quot;从关联的合作伙伴服务检索响应时出现读取错误。 重试请求可能会解决此问题。”，
    “helpUrl”：“https://experienceleague.adobe.com/docs/primetime/authentication/home.html”，
    “action”：“retry”
    }
    }
    ]
    
    ”&#39;

</td>
  </tr>
</tbody>
</table>

### 场景7：在没有有效AuthN会话的情况下调用预授权流。

<table>
<thead>
  <tr>
    <th>增强的错误代码标志</th>
    <th>响应</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>已禁用/已启用</td>
    <td>

    ``JavaScript
    {
    `状态&quot;： {
    `状态&quot;： 0，
    `代码&quot;： &quot;authentication_session_missing&quot;，
    `message&quot;： &quot;无法检索与此请求关联的身份验证会话。 用户必须使用支持的MVPD重新进行身份验证才能继续。&quot;，
    &quot;action&quot;： &quot;authentication&quot;
    }，
    &quot;decisions&quot;： []
    }
    
    &quot;&#39;

</td>
  </tr>
</tbody>
</table>



### 情景8：在setRequestor调用完成之前调用了预授权流程

<table>
<thead>
  <tr>
    <th>增强的错误代码标志</th>
    <th>响应</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>已禁用/已启用</td>
    <td>

    &quot;&#39;JavaScript
    {
    &quot;状态&quot;： {
    &quot;状态&quot;： 0，
    &quot;代码&quot;： &quot;requestor_not_configured&quot;，
    &quot;消息&quot;： &quot;尚未配置请求者，这是使用除setRequestor API之外的任何API的先决条件。&quot;，
    &quot;操作&quot;： &quot;retry&quot;
    }，
    &quot;decisions&quot;： []
    }
    &quot;&#39;

</td>
  </tr>
</tbody>
</table>
