---
id: 2874
title: 'HTML Panel Tips #18: Photoshop JSON Callback'
date: 2015-09-03T19:37:22+01:00
author: Davide Barranca
excerpt: |
  CC2015 previews a new Photoshop Event listening system and deprecates the "com.adobe.PhotoshopCallback", due to a bug that makes all the extension receiving the event - that is: if two panels are registered for the event "make", then each panel sees the other panel's "make" event, and must ignore it.
  
  The solution implemented marks a break in retro-compatibility and is going to be the only one accepted from Photoshop CC2016 (version 17.x) onwards. Let's have a look at what is that all about.
layout: post
guid: http://www.davidebarranca.com/?p=2874
permalink: /2015/09/html-panel-tips-18-photoshop-json-callback/
seo_post_level_layout:
  - ""
link_url_field:
  - ""
seo_post_meta_description:
  - From Photoshop CC2015, com.adobe.PhotoshopCallback event is deprecated, replaced by com.adobe.PhotoshopJSONCallback. An example is provided.
image: /wp-content/uploads/2015/09/PhotoshopJSONCallback.png
categories:
  - Coding
  - HTML Panels
tags:
  - Events
  - PhotoshopCallback
  - PhotoshopJSONCallback
---
<div class="pf-content">
  <p>
    CC2015 previews a new Photoshop Event listening system and deprecates the <code>"com.adobe.PhotoshopCallback"</code>, due to a bug that makes all the extension receiving the event &#8211; that is: if two panels are registered for the event &#8220;make&#8221;, then each panel sees the other panel&#8217;s &#8220;make&#8221; event, and must ignore it.
  </p>
  
  <p>
    The solution implemented marks a break in retro-compatibility and is going to be the only one accepted from Photoshop CC2016 (version 17.x) onwards. Let&#8217;s have a look at what is that all about.<!--more-->
  </p>
  
  <p>
    Please refer to <a href="http://localhost:8888/2014/02/html-panels-tips-7-events-photoshopregisterevent-photoshopcallback/">HTML Panels Tips: #7 Photoshop Events, Take 1</a> to review how PS events listening works. Now, there are three different changes you must be aware of:
  </p>
  
  <ol>
    <li>
      <code>"com.adobe.PhotoshopJSONCallback"</code> is the replacement event &#8211; no more <code>"com.adobe.PhotoshopCallback"</code>.
    </li>
    <li>
      The ExtensionID must be <strong>appended as a suffix</strong>.
    </li>
  </ol>
  
  <p>
    So what used to be:
  </p>
  
  <pre class="lang:js decode:true ">var csInterface = new CSInterface();
csInterface.addEventListener("PhotoshopCallback", PSCallback);</pre>
  
  <p>
    now becomes:
  </p>
  
  <pre class="lang:js decode:true">var csInterface = new CSInterface();
var extensionId =  csInterface.getExtensionID();

