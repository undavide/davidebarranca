---
title: " ScriptUI Window in Photoshop - Palette vs. Dialog\t\t"
tags:
  - dialog
  - Extendscript
  - Javascript
  - palette
  - Photoshop @en
  - ScriptUI
  - waitForRedraw()
  - Window
url: 1290.html
id: 1290
comments: false
category:
  - Coding
  - ExtendScript / Javascript
date: 2012-10-28 01:50:22
---

ExtendScript - one of the scripting languages supported by Creative Suite applications, and the only one cross-platform - has several extra features compared to Javascript. Besides Filesystem access (a notable addition), there's the ScriptUI component: which allows you to add graphic user interfaces (GUI) to scripts. Photoshop support is dated to CS2, according to the _unofficial_ (yet essential) [ScriptUI for Dummies](http://www.kahrel.plus.com/indesign/scriptui.html "ScriptUI for Dummies") guide made by Peter Kahrel. As it happens often in scripting CS applications, the implementation of some feature is _version and platform dependent_ \- i.e. PS CS5 on PC is different from PS CS6 on Mac. In this post I'm going to focus on one particular aspect of ScriptUI, namely the Window object.

Two of a kind
-------------

Windows can be either **dialogs** or **palettes** (at least in Photoshop). They look and are substantially different.

### Palettes

Open the ExtendScript ToolKit (ESTK) and run the following code:

#target estoolkit
var win, windowResource;

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

win = new Window(windowResource);

win.bottomGroup.cancelButton.onClick = function() {
  return win.close();
};
win.bottomGroup.applyButton.onClick = function() {
  return win.close();
};

win.show();

