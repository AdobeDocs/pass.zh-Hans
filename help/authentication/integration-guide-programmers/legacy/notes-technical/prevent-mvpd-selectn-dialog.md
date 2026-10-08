---
title: 防止在选择对话框中显示MVPD
description: 防止在选择对话框中显示MVPD
exl-id: 20faf501-c006-45e2-a725-fb1273ecaffe
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '128'
ht-degree: 0%
---
# （旧版）防止MVPD出现在“选择”对话框中

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

>[!IMPORTANT]
>
> 确保随时了解汇总在[产品公告](/help/authentication/product-announcements.md)页中的最新Adobe Pass身份验证产品公告和停用时间表。

## 问题 {#issue-prevent-mvpd-sel-dialog}

您需要防止在MVPD选择器中显示特定于（“阻止列表”）的MVPD。


## 解决方案 {#solution-prevent-mvpd-sel-dialog}

解决方案是在调用`displayProviderDialog()`时执行阻止列表。

例如，如果您希望CableCompany_1和CableCompany_2不要显示在MVPD选择器中，则可以执行以下示例中显示的操作。

```C
function displayProviderDialog(mvpdList) {
    var allowlisted = new Array();
    for (var i = 0; i < mvpdList.length; i = i + 1) {
        var currentMvpd = mvpdList[i];
        if (!isBlocklisted(currentMvpd.ID)) {
            allowlisted.push(currentMvpd);
        }
    }
    displayAllowlisted(allowlisted);
}

function isBlocklisted(mvpdID) {
    // Implement block-listing on MVPD IDs.
    return (mvpdID === 'CableCompany_1' || mvpdID === 'CableCompany_2');
}

function displayAllowlisted(list) {
    // TODO: Implement site-specific logic here.
} 
```
