---
id: 2503
title: 'HTML Panels Tips: #8 Photoshop Events, Take 2'
date: 2014-02-07T22:53:41+01:00
author: Davide Barranca
excerpt: "There's a second way to listen to Photoshop Events from an HTML Panel - i.e. using the PSHostAdapters libraries - and today's tip will show you how."
layout: post
guid: http://www.davidebarranca.com/?p=2503
permalink: /2014/02/html-panels-tips-8-photoshop-events-pshostadapter-libraries/
standard_seo_post_level_layout:
  - ""
standard_link_url_field:
  - ""
standard_seo_post_meta_description:
  - 'This tip shows you the second way to make an HTML Panel listen and react to Photoshop Events: via PSHostAdapter libraries.'
image: /wp-content/uploads/2014/02/PSEvents.png
categories:
  - Coding
  - HTML Panels
tags:
  - addEventListener
  - Events
  - PSHostAdapter
---
<div class="pf-content">
  <p>
    There&#8217;s a second way to listen to Photoshop Events from an HTML Panel &#8211; i.e. using the PSHostAdapters libraries &#8211; and today&#8217;s tip will show you how.
  </p>
  
  <p>
    <!--more-->
  </p>
  
  <p>
    In <a title="HTML Panels Tips: #7 Photoshop Events, Take 1" href="http://localhost:8888/2014/02/html-panels-tips-7-events-photoshopregisterevent-photoshopcallback/" target="_blank">Tip #7</a> I&#8217;ve demoed how using <code>PhotoshopRegisterEvent</code> let you listen to events and register callbacks: but that is just one way out of the two available (as it was back in the Flash land) so let&#8217;s find the second one.
  </p>
  
  <p>
    <img class="alignleft size-full wp-image-2497" alt="PSEvents" src="http://localhost:8888/wp-content/uploads/2014/02/PSEvents.png" width="270" height="159" srcset="http://localhost:8888/wp-content/uploads/2014/02/PSEvents.png 270w, http://localhost:8888/wp-content/uploads/2014/02/PSEvents-150x88.png 150w" sizes="(max-width: 270px) 100vw, 270px" />
  </p>
  
  <h2>
    Example
  </h2>
  
  <p>
    I&#8217;ll be using as a starting point the very same Tip #7 code. This example panel has a switch that enables a listener on the cut, copy and paste events in Photoshop &#8211; the callback passes the corresponding StringID for displaying to the Text Area. Mind you, when an HTML Panel is open keyboard shortcuts might not work, so you&#8217;d be better off using the menus in order to trigger the events correctly &#8211; Adobe engineers are aware and will fix that in the future.
  </p>
  
  <h3>
    Libraries
  </h3>
  
  <p>
    You need two PSHostAdapter libraries, that you can grab from the Extension Builder installation (in the Eclipse application): they are <code>ps_host_adapter-2.0.js</code> and (depending on your OS) <code>PSHostAdapter.plugin</code> or <code>PSHostAdapter.plugin.8li</code>. The following paths come from my OSX installation, for PC users it shouldn&#8217;t be any different &#8211; just use the Eclipse application as the root:
  </p>
  
  <pre class="lang:js highlight:0 decode:true">/Applications/eclipse/plugins/com.adobe.cside.html.libsinstaller_1.0.0.201307260955/archive/jsar-1.0/release/ps_host_adapter-2.0.js

/Applications/eclipse/plugins/com.adobe.cside.html.libsinstaller_1.0.0.201307260955/archive/csadapters-3.0/ps_host_adapter/</pre>
  
  <p>
    You should copy the <code>ps_host_adapter-2.0.js</code> somewhere in your project and include it in the index.html. Conversely, <code>PSHostAdapter.plugin</code> or <code>PSHostAdapter.plugin.8li</code> file must go in the Photoshop Plug-Ins folder (<code>Plug-Ins/Automate/</code> works just fine on my OSX installation).
  </p>
  
  <h3>
    HTML
  </h3>
  
  <p>
    It&#8217;s basically the same of Tip #7, plus the new linked script. As you see, I&#8217;m using the Topcoat CSS library to style the panel GUI (refer to <a title="HTML Panels Tips: #6 integrating Topcoat CSS" href="http://localhost:8888/2014/02/html-panels-tips-6-integrating-topcoat-css/" target="_blank">Tip #6</a> for detailed instruction).
  </p>
  
  <pre class="lang:xhtml decode:true ">&lt;!doctype html&gt;
