---
id: 2431
title: 'HTML Panels Tips: #1 Debugging'
date: 2014-01-30T22:37:32+01:00
author: Davide Barranca
excerpt: "Since latest updates, Photoshop CC (and possibly other Adobe apps too) need remote connecting for debugging - here's a quick how-to."
layout: post
guid: http://www.davidebarranca.com/?p=2431
permalink: /2014/01/html-panels-tips-1-debugging/
standard_seo_post_level_layout:
  - ""
standard_link_url_field:
  - ""
standard_seo_post_meta_description:
  - 'HTML Panels tip - How to debug an HTML Extension in Photoshop via remote connection with the Chrome Developer Tools'
seo_post_level_layout:
  - ""
link_url_field:
  - ""
seo_post_meta_description:
  - How to use remote debugging with Google Chrome, Safari or Firefox in Creative Cloud apps
image: /wp-content/uploads/2014/01/ChromeDebuggingTool.jpg
categories:
  - Coding
  - HTML Panels
tags:
  - Chrome Developer Tools
  - debugging
  - extension
  - html
  - panel
---
<div class="pf-content">
  <p>
    Since latest updates, Photoshop (and possibly other Adobe apps too) need remote connecting for debugging HTML Extensions &#8211; here&#8217;s a quick how-to.<br /> <!--more-->
    
    <br /> <img class="alignleft size-full wp-image-2434" src="http://localhost:8888/wp-content/uploads/2014/01/debug.png" alt=".debug file" width="300" height="223" srcset="http://localhost:8888/wp-content/uploads/2014/01/debug.png 300w, http://localhost:8888/wp-content/uploads/2014/01/debug-150x111.png 150w" sizes="(max-width: 300px) 100vw, 300px" />In the root folder of your extension, create a blank <code>.debug</code> file (you may need to make your OS show hidden files &#8211; <a href="http://localhost:8888/wp-content/uploads/2014/01/Toggle-Hidden-Files.app_.zip">download here</a> an Automator app for OSX that toggles the visibility on/off).
  </p>
  
  <p>
    This file needs to be formatted as an XML and should contain information about the ID of your extension, and the host/port (in the range 1024 &#8211; 65534) you want to connect through.
  </p>
  
  <pre class="lang:xhtml mark:3,5 decode:true crayon-selected">&lt;?xml version="1.0" encoding="UTF-8"?&gt; 
&lt;ExtensionList&gt;
    &lt;Extension Id="com.example.communication"&gt;
        &lt;HostList&gt;
           
            &lt;!-- Comment Host tags according to the apps you want your panel to support --&gt;
            
            &lt;!-- Photoshop --&gt;
            &lt;Host Name="PHXS" Port="8088"/&gt;
            
            &lt;!-- Illustrator --&gt;
            &lt;Host Name="ILST" Port="8088"/&gt;

            &lt;!-- InDesign --&gt;
            &lt;Host Name="IDSN" Port="8088" /&gt;
            
            &lt;!-- Premiere --&gt;
            &lt;Host Name="PPRO" Port="8088" /&gt;
            
            &lt;!-- AfterEffects --&gt;
            &lt;Host Name="AEFT" Port="8088" /&gt;
            
            &lt;!-- PRELUDE --&gt;
            &lt;Host Name="PRLD" Port="8088" /&gt;
            
            &lt;!-- FLASH Pro --&gt;
            &lt;Host Name="FLPR" Port="8088" /&gt;
                        
        &lt;/HostList&gt;
    &lt;/Extension&gt;
&lt;/ExtensionList&gt;</pre>
  
  <p>
    In my case the ID is <code>com.example.communication</code> and the port <code>8088</code>.
  </p>
  
  <p>
    <img class="alignleft size-medium wp-image-2438" src="http://localhost:8888/wp-content/uploads/2014/01/localhost-300x160.png" alt="localhost" width="300" height="160" srcset="http://localhost:8888/wp-content/uploads/2014/01/localhost-300x160.png 300w, http://localhost:8888/wp-content/uploads/2014/01/localhost-150x80.png 150w, http://localhost:8888/wp-content/uploads/2014/01/localhost.png 314w" sizes="(max-width: 300px) 100vw, 300px" />Now, while your Photoshop extension is open, point your browser (either Chrome, Safari, Firefox&#8230;) to <code>http://localhost:8088</code> (of course use the port you&#8217;ve set in the XML file). You should see something like the screenshot on the left. If you click the index.html link the Chrome Developer Tools or the Safari/Firefox equivalent will open and you&#8217;ll be able to inspect your extension like any other webpage on the internet.
  </p>
  
  <p>
    Mind you, as far as I know this debug method doesn&#8217;t reach the JSX level &#8211; it&#8217;s just for the HTML panel and its JS; if you&#8217;re wondering a <code>debugger;</code> call in the JSX doesn&#8217;t fire up ESTK.<br /> <img class="aligncenter size-full wp-image-2440" src="http://localhost:8888/wp-content/uploads/2014/01/ChromeDebuggingTool.jpg" alt="Chrome Debugging Tool" width="570" height="308" srcset="http://localhost:8888/wp-content/uploads/2014/01/ChromeDebuggingTool.jpg 570w, http://localhost:8888/wp-content/uploads/2014/01/ChromeDebuggingTool-150x81.jpg 150w, http://localhost:8888/wp-content/uploads/2014/01/ChromeDebuggingTool-300x162.jpg 300w" sizes="(max-width: 570px) 100vw, 570px" />
  </p>
</div>

<!-- Share-Widget Button BEGIN --><a href="javascript:void(0);" myshare\_id="mys\_shareit" myshare\_url="http://localhost:8888/2014/01/html-panels-tips-1-debugging/" myshare\_title="HTML Panels Tips: #1 Debugging" rel="nofollow" onclick=" return false;" style="text-decoration:none; color:#000000; font-size:11px; line-height:20px;"> 

<img src="http://localhost:8888/wp-content/plugins/share-widget/img/share-button-white-small.png" height="20" alt="Share" style="border:0" /> </a> <!-- Share-Widget Button END -->