---
title: " HTML Panels Tips: #12 CEP Application Events\t\t"
tags:
  - CEP
  - Events
  - Host
url: 2682.html
id: 2682
category:
  - Coding
  - HTML Panels
date: 2014-07-10 15:49:32
---

HTML Panels can listen to CEP Application Events - that is to say, Events dispatched by the Host (Photoshop, InDesign...) when something related to the app itself, or its documents, happens.

List of Photoshop Events
------------------------

To date (CC 2014, version 2014.0.0 Release, 20140508.r58) this is the list of the Events that Photoshop is able to dispatch. Hopefully the names are self-explanatory.

*   `"com.adobe.csxs.events.AppOffline"`
*   `"com.adobe.csxs.events.AppOnline"`
*   `"applicationActivate"`
*   `"applicationBeforeQuit"`
*   `"documentAfterActivate"`
*   `"documentAfterDeactivate"`
*   `"documentEdited"`

Mind you, according to the _Extension SDK guide_ some of them aren't supported in Photoshop yet; others aren't listed there (but they come from the CEP HTML Test Panel from the [Samples GitHub repo](https://github.com/Adobe-CEP/Samples/tree/master/CEP_HTML_Test_Extension_5.0 "Adobe CEP Samples")).

Implementation
--------------

As easy as it gets - this goes in the JS:

var csInterface = new CSInterface();
csInterface.addEventListener("appOffline", logEvent);
// just a demo callback
function logEvent(evt) { console.dir(evt); }

Event Parameters
----------------

The event object passed to the callback in Photoshop has several properties:

*   `appId: "PHSX"`
*   `extensionId: ""`
*   `scope: "APPLICATION"`
*   `type`
*   `data`

According to each `type`, the `data` might come as an XML String, as follows:

type: "com.adobe.csxs.events.AppOffline"
data: ""

type: "com.adobe.csxs.events.AppOnline"
data: ""

type: "applicationActivate"
data: "<applicationActivate/>"

type: "applicationBeforeQuit"
data: "<applicationBeforeQuit/>"

type: "documentAfterActivate"
data: "<documentAfterActivate><name>icon</name><url>file:///Macintosh SSD/Users/Davide/Desktop/icon.tif</url></documentAfterActivate>"

type: "documentAfterDeactivate"
data: "<documentAfterDeactivate><name>icon</name><url>file:///Macintosh SSD/Users/Davide/Desktop/icon.tif</url></documentAfterDeactivate>"

type: "documentEdited"
data: "<documentEdited><name>icon</name><url>file:///Macintosh SSD/Users/Davide/Desktop/icon.tif</url></documentEdited>"

Mind you, I've been able to log the `applicationBeforeQuit` event only trying to quit Photoshop when an unsaved document was open - this gives the time for the `console.dir` to be logged, otherwise the Object is empty, after the app has quit. Also, I haven't been able to successfully intercept the `"com.adobe.csxs.events.ExtensionUnloaded"` event, which would be of _great use_.