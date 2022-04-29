---
title: "Adobe UXP: Things you need to know! #13 Manifest v5"
date: 2022-04-29
author: "Davide Barranca"
excerpt: "UXP has updated the Manifest to version 5, let's have a look at what this actually means for your Photoshop plugins."
layout: post
description: "UXP has updated the Manifest to version 5, let's have a look at what this actually means for your Photoshop plugins."
image: "/wp-content/uploads/2020/10/TYNTN.png"
media_card: /wp-content/uploads/2022/04/TYNTN-twitter13.png
category:
  - Development
tags:
  - UXP
---

UXP has updated the Manifest to version 5. We'll see what's new, what's changed, how to deal with your plugins' code and users. I'm going to summarize the most salient/tricky points, and link the relevant documentation for you to dig deeper.

## Prerequisites and the CC Marketplace

Manifest v5 is officially supported since **Photoshop 2022** (version **23.3.0**), that sports **UXP v6.0.2**. Developers are allowed to submit Manifest v5 based plugins in the CC Marketplace, and users can install them provided that they've got **Creative Cloud Desktop v5.7**.

What happens when you push an updated version of your UXP plugin with Manifest v5 to the CC Marketplace? It depends on what users currently have installed in their system.

- Photoshop v23.3 and an older (Manifest v4) version of your plugin: they'll be able to update your plugin and use the latest one (with Manifest v5).
- Photoshop older than v23.3 and an older (Manifest v4) version of your plugin: they must update Photoshop to be allowed to install the new one. Otherwise, they can keep using the older (Manifest v4) UXP plugin version.
- Photoshop older than v23.3 but they've not acquired your plugin yet: they _will not_ be offered the older (Manifest v4) plugin. They must update Photoshop and get the newest (Manifest v5) plugin.

In other words, **the CC Marketplace doesn't work like the Apple Store**, where you're deployed the latest product version that is compatible with your system. Here, only the latest product is made available, and users must keep up with the minimum requirements if they want to get it.

## Photoshop API v2

With the Manifest v5 you can unlock all the new Photoshop Scripting API features currently available; they're documented in the [Adobe Developer](https://developer.adobe.com/photoshop/uxp/2022/ps_reference/) website (see the [changelog](https://developer.adobe.com/photoshop/uxp/2022/ps_reference/changelog/)). 

They're not on by default, though: you must explicitly declare to use the Photoshop API v2 in the `manifest.json`.

```json
"manifestVersion": 5,
"host": [
  {
    "app": "PS",
    "minVersion": "23.3.0",
    "data": {
      "apiVersion": 2
    }
  }
],
```

Few things to note:

- `"host"` is now an _array_ of objects: in fact Manifest v5 supports multi-host UXP plugins (at the moment Photoshop and Adobe XD at best).
- There's a new `"data"` property, where you can specify the `"apiVersion"`: set it to `2`.
- As expected, `"manifestVersion"` is now `5`.

You can still set `"apiVersion"` to `1` if you want, but you'll get a warning message in the Console and, most importantly, you'll miss all the new PS Scripting features[^api12].

