---
title: " HTML Panel Tips #25: CC 2018 Survival Guide\t\t"
tags:
  - CC 2018
url: 3181.html
id: 3181
category:
  - Coding
  - HTML Panels
date: 2017-10-23 00:04:41
---

That time of the year has come, and Photoshop CC 2018 is here. Read along to find out Everything You Always Wanted to Know About CEP 8* (*But Were Afraid to Ask). A brief history of Photoshop's HTML Panels support for your pleasure:

**Creative Cloud version**

**PS Internal version**

**CEP version**

Photoshop CC

14.x

4.0

Photoshop CC 2014

15.x

5.0

Photoshop CC 2015

16.x

6.0

Photoshop CC 2015.5

17.x

7.0

Photoshop CC 2017

18.x

7.0

Photoshop CC 2018

19.x

8.0

Set aside the weird correspondence in the above version numbers, you see that we've jumped to CEP 8, and Photoshop internal version is now 19.0.0.

### Versions

CEF has been bumped to 3.2987, Chromium is 57.0.2987.74 (leap of faith because for some mysterious reason `window.location = "chrome://version"` doesn't work anymore) and Node.js is 7.7.4, whereas Generator's Node.js is 4.8.4. Enough with digits, let's dig into actual stuff. You'd be tempted to use `Version="8.0"` in manifest.xml, like:

<?xml version="1.0"?>
<ExtensionManifest xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
  ExtensionBundleId="com.example.helloWorld" 
  ExtensionBundleVersion="1.0.0" Version="8.0">
  <ExtensionList>
    <Extension Id="com.example.helloWorld.panel" Version="1.0.0"/>
  </ExtensionList>
  <ExecutionEnvironment>
    <HostList>
      <Host Name="PHXS" Version="19.0"/>
      <Host Name="PHSP" Version="19.0"/>
    </HostList>
    <LocaleList>
      <Locale Code="All"/>
    </LocaleList>
    <RequiredRuntimeList>
      <RequiredRuntime Name="CSXS" Version="8.0"/>
    </RequiredRuntimeList>
  </ExecutionEnvironment>
  <!\-\- etc. -->
</ExtensionManifest>

Tempting, but **don't do it**. `Version` in `<ExtensionManifest>` and `<RequiredRuntime>` has always matched CEP version, but in this case setting the two to `"8.0"` will make your panel disappear from the Window > Extensions menu. Why? We're in the same boat, I've asked around but got no plausible answer yet. Stick to `"7.0"`.

### CSInterface

I've inspected the diffs between CSInterface.js (CEP 7 vs. CEP 8) and it's just punctuation in comments. LOL.

### Debugging

Debugging tips: Google Chrome 62.0.3202.62 (Official Build) (64-bit) at least on macOS Sierra 10.12.6 can't connect anymore a debug session. It seems it can: you successfully load localhost:8088 (or whatever port you're using) and the usual textual link appears. Click it, and you're staring at a blank Chrome Developer Tools page. Out of curiosity I opened the Console and got a "Uncaught Error: No setting registered: showAdvancedHeapSnapshotProperties" referring to `inspector.js`. ![](http://localhost:8888/wp-content/uploads/2017/10/console-700x226.png) Workaround: download [Google Chromium](https://chromium.woolyss.com/download/en/#mac). The theory is that you should test your panel with the exact Chromium version implemented in the host application: yet, finding it is a majestic pain in the butt if you try to follow the [official instruction](https://www.chromium.org/getting-involved/download-chromium) for old builds. The closest one I've been able to get is 57.0.2987.0 and you can get it too [here](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Mac/444967/). For debugging in a working DevTool, Chromium-latest should work too.

### Node.js

There must be some kind of bloody war going on at the Adobe headquarters around Node.js. Only between CC 2015.0 and CC 2015.1.2 its implementation changed thrice, and of course there's something new and backward not compatible in CEP 8 as well. Anyway, the State of the Art is as follows. Node is still **disabled by default**. Very like with CEP 7, if you want to enable it, add these in the `manifest.xml`:

<!\-\- etc. -->
<Resources>
  <CEFCommandLine> 
    <Parameter>--enable-nodejs</Parameter> 
    <Parameter>--mixed-context</Parameter>
  </CEFCommandLine>
<!\-\- etc. -->

Mind you, `--mixed-context` is optional. So you basically have three possible scenarios:

1.  Forget about Node entirely (no `--enable-nodejs` flag): it's disabled.
2.  Enable Node in a **Separate Context** (`--enable-nodejs` but no `--mixed-context`).
3.  Enable Node in a **Mixed Context** (`--enable-nodejs` and `--mixed-context`).

As a quick reminder on Contexts, due to the io.js switch back in an earlier version, I’ve been told, Node.js and the Browser/Panel contexts don’t talk to each other anymore. What does this mean? If you have a Node.js module and try to access the jQuery $ object, or csInterface, or anything else defined in the Browser context (e.g. your main.js file), it won’t work. Workaround: use the global `window` object as a bridge. Or enable Mixed Context. This hasn't change from CEP7, so I'll skip to the new stuff. Besides injecting in the global space `Buffer`, `global`, `process`, `require` and `module`, CEP 8 adds a new `cep_node` global, which has its own `Buffer`, `global`, `process` and `require` properties. These new props props are useful because now an `<iframe>` (which still needs to have its own `enable-nodejs` specified), is going access `cep_node`, that you can use as a bridge object between `<iframe>` elements when the **Context is Separate**. This marks a difference with CEP7, where Node globals were always available. Also, order of appearance is important to access data, see below:

    <body>
      <!\-\- Node globals: OK, cep_node:OK -->
      <iframe src="iframe1.html" enable-nodejs>
        <!\-\- Node globals: NO, cep_node:OK -->
        <!\-\- You can set node_cep props, and access them \*later\*, e.g.
        <script>
          cep_node.process.pippo = "Goofy";
        </script> -->
      </iframe>
      <iframe src="iframe2.html" enable-nodejs>
         <!\-\- Node globals: NO, cep_node:OK -->
         <!\-\- You can access node_cep props set \*before\*
        <script>
          console.log(cep_node.process.pippo); // "Goofy"
        </script> -->
      </iframe>
    </body> 
   <!\-\- <script> tags belongs to iframe1.html and iframe2.html -->

