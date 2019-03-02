---
title: " HTML Panels Tips #15 Asynchronous vs. Synchronous\t\t"
tags:
  - asynchronous
  - CEP 5.2
  - synchronous
url: 2743.html
id: 2743
category:
  - Coding
  - HTML Panels
date: 2014-12-02 17:37:31
---

Today's tip focuses on the Asynchronous nature of `CSInterface.evalScript()` calls, and on ways to make it work in a synchronous fashion.

Synchronous vs. Asynchronous
----------------------------

Quoting [StackOverflow](http://stackoverflow.com/questions/748175/asynchronous-vs-synchronous-execution-what-does-it-really-mean "Synchronous vs Asynchronous"):

> When you execute something synchronously, you wait for it to finish before moving on to another task. When you execute something asynchronously, you can move on to another task before it finishes.

Or visually: ![Synchronous](http://localhost:8888/wp-content/uploads/2014/12/sync.png) ![Asynchronous](http://localhost:8888/wp-content/uploads/2014/12/async.png) Back in Flash days `evalScript` was synchronous, now in HTML land is asynchronous:

CSXSInterface.instance.evalScript('jsxFunction'); // Sync
csInterface().evalScript('jsxFunction'); // Async

Async example
-------------

Remember that evalScript function takes two params:

*   The ExtendScript code to run.
*   A callback function, which in turn takes as a param the returned value from the ExtendScript call.

The default behavior is as follows.

var csInterface = new CSInterface();
var numberOfDocuments = undefined;

csInterface.evalScript('app.documents.length', function(result) {
  numberOfDocuments = result;
});

alert(numberOfDocuments); // undefined

In the above example the `app.documents.length` statement is executed and directly returned - the number of currently opened documents in Photoshop is passed to the callback, which assignes it to the `numberOfDocuments` variable. The alert says "undefined" because of the **asynchronous** nature of evalScript: line #4 executes, then the interpreter goes ahead to line #8 not waiting for the ExtendScript to return (in this example, a single line of code, but in real world a long chain of functions that trigger usually time consuming Photoshop operations) so when `alert(numberOfDocuments)` is called, `numberOfDocuments` is still _undefined_. So to speak, it is like asking your partner to do the shopping then, as soon as (s)he goes out, open the fridge and say "rats, it's empty!"

Sync workarounds
----------------

There are few different ways to mimic a synchronous behavior

### 1\. Set a Timeout

The easiest way is to just wait for your partner to get back from the store:

var csInterface = new CSInterface();
var numberOfDocuments = undefined;

csInterface.evalScript('app.documents.length', function (result) {
    numberOfDocuments = result;
});
setTimeout( alert(numberOfDocuments), 500);

Although, it's not ideal. You might not know in advance how long to wait - it depends of the kind of ExtendScript task you're executing.

### 2\. Nest code in Callbacks

If the alert is moved within the callback it works properly:

var csInterface = new CSInterface();
var numberOfDocuments = undefined;

csInterface.evalScript('app.documents.length', function (result) {
    numberOfDocuments = result;
    alert(numberOfDocuments);
});

Of course this can lead to the so-called _callback hell_ \- nesting evalScript calls one inside another, like:

csInterface.evalScript('/* some code */', function(result) {
    csInterface.evalScript('/* some other code */', function (result) {
        // etc...
    })
});

Yet if you've not fancy needs, it works.

### 3\. Event driven callbacks

This method is supported only from CEP 5.2 onwards. You can implement custom Events via _PlugPlugExternalObject_, that are dispatched by ExtendScript and listened from JS. I've put the ExtendScript code in its own JSX file:

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

Then in the JS you set up a listener, which is the one who fires the alert();

var csInterface = new CSInterface();

csInterface.addEventListener("alertDocumentsNumber", function(event) {
    alert("The number of open documents is: " + event.data);
});

csInterface.evalScript('getNumberOfDocuments()', function(result) {
    if (result == false) { alert ("There's been a problem."); }
});

Mind you, as far as other devs such as [Kris Coppieters](http://www.rorohiko.com "Kris Coppieters") have supposed, "_even if you manage to make multiple JSX calls from different parts of your JS, probably you can't run multiple ExtendScripts concurrently_".

Acknowledgements
----------------

I'd like to thank Zihong Chen and Ten from Adobe and the others who've helped shedding some light upon this topic in the [forums](https://forums.adobe.com/thread/1360552 "Updating a variable from JSX").