csInterface.addEventListener("com.adobe.PhotoshopJSONCallback" + extensionId, PhotoshopCallbackUnique);</pre>
  
  <ol start="3">
    <li>
      The data property of the CSEvent that the PhotoshopCallbackUnique receives is a <strong>JSON string</strong> hidden inside a string.
    </li>
  </ol>
  
  <pre class="lang:js decode:true">function PhotoshopCallbackUnique(csEvent) {
    console.log(csEvent);
     // ver1,{ "eventID":1298866208,
     //     "eventData":
     //        { "documentID":1566,
     //            "new":
     //             { "_obj":"document",
     //                 "depth":8,
     //                 "fill":
     //                    { "_enum":"fill",
     //                        "_value":"white"
     //                    },
     //                 "height":
     //                    { "_unit":"distanceUnit",
     //                        "_value":360
     //                    },
     //                 "mode":
     //                    { "_class":"RGBColorMode"
     //                    },
     //                 "pixelScaleFactor":1,
     //                 "profile":"sRGB IEC61966-2.1",
     //                 "resolution":
     //                    { "_unit":"densityUnit",
     //                        "_value":300
     //                    },
     //                 "width":
     //                    { "_unit":"distanceUnit",
     //                        "_value":504
     //                    }
     //                }
     //        }
     // }
}</pre>
  
  <p>
    As you see, there&#8217;s a <code>ver1,</code> that needs to be stripped. Also, as a bonus, extra information is provided as JSON (here a New Document&#8217;s been created).
  </p>
  
  <p>
    I&#8217;ve uploaded a <a href="https://github.com/undavide/PS-Panels-Boilerplate/tree/master/src/com.undavide.PhotoshopJSONCallback">demo extension</a> on my GitHub repo, which looks like:
  </p>
  
  <p>
    <a href="https://github.com/undavide/PS-Panels-Boilerplate/tree/master/src/com.undavide.PhotoshopJSONCallback"><img class="aligncenter size-medium wp-image-2876" src="http://localhost:8888/wp-content/uploads/2015/09/PhotoshopJSONCallback-289x300.png" alt="PhotoshopJSONCallback" width="289" height="300" srcset="http://localhost:8888/wp-content/uploads/2015/09/PhotoshopJSONCallback-289x300.png 289w, http://localhost:8888/wp-content/uploads/2015/09/PhotoshopJSONCallback-150x156.png 150w, http://localhost:8888/wp-content/uploads/2015/09/PhotoshopJSONCallback.png 624w" sizes="(max-width: 289px) 100vw, 289px" /></a>
  </p>
  
  <p>
    which basically let you switch on/off listeners to the corresponding events (&#8220;make&#8221;, &#8220;duplicate&#8221;, etc are the stringID). You can find the full code on GitHub, yet the relevant files are:
  </p>
  
  <h2>
    HTML
  </h2>
  
  <p>
    I&#8217;ve used Topcoat CSS (apparently not anymore maintained, but I love them), nothing too fancy.
  </p>
  
  <pre class="lang:xhtml decode:true">&lt;!doctype html&gt;
&lt;html&gt;
&lt;head&gt;
&lt;meta charset="utf-8"&gt;
&lt;link id="hostStyle" rel="stylesheet" href="css/theme.css"/&gt;
&lt;link id="theme" rel="stylesheet" href="css/light.css"/&gt;
&lt;title&gt;&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;
    
    &lt;div id="container" &gt;
        &lt;h3 class="center"&gt;Photoshop Events - JSON Callback&lt;/h3&gt;
        &lt;h4 class="center"&gt;Listen for events:&lt;/h4&gt;
        
        &lt;div class="switchContainer"&gt;
            &lt;label class="topcoat-switch"&gt;
                &lt;input id="make" type="checkbox" class="topcoat-switch__input"&gt;
                &lt;div class="topcoat-switch__toggle"&gt;&lt;/div&gt;
            &lt;/label&gt;
            &lt;label class="switch-label"&gt;"make"&lt;/label&gt;
        &lt;/div&gt;

        &lt;div class="switchContainer"&gt;
            &lt;label class="topcoat-switch"&gt;
                &lt;input id="duplicate" type="checkbox" class="topcoat-switch__input"&gt;
                &lt;div class="topcoat-switch__toggle"&gt;&lt;/div&gt;
            &lt;/label&gt;
            &lt;label class="switch-label"&gt;"duplicate"&lt;/label&gt;
        &lt;/div&gt;

        &lt;div class="switchContainer"&gt;
            &lt;label class="topcoat-switch"&gt;
                &lt;input id="delete" type="checkbox" class="topcoat-switch__input"&gt;
                &lt;div class="topcoat-switch__toggle"&gt;&lt;/div&gt;
            &lt;/label&gt;
            &lt;label class="switch-label"&gt;"delete"&lt;/label&gt;
        &lt;/div&gt;

        &lt;div class="switchContainer"&gt;
            &lt;label class="topcoat-switch"&gt;
                &lt;input id="close" type="checkbox" class="topcoat-switch__input"&gt;
                &lt;div class="topcoat-switch__toggle"&gt;&lt;/div&gt;
            &lt;/label&gt;
            &lt;label class="switch-label"&gt;"close"&lt;/label&gt;
        &lt;/div&gt;

    &lt;/div&gt;

    &lt;script src="js/libs/CSInterface.js"&gt;&lt;/script&gt;
    &lt;script src="js/libs/jquery-2.0.2.min.js"&gt;&lt;/script&gt;
    &lt;script src="js/themeManager.js"&gt;&lt;/script&gt;
    &lt;script src="js/main.js"&gt;&lt;/script&gt;

&lt;/body&gt;
&lt;/html&gt;</pre>
  
  <h2>
    JS
  </h2>
  
  <p>
    The <code>init</code> function starts the <code>themeManager</code> and then activate <em>persistence</em> (if you need a refresh on the topic, have a look at this older <a href="http://localhost:8888/2014/02/html-panels-tips-9-persistence/">HTML Panel Tip</a>). Be careful with persistence, since if it&#8217;s active it will constantly cache the extension in the Chrome Dev Tools (i.e. you change code, reboot the panel and changes aren&#8217;t loaded &#8211; you need to reboot Photoshop. This happens on OSX at least; knowing it will save some head scratching time).
  </p>
  
  <p>
    Then you add a listener for the PhotoshopJSONCallback plus extensionID (line 24).
  </p>
  
  <p>
    On line 26 <code>PhotoshopCallbackUnique</code> strips the &#8220;ver1&#8221; string in the event.data, then parse the JSON and puts it back as an Object.
  </p>
  
  <p>
    Line 44, <code>toggleEventRegistering</code>: this is the unique switches onChange callback, which duty is to dispatch the <code>com.adobe.PhotoshopRegisterEvent</code> or <code>com.adobe.PhotoshopUnRegisterEvent</code>, corresponding to the on-the-fly-calculated typeID of the provided stringID.
  </p>
  
  <pre class="striped:true nums:true lang:js decode:true">/*jslint vars: true, plusplus: true, devel: true, nomen: true, regexp: true, indent: 4, maxerr: 50 */
/*global $, window, location, CSInterface, SystemPath, themeManager*/
line
(function () {
    'use strict';

    var csInterface = new CSInterface();
    var extensionId =  csInterface.getExtensionID();

    function PersistentOn() {
        var event = new CSEvent("com.adobe.PhotoshopPersistent", "APPLICATION");
        event.extensionId = extensionId;
        csInterface.dispatchEvent(event);
    }

    function init() {
        // Styling        
        themeManager.init();

        // Persistence
        PersistentOn();

        // Listener for PhotoshopJSONCallback
        csInterface.addEventListener("com.adobe.PhotoshopJSONCallback" + extensionId, PhotoshopCallbackUnique);

        function PhotoshopCallbackUnique(csEvent) {
            console.log(csEvent.toString());
            try {
                if (typeof csEvent.data === "string") {
                    var eventData = csEvent.data.replace("ver1,{", "{");
                    var eventDataObject = JSON.parse(eventData);
                    csEvent.data = eventDataObject;

                    console.log(csEvent);

                } else {
                    console.log("PhotoshopCallbackUnique expecting string for csEvent.data!");
                }
            } catch(e) {
                console.log("PhotoshopCallbackUnique catch: " + e);
            }
        }

        function toggleEventRegistering (eventStringID, isOn) {
            var event;
            if (isOn) {
                event = new CSEvent("com.adobe.PhotoshopRegisterEvent", "APPLICATION");
            } else {
                event = new CSEvent("com.adobe.PhotoshopUnRegisterEvent", "APPLICATION");
            }
            event.extensionId = extensionId;

            csInterface.evalScript("app.stringIDToTypeID('" + eventStringID + "')", function (typeID) {
                event.data = typeID;
                csInterface.dispatchEvent(event);
                console.log("Dispatched Event " + eventStringID, event);
            });
        }

        $('#make').change(function() {
            toggleEventRegistering(this.id, this.checked);
        });
        $('#duplicate').change(function() {
            toggleEventRegistering(this.id, this.checked);
        });
        $('#delete').change(function() {
            toggleEventRegistering(this.id, this.checked);
        });
        $('#close').change(function() {
            toggleEventRegistering(this.id, this.checked);
        });
    }
        
    init();

}());
    
</pre>
  
  <p>
    I hope this help clarifying how things work now. I repeat, both <code>PhotoshopCallback</code> <strong>and</strong> <code>PhotoshopJSONCallback</code> are usable in Photoshop CC2015 (the first is &#8220;just&#8221; deprecated), while from CC2016 onwards only the latter will work. I&#8217;d like to thanks Tom Ruark who pointed me to the Adobe&#8217;s original code, that I&#8217;ve refactored a bit for this blogpost.
  </p>
  
  <p>
    Also, I take the chance to tell you I&#8217;m working at a more comprehensive learning resource about Photoshop HTML Panels (ebook + video series), so stay tuned here. Ciao,
  </p>
  
  <p>
    -Davide
  </p>
</div>

<!-- Share-Widget Button BEGIN --><a href="javascript:void(0);" myshare\_id="mys\_shareit" myshare\_url="http://localhost:8888/2015/09/html-panel-tips-18-photoshop-json-callback/" myshare\_title="HTML Panel Tips #18: Photoshop JSON Callback" rel="nofollow" onclick=" return false;" style="text-decoration:none; color:#000000; font-size:11px; line-height:20px;"> 

<img src="http://localhost:8888/wp-content/plugins/share-widget/img/share-button-white-small.png" height="20" alt="Share" style="border:0" /> </a> <!-- Share-Widget Button END -->