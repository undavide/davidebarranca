---
title: 'HTML Panels Tips #15 Asynchronous vs. Synchronous'
date: 2014-12-02T17:37:31+01:00
author: Davide Barranca
excerpt: "Today's tip focuses on the Asynchronous nature of CSInterface.evalScript() calls, and on ways to make it work in a synchronous fashion."
layout: post
permalink: /2014/12/html-panels-tips-15-asynchronous-vs-synchronous-evalscript/
description: "In HTML Panels, CSInterface.evalScript calls are asynchronous - there are few ways to mimic synchronous behaviours, which I will be showing."
image: /wp-content/uploads/2014/12/featured.png
category:
  - CEP
tags:
  - HTML Panels Tips
  - evalScript
  - asynchronous
---

Today's tip focuses on the Asynchronous nature of `CSInterface.evalScript()` calls, and on ways to make it work in a synchronous fashion.

## Synchronous vs. Asynchronous

Quoting [StackOverflow](http://stackoverflow.com/questions/748175/asynchronous-vs-synchronous-execution-what-does-it-really-mean "Synchronous vs Asynchronous"):

> When you execute something synchronously, you wait for it to finish before moving on to another task. When you execute something asynchronously, you can move on to another task before it finishes.

Or visually:

![Synchronous](/wp-content/uploads/2014/12/sync.png)

![Asynchronous](/wp-content/uploads/2014/12/async.png)

Back in Flash days `evalScript` was synchronous, now in HTML land is asynchronous:

{% highlight js %}
CSXSInterface.instance.evalScript('jsxFunction'); // Sync
csInterface().evalScript('jsxFunction'); // Async
{% endhighlight %}

## Async example

Remember that evalScript function takes two params:

*   The ExtendScript code to run.
*   A callback function, which in turn takes as a param the returned value from the ExtendScript call.

The default behavior is as follows.

{% highlight js %}
var csInterface = new CSInterface();
var numberOfDocuments = undefined;

csInterface.evalScript('app.documents.length', function(result) {
  numberOfDocuments = result;
});

alert(numberOfDocuments); // undefined
{% endhighlight %}

In the above example the `app.documents.length` statement is executed and directly returned - the number of currently opened documents in Photoshop is passed to the callback, which assigns it to the `numberOfDocuments` variable. The alert says "undefined" because of the **asynchronous** nature of evalScript: line #4 executes, then the interpreter goes ahead to line #8 not waiting for the ExtendScript to return (in this example, a single line of code, but in real world a long chain of functions that trigger usually time consuming Photoshop operations) so when `alert(numberOfDocuments)` is called, `numberOfDocuments` is still _undefined_. So to speak, it is like asking your partner to do the shopping then, as soon as (s)he goes out, open the fridge and say "rats, it's empty!"

## Sync workarounds

There are few different ways to mimic a synchronous behavior

### 1. Set a Timeout

The easiest way is to just wait for your partner to get back from the store:

{% highlight js %}
var csInterface = new CSInterface();
var numberOfDocuments = undefined;

csInterface.evalScript('app.documents.length', function (result) {
  numberOfDocuments = result;
});
setTimeout( alert(numberOfDocuments), 500);
{% endhighlight %}

Although, it's not ideal. You might not know in advance how long to wait - it depends of the kind of ExtendScript task you're executing.

### 2. Nest code in Callbacks

If the alert is moved within the callback it works properly:

{% highlight js %}
var csInterface = new CSInterface();
var numberOfDocuments = undefined;

csInterface.evalScript('app.documents.length', function (result) {
  numberOfDocuments = result;
  alert(numberOfDocuments);
});
{% endhighlight %}

Of course this can lead to the so-called _callback hell_ – nesting evalScript calls one inside another, like:

{% highlight js %}
csInterface.evalScript('/* some code */', function(result) {
  csInterface.evalScript('/* some other code */', function (result) {
    // etc...
  })
});
{% endhighlight %}

Yet if you've not fancy needs, it works.

### 3. Event driven callbacks

This method is supported only from CEP 5.2 onwards. You can implement custom Events via _PlugPlugExternalObject_, that are dispatched by ExtendScript and listened from JS. I've put the ExtendScript code in its own JSX file:

{% highlight js %}
function getNumberOfDocuments () {
  try {
    var xLib = new ExternalObject("lib:\\PlugPlugExternalObject");
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
}
{% endhighlight %}

Then in the JS you set up a listener, which is the one who fires the alert();

{% highlight js %}
var csInterface = new CSInterface();

csInterface.addEventListener("alertDocumentsNumber", function(event) {
  alert("The number of open documents is: " + event.data);
});

csInterface.evalScript('getNumberOfDocuments()', function(result) {
  if (result == false) { alert ("There's been a problem."); }
});
{% endhighlight %}

Mind you, as far as other devs such as [Kris Coppieters](http://www.rorohiko.com) have supposed, "_even if you manage to make multiple JSX calls from different parts of your JS, probably you can't run multiple ExtendScripts concurrently_".

## Acknowledgements

I'd like to thank Zihong Chen and Ten from Adobe and the others who've helped shedding some light upon this topic in the [forums](https://forums.adobe.com/thread/1360552 "Updating a variable from JSX").

{% include_relative cepBook.md %}
