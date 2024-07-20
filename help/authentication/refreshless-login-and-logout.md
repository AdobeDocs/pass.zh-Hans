---
title: 无刷新登录和注销
description: 无刷新登录和注销
exl-id: 3ce8dfec-279a-4d10-93b4-1fbb18276543
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1761'
ht-degree: 0%

---

# 无刷新登录和注销 {#tefresh-less-login-and-logout}

>[!NOTE]
>
>此页面上的内容仅供参考。 使用此API需要来自Adobe的当前许可证。 不允许未经授权使用。

## 概述 {#overview}

对于Web应用程序，您必须考虑身份验证和注销用户时可能出现的一些不同情况。  MVPD要求用户登录MVPD的网页进行身份验证，并且会影响以下附加因素：

- 某些MVPD要求从您的站点完全重定向到其登录页面
- 某些MVPD要求您在网站上打开iFrame以显示MVPD的登录页面
- 某些浏览器无法很好地处理iFrame场景，因此对于这些浏览器，更好的替代方法是使用弹出窗口而不是iFrame

在Adobe Pass Authentication 2.7之前，所有这些用于验证用户的场景都涉及程序员页面的完整页面刷新。对于2.7和后续版本，Adobe Pass身份验证团队改进了这些流程，以便用户不必在登录和注销期间在应用程序上经历页面刷新。


## 详细说明 {#detailed_description}

让我们从最初的身份验证和注销流程概要开始，然后按照经过改进的身份验证和注销流程进行总结。 请注意，前四个部分介绍了普通的MVPD（非TempPass），而最后一部分则描述了需要应用于TempPass的特殊实施：