As opposed to the previous case and still specific to CEP8, with **Mixed Context** the Node globals are always available, everywhere (`<iframe>` elements included), but this time `cep_node` is rebuilt from scratch in each `<iframe>`.

    <body>
      <!\-\- Node globals: OK, cep_node:OK -->
      <iframe src="iframe1.html" enable-nodejs>
        <!\-\- Node globals: OK, cep_node:OK -->
        <!\-\- set node_cep props only for this iframe only, e.g.
        <script>
          cep_node.process.pippo = "Goofy";
        </script> -->
      </iframe>
      <iframe src="iframe2.html" enable-nodejs>
         <!\-\- Node globals: OK, cep_node:OK -->
         <!\-\- cep_node props set in another iframe are not available
        <script>
          console.log(cep_node.process.pippo); // undefined
        </script> -->
      </iframe>
    </body> 
    <!\-\- <script> tags belongs to iframe1.html and iframe2.html -->

What else: `module` and `exports` globals may conflict with some JS libraries, e.g. jQuery, resulting in the library being loaded in the Node context rather than in the Browser’s. As a fix, try:

<!\-\- Insert above script imports -->
<script>
  if (typeof module === 'object') {
    window.module = module; module = undefined;
  }
  if (typeof exports === 'object') {
  	window.exports = exports; exports = undefined;
  }
</script>
<!\-\- JS imports -->
<script src="scripts/jquery.js"></script>
<script src="scripts/csinterface.js"></script>
<!\-\- Insert after JS imports, IF you need module, exports -->
<script>
  if (window.module) module = window.module;
  if (window.exports) exports = window.exports;
</script>

Oh, now in CEP 8 `require()` paths need to be absolute and not relative, like:

	// Replace this:
	require("./js/lib/javascriptobfuscator.js");
	
	// With either this:
	var dir = csInterface.getSystemPath("extension")
	require(dir + "/js/lib/javascriptobfuscator.js");
	
	// Or:
	require(__dirname + "/js/lib/javascriptobfuscator.js");

And I'd say this sums up things for Node.js

### Bugs

I'm listing here the known CEP 8 bugs, even the one already mentioned in the post.

*   `Version="8.0"` in the `<ExtensionManifest>` and `<RequiredRuntime>` tags in `manifest.xml` (to match CEP version) makes the Panel disappear from the Extensions list.
*   **Panel Geometry bug**: setting `<Size>`, `<MinSize>` and `<MaxSize>` with equal values doesn't produce a fixed size (unresizable) panel anymore, as it used to do with _any_ other pre-2018 version. In order to make a fixed sized panel in 2018, set only the `<Size>` tag. That is to say: panel is fixed size in 2018 but not in CC, 2014, 2015, 2015.5, 2017 or the other way around. Very funny. Moreover, the very fist time a panel is collapsed/reopened, it assumes forever some completely wrong width/height, and may hide relevant content.
*   **Bug with Panel Refresh**: (either via ⌘+R on Chromium Dev Tools, or `window.location.reload(true)` in the panel). You may run into problems with Node after the first refresh unless you have both `--enable-nodejs` and `--mixed-context` flags set.
*   As I mentioned before, setting the panel's `window.location = "chrome://version"` **doesn't load the Chrome Version page in the Panel** anymore – it used to work fine with CC 2017.
*   Not really a bug, but be aware that some videos that used to play well (e.g. from YouTube) in CEP 7, won't play at all in CEP 8. This is due to a WebM VP9 codec that Chromium doesn't support anymore (the codec License doesn't get along well with the Chromium Open Source License, so they dropped the support).

### Documentation

You can have a look at the official CEP 8 Cookbook [here](https://github.com/Adobe-CEP/CEP-Resources/blob/master/CEP_8.x/Documentation/CEP%208.0%20HTML%20Extension%20Cookbook.md). While writing this post, the [CEP Github repository](https://github.com/Adobe-CEP/CEP-Resources/) still has no CEF Client (CEF Client has been added and is found [here](https://github.com/Adobe-CEP/CEP-Resources/tree/master/CEP_8.x)), no updated ZXPSignCmd. Admins of the Adobe-CEP GitHub repository accepts push requests for the Cookbook, so users are allowed / encouraged to make the documentation better!

* * *

### The Photoshop HTML Panels Development Course – CC 2018 Edition

[![Photoshop HTML Panels Development course](http://localhost:8888/wp-content/uploads/2016/03/BookVideo-300x193.jpg)](http://htmlpanelsbook.com "Photoshop HTML Panels Development course") If you're reading here, you might be interested in my Photoshop Panels development – so let me inform you that I've authored a full course:

*   **303 pages** PDF
*   **3 hours** of HD screencasts
*   **28 custom Panels** with commented code

[Check it out!](http://htmlpanelsbook.com) Please help me spread the news – I'll be able to keep producing more exclusive content on this blog. Thank you!