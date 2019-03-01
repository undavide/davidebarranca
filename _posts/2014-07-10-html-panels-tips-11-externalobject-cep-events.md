---
id: 2672
title: 'HTML Panels Tips: #11 CEP Events (ExternalObject)'
date: 2014-07-10T10:03:47+01:00
author: Davide Barranca
excerpt: 'The new CEP5 (from Photoshop CC 2014 onwards) has introduced the possibility to dispatch Custom Events from JSX and listen to them in the HTML Panel - this Tip shows you how to implement this communication.'
layout: post
guid: http://www.davidebarranca.com/?p=2672
permalink: /2014/07/html-panels-tips-11-externalobject-cep-events/
seo_post_level_layout:
  - ""
link_url_field:
  - ""
seo_post_meta_description:
  - The new CEP5 (PS CC 2014) lets you dispatch ExtendScript Custom Events with payloads, using the integrated plugplugExternalObject library.
image: /wp-content/uploads/2014/07/JSX2CEP-panel.png
categories:
  - Coding
  - HTML Panels
tags:
  - CEP5
  - Events
  - ExternalObject
---
<div class="pf-content">
  <p>
    The new CEP5 (from Photoshop CC 2014 onwards) has introduced the possibility to dispatch <strong>Custom Events</strong> from JSX and listen to them in the HTML Panel &#8211; this Tip shows you how to implement this communication.
  </p>
  
  <h2>
    Events et al.
  </h2>
  
  <p>
    I&#8217;ve already covered Events in this series, so let me summarize what we can do with them and the different kinds of Events we might deal with.
  </p>
  
  <ol>
    <li>
      <strong>CEP Host Application Events</strong> &#8211; i.e. Photoshop events related either to the app itself or the documents (but from the &#8220;host app point of view&#8221;, see <a title="HTML Panels Tips: #12 CEP Application Events" href="http://localhost:8888/2014/07/html-panels-tips-12-cep-application-events/" target="_blank">this post</a>) such as: <ul>
        <li>
          <code>appOffline</code>
        </li>
        <li>
          <code>appOnline</code>
        </li>
        <li>
          <code>applicationActivate</code>
        </li>
        <li>
          <code>documentAfterActivate</code>
        </li>
        <li>
          <code>documentAfterDeactivate</code>
        </li>
      </ul>
    </li>
    
    <li>
      <strong>ExtendScript Events</strong> &#8211; i.e. Events related to <em>actual</em> Photoshop operations (dispatched when the user creates new Layer, selects/moves something, etc), see <a title="HTML Panels Tips: #7 Photoshop Events, Take 1" href="http://localhost:8888/2014/02/html-panels-tips-7-events-photoshopregisterevent-photoshopcallback/" target="_blank">this post</a> and a <a title="HTML Panels Tips: #8 Photoshop Events, Take 2" href="http://localhost:8888/2014/02/html-panels-tips-8-photoshop-events-pshostadapter-libraries/" target="_blank">different approach</a>.
    </li>
    <li>
      <strong>Custom ExtendScript Events</strong> &#8211; i.e. Events you dispatch yourself in the JSX to communicate with, and pass data to, the HTML Panel (what this post is all about).
    </li>
  </ol>
  
  <h2>
    ExternalObject
  </h2>
  
  <p>
    Photoshop, Illustrator and Premiere Pro CC 2014 by the time of this writing (July 2014) support natively the dispatching of custom Events (using the <code>PlugPlugExternalObject</code> &#8211; which library is embedded in the app and we can happily forget about it), while InDesign must explicitly add and reference it (<a title="PlugPlugExternalObject library for InDesign" href="https://github.com/Adobe-CEP/CEP-Resources/releases/tag/1" target="_blank">download it here</a> and see the SDK Guide at pag 45).
  </p>
  
  <p>
    Basically you set an Event Listener in the JS for a custom event (named as you like), providing a callback. Then in the JSX you create and dispatch a <code>CSXSEvent()</code> at will.
  </p>
  
  <h2>
    Example
  </h2>
  
  <p>
    You can download a full panel from my <strong>PS Panels Boilerplate</strong> repository on GitHub <a title="JSX to CEP Events Panel" href="https://github.com/undavide/PS-Panels-Boilerplate/tree/master/src/com.undavide.JSX2CEP" target="_blank">here</a>. As follows some code highlights to demo the mechanism.
  </p>
  
  <h3>
    JS
  </h3>
  
  <p>
    In the JS side there&#8217;s a simple EventListener with a callback as usual.
  </p>
  
  <pre class="lang:js decode:true">var csInterface = new CSInterface();
csInterface.addEventListener("My Custom Event", function(evt) {
    console.log('Data from the JSX payload: ' + evt.data);
});</pre>
  
  <h3>
    JSX
  </h3>
  
  <p>
    In the JSX you create an ExternalObject instance: <code>"lib:\PlugPlugExternalObject"</code> is the only parameter to pass. If there&#8217;s no error (the library exists and it&#8217;s available) you create a new <code>CSXSEvent</code> setting the type (which is referenced in the EventListener &#8211; the string must be the same, in the example &#8220;My Custom Event&#8221;) and a data property which is the payload that the HTML Panel is going to receive.
  </p>
  
  <pre class="lang:js decode:true ">try {
    var xLib = new ExternalObject("lib:\PlugPlugExternalObject");
} catch (e) {
    alert(e);
}

if (xLib) {
    var eventObj = new CSXSEvent(); 
    eventObj.type = "My Custom Event";
    eventObj.data = "some payload data...";
    eventObj.dispatch();
}</pre>
  
  <p>
    <a href="https://github.com/undavide/PS-Panels-Boilerplate/tree/master/src/com.undavide.JSX2CEP" target="_blank"><img class="alignleft wp-image-2679 size-medium" src="http://localhost:8888/wp-content/uploads/2014/07/JSX2CEP-panel-191x300.png" alt="JSX2CEP-panel" width="191" height="300" srcset="http://localhost:8888/wp-content/uploads/2014/07/JSX2CEP-panel-191x300.png 191w, http://localhost:8888/wp-content/uploads/2014/07/JSX2CEP-panel-150x235.png 150w, http://localhost:8888/wp-content/uploads/2014/07/JSX2CEP-panel.png 312w" sizes="(max-width: 191px) 100vw, 191px" /></a>In the <a title="ExtendScript to CEP Communication" href="https://github.com/undavide/PS-Panels-Boilerplate/tree/master/src/com.undavide.JSX2CEP" target="_blank">GitHub panel</a> things are slightly more elaborate: I&#8217;ve set a JSX function triggered by a button &#8211; which sends Custom Events spaced 1 second each. The Events&#8217; payloads are then logged in a textarea.
  </p>
  
  <p>
    I&#8217;m facing two issues, though. First, I would expect that the log messages come time spaced (first log, 1&#8243; wait, second log, 1&#8243; wait, third log, 1&#8243; wait, etc), while the get to the textarea all together.
  </p>
  
  <p>
    Second, the [Return Message&#8230;] which comes as the result value passed to the evalScript() callback appears as the first, and not last, log.
  </p>
  
  <p>
    As far as I understand, the Events are kept on hold until the whole JSX routine is done and the evalScript() callback executed &#8211; whether this is a problem of Event dispatching (the Photoshop / ExtendScript side) or Event listening (the JS / HTML Panel side) I can&#8217;t really say.
  </p>
  
  <h2>
    Bonus
  </h2>
  
  <p>
    A Twitter conversation withthe digital artist <a href="http://hundredsofsparrows.com" target="_blank">Sergey Kritskiy</a> led me to experiment whether the Panel is able to listen for CSXSEvents dispatched only from its own JSX files (the ones in its scope), or not. In other words, can an (open) HTML Panel fire a callback in response to a Custom Event of the right type coming from whatever Script might happen to run in Photoshop in that moment?
  </p>
  
  <p>
    The answer is yes: in fact if you keep the JSX to CEP Communication panel open and run in the ExtendScript ToolKit the same chunk of code provided in the JSX section above in this very post, the Panel shows &#8220;some payload data&#8230;&#8221; in its textarea.
  </p>
  
  <p>
    &nbsp;
  </p>
</div>

<!-- Share-Widget Button BEGIN --><a href="javascript:void(0);" myshare\_id="mys\_shareit" myshare\_url="http://localhost:8888/2014/07/html-panels-tips-11-externalobject-cep-events/" myshare\_title="HTML Panels Tips: #11 CEP Events (ExternalObject)" rel="nofollow" onclick=" return false;" style="text-decoration:none; color:#000000; font-size:11px; line-height:20px;"> 

<img src="http://localhost:8888/wp-content/plugins/share-widget/img/share-button-white-small.png" height="20" alt="Share" style="border:0" /> </a> <!-- Share-Widget Button END -->