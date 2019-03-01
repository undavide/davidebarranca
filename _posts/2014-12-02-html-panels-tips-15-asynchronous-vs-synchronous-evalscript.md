---
id: 2743
title: 'HTML Panels Tips #15 Asynchronous vs. Synchronous'
date: 2014-12-02T17:37:31+01:00
author: Davide Barranca
excerpt: "Today's tip focuses on the Asynchronous nature of CSInterface.evalScript() calls, and on ways to make it work in a synchronous fashion."
layout: post
guid: http://www.davidebarranca.com/?p=2743
permalink: /2014/12/html-panels-tips-15-asynchronous-vs-synchronous-evalscript/
seo_post_level_layout:
  - ""
link_url_field:
  - ""
seo_post_meta_description:
  - 'In HTML Panels, CSInterface.evalScript calls are asynchronous - there are few ways to mimic synchronous behaviours, which I will be showing.'
image: /wp-content/uploads/2014/12/featured.png
categories:
  - Coding
  - HTML Panels
tags:
  - asynchronous
  - CEP 5.2
  - synchronous
---
<div class="pf-content">
  <p>
    Today&#8217;s tip focuses on the Asynchronous nature of <code>CSInterface.evalScript()</code> calls, and on ways to make it work in a synchronous fashion.<!--more-->
  </p>
  
  <h2>
    Synchronous vs. Asynchronous
  </h2>
  
  <p>
    Quoting <a title="Synchronous vs Asynchronous" href="http://stackoverflow.com/questions/748175/asynchronous-vs-synchronous-execution-what-does-it-really-mean" target="_blank">StackOverflow</a>:
  </p>
  
  <blockquote>
    <p>
      When you execute something synchronously, you wait for it to finish before moving on to another task.<br /> When you execute something asynchronously, you can move on to another task before it finishes.
    </p>
  </blockquote>
  
  <p>
    Or visually:
  </p>
  
  <p>
    <img class="aligncenter size-full wp-image-2751" src="http://localhost:8888/wp-content/uploads/2014/12/sync.png" alt="Synchronous" width="584" height="125" srcset="http://localhost:8888/wp-content/uploads/2014/12/sync.png 584w, http://localhost:8888/wp-content/uploads/2014/12/sync-150x32.png 150w, http://localhost:8888/wp-content/uploads/2014/12/sync-300x64.png 300w" sizes="(max-width: 584px) 100vw, 584px" />
  </p>
  
  <p>
    <img class="aligncenter size-full wp-image-2749" src="http://localhost:8888/wp-content/uploads/2014/12/async.png" alt="Asynchronous" width="584" height="125" srcset="http://localhost:8888/wp-content/uploads/2014/12/async.png 584w, http://localhost:8888/wp-content/uploads/2014/12/async-150x32.png 150w, http://localhost:8888/wp-content/uploads/2014/12/async-300x64.png 300w" sizes="(max-width: 584px) 100vw, 584px" />
  </p>
  
  <p>
    Back in Flash days <code>evalScript</code> was synchronous, now in HTML land is asynchronous:
  </p>
  
  <pre class="lang:js decode:true ">CSXSInterface.instance.evalScript('jsxFunction'); // Sync
csInterface().evalScript('jsxFunction'); // Async</pre>
  
  <h2>
    Async example
  </h2>
  
  <p>
    Remember that evalScript function takes two params:
  </p>
  
  <ul>
    <li>
      The ExtendScript code to run.
    </li>
    <li>
      A callback function, which in turn takes as a param the returned value from the ExtendScript call.
    </li>
  </ul>
  
  <p>
    The default behavior is as follows.
  </p>
  
  <pre class="nums:true lang:js mark:4,8 decode:true">var csInterface = new CSInterface();
var numberOfDocuments = undefined;

csInterface.evalScript('app.documents.length', function(result) {
  numberOfDocuments = result;
});

alert(numberOfDocuments); // undefined</pre>
  
  <p>
    In the above example the <code>app.documents.length</code> statement is executed and directly returned &#8211; the number of currently opened documents in Photoshop is passed to the callback, which assignes it to the <code>numberOfDocuments</code> variable.
  </p>
  
  <p>
    The alert says &#8220;undefined&#8221; because of the <strong>asynchronous</strong> nature of evalScript: line #4 executes, then the interpreter goes ahead to line #8 not waiting for the ExtendScript to return (in this example, a single line of code, but in real world a long chain of functions that trigger usually time consuming Photoshop operations) so when <code>alert(numberOfDocuments)</code> is called, <code>numberOfDocuments</code> is still <em>undefined</em>.
  </p>
  
  <p>
    So to speak, it is like asking your partner to do the shopping then, as soon as (s)he goes out, open the fridge and say &#8220;rats, it&#8217;s empty!&#8221;
  </p>
  
  <h2>
    Sync workarounds
  </h2>
  
  <p>
    There are few different ways to mimic a synchronous behavior
  </p>
  
  <h3>
    1. Set a Timeout
  </h3>
  
  <p>
    The easiest way is to just wait for your partner to get back from the store:
  </p>
  
  <pre class="lang:js decode:true">var csInterface = new CSInterface();
