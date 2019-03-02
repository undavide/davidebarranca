---
title: " HTML Panel Tips #18: Photoshop JSON Callback\t\t"
tags:
  - Events
  - PhotoshopCallback
  - PhotoshopJSONCallback
url: 2874.html
id: 2874
category:
  - Coding
  - HTML Panels
date: 2015-09-03 19:37:22
---

CC2015 previews a new Photoshop Event listening system and deprecates the `"com.adobe.PhotoshopCallback"`, due to a bug that makes all the extension receiving the event - that is: if two panels are registered for the event "make", then each panel sees the other panel's "make" event, and must ignore it. The solution implemented marks a break in retro-compatibility and is going to be the only one accepted from Photoshop CC2016 (version 17.x) onwards. Let's have a look at what is that all about. Please refer to [HTML Panels Tips: #7 Photoshop Events, Take 1](http://localhost:8888/2014/02/html-panels-tips-7-events-photoshopregisterevent-photoshopcallback/) to review how PS events listening works. Now, there are three different changes you must be aware of:

1.  `"com.adobe.PhotoshopJSONCallback"` is the replacement event - no more `"com.adobe.PhotoshopCallback"`.
2.  The ExtensionID must be **appended as a suffix**.

So what used to be:

var csInterface = new CSInterface();
csInterface.addEventListener("PhotoshopCallback", PSCallback);

now becomes:

var csInterface = new CSInterface();
var extensionId =  csInterface.getExtensionID();

csInterface.addEventListener("com.adobe.PhotoshopJSONCallback" + extensionId, PhotoshopCallbackUnique);

3.  The data property of the CSEvent that the PhotoshopCallbackUnique receives is a **JSON string** hidden inside a string.

function PhotoshopCallbackUnique(csEvent) {
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
}

As you see, there's a `ver1,` that needs to be stripped. Also, as a bonus, extra information is provided as JSON (here a New Document's been created). I've uploaded a [demo extension](https://github.com/undavide/PS-Panels-Boilerplate/tree/master/src/com.undavide.PhotoshopJSONCallback) on my GitHub repo, which looks like: [![PhotoshopJSONCallback](http://localhost:8888/wp-content/uploads/2015/09/PhotoshopJSONCallback-289x300.png)](https://github.com/undavide/PS-Panels-Boilerplate/tree/master/src/com.undavide.PhotoshopJSONCallback) which basically let you switch on/off listeners to the corresponding events ("make", "duplicate", etc are the stringID). You can find the full code on GitHub, yet the relevant files are:

HTML
----

I've used Topcoat CSS (apparently not anymore maintained, but I love them), nothing too fancy.

<!doctype html>
<html>
<head>
<meta charset="utf-8">
<link id="hostStyle" rel="stylesheet" href="css/theme.css"/>
<link id="theme" rel="stylesheet" href="css/light.css"/>
<title></title>
</head>
<body>
    
    <div id="container" >
        <h3 class="center">Photoshop Events - JSON Callback</h3>
        <h4 class="center">Listen for events:</h4>
        
        <div class="switchContainer">
            <label class="topcoat-switch">
                <input id="make" type="checkbox" class="topcoat-switch__input">
                <div class="topcoat-switch__toggle"></div>
            </label>
            <label class="switch-label">"make"</label>
        </div>

        <div class="switchContainer">
            <label class="topcoat-switch">
                <input id="duplicate" type="checkbox" class="topcoat-switch__input">
                <div class="topcoat-switch__toggle"></div>
            </label>
            <label class="switch-label">"duplicate"</label>
        </div>

        <div class="switchContainer">
            <label class="topcoat-switch">
                <input id="delete" type="checkbox" class="topcoat-switch__input">
                <div class="topcoat-switch__toggle"></div>
            </label>
            <label class="switch-label">"delete"</label>
        </div>

        <div class="switchContainer">
            <label class="topcoat-switch">
                <input id="close" type="checkbox" class="topcoat-switch__input">
                <div class="topcoat-switch__toggle"></div>
            </label>
            <label class="switch-label">"close"</label>
        </div>

    </div>

    <script src="js/libs/CSInterface.js"></script>
    <script src="js/libs/jquery-2.0.2.min.js"></script>
    <script src="js/themeManager.js"></script>
    <script src="js/main.js"></script>

</body>
</html>

JS
--

The `init` function starts the `themeManager` and then activate _persistence_ (if you need a refresh on the topic, have a look at this older [HTML Panel Tip](http://localhost:8888/2014/02/html-panels-tips-9-persistence/)). Be careful with persistence, since if it's active it will constantly cache the extension in the Chrome Dev Tools (i.e. you change code, reboot the panel and changes aren't loaded - you need to reboot Photoshop. This happens on OSX at least; knowing it will save some head scratching time). Then you add a listener for the PhotoshopJSONCallback plus extensionID (line 24). On line 26 `PhotoshopCallbackUnique` strips the "ver1" string in the event.data, then parse the JSON and puts it back as an Object. Line 44, `toggleEventRegistering`: this is the unique switches onChange callback, which duty is to dispatch the `com.adobe.PhotoshopRegisterEvent` or `com.adobe.PhotoshopUnRegisterEvent`, corresponding to the on-the-fly-calculated typeID of the provided stringID.

/\*jslint vars: true, plusplus: true, devel: true, nomen: true, regexp: true, indent: 4, maxerr: 50 \*/
/\*global $, window, location, CSInterface, SystemPath, themeManager\*/
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
    

I hope this help clarifying how things work now. I repeat, both `PhotoshopCallback` **and** `PhotoshopJSONCallback` are usable in Photoshop CC2015 (the first is "just" deprecated), while from CC2016 onwards only the latter will work. I'd like to thanks Tom Ruark who pointed me to the Adobe's original code, that I've refactored a bit for this blogpost. Also, I take the chance to tell you I'm working at a more comprehensive learning resource about Photoshop HTML Panels (ebook + video series), so stay tuned here. Ciao, -Davide