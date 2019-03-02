---
title: " HTML Panel Tips #19: CC2015.1 (CEP6.1) Node.js Fixes\t\t"
tags:
  - CEP6.1
  - Node.js
url: 2906.html
id: 2906
category:
  - Coding
  - HTML Panels
date: 2015-12-01 11:11:41
---

The [latest release](https://helpx.adobe.com/photoshop/using/whats-new.html) of Photoshop introduces, among the rest, a new version of the Common Extensibility Platform, **CEP6.1**. In turn, CEP6.1 is a major break in backward compatibility due to the way it manages Node.js. If you have an extension using Node.js, chances are that it's now broken in Photoshop CC2015.1. Read along to know why, and how to fix it. **\[UPDATE\] **With the release of Photoshop CC2015.1.2, the unified context has been brought back! Read the relevant section below to find out how to enable it.

### Node.js is now io.js

Adobe has switched to the **io.js** branch. If you didn't know, some Node maintainers [forked the mainline](http://anandmanisankar.com/posts/nodejs-iojs-why-the-fork/) and started working on io.js - this happened quite a while ago: in fact now the branches have merged back, and the versioning has changed too: Node is not anymore 0.12 or the like - we count like 5.1 (current version when I write this). Why did Adobe picked the older io.js is a question that yours truly cannot address.

### Node.js is disabled by default

Due to security concerns, Node.js was by default enabled in CEP6.0 and prior versions - it's now **disabled**, unless you add in the `manifest.xml`, in the `<resources>` tag the following:

<CEFCommandLine>
  <Parameter>--enable-nodejs</Parameter>
</CEFCommandLine>

Also, you need to explicitly enable Node in iframes too:

<iframe id="someID" class="someClass" enable-nodejs>

### Node.js and Browser contexts are detached

**\[UPDATE\] **The unified context is back in the PS **CC2015.1.2** update, but it's **disabled by default**. In order to enable it, put this one in the `manifest.xml`:

<CEFCommandLine>
  <Parameter>--enable-nodejs</Parameter>
  <Parameter>--mixed-context</Parameter>
</CEFCommandLine>

What follows below still applies for previous version of Photoshop.

* * *

  Due to the io.js move, Node.js and the Browser context don't talk to each other anymore. This is a big pain in the ass, and, according to Adobe's engineers, a unified context will be back in CEP7 (that should be released, if they keep the same pace, alongside with Photoshop CC2016, so many months from now). What does the above mean? If you have a Node.js module and try to access jQuery `$` object, or `csInterface`, or anything else defined in the Browser context (e.g. your `main.js` file), it will fail! So much fun. Workaround: use the global `window` object as a bridge:

// ===========
// in main.js
// ===========
var csInterface = new CSInterface();
window.csInterface = csInterface;      
window.$ = $;
// etc.

// =====================
// in your node.js files
// =====================
var $ = window.$;
var document = window.document;
var csInterface = window.csInterface;
// etc.

### Chrome only for debugging

No way to use Safari anymore, only the Chrome Developer Tools.

### That's all folks

When/if I find something else pertinent to the CEP6.1 switch, I'll update this post. If you do, please add in the comments below! Thanks.