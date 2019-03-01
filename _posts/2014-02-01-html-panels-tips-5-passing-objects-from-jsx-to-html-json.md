---
id: 2466
title: 'HTML Panels Tips: #5 passing Objects from JSX to HTML'
date: 2014-02-01T14:03:01+01:00
author: Davide Barranca
excerpt: This tip covers how to pass Objects from Photoshop (the JSX file) to the HTML Panel.
layout: post
guid: http://www.davidebarranca.com/?p=2466
permalink: /2014/02/html-panels-tips-5-passing-objects-from-jsx-to-html-json/
standard_seo_post_level_layout:
  - ""
standard_link_url_field:
  - ""
standard_seo_post_meta_description:
  - How to pass an Object from Photoshop JSX Extendscript code back to the HTML panel via JSON
image: /wp-content/uploads/2014/02/Console.png
categories:
  - Coding
  - HTML Panels
tags:
  - html panels
  - JSON
  - tip
---
<div class="pf-content">
  <p>
    This tip covers how to pass Objects from Photoshop (the JSX file) to the HTML Panel.
  </p>
  
  <p>
    <!--more-->
  </p>
  
  <p>
    Panels technology doesn&#8217;t allow (for the moment) to communicate from the JSX to the HTML except via function return (see <a title="HTML Panels Tips: #3 Get data from JSX and send it to HTML" href="http://localhost:8888/2014/01/html-panels-tips-3-get-data-from-jsx-send-it-to-html/" target="_blank">Tip #3</a>).
  </p>
  
  <p>
    In order to successfully pass Objects, you need to include <a title="Douglas Crockford JSON on GitHub" href="https://github.com/douglascrockford/JSON-js/blob/master/json2.js" target="_blank">json2.js</a> in the JSX and <strong>stringify the Object as a JSON Object</strong>. Then, in the HTML Panel you must <strong>parse the JSON Object</strong> in the callback.
  </p>
  
  <h2>
    Example
  </h2>
  
  <p>
    This panel has just one button: it triggers a function in the JSX that returns an Object, which is then parsed and logged in the console. In order to follow the demo code, you&#8217;d better know how to debug a panel (<a title="HTML Panels Tips: #1 Debugging" href="http://localhost:8888/2014/01/html-panels-tips-1-debugging/" target="_blank">Tip #1</a>), evaluate external JSX  code (<a title="HTML Panels Tips: #2 Including multiple JSX" href="http://localhost:8888/2014/01/html-panels-tips-2-including-multiple-jsx/" target="_blank">Tip #2</a>) and have a good grasp on how primitive data is passed from JSX to HTML (<a title="HTML Panels Tips: #3 Get data from JSX and send it to HTML" href="http://localhost:8888/2014/01/html-panels-tips-3-get-data-from-jsx-send-it-to-html/" target="_blank">Tip #3</a>).
  </p>
  
  <h3>
    HTML
  </h3>
  
  <p>
    The usual bare code stripped from <a title="CC Extensibility Helpers" href="http://davidderaedt.github.io/ccext-website/" target="_blank">boilerplate</a> by <a title="David Deraedt on Twitter" href="https://twitter.com/davidderaedt" target="_blank">David Deraedt</a>.
  </p>
  
  <pre class="lang:xhtml decode:true">&lt;!doctype html&gt;
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
        &lt;button id="btnSend"&gt;JSX -&gt; HTML&lt;/button&gt;
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
    In the Javascript it is first declared the <code>loadJSX()</code> function (hardwired to look in the jsx/ folder), called in the <code>init()</code> in order to evaluate in the Photoshop context the <code>json2.js</code> library. Mind you, ExtendScript doesn&#8217;t support JSON natively so the original library must be included. I&#8217;ve used the commented version by Douglas Crockford, beware that minification in ExtendScript <a title="Testing minified JS Libraries in ExtendScript" href="http://localhost:8888/2013/08/testing-minified-js-libraries-in-extendscript/" target="_blank">can be tricky</a>.
  </p>
  
  <p>
    I&#8217;ve provided to the <code>CSInterface.evalScript()</code> a callback as an anonymous function, which parses the result as a JSON Object (see next section).
  </p>
  
  <pre class="lang:js decode:true">(function () {
    'use strict';
    var csInterface = new CSInterface();

    function loadJSX (fileName) {
        var extensionRoot = csInterface.getSystemPath(SystemPath.EXTENSION) + "/jsx/";
        csInterface.evalScript('$.evalFile("' + extensionRoot + fileName + '")');
    }

    function init() {
        themeManager.init();
        loadJSX("json2.js");

        $("#btnSend").click(function () {
            csInterface.evalScript( 'sendObjToHTML()', function(result) {
                var o = JSON.parse(result);
                var o = result;
                var str = "";
                for (var prop in o) {
                    str += prop + " [" + typeof o[prop] + "]: " + o[prop] + ".\n";
                }
                console.log(str);
            } );
        });
    }    

    init();
}());</pre>
  
  <h3>
    JSX
  </h3>
  
  <p>
    Here the <code>sendObjToHTML()</code> is declared: it first builds an Object literal with arbitrary data (you&#8217;ll fill it with useful, actual stuff: Photoshop active Document&#8217;s width, height, ecc.); then the Object is returned as a JSON (stringified) Object. This very Object is the result argument in the anonymous callback in the main.js above.
  </p>
  
  <pre class="lang:js decode:true">function sendObjToHTML () {
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
    return JSON.stringify(obj);
}</pre>
  
  <p>
    When the panel is open in Photoshop &#8211; provided you&#8217;ve set things according to <a title="HTML Panels Tips: #1 Debugging" href="http://localhost:8888/2014/01/html-panels-tips-1-debugging/" target="_blank">Tip #1</a> &#8211; open Chrome (or Safari, or Firefox) and go to localhost:8088 or whatever port you&#8217;ve opened in the .debug file.
  </p>
</div>

<!-- Share-Widget Button BEGIN --><a href="javascript:void(0);" myshare\_id="mys\_shareit" myshare\_url="http://localhost:8888/2014/02/html-panels-tips-5-passing-objects-from-jsx-to-html-json/" myshare\_title="HTML Panels Tips: #5 passing Objects from JSX to HTML" rel="nofollow" onclick=" return false;" style="text-decoration:none; color:#000000; font-size:11px; line-height:20px;"> 

<img src="http://localhost:8888/wp-content/plugins/share-widget/img/share-button-white-small.png" height="20" alt="Share" style="border:0" /> </a> <!-- Share-Widget Button END -->