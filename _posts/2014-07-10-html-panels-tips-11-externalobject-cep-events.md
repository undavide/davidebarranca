---
title: 'HTML Panels Tips: #11 CEP Events (ExternalObject)'
date: 2014-07-10T10:03:47+01:00
author: Davide Barranca
excerpt: "The new CEP5 (PS CC 2014) lets you dispatch ExtendScript Custom Events with payloads, using the integrated plugplugExternalObject library."
layout: post
permalink: /2014/07/html-panels-tips-11-externalobject-cep-events/
description: "The new CEP5 (PS CC 2014) lets you dispatch ExtendScript Custom Events with payloads, using the integrated plugplugExternalObject library."
image: /wp-content/uploads/2014/07/JSX2CEP-panel.png
categories:
  – CEP
tags:
  - HTML Panels Tips
  - Events
---

The new CEP5 (from Photoshop CC 2014 onwards) has introduced the possibility to dispatch **Custom Events** from JSX and listen to them in the HTML Panel - this Tip shows you how to implement this communication.

## Events et al.

I've already covered Events in this series, so let me summarize what we can do with them and the different kinds of Events we might deal with.

1.  **CEP Host Application Events** - i.e. Photoshop events related either to the app itself or the documents (but from the "host app point of view", see [this post](/2014/07/html-panels-tips-12-cep-application-events/ "HTML Panels Tips: #12 CEP Application Events")) such as:
    *   `appOffline`
    *   `appOnline`
    *   `applicationActivate`
    *   `documentAfterActivate`
    *   `documentAfterDeactivate`
2.  **ExtendScript Events** - i.e. Events related to _actual_ Photoshop operations (dispatched when the user creates new Layer, selects/moves something, etc), see [this post](/2014/02/html-panels-tips-7-events-photoshopregisterevent-photoshopcallback/ "HTML Panels Tips: #7 Photoshop Events, Take 1") and a [different approach](/2014/02/html-panels-tips-8-photoshop-events-pshostadapter-libraries/ "HTML Panels Tips: #8 Photoshop Events, Take 2").
3.  **Custom ExtendScript Events** - i.e. Events you dispatch yourself in the JSX to communicate with, and pass data to, the HTML Panel (what this post is all about).

## ExternalObject

Photoshop, Illustrator and Premiere Pro CC 2014 by the time of this writing (July 2014) support natively the dispatching of custom Events (using the `PlugPlugExternalObject` - which library is embedded in the app and we can happily forget about it), while InDesign must explicitly add and reference it ([download it here](https://github.com/Adobe-CEP/CEP-Resources/releases/tag/1 "PlugPlugExternalObject library for InDesign") and see the SDK Guide at pag 45). Basically you set an Event Listener in the JS for a custom event (named as you like), providing a callback. Then in the JSX you create and dispatch a `CSXSEvent()` at will.

## Example

You can download a full panel from my **PS Panels Boilerplate** repository on GitHub [here](https://github.com/undavide/PS-Panels-Boilerplate/tree/master/src/com.undavide.JSX2CEP "JSX to CEP Events Panel"). As follows some code highlights to demo the mechanism.

### JS

In the JS side there's a simple EventListener with a callback as usual.

{% highlight js %}
var csInterface = new CSInterface();
csInterface.addEventListener("My Custom Event", function(evt) {
    console.log('Data from the JSX payload: ' + evt.data);
});
{% endhighlight %}

### JSX

In the JSX you create an ExternalObject instance: `"lib:\PlugPlugExternalObject"` is the only parameter to pass. If there's no error (the library exists and it's available) you create a new `CSXSEvent` setting the type (which is referenced in the EventListener - the string must be the same, in the example "My Custom Event") and a data property which is the payload that the HTML Panel is going to receive.

{% highlight js %}
try {
    var xLib = new ExternalObject("lib:\\PlugPlugExternalObject");
} catch (e) {
    alert(e);
}

if (xLib) {
    var eventObj = new CSXSEvent();
    eventObj.type = "My Custom Event";
    eventObj.data = "some payload data...";
    eventObj.dispatch();
}
{% endhighlight %}

[![JSX2CEP-panel](/wp-content/uploads/2014/07/JSX2CEP-panel-191x300.png)](https://github.com/undavide/PS-Panels-Boilerplate/tree/master/src/com.undavide.JSX2CEP)

In the [GitHub panel](https://github.com/undavide/PS-Panels-Boilerplate/tree/master/src/com.undavide.JSX2CEP "ExtendScript to CEP Communication") things are slightly more elaborate: I've set a JSX function triggered by a button - which sends Custom Events spaced 1 second each. The Events' payloads are then logged in a textarea.

I'm facing two issues, though. First, I would expect that the log messages come time spaced (first log, 1" wait, second log, 1" wait, third log, 1" wait, etc), while the get to the textarea all together. Second, the [Return Message...] which comes as the result value passed to the evalScript() callback appears as the first, and not last, log. As far as I understand, the Events are kept on hold until the whole JSX routine is done and the evalScript() callback executed - whether this is a problem of Event dispatching (the Photoshop / ExtendScript side) or Event listening (the JS / HTML Panel side) I can't really say.

## Bonus

A Twitter conversation with the digital artist [Sergey Kritskiy](http://hundredsofsparrows.com) led me to experiment whether the Panel is able to listen for CSXSEvents dispatched only from its own JSX files (the ones in its scope), or not. In other words, can an (open) HTML Panel fire a callback in response to a Custom Event of the right type coming from whatever Script might happen to run in Photoshop in that moment? The answer is yes: in fact if you keep the JSX to CEP Communication panel open and run in the ExtendScript ToolKit the same chunk of code provided in the JSX section above in this very post, the Panel shows "some payload data..." in its textarea.

{% include_relative cepBook.md %}
