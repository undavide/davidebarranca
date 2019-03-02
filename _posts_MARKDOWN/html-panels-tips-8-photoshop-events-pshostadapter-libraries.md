---
title: " HTML Panels Tips: #8 Photoshop Events, Take 2\t\t"
tags:
  - addEventListener
  - Events
  - PSHostAdapter
url: 2503.html
id: 2503
category:
  - Coding
  - HTML Panels
date: 2014-02-07 22:53:41
---

There's a second way to listen to Photoshop Events from an HTML Panel - i.e. using the PSHostAdapters libraries - and today's tip will show you how. In [Tip #7](http://localhost:8888/2014/02/html-panels-tips-7-events-photoshopregisterevent-photoshopcallback/ "HTML Panels Tips: #7 Photoshop Events, Take 1") I've demoed how using `PhotoshopRegisterEvent` let you listen to events and register callbacks: but that is just one way out of the two available (as it was back in the Flash land) so let's find the second one. ![PSEvents](http://localhost:8888/wp-content/uploads/2014/02/PSEvents.png)

Example
-------

I'll be using as a starting point the very same Tip #7 code. This example panel has a switch that enables a listener on the cut, copy and paste events in Photoshop - the callback passes the corresponding StringID for displaying to the Text Area. Mind you, when an HTML Panel is open keyboard shortcuts might not work, so you'd be better off using the menus in order to trigger the events correctly - Adobe engineers are aware and will fix that in the future.

### Libraries

You need two PSHostAdapter libraries, that you can grab from the Extension Builder installation (in the Eclipse application): they are `ps_host_adapter-2.0.js` and (depending on your OS) `PSHostAdapter.plugin` or `PSHostAdapter.plugin.8li`. The following paths come from my OSX installation, for PC users it shouldn't be any different - just use the Eclipse application as the root:

/Applications/eclipse/plugins/com.adobe.cside.html.libsinstaller\_1.0.0.201307260955/archive/jsar-1.0/release/ps\_host_adapter-2.0.js

/Applications/eclipse/plugins/com.adobe.cside.html.libsinstaller\_1.0.0.201307260955/archive/csadapters-3.0/ps\_host_adapter/

You should copy the `ps_host_adapter-2.0.js` somewhere in your project and include it in the index.html. Conversely, `PSHostAdapter.plugin` or `PSHostAdapter.plugin.8li` file must go in the Photoshop Plug-Ins folder (`Plug-Ins/Automate/` works just fine on my OSX installation).

### HTML

It's basically the same of Tip #7, plus the new linked script. As you see, I'm using the Topcoat CSS library to style the panel GUI (refer to [Tip #6](http://localhost:8888/2014/02/html-panels-tips-6-integrating-topcoat-css/ "HTML Panels Tips: #6 integrating Topcoat CSS") for detailed instruction).

<!doctype html>
<html>
<head>
<meta charset="utf-8">
<link id="hostStyle" rel="stylesheet" href="css/theme.css"/>
<link id="theme" rel="stylesheet" href="css/light.css"/>
<title></title>
</head>
<body>

    <div style="width: 80%; margin:0 auto">
        <h3 class="center">PS Events</h3>
        <label class="topcoat-switch">
            <input id="registerEvent" type="checkbox" class="topcoat-switch__input">
            <div class="topcoat-switch__toggle"></div>
        </label>
        <input type="text" id ="result" class="topcoat-text-input" style="margin-left:10px" placeholder="Listen for Events" value="">
    </div>

    <script src="js/libs/CSInterface-4.0.0.js"></script>
    <script src="js/libs/ps\_host\_adapter-2.0.js"></script>
    <script src="js/libs/jquery-2.0.2.min.js"></script>
    <script src="js/themeManager.js"></script>
    <script src="js/main.js"></script>

</body>
</html>

###  Javascript

Compared to [Photoshop Events, Take 1](http://localhost:8888/2014/02/html-panels-tips-7-events-photoshopregisterevent-photoshopcallback/ "HTML Panels Tips: #7 Photoshop Events, Take 1") the `Register()` function is where things are different - the rest of the code is unchanged. It's a matter of adding an Event Listener to a `PSEventAdapter` Instance, passing either a constant (see in `ps_host_adapter-2.0.js` for a list of the available ones) or an event TypeID, which you can grab as usual from the Scripting Listener log. The `PSCallback()` function communicates then with the JSX the `event.type` in order to retrieve the StringID, which is passed to the Text Area.

(function () {
    'use strict';

    var csInterface = new CSInterface();   

    function Register(inOn) {

        // Events from ps\_host\_adapter-2.0.js
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

}());

Which is similar to the code for Flash panels in Extension Builder 2:

PSEventAdapter.getInstance().addEventListener(PSEvent.ROTATE, handleEvent);
private function handleEvent(event:PSEvent):void
{
    //handle rotate event
}

###  JSX

Just a dedicate function here:

function convertTypeID (tID) {
	return typeIDToStringID(Number(tID));
}