---
title: 证书常见问题解答
description: 证书常见问题解答
exl-id: d4e493b0-4467-42b1-9758-16c5941d8051
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 0%

---

# （旧版）证书常见问题解答 {#certificates-q}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

>[!IMPORTANT]
>
> 确保随时了解汇总在[产品公告](/help/authentication/product-announcements.md)页中的最新Adobe Pass身份验证产品公告和停用时间表。

</br>

**问题1：**&#x200B;是否可以跨iOS和Android注册证书？

**A：** iOS和Android的证书在当前配置中是相同的。 本机证书用于两个平台。

</br>

**问题2：**&#x200B;相同的iOS证书能否用作生产和暂存环境中的主证书和备份证书？ 如果不推荐，您能否提供说明？

**A：**&#x200B;配置与主证书和备份证书相同的证书没有意义。 我们拥有主证书和备份证书的概念，因此我们可以为程序员配置多个证书，以防主证书过期或被吊销。 拥有备份证书将使程序员有时间更改主要证书，而不会影响发布环境。 但是，您可以对生产和暂存配置文件使用同一组主证书和备份证书。

</br>

**问题3：**&#x200B;使用新的灵活TempPass的网页是否需要新的证书？

**A：**&#x200B;证书（以及任何证书）已配置为Media Company &amp; Programmer级别。 FlexibleTempPass是一种MVPD，您无需为其配置任何证书，因此，如果您要将现有程序员与灵活的TempPass集成，将使用已在程序员/媒体公司级别配置的证书。
