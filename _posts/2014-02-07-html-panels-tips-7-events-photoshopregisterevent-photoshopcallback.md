---
title: 'HTML Panels Tips: #7 Photoshop Events, Take 1'
date: 2014-02-07T16:22:18+01:00
author: Davide Barranca
excerpt: "The first way for an HTML Panel to listen for Photoshop Events: PhotoshopRegisterEvent and PhotoshopCallback"
layout: post
permalink: /2014/02/html-panels-tips-7-events-photoshopregisterevent-photoshopcallback/
description: "The first way for an HTML Panel to listen for Photoshop Events: PhotoshopRegisterEvent and PhotoshopCallback"
image: /wp-content/uploads/2014/02/PSEvents1.png
category:
  - CEP
tags:  
  - HTML Panels Tips
---

This tip shows you the first way to make an HTML Panel listen and react to Photoshop Events: via `PhotoshopRegisterEvent`. Back in the Flash land, there were **two ways** to register callbacks for Photoshop Events: using either a **PSHosttAdapter** library, or an **ExternalInterface** object and PhotoshopCallback. Today's tip is about the latter (see [Tip #8](/2014/02/html-panels-tips-8-photoshop-events-pshostadapter-libraries/ "HTML Panels Tips: #8 Photoshop Events, Take 2") for the PSHostAdapter libraries). What we used to write in **ActionScript** was something like:

{% highlight as %}
// Set TypeID for desired events
private const COPY_INT:int = Photoshop.app.charIDToTypeID(’copy’);
private const PASTE_INT:int = Photoshop.app.charIDToTypeID(’past’);

// Register for events
CSInterface.PhotoshopRegisterEvent(COPY_INT + "," + PASTE_INT);
// attach a callback
ExternalInterface.addCallback("PhotoshopCallback" + CSInterface.getExtensionId(), myPhotoshopCallback);
// Define the callback
private function myPhotoshopCallback(eventID:Number, descID:Number):void {
//
}
{% endhighlight %}

Things are slightly different now in **Javascript**. and are based on `CSInterface` and the dispatching of a custom Event. As follows the mechanism in a nutshell, then I'll show you an actual example:

{% highlight js %}
var csInterface = new CSInterface();

// Create a new Event
var event = new CSEvent("com.adobe.PhotoshopRegisterEvent", "APPLICATION");

// Set Event properties: extension id
event.extensionId = "com.example.psevents";

// Set Event properties: data (as Type ID string, comma separated)
// 1668247673 = charIDToTypeID( "copy" ) = copy
// 1885434740 = charIDToTypeID( "past" ) = paste
// 1668641824 = charIDToTypeID( "cut " ) = cut
event.data = "1668247673, 1885434740, 1668641824";

// Dispatch the Event
csInterface.dispatchEvent(event);

// Attach a callback
csInterface.addEventListener("PhotoshopCallback", PSCallback);

// Define the callback
function PSCallback(csEvent) {
  // ...
}
{% endhighlight %}

## Example

![PSEvents](/wp-content/uploads/2014/02/PSEvents1.png)

This Panel has a switch to turn ON/OFF the listener for few PS Events (cut, copy, paste). When the listener is active and the user cuts, copies or pastes, the Event's StringID is written in the text area. In case you need a refresh of HTML Panels related topics to go through this one, here's the [whole list](/category/code/html-panels/ "HTML Panels Tips"). You might need a refresh because this example uses Topcoat CSS (see [HTML Panels Tip #6](/2014/02/html-panels-tips-6-integrating-topcoat-css/ "HTML Panels Tips: #6 integrating Topcoat CSS")) and communication between HTML and JSX ([Tip #4](/2014/01/html-panels-tips-4-passing-objects-from-html-to-jsx/ "HTML Panels Tips: #4 passing Objects from HTML to JSX")).

### HTML

The Panel uses [boilerplate code](http://davidderaedt.github.io/ccext-website/ "CC Extensibility Helpers") by [David Deraedt](https://twitter.com/davidderaedt "David Deraedt on Twitter") with the addition of Topcoat CSS (refer to [HTML Panels Tip #6](/2014/02/html-panels-tips-6-integrating-topcoat-css/ "HTML Panels Tips: #6 integrating Topcoat CSS") for the details). I've set up a Topcoat Switch and a Text Area:

{% highlight js %}
<!doctype html>
<html>

<head>
  <meta charset="utf-8">
  <link id="hostStyle" rel="stylesheet" href="css/theme.css" />
  <link id="theme" rel="stylesheet" href="css/light.css" />
  <title></title>
</head>

<body>
  <div style="width: 80%; margin:0 auto">
    <h3 class="center">PS Events</h3>
    <label class="topcoat-switch">
      <input id="registerEvent" type="checkbox" class="topcoat-switch__input">
      <div class="topcoat-switch__toggle"></div>
    </label>
    <input type="text" id="result" class="topcoat-text-input" style="margin-left:10px" placeholder="Listen for Events" value="">
  </div>

  <script src="js/libs/CSInterface-4.0.0.js"></script>
  <script src="js/libs/jquery-2.0.2.min.js"></script>
  <script src="js/themeManager.js"></script>
  <script src="js/main.js"></script>

</body>

</html>
{% endhighlight %}

### Javascript

In `main.js` it's defined the Register() function that attaches (or remove) the listener, dispatching a CSEvent as you've already seen. A jQuery function binds the switch to `Register()` and finally a `PSCallback` function is defined: there, the Event data String is split and passed for evaluation to JSX (see [Tip #4](/2014/01/html-panels-tips-4-passing-objects-from-html-to-jsx/ "HTML Panels Tips: #4 passing Objects from HTML to JSX") for details about HTML to JSX data exchange). The `evalScript` callback (that is, the JSX return value) is displayed in the Text Area.

{% highlight js %}
(function() {
  'use strict';

  var csInterface = new CSInterface();

  function Register(inOn) {

    if (inOn) {
      var event = new CSEvent("com.adobe.PhotoshopRegisterEvent", "APPLICATION");
    } else {
      var event = new CSEvent("com.adobe.PhotoshopUnRegisterEvent", "APPLICATION");
    }
    event.extensionId = "com.example.psevents";

    // some events:
    // 1668247673 = charIDToTypeID( "copy" ) = copy
    // 1885434740 = charIDToTypeID( "past" ) = paste
    // 1668641824 = charIDToTypeID( "cut " ) = cut
    event.data = "1668247673, 1885434740, 1668641824";
    csInterface.dispatchEvent(event);
  }

  function init() {

    themeManager.init();

    // Switch onChange callback
    $('#registerEvent').change(function() {
      Register($(this).is(':checked')); // true or false
    });
  }

  function PSCallback(csEvent) {
    var dataArray = csEvent.data.split(",");
    // send to JSX to convert typeIDs to stringIDs
    csInterface.evalScript('convertTypeID(' + JSON.stringify(dataArray\[0\]) + ')', function(res) {
      $('#result').val(res.toString());
    });
  }

  init();
  csInterface.addEventListener("PhotoshopCallback", PSCallback);

}());
{% endhighlight %}

### JSX

In the JSX file there's just a function, that converts the TypeID to StringID and returns it.

{% highlight js %}
function convertTypeID (typeArray) {
  return typeIDToStringID(Number(typeArray));
}
{% endhighlight %}

{% include_relative cepBook.md %}
