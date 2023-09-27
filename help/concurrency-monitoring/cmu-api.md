---
title: API概述
description: API概述
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '2054'
ht-degree: 0%

---


# 并发监控使用情况API {#cmu-api-usage}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## API概述 {#api-overview}

并发监控使用(CMU)作为WOLAP（基于Web）实现 [在线分析处理](http://en.wikipedia.org/wiki/Online_analytical_processing))项目。 CMU是由数据仓库支持的通用业务报告Web API。 它用作HTTP查询语言，使典型的OLAP操作能够以RESTfully执行。


>[!NOTE]
>
>CMU API并非一般可用。 有关可用性问题，请联系您的Adobe代表。

CMU API提供了基础OLAP多维数据集的分层视图。 每个资源([维度](/help/authentication/entitlement-service-monitoring-overview.md#progr-filter-metrics) 在维度层次结构中，映射为URL路径区段)生成包含（汇总）的报表 [量度](/help/authentication/entitlement-service-monitoring-overview.md#programmers-can-monitor-the-following-metrics) 用于当前选择。 每个资源都指向其父资源（用于累计）及其子资源（用于向下钻取）。 切片和切片是通过将维度固定到特定值或范围的查询字符串参数实现的。

REST API根据维度路径、提供的过滤器和所选的量度，在请求中指定的时间间隔（如果未提供，则回退到默认值）内提供可用数据。 时间范围不适用于不包含时间维度（年、月、日、小时、分钟、秒）的报表。

端点URL根路径将返回单个记录中的总体汇总量度，以及指向可用深入分析选项的链接。 API版本被映射为端点URI路径的尾随区段。 例如， https://mgmt.auth.adobe.com/cmu/*v2* 表示客户端将访问WOLAP版本2。

可通过响应中包含的链接发现可用的URL路径。 保持有效的URL路径以映射基础向下钻取树中包含（预先）聚合度量的路径。 采用/dimension1/dimension2/dimension3格式的路径将反映这三个维度（相当于SQL子句GROUP BY dimension1、dimension2和dimension3）的预聚合。 如果此类预聚合不存在，并且系统无法动态计算它，则API将返回“404未找到”响应。

### 向下钻取树 {#drill-down-tree}

以下深入分析树说明了CMU 2.0中可用的维度（资源）：

**CM租户可用的Dimension**

![](assets/new_breakdown.png)

A `GET` 到 `https://mgmt.auth.adobe.com/cmu/v2` API端点将返回包含以下内容的表示形式：

* 可用根向下钻取路径的链接：

  ```html
  <link rel="drill-down" href="/cmu/v2/dimensionA"/>
  <link rel="drill-down" href="/cmu/v2/dimensionB"/>
  ```

* 所有量度的概要（聚合值）（在默认间隔内，由于未提供查询字符串参数，请参见下文）。

遵循向下钻取路径（逐步）： /dimensionA/year/month/day/dimensionX会检索以下响应：

* 指向“dimensionY”和“dimensionZ”向下钻取选项的链接
* 包含dimensionX每个值的每日聚合的报表

### **过滤器**

除日期/时间维度外，任何可用于当前投影的维度（维度路径）都可以通过将其名称用作查询字符串参数进行筛选。

可以使用以下筛选选项：

* **等于** 过滤器通过将维度名称设置为查询字符串中的特定值来提供。
* **在** 通过多次添加具有不同值的相同dimension-name参数，可以指定过滤器： dimension=value1&amp;dimension=value2
* **不等于** 筛选器必须使用“！” 维度名称后的符号将导致“！”=&#39; &quot;operator&quot;： dimension！=value
* **不在……之内** 过滤器需要&#39;！=&#39;运算符多次使用，一次用于集合中的每个值：维度！=value1&amp;dimension！=value2&amp;...


查询字符串中的维名称也有特殊用法：如果维名称用作无值的查询字符串参数，这将指示API返回报表中包含该维的投影。

示例CMU查询：

| URL | SQL等效项 |
|:---|:---|
| /dimension1/dimension2/dimension3？dimension1=值1 | 从投影中选择*，其中dimension1 = &#39;value1&#39;分组依据dimension1、dimension2、dimension3 |
| /dimension1/dimension2/dimension3？dimension1=value1&amp;dimension1=value2 | 从投影中选择*，其中dimension1在(&#39;value1&#39;， &#39;value2&#39;)分组依据dimension1， dimension2， dimension3 |
| /dimension1/dimension2/dimension3？dimension1！=value1 | 从投影中选择*，其中dimension1 &lt;> &#39;value1&#39;按dimension1、dimension2、dimension3分组 |
| /dimension1/dimension2/dimension3？dimension1！=value1&amp;dimension2！=value2 | 从投影中选择*，其中dimension1不在(&#39;value1&#39;， &#39;value2&#39;)分组依据dimension1， dimension2， dimension3 |
| 假定没有直接路径： /dimension1/dimension3，但存在路径： /dimension1/dimension2/dimension3  </br></br> /dimension1？dimension3 | 从投影分组中选取*，按维1，维3 |

>[!NOTE]
>
>这些筛选技术均不适用于日期/时间维度。 筛选日期/时间维度的唯一方法是将开始和结束查询字符串参数（如下所述）设置为所需的值。

以下查询字符串参数对API具有保留的含义（因此它们不能用作维度名称，否则不可能对此类维度进行过滤）。

CMU API保留的查询字符串参数：

| 参数 | 可选 | 描述 | 默认值 | 示例 |
|:--------------|:--------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------|:------------------------------------------|
| access_token | 是 | 如果启用了IMS OAuth保护，则可以将IMS令牌作为标准授权持有者令牌或查询字符串参数传递。 | 无 | access_token=XXXXXX |
| dimension-name | 是 | 任何维度名称 — 包含在当前URL路径或任何有效子路径中；该值将被视为等于过滤器。 如果未提供值，这将强制将指定的尺寸包含在输出中，即使它未包含或与当前路径相邻也是如此 | 无 | someDimension=someValue&amp;someOtherDimension |
| 结束 | 是 | 报告的结束时间（以毫秒为单位） | 服务器的当前时间 | end=2012-07-30 |
| 格式 | 是 | 用于内容协商（具有相同的效果，但优先级低于路径“扩展” — 请参阅下文）。 | 无：内容协商将尝试其他策略 | format=json |
| limit | 是 | 要返回的最大行数 | 如果未在请求中指定限制，则服务器在自链接中报告的默认值 | limit=1500 |
| 量度 | 是 | 要返回的量度名称列表（以逗号分隔）；这应当用于筛选可用量度的子集（以减小有效负载大小），并且还用于强制API返回包含所请求量度的投影（而不是默认的最佳投影）。 | 如果未提供此参数，则将返回可用于当前投影的所有量度。 | metrics=m1，m2 |
| 开始 | 是 | 报表的开始时间设置为ISO8601；如果仅提供前缀，服务器将填充剩余部分：例如，start=2012将导致开始时间=2012-01-01:00:00:00 | 服务器在自链接中报告；服务器尝试根据选定的时间粒度提供合理的默认值 | start=2012-07-15 |


当前唯一可用的HTTP方法是GET。 未来版本中可能会提供对OPTIONS/HEAD方法的支持。



## CMU API状态代码 {#cmu-api-status-codes}

| 状态代码 | 原因短语 | 描述 |
|:-----------|:---------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 200 | 确定 | 响应将包含“汇总”和“向下展开”链接（如果适用）。 报告将作为资源的属性呈现：嵌套的“report”元素/属性。 |
| 400 | 错误请求 | 响应正文将包含一条文本消息，说明请求的问题。  400 Bad Request状态在响应正文中（纯/文本媒体类型）随附说明文本，提供有关客户端错误的有用信息。 除了应用于非现有维度的无效日期格式或过滤器等琐碎情况之外，系统还将拒绝响应需要动态返回或聚合大量数据的查询。 |
| 401 | 未授权 | 由请求导致的，该请求不包含正确的OAuth标头，用于对用户进行身份验证 |
| 403 | 禁止 | 指示在当前安全上下文中不允许该请求；当用户通过身份验证但无权访问请求的信息时，会发生这种情况 |
| 404 | 未找到 | 在请求中提供了无效的URL路径时发生。 如果客户端遵循随200个响应提供的“深入分析”/“汇总”链接，则绝不会发生这种情况 |
| 405 | 不允许使用该方法 | 表示在请求中使用了不受支持的方法。 虽然当前仅支持GET方法，但未来版本可能允许HEAD或OPTIONS |
| 406 | 不可接受 | 表示客户端请求的媒体类型不受支持 |
| 500 | 内部服务器错误 | “这绝不应该发生” |
| 503 | 服务不可用 | 表示应用程序或其依赖项中的错误 |

## 数据格式 {#data-formats}

数据采用以下格式：

* JSON（默认）
* XML
* CSV
* HTML（演示目的）


客户端可以使用以下内容协商策略（优先级由列表中的位置给出 — 首先是）：

1. 在URL路径的最后一个段后附加一个“文件扩展名”：例如/cmu/v2/tenant/year/month/day.xml。 如果URL包含查询字符串，则扩展必须位于问号之前： `/cmu/v2/tenant/year/month/day.csv?mvpd=SomeMVPD`
1. 格式查询字符串参数：例如， `/cmu/report?format=json`
1. 标准HTTP接受标头：例如， `Accept: application/xml`

“extension”和查询参数都支持以下值：

* xml
* json
* csv
* html

如果任何策略均未指定任何媒体类型，则默认情况下，API将生成JSON内容。

## 超文本应用程序语言(HAL) {#hypertext-app-lang}

对于JSON和XML，有效负载将编码为HAL，如下所述： `http://stateless.co/hal_specification.html`.

实际报表（称为“报表”的嵌套标记/属性）将由包含所有选定/适用维度和量度及其值的实际记录列表组成，编码如下：

### JSON {#json}

```js
 "report": [
  {
    "dimension1": "d1",
    ...
    "metric1": "m1",
    ...
  }, {
    ...
  }
]
```

### XML {#xml}

```xml
 <report>
  <record dimension1="d1" ... metric1="m1" ... />
  ...
</report>
```

对于XML和JSON格式，记录中字段（维度和量度）的顺序是未指定的，但是一致的（所有记录中的顺序将相同）。 但是，客户不应依赖记录中字段的任何特定顺序。

资源链接（JSON中的“self”rel和XML中的“href”资源属性）包含当前路径以及用于内联报表的查询字符串。 查询字符串将揭示所有隐式和显式参数，以便有效负荷将显式指出使用的时间间隔、隐式过滤器（如果有）等。 资源中的其余链接将包含可以遵循的所有可用区段，以便深入查看当前数据。 此外，还会提供一个汇总链接，该链接将指向父级路径（如果有）。 此 `href` 下钻链接/汇总链接的值仅包含URL路径（它不包括查询字符串，因此如果需要，需要由客户端附加）。 请注意，并非当前资源使用（或默示）的所有查询字符串参数都适用于“汇总”或“向下钻取”链接（例如，过滤器可能不适用于子资源或超级资源）。

示例(假设我们有一个称为客户端的量度，并且有一个预聚合 `year/month/day/...`)：

* `https://mgmt.auth.adobe.com/cmu/v2/year/month.xml`

  ```xml
  <resource href="/cmu/v2/year/month?start=2012-07-20T00:00:00&end=2012-08-20T14:35:21">
    <links>
      <link rel="roll-up" href="/cmu/v2/year"/>
      <link rel="drill-down" href="/cmu/v2/year/month/day"/>
    </links>
    <report>
      <record month="6" year="2012" clients="205"/>
      <record month="7" year="2012" clients="466"/>
    </report>
  </resource>
  ```

* `https://mgmt.auth.adobe.com/cmu/v2/year/month.json`

  ```js
  {
    "_links" : {
      "self" : {
        "href" : "/cmu/v2/year/month?start=2012-07-20T00:00:00&end=2012-08-20T14:35:21"
      },
      "roll-up" : {
        "href" : "/cmu/v2/year"
      },
      "drill-down" : {
        "href" : "/cmu/v2/year/month/day"
      }
    },
    "report" : [ {
      "month" : "6",
      "year" : "2012",
      "clients" : "205"
    }, {
      "month" : "7",
      "year" : "2012",
      "clients" : "466"
    } ]
  }
  ```

### CSV {#csv}

在CSV数据格式中，不会内联提供链接或其他元数据（标题行除外），而是以文件名提供选择元数据，文件名将遵循以下模式：

```html
report__<start-date>_<end-date>_<filter-values,...>.csv
```

CSV将包含一个标题行，然后作为后续行包含报表数据。 标题行将包含所有维度，后跟所有量度。 报表数据的排序顺序将反映在维度的顺序中。 因此，如果数据先按D1排序，然后再按D2排序，则CSV标头将如下所示： `D1, D2, ...metrics....`

标题行中字段的顺序将反映表数据的排序顺序。

示例： https://mgmt.auth.adobe.com/cmu/v2/year/month.csv将生成一个名为的文件 ```report__2012-07-20_2012-08-20_1000.csv``` ，内容如下：

| 年 | 月 | 客户端 |
|:----:|:-----:|:-------:|
| 2012 | 6 | 580 |
| 2012 | 7 | 231 |

## 数据新鲜度 {#data-freshness}

尽管请求包含Last-Modified标头，但 **不会** 反映报表在正文中的上次更新时间。 常规报告是按常规计算的，遵循以下规则：

* 如果时间粒度为 **年** 或 **月**，则报告每2天更新一次
* 如果时间粒度为 **天**，则报告每3小时更新一次
* 如果时间粒度为 **小时**，则报告每小时更新一次
* 如果时间粒度为 **分钟**，则报告每分钟更新一次

此 **活动级别** 和 **并发级别** 无论时间粒度如何，报表每天都会更新。

## GZIP压缩 {#gzip-compression}

Adobe强烈建议您在获取CMU报表的客户端中启用gzip支持。 这样做将大大减小响应的大小，从而缩短响应时间。 （CMU数据的压缩比在20-30范围内。）

要在客户端中启用gzip压缩，请按照以下方式设置Accept-Encoding：标头：

```
Accept-Encoding: gzip, deflate
```

## 相关信息 {#related-information}

* [CMU概述](/help/concurrency-monitoring/cm-usage-reports.md)
