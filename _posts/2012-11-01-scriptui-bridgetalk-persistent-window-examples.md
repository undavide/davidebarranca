---
id: 1316
title: 'ScriptUI &#8211; BridgeTalk persistent Window examples'
date: 2012-11-01T22:29:23+01:00
author: Davide Barranca
excerpt: "ScriptUI Windows can be tricky in Photoshop, especially if you want to create a non-modal, persistent and idle palette. While a couple of workarounds are possible, as I've shown in a previous post, there's a better alternative involving BridgeTalk."
layout: post
guid: http://www.davidebarranca.com/?p=1316
permalink: /2012/11/scriptui-bridgetalk-persistent-window-examples/
standard_seo_post_level_layout:
  - ""
standard_link_url_field:
  - ""
standard_seo_post_meta_description:
  - "BridgeTalk provides Photoshop with persistent, idle, non-modal ScriptUI 'palette' Windows that are truly platform independent (Mac/PC)."
image: /wp-content/uploads/2012/10/ESTK_palette.png
categories:
  - Coding
  - ExtendScript / Javascript
tags:
  - BridgeTalk
  - Extendscript
  - palette
  - ScriptUI
  - tutorial
  - Window
---
<div class="pf-content">
  <p>
    In a <a title="ScriptUI Window in Photoshop – Palette vs. Dialog" href="http://localhost:8888/2012/10/scriptui-window-in-photoshop-palette-vs-dialog/" target="_blank">previous post</a> I&#8217;ve shown how to use either <code>app.refresh()</code> or the <code>waitForRedraw()</code> function in a loop in order to keep alive and idle a <em>palette</em> Window in Photoshop (since, unlike other CS apps like InDesign or the ExtendScript ToolKit, aka ESTK itself, palettes&#8217; lifespan ends when there&#8217;s no more code to execute).
  </p>
  
  <p>
    I&#8217;ve recently found that BridgeTalk can provide a good alternative since, quoting Adobe&#8217;s <a title="Adobe Scripting Forum" href="http://forums.adobe.com/message/1115629#1115629" target="_blank">Bob Stucky</a>, it &#8220;<em>uses a persistent engine to process BT messages</em>&#8220;. I&#8217;ve had no luck following the forum&#8217;s snippet, so (thanks also to some code kindly made available by <a title="Save as Psd, save as Png and delete script" href="http://kasyan.ho.com.ua/save_psd_png_delete.html" target="_blank">Kasyan Servetsky</a>) I&#8217;ve ended up with working examples that I&#8217;m going to share here.
  </p>
  
  <h2>
    BridgeTalk basics
  </h2>
  
  <p>
    The Javascript Tools Guide (installed by default alongside the ESTK) is quite straightforward:
  </p>
  
  <blockquote>
    <p>
      The Adobe scripting environment provides an interapplication messaging framework, a way for to send and receive information and scripts from one Adobe application to another.
    </p>
  </blockquote>
  
  <p>
    The interesting part is that <strong>one app can message itself</strong>: that is, to be the sender and the receiver at the same time. The communication system includes a small set of cross-DOM function (general ones: open, print, close, etc. or application specific: photomerge, etc), but you can control an application in its native DOM language via <strong>messages</strong> deployed by means of <strong>BridgeTalk objects</strong>. A simple example is as follows:
  </p>
  
  <pre class="lang:js decode:true">// the BridgeTalk Object
var bt = new BridgeTalk();

// the communication target
bt.target = "photoshop";

// The script to be executed as a String
var message = "alert('Hello Photoshop')";

// assign to the object's body the message
bt.body = message;

// send the message to the target app
bt.send(); // an alert will pop-up in Photoshop</pre>
  
  <p>
    Things can be more sophisticated, but this main concept holds always  true. I&#8217;ll provide you with few examples, following different strategies.
  </p>
  
  <h2>
    Example #1 &#8211; Strings
  </h2>
  
  <p>
    Using a window resource string to create the &#8216;<em>palette</em>&#8216; and the <code>.toString()</code> function on the object.
  </p>
  
  <pre class="lang:js decode:true">#target photoshop
