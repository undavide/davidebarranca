---
title: " HTML Panels Tips: #6 integrating Topcoat CSS\t\t"
tags:
  - html panels
  - theme
  - tip
  - Topcoat
url: 2474.html
id: 2474
category:
  - Coding
  - HTML Panels
date: 2014-02-02 16:27:01
---

Today's tip is about integration of the Topcoat CSS library with themeManager.js in Photoshop HTML Panels. Adobe's [Topcoat](http://topcoat.io "Topcoat") is:

> a brand new open source CSS library designed to help developers build web apps with an emphasis on speed. It evolved from the Adobe design language developed for Brackets, Edge Reflow, and feedback from the PhoneGap app developer community.

It comes with [Desktop](http://topcoat.io/topcoat/index.html "Topcoat Desktop theme") and [Mobile](http://topcoat.io/topcoat/topcoat-mobile-dark.html "Topcoat Mobile theme") themes, both in Light of Dark shades - a really neat design. HTML Panels have _sort of_ a built-in system to keep in sync with Photoshop interface (four shades, from light gray to black). I've said "sort of" because you need to explicitly set the functions, but they're provided by default by both Extension Builder and [boilerplate code](http://davidderaedt.github.io/ccext-website/ "CC Extensibility Helpers") by [David Deraedt](https://twitter.com/davidderaedt "David Deraedt on Twitter"). Basically it's a matter of triggering some styling when the PS Interface changes - you do this listening for the `CSInterface.THEME_COLOR_CHANGED_EVENT`:

/\* themeManager.js */

function updateThemeWithAppSkinInfo(appSkinInfo) { 
  // ... All styling stuff here
}

function onAppThemeColorChanged(event) {
  var skinInfo = JSON.parse(window.\_\_adobe\_cep__.getHostEnvironment()).appSkinInfo;
  updateThemeWithAppSkinInfo(skinInfo);
}

function init() {
  var csInterface = new CSInterface();
  updateThemeWithAppSkinInfo(csInterface.hostEnvironment.appSkinInfo);
  csInterface.addEventListener(CSInterface.THEME\_COLOR\_CHANGED_EVENT, onAppThemeColorChanged);
}

\[caption id="attachment_2475" align="alignright" width="277"\]![hostObject](http://localhost:8888/wp-content/uploads/2014/02/hostObject.png) AppSkinInfo Object\[/caption\] The above code first runs `updateThemeWithAppSkinInfo()`, (the function that performs the actual styling - changes CSS rules, etc)  passing as a parameter an `appSkinInfo` object. It contains various host environment (i.e. Photoshop) information, as the screenshot from the Chrome Developer Tools console shows on the right. Depending on these values, you might decide for instance to change the background of your HTML Panel to match the Photoshop own `panelBackgroundColor` (or font size, color, whatever). Mind you: Topcoat already comes in two flavors (Light and Dark) so you can always decide to switch to the corresponding CSS depending on the Photoshop's interface color. You'd pick Topcoat's Light for the PS LigthGray and MidGray, and Topcoat's Dark for the PS DarkGray and Black interface. I personally don't like this option. Topcoat's background color doesn't match with Photoshop (I admit I'm a bit picky on this - after all we're adapting a library that's not been built for this very purpose). Let's call 1, 2, 3, 4 the PS interface shades: Topcoat Dark is too light for 1 and too dark for 2, while Topcoat Light is too light for 3 and too dark for 4. I'll show you in the following example how to better integrate the two systems.

Example
-------

As I'm compiling these tips while I discover new things working on actual projects, so I'll show you as the example a simple panel I'm currently porting to HTML (called [PS Projects](http://localhost:8888/2013/10/introducing-ps-projects-for-photoshop-cc-cs6/ "‘PS Projects’ for Photoshop CC/CS6")).

### HTML

The code here is really basic - I don't need anything fancy but three buttons: I've set a **Large Button Bar** using a snippet from Topcoat examples. As you can see, I've set IDs for the stylesheets - the reason is going to be clear soon.

<!doctype html>
<html>
<head>
<meta charset="utf-8">
<link id="hostStyle" rel="stylesheet" href="css/theme.css"/>
<link id="theme" rel="stylesheet" href="css/light.css"/>
<title></title>
</head>
<body>
	<h3 class="center">PS Projects 2.0</h3>
	<div class="topcoat-button-bar" style="margin-left:20px">
		<div class="topcoat-button-bar__item">
			<button id="create" class="topcoat-button-bar__button--large">Create New</button>
		</div>
		<div class="topcoat-button-bar__item">
			<button id="load" class="topcoat-button-bar__button--large">Load...</button>
		</div>
		<div class="topcoat-button-bar__item">
			<button id="modify" class="topcoat-button-bar__button--large">Modify...</button>
		</div>
	</div>          

	<script src="js/libs/CSInterface-4.0.0.js"></script>
	<script src="js/libs/jquery-2.0.2.min.js"></script>
	<script src="js/themeManager.js"></script>
	<script src="js/main.js"></script>

</body>
</html>

### Javascript

The main.js contains (besides stuff I need for JSX operations - the actual routines of my panel) a call to `themeManager.init()`. The `themeManager.js` is as follows:

var themeManager = (function () {
  'use strict'; 

  /\* Convert the Color object to string in hexadecimal format; */
  function toHex(color, delta) {
    function computeValue(value, delta) {
      var computedValue = !isNaN(delta) ? value + delta : value;
      if (computedValue < 0) {
        computedValue = 0;
      } else if (computedValue > 255) {
        computedValue = 255;
      }            
      computedValue = Math.floor(computedValue);
      computedValue = computedValue.toString(16);
      return computedValue.length === 1 ? "0" + computedValue : computedValue;
    } 
    var hex = "";
    if (color) {
      hex = computeValue(color.red, delta) + computeValue(color.green, delta) + computeValue(color.blue, delta);
    }
    return hex;
  }

  function addRule(stylesheetId, selector, rule) {
    var stylesheet = document.getElementById(stylesheetId);   
    if (stylesheet) {
      stylesheet = stylesheet.sheet;
      if (stylesheet.addRule) {
        stylesheet.addRule(selector, rule);
      } else if (stylesheet.insertRule) {
        stylesheet.insertRule(selector + ' { ' + rule + ' }', stylesheet.cssRules.length);
      }
    }
  }

  /\* Update the theme with the AppSkinInfo retrieved from the host product. */
  function updateThemeWithAppSkinInfo(appSkinInfo) {
    // console.log(appSkinInfo)
    var panelBgColor = appSkinInfo.panelBackgroundColor.color;
    var bgdColor = toHex(panelBgColor);
    var fontColor = "F0F0F0";
    if (panelBgColor.red > 122) {
      fontColor = "000000";
    }

    var styleId = "hostStyle";
    addRule(styleId, "body", "background-color:" + "#" + bgdColor);
    addRule(styleId, "body", "color:" + "#" + fontColor);

    var isLight = appSkinInfo.panelBackgroundColor.color.red >= 127;
    if (isLight) {
      $("#theme").attr("href", "css/light.css");
    } else {
      $("#theme").attr("href", "css/dark.css");
    }
  }

  function onAppThemeColorChanged(event) {
    var skinInfo = JSON.parse(window.\_\_adobe\_cep__.getHostEnvironment()).appSkinInfo;
    updateThemeWithAppSkinInfo(skinInfo);
  }

  function init() {   
    var csInterface = new CSInterface();
    updateThemeWithAppSkinInfo(csInterface.hostEnvironment.appSkinInfo);
    csInterface.addEventListener(CSInterface.THEME\_COLOR\_CHANGED_EVENT, onAppThemeColorChanged);
  }

  return {
    init: init
  };

}());

The `toHex()` and `addRule()` are utility functions, while `init()` (orders the styling update and adds a theme change event listener) and `onAppThemeColorChanged()` (retrieves the AppSkinInfo object and pass it forwards) are the same as in the first code chunk, up in the page. `updateThemeWithAppSkinInfo()` extracts from the `AppSkinInfo` object the panels background color and set new `background-color` and `color` rules in the stylesheet with `id="hostStyle"`. Then, depending on the Red component of the background color (an arbitrary choice) it determines whether the interface `isLight` or not, and load a dark or light Topcoat CSS using jQuery.

### CSS

The HTML loads two CSS files, even if the panel deals with a total of three. The one with `id="hostStyle"` is **theme.css** and contains  minor tweaks - it's the target of the `addRule()` function, which will add there `background-color` and `color` rules depending on the PS interface theme (lightGray, darkGray, etc). Then there's `dark.css` and `light.css` \- which are the original `topcoat-desktop-dark.css` and `topcoat-desktop-light.css` renamed. In order not to conflict with styles set via `themeManager.js` I've commented out some bits in the body section of both of them:

/\* dark.css and light.css */
// ...
body {
  margin: 0;
  padding: 0;
  /*background: #4b4d4e;
  color: #000;*/
  font: 16px "Source Sans", helvetica, arial, sans-serif;
  font-weight: 400;
}
// ...

As a result I've made the two Topcoat Dark and Light versions synchronize with the exact background-color and color. ![Topcoat](http://localhost:8888/wp-content/uploads/2014/02/Topcoat.png)