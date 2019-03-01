---
id: 1290
title: 'ScriptUI Window in Photoshop &#8211; Palette vs. Dialog'
date: 2012-10-28T01:50:22+01:00
author: Davide Barranca
excerpt: "Photoshop is a bit picky when it comes to ScriptUI Window elements - compared to other Creative Suite applications, 'palette' and 'dialog' work in a peculiar way and their behavior is platform / version dependent - alas."
layout: post
guid: http://www.davidebarranca.com/?p=1290
permalink: /2012/10/scriptui-window-in-photoshop-palette-vs-dialog/
standard_seo_post_level_layout:
  - ""
standard_link_url_field:
  - ""
standard_seo_post_meta_description:
  - "ExtendScript lets you build GUI in CS Applications yet Photoshop implementation's tricky and requires different platform/version workarounds"
image: /wp-content/uploads/2012/10/PS_palettes.png
categories:
  - Coding
  - ExtendScript / Javascript
tags:
  - dialog
  - Extendscript
  - Javascript
  - palette
  - Photoshop @en
  - ScriptUI
  - waitForRedraw()
  - Window
---
<div class="pf-content">
  <p>
    ExtendScript &#8211; one of the scripting languages supported by Creative Suite applications, and the only one cross-platform &#8211; has several extra features compared to Javascript. Besides Filesystem access (a notable addition), there&#8217;s the ScriptUI component: which allows you to add graphic user interfaces (GUI) to scripts.
  </p>
  
  <p>
    Photoshop support is dated to CS2, according to the <em>unofficial</em> (yet essential) <a title="ScriptUI for Dummies" href="http://www.kahrel.plus.com/indesign/scriptui.html" target="_blank">ScriptUI for Dummies</a> guide made by Peter Kahrel. As it happens often in scripting CS applications, the implementation of some feature is <em>version and platform dependent</em> &#8211; i.e. PS CS5 on PC is different from PS CS6 on Mac. In this post I&#8217;m going to focus on one particular aspect of ScriptUI, namely the Window object.
  </p>
  
  <h2>
    Two of a kind
  </h2>
  
  <p>
    Windows can be either <strong>dialogs</strong> or <strong>palettes</strong> (at least in Photoshop). They look and are substantially different.
  </p>
  
  <h3>
    Palettes
  </h3>
  
  <p>
    Open the ExtendScript ToolKit (ESTK) and run the following code:
  </p>
  
  <pre class="lang:js decode:true">#target estoolkit
var win, windowResource;

windowResource = "palette {  \
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
}"

win = new Window(windowResource);

win.bottomGroup.cancelButton.onClick = function() {
  return win.close();
};
win.bottomGroup.applyButton.onClick = function() {
  return win.close();
};

win.show();</pre>
  
  <p>
    The result (it runs in ESTK, since the first line is the preprocessing directive <code>#target estoolkit</code>) is as follows:
  </p>
  
  <p>
    <img class="aligncenter size-full wp-image-1293" title="ESTK palette" src="http://localhost:8888/wp-content/uploads/2012/10/ESTK_palette.png" alt="ESTK palette" width="409" height="160" srcset="http://localhost:8888/wp-content/uploads/2012/10/ESTK_palette.png 409w, http://localhost:8888/wp-content/uploads/2012/10/ESTK_palette-150x58.png 150w, http://localhost:8888/wp-content/uploads/2012/10/ESTK_palette-300x117.png 300w" sizes="(max-width: 409px) 100vw, 409px" />
  </p>
  
  <p>
    The above is a <strong>palette</strong>, a <strong>non-modal</strong> window. Something that lets you interact <em>either with the window itself</em> (clicking buttons, dragging sliders, etc) <em>and with the host program</em> &#8211; here the ESTK (so you can select, say, menu items, other panels, etc.). It&#8217;s the Javascript equivalent of Panels and third party Extensions &#8211; they show up and wait there, letting you work with Photoshop.
  </p>
  
  <h3>
    Dialogs
  </h3>
  
  <p>
    In ESTK run the following code:
  </p>
  
  <pre class="height-set:true height:300 lang:default mark:4 decode:true">#target estoolkit
var win, windowResource;

windowResource = "dialog {  \
    orientation: 'column', \
    alignChildren: ['fill', 'top'],  \
    preferredSize:[300, 130], \
    text: 'ScriptUI Window - dialog',  \
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
}"

win = new Window(windowResource);

win.bottomGroup.cancelButton.onClick = function() {
  return win.close();
};
win.bottomGroup.applyButton.onClick = function() {
  return win.close();
};

