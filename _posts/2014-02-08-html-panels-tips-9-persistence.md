---
id: 2512
title: 'HTML Panels Tips: #9 Persistence'
date: 2014-02-08T18:27:18+01:00
author: Davide Barranca
excerpt: "How to make the HTML Panel's session persist even if the panel is closed in Photoshop."
layout: post
guid: http://www.davidebarranca.com/?p=2512
permalink: /2014/02/html-panels-tips-9-persistence/
standard_seo_post_level_layout:
  - ""
standard_link_url_field:
  - ""
standard_seo_post_meta_description:
  - How to set up Persistence (i.e. persistent sessions) in Photoshop HTML Panels, dispatching a PhotoshopPersistent CSEvent.
seo_post_level_layout:
  - ""
link_url_field:
  - ""
seo_post_meta_description:
  - ""
image: /wp-content/uploads/2014/02/persistent.png
categories:
  - Coding
  - HTML Panels
tags:
  - persistent
  - photoshopPersistent
  - tip
---
<div class="pf-content">
  <p>
    How to make the HTML Panel&#8217;s session persist even if the panel is closed in Photoshop.
  </p>
  
  <p>
    <!--more-->
  </p>
  
  <p>
    Each time an HTML Panel is launched, its GUI resets to the default values and initialization is performed. This happens not only in between Photoshop sessions, but also when the Panel is collapsed, iconized or closed. Today&#8217;s tip shows you how to make the Panel persistent, as we did back in Flash land with:
  </p>
  
  <pre class="lang:as decode:true">// In Flash panels you would:
