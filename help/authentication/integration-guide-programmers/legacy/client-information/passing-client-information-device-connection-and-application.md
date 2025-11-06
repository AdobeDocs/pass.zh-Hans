---
title: 传递客户端信息（设备、连接和应用程序）
description: 传递客户端信息（设备、连接和应用程序）
exl-id: 0b21ef0e-c169-48ff-ac01-25411cfece1e
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '1665'
ht-degree: 1%

---

# （旧版）传递客户端信息（设备、连接和应用程序） {#pass-client-info}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

>[!IMPORTANT]
>
> 确保随时了解汇总在[产品公告](/help/authentication/product-announcements.md)页中的最新Adobe Pass身份验证产品公告和停用时间表。

## 范围 {#pass-client-info-scope}

本文档汇总有关将客户端信息（设备、连接和应用程序）从程序员应用程序传递到Adobe Pass身份验证REST API或SDK的详细信息和指南。

提供客户信息的好处包括：

* 在某些设备类型和可支持HBA的MVPD的情况下，能够正确启用Home Base Authentication (HBA)。
* 在某些设备类型的情况下正确应用TTL的功能（例如，为电视连接设备上的身份验证会话配置较长的TTL）。
* 能够使用授权服务监控(ESM)在各种设备类型中正确汇总细分报表中的业务量度。
* 取消阻止正确应用各种业务规则(例如 降级)。

## 概述 {#pass-client-info-overview}

客户端信息包括：

* **设备**&#x200B;有关用户试图从中使用程序员内容的设备的硬件和软件属性的信息。
* **连接**&#x200B;有关用户从中连接到Adobe Pass身份验证服务和/或程序员服务的设备的连接属性的信息（例如，服务器到服务器实现）。
* **应用程序**&#x200B;有关用户从中尝试使用程序员内容的已注册应用程序的信息。

客户端信息是使用下表提供的键构建的JSON对象。

>[!NOTE]
>
>以下&#x200B;**密钥**&#x200B;是要在客户端信息JSON对象中发送的&#x200B;**必需**： **模型**，**osName**。
>
>以下键具有&#x200B;**个受限制的**&#x200B;值： `primaryHardwareType`、`osName`、`osFamily`、`browserName`、`browserVendor`、`connectionSecure`。