&lt;html&gt;
&lt;head&gt;
&lt;meta charset="utf-8"&gt;
&lt;link id="hostStyle" rel="stylesheet" href="css/theme.css"/&gt;
&lt;link id="theme" rel="stylesheet" href="css/light.css"/&gt;
&lt;title&gt;&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;

    &lt;div style="width: 80%; margin:0 auto"&gt;
        &lt;h3 class="center"&gt;PS Events&lt;/h3&gt;
        &lt;label class="topcoat-switch"&gt;
            &lt;input id="registerEvent" type="checkbox" class="topcoat-switch__input"&gt;
            &lt;div class="topcoat-switch__toggle"&gt;&lt;/div&gt;
        &lt;/label&gt;
        &lt;input type="text" id ="result" class="topcoat-text-input" style="margin-left:10px" placeholder="Listen for Events" value=""&gt;
    &lt;/div&gt;

    &lt;script src="js/libs/CSInterface-4.0.0.js"&gt;&lt;/script&gt;
    &lt;script src="js/libs/ps_host_adapter-2.0.js"&gt;&lt;/script&gt;
    &lt;script src="js/libs/jquery-2.0.2.min.js"&gt;&lt;/script&gt;
    &lt;script src="js/themeManager.js"&gt;&lt;/script&gt;
    &lt;script src="js/main.js"&gt;&lt;/script&gt;

&lt;/body&gt;
&lt;/html&gt;</pre>
  
  <h3>
     Javascript
  </h3>
  
  <p>
    Compared to <a title="HTML Panels Tips: #7 Photoshop Events, Take 1" href="http://localhost:8888/2014/02/html-panels-tips-7-events-photoshopregisterevent-photoshopcallback/" target="_blank">Photoshop Events, Take 1</a> the <code>Register()</code> function is where things are different &#8211; the rest of the code is unchanged. It&#8217;s a matter of adding an Event Listener to a <code>PSEventAdapter</code> Instance, passing either a constant (see in <code>ps_host_adapter-2.0.js</code> for a list of the available ones) or an event TypeID, which you can grab as usual from the Scripting Listener log.
  </p>
  
  <p>
    The <code>PSCallback()</code> function communicates then with the JSX the <code>event.type</code> in order to retrieve the StringID, which is passed to the Text Area.
  </p>
  
  <pre class="lang:js decode:true">(function () {
    'use strict';

    var csInterface = new CSInterface();   

    function Register(inOn) {

        // Events from ps_host_adapter-2.0.js
        // PSEvent.CUT = "1668641824";
        // PSEvent.COPY = "1668247673";
        // PSEvent.PASTE = "1885434740";
        if (inOn) {
            PSEventAdapter.getInstance().addEventListener( PSEvent.CUT, PSCallback);
            PSEventAdapter.getInstance().addEventListener( PSEvent.COPY, PSCallback);
            PSEventAdapter.getInstance().addEventListener( PSEvent.PASTE, PSCallback);
        } else {
            PSEventAdapter.getInstance().removeEventListener(PSEvent.CUT, PSCallback);
            PSEventAdapter.getInstance().removeEventListener(PSEvent.COPY, PSCallback);
            PSEventAdapter.getInstance().removeEventListener(PSEvent.PASTE, PSCallback);
        }
    }

    function init() {

        themeManager.init();

        $('#registerEvent').change(function() {
            Register( $(this).is(':checked') );
        });
    }

    function PSCallback(csEvent) {
        // send to JSX to convert typeIDs to stringIDs
        csInterface.evalScript('convertTypeID(' + JSON.stringify(csEvent.type) + ')', function(res) {
            $('#result').val(res.toString());
        });
    }

    init();

}());</pre>
  
  <p>
    Which is similar to the code for Flash panels in Extension Builder 2:
  </p>
  
  <pre class="lang:as decode:true">PSEventAdapter.getInstance().addEventListener(PSEvent.ROTATE, handleEvent);
private function handleEvent(event:PSEvent):void
{
    //handle rotate event
}</pre>
  
  <h3>
     JSX
  </h3>
  
  <p>
    Just a dedicate function here:
  </p>
  
  <pre class="lang:js decode:true ">function convertTypeID (tID) {
	return typeIDToStringID(Number(tID));
}</pre>
  
  <p>
    &nbsp;
  </p>
</div>

<!-- Share-Widget Button BEGIN --><a href="javascript:void(0);" myshare\_id="mys\_shareit" myshare\_url="http://localhost:8888/2014/02/html-panels-tips-8-photoshop-events-pshostadapter-libraries/" myshare\_title="HTML Panels Tips: #8 Photoshop Events, Take 2" rel="nofollow" onclick=" return false;" style="text-decoration:none; color:#000000; font-size:11px; line-height:20px;"> 

<img src="http://localhost:8888/wp-content/plugins/share-widget/img/share-button-white-small.png" height="20" alt="Share" style="border:0" /> </a> <!-- Share-Widget Button END -->