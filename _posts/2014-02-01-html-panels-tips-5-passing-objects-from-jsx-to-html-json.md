---
title: 'HTML Panels Tips: #5 passing Objects from JSX to HTML'
date: 2014-02-01T14:03:01+01:00
author: Davide Barranca
excerpt: "How to pass an Object from Photoshop JSX ExtendScript code back to the HTML panel via JSON"
layout: post
permalink: /2014/02/html-panels-tips-5-passing-objects-from-jsx-to-html-json/
description: "How to pass an Object from Photoshop JSX ExtendScript code back to the HTML panel via JSON"
image: /wp-content/uploads/2014/02/Console.png
categories:
  - CEP
tags:
- HTML Panels Tips
---

This tip covers how to pass Objects from Photoshop (the JSX file) to the HTML Panel. Panels technology doesn't allow (for the moment) to communicate from the JSX to the HTML except via function return (see [Tip #3](/2014/01/html-panels-tips-3-get-data-from-jsx-send-it-to-html/ "HTML Panels Tips: #3 Get data from JSX and send it to HTML")). In order to successfully pass Objects, you need to include [json2.js](https://github.com/douglascrockford/JSON-js/blob/master/json2.js "Douglas Crockford JSON on GitHub") in the JSX and **stringify the Object as a JSON Object**. Then, in the HTML Panel you must **parse the JSON Object** in the callback.

## Example

This panel has just one button: it triggers a function in the JSX that returns an Object, which is then parsed and logged in the console. In order to follow the demo code, you'd better know how to debug a panel ([Tip #1](/2014/01/html-panels-tips-1-debugging/ "HTML Panels Tips: #1 Debugging")), evaluate external JSX  code ([Tip #2](/2014/01/html-panels-tips-2-including-multiple-jsx/ "HTML Panels Tips: #2 Including multiple JSX")) and have a good grasp on how primitive data is passed from JSX to HTML ([Tip #3](/2014/01/html-panels-tips-3-get-data-from-jsx-send-it-to-html/ "HTML Panels Tips: #3 Get data from JSX and send it to HTML")).

### HTML

The usual bare code stripped from [boilerplate](http://davidderaedt.github.io/ccext-website/ "CC Extensibility Helpers") by [David Deraedt](https://twitter.com/davidderaedt "David Deraedt on Twitter").

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
    <button id="btnSend">JSX -> HTML</button>
  </div>

  <script src="js/libs/CSInterface-4.0.0.js"></script>
  <script src="js/libs/jquery-2.0.2.min.js"></script>
  <script src="js/themeManager.js"></script>
  <script src="js/main.js"></script>
</body>
</html>
{% endhighlight %}

### Main.js

In the Javascript it is first declared the `loadJSX()` function (hardwired to look in the jsx/ folder), called in the `init()` in order to evaluate in the Photoshop context the `json2.js` library. Mind you, ExtendScript doesn't support JSON natively so the original library must be included. I've used the commented version by Douglas Crockford, beware that minification in ExtendScript [can be tricky](/2013/08/testing-minified-js-libraries-in-extendscript/ "Testing minified JS Libraries in ExtendScript"). I've provided to the `CSInterface.evalScript()` a callback as an anonymous function, which parses the result as a JSON Object (see next section).

{% highlight js %}
(function() {
  'use strict';
  var csInterface = new CSInterface();

  function loadJSX(fileName) {
    var extensionRoot = csInterface.getSystemPath(SystemPath.EXTENSION) + "/jsx/";
    csInterface.evalScript('$.evalFile("' + extensionRoot + fileName + '")');
  }

  function init() {
    themeManager.init();
    loadJSX("json2.js");

    $("#btnSend").click(function() {
      csInterface.evalScript('sendObjToHTML()', function(result) {
        var o = JSON.parse(result);
        var o = result;
        var str = "";
        for (var prop in o) {
          str += prop + " \[" + typeof o\[prop\] + "\]: " + o\[prop\] + ".\\n";
        }
        console.log(str);
      });
    });
  }

  init();
}());
{% endhighlight %}

### JSX

Here the `sendObjToHTML()` is declared: it first builds an Object literal with arbitrary data (you'll fill it with useful, actual stuff: Photoshop active Document's width, height, ecc.); then the Object is returned as a JSON (stringified) Object. This very Object is the result argument in the anonymous callback in the main.js above.

function sendObjToHTML() {
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
  return JSON.stringify(obj);
}

When the panel is open in Photoshop - provided you've set things according to [Tip #1](/2014/01/html-panels-tips-1-debugging/ "HTML Panels Tips: #1 Debugging") - open Chrome (or Safari, or Firefox) and go to localhost:8088 or whatever port you've opened in the .debug file.

{% include_relative cepBook.md %}