|   | 键 | 受限 | 描述 | 可能值 |
|---|---|---|---|---|
|            | 主要硬件类型 | #是 | 设备的主要硬件类型。 | #值受到限制：                                                                     相机                                                      数据收集终端                                                      桌面                                                      嵌入式网络模块                                                      电子阅读器                                                      游戏控制台                                                      Geolocationtracker                                                      眼镜                                                      MediaPlayer                                                      移动电话                                                      支付终端                                                      插件调制解调器                                                      机顶盒                                                      TV                                                      平板电脑                                                      无线热点                                                      手表                                                      未知 |
| #mandatory | 模型 | 否 | 设备的型号名称。 | 例如iPhone、SM-G930V、AppleTV等。 |
|            | 版本 | 否 | 设备的版本。 | 例如2.0.1等。 |
|            | 制造商 | 否 | 设备的制造公司/组织。 | 例如，三星、LG、ZTE、华为、摩托罗拉、Apple等。 |
|            | 供应商 | 否 | 设备的销售公司/组织。 | 例如Apple、Samsung、LG、Google等。 |
| #mandatory | 操作系统名称 | #是 | 设备的操作系统(OS)名称。 | #值受到限制：                                                   Android                   Chrome操作系统                   Linux                   Mac操作系统                   OS X                   OpenBSD                   Roku OS                   Windows                   iOS                   tvOS                   webOS |
|            | osFamily | 是 | 设备的操作系统(OS)组名称。 | #值受到限制：                                                   Android                   BSD                   Linux                   PlayStation操作系统                   Roku OS                   Symbian                   Tizen                   Windows                   iOS                   macOS                   tvOS                   webOS |
|            | 操作系统供应商 | 否 | 设备的操作系统(OS)供应商。 | Amazon                   Apple                   Google                   LG                   Microsoft                   Mozilla                   任天堂                   Nokia                   Roku                   Samsung                   Sony                   Tizen项目 |
|            | osVersion | 否 | 设备的操作系统(OS)版本。 | 例如10.2、9.0.1等。 |
|            | browserName | #是 | 浏览器的名称。 | #值受到限制：                                                   Android Browser                   Chrome                   Edge                   Firefox                   Internet Explorer                   Opera                   Safari                   SeaMonke                   Symbian Browser |
|            | browserVendor | #是 | 浏览器的构建公司/组织。 | #值受到限制：                                                   Amazon                   Apple                   Google                   Microsoft                   摩托罗拉                   Mozilla                   Netscape                   任天堂                   Nokia                   Samsung                   索尼·爱立信 |
|            | browserVersion | 否 | 设备的浏览器版本。 | 例如60.0.3112 |
|            | userAgent | 否 | 设备的用户代理。 | 例如，Mozilla/5.0(Macintosh；英特尔Mac OS X 10_12_3) AppleWebKit/602.4.8（KHTML，如Gecko）版本/10.0.3 Safari/602.4.8 |
|            | 显示宽度 | 否 | 设备的物理屏幕宽度。 |                                                                                                                                                                                                                                                                                                                                                           |
|            | displayheight | 否 | 设备的物理屏幕高度。 |                                                                                                                                                                                                                                                                                                                                                           |
|            | displayPpi | 否 | 设备的物理屏幕像素密度。 | 例如294 |
|            | 对角屏幕大小 | 否 | 设备的物理屏幕对角尺寸（英寸）。 | 例如5.5、10.1 |
|            | connectionIp | 否 | 用于发送HTTP请求的设备的IP。 | 如8.8.4.4 |
|            | connectionPort | 否 | 用于发送HTTP请求的设备的端口。 | 例如53124 |
|            | connectionType | 否 | 网络连接类型。 | 例如WiFi、LAN、3G、4G、5G |
|            | connectionSecure | #是 | 网络连接安全状态。 | #值受到限制：                                                   true — 在安全网络的情况下                   false — 在公共热点的情况下 |
|            | applicationId | 否 | 应用程序的唯一标识符。 | 例如CNN |

## API引用 {#api-ref}

此部分介绍在使用Adobe Pass身份验证REST API或SDK时负责处理客户端信息的API。

### REST API {#rest-api}

Adobe Pass身份验证服务支持通过以下方式接收客户端信息：

* 作为&#x200B;**标头：“X-Device-Info”**
* 作为&#x200B;**查询参数： &quot;device_info&quot;**
* 作为&#x200B;**post参数： &quot;device_info&quot;**

>[!IMPORTANT]
>
>在所有三种情况下，标头或参数的有效负载必须是&#x200B;**Base64编码和URL编码**。

**SDK**

#### JavaScript SDK {#js-sdk}

AccessEnabler JavaScript SDK默认构建一个客户端信息JSON对象，除非覆盖，否则该对象将传递到Adobe Pass身份验证服务。

AccessEnabler JavaScript SDK仅支持&#x200B;**通过** setRequestor[的](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#setrequestor(inRequestorID,endpoints,options))applicationId *选项参数覆盖客户端信息JSON对象中的“applicationId”键*。

>[!CAUTION]
>
>`applicationId`参数值必须是纯文本字符串值。
>如果程序员应用程序决定传递applicationId，则其余的客户端信息密钥仍由AccessEnabler JavaScript SDK计算。

#### iOS/tvOS SDK {#ios-tvos-sdk}

默认情况下，AccessEnabler iOS/tvOS SDK会构建一个客户端信息JSON对象，除非覆盖，否则该对象将传递到Adobe Pass身份验证服务。

