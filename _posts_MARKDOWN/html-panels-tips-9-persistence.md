---
title: " HTML Panels Tips: #9 Persistence\t\t"
tags:
  - persistent
  - photoshopPersistent
  - tip
url: 2512.html
id: 2512
category:
  - Coding
  - HTML Panels
date: 2014-02-08 18:27:18
---

How to make the HTML Panel's session persist even if the panel is closed in Photoshop. Each time an HTML Panel is launched, its GUI resets to the default values and initialization is performed. This happens not only in between Photoshop sessions, but also when the Panel is collapsed, iconized or closed. Today's tip shows you how to make the Panel persistent, as we did back in Flash land with:

// In Flash panels you would:
CSXSInterface.instance.evalScript("PhotoshopPersistent");

The mechanism now in HTML land requires Event dispatching - see [Tip #7](http://localhost:8888/2014/02/html-panels-tips-7-events-photoshopregisterevent-photoshopcallback/ "HTML Panels Tips: #7 Photoshop Events, Take 1"): Photoshop Events Take 1 for the details.

Code on GitHub
--------------

Please [find here](https://github.com/undavide/PS-Panels-Boilerplate/tree/master/src/com.undavide.persistent1 "Persistence Panel on GitHub") the complete code for this example.

Example
-------

![Persistent](http://localhost:8888/wp-content/uploads/2014/02/persistent.png) This panel has a switch that enables Panel's persistence, and a bunch of other switches that have no effect other than show persistence in action. When the Persistent is ON, you can play with the other ones and collapse/close then reopen the panel: the secondary switches are shown in the state they were before. Conversely, if Persistent is OFF, each time the panel is opened all the switches are reset.

### HTML

I make use of the Topcoat CSS Library (refer to [Tip #6](http://localhost:8888/2014/02/html-panels-tips-6-integrating-topcoat-css/ "HTML Panels Tips: #6 integrating Topcoat CSS") for the implementation).

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
        <h3 class="center">Panel Persistence</h3>
        <label class="switch-label">Persistent:</label>
        <label class="topcoat-switch">
            <input id="persistenceSwitch" type="checkbox" class="topcoat-switch__input">
            <div class="topcoat-switch__toggle"></div>
        </label>
        <hr>
        <div class="flex-container">
            <label class="topcoat-switch switch">
                <input id="testSwitch1" type="checkbox" class="topcoat-switch__input">
                <div class="topcoat-switch__toggle"></div>
            </label>
            <label class="topcoat-switch switch">
                <input id="testSwitch2" type="checkbox" class="topcoat-switch__input">
                <div class="topcoat-switch__toggle"></div>
            </label>
            <label class="topcoat-switch switch">
                <input id="testSwitch3" type="checkbox" class="topcoat-switch__input">
                <div class="topcoat-switch__toggle"></div>
            </label>
            <label class="topcoat-switch switch">
                <input id="testSwitch4" type="checkbox" class="topcoat-switch__input">
                <div class="topcoat-switch__toggle"></div>
            </label>
            <label class="topcoat-switch switch">
                <input id="testSwitch5" type="checkbox" class="topcoat-switch__input">
                <div class="topcoat-switch__toggle"></div>
            </label>
            <label class="topcoat-switch switch">
                <input id="testSwitch6" type="checkbox" class="topcoat-switch__input">
                <div class="topcoat-switch__toggle"></div>
            </label>
            <label class="topcoat-switch switch">
                <input id="testSwitch7" type="checkbox" class="topcoat-switch__input">
                <div class="topcoat-switch__toggle"></div>
            </label>
            <label class="topcoat-switch switch">
                <input id="testSwitch8" type="checkbox" class="topcoat-switch__input">
                <div class="topcoat-switch__toggle"></div>
            </label>
            <label class="topcoat-switch switch">
                <input id="testSwitch9" type="checkbox" class="topcoat-switch__input">
                <div class="topcoat-switch__toggle"></div>
            </label>
        </div>
    </div>

    <script src="js/libs/CSInterface-4.0.0.js"></script>
    <script src="js/libs/jquery-2.0.2.min.js"></script>
    <script src="js/themeManager.js"></script>
    <script src="js/main.js"></script>

</body>
</html>

### Javascript

The `main.js` file is where things happen. A `Persistent()` function is called every time the `persistenceSwitch` changes: it sets up and dispatches a `CSEvent` (see the highlighted lines), which is bound to the extension's id (`gExtensionId` variable).

(function () {
    'use strict';

    var csInterface = new CSInterface();   
    var gExtensionId = "com.example.persistent";

    function Persistent(inOn) {

        if (inOn){
            var event = new CSEvent("com.adobe.PhotoshopPersistent", "APPLICATION");
        } else {
            var event = new CSEvent("com.adobe.PhotoshopUnPersistent", "APPLICATION");
        }
        event.extensionId = gExtensionId;
        csInterface.dispatchEvent(event);
    }

    function init() {

        themeManager.init();

        $('#persistenceSwitch').change(function() {
            Persistent( $(this).is(':checked') );
        });
    }

    init();

}());

This makes the Panel persistent or... "unpersistent" ;-)