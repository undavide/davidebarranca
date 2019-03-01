---
id: 2452
title: 'HTML Panels Tips: #3 Get data from JSX and send it to HTML'
date: 2014-01-31T16:21:30+01:00
author: Davide Barranca
excerpt: This is how Photoshop (i.e. the JSX) sends back data to the HTML Panel.
layout: post
guid: http://www.davidebarranca.com/?p=2452
permalink: /2014/01/html-panels-tips-3-get-data-from-jsx-send-it-to-html/
standard_seo_post_level_layout:
  - ""
standard_link_url_field:
  - ""
standard_seo_post_meta_description:
  - How to pass data from Photoshop CC (ExtendScript code in JSX) back to the HTML Panel.
image: /wp-content/uploads/2014/01/JSX_2_HTML.png
categories:
  - Coding
  - HTML Panels
tags:
  - html panels
  - jsx
  - tip
---
<div class="pf-content">
  <p>
    This is how Photoshop (i.e. the JSX) sends back data to the HTML Panel.
  </p>
  
  <p>
    <!--more-->
  </p>
  
  <p>
    The HTML Panel communicates with Photoshop via evalScript &#8211; this is in the <code>main.js</code>:
  </p>
  
  <pre class="lang:default decode:true">var csInterface = new CSInterface();
csInterface.evalScript('jsxFun()');</pre>
  
  <p>
    In the JSX there must be some:
  </p>
  
  <pre class="lang:default decode:true">function jsxFun() {
    // ...
}</pre>
  
  <p>
    In order to enable double-way communication, <strong>provide a callback</strong> in the evalScript function:
  </p>
  
  <pre class="lang:default mark:2 decode:true">var csInterface = new CSInterface();
csInterface.evalScript('jsxFun()', callback); 
// callback is a function defined somewhere (or anonymous)</pre>
  
  <h2>
    Example
  </h2>
  
  <p>
    <img class="alignleft size-full wp-image-2453" alt="JSX_2_HTML" src="http://localhost:8888/wp-content/uploads/2014/01/JSX_2_HTML.png" width="191" height="178" srcset="http://localhost:8888/wp-content/uploads/2014/01/JSX_2_HTML.png 191w, http://localhost:8888/wp-content/uploads/2014/01/JSX_2_HTML-150x139.png 150w" sizes="(max-width: 191px) 100vw, 191px" />A very trivial example for a panel with one button that retrieves and displays in the panel the Photoshop active document&#8217;s name (forgive my total lack of design for this panel &#8211; it&#8217;s just to show you the bare mechanism) is as follows.
  </p>
  
  <h3>
    HTML
  </h3>
  
  <p>
    I define a button with <code>id="btnDocName"</code>, and an <code>h3</code> tag which will contain the Doc&#8217;s name.
  </p>
  
  <pre class="lang:default mark:14 decode:true">&lt;!doctype html&gt;
&lt;html&gt;
&lt;head&gt;
&lt;meta charset="utf-8"&gt;
&lt;link rel="stylesheet" href="css/styles.css"/&gt;
&lt;link id="hostStyle" rel="stylesheet" href="css/theme.css"/&gt;
&lt;title&gt;&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;

    &lt;div id="content"&gt;        
        &lt;h1&gt;JSX -&gt; HTML&lt;/h1&gt;
        &lt;h3 id="docName"&gt;&nbsp;&lt;/h3&gt;
        &lt;button id="btnDocName"&gt;Retrieve Document Name&lt;/button&gt;
    &lt;/div&gt;

    &lt;script src="js/libs/CSInterface-4.0.0.js"&gt;&lt;/script&gt;
    &lt;script src="js/libs/jquery-2.0.2.min.js"&gt;&lt;/script&gt;
    &lt;script src="js/themeManager.js"&gt;&lt;/script&gt;
    &lt;script src="js/main.js"&gt;&lt;/script&gt;

&lt;/body&gt;
&lt;/html&gt;</pre>
  
  <h3>
     Main.js
  </h3>
  
  <p>
    Besides themeManager (<a title="CC Extensibility Helpers" href="http://davidderaedt.github.io/ccext-website/" target="_blank">boilerplate code</a> is by the great <a title="David Deraedt on Twitter" href="https://twitter.com/davidderaedt" target="_blank">David Deraedt</a>) you see I evaluate (i.e. call) a <code>getDocName()</code> function (which belongs to the JSX) &#8216;ve used an anonymous function to define a callback on the fly. The result argument is just the return value in the JSX, and it&#8217;s used to overwrite the h3 tag content in the Panel.
  </p>
  
  <pre class="lang:default mark:7-8 decode:true crayon-selected">(function () {
    'use strict';
    var csInterface = new CSInterface();
    function init() {
        themeManager.init();
        $("#btnDocName").click(function () {
            csInterface.evalScript('getDocName()', function(result) {
                document.getElementById("docName").innerHTML= result;
            });
        });
    }
    init();
}());</pre>
  
  <h3>
    JSX
  </h3>
  
  <p>
    Contains the getDocName() function and returns the active document name as a String (the <code>toString()</code> shouldn&#8217;t be mandatory):
  </p>
  
  <pre class="lang:default decode:true">function getDocName(){
    return app.documents.length ? app.activeDocument.name : "No docs open!";
}</pre>
  
  <p>
    So as a result when you click the button on the HTML Panel, it evaluates the function in the JSX, which in turn passes its return value to the callback.
  </p>
</div>

<!-- Share-Widget Button BEGIN --><a href="javascript:void(0);" myshare\_id="mys\_shareit" myshare\_url="http://localhost:8888/2014/01/html-panels-tips-3-get-data-from-jsx-send-it-to-html/" myshare\_title="HTML Panels Tips: #3 Get data from JSX and send it to HTML" rel="nofollow" onclick=" return false;" style="text-decoration:none; color:#000000; font-size:11px; line-height:20px;"> 

<img src="http://localhost:8888/wp-content/plugins/share-widget/img/share-button-white-small.png" height="20" alt="Share" style="border:0" /> </a> <!-- Share-Widget Button END -->