CSXSInterface.instance.evalScript("PhotoshopPersistent");</pre>
  
  <p>
    The mechanism now in HTML land requires Event dispatching &#8211; see <a title="HTML Panels Tips: #7 Photoshop Events, Take 1" href="http://localhost:8888/2014/02/html-panels-tips-7-events-photoshopregisterevent-photoshopcallback/" target="_blank">Tip #7</a>: Photoshop Events Take 1 for the details.
  </p>
  
  <h2>
    Code on GitHub
  </h2>
  
  <p>
    Please <a title="Persistence Panel on GitHub" href="https://github.com/undavide/PS-Panels-Boilerplate/tree/master/src/com.undavide.persistent1" target="_blank">find here</a> the complete code for this example.
  </p>
  
  <h2>
    Example
  </h2>
  
  <p>
    <img class="alignleft size-full wp-image-2513" src="http://localhost:8888/wp-content/uploads/2014/02/persistent.png" alt="Persistent" width="315" height="306" srcset="http://localhost:8888/wp-content/uploads/2014/02/persistent.png 315w, http://localhost:8888/wp-content/uploads/2014/02/persistent-150x145.png 150w, http://localhost:8888/wp-content/uploads/2014/02/persistent-300x291.png 300w" sizes="(max-width: 315px) 100vw, 315px" />
  </p>
  
  <p>
    This panel has a switch that enables Panel&#8217;s persistence, and a bunch of other switches that have no effect other than show persistence in action.
  </p>
  
  <p>
    When the Persistent is ON, you can play with the other ones and collapse/close then reopen the panel: the secondary switches are shown in the state they were before. Conversely, if Persistent is OFF, each time the panel is opened all the switches are reset.
  </p>
  
  <h3>
    HTML
  </h3>
  
  <p>
    I make use of the Topcoat CSS Library (refer to <a title="HTML Panels Tips: #6 integrating Topcoat CSS" href="http://localhost:8888/2014/02/html-panels-tips-6-integrating-topcoat-css/" target="_blank">Tip #6</a> for the implementation).
  </p>
  
  <pre class="height-set:true lang:xhtml decode:true">&lt;!doctype html&gt;
&lt;html&gt;
&lt;head&gt;
&lt;meta charset="utf-8"&gt;
&lt;link id="hostStyle" rel="stylesheet" href="css/theme.css"/&gt;
&lt;link id="theme" rel="stylesheet" href="css/light.css"/&gt;
&lt;title&gt;&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;

    &lt;div id="container" &gt;
        &lt;h3 class="center"&gt;Panel Persistence&lt;/h3&gt;
        &lt;label class="switch-label"&gt;Persistent:&lt;/label&gt;
        &lt;label class="topcoat-switch"&gt;
            &lt;input id="persistenceSwitch" type="checkbox" class="topcoat-switch__input"&gt;
            &lt;div class="topcoat-switch__toggle"&gt;&lt;/div&gt;
        &lt;/label&gt;
        &lt;hr&gt;
        &lt;div class="flex-container"&gt;
            &lt;label class="topcoat-switch switch"&gt;
                &lt;input id="testSwitch1" type="checkbox" class="topcoat-switch__input"&gt;
                &lt;div class="topcoat-switch__toggle"&gt;&lt;/div&gt;
            &lt;/label&gt;
            &lt;label class="topcoat-switch switch"&gt;
                &lt;input id="testSwitch2" type="checkbox" class="topcoat-switch__input"&gt;
                &lt;div class="topcoat-switch__toggle"&gt;&lt;/div&gt;
            &lt;/label&gt;
            &lt;label class="topcoat-switch switch"&gt;
                &lt;input id="testSwitch3" type="checkbox" class="topcoat-switch__input"&gt;
                &lt;div class="topcoat-switch__toggle"&gt;&lt;/div&gt;
            &lt;/label&gt;
            &lt;label class="topcoat-switch switch"&gt;
                &lt;input id="testSwitch4" type="checkbox" class="topcoat-switch__input"&gt;
                &lt;div class="topcoat-switch__toggle"&gt;&lt;/div&gt;
            &lt;/label&gt;
            &lt;label class="topcoat-switch switch"&gt;
                &lt;input id="testSwitch5" type="checkbox" class="topcoat-switch__input"&gt;
                &lt;div class="topcoat-switch__toggle"&gt;&lt;/div&gt;
            &lt;/label&gt;
            &lt;label class="topcoat-switch switch"&gt;
                &lt;input id="testSwitch6" type="checkbox" class="topcoat-switch__input"&gt;
                &lt;div class="topcoat-switch__toggle"&gt;&lt;/div&gt;
            &lt;/label&gt;
            &lt;label class="topcoat-switch switch"&gt;
                &lt;input id="testSwitch7" type="checkbox" class="topcoat-switch__input"&gt;
                &lt;div class="topcoat-switch__toggle"&gt;&lt;/div&gt;
            &lt;/label&gt;
            &lt;label class="topcoat-switch switch"&gt;
                &lt;input id="testSwitch8" type="checkbox" class="topcoat-switch__input"&gt;
                &lt;div class="topcoat-switch__toggle"&gt;&lt;/div&gt;
            &lt;/label&gt;
            &lt;label class="topcoat-switch switch"&gt;
                &lt;input id="testSwitch9" type="checkbox" class="topcoat-switch__input"&gt;
                &lt;div class="topcoat-switch__toggle"&gt;&lt;/div&gt;
            &lt;/label&gt;
        &lt;/div&gt;
    &lt;/div&gt;

    &lt;script src="js/libs/CSInterface-4.0.0.js"&gt;&lt;/script&gt;
    &lt;script src="js/libs/jquery-2.0.2.min.js"&gt;&lt;/script&gt;
    &lt;script src="js/themeManager.js"&gt;&lt;/script&gt;
    &lt;script src="js/main.js"&gt;&lt;/script&gt;

&lt;/body&gt;
&lt;/html&gt;</pre>
  
  <h3>
    Javascript
  </h3>
  
  <p>
    The <code>main.js</code> file is where things happen. A <code>Persistent()</code> function is called every time the <code>persistenceSwitch</code> changes: it sets up and dispatches a <code>CSEvent</code> (see the highlighted lines), which is bound to the extension&#8217;s id (<code>gExtensionId</code> variable).
  </p>
  
  <pre class="lang:js mark:10,12 decode:true ">(function () {
    'use strict';

    var csInterface = new CSInterface();   
    var gExtensionId = "com.example.persistent";

    function Persistent(inOn) {

        if (inOn){
            var event = new CSEvent("com.adobe.PhotoshopPersistent", "APPLICATION");
        } else {
            var event = new CSEvent("com.adobe.PhotoshopUnPersistent", "APPLICATION");
        }
        event.extensionId = gExtensionId;
        csInterface.dispatchEvent(event);
    }

    function init() {

        themeManager.init();

        $('#persistenceSwitch').change(function() {
            Persistent( $(this).is(':checked') );
        });
    }

    init();

}());</pre>
  
  <p>
    This makes the Panel persistent or&#8230; &#8220;unpersistent&#8221; 😉
  </p>
</div>

<!-- Share-Widget Button BEGIN --><a href="javascript:void(0);" myshare\_id="mys\_shareit" myshare\_url="http://localhost:8888/2014/02/html-panels-tips-9-persistence/" myshare\_title="HTML Panels Tips: #9 Persistence" rel="nofollow" onclick=" return false;" style="text-decoration:none; color:#000000; font-size:11px; line-height:20px;"> 

<img src="http://localhost:8888/wp-content/plugins/share-widget/img/share-button-white-small.png" height="20" alt="Share" style="border:0" /> </a> <!-- Share-Widget Button END -->