The result (it runs in ESTK, since the first line is the preprocessing directive `#target estoolkit`) is as follows: ![ESTK palette](http://localhost:8888/wp-content/uploads/2012/10/ESTK_palette.png "ESTK palette") The above is a **palette**, a **non-modal** window. Something that lets you interact _either with the window itself_ (clicking buttons, dragging sliders, etc) _and with the host program_ \- here the ESTK (so you can select, say, menu items, other panels, etc.). It's the Javascript equivalent of Panels and third party Extensions - they show up and wait there, letting you work with Photoshop.

### Dialogs

In ESTK run the following code:

#target estoolkit
var win, windowResource;

windowResource = "dialog {  \
    orientation: 'column', \
    alignChildren: \['fill', 'top'\],  \
    preferredSize:\[300, 130\], \
    text: 'ScriptUI Window - dialog',  \
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

win = new Window(windowResource);

win.bottomGroup.cancelButton.onClick = function() {
  return win.close();
};
win.bottomGroup.applyButton.onClick = function() {
  return win.close();
};

win.show();

Compared to the first script, there's little difference (the windowResource string contains `dialog` instead of `palette`) and the Window looks like this: ![ESTK dialog](http://localhost:8888/wp-content/uploads/2012/10/ESTK_dialog.png "ESTK dialog") We're still within ESTK: the red, closing button is disappeared, the titlebar is bigger, but the main difference is that **dialogs** are **modal**: that is, the focus is on them and you _can not interact with the host application_. If you try to select, say, a menu item the program complains with a beep.

Photoshop support
-----------------

In the ScriptUI for Dummies guide it's asserted that Photoshop doesn't support palette windows. While the more hours I spend debugging unwanted behaviors the more I tend to agree with Peter Kahrel, I would personally express the statement as follows:

> Photoshop supports both dialog and palette Windows; while the former kind's behavior is consistent with other CS applications, palette implementation in PS is peculiar and version/platform dependent.

### PS dialogs implementation

It's basically what you'd expect - a modal window (which aspect depends on the PS version and platform). Yet the behavior is exactly the same no matter what is the combination, PC/Mac - CS5/CS6. In order to test it, just change the first line of the two scripts above:

#target photoshop

// etc.

  \[caption id="attachment_1295" align="aligncenter" width="1006"\][![PS dialogs](http://localhost:8888/wp-content/uploads/2012/10/PS_dialogs.png "PS dialogs")](http://localhost:8888/wp-content/uploads/2012/10/PS_dialogs.png) First row: Mac CS5, Mac CS6.  
Second row: PC CS5, PC CS6.\[/caption\] One thing to notice is that PC versions have a red X closing button, while Mac versions do not (for some reason, CS5 rendering of the slider bar is thicker compared to CS6).

### PS palettes implementation

Here things get "interesting". First, if you run the palette example with  `#target photoshop` you're not going to see very much. The Window that you expect to see just flashes briefly and disappear. This is what will happen to every Snp*.jsx examples provided alongside with the ESTK application, not just my code. The answer lies in the fact that palettes keep showing **only** if there's something going on in the script, **they can not stay idle** waiting for the user to do something like modal dialogs do.

var win, windowResource, i;

windowResource = "palette {  \    orientation: 'column', \    alignChildren: \['fill', 'top'\],  \    preferredSize:\[300, 130\], \    text: 'ScriptUI Window - dialog',  \    margins:15, \    \    sliderPanel: Panel { \        orientation: 'row', \        alignChildren: 'right', \        margins:15, \        text: ' PANEL ', \        st: StaticText { text: 'Value:' }, \        sl: Slider { minvalue: 1, maxvalue: 100, value: 30, size:\[220,20\] }, \        te: EditText { text: '30', characters: 5, justify: 'left'} \        } \    \    bottomGroup: Group{ \        cd: Checkbox { text:'Checkbox value', value: true }, \        cancelButton: Button { text: 'Cancel', properties:{name:'cancel'}, size: \[120,24\], alignment:\['right', 'center'\] }, \        applyButton: Button { text: 'Apply', properties:{name:'ok'}, size: \[120,24\], alignment:\['right', 'center'\] }, \    }\\}";
win = new Window(windowResource);
win.show();

app.documents.add(); // adds a new document
app.activeDocument.activeLayer.applyAddNoise (400, NoiseDistribution.GAUSSIAN, true) // applies Noise
for (i = 0; i < 3; i++) { // Blurs it 3 times
    app.activeDocument.activeLayer.applyGaussianBlur(1);
    $.sleep (2000); // waits 2 seconds
    app.refresh(); // refreshes PS
}

In the above example, after the `win.show()` the script is busy creating a new document, applying and blurring some noise just to kill some time. In the meantime, the palette window shows and keep showing; when the script is done with the task, the palette is automatically closed with no need of user interaction whatsoever, nor explicit `win.close()`. In order to keep Photoshop busy, I've found two working alternatives (because a straight `$.sleep()` loop makes PS quite non-responsive) involving the following functions:

// According to the PS Javascript Reference:
// "Pauses the script while the application refreshes. 
// Use to slow down execution and show the results to 
// the user as the script runs. 
// Use carefully; 
// your script runs much more slowly when using this method."
app.refresh();

// According to one example's comment in the 
// Photoshop Javascript Reference:
// "A helper function for debugging
// It also helps the user see what is going on"
var waitForRedraw = function() {
  var d;
  d = new ActionDescriptor();
  d.putEnumerated(app.stringIDtoTypeID('state'), app.stringIDtoTypeID('state'), app.stringIDtoTypeID('redrawComplete'));
  return executeAction(app.stringIDtoTypeID('wait'), d, DialogModes.NO);
};

The comments in the code should be self-explanatory. As far as I know, waitForRedraw() should work also in earlier PS versions - it's not documented, it's just used in one Reference's demo scripts.

### PS palette working example

One possible way to implement working palette Windows in Photoshop has the following logic:

var isDone, s2t, waitForRedraw, win, windowResource;

// Shortcut function
s2t = function(stringID) {
  return app.stringIDToTypeID(stringID);
};

waitForRedraw = function() {
  var d;
  d = new ActionDescriptor();
  d.putEnumerated(s2t('state'), s2t('state'), s2t('redrawComplete'));
  return executeAction(s2t('wait'), d, DialogModes.NO);
};

//sentinel variable
isDone = false;

windowResource = "palette {  \    orientation: 'column', \    alignChildren: \['fill', 'top'\],  \    preferredSize:\[300, 130\], \    text: 'ScriptUI Window - palette',  \    margins:15, \    \    sliderPanel: Panel { \        orientation: 'row', \        alignChildren: 'right', \        margins:15, \        text: ' PANEL ', \        st: StaticText { text: 'Value:' }, \        sl: Slider { minvalue: 1, maxvalue: 100, value: 30, size:\[220,20\] }, \        te: EditText { text: '30', characters: 5, justify: 'left'} \        } \    \    bottomGroup: Group{ \        cd: Checkbox { text:'Checkbox value', value: true }, \        cancelButton: Button { text: 'Cancel', properties:{name:'cancel'}, size: \[120,24\], alignment:\['right', 'center'\] }, \        applyButton: Button { text: 'Apply', properties:{name:'ok'}, size: \[120,24\], alignment:\['right', 'center'\] }, \    }\\}";

win = new Window(windowResource);

// Button listeners
win.bottomGroup.cancelButton.onClick = function() {
  return isDone = true;
};
win.bottomGroup.applyButton.onClick = function() {
  return isDone = true;
};

// don't forget this one!
win.onClose = function() {
  return isDone = true;
};

win.show();

while (isDone === false) {
  app.refresh(); // or, alternatively, waitForRedraw();
}

The key point is the `isDone` variable, which is set false at the beginning. After the Window is shown, a while loop keeps checking for this sentinel value and call `app.refresh()`, or alternatively `waitForRedraw()` in order to keep the palette showing. Instead of an explicit call to `win.close()`, it's a better option to attach to the buttons' `onClick()` functions a sentinel value's switch to true. This way the while loop breaks and the Window automatically closes. Mind you, it's crucial to set `isDone = true` even in the `win.onClose()` function, otherwise Photoshop will keep evaluating the variable even after the Window has been closed clicking the red dismiss button (see following screenshot) - resulting in some degree of non-responsiveness. \[caption id="attachment_1307" align="aligncenter" width="923"\][![PS palettes](http://localhost:8888/wp-content/uploads/2012/10/PS_palettes.png "PS palettes")](http://localhost:8888/wp-content/uploads/2012/10/PS_palettes.png) First row: Mac CS5, Mac CS6.  
Second row: PC CS5, PC CS6.\[/caption\] Few cosmetic differences are present this time too, including the position of the dismiss button (top-left for Mac, top-right for PC). This button is taken into account with the `onClose()` function as shown in the code example.

Platform specific behaviors (aka bugs)
--------------------------------------

Things are getting weird if you start to compare Mac and PC implementations of ScriptUI windows in Photoshop CS5 and CS6 (the tests on the PC side have been made on a virtualized Window7 system running in Parallels Desktop). It happens that, depending on how you mix platform, version and Window type, you end up with different behaviors (_modal_ and _non-modal_), as follows: \[caption id="attachment_1308" align="aligncenter" width="690"\][![Behavior Table](http://localhost:8888/wp-content/uploads/2012/10/BehaviorTable.png "Behavior Table")](http://localhost:8888/wp-content/uploads/2012/10/BehaviorTable.png) Comparison of the ScriptUI Window type's behavior (_modal_ or _non-modal_) against Photoshop version and platform.\[/caption\] While **dialog**'s behavior is nicely uniform between Mac and PC, CS5 and CS6; **palettes** are a fiasco. Mac behavior is always _non-modal_ (as it should be - palettes are non-modal by definition), while PC's one depends on the Photoshop version and function used. Basically, only CS5 with app.refresh() works as it's supposed to do, while in CS6 both functions fail. From my personal standpoint, this is a **bug** \- and I haven't found any workaround yet.

Conclusion
----------

To sum up:

*   Palettes are defined within Creative Suite applications as _non-modal_ Windows that can wait idle.
*   The Photoshop implementation misses entirely the "can wait idle" part - palettes keep showing only if there's some code running in background.
*   There's a workaround to make palettes behave as they're supposed to do, involving either `app.refresh()` or `waitForRedraw()` loops and a sentinel variable.
*   On Mac the workaround, no matter on CS5 and CS6 or what function is used, makes the palettes _non-modal_ (as they're supposed to be by definition)
*   On PC, only app.refresh() in CS5 returns a _non-modal_ Window - CS6 ones are always _modal_.

Which is quite a pain in the neck, especially if you need your palettes to work as they're supposed to do in a platform and version independent way.