---
title: 附录B “调试提示”
description: 附录B “调试提示”
exl-id: ea024797-315e-47c0-99ea-1ac49c8c9697
source-git-commit: 9dc25b66d12b05a8afe16d1a866707880b5d6a51
workflow-type: tm+mt
source-wordcount: '184'
ht-degree: 0%

---

# （旧版）附录B：调试提示 {#appendix-b-debugging-tips}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

>[!IMPORTANT]
>
> 确保随时了解汇总在[产品公告](/help/authentication/product-announcements.md)页中的最新Adobe Pass身份验证产品公告和停用时间表。

## 清除临时数据 {#clearing-temporary-data}

Adobe Pass身份验证存储临时数据，例如浏览器缓存、LSO缓存和Cookie。 清除临时数据非常重要，这可确保您在测试时操作简单明了。

- [清除浏览器缓存和Cookie](#clearing-the-browser-cache-and-cookies)
- [正在清除LSO缓存](#clearing-lsos-cache)


## 清除浏览器缓存和Cookie {#clearing-the-browser-cache-and-cookies}

虽然浏览器可靠，但在Firefox中：“工具” — \>“清除最近历史记录……” — \>在“清除的时间范围：”上选择“所有内容”；在“详细信息”上选中“Cookie”和“缓存” — \>单击“立即清除”。


## 正在清除LSO缓存 {#clearing-lsos-cache}

访问[Flash Player帮助](http://www.macromedia.com/support/documentation/en/flashplayer/help/settings_manager07.html)。

选择```entitlement.\*```（取决于测试的内容）并单击“删除网站”。


## 调试工具 {#tools}

Adobe Pass身份验证工程师使用以下调试工具：

- Firebug - <http://www.getfirebug.com/>
- Flashbug（与Flash Player的调试版本配合使用）
- Fiddler - <http://www.fiddler2.com/fiddler2/>
- Charles - <http://www.charlesproxy.com/>
- Wireshark - <http://www.wireshark.org/>