win.show();</pre>
  
  <p>
    Compared to the first script, there&#8217;s little difference (the windowResource string contains <code>dialog</code> instead of <code>palette</code>) and the Window looks like this:
  </p>
  
  <p>
    <img class="aligncenter size-full wp-image-1294" title="ESTK dialog" src="http://localhost:8888/wp-content/uploads/2012/10/ESTK_dialog.png" alt="ESTK dialog" width="509" height="266" srcset="http://localhost:8888/wp-content/uploads/2012/10/ESTK_dialog.png 509w, http://localhost:8888/wp-content/uploads/2012/10/ESTK_dialog-150x78.png 150w, http://localhost:8888/wp-content/uploads/2012/10/ESTK_dialog-300x156.png 300w" sizes="(max-width: 509px) 100vw, 509px" />
  </p>
  
  <p>
    We&#8217;re still within ESTK: the red, closing button is disappeared, the titlebar is bigger, but the main difference is that <strong>dialogs</strong> are <strong>modal</strong>: that is, the focus is on them and you <em>can not interact with the host application</em>. If you try to select, say, a menu item the program complains with a beep.
  </p>
  
  <h2>
    Photoshop support
  </h2>
  
  <p>
    In the ScriptUI for Dummies guide it&#8217;s asserted that Photoshop doesn&#8217;t support palette windows. While the more hours I spend debugging unwanted behaviors the more I tend to agree with Peter Kahrel, I would personally express the statement as follows:
  </p>
  
  <blockquote>
    <p>
      Photoshop supports both dialog and palette Windows; while the former kind&#8217;s behavior is consistent with other CS applications, palette implementation in PS is peculiar and version/platform dependent.
    </p>
  </blockquote>
  
  <h3>
    PS dialogs implementation
  </h3>
  
  <p>
    It&#8217;s basically what you&#8217;d expect &#8211; a modal window (which aspect depends on the PS version and platform). Yet the behavior is exactly the same no matter what is the combination, PC/Mac &#8211; CS5/CS6. In order to test it, just change the first line of the two scripts above:
  </p>
  
  <pre class="lang:js decode:true">#target photoshop

// etc.</pre>
  
  <p>
    &nbsp;
  </p>
  
  <div id="attachment_1295" style="width: 1016px" class="wp-caption aligncenter">
    <a href="http://localhost:8888/wp-content/uploads/2012/10/PS_dialogs.png" target="_blank"><img aria-describedby="caption-attachment-1295" class="size-full wp-image-1295 " title="PS dialogs" src="http://localhost:8888/wp-content/uploads/2012/10/PS_dialogs.png" alt="PS dialogs" width="1006" height="435" srcset="http://localhost:8888/wp-content/uploads/2012/10/PS_dialogs.png 1006w, http://localhost:8888/wp-content/uploads/2012/10/PS_dialogs-150x64.png 150w, http://localhost:8888/wp-content/uploads/2012/10/PS_dialogs-300x129.png 300w" sizes="(max-width: 1006px) 100vw, 1006px" /></a>
    
    <p id="caption-attachment-1295" class="wp-caption-text">
      First row: Mac CS5, Mac CS6.<br />Second row: PC CS5, PC CS6.
    </p>
  </div>
  
  <p>
    One thing to notice is that PC versions have a red X closing button, while Mac versions do not (for some reason, CS5 rendering of the slider bar is thicker compared to CS6).
  </p>
  
  <h3>
    PS palettes implementation
  </h3>
  
  <p>
    Here things get &#8220;interesting&#8221;. First, if you run the palette example with  <code>#target photoshop</code> you&#8217;re not going to see very much. The Window that you expect to see just flashes briefly and disappear. This is what will happen to every Snp*.jsx examples provided alongside with the ESTK application, not just my code.
  </p>
  
  <p>
    The answer lies in the fact that palettes keep showing <strong>only</strong> if there&#8217;s something going on in the script, <strong>they can not stay idle</strong> waiting for the user to do something like modal dialogs do.
  </p>
  
  <pre class="lang:js decode:true">var win, windowResource, i;

