---
title: " HTML Panels Tips: #3 Get data from JSX and send it to HTML\t\t"
tags:
  - html panels
  - jsx
  - tip
url: 2452.html
id: 2452
category:
  - Coding
  - HTML Panels
date: 2014-01-31 16:21:30
---

This is how Photoshop (i.e. the JSX) sends back data to the HTML Panel. The HTML Panel communicates with Photoshop via evalScript - this is in the `main.js`:

var csInterface = new CSInterface();
csInterface.evalScript('jsxFun()');

In the JSX there must be some:

function jsxFun() {
    // ...
}

In order to enable double-way communication, **provide a callback** in the evalScript function:

var csInterface = new CSInterface();
csInterface.evalScript('jsxFun()', callback); 
// callback is a function defined somewhere (or anonymous)

Example
-------

![JSX_2_HTML](http://localhost:8888/wp-content/uploads/2014/01/JSX_2_HTML.png)A very trivial example for a panel with one button that retrieves and displays in the panel the Photoshop active document's name (forgive my total lack of design for this panel - it's just to show you the bare mechanism) is as follows.

### HTML

I define a button with `id="btnDocName"`, and an `h3` tag which will contain the Doc's name.

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
        <h1>JSX -> HTML</h1>
        <h3 id="docName">&nbsp;</h3>
        <button id="btnDocName">Retrieve Document Name</button>
    </div>

    <script src="js/libs/CSInterface-4.0.0.js"></script>
    <script src="js/libs/jquery-2.0.2.min.js"></script>
    <script src="js/themeManager.js"></script>
    <script src="js/main.js"></script>

</body>
</html>

###  Main.js

Besides themeManager ([boilerplate code](http://davidderaedt.github.io/ccext-website/ "CC Extensibility Helpers") is by the great [David Deraedt](https://twitter.com/davidderaedt "David Deraedt on Twitter")) you see I evaluate (i.e. call) a `getDocName()` function (which belongs to the JSX) 've used an anonymous function to define a callback on the fly. The result argument is just the return value in the JSX, and it's used to overwrite the h3 tag content in the Panel.

(function () {
    'use strict';
    var csInterface = new CSInterface();
    function init() {
        themeManager.init();
        $("#btnDocName").click(function () {
            csInterface.evalScript('getDocName()', function(result) {
                document.getElementById("docName").innerHTML= result;
            });
        });
    }
    init();
}());

### JSX

Contains the getDocName() function and returns the active document name as a String (the `toString()` shouldn't be mandatory):

function getDocName(){
    return app.documents.length ? app.activeDocument.name : "No docs open!";
}

So as a result when you click the button on the HTML Panel, it evaluates the function in the JSX, which in turn passes its return value to the callback.