---
title: " ScriptUI - BridgeTalk persistent Window examples\t\t"
tags:
  - BridgeTalk
  - Extendscript
  - palette
  - ScriptUI
  - tutorial
  - Window
url: 1316.html
id: 1316
category:
  - Coding
  - ExtendScript / Javascript
date: 2012-11-01 22:29:23
---

In a [previous post](http://localhost:8888/2012/10/scriptui-window-in-photoshop-palette-vs-dialog/ "ScriptUI Window in Photoshop – Palette vs. Dialog") I've shown how to use either `app.refresh()` or the `waitForRedraw()` function in a loop in order to keep alive and idle a _palette_ Window in Photoshop (since, unlike other CS apps like InDesign or the ExtendScript ToolKit, aka ESTK itself, palettes' lifespan ends when there's no more code to execute). I've recently found that BridgeTalk can provide a good alternative since, quoting Adobe's [Bob Stucky](http://forums.adobe.com/message/1115629#1115629 "Adobe Scripting Forum"), it "_uses a persistent engine to process BT messages_". I've had no luck following the forum's snippet, so (thanks also to some code kindly made available by [Kasyan Servetsky](http://kasyan.ho.com.ua/save_psd_png_delete.html "Save as Psd, save as Png and delete script")) I've ended up with working examples that I'm going to share here.

BridgeTalk basics
-----------------

The Javascript Tools Guide (installed by default alongside the ESTK) is quite straightforward:

> The Adobe scripting environment provides an interapplication messaging framework, a way for to send and receive information and scripts from one Adobe application to another.

The interesting part is that **one app can message itself**: that is, to be the sender and the receiver at the same time. The communication system includes a small set of cross-DOM function (general ones: open, print, close, etc. or application specific: photomerge, etc), but you can control an application in its native DOM language via **messages** deployed by means of **BridgeTalk objects**. A simple example is as follows:

// the BridgeTalk Object
var bt = new BridgeTalk();

// the communication target
bt.target = "photoshop";

// The script to be executed as a String
var message = "alert('Hello Photoshop')";

// assign to the object's body the message
bt.body = message;

// send the message to the target app
bt.send(); // an alert will pop-up in Photoshop

Things can be more sophisticated, but this main concept holds always  true. I'll provide you with few examples, following different strategies.

Example #1 - Strings
--------------------

Using a window resource string to create the '_palette_' and the `.toString()` function on the object.

#target photoshop
function WinObject() {
  // Long resource String for 'palette' Window
  var windowResource = "palette {          orientation: 'column',         alignChildren: \['fill', 'top'\],          preferredSize:\[300, 130\],         text: 'ScriptUI Window - palette',          margins:15,                 sliderPanel: Panel {             orientation: 'row',             alignChildren: 'right',             margins:15,             text: ' PANEL ',             st: StaticText { text: 'Value:' },             sl: Slider { minvalue: 1, maxvalue: 100, value: 30, size:\[220,20\] },             te: EditText { text: '30', characters: 5, justify: 'left'}             }                 bottomGroup: Group{             cd: Checkbox { text:'Checkbox value', value: true },             cancelButton: Button { text: 'Cancel', properties:{name:'cancel'}, size: \[120,24\], alignment:\['right', 'center'\] },             applyButton: Button { text: 'Apply', properties:{name:'ok'}, size: \[120,24\], alignment:\['right', 'center'\] },         }    }";
  var win = new Window(windowResource);

  win.bottomGroup.cancelButton.onClick = function() { win.close() };
  win.bottomGroup.applyButton.onClick = function() { win.close() };

  // Show the Window
  win.show();
};

// String message for BridgeTalk
var message = WinObject.toString();

// construct an anonymous instance and add it to the string
message += "\\nnew WinObject();"
// $.writeln(message); // check it in the ESTK Console, just in case

var bt = new BridgeTalk();
bt.target = "photoshop";
bt.body = message;
bt.send();

Everything's been packed into the WinObject - which `.toString()` function exactly replicate in the message string (you must add there the call to create an anonymous instance, though).

Example #2 - Binary Strings
---------------------------

Let's say we've this usual script:

function winObject() {
  // Long resource String for 'palette' Window
  var windowResource = "palette {          orientation: 'column',         alignChildren: \['fill', 'top'\],          preferredSize:\[300, 130\],         text: 'ScriptUI Window - palette',          margins:15,                 sliderPanel: Panel {             orientation: 'row',             alignChildren: 'right',             margins:15,             text: ' PANEL ',             st: StaticText { text: 'Value:' },             sl: Slider { minvalue: 1, maxvalue: 100, value: 30, size:\[220,20\] },             te: EditText { text: '30', characters: 5, justify: 'left'}             }                 bottomGroup: Group{             cd: Checkbox { text:'Checkbox value', value: true },             cancelButton: Button { text: 'Cancel', properties:{name:'cancel'}, size: \[120,24\], alignment:\['right', 'center'\] },             applyButton: Button { text: 'Apply', properties:{name:'ok'}, size: \[120,24\], alignment:\['right', 'center'\] },         }    }";
  var win = new Window(windowResource);
  win.bottomGroup.cancelButton.onClick = function() { win.close() };
  win.bottomGroup.applyButton.onClick = function() { win.close() };
  win.show();
};
winObject()

In ESTK do _File - Export as Binary_ in a temp file, which will look like:

@JSXBIN@ES@2.0@MyBbyBnABMAbyBn0AFJCnASzOjXjJjOjEjPjXiSjFjTjPjVjSjDjFBAne2kVDjQjB
jMjFjUjUjFhAjbhAhAhAhAhAhAhAhAhAhAjPjSjJjFjOjUjBjUjJjPjOhahAhHjDjPjMjVjNjOhHhMh
AhAhAhAhAhAhAhAhAjBjMjJjHjOiDjIjJjMjEjSjFjOhahAibhHjGjJjMjMhHhMhAhHjUjPjQhHidhM
hAhAhAhAhAhAhAhAhAhAjQjSjFjGjFjSjSjFjEiTjJjajFhaibhThQhQhMhAhRhThQidhMhAhAhAhAh
// ... etc. etc.

Add a backslash at each line's end:

@JSXBIN@ES@2.0@MyBbyBnABMAbyBn0AFJCnASzOjXjJjOjEjPjXiSjFjTjPjVjSjDjFBAne2kVDjQjB\
jMjFjUjUjFhAjbhAhAhAhAhAhAhAhAhAhAjPjSjJjFjOjUjBjUjJjPjOhahAhHjDjPjMjVjNjOhHhMh\
AhAhAhAhAhAhAhAhAjBjMjJjHjOiDjIjJjMjEjSjFjOhahAibhHjGjJjMjMhHhMhAhHjUjPjQhHidhM\
hAhAhAhAhAhAhAhAhAhAjQjSjFjGjFjSjSjFjEiTjJjajFhaibhThQhQhMhAhRhThQidhMhAhAhAhAh\
// ... etc. etc.

and use it as a message String for the BridgeTalk body:

var message = "@JSXBIN@ES@2.0@MyBbyBnABMAbyBn0AFJCnASzOjXjJjOjEjPjXiSjFjTjPjVjSjDjFBAne2kVDjQjB\
jMjFjUjUjFhAjbhAhAhAhAhAhAhAhAhAhAjPjSjJjFjOjUjBjUjJjPjOhahAhHjDjPjMjVjNjOhHhMh\
AhAhAhAhAhAhAhAhAjBjMjJjHjOiDjIjJjMjEjSjFjOhahAibhHjGjJjMjMhHhMhAhHjUjPjQhHidhM\
// ...
// ... etc. etc. etc.
// ...
EnAEXzFjDjMjPjTjFHfjCfnf0DzAICEnfJFnABXEfXzLjBjQjQjMjZiCjVjUjUjPjOJfXGfVCfBNyBn\
AMFbyBn0ABJFnAEXHfjCfnf0DICFnfJGnAEXzEjTjIjPjXKfVCfBnfACB40BiAC4B0AiAACAzJjXjJj\
OiPjCjKjFjDjULAHBJInAEjLfnf0DIByB";

var bt = new BridgeTalk();
bt.target = "photoshop";
bt.body = message;
bt.send();

This way you're able to pack different stuff - an entire script - into the string.

 Example #3 - External scripts
------------------------------

You can load as a string an external file (either binary or not). In the following code, both files are in the same folder:

var currentPath = (new File($.fileName)).path // retrieve the current script path
var scriptToLoad = new File (currentPath + "/paletteWindow.jsx") // the script to load
try {
    if (!scriptToLoad.exists) { throw new Error("script not found!"); }
    scriptToLoad.open ("r"); // read only
    var message = scriptToLoad.read();
    scriptToLoad.close()
} catch (error) {
    alert("Error parsing the file: " + error.description);
}
var bt = new BridgeTalk();
bt.target = "photoshop";
bt.body = message;
bt.send();

Caveats
-------

*   For some reason that I miss, if you don't have any button's callback (or at least a `window.onClose()` function, even an empty one) in the Window, the palette won't display.
*   Be aware that window resource strings can't contain backslashes `\` as you'd write normally (see the code below). This applies to images embedded as strings as well - they don't return any error yet prevent the window from displaying.

windowResource = "palette {  \
        orientation: 'column', \
        alignChildren: \['fill', 'top'\],  \
        preferredSize:\[300, 130\], \
        text: 'ScriptUI Window - palette',  \
        margins:15, \
        \
        sliderPanel: Panel { \
            orientation: 'row', \
            alignChildren: 'right', \
            margins:15, \
            text: ' PANEL ', \
            st: StaticText { text: 'Value:' }, \
            sl: Slider { minvalue: 1, maxvalue: 100, value: 30, size:\[220,20\] }, \
            te: EditText { text: '30', characters: 5, justify: 'left'} \
            } \
        \
        bottomGroup: Group{ \
            cd: Checkbox { text:'Checkbox value', value: true }, \
            cancelButton: Button { text: 'Cancel', properties:{name:'cancel'}, size: \[120,24\], alignment:\['right', 'center'\] }, \
            applyButton: Button { text: 'Apply', properties:{name:'ok'}, size: \[120,24\], alignment:\['right', 'center'\] }, \
        }\
    }"

*    The way you write a function results in different `.toString()` outputs, for instance:

// Function Declaration
function foo()  { alert("foo") }
foo.toString(); 
// function foo()  { alert("foo") }
foo.toSource();
// (function foo()  { alert("foo") })

// Function Expression
var bar = function() { alert("bar") }
bar.toString(); 
// function () { alert("bar") } 
bar.toSource(); 
// (function() { alert("bar") })

// Named Function
var baz = function baz() { alert("baz") }
baz.toString();
// function baz() { alert("baz") }

When using Function Declaration / Named function the name is correctly parsed, while in Function Expressions the function assigned to the variable is anonymous and can't return its name ([Coffeescript](http://www.coffeescript.org "Coffeescript") for instance uses Function Expressions by default):

var qux = function() { alert("qux") }
$.writeln(qux.name); // anonymous

As a consequence, you've to explicitly make a correct message:

// custom function
// the parameter is a String
var stringifyFunction = function (fn) {
  var functionBody = eval(fn); 
  return fn + " = " + functionBody;
}

// the Function to send
var foo = function () { alert("foo") }

// BridgeTalk obj
var bt = new BridgeTalk();
bt.target = "photoshop";
var message = stringifyFunction ("foo"); // "foo" as a String
message += "foo()"; // to call it
bt.body = message;
// $.writeln(message)
bt.send();

Following the above pattern, things get slightly more verbose if you need prototypes.

Conclusions
-----------

The BridgeTalk generated '_palette_' Window is finally a true, non-modal, idle Window object in Photoshop, no matter whether PC or Mac (the workarounds I've shown in a [previous post](http://localhost:8888/2012/10/scriptui-window-in-photoshop-palette-vs-dialog/ "ScriptUI Window in Photoshop – Palette vs. Dialog") work only partially on PC). My suggestion, if you need such a feature, is to implement in an early stage of the script coding the BridgeTalk communication structure - since in a quite complex project I'm spending my time on (which started way before I knew about this technique), I'm still unable to make it work. The debugging is... quite complicate!