- [原始身份验证流程](#orig_authn)
- [原始注销流程](#orig_logout)
- [身份验证流程得到改进](#improved_authn)
- [改进的注销流程](#improved_logout)
- [TempPass流](#improved_temppass)

</br>

## 原始身份验证/注销流程 {#orig_authn}

**身份验证**

Adobe Pass身份验证Web客户端有两种身份验证方式，具体取决于MVPD的要求：

1. **全页重定向 —**&#x200B;用户选择提供程序后    （使用全页重定向进行配置）    在AccessEnabler上调用程序员网站`setSelectedProvider(<mvpd>)`，用户将被重定向到MVPD的登录页面。 用户提供有效凭据后，他将被重定向回程序员网站。 AccessEnabler已初始化，并在`setRequestor`期间从Adobe Pass身份验证中检索身份验证令牌。
1. **iFrame/弹出窗口 —**&#x200B;用户选择提供程序（配置了iFrame）后，将在AccessEnabler上调用`setSelectedProvider(<mvpd>)`。 此操作将触发`createIFrame(width, height)`回调，通知程序员使用名称`"mvpdframe"`和提供的维度创建一个iFrame（或弹出窗口，具体取决于浏览器/首选项）。 创建iFrame/弹出窗口后，AccessEnabler会在iFrame/弹出窗口中加载MVPD的登录页面。 用户提供有效凭据，iFrame/弹出窗口被重定向到Adobe Pass身份验证，该身份验证返回一个JS代码片段，该代码片段关闭iFrame/弹出窗口并重新加载父页面（程序员网站）。 与流程1类似，在`setRequestor`期间检索身份验证令牌。

`displayProviderDialog`回调（由`getAuthentication`/`getAuthorization`触发）返回MVPD及其相应设置的列表。 MVPD的`iFrameRequired`属性允许程序员知道它应该激活流1还是流2。 请注意，程序员仅需要对流程2执行额外操作（创建iFrame/弹出窗口）。

**取消身份验证**

还有一种情况，用户通过关闭登录页面来显式取消身份验证流程。 以下是面向程序员的场景和建议的解决方案：

1. **全页重定向 —**&#x200B;当登录页面关闭时，用户需要再次导航到程序员网站，并从头开始启动整个流程。 在此方案中，程序员无需执行任何显式操作。
1. **iFrame -**&#x200B;建议程序员将iFrame托管在附有“关闭”按钮的`div`（或类似的UI组件）中。 当用户按下“关闭”按钮时，程序员将销毁iFrame以及关联的UI并执行`setSelectedProvider(null)`。 此调用允许AccessEnabler清除其内部状态，并允许用户启动后续身份验证流程。 将触发`setAuthenticationStatus`和`sendTrackingData(AUTHENTICATION_DETECTION...)`以指示失败的身份验证流程（在`getAuthentication`和`getAuthorization`上）。
1. **弹出窗口 —**&#x200B;某些浏览器无法准确检测窗口关闭事件，因此需要在此处采取其他方法（与上述iFrame流程相反）。 Adobe建议程序员初始化一个定时器，定期验证登录弹出窗口是否存在。 如果窗口不存在，程序员可以确保用户手动取消登录流程，并且程序员可以继续调用`setSelectedProvider(null)`。 触发的回调与上面流程2中的相同。

</br>

## 原始注销流程 {#orig_logout}

AccessEnabler的注销API会清除库的本地状态，并在当前选项卡/窗口中加载MVPD的注销URL。 浏览器导航到MVPD的注销端点，进程完成后，用户将被重定向回程序员网站。 代表用户所需的唯一操作是按注销按钮/链接并启动流；无需用户在MVPD的注销端点上进行交互。

**页面刷新时的原始身份验证/注销流程**

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/AE_with_refresh_web.png)

</br>

## 改进了（无刷新）身份验证 {#improved_authn}

>[!NOTE]
>
>改进的无刷新登录和注销流程要求浏览器支持现代HTML5技术，包括Web消息传送。

上面讨论的身份验证（登录）和注销流都通过在每个流完成后重新加载主页提供了类似的用户体验。  当前功能旨在通过提供无刷新（后台）登录和注销来改善用户体验。 通过向`setRequestor` API的`configInfo`参数传递两个布尔标记（`backgroundLogin`和`backgroundLogout`），程序员可以启用/禁用后台登录和注销。 默认情况下，后台登录/注销处于禁用状态（这提供了与以前实施的兼容性）。

**示例：**

```JSON
    var configInfo = {
        callSetConfig: true,
        backgroundLogin: true,
        backgroundLogout: true
    };
    accessEnabler.setRequestor(REQUESTOR_ID, null, configInfo);
```

**身份验证**

以下几点描述了原始验证流和改进流之间的过渡：

1. 全页重定向将被用于执行MVPD登录的新浏览器选项卡替换。 当用户选择MVPD （带有`iFrameRequired = false`）时，程序员需要创建名为`mvpdwindow`的新选项卡（通过`window.open`）。 然后程序员执行`setSelectedProvider(<mvpd>)`，允许AccessEnabler在新选项卡中加载MVPD登录URL。 在用户提供有效凭据后，Adobe Pass身份验证将关闭选项卡，并向程序员的网站发送window.postMessage，以向AccessEnabler发出身份验证流程已完成的信号。 将触发以下回调：

   - 如果流程由`getAuthentication`启动： `setAuthenticationStatus`和`sendTrackingData(AUTHENTICATION_DETECTION...)`将触发以表示身份验证成功/不成功。

   - 如果流程由`getAuthorization`启动： `setToken/tokenRequestFailed`和`sendTrackingData(AUTHORIZATION_DETECTION...)`将触发以表示授权成功/不成功。

1. iFrame/弹出窗口流程大致保持不变，区别在于用户提供有效凭据后，不会重新加载父页面。 iFrame/弹出窗口将在登录后自动关闭，并向父页面发送`window.postMessage`，通知AccessEnabler流已完成。 触发的回调与上一流程&#x200B;**中的相同，再加上以下新回调：`destroyIFrame`**。 `destroyIFrame`回调允许程序员删除任何关联的iFrame/辅助组件，如UI装饰。 旧身份验证流程中不需要存在此回调，因为登录完成后，Adobe Pass身份验证将重新加载程序员页面，从而破坏其上的所有UI组件。

</br>

>[!IMPORTANT]
> 
>必须将MVPD登录iFrame或弹出窗口作为包含AccessEnabler实例的页面的直接子级加载。 如果MVPD登录iFrame或弹出窗口在包含AccessEnabler实例的页面下嵌套了两个或多个级别，则流可能会挂起。 例如，如果在主页面和MVPD iFrame(Page =\> iFrame =\> MVPD iFrame)之间有一个iFrame，则登录流可能会失败。

</br>

**取消身份验证**

以下是取消身份验证的流程：

1. **浏览器选项卡 —**&#x200B;由于该选项卡基本上是一个新窗口，因此捕获其关闭事件与方案3中所述的旧身份验证流的限制相同。 此外，计时器方法在此处不可用，因为无法区分用户手动关闭的选项卡和在登录流结束时自动关闭的选项卡。 此处的解决方案是让AccessEnabler在用户取消流时保持“静默”（不触发回调）。 此外，程序员无需采取任何具体操作。 用户将能够启动另一个身份验证流程，而不会收到“多个身份验证请求错误”错误（此错误已在后台登录的AccessEnabler中禁用）。

1. **iFrame -**&#x200B;程序员可以使用旧身份验证流程中的方案2中讨论的方法(从iFrame创建包装器UI以及触发`setSelectedProvider(null)`的相关“关闭”按钮。 尽管这种方法已不再强烈要求（后台登录允许使用多个身份验证流程，如上面的场景1中所述），但Adobe仍建议这样做。

1. **弹出窗口 —**&#x200B;这与上面的“浏览器”选项卡流程相同。

</br>

## 改进的注销流程 {#improved_logout}

新的注销流将在隐藏的iFrame中执行，从而消除完整页面重定向。  这是可能的，因为用户不需要在MVPD的注销页面上执行特定操作。

注销流程完成后，它会将iFrame重定向到自定义Adobe Pass身份验证端点。 这将向父项提供执行`window.postMessage`的JS代码片段，通知AccessEnabler注销已完成。 触发了以下回调： `setAuthenticationStatus()`和`sendTrackingData(AUTHENTICATION_DETECTION ...)`，指示用户不再进行身份验证。

下图显示了无刷新流程，通过该流程，用户无需刷新应用程序的主页即可登录到其MVPD：

**已改进（无刷新）的身份验证/注销流程**

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/AE_with_no_refresh_web.png)

</br>

## TempPass流 {#improved_temppas}

无刷新登录对TempPass类型的MVPD采用不同的方法。

由于TempPass流要求自动创建并关闭窗口，而没有明确的用户交互，因此它可能会给某些浏览器（弹出窗口阻止程序）带来问题。 因此，AccessEnabler在后台实施登录阶段，而不需要由程序员创建的Web容器。

以下是程序员在实施TempPass以进行无刷新登录和注销时需要注意的一些方面：

- 在开始身份验证之前，只需要为非TempPass MVPD创建iFrame或弹出窗口。 通过读取MVPD对象的`tempPass`属性（由`setConfig()` / `displayProviderDialog()`返回），程序员可以检测MVPD是否为TempPass。

- `createIFrame()`回调必须包含对TempPass的检查，并且仅在MVPD不是TempPass时才执行其逻辑。

- `destroyIFrame()`回调必须包含对TempPass的检查，并且仅在MVPD不是TempPass时才执行其逻辑。

- 身份验证完成后将调用`setAuthenticationStatus()`和`sendTrackingData()`回调（与普通MVPD的无刷新流中完全相同）。

>[!NOTE]
>
>此流程仅适用于无刷新的TempPass。 对于刷新流，需要显式处理TempPass（当TempPass需要iFrame/弹出窗口时）

</br>

以下代码示例演示了如何处理程序员网站上的MVPD窗口（适用于普通MVPD和TempPass）：

```javascript
    var aeHostname = "https://entitlement.auth.adobe.com";
    var mvpdWindow = null;
    var mvpd = <mvpd_object_from_displayProviderDialog>;
    var useIframeLogin = <boolean_depending_on_browser_or_Programmer_preferences>;
    var backgroundLogin = <boolean_depending_on_Programmer_preferences>;
     
    // Do not create any windows for refreshless and temp pass
    if (!(backgroundLogin && mvpd.tempPass)) {
        if (backgroundLogin && !mvpd.popup) {
            mvpdWindow = window.open(aeHostname, "mvpdwindow");
        } else if (mvpd.popup && !useIframeLogin) {
            var width = mvpd.width;
            var height = mvpd.height;
            // Center on screen
            var top = (document.all) ? window.screenTop : window.screenY + 100;
            var left = (document.all) ? window.screenLeft : window.screenX + window.innerWidth / 2 - width / 2;
        
            mvpdWindow = window.open(aeHostname, "mvpdframe",
                           "width=" + width + ",height=" + height + ",top=" + top + ",left=" + left);
            // Monitor the mvpd popup for close
            if (!backgroundLogin) {
                clearInterval(cancelTimer);
                cancelTimer = setInterval(function () {
                    if (mvpdWindow && mvpdWindow.closed) {
                        clearInterval(cancelTimer);
                        $('#mvpddiv').hide();
                        accessEnablerAPI.setSelectedProvider(null);
                    }
                }, 200);
            }
        }
    }
```
