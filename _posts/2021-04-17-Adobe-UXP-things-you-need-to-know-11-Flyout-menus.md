---
title: "Adobe UXP: Things you need to know! #11 Flyout Menus and Entrypoints"
date: 2021-04-17
author: "Davide Barranca"
excerpt: "In this episode I'll show you how to setup Flyout Menus for UXP plugins. Feel free to watch the video or read the article, they cover the same ground."
layout: post
description: "In this episode I'll show you how to setup Flyout Menus for UXP plugins. Feel free to watch the video or read the article, they cover the same ground."
image: "/wp-content/uploads/2020/10/TYNTN.png"
media_card: /wp-content/uploads/2021/04/TYNTN-twitter11.png
category:
  - Development
tags:
  - UXP
---

In this episode I'll show you how to setup Flyout Menus for UXP plugins, while introducing UXP Entrypoints. Feel free to watch the video or read the article, they cover the same ground.

<iframe width="560" height="315" src="https://www.youtube.com/embed/v-x1ZrOtlzQ" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Flyouts

Flyouts are the kind of popup menus that appear when you click the top-right corner of UXP plugin's panels. They're traditionally used to launch info/about dialogs, or directly set Preferences in a remarkably unobtrusive way: the Flyout is easily reachable (stuff in there is always just one click away) and it helps you tremendously in keeping the precious real estate of the UI as uncluttered as possible.

![](/wp-content/uploads/2021/04/Panel.jpg)

Flyouts can store either single items, or group them inside submenus: which in turn can also be nested in a sort-of Russian doll fashion multiple times. In addition, all Flyout items can be dynamically set as _enabled_ or _disabled_ (grayed out), and _checked_ or _unchecked_ (with or without a flag besides their name).

A UXP panel is always (when you instruct it to do so, which we'll see in a moment) listening for Flyout user interactions, i.e. clicks. There's just one callback in charge of dealing with those clicks, and within that function we'll write the logic to handle the appropriate event—in other words, to recognize which menu item's been clicked and then act accordingly.

The subject of Flyout menus is also a way for me to introduce another crucial aspect of UXP plugins programming.

## Entrypoints

You use `entrypoints` to bootstrap the plugin and set lifecycles hooks, i.e. functions that are expected to run when a particular event happens in the plugin's life. They operate on the level of the **plugin** itself, or the **panels** and the **commands** that the plugin can be made of[^command].

[^command]: If you need a reminder, a Command is a GUI-less script that belongs to the plugin's "Plugins" menu and is set via the Manifest. Look back to [Episode #04 - Commands vs. Panels and the manifest.json]({% post_url  2020-11-18-Adobe-UXP-things-you-need-to-know-4-commands-panels-manifest %}).

In addition, it's used to setup the Flyout menu and its callback. Although at the time of this writing (`uxp-4.4.2-PR-6039.17` in Photoshop 2021 v.22.3 release) many of the hooks aren't functional yet, let me show you how to use it.

First you need to require `uxp` and use the `entrypoints.setup()` method, that accepts an object.

```js
// index.js
const { entrypoints } = require('uxp');
entrypoints.setup({
  plugin: { /* ... */},
  panels: { /* ... */},
  commands: { /* ... */ },
})
```

You're allowed to call `setup()` only once in your code, otherwise an error will be thrown. The `plugin` property contains lifecycle hooks on the plugin level, `create` and `destroy`. Neither of them work so let's skate over this; I won't talk about `commands` either (see [episode #04]({% post_url  2020-11-18-Adobe-UXP-things-you-need-to-know-4-commands-panels-manifest %}) if you need to catch up), so let's focus on the `panels` entry.

### Panel's lifecycle hooks

As I've already mentioned in this series, a single UXP plugin can contain more than one panel. Each panel can have its own _panel level_ entrypoint:  in the `panels` object you should identify them through their `id`, that you've set in the `manifest.json`:

```json
// manifest.json
{
  "id": "com.davidebarranca.flyout",
  "name": "UXP Flyout example",
  "version": "1.0.0",
  "main": "index.html",
  "host": [ { "app": "PS", "minVersion": "22.0.0" } ],
  "manifestVersion": 4,
  "entrypoints": [
    {
      "type": "panel",
      "id": "vanilla",
      /* ... */
    },
  ],
  /* ... */
}
```

Here we have only one panel, with `id` equal to `vanilla`, so the code in the `entrypoints` becomes:

```js
// index.js
const { entrypoints } = require('uxp');
entrypoints.setup({
  plugin: { create(){}, destroy(){} }, // NOT WORKING
  panels: {
    vanilla: {
      // Lifecycle hooks
      create(){},       // NOT WORKING
      hide(){},         // NOT WORKING
      destroy(){},      // NOT WORKING
      show(event){},    // callback when panel is made visible
      // Flyout menu
      menuItems: [],    // Flyout menu's structure
      invokeMenu(id){}, // callback for Flyout menu click events
    }
  }
})
```

As you see, the `create`, `hide`, and `destroy` hooks (theoretically for when the panel is created the first time, hidden or destroyed) don't fire yet. `show` runs fine instead, but only once (so it's kind of a `create` I suppose).

### Panel's Flyout menu

The actual Flyout menu stuff we're interested about is in the `menuItems` array, and the `invokeMenu` callback. Let's start with the former: the code to produce the Flyout from the first screenshot in this article is as follows.