var numberOfDocuments = undefined;

csInterface.evalScript('app.documents.length', function (result) {
    numberOfDocuments = result;
});
setTimeout( alert(numberOfDocuments), 500);</pre>
  
  <p>
    Although, it&#8217;s not ideal. You might not know in advance how long to wait &#8211; it depends of the kind of ExtendScript task you&#8217;re executing.
  </p>
  
  <h3>
    2. Nest code in Callbacks
  </h3>
  
  <p>
    If the alert is moved within the callback it works properly:
  </p>
  
  <pre class="lang:js decode:true ">var csInterface = new CSInterface();
var numberOfDocuments = undefined;

csInterface.evalScript('app.documents.length', function (result) {
    numberOfDocuments = result;
    alert(numberOfDocuments);
});

</pre>
  
  <p>
    Of course this can lead to the so-called <em>callback hell</em> &#8211; nesting evalScript calls one inside another, like:
  </p>
  
  <pre class="lang:js decode:true ">csInterface.evalScript('/* some code */', function(result) {
    csInterface.evalScript('/* some other code */', function (result) {
        // etc...
    })
});

</pre>
  
  <p>
    Yet if you&#8217;ve not fancy needs, it works.
  </p>
  
  <h3>
    3. Event driven callbacks
  </h3>
  
  <p>
    This method is supported only from CEP 5.2 onwards.<br /> You can implement custom Events via <em>PlugPlugExternalObject</em>, that are dispatched by ExtendScript and listened from JS.<br /> I&#8217;ve put the ExtendScript code in its own JSX file:
  </p>
  
  <pre class="lang:js decode:true">function getNumberOfDocuments () {
    try {
        var xLib = new ExternalObject("lib:\PlugPlugExternalObject");
    } catch(e) {
    alert(e.message);
    return false;
    }

    var eventObj = new CSXSEvent();
    eventObj.type = "alertDocumentsNumber";

    // data must be a string otherwise PS (at least CC 2014.1.2) crashes
    eventObj.data = app.documents.length.toString();

    eventObj.dispatch();
    xLib.unload();
    return true;
}</pre>
  
  <p>
    Then in the JS you set up a listener, which is the one who fires the alert();
  </p>
  
  <pre class="lang:js decode:true ">var csInterface = new CSInterface();

csInterface.addEventListener("alertDocumentsNumber", function(event) {
    alert("The number of open documents is: " + event.data);
});

csInterface.evalScript('getNumberOfDocuments()', function(result) {
    if (result == false) { alert ("There's been a problem."); }
});
</pre>
  
  <p>
    Mind you, as far as other devs such as <a title="Kris Coppieters" href="http://www.rorohiko.com" target="_blank">Kris Coppieters</a> have supposed, &#8220;<em>even if you manage to make multiple JSX calls from different parts of your JS, probably you can&#8217;t run multiple ExtendScripts concurrently</em>&#8220;.
  </p>
  
  <h2>
    Acknowledgements
  </h2>
  
  <p>
    I&#8217;d like to thank Zihong Chen and Ten from Adobe and the others who&#8217;ve helped shedding some light upon this topic in the <a title="Updating a variable from JSX" href="https://forums.adobe.com/thread/1360552" target="_blank">forums</a>.
  </p>
</div>

<!-- Share-Widget Button BEGIN --><a href="javascript:void(0);" myshare\_id="mys\_shareit" myshare\_url="http://localhost:8888/2014/12/html-panels-tips-15-asynchronous-vs-synchronous-evalscript/" myshare\_title="HTML Panels Tips #15 Asynchronous vs. Synchronous" rel="nofollow" onclick=" return false;" style="text-decoration:none; color:#000000; font-size:11px; line-height:20px;"> 

<img src="http://localhost:8888/wp-content/plugins/share-widget/img/share-button-white-small.png" height="20" alt="Share" style="border:0" /> </a> <!-- Share-Widget Button END -->