---
title: " HTML Panels Tips: #4 passing Objects from HTML to JSX\t\t"
tags:
  - html panels
  - objects
  - tip
url: 2456.html
id: 2456
category:
  - Coding
  - HTML Panels
date: 2014-01-31 23:53:10
---

How to pass complex Objects from the HTML Panel to a Photoshop's ExtendScript function in a JSX file. You may want to pack different information from the HTML Panel (for instance all the values in the GUI) in a single Object, and pass just it as a parameter to a JSX function - that the Photoshop interpreter will eventually execute. For primitive, multiple parameters this is easily done via `CSInterface.evalScript()`, in the main.js:

var csInterface = new CSInterface();
csInterface.evalScript('sayHello("Ciao", 2014)');

...and providing a corresponding function in the JSX file:

function sayHello(str, num) {
   alert("Photoshop says " + str + "! Have a great " + num);
}

How do you deal with Objects? **Stringify them as JSON objects first**. No need to parse them afterwards.

Example
-------

I use as always the [boilerplate code](http://davidderaedt.github.io/ccext-website/ "CC Extensibility Helpers") by [David Deraedt](https://twitter.com/davidderaedt "David Deraedt on Twitter").

### HTML

This is a minimal `index.html` with just a button.

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

### Main.js

Here I've hardcoded an Object literal (feel free to fill it with actual values from the HTML GUI), which is **stringified as a JSON object** in order to pass it as a parameter:

(function () {
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
        $("#btnSend").click(function () {
            csInterface.evalScript( 'parseObj(' + JSON.stringify(obj) + ')' );
        });
    }        
    init();
}());

### JSX

In the ExtendScript file I've provided a logging function to actually verify that the Object is interpreted correctly.

function parseObj(o) {
  var str = "";
    for (prop in o) {
      str += prop + " \[" + typeof o\[prop\] + "\]: " + o\[prop\] + ".\\n";
    }
  alert(str);
}

![Object](http://localhost:8888/wp-content/uploads/2014/01/Object.png)Mind you: I've experimented importing a JSON library in the ExtendScript code (which doesn't natively support it) and tried to `var obj=JSON.parse(o);` to no avail. Apparently the code is OK as it is, without it.