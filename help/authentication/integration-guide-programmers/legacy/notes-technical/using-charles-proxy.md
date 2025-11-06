---
title: 使用Charles代理
description: 使用Charles代理
exl-id: bb38543f-f6bc-4b5a-91b8-41bc51ee4c56
source-git-commit: 175755aa7463257487b29c5f4da989cf34e91bfd
workflow-type: tm+mt
source-wordcount: '376'
ht-degree: 0%

---

# （旧版）使用Charles代理 {#using-charles-proxy}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

>[!IMPORTANT]
>
> 确保随时了解汇总在[产品公告](/help/authentication/product-announcements.md)页中的最新Adobe Pass身份验证产品公告和停用时间表。

**Charles：** <http://charlesproxy.com>


## 下载、安装并开始使用Charles代理 {#download-install-and-get-stared-with-charles-proxy}

- **下载** - <http://www.charlesproxy.com/download/>
- **安装** - <http://www.charlesproxy.com/documentation/installation/>
- **快速入门** - <http://www.charlesproxy.com/documentation/getting-started/>


## “结构”与“顺序”选项卡 {#structure-vs-sequence-tabs}

查看流量的方式有两种：

1. **结构** — 请求按主机分组
1. **序列** — 请求按其调用顺序列出


## SSL和证书 {#ssl-and-certificates}

启用SSL代理`\[ *Proxy -\> Proxy Settings... -\> SSL* \]`

选中“启用SSL代理”复选框并添加所有HTTPS位置。

<!-- NOTE TO WRITER - THESE IMAGES LINKS ARE BROKEN
![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/ProxySettings.PNG) ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/SSLSettings.PNG) ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/AddHttpsLocations.PNG)
-->

- SSL代理 — <http://www.charlesproxy.com/documentation/proxying/ssl-proxying/>
- SSL证书 — <http://www.charlesproxy.com/documentation/using-charles/ssl-certificates/>
- 从移动设备进行SSL代理 — 请参阅下文。


## 忽略/排除主机 {#ignore-/-exclude-hosts}

如果输出变得过于杂乱，您可以选择忽略或排除位置。您可以通过执行以下任一操作来忽略或排除位置：

- 右键单击要忽略的请求，然后选择“忽略”
- 手动添加要从`\[ *Proxy -\> Recording Settings... -\> Exclude* \]`排除的位置


## DNS欺骗 {#dns-spoffing}

`\[ *Tools -\> DNS Spoofing...* \]`



当尝试将请求重定向到其他IP时，DNS欺骗会非常有用，尤其是在使用移动设备时：

<!-- NOTE TO WRITER - THESE IMAGES LINKS ARE BROKEN
![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/DNSSpoofing.PNG)
-->

<http://www.charlesproxy.com/documentation/tools/dns-spoofing/>


## 映射远程 {#map-remote}

`\[ *Tools -\> Map Remote...* \]`



使用映射远程，您可以将“传入”请求重定向到其他端点。 此功能最常见的使用案例是“映射”`AccessEnabler.swf`到`AccessEnablerDebug.swf:`

<!-- NOTE TO WRITER - THESE IMAGES LINKS ARE BROKEN
![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/MapRemote.PNG) ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/MapRemoteAdd.PNG)
-->

<http://www.charlesproxy.com/documentation/tools/map-remote/>



## 反向代理 {#reverse-proxy}

<http://www.charlesproxy.com/documentation/proxying/reverse-proxy/>

## 移动设备 {#mobile}

### 在iOS设备(iPhone / iPad)上使用Charles {#use-charles-on-an-ios-device-(iphone-/-ipad)}

#### 来自iPhone的SSL连接 {#ssl-connection-from-iphone}

从您的iOS设备浏览到<http://charlesproxy.com/charles.crt>。  这将启动证书安装对话框：

<!-- NOTE TO WRITER - THESE IMAGES LINKS ARE BROKEN
![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceSSLCertificate1\(1\).PNG)![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceSSLCertificate2\(1\).PNG)![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceSSLCertificate3.PNG)
-->

</br>

单击`\[ *Install*... *Install*... *Done* \]`以完成证书安装。

<http://www.charlesproxy.com/documentation/faqs/ssl-connections-from-within-iphone-applications/>



#### 使用iOS设备中的Charles {#using-charles-from-an-ios-device}

在iOS设备上，选择`\[ *Settings* -\> *Wi-FI* -\> (*YOUR\_WIFI\_NETWORK)* \]`。 单击网络旁边的蓝色小箭头，然后向下转到HTTP代理并选择“手动”：


</br>

<!-- NOTE TO WRITER - THESE IMAGES LINKS ARE BROKEN
![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceManualProxy1.png)![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceManualProxy2.PNG)
-->

</br>
在此，您需要指定运行Charles的计算机的IP和端口。 <span style="line-height: 1.6em;">如果您现在在iOS设备上打开Safari并尝试打开网页，则运行Charles的计算机上应该出现以下弹出窗口：

</br>

<!-- NOTE TO WRITER - THESE IMAGES LINKS ARE BROKEN
![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceManualProxy3.PNG)
-->

</br>
单击“允许”以允许设备使用Charles代理其所有
请求。

<http://www.charlesproxy.com/documentation/faqs/using-charles-from-an-iphone/>


#### iOS — 信任任何证书 {#ios-trust-any-certificates}

<http://stackoverflow.com/questions/933331/how-to-use-nsurlconnection-to-connect-with-ssl-for-an-untrusted-cert>

#### iOS身份验证错误 — 找不到adobepass.ios.app

<https://tve.zendesk.com/entries/22135907-ios-authentication-error-adobepass-ios-app-cannot-be-found>


## 将Charles用于Android

<http://www.charlesproxy.com/documentation/configuration/browser-and-system-configuration>


从您的Android设备浏览到[Charles代理](http://charlesproxy.com/charles.crt)。
