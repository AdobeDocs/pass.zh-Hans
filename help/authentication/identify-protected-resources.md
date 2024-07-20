---
title: 识别受保护的资源
description: 识别受保护的资源
exl-id: e96aea02-54b2-491d-ba91-253c0d0e681c
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '255'
ht-degree: 0%

---

# 识别受保护的资源 {#identifying-protected-resources}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## 概述 {#overview}

每个授权请求（或检查授权的请求）都必须包含用户请求访问的受保护资源的唯一标识符。 根据MVPD和参与程序员的协议，受保护的资源可以是任何级别的授权内容。 潜在的受保护资源必须适应这种粒度越来越具体的树结构：

- 网络
   - 渠道
      - 显示
         - 集
            - 资产

</br>

## 媒体RSS格式 {#media_rss}

资源可以通过简单字符串（渠道的唯一标识符）进行标识，也可以以Media RSS格式(MRSS)表示，如Adobe(或Adobe Pass身份验证授权合作伙伴)与参与的MVPD和程序员之间商定的那样。 用作资源说明符的RSS字符串可以包含其他信息，如评级和家长控制元数据。


如果您使用简单资源标识符（如“TNT”），则假定它代表一个渠道，并将其转换为此RSS资源说明符：

```RSS
    <rss version="2.0"> 
        <channel>
            <title>TNT</title>
        </channel>
    </rss>
```


例如，更复杂的说明符可能包括其他评级信息。 您可以将整个RSS字符串传递到需要资源ID的Access Enabler函数，例如[`getAuthorization()`](/help/authentication/rest-api-reference.md)：

```rss
    var resource = 
        '<rss version="2.0" xmlns:media="http://search.yahoo.com/mrss/"> 
             <channel>
                 <title>TNT</title>
                 <media:rating scheme="urn:mpaa">pg</media:rating>
             </channel>
         </rss>'; 
    getAuthorization(resource);
```

资源说明符对Adobe Pass身份验证是不透明的；它们只是传递到MVPD。 如果MVPD无法识别或无法解析您的资源说明符，则会向Adobe Pass身份验证返回错误，该错误会将错误传递回`tokenRequestFailed()`回调。

<!--
## Related Information {#related}

-  User Metadata
-  Preflight Authorization
-->
