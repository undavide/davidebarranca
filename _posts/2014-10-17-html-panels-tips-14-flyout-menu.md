---
id: 2732
title: 'HTML Panels Tips: #14 Flyout Menu'
date: 2014-10-17T01:11:27+01:00
author: Davide Barranca
excerpt: "With Photoshop CC 2014.2 (implementing CEP 5.2) it's finally possible to have flyout menus in HTML Panels too - as we had back in Flash land. This Tip shows you how to set them and deal with their click handler."
layout: post
guid: http://www.davidebarranca.com/?p=2732
permalink: /2014/10/html-panels-tips-14-flyout-menu/
seo_post_level_layout:
  - ""
link_url_field:
  - ""
seo_post_meta_description:
  - "How to implement a Flyout Menu in Photoshop HTML Panels using Adobe's CEP 5.2. "
image: /wp-content/uploads/2014/10/screenshot.png
categories:
  - Coding
  - HTML Panels
tags:
  - CEP 5.2
  - flyout
  - menu
---
<div class="pf-content">
  <p>
    With Photoshop CC 2014.2 (implementing <strong>CEP 5.2</strong>) it&#8217;s finally possible to have flyout menus in HTML Panels too &#8211; as we had back in Flash land. This Tip shows you how to set them and deal with their click handler.<!--more-->
  </p>
  
  <p>
    <img class="aligncenter size-full wp-image-2733" src="http://localhost:8888/wp-content/uploads/2014/10/screenshot.png" alt="Flyout Menu Screenshot" width="602" height="243" srcset="http://localhost:8888/wp-content/uploads/2014/10/screenshot.png 602w, http://localhost:8888/wp-content/uploads/2014/10/screenshot-150x60.png 150w, http://localhost:8888/wp-content/uploads/2014/10/screenshot-300x121.png 300w" sizes="(max-width: 602px) 100vw, 602px" />
  </p>
  
  <p>
    The <strong>full code</strong> of this demo extension is available on my GitHub page in the <a title="PS Panels Boilerplate on GitHub" href="https://github.com/undavide/PS-Panels-Boilerplate/tree/master/src/com.undavide.flyout" target="_blank">PS Panels Boilerplate</a> project.
  </p>
  
  <p>
    Things are pretty simple, though: you first set an<strong> XML String</strong> which describes the menu:
  </p>
  
  <pre class="lang:js decode:true">var flyoutXML = '\
&lt;Menu&gt; \
	&lt;MenuItem Id="enabledMenuItem" Label="Enabled Menu Item" Enabled="true" Checked="false"/&gt; \
	&lt;MenuItem Id="disabledMenuItem" Label="Disabled Menu Item" Enabled="false" Checked="false"/&gt; \
	\
	&lt;MenuItem Label="---" /&gt; \
	\
	&lt;MenuItem Id="checkableMenuItem" Label="Checkable Menu Item" Enabled="true" Checked="true"/&gt; \
	\
	&lt;MenuItem Label="---" /&gt; \
	\
	&lt;MenuItem Id="actionMenuItem" Label="Click me to enable/disable the Target Menu!" Enabled="true" Checked="false"/&gt; \
	&lt;MenuItem Id="targetMenuItem" Label="Target Menu Item" Enabled="true" Checked="false"/&gt; \
	\
	&lt;MenuItem Label="---" /&gt; \
	\
	&lt;MenuItem Label="Parent Menu (wont work on PS CC 2014.2.0)"&gt; \
		&lt;MenuItem Label="Child Menu 1"/&gt; \
		&lt;MenuItem Label="Child Menu 2"/&gt; \
	&lt;/MenuItem&gt; \
&lt;/Menu&gt;';</pre>
  
  <p>
    Remember the <code>\</code> at the lines&#8217; end to escape the carriage return. Nothing fancy, just <code>MenuItem</code> tags to define the structure. Mind you, nested menus don&#8217;t work right now (in Photoshop CC 2014.2.0 &#8211; although After Effects and Premiere are OK) but the bug is going to be fixed.
  </p>
  
  <p>
    You actually <strong>build</strong> the menu feeding <code>setPanelFlyoutMenu</code> with the XML string:
  </p>
  
  <pre class="lang:js decode:true ">// Uses the XML string to build the menu
csInterface.setPanelFlyoutMenu(flyoutXML);</pre>
  
  <p>
    An event listener has been added to respond to clicks (the event is <code>"com.adobe.csxs.events.flyoutMenuClicked"</code>):
  </p>
  
  <pre class="lang:js decode:true">csInterface.addEventListener("com.adobe.csxs.events.flyoutMenuClicked", flyoutMenuClickedHandler);</pre>
  
  <p>
    When a click occurs, an <code>event</code> is passed to the callback. It&#8217;s <code>data</code> attribute is an object containing both <code>menuId</code> and <code>menuName</code> . The callback in my case is as follows:
  </p>
  
  <pre class="lang:js decode:true">// Ugly workaround to keep track of "checked" and "enabled" statuses
var checkableMenuItem_isChecked = true;
var targetMenuItem_isEnabled = true;
		// Flyout Menu Click Callback
function flyoutMenuClickedHandler (event) {

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
}</pre>
  
  <p>
    The code above:
  </p>
  
  <ul>
    <li>
      fires a simple alert with the Label when a menu item is clicked;
    </li>
    <li>
      lets you check/uncheck a menu item;
    </li>
    <li>
      toggles the enabled/disabled status of a menu item when you click on a different item.
    </li>
  </ul>
  
  <p>
    I haven&#8217;t been able to retrieve the <em>enabled/disabled</em> and <em>checked/unchecked</em> menu items status dynamically, so I&#8217;ve worked around storing them in vars. Not ideal maybe, but it works
  </p>
  
  <p>
    As you see, I&#8217;ve used the <code>updatePanelMenuItem</code> function, which is used this way (straight from <code>CSInterface.js</code> sourcecode):
  </p>
  
  <pre class="lang:js decode:true">/**
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
CSInterface.prototype.updatePanelMenuItem = function(menuItemLabel, enabled, checked)
{
    var ret = false;
    if (this.getHostCapabilities().EXTENDED_PANEL_MENU) 
    {
        var itemStatus = new MenuItemStatus(menuItemLabel, enabled, checked);
        ret = window.__adobe_cep__.invokeSync("updatePanelMenuItem", JSON.stringify(itemStatus));
    }
    return ret;
};</pre>
  
  <p>
    That&#8217;s it, hope this helps! The extension is available on GitHub in my <a title="PS Panels Boilerplate on GitHub" href="https://github.com/undavide/PS-Panels-Boilerplate/tree/master/src/com.undavide.flyout" target="_blank">PS Panels Boilerplate</a> project.
  </p>
</div>

<!-- Share-Widget Button BEGIN --><a href="javascript:void(0);" myshare\_id="mys\_shareit" myshare\_url="http://localhost:8888/2014/10/html-panels-tips-14-flyout-menu/" myshare\_title="HTML Panels Tips: #14 Flyout Menu" rel="nofollow" onclick=" return false;" style="text-decoration:none; color:#000000; font-size:11px; line-height:20px;"> 

<img src="http://localhost:8888/wp-content/plugins/share-widget/img/share-button-white-small.png" height="20" alt="Share" style="border:0" /> </a> <!-- Share-Widget Button END -->