[^api12]: To sum up: Manifest v4 uses the PS API 1 (by default, there's no need to set it); with Manifest v5 you can choose between API 1 and 2.

## Modal State

Speaking of Photoshop Scripting, among all the DOM additions the one API v2 feature that is going to have the biggest impact (and by far) on your existing code is the new requirement to put Photoshop in a _Modal State_ when some operations are performed. Quoting the [official documentation](https://developer.adobe.com/photoshop/uxp/2022/ps_reference/media/executeasmodal/):

> (it) is needed when a plugin wants to make modifications to the Photoshop state. This includes scenarios where the plugin creates or modify documents, or the plugin wishes to update UI or preference state.

When Photoshop is in a modal state, some menus are disabled and the script _steals_ the host application's attention, so to speak; very little interaction is then allowed.

For which operations is the modal state now required? An annoyingly lot of them, you'd be tempted to instinctively reply, e.g. when:

- Dealing with File I/O (e.g. saving a document)
- Setting some properties via BatchPlay (e.g. History states)
- Acting on layers (e.g. running Filters or Adjustments)
- Anything else that _"may modify the state of Photoshop"_.

What in API v1 used to work out-of-the-box, may now throw `"Uncaught Error: Event: <whatever> may modify the state of Photoshop. Such events are only allowed from inside a modal scope"`.

![](/wp-content/uploads/2022/04/console.jpg)

How do you create such _modal scope_ then? With the mighty `executeAsModal()` function, with which you're going to wrap all offending calls. This is a very basic example:

```js
const ps = require('photoshop');
const bp = ps.action.batchPlay;
const executeAsModal = ps.core.executeAsModal;

await executeAsModal(async () => {
  // In the modal state now
  await ps.app.activeDocument.duplicate();
  await bp([{ _obj: "invert" }], {}); // Image > Adjustments > Invert
}, {commandName: "Modifying the state of PS like there's no tomorrow"})
```

As a bonus, if the code you're running takes more than 2 seconds to complete, a progress bar pops up by default.

![](/wp-content/uploads/2022/04/progress.jpg)

In fact, the whole rationale is to give users a way to interrupt tasks if/when needed. The function's signature is:

```js
executeAsModal(targetFunction, options)
```

In my example I've used an anonymous (and asynchrohous) `targetFunction` inline; `executeAsModal()` itself returns a promise, hence you may want to use `await`[^await] in front of it.

[^await]: The whole syntax can get a bit funny with all the nested `async`/`await`.

The `targetFunction` in turn receives two objects as parameters (that I won't mention here for simplicity's sake), while in the `options` object the only required property is `commandName`, that defines the string that appears in the progress bar popup.

I strongly encourage you to read the whole content of its [documentation page](https://developer.adobe.com/photoshop/uxp/2022/ps_reference/media/executeasmodal/), as `executeAsModal()`—when you get over the initial annoyance—is _extremely powerful_. I won't cover every feature in detail here, but let me mention that among the rest you can use it to:

- Suspend and resume the History _multiple times_.
- Update both the progress bar and the displayed text.
- Register documents to be automatically closed if the user cancels the process (how cool is that).

If you're wondering, API v2 also include `suspendHistory()` (see [the documentation](https://developer.adobe.com/photoshop/uxp/2022/ps_reference/classes/document/#suspendhistory)) which is a simpler wrapper for `executeAsModal()`.

## Permissions

The Manifest v5 introduces a new `"requiredPermissions"` property, that may look something like this:

```
"requiredPermissions": {
  "launchProcess": { 
    "schemes": ["https", "mailto", "file"], 
    "extensions": [".pdf"]
  }, 
  "network": { "domains": ["https://cc-extensions.com"] },
  "allowCodeGenerationFromStrings": true,
  "localFileSystem": "plugin",
  "clipboard": "read"
}
```

The best documentation for each one of the properties at the moment of this writing is found in [this article](https://medium.com/adobetech/big-updates-coming-to-uxp-3185f0e47130) by Padma Krishnamoorthy; I'll leave it to her for the technical details, let me cover the bird-eye view here.

In case you haven't noticed yet, the UXP team takes the concepts of Users' **trust** and **consent** quite seriously. In this spirit, in the Manifest you're going to _declare upfront_ what kind of permissions your plugin needs, e.g. to open a website, read the clipboard, mess with the Filesystem, etc. Eventually, this information is going to be displayed in your plugin's CC Marketplace product page—not yet, but they plan to—in order to give users full disclosure about the ways your plugin will interact with their system. I suggest to keep extra permissions at minimum, and include only the ones you really need.

Be also aware that, in addition to the permissions that you have to generically declare in the Manifest (I want to access `"https"` URLs, I need to open `".pdf"` files), a "Request for Permission" popup will open _anyway_ when you reach out for _that specific_ https address, or _that specific_ pdf file.

![](/wp-content/uploads/2022/04/request.jpg)

This way users can grant access to, and are exactly informed about, the exact resource your plugin is dealing with.

## Changes in Entrypoints

Finally, there's one little detail in the Manifest v5 transition that is able to crash your plugin, and it boils down to the different parameter received by the `show()` function in the panel's `entrypoints`.

```js
const entrypoints = require('uxp').entrypoints
entrypoints.setup({
  panels: {
    yourpanel: { 
      menuItems: [ /*...*/ ]
      invokeMenu(id) {/*...*/},
      create() { /*...*/ }, 
      destroy(){ /*...*/ }, 
      show(payload){ /*...*/ }, // <== this guy here
      hide(){ /*...*/ } 
    }
  }
})
```

In Manifest v4 plugins, `show()` used to receive an object with a `node` property: the HTML `<body>` that very likely you've always used to `.appendChild()` a `<div>` to.

```js
// Manifest v4 
var root;
create() {
  root = document.createElement("div");
  // etc...
}
show(payload) {
  payload.node.appendChild(root); // <== <body> is the payload.node
}
```

In Manifest v5 UXP plugins there's no need to access the `node` property, the `<body>` is received directly:

```js
// Manifest v5 
show(payload) {
  payload.appendChild(root);  // <== <body> is the payload
}
```

As far as I know this has still to be fixed e.g. in the UXP Developer Tool's React template (in which case, find line 44 and substitute `event.node` with `event` and it'll work back as normal).

## Conclusions

`git checkout -b manifest5` and be happy 😄.

---

{% include SUPPORT.md %}

Stay safe, get the Nth vaccine shot if/when you can – bye!

{% include UXP_TYNTN.md %}

---
