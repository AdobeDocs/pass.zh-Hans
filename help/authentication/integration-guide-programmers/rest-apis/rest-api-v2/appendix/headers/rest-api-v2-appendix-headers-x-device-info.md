---
title: 标头 — X-Device-Info
description: REST API V2 — 标头 — X-Device-Info
exl-id: 0ef25e06-86de-427a-a938-7ba3817f0d5e
source-git-commit: 81d3c3835d2e97e28c2ddb9c72d1a048a25ad433
workflow-type: tm+mt
source-wordcount: '1122'
ht-degree: 2%

---

# 标头 — X-Device-Info {#header-x-device-info}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## 概述 {#overview}

<b>X-Device-Info</b>请求标头包含与实际流设备相关的客户端信息（设备、连接和应用程序）。

## 语法 {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>X — 设备信息</b>： &lt;设备信息&gt;</td>
   </tr>
   <tr>
      <td>标题类型</td>
      <td>请求标头</td>
   </tr>
   <tr>
      <td>标准</td>
      <td>否</td>
   </tr>
</table>

## 指令 {#directives}

<b>&lt;设备信息></b>

JSON元素的`Base64-encoded`值，至少包含下表要求标记的属性。

<table style="table-layout:auto">
    <tr>
        <th style="background-color: #EFF2F7; width: 15%;">存在</th>
        <th style="background-color: #EFF2F7; width: 15%;">键</th>
        <th style="background-color: #EFF2F7;">描述</th>    
        <th style="background-color: #EFF2F7; width: 15%;">受限</th>
        <th style="background-color: #EFF2F7;">可能值</th>
    </tr>
    <tr>
        <td></td>
        <td>主要硬件类型</td>
        <td>设备的主要硬件类型。</td>
        <td>&amp;amp；检查；</td>
        <td>
            值受限制：
            <ul>
                <li>相机</li>
                <li>数据收集终端</li>
                <li>桌面</li>
                <li>嵌入式网络模块</li>
                <li>电子阅读器</li>
                <li>游戏控制台</li>
                <li>Geolocationtracker</li>
                <li>眼镜</li>
                <li>MediaPlayer</li>
                <li>移动电话</li>
                <li>支付终端</li>
                <li>插件调制解调器</li>
                <li>机顶盒</li>
                <li>TV</li>
                <li>平板电脑</li>
                <li>无线热点</li>
                <li>手表</li>
                <li>未知</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td><i>必填</i></td>
        <td>模型</td>
        <td>设备的型号名称。</td>
        <td></td>
        <td>例如iPhone、SM-G930V、AppleTV等。</td>
    </tr>
    <tr>
        <td><i>必填</i></td>
        <td>版本</td>
        <td>设备的版本。</td>
        <td></td>
        <td>例如2.0.1等。</td>
    </tr>
    <tr>
        <td></td>
        <td>制造商</td>
        <td>设备的制造公司/组织。</td>
        <td></td>
        <td>例如，三星、LG、ZTE、华为、摩托罗拉、Apple等。</td>
    </tr>
    <tr>
        <td></td>
        <td>供应商</td>
        <td>设备的销售公司/组织。</td>
        <td></td>
        <td>例如Apple、Samsung、LG、Google等。</td>
    </tr>
    <tr>
        <td><i>必填</i></td>
        <td>操作系统名称</td>
        <td>设备的操作系统(OS)名称。</td>
        <td>&amp;amp；检查；</td>
        <td>
            值受限制：
            <ul>
                <li>Android</li>
                <li>Chrome操作系统</li>
                <li>Linux</li>
                <li>Mac操作系统</li>
                <li>OS X</li>
                <li>OpenBSD</li>
                <li>Roku OS</li>
                <li>Windows</li>
                <li>iOS</li>
                <li>tvOS</li>
                <li>webOS</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>osFamily</td>
        <td>设备的操作系统(OS)组名称。</td>
        <td>&amp;amp；检查；</td>
        <td>
            值受限制：
            <ul>
                <li>Android</li>
                <li>BSD</li>
                <li>Linux</li>
                <li>PlayStation操作系统</li>
                <li>Roku OS</li>
                <li>Symbian</li>
                <li>Tizen</li>
                <li>Windows</li>
                <li>iOS</li>
                <li>tvOS</li>
                <li>macOS</li>
                <li>webOS</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>操作系统供应商</td>
        <td>设备的操作系统(OS)供应商。</td>
        <td>&amp;amp；检查；</td>
        <td>
            值受限制：
            <ul>
                <li>Amazon</li>
                <li>Apple</li>
                <li>Google</li>
                <li>LG</li>
                <li>Microsoft</li>
                <li>Mozilla</li>
                <li>任天堂</li>
                <li>Nokia</li>
                <li>Roku</li>
                <li>Samsung</li>
                <li>Sony</li>
                <li>Tizen项目</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td><i>必填</i></td>
        <td>osVersion</td>
        <td>设备的操作系统(OS)版本。</td>
        <td></td>
        <td>例如10.2、9.0.1等。</td>
    </tr>
    <tr>
        <td></td>
        <td>browserName</td>
        <td>浏览器的名称。</td>
        <td>&amp;amp；检查；</td>
        <td>
            值受限制：
            <ul>
                <li>Android Browser</li>
                <li>Chrome</li>
                <li>Edge</li>
                <li>Firefox</li>
                <li>Internet Explorer</li>
                <li>Opera</li>
                <li>Safari</li>
                <li>SeaMonke</li>
                <li>Symbian Browser</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>browserVendor</td>
        <td>浏览器的构建公司/组织。</td>
        <td>&amp;amp；检查；</td>
        <td>
            值受限制：
            <ul>
                <li>Amazon</li>
                <li>Apple</li>
                <li>Google</li>
                <li>Microsoft</li>
                <li>摩托罗拉</li>
                <li>Mozilla</li>
                <li>Netscape</li>
                <li>任天堂</li>
                <li>Nokia</li>
                <li>Samsung</li>
                <li>索尼·爱立信</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>browserVersion</td>
        <td>设备的浏览器版本。</td>
        <td></td>
        <td>例如60.0.3112</td>
    </tr>
    <tr>
        <td></td>
        <td>userAgent</td>
        <td>设备的用户代理。</td>
        <td></td>
        <td>例如，Mozilla/5.0(Macintosh；英特尔Mac OS X 10_12_3) AppleWebKit/602.4.8（KHTML，如Gecko）版本/10.0.3 Safari/602.4.8</td>
    </tr>
    <tr>
        <td></td>
        <td>显示宽度</td>
        <td>设备的物理屏幕宽度。</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td></td>
        <td>displayheight</td>
        <td>设备的物理屏幕高度。</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td></td>
        <td>displayPpi</td>
        <td>设备的物理屏幕像素密度。</td>
        <td></td>
        <td>例如294</td>
    </tr>
    <tr>
        <td></td>
        <td>对角屏幕大小</td>
        <td>设备的物理屏幕对角尺寸（英寸）。</td>
        <td></td>
        <td>例如5.5、10.1</td>
    </tr>
    <tr>
        <td></td>
        <td>connectionIp</td>
        <td>用于发送HTTP请求的设备的IP。</td>
        <td></td>
        <td>例如8.8.4.4</td>
    </tr>
    <tr>
        <td></td>
        <td>connectionPort</td>
        <td>用于发送HTTP请求的设备的端口。</td>
        <td></td>
        <td>例如53124</td>
    </tr>
    <tr>
        <td><i>必填</i></td>
        <td>connectionType</td>
        <td>网络连接类型。</td>
        <td></td>
        <td>例如WiFi、LAN、3G、4G、5G</td>
    </tr>
    <tr>
        <td></td>
        <td>connectionSecure</td>
        <td>网络连接安全状态。</td>
        <td>&amp;amp；检查；</td>
        <td>
            值受限制：
            <ul>
                <li>true — 在安全网络的情况下</li>
                <li>false — 在公共热点的情况下</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>applicationId</td>
        <td>应用程序的唯一标识符。</td>
        <td></td>
        <td>例如REF30</td>
    </tr>