windowResource = "palette {  \    orientation: 'column', \    alignChildren: ['fill', 'top'],  \    preferredSize:[300, 130], \    text: 'ScriptUI Window - dialog',  \    margins:15, \    \    sliderPanel: Panel { \        orientation: 'row', \        alignChildren: 'right', \        margins:15, \        text: ' PANEL ', \        st: StaticText { text: 'Value:' }, \        sl: Slider { minvalue: 1, maxvalue: 100, value: 30, size:[220,20] }, \        te: EditText { text: '30', characters: 5, justify: 'left'} \        } \    \    bottomGroup: Group{ \        cd: Checkbox { text:'Checkbox value', value: true }, \        cancelButton: Button { text: 'Cancel', properties:{name:'cancel'}, size: [120,24], alignment:['right', 'center'] }, \        applyButton: Button { text: 'Apply', properties:{name:'ok'}, size: [120,24], alignment:['right', 'center'] }, \    }\}";
win = new Window(windowResource);
win.show();

app.documents.add(); // adds a new document
app.activeDocument.activeLayer.applyAddNoise (400, NoiseDistribution.GAUSSIAN, true) // applies Noise
for (i = 0; i &lt; 3; i++) { // Blurs it 3 times
    app.activeDocument.activeLayer.applyGaussianBlur(1);
    $.sleep (2000); // waits 2 seconds
    app.refresh(); // refreshes PS
}</pre>
  
  <p>
    In the above example, after the <code>win.show()</code> the script is busy creating a new document, applying and blurring some noise just to kill some time. In the meantime, the palette window shows and keep showing; when the script is done with the task, the palette is automatically closed with no need of user interaction whatsoever, nor explicit <code>win.close()</code>.
  </p>
  
  <p>
    In order to keep Photoshop busy, I&#8217;ve found two working alternatives (because a straight <code>$.sleep()</code> loop makes PS quite non-responsive) involving the following functions:
  </p>
  
  <pre class="lang:js decode:true">// According to the PS Javascript Reference:
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
};</pre>
  
  <p>
    The comments in the code should be self-explanatory. As far as I know, waitForRedraw() should work also in earlier PS versions &#8211; it&#8217;s not documented, it&#8217;s just used in one Reference&#8217;s demo scripts.
  </p>
  
  <h3>
    PS palette working example
  </h3>
  
  <p>
    One possible way to implement working palette Windows in Photoshop has the following logic:
  </p>
  
  <pre class="lang:js decode:true">var isDone, s2t, waitForRedraw, win, windowResource;

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

