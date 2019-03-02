---
title: 'HTML Panels Tips: #4 passing Objects from HTML to JSX'
date: 2014-01-31T23:53:10+01:00
author: Davide Barranca
excerpt: "How to pass complex Objects from an HTML Panel to an ExtendScript function in a JSX file"
layout: post
permalink: /2014/01/html-panels-tips-4-passing-objects-from-html-to-jsx/
description: "How to pass complex Objects from an HTML Panel to an ExtendScript function in a JSX file"
image: /wp-content/uploads/2014/01/Object.png
categories:
  - CEP
tags:
  - HTML Panels Tips
---

How do you pass complex Objects from the HTML Panel to a Photoshop's ExtendScript function in a JSX file? You may want to pack different information from the HTML Panel (for instance all the values in the GUI) in a single Object, and pass just it as a parameter to a JSX function - that the Photoshop interpreter will eventually execute. For primitive, multiple parameters this is easily done via `CSInterface.evalScript()`, in the main.js:

{% highlight js %}
var csInterface = new CSInterface();
csInterface.evalScript('sayHello("Ciao", 2014)');
{% endhighlight %}

...and providing a corresponding function in the JSX file:

{% highlight js %}
function sayHello(str, num) {
  alert("Photoshop says " + str + "! Have a great " + num);
}
{% endhighlight %}

How do you deal with Objects? **Stringify them as JSON objects first**. No need to parse them afterwards.

## Example

I use as always the [boilerplate code](http://davidderaedt.github.io/ccext-website/ "CC Extensibility Helpers") by [David Deraedt](https://twitter.com/davidderaedt "David Deraedt on Twitter").

### HTML

This is a minimal `index.html` with just a button.

{% highlight html %}
<!doctype html>
<html>
<head>
<meta charset="utf-8">
<link rel="stylesheet" href="css/styles.css"/>
<link id="hostStyle" rel="stylesheet" href="css/theme.css"/>
<title></title>
</head>
<body>
  <div id="content">        
    <h1>Object communication</h1>
      <button id="btnSend">HTML -> JSX</button>
  </div>

  <script src="js/libs/CSInterface-4.0.0.js"></script>
  <script src="js/libs/jquery-2.0.2.min.js"></script>
  <script src="js/themeManager.js"></script>
  <script src="js/main.js"></script>
</body>
</html>
{% endhighlight %}

### Main.js

Here I've hardcoded an Object literal (feel free to fill it with actual values from the HTML GUI), which is **stringified as a JSON object** in order to pass it as a parameter:

{% highlight js %}
(function() {
  'use strict';
  var csInterface = new CSInterface();

  function init() {
    themeManager.init();
    var obj = {
      str: "Love your job!",
      num: 38,
      today: new Date(),
      nestedObj: {
        nestedStr: "Even if nothing works",
        nestedNum: 8,
        nestedDate: new Date()
      }
    }
    $("#btnSend").click(function() {
      csInterface.evalScript('parseObj(' + JSON.stringify(obj) + ')');
    });
  }
  init();
}());
{% endhighlight %}

### JSX

In the ExtendScript file I've provided a logging function to actually verify that the Object is interpreted correctly.

{% highlight js %}
function parseObj(o) {
  var str = "";
    for (prop in o) {
      str += prop + " \[" + typeof o\[prop\] + "\]: " + o\[prop\] + ".\\n";
    }
  alert(str);
}
{% endhighlight %}

![Object](/wp-content/uploads/2014/01/Object.png)

Mind you: I've experimented importing a JSON library in the ExtendScript code (which doesn't natively support it) and tried to `var obj=JSON.parse(o);` to no avail. Apparently the code is OK as it is, without it.

{% include_relative cepBook.md %}