</table>


## 示例 {#examples}

```JSON
// Device information
// {
//  "primaryHardwareType" : "MobilePhone",
//  "model":"SM-S901U",
//  "vendor":"samsung",
//  "version":"r0q",
//  "manufacturer":"samsung",
//  "osName":"Android",
//  "osVersion":"14"
// }
 
// BASE64-encoded
// ewogICJwcmltYXJ5SGFyZHdhcmVUeXBlIiA6ICJNb2JpbGVQaG9uZSIsCiAgIm1vZGVsIjoiU00tUzkwMVUiLAogICJ2ZW5kb3I
// iOiJzYW1zdW5nIiwKICAidmVyc2lvbiI6InIwcSIsCiAgIm1hbnVmYWN0dXJlciI6InNhbXN1bmciLAogICJvc05hbWUiOiJBbmRyb
// 2lkIiwKICAib3NWZXJzaW9uIjoiMTQiCn0=
 
X-Device-Info: ewogICJwcmltYXJ5SGFyZHdhcmVUeXBlIiA6ICJNb2JpbGVQaG9uZSIsCiAgIm1vZGVsIjoiU00tUzkwMVUiLAogICJ2ZW5kb3IiOiJzYW1zdW5nIiwKICAidmVyc2lvbiI6InIwcSIsCiAgIm1hbnVmYWN0dXJlciI6InNhbXN1bmciLAogICJvc05hbWUiOiJBbmRyb2lkIiwKICAib3NWZXJzaW9uIjoiMTQiCn0=
```

