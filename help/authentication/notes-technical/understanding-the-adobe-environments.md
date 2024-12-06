---
title: 了解Adobe环境
description: 了解Adobe环境
exl-id: bb6cf37f-48cd-47bb-b3c2-f7a96e49b12d
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '208'
ht-degree: 0%

---

# 了解Adobe环境 {#understanding-the-adobe-environments}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

提供了描述Adobe环境的官方文档[在预测试中设置环境和测试](/help/authentication/notes-technical/setting-up-your-environment-and-testing-in-prequal.md)：

Adobe环境，总结如下：

Adobe有两个环境：**资格预审**&#x200B;和&#x200B;**版本**。

* 在资格预审环境中，我们正在准备发布新的内部版本。

* 当前的发行版构建在发行版环境中。

每个环境都有两个配置文件：**暂存**&#x200B;和&#x200B;**生产**。

* 暂存配置文件连接到MVPD暂存服务器
* 生产配置文件连接到MVPDs生产配置文件。

之所以具有这两个配置文件，是因为在暂存配置文件中，我们在为新合作伙伴准备上线，他们希望使用即将发布的版本（资格预审）或版本版本（更稳定）来测试系统。

如果合作伙伴想要测试新版本，则需要执行几个其他步骤。 请参阅[在预qual](/help/authentication/notes-technical/setting-up-your-environment-and-testing-in-prequal.md)中设置环境和测试。

通过执行上述步骤，可以确保在资格预审环境中测试即将发布的版本。