AccessEnabler iOS/tvOS SDK支持&#x200B;**通过** setOptions[的device_info参数覆盖整个](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setoptions)客户端信息JSON对象。

>[!CAUTION]
>
>*device_info*&#x200B;参数值必须为&#x200B;**Base64编码** *NSString*&#x200B;值。
>
>如果程序员应用程序决定传递&#x200B;*device_info*，则将覆盖AccessEnabler iOS/tvOS SDK计算的所有客户端信息密钥。 因此，计算并传递尽可能多的键的值非常重要。 有关实施的更多详细信息，请参阅[概述](#pass-client-info-overview)表和[iOS/tvOS指南](#ios-tvos)。

#### Android/FireOS SDK {#and-fire-os-sdk}

默认情况下，`AccessEnabler`个Android/FireOS SDK会生成一个客户端信息JSON对象，除非覆盖，否则该对象将传递到Adobe Pass身份验证服务。

`AccessEnabler` Android/FireOS SDK支持通过&#x200B;**setOptions**&#x200B;的/[setOptions](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#setOptions)的[参数覆盖整个](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#fire_setOption)客户端信息JSON对象。`device_info`

>[!NOTE]
>
>`device_info`参数值必须为&#x200B;**Base64编码**&#x200B;字符串值。

>[!IMPORTANT]
>
>如果程序员应用程序决定传递`device_info`，则将覆盖`AccessEnabler` Android/FireOS SDK计算的所有客户端信息密钥。 因此，计算并传递尽可能多的键的值非常重要。 有关实现的更多详细信息，请参阅[概述](#pass-client-info-overview)表以及[Android](#android)和[FireOS](#fire-tv)指南。

## 指南 {#cookbooks}

本节介绍一本指南，介绍在不同设备类型的情况下构建客户端信息JSON对象。

>[!IMPORTANT]
>
>标有&#x200B;**的密钥！**&#x200B;是必须发送的。

### Android {#android}

设备信息可通过以下方式构建：

|   | 键 | Source | 值（示例） |
|---|---------------|-----------------------------|---------------|
| ！ | 模型 | Build.MODEL | GT-I9505 |
|   | 供应商 | Build.BRAND | 三星 |
|   | 制造商 | Build.MANUFACTURER | 三星 |
| ！ | 版本 | Build.DEVICE | jflte |
|   | 显示宽度 | DisplayMetrics.widthPixels | 600 |
|   | displayheight | DisplayMetrics.heightPixels | 800 |
| ！ | 操作系统名称 | 硬编码 | Android |
| ！ | osVersion | Build.VERSION.RELEASE | 5.0.1 |

连接信息可通过以下方式构建：

|   | 键 | Source | 值（示例） |
|---|---|---|---|
| ！ | connectionType | `<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>` `getSystemService(Context.CONNECTIVITY_SERVICE).getActiveNetworkInfo().getType()` | `"WIFI","BLUETOOTH","MOBILE","ETHERNET","VPN","DUMMY","MOBILE_DUN","WIMAX","notAccessible"` |
|   | connectionSecure |                                                                                                                                                           |                                                                                           |

应用程序信息可通过以下方式构建：

|   | 键 | Source | 值（示例） |
|---|---------------|-----------|--------------|
|   | applicationId | 硬编码 | CNN |

>[!IMPORTANT]
>
>必须将设备、连接和应用程序信息添加到同一JSON对象中。 之后，生成的对象必须为&#x200B;**Base64编码**。 此外，对于Adobe Pass身份验证REST API，该值必须为&#x200B;**URL编码**。

**示例代码**

```JAVA
private JSONObject computeClientInformation() {
     String LOGGING_TAG = "DefineClass.class";
  
     JSONObject clientInformation = new JSONObject();

     String connectionType;

     try {
          ConnectivityManager cm = (ConnectivityManager) getContext().getSystemService(CONNECTIVITY_SERVICE);
          NetworkInfo activeNetwork = cm.getActiveNetworkInfo();

          if (activeNetwork != null && activeNetwork.isConnectedOrConnecting()) {
              switch (activeNetwork.getType()) {
                    case ConnectivityManager.TYPE_WIFI: {
                        connectionType = "WIFI";
                        break;
                    }
                    case ConnectivityManager.TYPE_BLUETOOTH: {
                        connectionType = "BLUETOOTH";
                        break;
                    }
                    case ConnectivityManager.TYPE_MOBILE: {
                        connectionType = "MOBILE";
                        break;
                    }
                    case ConnectivityManager.TYPE_ETHERNET: {
                        connectionType = "ETHERNET";
                        break;
                    }
                    case ConnectivityManager.TYPE_VPN: {
                        connectionType = "VPN";
                        break;
                    }
                    case ConnectivityManager.TYPE_DUMMY: {
                        connectionType = "DUMMY";
                        break;
                    }
                    case ConnectivityManager.TYPE_MOBILE_DUN: {
                        connectionType = "MOBILE_DUN";
                        break;
                    }
                    case ConnectivityManager.TYPE_WIMAX: {
                        connectionType = "WIMAX";
                        break;
                    }
                    default:
                       connectionType = ConnectivityManager.EXTRA_OTHER_NETWORK_INFO;
              }
          } else {
                connectionType = ConnectivityManager.EXTRA_NO_CONNECTIVITY;
          }
     } catch (Exception e) {
          connectionType = "notAccessible";
     }

     try {
          clientInformation.put("model",Build.MODEL);
          clientInformation.put("vendor", Build.BRAND);
          clientInformation.put("manufacturer",Build.MANUFACTURER);
          clientInformation.put("version",Build.DEVICE);
          clientInformation.put("osName","Android");
          clientInformation.put("osVersion",Build.VERSION.RELEASE);
          clientInformation.put("connectionType",connectionType);
          clientInformation.put("applicationId","CNN");
     } catch (JSONException e) {
          Log.e(LOGGING_TAG, e.getMessage());
     }

     return Base64.encodeToString(clientInformation.toString().getBytes(), Base64.NO_WRAP);
}
```

>[!NOTE]
>
>**资源：**
>* Java开发人员文档中的公共类[生成](https://developer.android.com/reference/android/os/Build.html){target=_blank}。

### FireTV {#fire-tv}

设备信息可通过以下方式构建：

|   | 键 | Source | 值（例如） |
|---|---------------|-----------------------------|--------------|
| ！ | 模型 | Build.MODEL | AFTM |
|   | 供应商 | Build.BRAND | Amazon |
|   | 制造商 | Build.MANUFACTURER | Amazon |
| ！ | 版本 | Build.DEVICE | 蒙托亚 |
|   | 显示宽度 | DisplayMetrics.widthPixels |              |
|   | displayheight | DisplayMetrics.heightPixels |              |
| ！ | 操作系统名称 | 硬编码 | Android |
| ！ | osVersion | Build.VERSION.RELEASE | 5.1.1 |

连接信息可通过以下方式构建：

|   | 键 | Source | 值（示例） |
|---|------------------|--------|---------------|
| ！ | connectionType |        |               |
|   | connectionSecure |        |               |

应用程序信息可通过以下方式构建：

|   | 键 | Source | 值（示例） |
|---|---------------|-----------|--------------|
|   | applicationId | 硬编码 | CNN |

>[!IMPORTANT]
>
>必须将设备、连接和应用程序信息添加到同一JSON对象中。 之后，生成的对象必须为&#x200B;**Base64编码**。 此外，对于Adobe Pass身份验证REST API，该值必须为&#x200B;**URL编码**。

>[!NOTE]
>
>**资源：**
>* Android开发人员文档中的公共类[生成](https://developer.android.com/reference/android/os/Build.html){target=_blank}。
>* [识别FireTV设备](https://developer.amazon.com/docs/fire-tv/identify-amazon-fire-tv-devices.html){target=_blank}

### iOS/tvOS {#ios-tvos}

设备信息可通过以下方式构建：

|   | 键 | Source | 值（示例） |
|---|---------------|------------------------|--------------|
| ！ | 模型 | uname.machine | iPhone |
|   | 供应商 | 硬编码 | Apple |
|   | 制造商 | 硬编码 | Apple |
| ！ | 版本 | uname.machine | 8,1 |
|   | 显示宽度 | UIScreen.mainScreen | 320 |
|   | displayheight | UIScreen.mainScreen | 568 |
| ！ | 操作系统名称 | UIDevice.systemName | iOS |
| ！ | osVersion | UIDevice.systemVersion | 10.2 |

连接信息可通过以下方式构建：

|   | 键 | Source | 值（示例） |
|---|------------------|-------------------------------------------|--------------|
| ！ | connectionType | [可达性currentReachabilityStatus] |              |
|   | connectionSecure |                                           |              |


应用程序信息可通过以下方式构建：

|   | 键 | Source | 值（示例） |
|---|---------------|-----------|--------------|
|   | applicationId | 硬编码 | CNN |

>[!IMPORTANT]
>
>必须将设备、连接和应用程序信息添加到同一JSON对象中。 之后，生成的对象必须为Base64编码。 此外，对于Adobe Pass身份验证REST API，该值必须为URL编码。

**示例代码**

```C
+ (NSString *)computeClientInformation {        
        struct utsname u;
        uname(&u);

        NSString *hardware = [NSString stringWithCString:u.machine encoding:NSUTF8StringEncoding];

        UIDevice *device = [UIDevice currentDevice];

        NSString *deviceType;

        switch (UI_USER_INTERFACE_IDIOM()) {
            case UIUserInterfaceIdiomPhone:
                deviceType = @"MobilePhone";
                break;
            case UIUserInterfaceIdiomPad:
                deviceType = @"Tablet";
                break;
            case UIUserInterfaceIdiomTV:
                deviceType = @"TV";
                break;
            default:
                deviceType = @"Unknown";
        }

        CGRect screenRect = [[UIScreen mainScreen] bounds];
        NSNumber *screenWidth = @((float) screenRect.size.width);
        NSNumber *screenHeight = @((float) screenRect.size.height);

        Reachability *reachability = [Reachability reachabilityForInternetConnection];
        [reachability startNotifier];

        NetworkStatus status = [reachability currentReachabilityStatus];

        NSString *connectionType;

        if (status == NotReachable) {
            connectionType = @"notConnected";
        } else if (status == ReachableViaWiFi) {
            connectionType = @"WiFi";
        } else if (status == ReachableViaWWAN) {
            connectionType = @"cellular";
        }

        NSMutableDictionary *clientInformation = [@{
                @"type": deviceType,
                @"model": device.model,
                @"vendor": @"Apple",
                @"manufacturer": @"Apple",
                @"version": [hardware stringByReplacingOccurrencesOfString:device.model withString:@""],
                @"osName": device.systemName,
                @"osVersion": device.systemVersion,
                @"displayWidth": screenWidth,
                @"displayHeight": screenHeight,
                @"connectionType": connectionType,
                @"applicationId": @"CNN" 
        } mutableCopy];

        NSError *error;
        NSData *jsonData = [NSJSONSerialization dataWithJSONObject:clientInformation options:NSJSONWritingPrettyPrinted error:&error];
        NSString *base64Encoded = [jsonData base64EncodedStringWithOptions:0];

        return base64Encoded;
}
```

>[!NOTE]
>
>**资源：**
>* [UIDevice](https://developer.apple.com/documentation/uikit/uidevice#//apple_ref/occ/cl/UIDevice){target=_blank}
>* [uname](https://man7.org/linux/man-pages/man2/uname.2.html){target=_blank}
>* [关于可达性](https://developer.apple.com/library/archive/samplecode/Reachability/Introduction/Intro.html){target=_blank}

### Roku {#roku}

设备信息可通过以下方式构建：

| 键 | Source | 值（示例） |                 |
|-----|---------------|--------------------------------------------|-----------------|
| ！ | 模型 | 硬编码 | “Roku” |
|     | 供应商 | ifDeviceInfo.GetModelDetails().VendorName | “Sharp”、“Roku” |
|     | 制造商 | ifDeviceInfo.GetModelDetails().VendorName | “Sharp”、“Roku” |
| ！ | 版本 | ifDeviceInfo.GetModelDetails().ModelNumber | “5303X” |
|     | 显示宽度 | ifDeviceInfo.GetDisplaySize().w | 1920 |
|     | displayheight | ifDeviceInfo.GetDisplaySize().h | 1080 |
| ！ | 操作系统名称 | 硬编码 | “Roku” |
| ！ | osVersion | ifDeviceInfo.getVersion() |                 |

连接信息可通过以下方式构建：

|   | 键 | Source | 值（示例） |
|---|---|---|---|
| ！ | connectionType | ifDeviceInfo.GetConnectionType() | “WifiConnection”、“WiredConnection” |
|   | connectionSecure | 硬编码 | 如果连接是有线的，则为true |

应用程序信息可通过以下方式构建：

|   | 键 | Source | 值（示例） |
|---|---------------|-----------|--------------|
|   | applicationId | 硬编码 | CNN |

>[!IMPORTANT]
>
>必须将设备、连接和应用程序信息添加到同一JSON对象中。 之后，生成的对象必须为&#x200B;**Base64编码**。 此外，对于Adobe Pass身份验证REST API，该值必须为URL编码。

>[!NOTE]
>
>有关详细信息，请参阅[ifDeviceInfo](https://developer.roku.com/docs/references/brightscript/interfaces/ifdeviceinfo.md)

### XBOX 1/360 {#xbox}

设备信息可通过以下方式构建：

|   | 键 | Source | 值（示例） |
|---|---|---|---|
| ！ | 模型 | EasClientDeviceInformation.SystemProductName |                 |
|   | 供应商 | 硬编码 | Microsoft |
|   | 制造商 | 硬编码 | Microsoft |
| ！ | 版本 | EasClientDeviceInformation.SystemHardwareVersion |                 |
|   | 显示宽度 | DisplayInformation.ScreenWidthInRawPixels | 1920 |
|   | displayheight | DisplayInformation.ScreenHeightInRawPixels | 1080 |
| ！ | 操作系统名称 | EasClientDeviceInformation.OperatingSystem |                 |
| ！ | osVersion | EasClientDeviceInformation.SystemFirmwareVersion |                 |

连接信息可通过以下方式构建：

|   | 键 | Source | 示例 |
|---|---|---|---|
| ！ | connectionType |                                                   |                   |
|   | connectionSecure | NetworkAuthenticationtype | “无”、“Wpa”等 |

应用程序信息可通过以下方式构建：

| 键 | Source | 值（示例） |
|---|---|---|
| applicationId | 硬编码 | CNN |

>[!IMPORTANT]
>
>必须将设备、连接和应用程序信息添加到同一JSON对象中。 之后，生成的对象必须为&#x200B;**Base64编码**。 此外，对于Adobe Pass身份验证REST API，该值必须为&#x200B;**URL编码**。

**资源**

* [EasClientDeviceInformation类](https://docs.microsoft.com/en-us/uwp/api/windows.security.exchangeactivesyncprovisioning.easclientdeviceinformation?view=winrt-22000)
* [DisplayInformation类](https://docs.microsoft.com/en-us/uwp/api/windows.graphics.display.displayinformation?view=winrt-22000)