## 指南 {#cookbooks}

>[!IMPORTANT]
> 
> 提供了代码段和文档资源以供参考。
> 
> 代码片段并非详尽无遗，可能需要进行额外的修改才能在您的项目中正常工作。
>
> 无论实际实施如何，`X-Device-Info`标头都必须包含一个值，其格式如[指令](#directives)部分中所述。

### 浏览器 {#browsers}

对于在浏览器中运行的客户端应用程序，可以省略`X-Device-Info`标头，因为浏览器将自动在`User-Agent`标头中发送所需的最小信息集。

您仍然可以使用`X-Device-Info`标头提供有关设备、连接和应用程序的附加信息，以防您的客户端应用程序集成了提供设备识别机制的库或服务。

### 移动设备 {#mobile-devices}

#### iOS和iPadOS {#ios-ipados}

要为运行[iOS或iPadOS](https://developer.apple.com/documentation/ios-ipados-release-notes)的设备构建`X-Device-Info`标头，您可以参考以下文档及以下代码片段：

* [UIDevice](https://developer.apple.com/documentation/uikit/uidevice#//apple_ref/occ/cl/UIDevice)的Apple开发人员文档。
* 有关[可访问性](https://developer.apple.com/library/archive/samplecode/Reachability/Introduction/Intro.html)的Apple开发人员文档。
* Linux手动文档[uname](https://man7.org/linux/man-pages/man2/uname.2.html)。

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
                @"applicationId": @"REF30" 
        } mutableCopy];

        NSError *error;
        NSData *jsonData = [NSJSONSerialization dataWithJSONObject:clientInformation options:NSJSONWritingPrettyPrinted error:&error];
        NSString *base64Encoded = [jsonData base64EncodedStringWithOptions:0];

        return base64Encoded;
}
```

设备信息可通过以下方式构建：

| 键 | Source | 值（示例） |
|---------------|------------------------|-----------------|
| 模型 | uname.machine | iPhone |
| 供应商 | 硬编码 | Apple |
| 制造商 | 硬编码 | Apple |
| 版本 | uname.machine | 8,1 |
| 显示宽度 | UIScreen.mainScreen | 320 |
| displayheight | UIScreen.mainScreen | 568 |
| 操作系统名称 | UIDevice.systemName | iOS |
| osVersion | UIDevice.systemVersion | 10.2 |

连接信息可通过以下方式构建：

| 键 | Source | 值（示例） |
|------------------|------------------------------------------|-----------------|
| connectionType | [可达性currentReachabilityStatus] |                 |
| connectionSecure |                                          |                 |


应用程序信息可通过以下方式构建：

| 键 | Source | 值（示例） |
|---------------|-----------|-----------------|
| applicationId | 硬编码 | REF30 |

#### Android {#android}

要为运行[Android](https://developer.android.com/about/versions)的设备生成`X-Device-Info`标头，您可以参考以下文档以及下面的代码片段：

* [Build](https://developer.android.com/reference/android/os/Build.html)类的Android开发人员文档。

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
          clientInformation.put("model", Build.MODEL);
          clientInformation.put("vendor", Build.BRAND);
          clientInformation.put("manufacturer", Build.MANUFACTURER);
          clientInformation.put("version", Build.DEVICE);
          clientInformation.put("osName", "Android");
          clientInformation.put("osVersion", Build.VERSION.RELEASE);
          clientInformation.put("connectionType", connectionType);
          clientInformation.put("applicationId", "REF30");
     } catch (JSONException e) {
          Log.e(LOGGING_TAG, e.getMessage());
     }

     return Base64.encodeToString(clientInformation.toString().getBytes(), Base64.NO_WRAP);
}
```

设备信息可通过以下方式构建：

| 键 | Source | 值（示例） |
|---------------|-----------------------------|-----------------|
| 模型 | Build.MODEL | GT-I9505 |
| 供应商 | Build.BRAND | 三星 |
| 制造商 | Build.MANUFACTURER | 三星 |
| 版本 | Build.DEVICE | jflte |
| 显示宽度 | DisplayMetrics.widthPixels | 600 |
| displayheight | DisplayMetrics.heightPixels | 800 |
| 操作系统名称 | 硬编码 | Android |
| osVersion | Build.VERSION.RELEASE | 5.0.1 |

连接信息可通过以下方式构建：

| 键 | Source | 值（示例） |
|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------|
| connectionType | `<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>` `getSystemService(Context.CONNECTIVITY_SERVICE).getActiveNetworkInfo().getType()` | `"WIFI","BLUETOOTH","MOBILE","ETHERNET","VPN","DUMMY","MOBILE_DUN","WIMAX","notAccessible"` |
| connectionSecure |                                                                                                                                                               |                                                                                             |

应用程序信息可通过以下方式构建：

| 键 | Source | 值（示例） |
|---------------|-----------|-----------------|
| applicationId | 硬编码 | REF30 |

### TV连接的设备 {#tv-connected-devices}

#### tvOS {#tvos}

要为运行[tvOS](https://developer.apple.com/documentation/tvos-release-notes)的设备生成`X-Device-Info`标头，您可以参考以下文档及以下代码片段：

* [UIDevice](https://developer.apple.com/documentation/uikit/uidevice#//apple_ref/occ/cl/UIDevice)的Apple开发人员文档。
* 有关[可访问性](https://developer.apple.com/library/archive/samplecode/Reachability/Introduction/Intro.html)的Apple开发人员文档。
* Linux手动文档[uname](https://man7.org/linux/man-pages/man2/uname.2.html)。

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
                @"applicationId": @"REF30" 
        } mutableCopy];

        NSError *error;
        NSData *jsonData = [NSJSONSerialization dataWithJSONObject:clientInformation options:NSJSONWritingPrettyPrinted error:&error];
        NSString *base64Encoded = [jsonData base64EncodedStringWithOptions:0];

        return base64Encoded;
}
```

设备信息可通过以下方式构建：

| 键 | Source | 值（示例） |
|---------------|------------------------|-----------------|
| 模型 | uname.machine | AppleTV |
| 供应商 | 硬编码 | Apple |
| 制造商 | 硬编码 | Apple |
| 版本 | uname.machine | 8,1 |
| 显示宽度 | UIScreen.mainScreen | 1920 |
| displayheight | UIScreen.mainScreen | 1080 |
| 操作系统名称 | UIDevice.systemName | tvOS |
| osVersion | UIDevice.systemVersion | 10.2 |

连接信息可通过以下方式构建：

| 键 | Source | 值（示例） |
|------------------|------------------------------------------|-----------------|
| connectionType | [可达性currentReachabilityStatus] |                 |
| connectionSecure |                                          |                 |

应用程序信息可通过以下方式构建：

| 键 | Source | 值（示例） |
|---------------|-----------|-----------------|
| applicationId | 硬编码 | REF30 |

#### Fire操作系统 {#fireos}

要为运行[Fire OS](https://developer.amazon.com/docs/fire-tv/fire-os-overview.html)的设备生成`X-Device-Info`标头，您可以参考以下文档：

* [Build](https://developer.android.com/reference/android/os/Build.html)类的Android开发人员文档。
* [识别Fire TV设备](https://developer.amazon.com/docs/fire-tv/identify-amazon-fire-tv-devices.html)的Amazon开发人员文档。

设备信息可通过以下方式构建：

| 键 | Source | 值（示例） |
|---------------|-----------------------------|-----------------|
| 模型 | Build.MODEL | AFTM |
| 供应商 | Build.BRAND | Amazon |
| 制造商 | Build.MANUFACTURER | Amazon |
| 版本 | Build.DEVICE | 蒙托亚 |
| 显示宽度 | DisplayMetrics.widthPixels |                 |
| displayheight | DisplayMetrics.heightPixels |                 |
| 操作系统名称 | 硬编码 | Android |
| osVersion | Build.VERSION.RELEASE | 5.1.1 |

连接信息可通过以下方式构建：

| 键 | Source | 值（示例） |
|------------------|--------|-----------------|
| connectionType |        |                 |
| connectionSecure |        |                 |

应用程序信息可通过以下方式构建：

| 键 | Source | 值（示例） |
|---------------|-----------|-----------------|
| applicationId | 硬编码 | REF30 |

#### Roku OS {#rokuos}

要为运行[Roku OS](https://developer.roku.com/docs/developer-program/release-notes/roku-os-release-notes.md)的设备生成`X-Device-Info`标头，您可以参考以下文档：

* [ifDeviceInfo](https://developer.roku.com/docs/references/brightscript/interfaces/ifdeviceinfo.md)的Roku开发人员文档。

设备信息可通过以下方式构建：

| 键 | Source | 值（示例） |
|---------------|--------------------------------------------|-----------------|
| 模型 | 硬编码 | “Roku” |
| 供应商 | ifDeviceInfo.GetModelDetails().VendorName | “Sharp”、“Roku” |
| 制造商 | ifDeviceInfo.GetModelDetails().VendorName | “Sharp”、“Roku” |
| 版本 | ifDeviceInfo.GetModelDetails().ModelNumber | “5303X” |
| 显示宽度 | ifDeviceInfo.GetDisplaySize().w | 1920 |
| displayheight | ifDeviceInfo.GetDisplaySize().h | 1080 |
| 操作系统名称 | 硬编码 | “Roku” |
| osVersion | ifDeviceInfo.getVersion() |                 |

连接信息可通过以下方式构建：

| 键 | Source | 值（示例） |
|-------------------|------------------------------------|---------------------------------------|
| connectionType | ifDeviceInfo.GetConnectionType() | “WifiConnection”、“WiredConnection” |
| connectionSecure | 硬编码 | 如果连接是有线的，则为true |

应用程序信息可通过以下方式构建：

| 键 | Source | 值（示例） |
|---------------|-----------|-----------------|
| applicationId | 硬编码 | REF30 |

### 其他  {#others}

对于文档中未涵盖的设备平台，客户端信息（设备、连接和应用程序）应链接到任何可用的硬件和操作系统(OS)属性，通常在设备的硬件和操作系统手册中指定。