function WinObject() {
  // Long resource String for 'palette' Window
  var windowResource = "palette {          orientation: 'column',         alignChildren: ['fill', 'top'],          preferredSize:[300, 130],         text: 'ScriptUI Window - palette',          margins:15,                 sliderPanel: Panel {             orientation: 'row',             alignChildren: 'right',             margins:15,             text: ' PANEL ',             st: StaticText { text: 'Value:' },             sl: Slider { minvalue: 1, maxvalue: 100, value: 30, size:[220,20] },             te: EditText { text: '30', characters: 5, justify: 'left'}             }                 bottomGroup: Group{             cd: Checkbox { text:'Checkbox value', value: true },             cancelButton: Button { text: 'Cancel', properties:{name:'cancel'}, size: [120,24], alignment:['right', 'center'] },             applyButton: Button { text: 'Apply', properties:{name:'ok'}, size: [120,24], alignment:['right', 'center'] },         }    }";
  var win = new Window(windowResource);

  win.bottomGroup.cancelButton.onClick = function() { win.close() };
  win.bottomGroup.applyButton.onClick = function() { win.close() };

  // Show the Window
  win.show();
};

// String message for BridgeTalk
var message = WinObject.toString();

// construct an anonymous instance and add it to the string
message += "\nnew WinObject();"
// $.writeln(message); // check it in the ESTK Console, just in case

var bt = new BridgeTalk();
bt.target = "photoshop";
bt.body = message;
bt.send();</pre>
  
  <p>
    Everything&#8217;s been packed into the WinObject &#8211; which <code>.toString()</code> function exactly replicate in the message string (you must add there the call to create an anonymous instance, though).
  </p>
  
  <h3>
  </h3>
  
  <h2>
    Example #2 &#8211; Binary Strings
  </h2>
  
  <p>
    Let&#8217;s say we&#8217;ve this usual script:
  </p>
  
  <pre class="lang:js decode:true">function winObject() {
  // Long resource String for 'palette' Window
  var windowResource = "palette {          orientation: 'column',         alignChildren: ['fill', 'top'],          preferredSize:[300, 130],         text: 'ScriptUI Window - palette',          margins:15,                 sliderPanel: Panel {             orientation: 'row',             alignChildren: 'right',             margins:15,             text: ' PANEL ',             st: StaticText { text: 'Value:' },             sl: Slider { minvalue: 1, maxvalue: 100, value: 30, size:[220,20] },             te: EditText { text: '30', characters: 5, justify: 'left'}             }                 bottomGroup: Group{             cd: Checkbox { text:'Checkbox value', value: true },             cancelButton: Button { text: 'Cancel', properties:{name:'cancel'}, size: [120,24], alignment:['right', 'center'] },             applyButton: Button { text: 'Apply', properties:{name:'ok'}, size: [120,24], alignment:['right', 'center'] },         }    }";
  var win = new Window(windowResource);
  win.bottomGroup.cancelButton.onClick = function() { win.close() };
  win.bottomGroup.applyButton.onClick = function() { win.close() };
  win.show();
};
winObject()</pre>
  
  <p>
    In ESTK do <em>File &#8211; Export as Binary</em> in a temp file, which will look like:
  </p>
  
  <pre class="striped:false nums:false lang:default decode:true">@JSXBIN@ES@2.0@MyBbyBnABMAbyBn0AFJCnASzOjXjJjOjEjPjXiSjFjTjPjVjSjDjFBAne2kVDjQjB
jMjFjUjUjFhAjbhAhAhAhAhAhAhAhAhAhAjPjSjJjFjOjUjBjUjJjPjOhahAhHjDjPjMjVjNjOhHhMh
AhAhAhAhAhAhAhAhAjBjMjJjHjOiDjIjJjMjEjSjFjOhahAibhHjGjJjMjMhHhMhAhHjUjPjQhHidhM
hAhAhAhAhAhAhAhAhAhAjQjSjFjGjFjSjSjFjEiTjJjajFhaibhThQhQhMhAhRhThQidhMhAhAhAhAh
// ... etc. etc.</pre>
  
  <p>
    Add a backslash at each line&#8217;s end:
  </p>
  
  <pre class="striped:false nums:false lang:default decode:true">@JSXBIN@ES@2.0@MyBbyBnABMAbyBn0AFJCnASzOjXjJjOjEjPjXiSjFjTjPjVjSjDjFBAne2kVDjQjB\
