---
id: 2048
title: Adobe Extension Manager CC and Exchange issues
date: 2013-06-20T23:35:30+01:00
author: Davide Barranca
excerpt: Soon after the public launch of Adobe Creative Cloud apps, some users reported to have run into troubles installing extensions from the Adobe Exchange panel. Here are the most common issues and the suggested workarounds, waiting for official patches.
layout: post
guid: http://www.davidebarranca.com/?p=2048
permalink: /2013/06/adobe-extension-manager-cc-and-exchange-issues/
standard_seo_post_level_layout:
  - ""
standard_link_url_field:
  - ""
standard_seo_post_meta_description:
  - 'Extension Manager CC errors have been reported while installing extensions from the Exchange panel: here are the suggested workarounds.'
seo_post_level_layout:
  - ""
link_url_field:
  - ""
seo_post_meta_description:
  - ""
image: /wp-content/uploads/2013/06/cc_app.png
categories:
  - Extensions and Scripts
  - Photoshop
tags:
  - Adobe Exchange
  - Adobe Extension Manager CC
  - Creative Cloud
  - error
  - extensions
  - installation
---
<div class="pf-content">
  <p>
    Soon after the public launch of Creative Cloud applications, some users reported to have run into troubles installing extensions from the Adobe Exchange panel because of Adobe Extension Manager CC errors. Here are the most common issues and the suggested workarounds, waiting for the official fix.
  </p>
  
  <p>
    <strong>UPDATE</strong> (July 2015): if you&#8217;re in troubles with Adobe Extension Manager CC after having updated to CC2015 be aware that you&#8217;re out of luck. It&#8217;s been discontinued, that is: <strong>Adobe Extension Manager will not support CC2015</strong>. Please <a href="http://localhost:8888/2015/06/html-panel-tips-17-cc2015-survival-guide/">read this page</a> for updated information and workaround.<!--more-->
  </p>
  
  <h2>
    Troubleshooting
  </h2>
  
  <p>
    First and above all, please check to have every CC app <strong>up-to-date</strong> &#8211; you may need to quit the CC app and restart to see the updates.
  </p>
  
  <p>
    To the date of this writing, the Adobe Exchange panel&#8217;s version is 1.0.0. When you launch Exchange, it should automatically checks whether updates are available or not. To be sure, please look for the <code>ExtensionBundleVersion="1.0.0"</code> string in these files:
  </p>
  
  <pre class="theme:vs2012-black font-size:14 line-height:18 toolbar:2 nums:false whitespace-before:1 whitespace-after:1 lang:default highlight:0 decode:true">[Mac] /Library/Application Support/Adobe/CEPServiceManager4/extensions/AdobeExchange/CSXS/manifest.xml
