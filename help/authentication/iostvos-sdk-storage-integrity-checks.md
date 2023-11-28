---
title: iOS/tvOS存储完整性检查机制
description: iOS/tvOS完整性检查机制
source-git-commit: 444a81ad18afcb26dcf3ae8b41f4d02c465f4655
workflow-type: tm+mt
source-wordcount: '327'
ht-degree: 0%

---

# iOS/tvOS完整性检查机制 {#iostvos-sdk-storage-integrity-checks}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## 介绍 {#Intro}

从iOS/tvOS AccessEnabler SDK版本3.8.3开始，AccessEnabler初始化时提供执行存储完整性检查的选项。

为了使用此机制，对API进行了扩展，为AccessEnabler类提供了额外的初始化方法。

```
- (nonnull id) initWithStorageCheck:(IntegrityCheckType)performIntegrityCheck softwareStatement:(nonnull NSString *)softwareStatement;
```


## 完整性检查 {#Checks}

当怀疑AccessEnabler存储发生损坏时（例如，在读/写存储操作期间发生争用情况），存储完整性检查非常有用。

在AccessEnabler初始化时可以执行以下检查：
- 存储可操作性：检查读写操作是否成功
- 存储的值完整性：检查所有值是否有效并且采用预期格式

>[!IMPORTANT]
> 
>如果其中一项检查失败，则清除存储中的所有值并将用户注销，这可能会导致用户体验不佳。 仅在认为必要时才使用存储完整性检查。


## 默认行为 {#Default}

使用默认初始化方法初始化AccessEnabler时，存储完整性检查默认关闭：

```
///  SWIFT
let accessEnabler: AccessEnabler = AccessEnaler(softwareStatement)

///  Objective C
AccessEnabler *accessEnabler = [[AccessEnabler alloc] init:softwareStatement];
```

要明确指定在AccessEnabler初始化时执行的存储完整性检查，请使用以下初始化方法：

```
///  SWIFT
let accessEnabler: AccessEnabler = AccessEnabler(storageCheck: IntegrityCheckType.INTEGRITY_CHECK_ALL, softwareStatement: softwareStatement)

///  Objective C
AccessEnabler *accessEnabler = [[AccessEnabler alloc] initWithStorageCheck:INTEGRITY_CHECK_ALL softwareStatement:softwareStatement];
```


## Integritychecktype {#Switcher}

IntegrityCheckType枚举向客户端应用程序公开，并具有以下值：

| 值 | 执行的检查 | 已清除存储 | 描述 | 推荐用例 |
|-----------------------|-----------------------------------------------------|-----------------|------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------|
| INTEGRITY_CHECK_NONE | 无 | 从不 | 在存储初始化时不会执行完整性检查 | 当SDK流按预期运行时 |
| INTEGRITY_CHECK_ALL | 存储可操作性 <br/> 存储值的有效性 | 检查时失败 | 在存储初始化时执行所有可用的完整性检查 | 当怀疑SDK存储已损坏时。 <br/> 如果任何完整性检查失败，用户将被注销 |
| INTEGRITY_CHECK_CLEAR | 无 | 始终 | 在存储初始化时，将清除存储 | 当SDK流无法按预期完成时 |