windowResource = "palette {  \    orientation: 'column', \    alignChildren: ['fill', 'top'],  \    preferredSize:[300, 130], \    text: 'ScriptUI Window - palette',  \    margins:15, \    \    sliderPanel: Panel { \        orientation: 'row', \        alignChildren: 'right', \        margins:15, \        text: ' PANEL ', \        st: StaticText { text: 'Value:' }, \        sl: Slider { minvalue: 1, maxvalue: 100, value: 30, size:[220,20] }, \        te: EditText { text: '30', characters: 5, justify: 'left'} \        } \    \    bottomGroup: Group{ \        cd: Checkbox { text:'Checkbox value', value: true }, \        cancelButton: Button { text: 'Cancel', properties:{name:'cancel'}, size: [120,24], alignment:['right', 'center'] }, \        applyButton: Button { text: 'Apply', properties:{name:'ok'}, size: [120,24], alignment:['right', 'center'] }, \    }\}";

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
}</pre>
  
  <p>
    The key point is the <code>isDone</code> variable, which is set false at the beginning. After the Window is shown, a while loop keeps checking for this sentinel value and call <code>app.refresh()</code>, or alternatively <code>waitForRedraw()</code> in order to keep the palette showing.
  </p>
  
  <p>
    Instead of an explicit call to <code>win.close()</code>, it&#8217;s a better option to attach to the buttons&#8217; <code>onClick()</code> functions a sentinel value&#8217;s switch to true. This way the while loop breaks and the Window automatically closes. Mind you, it&#8217;s crucial to set <code>isDone = true</code> even in the <code>win.onClose()</code> function, otherwise Photoshop will keep evaluating the variable even after the Window has been closed clicking the red dismiss button (see following screenshot) &#8211; resulting in some degree of non-responsiveness.
  </p>
  
  <div id="attachment_1307" style="width: 933px" class="wp-caption aligncenter">
    <a href="http://localhost:8888/wp-content/uploads/2012/10/PS_palettes.png" target="_blank"><img aria-describedby="caption-attachment-1307" class="size-full wp-image-1307 " title="PS palettes" src="http://localhost:8888/wp-content/uploads/2012/10/PS_palettes.png" alt="PS palettes" width="923" height="393" srcset="http://localhost:8888/wp-content/uploads/2012/10/PS_palettes.png 923w, http://localhost:8888/wp-content/uploads/2012/10/PS_palettes-150x63.png 150w, http://localhost:8888/wp-content/uploads/2012/10/PS_palettes-300x127.png 300w" sizes="(max-width: 923px) 100vw, 923px" /></a>
    
    <p id="caption-attachment-1307" class="wp-caption-text">
      First row: Mac CS5, Mac CS6.<br />Second row: PC CS5, PC CS6.
    </p>
  </div>
  
  <p>
    Few cosmetic differences are present this time too, including the position of the dismiss button (top-left for Mac, top-right for PC). This button is taken into account with the <code>onClose()</code> function as shown in the code example.
  </p>
  
  <h2>
    Platform specific behaviors (aka bugs)
  </h2>
  
  <p>
    Things are getting weird if you start to compare Mac and PC implementations of ScriptUI windows in Photoshop CS5 and CS6 (the tests on the PC side have been made on a virtualized Window7 system running in Parallels Desktop).
  </p>
  
  <p>
    It happens that, depending on how you mix platform, version and Window type, you end up with different behaviors (<em>modal</em> and <em>non-modal</em>), as follows:
  </p>
  
  <div id="attachment_1308" style="width: 700px" class="wp-caption aligncenter">
    <a href="http://localhost:8888/wp-content/uploads/2012/10/BehaviorTable.png" target="_blank"><img aria-describedby="caption-attachment-1308" class="size-full wp-image-1308 " title="Behavior Table" src="http://localhost:8888/wp-content/uploads/2012/10/BehaviorTable.png" alt="Behavior Table" width="690" height="471" srcset="http://localhost:8888/wp-content/uploads/2012/10/BehaviorTable.png 690w, http://localhost:8888/wp-content/uploads/2012/10/BehaviorTable-150x102.png 150w, http://localhost:8888/wp-content/uploads/2012/10/BehaviorTable-300x204.png 300w" sizes="(max-width: 690px) 100vw, 690px" /></a>
    
    <p id="caption-attachment-1308" class="wp-caption-text">
      Comparison of the ScriptUI Window type&#8217;s behavior (<em>modal</em> or <em>non-modal</em>) against Photoshop version and platform.
    </p>
  </div>
  
  <p>
    While <strong>dialog</strong>&#8216;s behavior is nicely uniform between Mac and PC, CS5 and CS6; <strong>palettes</strong> are a fiasco. Mac behavior is always <em>non-modal</em> (as it should be &#8211; palettes are non-modal by definition), while PC&#8217;s one depends on the Photoshop version and function used. Basically, only CS5 with app.refresh() works as it&#8217;s supposed to do, while in CS6 both functions fail.
  </p>
  
  <p>
    From my personal standpoint, this is a <strong>bug</strong> &#8211; and I haven&#8217;t found any workaround yet.
  </p>
  
  <h2>
    Conclusion
  </h2>
  
  <p>
    To sum up:
  </p>
  
  <ul>
    <li>
      Palettes are defined within Creative Suite applications as <em>non-modal</em> Windows that can wait idle.
    </li>
    <li>
      The Photoshop implementation misses entirely the &#8220;can wait idle&#8221; part &#8211; palettes keep showing only if there&#8217;s some code running in background.
    </li>
    <li>
      There&#8217;s a workaround to make palettes behave as they&#8217;re supposed to do, involving either <code>app.refresh()</code> or <code>waitForRedraw()</code> loops and a sentinel variable.
    </li>
    <li>
      On Mac the workaround, no matter on CS5 and CS6 or what function is used, makes the palettes <em>non-modal</em> (as they&#8217;re supposed to be by definition)
    </li>
    <li>
      On PC, only app.refresh() in CS5 returns a <em>non-modal</em> Window &#8211; CS6 ones are always <em>modal</em>.
    </li>
  </ul>
  
  <p>
    Which is quite a pain in the neck, especially if you need your palettes to work as they&#8217;re supposed to do in a platform and version independent way.
  </p>
</div>

<!-- Share-Widget Button BEGIN --><a href="javascript:void(0);" myshare\_id="mys\_shareit" myshare\_url="http://localhost:8888/2012/10/scriptui-window-in-photoshop-palette-vs-dialog/" myshare\_title="ScriptUI Window in Photoshop &#8211; Palette vs. Dialog" rel="nofollow" onclick=" return false;" style="text-decoration:none; color:#000000; font-size:11px; line-height:20px;"> 

<img src="http://localhost:8888/wp-content/plugins/share-widget/img/share-button-white-small.png" height="20" alt="Share" style="border:0" /> </a> <!-- Share-Widget Button END -->