[Win] C:\Program Files (x86)\Common Files\Adobe\CEPServiceManager4\extensions\AdobeExchange\CSXS\manifest.xml</pre>
  
  <p>
    The most frequent error I&#8217;ve been reported is Adobe Extension Manager CC saying something along these lines:
  </p>
  
  <blockquote>
    <p>
      Error occurred during copying file  &#8216;/private/var/folders/sg/39jyqx0s5fl6d41yxpvm5qm0000gn/T/TemporaryItems/FlashTmp7/com-cs-extensions-AdvancedLocalContrastEhancer.zxp to  &#8216;/Users/***/Library/ApplicationSupport/Adobe/Extension Manager CC/EM Store/Photoshop/com-cs-extensions-AdvancedLocalContrastEhancer_20130620003905.ZXP&#8217; The extension will not be installed.
    </p>
  </blockquote>
  
  <p>
    <img class="aligncenter size-full wp-image-2050" src="http://localhost:8888/wp-content/uploads/2013/06/AdobeExtensionManager_Error.png" alt="Adobe Extension Manager CC Error" width="363" height="220" srcset="http://localhost:8888/wp-content/uploads/2013/06/AdobeExtensionManager_Error.png 363w, http://localhost:8888/wp-content/uploads/2013/06/AdobeExtensionManager_Error-150x90.png 150w, http://localhost:8888/wp-content/uploads/2013/06/AdobeExtensionManager_Error-300x181.png 300w" sizes="(max-width: 363px) 100vw, 363px" />
  </p>
  
  <p>
    Adobe engineers are working on that and suggest to: &#8220;<strong>Click OK and close Extension Manager. Disregard the error messages in the Panel and try the installation again</strong>&#8220;.
  </p>
  
  <p>
    It turns out that what fires the error is some kind of variation of this pattern:
  </p>
  
  <ol>
    <li>
      You choose to install an extension from Exchange Panel (download is fairly fast, but possibly quite long wait while &#8220;Installing&#8221; with no progress information)
    </li>
    <li>
      Exchange Panel alerts: <em>Adobe Extension Manager is required to install this extension. Do you want to launch Extension Manager now?</em>
    </li>
    <li>
      Choosing Yes launches Adobe Extension Manager CC, and installation begins.
    </li>
    <li>
      Adobe Extension Manager shows the &#8220;Accept License&#8221; stuff.
    </li>
    <li>
      Adobe Extension Manager alerts: &#8220;<em>You have to quit Photoshop CC before installing, click Retry to continue</em>&#8220;
    </li>
    <li>
      In Photoshop, the Exchange panel shows an error message: &#8220;<em>Installation failed. Please close Extension Manager and try again.</em>&#8220;
    </li>
    <li>
      Quit Photoshop, back to Extension Manager CC you click Retry.
    </li>
    <li>
      Adobe Extension Manager CC <em>may</em> ask for administrator password.
    </li>
    <li>
      Adobe Extension Manager CC fires the error message: &#8220;<em>Error occurred copying file [&#8230;] The extension will not be installed</em>&#8220;. You click OK.
    </li>
    <li>
      There&#8217;s no extensions listed as installed in Adobe Extension Manager; the Exchange panels doesn&#8217;t mark the extension as installed.
    </li>
    <li>
      If you restart Photoshop, <strong>the extension is installed</strong>. (some report a crash after the first launch).
    </li>
  </ol>
  
  <p>
    <a href="http://localhost:8888/wp-content/uploads/2013/06/AdobeExchange_Warning.png" target="_blank"><img class="alignright size-medium wp-image-2051" src="http://localhost:8888/wp-content/uploads/2013/06/AdobeExchange_Warning-125x300.png" alt="Adobe Exchange Warning" width="125" height="300" /></a>
  </p>
  
  <h3>
    Other suggestions coming from Adobe
  </h3>
  
  <ul>
    <li>
      If you install Extension Manager CC through CC then please close CC before launching the Exchange panel.
    </li>
    <li>
      If you see the message in the screenshot at right in the Exchange panel during a product installation please click yes. CC will load and download Extension Manager CC. Once the download is complete, please close the CC application before clicking ’install’ again in the Exchange panel.
    </li>
    <li>
      If you are prompted to launch Extension Manager as part of a product installation in the Panel then please click Cancel and try the installation again. If you are prompted to launch Extension Manager again then please click OK.
    </li>
  </ul>
  
  <h3>
    If Adobe Extension Manager doesn&#8217;t list your application
  </h3>
  
  <p>
    It may happen that Photoshop or your app of choice (Illustrator, InDesign, etc) isn&#8217;t listed in the left column of Adobe Extension Manager (CC or CS6). If this happens, open the Adobe ExtendScript Tool Kit app, input <code>BridgeTalk.__diagnostics__;</code> and from the menu choose <em>Debug &#8211; Run</em>. (shortcut: <b>⌘</b>R).
  </p>
  
  <p style="text-align: center;">
    <a href="http://localhost:8888/wp-content/uploads/2013/06/ESTK_Diagnostic.png" target="_blank"><img class="aligncenter size-full wp-image-2058" src="http://localhost:8888/wp-content/uploads/2013/06/ESTK_Diagnostic.png" alt="ESTK Diagnostic" width="1232" height="738" srcset="http://localhost:8888/wp-content/uploads/2013/06/ESTK_Diagnostic.png 1232w, http://localhost:8888/wp-content/uploads/2013/06/ESTK_Diagnostic-150x89.png 150w, http://localhost:8888/wp-content/uploads/2013/06/ESTK_Diagnostic-300x179.png 300w, http://localhost:8888/wp-content/uploads/2013/06/ESTK_Diagnostic-1024x613.png 1024w" sizes="(max-width: 1232px) 100vw, 1232px" /></a>
  </p>
  
  <p>
    In the Javascript Console (if you don&#8217;t see it, find it in the <em>Window &#8211; Javascript Console</em> menu) look for the Photoshop or your favorite app path, and check that it is correct &#8211; i.e. it correspond with the actual application&#8217;s path. If that&#8217;s not the case, chances are that you&#8217;ve to uninstall and reinstall it in order to let Extension Manager properly recognize it.
  </p>
  
  <h3>
    Adobe Extension Manager CC known bugs
  </h3>
  
  <p>
    These are going to be included in the forthcoming fix.
  </p>
  
  <ol>
    <li>
      AEM doesn&#8217;t show product icons &#8211; just the default jigsaw one.
    </li>
    <li>
      Extensions updates don&#8217;t show up in AEM.
    </li>
  </ol>
  
  <p>
    If you&#8217;re in need to use the Update feature, please use the command line (run it to see usage help information):
  </p>
  
  <pre class="theme:vs2012-black font-size:14 line-height:18 toolbar:2 nums:false whitespace-before:1 whitespace-after:1 lang:default highlight:0 decode:true">[Win] ExManCmd.exe (run in Command Prompt)
[Mac] Adobe Extension Manager CC.app/Contents/MacOS/ExManCmd (run in Terminal)</pre>
  
  <h3>
    Keep calm and don&#8217;t panic
  </h3>
  
  <p>
    We all hate transitions, don&#8217;t we? Yet Apparently <strong>extensions are installed anyway</strong> and Adobe engineers are fully aware of the problem and currently working on a patch. Hey, <a title="Frankenstein Jr." href="http://www.youtube.com/watch?v=9AFf0ysgNiM" target="_blank">it could be worse</a> 😉
  </p>
</div>

<!-- Share-Widget Button BEGIN --><a href="javascript:void(0);" myshare\_id="mys\_shareit" myshare\_url="http://localhost:8888/2013/06/adobe-extension-manager-cc-and-exchange-issues/" myshare\_title="Adobe Extension Manager CC and Exchange issues" rel="nofollow" onclick=" return false;" style="text-decoration:none; color:#000000; font-size:11px; line-height:20px;"> 

<img src="http://localhost:8888/wp-content/plugins/share-widget/img/share-button-white-small.png" height="20" alt="Share" style="border:0" /> </a> <!-- Share-Widget Button END -->