```js
// index.js
const { entrypoints } = require('uxp');
entrypoints.setup({
  panels: {
    vanilla: {
      menuItems: [
        // by default all items are enabled and unchecked
        {
          label: "Preferences", submenu:
          [
            {id: "bell", label: "🔔  Notifications"},
            {id: "dynamite", label: "🧨  Self-destruct", enabled: false },
            {id: "spacer", label: "-" }, // SPACER
            {id: "enabler", label: "Enable  🧨" },
          ]
        },
        {id: "toggle", label: "Toggle me!", checked: true },
        {id: "about", label: "About" },
        {id: "reload", label: "Reload this panel" },
      ]
    }
  }
})
```

Let me walk you through the code. The structure is basically JSON: everything in the `menuItems` array will turn into a Flyout menu item, in the example we've got eight of them. Items are objects with a `label` property (please note that emoji do work there 🐋), and an `id` where it makes sense. I've omitted the `id` in the first object labelled `"Preferences"`, because this acts as a container: in fact it has a `submenu` property, which holds another array of objects, each one provided with both `id` and `label`.

Non-submenu items have the optional `enabled` and `checked` properties. By default, i.e. if you don't explicitly set them, they're always enabled and unchecked. Spacers are available too, you create them setting the label to a single minus `-`, then they get expanded to a full line and disabled by default.

Let me paste again the menu here to show what I'm after (keep in mind it's just a dummy menu):

![](/wp-content/uploads/2021/04/Panel.jpg)

- We have a "Preferences" menu, that contains two items. One of which is disabled (too dangerous!): clicking the "Enable 🧨" entry will re-enable it—actually it's slightly more complex but we'll see that in a minute.
- The "Toggle me!" item will check and uncheck itself.
- "About" is going to fire a popup dialog (I'll use a very lame `alert` instead), but it's there as a placeholder for any kind of scripting code you may want to run
- "Reload this panel" is a handy utility that will do what it suggests.

As is, the menu is shown when accessed from the UI, but it lacks any interactivity. This is why we also need the `invokeMenu` callback.

```js
// index.js
const { entrypoints } = require('uxp');
entrypoints.setup({
  panels: {
    vanilla: {
      menuItems: [ /* ... */ ],
      invokeMenu(id) {
        console.log("Clicked menu with ID", id);
        // Storing the menu items array
        const { menuItems } = entrypoints.getPanel("vanilla");
        switch (id) {
          case "enabler":
            menuItems.getItem("dynamite").enabled =
              !menuItems.getItem("dynamite").enabled;
            menuItems.getItem(id).label =
              menuItems.getItem(id).label == "Enable  🧨" ?
                                             "Disable  🧨" :
                                             "Enable  🧨";
            break;
          case "toggle":
            menuItems.getItem(id).checked = !menuItems.getItem(id).checked
            break;
          case "reload":
            window.location.reload()
            break;
          case "about":
            showAbout()
            break;
        }
      },
    }
  }
})
```

One thing to notice is that we have one callback, that receives the item's `id` as the only parameter: so it makes sense to use a `switch` statement with multiple cases.

Let me start with the `"toggle"`, which is simpler: it toggles its own `checked` property. In order to reference itself, it uses the `getItem()` method of the `menuItems` array, which in turn you retrieve with the `entrypoints.getPanel()` function, passing the panel `id` (the one in the `manifest.json`, here `vanilla`). So in the end it should be like:

```js
entrypoints.getPanel("vanilla").menuItems.getItem("toggle");
```

In my code I've stored the `menuItems` in a constant in advance, for convenience: I'm going to need it multiple times. Also, there's no need to explicitly write `"toggle"`, as we already are in the case where this is the `id`.

To recap, you `getPanel()` from the `entrypoints` via the panel `id`, then you access the `menuItems` array from the panel, then you `getItem()` from the `menuItems` via the item's `id`. Finally you access the property that you need (here `checked`) and assign the new value.

A slightly more complex example is the `"enabler"`, that toggles the boolean for the `"dynamite"`'s `enabled` property. In doing so, it also change it's own `label` (it toggles between `"Enable 🧨"` and `"Disable 🧨"`). I'm not sure whether it's really crucial from the UX point of view, but it was a nice way to show it's possible to change labels too. Same `menuItems.getItem()` dance than before.

The remaining two cases are the simplest ones: `"reload"` runs `window.location.reload()` to refresh the panel's view, while `"about"` calls the external function `showAbout()`, that is nothing but a bare wrapper for `showAlert()`.

```js
const photoshop = require('photoshop')
const showAbout = () => {
  photoshop.core.showAlert(
    "Hello everyone 🧢\n\n" +
    "This could also be a dialog...\n" +
    "See Episode #10"
  )
}
```

In the real world, this could be a fully fledged dialog (see [episode #10]({% post_url  2021-03-01-Adobe-UXP-things-you-need-to-know-10-Dialogs %})), or any command tapping directly into the host application Scripting API.

## Recap

Flyout menus are a quite convenient way to group commands and information, and it's not really difficult to build them. Items are stored in a JSON structure: they've properties such as `label`, `enabled`, `checked`, and can be nested in `submenu`s. A single handler function deals with user interaction via `id` of the clicked items. We've seen how to check/uncheck items, as well as enable/disable and change the labels too.

Flyouts are set up in the UXP plugin's Entrypoints, a special object that is used to define lifecycle hooks both on the plugin's and panel's level (most of which aren't functional yet, but they will in the future), commands (in conjunction with the `manifest.json`), and the flyout itself.

{% include SUPPORT.md %}

Stay safe, get the vaccine shot if/when you can – bye!

{% include UXP_TYNTN.md %}

---
