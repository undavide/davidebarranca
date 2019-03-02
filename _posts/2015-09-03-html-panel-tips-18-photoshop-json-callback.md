---
title: 'HTML Panel Tips #18: Photoshop JSON Callback'
date: 2015-09-03T19:37:22+01:00
author: Davide Barranca
excerpt: "From Photoshop CC2015, com.adobe.PhotoshopCallback event is deprecated, replaced by com.adobe.PhotoshopJSONCallback. An example is provided."
layout: post
permalink: /2015/09/html-panel-tips-18-photoshop-json-callback/
description: "From Photoshop CC2015, com.adobe.PhotoshopCallback event is deprecated, replaced by com.adobe.PhotoshopJSONCallback. An example is provided."
image: /wp-content/uploads/2015/09/PhotoshopJSONCallback.png
categories:
  - CEP
tags:
  - HTML Panels Tips
  - PhotoshopCallback
  - PhotoshopJSONCallback
---

CC 2015 previews a new Photoshop Event listening system and deprecates the `"com.adobe.PhotoshopCallback"`, due to a bug that makes all the extension receiving the event - that is: if two panels are registered for the event "make", then each panel sees the other panel's "make" event, and must ignore it. The solution implemented marks a break in retro-compatibility and is going to be the only one accepted from Photoshop CC2016 (version 17.x) onwards. Let's have a look at what is that all about.

Please refer to [HTML Panels Tips: #7 Photoshop Events, Take 1](/2014/02/html-panels-tips-7-events-photoshopregisterevent-photoshopcallback/) to review how PS events listening works. Now, there are three different changes you must be aware of:

1.  `"com.adobe.PhotoshopJSONCallback"` is the replacement event - no more `"com.adobe.PhotoshopCallback"`.
2.  The ExtensionID must be **appended as a suffix**.

So what used to be:

{% highlight js %}
var csInterface = new CSInterface();
csInterface.addEventListener("PhotoshopCallback", PSCallback);
{% endhighlight %}

now becomes:

{% highlight js %}
var csInterface = new CSInterface();
var extensionId =  csInterface.getExtensionID();
csInterface.addEventListener("com.adobe.PhotoshopJSONCallback" + extensionId, PhotoshopCallbackUnique);
{% endhighlight %}

3.  The data property of the CSEvent that the PhotoshopCallbackUnique receives is a **JSON string** hidden inside a string.

{% highlight js %}
function PhotoshopCallbackUnique(csEvent) {
  console.log(csEvent);
}
// ver1, {
//   "eventID": 1298866208,
//   "eventData": {
//     "documentID": 1566,
//     "new": {
//       "_obj": "document",
//       "depth": 8,
//       "fill": {
//         {
//           "_enum": "fill",
//           "_value": "white"
//         },
//         "height": {
//           "_unit": "distanceUnit",
//           "_value": 360
//         },
//         "mode": {
//           "_class": "RGBColorMode"
//         },
//         "pixelScaleFactor": 1,
//         "profile": "sRGB IEC61966-2.1",
//         "resolution": {
//           "_unit": "densityUnit",
//           "_value": 300
//         },
//         "width": {
//           "_unit": "distanceUnit",
//           "_value": 504
//         }
//       }
//     }
//   }
{% endhighlight %}

As you see, there's a `ver1,` that needs to be stripped. Also, as a bonus, extra information is provided as JSON (here a New Document's been created). I've uploaded a [demo extension](https://github.com/undavide/PS-Panels-Boilerplate/tree/master/src/com.undavide.PhotoshopJSONCallback) on my GitHub repo, which looks like:

<figure class="aligncenter">
	<img width="400" src="/wp-content/uploads/2015/09/PhotoshopJSONCallback.png" />
</figure>

Which basically let you switch on/off listeners to the corresponding events (`"make"`, `"duplicate"`, etc are the stringID). You can find the full code on GitHub, yet the relevant files are:

## HTML

I've used Topcoat CSS (apparently not anymore maintained, but I love them), nothing too fancy.

{% gist e28806e47548006a4377e5299494381e %}

## JS

The `init` function starts the `themeManager` and then activate _persistence_ (if you need a refresh on the topic, have a look at this older [HTML Panel Tip](/2014/02/html-panels-tips-9-persistence/)). Be careful with persistence, since if it's active it will constantly cache the extension in the Chrome Dev Tools (i.e. you change code, reboot the panel and changes aren't loaded - you need to reboot Photoshop. This happens on OSX at least; knowing it will save some head scratching time). Then you add a listener for the PhotoshopJSONCallback plus extensionID (line 24).

On line 26 `PhotoshopCallbackUnique` strips the `"ver1"` string in the `event.data`, then parse the JSON and puts it back as an Object. Line 44, `toggleEventRegistering`: this is the unique switches onChange callback, which duty is to dispatch the `com.adobe.PhotoshopRegisterEvent` or `com.adobe.PhotoshopUnRegisterEvent`, corresponding to the on-the-fly-calculated typeID of the provided stringID.

{% gist d4cd799c8cc3ebd0244bc7c0ac209c9a %}

I hope this help clarifying how things work now. I repeat, both `PhotoshopCallback` **and** `PhotoshopJSONCallback` are usable in Photoshop CC2015 (the first is "just" deprecated), while from CC2016 onwards only the latter will work. I'd like to thanks Tom Ruark who pointed me to the Adobe's original code, that I've refactored a bit for this blogpost. Also, I take the chance to tell you I'm working at a more comprehensive learning resource about Photoshop HTML Panels (ebook + video series), so stay tuned here.  
Ciao!

{% include_relative cepBook.md %}
