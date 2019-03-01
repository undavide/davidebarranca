---
id: 2456
title: 'HTML Panels Tips: #4 passing Objects from HTML to JSX'
date: 2014-01-31T23:53:10+01:00
author: Davide Barranca
excerpt: How to pass complex Objects from the HTML Panel to the Photoshop JSX file.
layout: post
guid: http://www.davidebarranca.com/?p=2456
permalink: /2014/01/html-panels-tips-4-passing-objects-from-html-to-jsx/
standard_seo_post_level_layout:
  - ""
standard_link_url_field:
  - ""
standard_seo_post_meta_description:
  - How to pass complex Objects from an HTML Panel to an ExtendScript function in a JSX file (Photoshop and other supported CC applications)
image: /wp-content/uploads/2014/01/Object.png
categories:
  - Coding
  - HTML Panels
tags:
  - html panels
  - objects
  - tip
---
<div class="pf-content">
  <p>
    How to pass complex Objects from the HTML Panel to a Photoshop&#8217;s ExtendScript function in a JSX file.
  </p>
  
  <p>
    <!--more-->
  </p>
  
  <p>
    You may want to pack different information from the HTML Panel (for instance all the values in the GUI) in a single Object, and pass just it as a parameter to a JSX function &#8211; that the Photoshop interpreter will eventually execute.
  </p>
  
  <p>
    For primitive, multiple parameters this is easily done via <code>CSInterface.evalScript()</code>, in the main.js:
  </p>
  
  <pre class="lang:js decode:true">var csInterface = new CSInterface();
csInterface.evalScript('sayHello("Ciao", 2014)');</pre>
  
  <p>
    &#8230;and providing a corresponding function in the JSX file:
  </p>
  
  <pre class="lang:js decode:true">function sayHello(str, num) {
   alert("Photoshop says " + str + "! Have a great " + num);
}</pre>
  
  <p>
    How do you deal with Objects? <strong>Stringify them as JSON objects first</strong>. No need to parse them afterwards.
  </p>
  
  <h2>
    Example
  </h2>
  
  <p>
    I use as always the <a title="CC Extensibility Helpers" href="http://davidderaedt.github.io/ccext-website/" target="_blank">boilerplate code</a> by <a title="David Deraedt on Twitter" href="https://twitter.com/davidderaedt" target="_blank">David Deraedt</a>.
  </p>
  
  <h3>
    HTML
  </h3>
  
  <p>
    This is a minimal <code>index.html</code> with just a button.
  </p>
  
  <pre class="lang:xhtml mark:12 decode:true">&lt;!doctype html&gt;
&lt;html&gt;
&lt;head&gt;
&lt;meta charset="utf-8"&gt;
&lt;link rel="stylesheet" href="css/styles.css"/&gt;
&lt;link id="hostStyle" rel="stylesheet" href="css/theme.css"/&gt;
&lt;title&gt;&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;div id="content"&gt;        
        &lt;h1&gt;Object communication&lt;/h1&gt;
        &lt;button id="btnSend"&gt;HTML -&gt; JSX&lt;/button&gt;
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
    Here I&#8217;ve hardcoded an Object literal (feel free to fill it with actual values from the HTML GUI), which is <strong>stringified as a JSON object</strong> in order to pass it as a parameter:
  </p>
  
  <pre class="lang:default mark:6-15,17 decode:true ">(function () {
    'use strict';
    var csInterface = new CSInterface();
    function init() {
        themeManager.init();
        var obj = {
            str: "Love your job!",
            num: 38,
            today: new Date(),
            nestedObj: {
                nestedStr: "Even if nothing works",
                nestedNum: 8,
                nestedDate: new Date()
            }
        }     
        $("#btnSend").click(function () {
            csInterface.evalScript( 'parseObj(' + JSON.stringify(obj) + ')' );
        });
    }        
    init();
}());</pre>
  
  <h3>
    JSX
  </h3>
  
  <p>
    In the ExtendScript file I&#8217;ve provided a logging function to actually verify that the Object is interpreted correctly.
  </p>
  
  <pre class="lang:default decode:true">function parseObj(o) {
  var str = "";
    for (prop in o) {
      str += prop + " [" + typeof o[prop] + "]: " + o[prop] + ".\n";
    }
  alert(str);
}</pre>
  
  <p>
    <img class="alignleft size-full wp-image-2459" alt="Object" src="http://localhost:8888/wp-content/uploads/2014/01/Object.png" width="300" height="132" srcset="http://localhost:8888/wp-content/uploads/2014/01/Object.png 300w, http://localhost:8888/wp-content/uploads/2014/01/Object-150x66.png 150w" sizes="(max-width: 300px) 100vw, 300px" />Mind you: I&#8217;ve experimented importing a JSON library in the ExtendScript code (which doesn&#8217;t natively support it) and tried to <code>var obj=JSON.parse(o);</code> to no avail. Apparently the code is OK as it is, without it.
  </p>
</div>

<!-- Share-Widget Button BEGIN --><a href="javascript:void(0);" myshare\_id="mys\_shareit" myshare\_url="http://localhost:8888/2014/01/html-panels-tips-4-passing-objects-from-html-to-jsx/" myshare\_title="HTML Panels Tips: #4 passing Objects from HTML to JSX" rel="nofollow" onclick=" return false;" style="text-decoration:none; color:#000000; font-size:11px; line-height:20px;"> 

<img src="http://localhost:8888/wp-content/plugins/share-widget/img/share-button-white-small.png" height="20" alt="Share" style="border:0" /> </a> <!-- Share-Widget Button END -->