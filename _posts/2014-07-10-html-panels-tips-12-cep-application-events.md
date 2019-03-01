---
id: 2682
title: 'HTML Panels Tips: #12 CEP Application Events'
date: 2014-07-10T15:49:32+01:00
author: Davide Barranca
excerpt: 'HTML Panels can listen to CEP Application Events - that is to say, Events dispatched by the Host (Photoshop, InDesign...) when something related to the app itself, or its documents, happens.'
layout: post
guid: http://www.davidebarranca.com/?p=2682
permalink: /2014/07/html-panels-tips-12-cep-application-events/
seo_post_level_layout:
  - ""
link_url_field:
  - ""
seo_post_meta_description:
  - HTML Panels can listen to CEP Application Events, dispatched by the Host (Photoshop) when something related to the app itself happens.
image: /wp-content/uploads/2014/07/Log.png
categories:
  - Coding
  - HTML Panels
tags:
  - CEP
  - Events
  - Host
---
<div class="pf-content">
  <p>
    HTML Panels can listen to CEP Application Events &#8211; that is to say, Events dispatched by the Host (Photoshop, InDesign&#8230;) when something related to the app itself, or its documents, happens.<!--more-->
  </p>
  
  <h2>
    List of Photoshop Events
  </h2>
  
  <p>
    To date (CC 2014, version 2014.0.0 Release, 20140508.r58) this is the list of the Events that Photoshop is able to dispatch. Hopefully the names are self-explanatory.
  </p>
  
  <ul>
    <li>
      <code>"com.adobe.csxs.events.AppOffline"</code>
    </li>
    <li>
      <code>"com.adobe.csxs.events.AppOnline"</code>
    </li>
    <li>
      <code>"applicationActivate"</code>
    </li>
    <li>
      <code>"applicationBeforeQuit"</code>
    </li>
    <li>
      <code>"documentAfterActivate"</code>
    </li>
    <li>
      <code>"documentAfterDeactivate"</code>
    </li>
    <li>
      <code>"documentEdited"</code>
    </li>
  </ul>
  
  <p>
    Mind you, according to the <em>Extension SDK guide</em> some of them aren&#8217;t supported in Photoshop yet; others aren&#8217;t listed there (but they come from the CEP HTML Test Panel from the <a title="Adobe CEP Samples" href="https://github.com/Adobe-CEP/Samples/tree/master/CEP_HTML_Test_Extension_5.0" target="_blank">Samples GitHub repo</a>).
  </p>
  
  <h2>
    Implementation
  </h2>
  
  <p>
    As easy as it gets &#8211; this goes in the JS:
  </p>
  
  <pre class="lang:js decode:true ">var csInterface = new CSInterface();
csInterface.addEventListener("appOffline", logEvent);
// just a demo callback
function logEvent(evt) { console.dir(evt); }</pre>
  
  <h2>
    Event Parameters
  </h2>
  
  <p>
    The event object passed to the callback in Photoshop has several properties:
  </p>
  
  <ul>
    <li>
      <code>appId: "PHSX"</code>
    </li>
    <li>
      <code>extensionId: ""</code>
    </li>
    <li>
      <code>scope: "APPLICATION"</code>
    </li>
    <li>
      <code>type</code>
    </li>
    <li>
      <code>data</code>
    </li>
  </ul>
  
  <p>
    According to each <code>type</code>, the <code>data</code> might come as an XML String, as follows:
  </p>
  
  <pre class="lang:js decode:true ">type: "com.adobe.csxs.events.AppOffline"
data: ""

type: "com.adobe.csxs.events.AppOnline"
data: ""

type: "applicationActivate"
data: "&lt;applicationActivate/&gt;"

type: "applicationBeforeQuit"
data: "&lt;applicationBeforeQuit/&gt;"

type: "documentAfterActivate"
data: "&lt;documentAfterActivate&gt;&lt;name&gt;icon&lt;/name&gt;&lt;url&gt;file:///Macintosh SSD/Users/Davide/Desktop/icon.tif&lt;/url&gt;&lt;/documentAfterActivate&gt;"

type: "documentAfterDeactivate"
data: "&lt;documentAfterDeactivate&gt;&lt;name&gt;icon&lt;/name&gt;&lt;url&gt;file:///Macintosh SSD/Users/Davide/Desktop/icon.tif&lt;/url&gt;&lt;/documentAfterDeactivate&gt;"

type: "documentEdited"
data: "&lt;documentEdited&gt;&lt;name&gt;icon&lt;/name&gt;&lt;url&gt;file:///Macintosh SSD/Users/Davide/Desktop/icon.tif&lt;/url&gt;&lt;/documentEdited&gt;"</pre>
  
  <p>
    Mind you, I&#8217;ve been able to log the <code>applicationBeforeQuit</code> event only trying to quit Photoshop when an unsaved document was open &#8211; this gives the time for the <code>console.dir</code> to be logged, otherwise the Object is empty, after the app has quit.
  </p>
  
  <p>
    Also, I haven&#8217;t been able to successfully intercept the <code>"com.adobe.csxs.events.ExtensionUnloaded"</code> event, which would be of <em>great use</em>.
  </p>
</div>

<!-- Share-Widget Button BEGIN --><a href="javascript:void(0);" myshare\_id="mys\_shareit" myshare\_url="http://localhost:8888/2014/07/html-panels-tips-12-cep-application-events/" myshare\_title="HTML Panels Tips: #12 CEP Application Events" rel="nofollow" onclick=" return false;" style="text-decoration:none; color:#000000; font-size:11px; line-height:20px;"> 

<img src="http://localhost:8888/wp-content/plugins/share-widget/img/share-button-white-small.png" height="20" alt="Share" style="border:0" /> </a> <!-- Share-Widget Button END -->