---
title: " Adobe Extension Manager CC and Exchange issues\t\t"
tags:
  - Adobe Exchange
  - Adobe Extension Manager CC
  - Creative Cloud
  - error
  - extensions
  - installation
url: 2048.html
id: 2048
comments: false
category:
  - Extensions and Scripts
  - Photoshop
date: 2013-06-20 23:35:30
---

Soon after the public launch of Creative Cloud applications, some users reported to have run into troubles installing extensions from the Adobe Exchange panel because of Adobe Extension Manager CC errors. Here are the most common issues and the suggested workarounds, waiting for the official fix. **UPDATE** (July 2015): if you're in troubles with Adobe Extension Manager CC after having updated to CC2015 be aware that you're out of luck. It's been discontinued, that is: **Adobe Extension Manager will not support CC2015**. Please [read this page](http://localhost:8888/2015/06/html-panel-tips-17-cc2015-survival-guide/) for updated information and workaround.

Troubleshooting
---------------

First and above all, please check to have every CC app **up-to-date** \- you may need to quit the CC app and restart to see the updates. To the date of this writing, the Adobe Exchange panel's version is 1.0.0. When you launch Exchange, it should automatically checks whether updates are available or not. To be sure, please look for the `ExtensionBundleVersion="1.0.0"` string in these files:

\[Mac\] /Library/Application Support/Adobe/CEPServiceManager4/extensions/AdobeExchange/CSXS/manifest.xml
\[Win\] C:\\Program Files (x86)\\Common Files\\Adobe\\CEPServiceManager4\\extensions\\AdobeExchange\\CSXS\\manifest.xml

The most frequent error I've been reported is Adobe Extension Manager CC saying something along these lines:

> Error occurred during copying file  '/private/var/folders/sg/39jyqx0s5fl6d41yxpvm5qm0000gn/T/TemporaryItems/FlashTmp7/com-cs-extensions-AdvancedLocalContrastEhancer.zxp to  '/Users/***/Library/ApplicationSupport/Adobe/Extension Manager CC/EM Store/Photoshop/com-cs-extensions-AdvancedLocalContrastEhancer_20130620003905.ZXP' The extension will not be installed.

![Adobe Extension Manager CC Error](http://localhost:8888/wp-content/uploads/2013/06/AdobeExtensionManager_Error.png) Adobe engineers are working on that and suggest to: "**Click OK and close Extension Manager. Disregard the error messages in the Panel and try the installation again**". It turns out that what fires the error is some kind of variation of this pattern:

1.  You choose to install an extension from Exchange Panel (download is fairly fast, but possibly quite long wait while "Installing" with no progress information)
2.  Exchange Panel alerts: _Adobe Extension Manager is required to install this extension. Do you want to launch Extension Manager now?_
3.  Choosing Yes launches Adobe Extension Manager CC, and installation begins.
4.  Adobe Extension Manager shows the "Accept License" stuff.
5.  Adobe Extension Manager alerts: "_You have to quit Photoshop CC before installing, click Retry to continue_"
6.  In Photoshop, the Exchange panel shows an error message: "_Installation failed. Please close Extension Manager and try again._"
7.  Quit Photoshop, back to Extension Manager CC you click Retry.
8.  Adobe Extension Manager CC _may_ ask for administrator password.
9.  Adobe Extension Manager CC fires the error message: "_Error occurred copying file \[...\] The extension will not be installed_". You click OK.
10.  There's no extensions listed as installed in Adobe Extension Manager; the Exchange panels doesn't mark the extension as installed.
11.  If you restart Photoshop, **the extension is installed**. (some report a crash after the first launch).

[![Adobe Exchange Warning](http://localhost:8888/wp-content/uploads/2013/06/AdobeExchange_Warning-125x300.png)](http://localhost:8888/wp-content/uploads/2013/06/AdobeExchange_Warning.png)

### Other suggestions coming from Adobe

*   If you install Extension Manager CC through CC then please close CC before launching the Exchange panel.
*   If you see the message in the screenshot at right in the Exchange panel during a product installation please click yes. CC will load and download Extension Manager CC. Once the download is complete, please close the CC application before clicking ’install’ again in the Exchange panel.
*   If you are prompted to launch Extension Manager as part of a product installation in the Panel then please click Cancel and try the installation again. If you are prompted to launch Extension Manager again then please click OK.

### If Adobe Extension Manager doesn't list your application

It may happen that Photoshop or your app of choice (Illustrator, InDesign, etc) isn't listed in the left column of Adobe Extension Manager (CC or CS6). If this happens, open the Adobe ExtendScript Tool Kit app, input `BridgeTalk.__diagnostics__;` and from the menu choose _Debug - Run_. (shortcut: **⌘**R).

[![ESTK Diagnostic](http://localhost:8888/wp-content/uploads/2013/06/ESTK_Diagnostic.png)](http://localhost:8888/wp-content/uploads/2013/06/ESTK_Diagnostic.png)

In the Javascript Console (if you don't see it, find it in the _Window - Javascript Console_ menu) look for the Photoshop or your favorite app path, and check that it is correct - i.e. it correspond with the actual application's path. If that's not the case, chances are that you've to uninstall and reinstall it in order to let Extension Manager properly recognize it.

### Adobe Extension Manager CC known bugs

These are going to be included in the forthcoming fix.

1.  AEM doesn't show product icons - just the default jigsaw one.
2.  Extensions updates don't show up in AEM.

If you're in need to use the Update feature, please use the command line (run it to see usage help information):

\[Win\] ExManCmd.exe (run in Command Prompt)
\[Mac\] Adobe Extension Manager CC.app/Contents/MacOS/ExManCmd (run in Terminal)

### Keep calm and don't panic

We all hate transitions, don't we? Yet Apparently **extensions are installed anyway** and Adobe engineers are fully aware of the problem and currently working on a patch. Hey, [it could be worse](http://www.youtube.com/watch?v=9AFf0ysgNiM "Frankenstein Jr.") ;-)