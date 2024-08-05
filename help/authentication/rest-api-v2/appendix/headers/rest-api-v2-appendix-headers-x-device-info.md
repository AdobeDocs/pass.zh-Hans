---
title: 标头 — X-Device-Info
description: REST API V2 — 标头 — X-Device-Info
source-git-commit: 150e064d0287eaac446c694fb5a2633f7ea4b797
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 0%

---


# 标头 — X-Device-Info {#header-x-device-info}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## 概述 {#overview}

<b>X-Device-Info</b>请求标头包含与实际流设备相关的客户端信息（设备、连接和应用程序）。

## 语法 {#syntax}

<table>
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

<table>
    <tr>
        <th style="background-color: #EFF2F7; width: 15%;">键</th>
        <th style="background-color: #EFF2F7;">描述</th>    
        <th style="background-color: #EFF2F7; width: 15%;">存在</th>
        <th style="background-color: #EFF2F7;">可能值</th>
    </tr>
    <tr>
        <td>主要硬件类型</td>
        <td>设备的主要硬件类型。</td>
        <td></td>
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
        <td>模型</td>
        <td>设备的型号名称。</td>
        <td><i>必填</i></td>
        <td>例如iPhone、SM-G930V、AppleTV等。</td>
    </tr>
    <tr>
        <td>版本</td>
        <td>设备的版本。</td>
        <td></td>
        <td>例如2.0.1等。</td>
    </tr>
    <tr>
        <td>制造商</td>
        <td>设备的制造公司/组织。</td>
        <td></td>
        <td>例如，三星、LG、ZTE、华为、摩托罗拉、Apple等。</td>
    </tr>
    <tr>
        <td>供应商</td>
        <td>设备的销售公司/组织。</td>
        <td></td>
        <td>例如Apple、Samsung、LG、Google等。</td>
    </tr>
    <tr>
        <td>操作系统名称</td>
        <td>设备的操作系统(OS)名称。</td>
        <td><i>必填</i></td>
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
        <td>osFamily</td>
        <td>设备的操作系统(OS)组名称。</td>
        <td></td>
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
        <td>操作系统供应商</td>
        <td>设备的操作系统(OS)供应商。</td>
        <td></td>
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
        <td>osVersion</td>
        <td>设备的操作系统(OS)版本。</td>
        <td></td>
        <td>例如10.2、9.0.1等。</td>
    </tr>
    <tr>
        <td>browserName</td>
        <td>浏览器的名称。</td>
        <td></td>
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
        <td>browserVendor</td>
        <td>浏览器的构建公司/组织。</td>
        <td></td>
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
        <td>browserVersion</td>
        <td>设备的浏览器版本。</td>
        <td></td>
        <td>例如60.0.3112</td>
    </tr>
    <tr>
        <td>userAgent</td>
        <td>设备的用户代理。</td>
        <td></td>
        <td>例如，Mozilla/5.0(Macintosh；英特尔Mac OS X 10_12_3) AppleWebKit/602.4.8（KHTML，如Gecko）版本/10.0.3 Safari/602.4.8</td>
    </tr>
    <tr>
        <td>显示宽度</td>
        <td>设备的物理屏幕宽度。</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td>displayheight</td>
        <td>设备的物理屏幕高度。</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td>displayPpi</td>
        <td>设备的物理屏幕像素密度。</td>
        <td></td>
        <td>例如294</td>
    </tr>
    <tr>
        <td>对角屏幕大小</td>
        <td>设备的物理屏幕对角尺寸（英寸）。</td>
        <td></td>
        <td>例如5.5、10.1</td>
    </tr>
    <tr>
        <td>connectionIp</td>
        <td>用于发送HTTP请求的设备的IP。</td>
        <td></td>
        <td>例如8.8.4.4</td>
    </tr>
    <tr>
        <td>connectionPort</td>
        <td>用于发送HTTP请求的设备的端口。</td>
        <td></td>
        <td>例如53124</td>
    </tr>
    <tr>
        <td>connectionType</td>
        <td>网络连接类型。</td>
        <td></td>
        <td>例如WiFi、LAN、3G、4G、5G</td>
    </tr>
    <tr>
        <td>connectionSecure</td>
        <td>网络连接安全状态。</td>
        <td></td>
        <td>
            值受限制：
            <ul>
                <li>true — 在安全网络的情况下</li>
                <li>false — 在公共热点的情况下</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>applicationId</td>
        <td>应用程序的唯一标识符。</td>
        <td></td>
        <td>例如CNN</td>
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
