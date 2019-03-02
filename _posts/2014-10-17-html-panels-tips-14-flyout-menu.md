---
title: 'HTML Panels Tips: #14 Flyout Menu'
date: 2014-10-17T01:11:27+01:00
author: Davide Barranca
excerpt: "How to implement a Flyout Menu in Photoshop HTML Panels using Adobe's CEP 5.2. "
layout: post
permalink: /2014/10/html-panels-tips-14-flyout-menu/
description:
  - "How to implement a Flyout Menu in Photoshop HTML Panels using Adobe's CEP 5.2."
image: /wp-content/uploads/2014/10/screenshot.png
category:
  - CEP
  - HTML Panels
tags:
  - HTML Panels Tips
  - flyout

---

With Photoshop CC 2014.2 (implementing **CEP 5.2**) it's finally possible to have flyout menus in HTML Panels too - as we had back in Flash land. This Tip shows you how to set them and deal with their click handler.

![Flyout Menu Screenshot](/wp-content/uploads/2014/10/screenshot.png)

The **full code** of this demo extension is available on my GitHub page in the [PS Panels Boilerplate](https://github.com/undavide/PS-Panels-Boilerplate/tree/master/src/com.undavide.flyout "PS Panels Boilerplate on GitHub") project. Things are pretty simple, though: you first set an **XML String** which describes the menu:

{% highlight xml %}
var flyoutXML = '\
<Menu> \
	<MenuItem Id="enabledMenuItem" Label="Enabled Menu Item" Enabled="true" Checked="false"/> \
	<MenuItem Id="disabledMenuItem" Label="Disabled Menu Item" Enabled="false" Checked="false"/> \
	\
	<MenuItem Label="---" /> \
	\
	<MenuItem Id="checkableMenuItem" Label="Checkable Menu Item" Enabled="true" Checked="true"/> \
	\
	<MenuItem Label="---" /> \
	\
	<MenuItem Id="actionMenuItem" Label="Click me to enable/disable the Target Menu!" Enabled="true" Checked="false"/> \
	<MenuItem Id="targetMenuItem" Label="Target Menu Item" Enabled="true" Checked="false"/> \
	\
	<MenuItem Label="---" /> \
	\
	<MenuItem Label="Parent Menu (wont work on PS CC 2014.2.0)"> \
		<MenuItem Label="Child Menu 1"/> \
		<MenuItem Label="Child Menu 2"/> \
	</MenuItem> \
</Menu>';
{% endhighlight %}

Remember the `\` at the lines' end to escape the carriage return. Nothing fancy, just `MenuItem` tags to define the structure. Mind you, nested menus don't work right now (in Photoshop CC 2014.2.0 - although After Effects and Premiere are OK) but the bug is going to be fixed. You actually **build** the menu feeding `setPanelFlyoutMenu` with the XML string:

{% highlight js %}
// Uses the XML string to build the menu
csInterface.setPanelFlyoutMenu(flyoutXML);
{% endhighlight %}

An event listener has been added to respond to clicks (the event is `"com.adobe.csxs.events.flyoutMenuClicked"`):

{% highlight js %}
csInterface.addEventListener("com.adobe.csxs.events.flyoutMenuClicked", flyoutMenuClickedHandler);
{% endhighlight %}

When a click occurs, an `event` is passed to the callback. It's `data` attribute is an object containing both `menuId` and `menuName` . The callback in my case is as follows:

{% highlight js %}
// Ugly workaround to keep track of "checked" and "enabled" statuses
var checkableMenuItem_isChecked = true;
var targetMenuItem_isEnabled = true;
// Flyout Menu Click Callback
function flyoutMenuClickedHandler(event) {

  // the event's "data" attribute is an object, which contains "menuId" and "menuName"
  console.dir(event);
  switch (event.data.menuId) {

    case "checkableMenuItem":
      checkableMenuItem_isChecked = !checkableMenuItem_isChecked;
      csInterface.updatePanelMenuItem("Checkable Menu Item", true, checkableMenuItem_isChecked);
      break;

    case "actionMenuItem":
      targetMenuItem_isEnabled = !targetMenuItem_isEnabled;
      csInterface.updatePanelMenuItem("Target Menu Item", targetMenuItem_isEnabled, false);
      break;

    default:
      console.log(event.data.menuName + " clicked!");
  }

  csInterface.evalScript("alert('Clicked!\\n \"" + event.data.menuName + "\"');");
}
{% endhighlight %}

The code above:

*   fires a simple alert with the Label when a menu item is clicked;
*   lets you check/uncheck a menu item;
*   toggles the enabled/disabled status of a menu item when you click on a different item.

I haven't been able to retrieve the _enabled/disabled_ and _checked/unchecked_ menu items status dynamically, so I've worked around storing them in vars. Not ideal maybe, but it works As you see, I've used the `updatePanelMenuItem` function, which is used this way (straight from `CSInterface.js` sourcecode):

{% highlight js %}
/**
 * Updates a menu item in the extension window's flyout menu, by setting the enabled
 * and selection status.
 *  
 * Since 5.2.0
 *
 * @param menuItemLabel The menu item label.
 * @param enabled       True to enable the item, false to disable it (gray it out).
 * @param checked       True to select the item, false to deselect it.
 *
 * @return false when the host application does not support this functionality (HostCapabilities.EXTENDED_PANEL_MENU is false).
 *         Fails silently if menu label is invalid.
 *
 * @see HostCapabilities.EXTENDED_PANEL_MENU
 */
CSInterface.prototype.updatePanelMenuItem = function(menuItemLabel, enabled, checked) {
  var ret = false;
  if (this.getHostCapabilities().EXTENDED_PANEL_MENU) {
    var itemStatus = new MenuItemStatus(menuItemLabel, enabled, checked);
    ret = window.__adobe_cep__.invokeSync("updatePanelMenuItem", JSON.stringify(itemStatus));
  }
  return ret;
};
{% endhighlight %}

That's it, hope this helps! The extension is available on GitHub in my [PS Panels Boilerplate](https://github.com/undavide/PS-Panels-Boilerplate/tree/master/src/com.undavide.flyout "PS Panels Boilerplate on GitHub") project.

{% include_relative cepBook.md %}
