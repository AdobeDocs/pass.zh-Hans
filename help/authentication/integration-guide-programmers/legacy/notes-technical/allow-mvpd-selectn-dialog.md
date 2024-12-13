---
title: 在“选择”对话框中允许MVPD
description: 在“选择”对话框中允许MVPD
exl-id: 2c0e0f06-ddc6-4bea-90dc-d7ef8e78d27e
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '151'
ht-degree: 0%

---

# （旧版）在“选择”对话框中允许MVPD {#allow-mvpds-selection-dialog}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

>[!IMPORTANT]
>
> 确保随时了解汇总在[产品公告](/help/authentication/product-announcements.md)页中的最新Adobe Pass身份验证产品公告和停用时间表。

## 问题 {#issue}

在对最终用户公开之前，程序员可能需要测试或检查新MVPD集成的用户体验。

## 解决方案 {#solution}

在`displayProviderDialog()`回调中，Adobe Pass身份验证返回与所选程序员（请求者ID）集成的所有MVPD。 但是，程序员可以在MVPD的返回数组中应用过滤器，并且只显示同时位于两个列表中的那些。

## 示例 {#example}

此示例演示如何在MVPD选择器对话框中仅显示CableCompany_1和CableCompany_2，而不显示CableCompany_NewIntegration。

```C
function displayProviderDialog(mvpdList) {
    var allowlisted = new Array();
    for (var i = 0; i < mvpdList.length; i = i + 1) {
        var currentMvpd = mvpdList[i];
        if ( isAllowListed(currentMvpd.ID) ) {
            allowlisted.push(currentMvpd);
        }
    }
    displayAllowlisted(allowlisted);
}

function isAllowListed(mvpdID) {
    // Implement allowlisting on MVPD IDs.
    return (mvpdID === 'CableCompany_1' || mvpdID === 'CableCompany_2');
}

function displayAllowlisted(list) {
    // TODO: Implement site-specific logic here.
}
```

<!--
**Related Information**
* [Prevent MVPDs from appearing in the Selection Dialog](/help/authentication/prevent-mvpd-selectn-dialog.md)
* **Code Samples**
* [Programmer integration guide](/help/authentication/programmer-integration-guide-overview.md)
-->
