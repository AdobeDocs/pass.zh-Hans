---
title: 如何将MVPD登录页面从iFrame迁移到弹出窗口
description: 如何将MVPD登录页面从iFrame迁移到弹出窗口
exl-id: 389ea0ea-4e18-4c2e-a527-c84bffd808b4
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '708'
ht-degree: 0%

---

# （旧版）如何将MVPD登录页面从iFrame迁移到弹出窗口 {#migr-mvpd-login-iframe-popup}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

>[!IMPORTANT]
>
> 确保随时了解汇总在[产品公告](/help/authentication/product-announcements.md)页中的最新Adobe Pass身份验证产品公告和停用时间表。

## 弹出窗口与iFrame {#popup-vs-iframe}

某些用户在MVPD登录页面的iFrame实施中遇到第三方Cookie问题。
<!--These issues are described in the tech notes linked below:

* [Adobe Pass Authentication and Safari login issues](https://tve.helpdocsonline.com/adobe-pass)
* [MVPD iFrame login and 3rd party cookies](https://tve.helpdocsonline.com/mvpd)-->

Adobe Pass身份验证团队&#x200B;**建议在Firefox和Safari上实施弹出窗口/新窗口登录页面**，而不是iFrame版本。  但是，如果您实施的是Internet Explorer的登录页面，则可能会遇到弹出窗口实施问题。 导致出现IE问题的原因在于，用户在弹出窗口中使用MVPD进行身份验证后，Adobe Pass身份验证将强制实施父页面重定向，而Internet Explorer将重定向视为弹出窗口阻止程序。 Adobe Pass身份验证团队&#x200B;**建议为Internet Explorer**&#x200B;实现iFrame登录。

本技术说明中提供的示例代码使用iFrame和弹出窗口的混合实现 — 在Internet Explorer上打开iFrame，在其他浏览器上打开弹出窗口。

考虑到iFrame实施已存在，技术说明的第一部分将介绍iFrame实施的代码，第二部分将介绍所做的更改以适应将弹出窗口实施作为默认设置的情况。


## 在iFrame中具有登录页面的MVPD选取器 {#mvpd-pickr-iframe}

以前的代码示例显示了一个HTML页面，该页面包含要在其中创建iFrame的&lt;div>标记以及“关闭iFrame”按钮：

```HTML
<body> 
    <div id="mvpddiv">
        <div style="background: red">
            <input type="button" id="btnCloseIframe" value="X" onclick="closeIframeAction();">
        </div>
        <br/>
        <!-- We use the "about:blank" value so that when the iFrame loads
             we do not see the the parent page. -->
        <iframe id="mvpdframe" name="mvpdframe" src="about:blank"></iframe>
    </div> 
</body>
```

以下是关联的&#x200B;**JavaScript**&#x200B;代码：

```JavaScript
/*
 * Callback indicating that the AccessEnabler swf has initialized
 */
function swfLoaded() {
    // AccessEnabler is loaded so we can use the API function it provides
    accessEnablerObject.setRequestor(requestorID); 
    accessEnablerObject.checkAuthentication();
} 
/*
* The code the correctly closes the opened IFrame and does not leave the
* AccessEnabler in an undefined state.
*/
function closeIframeAction () {
    accessEnablerObject.setSelectedProvider(null);
    /* We use the "about:blank" value so that when the iFrame loads
     * we do not see the the previous MVPD.
     */
    document.getElementById('mvpdframe').src="about:blank";
    document.getElementById('mvpddiv').style.visibility="hidden";
    document.getElementById('mvpddiv').style.display="none";
}
/*
* Some of the supported MVPDs require their login pages to be displayed within an iFrame.
* In your HTML page, you must include the following <div> tag: "<div id="mvpddiv"></div>".
* Called when the selected MVPD requires an iFrame in which to display its authentication UI.
*/
function createIFrame(inWidth, inHeight) {
     // Create the iFrame to the specified width and height for the auth page
    ifrm = document.createElement("IFRAME");
    ifrm.name = "mvpdframe";
    ...
    document.getElementById('mvpddiv').appendChild(ifrm);
    ...
    // Force the name into the DOM since it is still not refreshed, for IE
    window.frames["mvpdframe"].name = "mvpdframe";
}
/*
 * The custom non-Flash MVPD selector dialog will be implemented in this function.
 */
function displayProviderDialog(providers) {
    /* This is an example how previously the MVPD picker was build and will be replaced. */
}
/* 
* Sending the user selected MVPD of the custom dialog is achieved
* by calling the API function setSelectedProvider(providerID:String).
*/
function setSelectedProvider(providerID) {
    accessEnablerObject.setSelectedProvider(providerID);
}
```


## 在弹出窗口中登录页面的MVPD选取器 {#mvpd-pickr-popup}

由于我们将不再使用&#x200B;**iFrame**，因此HTML代码将不再包含iFrame或用于关闭iFrame的按钮。 以前包含iFrame的div - **mvpddiv** — 将被保留并用于以下内容：

* 在弹出窗口焦点丢失时通知用户MVPD登录页面已打开
* 提供链接以重新获得对弹出窗口的关注

```HTML
<body onload="javascript:loadAccessEnabler();"> 
   <div id="aeHolder">
       <div name="contentAccessEnabler" id="contentAccessEnabler"></div>
   </div>
   <div id="mvpddiv">
      <p>
      <strong>No login window?</strong>
      <br/>
      <a href="javascript:mvpdWindow.focus();">Click here to open it.</a>
      <br/>
      Still not working? Check popup blockers!
      </p>
   </div> 
 
   <div id="picker" style="display:none">
      Choose MVPD: <br/>
      <select id="mvpdList" multiple></select>
      <br/>
         <a id="authenticateButton" onclick="javascript:authenticate();">Authenticate</a>
   </div>
</body>
```

MVPD列表将作为选择&#x200B;**-mvpdList**&#x200B;显示在名为&#x200B;**选取器**&#x200B;的div中。

将使用新的API回调 — **setConfig(configXML)**。 调用setRequestor(requestorID)函数后会触发回调。 此回调将返回与先前设置的requestorID集成的MVPD列表。 在回调方法中，将解析传入的XML，并缓存MVPD列表。 MVPD选取器也会创建，但不显示。

```JavaScript
var mvpdList;  // The list of cached MVPDs
var mvpdWindow;  // The reference to the popup with the MVPD login page
var cancelTimer = 0;
/* Callback indicating that the AccessEnabler swf has initialized */
function swfLoaded() {
   accessEnablerObject = $('#AccessEnabler').get(0);
   // Using a custom implementation of the MVPD picker
   accessEnablerObject.setProviderDialogURL('none');
   accessEnablerObject.setRequestor(requestorID); 
}

function setConfig(configXML) {
   mvpdList = [];
   $.each($($.parseXML(configXML)).find('mvpd'), function (idx, item) {
      var mvpdId = $(item).find('id').text();
      mvpdList[mvpdId] = {
         displayName: $(item).find('displayName').text(),
         logo: $(item).find('logoUrl').text(),
         popup: $(item).find('iFrameRequired').text() == "true",
         width: $(item).find('iFrameWidth').text(),
         height: $(item).find('iFrameHeight').text()
      };

      $('#mvpdList').append($('<option value="' + mvpdId + '" title="' + mvpdId + '">' + mvpdList[mvpdId].displayName + '</option>'));
   });
   accessEnablerObject.getAuthentication();
}
```

调用getAuthentication()或getAuthorization()函数后，将触发displayProviderDialog()回调。 通常，在此回调中，将构建并显示MVPD列表。 由于MVPD选取器已构建，因此只需向用户显示即可。

```JavaScript
/*
 * The custom non-Flash MVPD selector dialog will be implemented in this function.
 */
function displayProviderDialog(providers) {
   $('#picker').show();
}
```

用户从选取器中选择MVPD后，需要创建弹出窗口。 如果弹出窗口是使用大约:blank或另一个域上的页面创建的，则某些浏览器可能会阻止弹出窗口，因此建议使用加载AccessEnabler的主机名打开弹出窗口。

在iFrame实施中，重置身份验证流由btnCloseIframe按钮和JavaScript函数closeIframeAction()完成，但现在无法再修饰iFrame。 因此，通过观察何时关闭弹出窗口（用户关闭或完成验证流程）可以获得相同的行为。 已添加一个代码片段，在用户失去弹出窗口的焦点时也会有帮助：

```HTML
"<a href="javascript:mvpdWindow.focus();">Click here to open it.</a>".
```

在createIFrame()回调中，将显示&#x200B;**mvpddiv** div。

```JavaScript
function createIFrame(width, height) {
   $('#mvpddiv').show();
   if (useIframeLoginStyle) {
      ...
   }
}

/* Function called when user has selected the MVPD from the picker */
function authenticate() {
   var selectedMvpd = $('#mvpdList').val()[0];
   if (mvpdList[selectedMvpd].popup) {
      mvpdWindow = window.open("http://entitlement.auth-staging.adobe.com", "mvpdframe", "width=" + mvpdList[selectedMvpd].width + ",height=" + mvpdList[selectedMvpd].height, true);
      if (!mvpdWindow) {
         // Message to user that popup blocker is not allowing the authentication process to start
      }
      //watch the mvpd popup for close
      clearInterval(cancelTimer);
      cancelTimer = setInterval(checkClosed, 200);
   }
   accessEnablerObject.setSelectedProvider(selectedMvpd);
}

function checkClosed() {
   try {
      if (mvpdWindow && mvpdWindow["closed"]) {
         clearInterval(cancelTimer);
         accessEnablerObject.setSelectedProvider(null);
         accessEnablerObject.getAuthentication();
         $('#mvpddiv').hide();
      }
   } catch (error) {
      console.log(error);
   }
}
```

>[!IMPORTANT]
>
>* 该示例代码包含一个用于所用requestorID - &#39;REF&#39;的硬编码变量，该变量应当被替换为真正的程序员请求者ID。
>* 此示例代码只能从与所用请求者ID关联的已列入白名单的域中正确运行。
>* 由于整个代码均可供下载，因此本技术说明中介绍的代码已被截断。 有关完整示例，请参阅&#x200B;**JS iFrame与弹出示例**。
>* 外部JavaScript库链接自[Google托管服务](https://developers.google.com/speed/libraries/)。