jMjFjUjUjFhAjbhAhAhAhAhAhAhAhAhAhAjPjSjJjFjOjUjBjUjJjPjOhahAhHjDjPjMjVjNjOhHhMh\
AhAhAhAhAhAhAhAhAjBjMjJjHjOiDjIjJjMjEjSjFjOhahAibhHjGjJjMjMhHhMhAhHjUjPjQhHidhM\
hAhAhAhAhAhAhAhAhAhAjQjSjFjGjFjSjSjFjEiTjJjajFhaibhThQhQhMhAhRhThQidhMhAhAhAhAh\
// ... etc. etc.</pre>
  
  <p>
    and use it as a message String for the BridgeTalk body:
  </p>
  
  <pre class="lang:default decode:true">var message = "@JSXBIN@ES@2.0@MyBbyBnABMAbyBn0AFJCnASzOjXjJjOjEjPjXiSjFjTjPjVjSjDjFBAne2kVDjQjB\
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
bt.send();</pre>
  
  <p>
    This way you&#8217;re able to pack different stuff &#8211; an entire script &#8211; into the string.
  </p>
  
  <h2>
     Example #3 &#8211; External scripts
  </h2>
  
  <p>
    You can load as a string an external file (either binary or not). In the following code, both files are in the same folder:
  </p>
  
  <pre class="lang:default decode:true">var currentPath = (new File($.fileName)).path // retrieve the current script path
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
bt.send();</pre>
  
  <h2>
    Caveats
  </h2>
  
  <ul>
    <li>
      For some reason that I miss, if you don&#8217;t have any button&#8217;s callback (or at least a <code>window.onClose()</code> function, even an empty one) in the Window, the palette won&#8217;t display.
    </li>
    <li>
      Be aware that window resource strings can&#8217;t contain backslashes <code>\</code> as you&#8217;d write normally (see the code below). This applies to images embedded as strings as well &#8211; they don&#8217;t return any error yet prevent the window from displaying.
    </li>
  </ul>
  
  <pre>windowResource = "palette {  \
        orientation: 'column', \
        alignChildren: ['fill', 'top'],  \
        preferredSize:[300, 130], \
        text: 'ScriptUI Window - palette',  \
        margins:15, \
        \
        sliderPanel: Panel { \
            orientation: 'row', \
            alignChildren: 'right', \
            margins:15, \
            text: ' PANEL ', \
            st: StaticText { text: 'Value:' }, \
            sl: Slider { minvalue: 1, maxvalue: 100, value: 30, size:[220,20] }, \
            te: EditText { text: '30', characters: 5, justify: 'left'} \
            } \
        \
        bottomGroup: Group{ \
            cd: Checkbox { text:'Checkbox value', value: true }, \
            cancelButton: Button { text: 'Cancel', properties:{name:'cancel'}, size: [120,24], alignment:['right', 'center'] }, \
            applyButton: Button { text: 'Apply', properties:{name:'ok'}, size: [120,24], alignment:['right', 'center'] }, \
        }\
    }"</pre>
  
  <ul>
    <li>
       The way you write a function results in different <code>.toString()</code> outputs, for instance:
    </li>
  </ul>
  
  <pre>// Function Declaration
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
// function baz() { alert("baz") }</pre>
  
  <p>
    When using Function Declaration / Named function the name is correctly parsed, while in Function Expressions the function assigned to the variable is anonymous and can&#8217;t return its name (<a title="Coffeescript" href="http://www.coffeescript.org" target="_blank">Coffeescript</a> for instance uses Function Expressions by default):
  </p>
  
  <pre>var qux = function() { alert("qux") }
$.writeln(qux.name); // anonymous</pre>
  
  <p>
    As a consequence, you&#8217;ve to explicitly make a correct message:
  </p>
  
  <pre>// custom function
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
bt.send();</pre>
  
  <p>
    Following the above pattern, things get slightly more verbose if you need prototypes.
  </p>
  
  <h2>
    Conclusions
  </h2>
  
  <p>
    The BridgeTalk generated &#8216;<em>palette</em>&#8216; Window is finally a true, non-modal, idle Window object in Photoshop, no matter whether PC or Mac (the workarounds I&#8217;ve shown in a <a title="ScriptUI Window in Photoshop – Palette vs. Dialog" href="http://localhost:8888/2012/10/scriptui-window-in-photoshop-palette-vs-dialog/" target="_blank">previous post</a> work only partially on PC).
  </p>
  
  <p>
    My suggestion, if you need such a feature, is to implement in an early stage of the script coding the BridgeTalk communication structure &#8211; since in a quite complex project I&#8217;m spending my time on (which started way before I knew about this technique), I&#8217;m still unable to make it work. The debugging is&#8230; quite complicate!
  </p>
</div>

<!-- Share-Widget Button BEGIN --><a href="javascript:void(0);" myshare\_id="mys\_shareit" myshare\_url="http://localhost:8888/2012/11/scriptui-bridgetalk-persistent-window-examples/" myshare\_title="ScriptUI &#8211; BridgeTalk persistent Window examples" rel="nofollow" onclick=" return false;" style="text-decoration:none; color:#000000; font-size:11px; line-height:20px;"> 

<img src="http://localhost:8888/wp-content/plugins/share-widget/img/share-button-white-small.png" height="20" alt="Share" style="border:0" /> </a> <!-- Share-Widget Button END -->