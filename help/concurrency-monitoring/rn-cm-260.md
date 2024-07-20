---
title: Adobe Pass Concurrency Monitoring 2.6.0发行说明
description: Adobe Pass Concurrency Monitoring 2.6.0发行说明
exl-id: f24980e3-ffe8-4b5e-8adc-ae443baed40f
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '202'
ht-degree: 0%

---

# Adobe Pass Concurrency Monitoring 2.6.0发行说明 {#cm-260}


本页介绍了此版本的新增功能、更改和已知问题：



## 发行日期： 2016年11月10日



## 新增功能

此版本添加了终止现有流的功能，以允许启动当前流（亦即，终止流）。



**远程终止**

* 在409冲突响应中，建议的“冲突”字段中列出的每个会话都将包含terminationCode属性。
* 系统会提示用户提供冲突会话列表，并允许用户选择要终止的会话
* 远程会话只能通过在会话初始化尝试中传递X-Terminate请求标头（将选定的终止代码作为值）来终止。
* 为410 Gone响应定义了一种新的“建议”，以指示终止了当前响应的会话。


有关更多详细信息，请参阅更新的文档。



>[!NOTE]
>
>更新了用于列出活动会话的会话定义，以包括应用程序名称和设备名称（而不是应用程序ID）。




## 错误修复 {#bug-fixes}

删除了服务器响应中的重复标头（修复操作涉及CORS标头和Date one）。




## 已知问题 {#known-issues}

不适用
