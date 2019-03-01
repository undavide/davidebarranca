---
id: 2136
title: 'ScriptUI Events: call(), dispatchEvent(), notify()'
date: 2013-08-04T19:20:33+01:00
author: Davide Barranca
excerpt: "In your code you may need to run a ScriptUI component's callback, or simulate a user interaction, maybe as a part of a subroutine. There are few ways to do this, with slight differences in the behavior: directly, using call(), notify() or dispatchEvent() - I've set up a commented demo Dialog that shows them all."
layout: post
guid: http://www.davidebarranca.com/?p=2136
permalink: /2013/08/extendscript-scriptui-events-call-notify-dispatchevent/
standard_seo_post_level_layout:
  - ""
standard_link_url_field:
  - ""
standard_seo_post_meta_description:
  - 'How to invoke a ScriptUI component’s callback with or without simulating user interaction: directly, using call(), notify(), dispatchEvent()'
image: /wp-content/uploads/2013/08/ScriptUI_Events.png
categories:
  - Coding
  - ExtendScript / Javascript
tags:
  - call()
  - callback
  - dispatchEvent()
  - Event
  - notify()
  - ScriptUI
---
<div class="pf-content">
  <p>
    <span itemprop="headline">In your code you may need to run a ScriptUI component&#8217;s callback, or simulate a user interaction, maybe as a part of a subroutine. There are few ways to do this, with slight differences in the behavior: directly, using <span itemprop="about">Javascript methods <code>call()</code>, <code>notify()</code> or <code>dispatchEvent()</code></span> &#8211; I&#8217;ve set up a commented demo Dialog that shows them all.</span><!--more-->
  </p>
  
  <h2>
    The Dialog
  </h2>
  
  <p>
    I&#8217;ve used a resource string to build the initial dialog (please scroll down to the post&#8217;s end to find the example completed with listeners, callbacks and button functions):
  </p>
  
  <pre class="theme:classic font-size:13 line-height:16 toolbar:1 nums:false whitespace-before:1 whitespace-after:1 lang:js decode:true" title="GUI">var windowRef = "dialog { \
	text: 'ScriptUI Events - www.davidebarranca.com', \
	orientation: 'column', \
	alignChildren: 'fill', \
	cbGroup: Group { \
		cbPanel: Panel { \
        preferredSize: ['155',''], \
			alignChildren: 'left', \
			cb1: Checkbox { text: 'CheckBox #1'}, \
			cb2: Checkbox { text: 'CheckBox #2'}, \
			cb3: Checkbox { text: 'CheckBox #3'}, \
			cb4: Checkbox { text: 'CheckBox #4'}, \
		}, \
		btGroup: Group { \
             preferredSize: ['170',''], \
			alignChildren: ['fill','center'], \
			orientation: 'column', \
			testCb1: Button { text: '#1 - onClick ()' }, \
			testCb2: Button { text: '#2 - notify ()' }, \
			testCb3: Button { text: '#2 - dispatchEvent ()' }, \
			testCb4: Button { text: '#1 - call (CB #3)' }, \
		}, \
	}, \
    spacer: Panel { size: [100,0]}, \
	rbGroup: Group { \
		rbPanel: Panel { \
        preferredSize: ['155',''], \
        alignChildren: 'left', \
			rb1: RadioButton { text: 'RadioButton #1', value: 'true'}, \
			rb2: RadioButton { text: 'RadioButton #2'}, \
		}, \
        btGroup: Group { \
             preferredSize: ['170',''], \
			alignChildren: ['fill','center'], \
			orientation: 'column', \
			testRb1: Button { text: 'Panel - dispatchEvent ()' }, \
			testRb2: Button { text: 'RB #2 - notify ()' }, \
		}, \
	}, \
    spacer: Panel { size: [100,0]}, \
    closeBtn: Button { text: 'Close' }\
}";

// Build the Window
var w = new Window(windowRef);
w.closeBtn.onClick = function() { w.close() };
w.show();</pre>
  
  <h2>
    Regular Listeners and Callbacks
  </h2>
  
  <p>
    Let&#8217;s code for the CheckBoxes a response to the <code>'click'</code> event in two different ways:
  </p>
  
  <pre class="theme:classic font-size:13 line-height:16 toolbar:1 nums:false whitespace-before:1 whitespace-after:1 lang:js decode:true" title="CheckBox Listeners">// Shortcuts for cb
var cb1 = w.cbGroup.cbPanel.cb1;
var cb2 = w.cbGroup.cbPanel.cb2;
var cb3 = w.cbGroup.cbPanel.cb3;

// CheckBox #1 onClick listener: no event is passed.
cb1.onClick = function(event) { 
	alert("Clicked checkbox: " + this.text + "\nValue: " + this.value);
	// uncomment the following alert to test that no event is passed:
	// alert("Using event.target.value: " + event.target.value); // Error: event is undefined
};

// CheckBox #2 'click' listener: an event is passed, 
// and the alert correctly shows the event.target.value
cb2.addEventListener('click', function(event) { 
	alert("Clicked checkbox: " + this.text + "\nValue: " + event.target.value);
});</pre>
  
  <p>
    The <strong>first CheckBox</strong> registers an <code>onClick</code> event: mind you, there&#8217;s no Event actually passed to the callback. If you uncomment the second alert, you see that <code>event.target.value</code> is undefined.
  </p>
  
  <p>
    The <strong>second CheckBox</strong> uses <code>addEventListener</code>, listening for the <code>'click'</code> event with an anonymous callback. Here, an actual Event is passed.
  </p>
  
  <p>
    For the <strong>RadioButtons</strong>, I&#8217;ve set up a callback triggered by clicks on the container (i.e. the Panel) level:
  </p>
  
  <pre class="theme:classic font-size:13 line-height:16 toolbar:1 nums:false whitespace-before:1 whitespace-after:1 lang:js decode:true" title="RadioButtons listener">// Shortcut for the RB panel
var rbPanel = w.rbGroup.rbPanel;

// RadioButton 'click' listener
// alerts the active RB text (works differently on PS and ESTK)
rbPanel.addEventListener('click', function(event) {
    for (var i = 0; i &lt; rbPanel.children.length; i++) {
        if (rbPanel.children[i].value == true) {
            alert("Active: " + rbPanel.children[i].text);
            return;
         }
     }
});</pre>
  
  <p>
    The loop finds the selected RadioButton and alerts its name: mind you, this works differently on <span itemprop="mentions" itemscope itemtype="http://schema.org/SoftwareApplication"><span itemprop="name">Photoshop</span></span> and ESTK because of the way they deal with event propagation.
  </p>
  
  <ul>
    <li>
      <strong>Photoshop</strong> gets the <code>'click'</code> on the Panel, lets it go through it, down to the RadioButton, then fires the callback &#8211; which evaluates the RadioButtons status and returns the name of the selected one: that is, the one you&#8217;ve clicked.
    </li>
    <li>
      <strong>ESTK</strong> gets the <code>'click'</code> on the Panel and immediately fires the callback, before allowing the <code>'click'</code> to reach the RadioButton: this way the alerted name isn&#8217;t the one of RadioButton you&#8217;ve clicked on, but the one that was selected before. When the callback is done, the <code>'click'</code> is allowed to propagate to the RadioButton, that is finally selected.
    </li>
  </ul>
  
  <h2>
    Simulated events: CheckBox
  </h2>
  
  <p>
    This is the code for the four checkbox&#8217;s buttons, which duty is to trigger the callbacks and sometimes simulate user interaction:
  </p>
  
  <pre class="theme:classic font-size:13 line-height:16 toolbar:1 nums:false whitespace-before:1 whitespace-after:1 lang:js decode:true" title="CheckBox Buttons">// Button: test the CB#1 callback via direct cb1.onClick() call.
// Result: the callback's alert shows, but the checkbox isn't toggled
w.cbGroup.btGroup.testCb1.onClick = function() { cb1.onClick() };

// Button: test the CB#1 callback via cb1.notify() call.
// Result: the callback's alert shows, and the checkbox is toggled
w.cbGroup.btGroup.testCb2.onClick = function() { cb2.notify('onClick') };
//w.cbGroup.btGroup.testCb2.onClick = function() { cb1.notify("onClick") }; // works as well

// Button: test the CB#2 callback via dispatchEvent() call.
// Result: the callback's alert shows, but the checkbox isn't toggled
w.cbGroup.btGroup.testCb3.onClick = function() { cb2.dispatchEvent(new UIEvent ('click')) };

// Button: uses the onClick.call() method of CB#1 passing the CB#2 as parameter
// Result: the CB#2 'borrows' the callback from the CB#1
// (i.e. the CB#2 becomes the callback's 'this')
w.cbGroup.btGroup.testCb4.onClick = function() { cb1.onClick.call(cb3) };</pre>
  
  <p>
    The <strong>first button</strong> directly calls the CheckBox #1 callback via <code>cb1.onClick()</code>. Mind you, <code>onClick()</code> is a regular method of the <code>cb1</code> component, so it can be called anytime like any other method of any other object. The callback is fired but the CheckBox is not toggled.
  </p>
  
  <p>
    Conversely, the <strong>second button</strong> uses <code>.notify()</code> to simulate the user interaction, as if the CheckBox #1 was actually clicked. As a result, its value (checked/unchecked, i.e. true/false) is toggled and the callback is fired. Mind you, the <code>notify()</code> parameter in this case is optional &#8211; not in other cases when you might want to simulate, say, a <code>Window.notify('onMove')</code> event.
  </p>
  
  <p>
    The <strong>third button</strong> acts on CheckBox #2, which registers the listener and the callback via <code>.addEventListener()</code>. To trigger its callback (since <code>.onClick()</code> doesn&#8217;t exist) you&#8217;ve to make the component dispatch a new Event via <code>.dispatchEvent()</code>: as a result the callback is fired but the CheckBox is not toggled. If you want to simulate user interaction, you can always rely on <code>.notify()</code>.
  </p>
  
  <p>
    The <strong>fourth button</strong> uses <code>.call()</code> to borrow from the CheckBox #1 the callback, and temporarily apply it to the CheckBox #3 (that hasn&#8217;t any <code>.onClick()</code> method). The <code>.call()</code> parameter <code>cb3</code> is used as a &#8220;<code>this</code>&#8221; reference: so when the function needs to evaluate this.value, it doesn&#8217;t refer anymore to <code>cb1</code> (the actual &#8220;owner&#8221; of the callback) but to <code>cb3</code>.<br /> As a result the alert is fired but the CheckBox isn&#8217;t toggled.
  </p>
  
  <h2>
    Simulated events: RadioButton
  </h2>
  
  <p>
    Things are similar when it comes to the RadioButtons:
  </p>
  
  <pre class="theme:classic font-size:13 line-height:16 toolbar:1 nums:false whitespace-before:1 whitespace-after:1 lang:js decode:true" title="RadioButton Buttons">// Button: test the RadioButton Panel via dispatchEvent()
// Result: triggers the Panel's callback and fires the alert
w.rbGroup.btGroup.testRb1.onClick = function() { rbPanel.dispatchEvent(new UIEvent ('click')) };

// Button: test the RadioButton #2 via notify()
// Result: selects the second RadioButton and fires the Panel's callback
w.rbGroup.btGroup.testRb2.onClick = function() { w.rbGroup.rbPanel.rb2.notify('onClick') };</pre>
  
  <p>
    The <strong>first button</strong> uses <code>.dispatchEvent()</code> on the container level (i.e. it&#8217;s the Panel that fires the <code>UIEvent</code>), so that the callback is triggered and the alert shown.
  </p>
  
  <p>
    The <strong>second button</strong> instead uses <code>.notify()</code> directly on the RadioButton simulating a user click. As a result, the RadioButton #2 is selected and the callback is fired (at least it is fired in Photoshop: ESTK ignores the callback for some reason).<br /> Mind you, <code>.addEventListener()</code> and <code>dispatchEvent()</code> uses <code>'click'</code> as the Event, while what you <code>notify()</code> is an <code>'onClick'</code>.
  </p>
  
  <h2>
    Conclusion and full script
  </h2>
  
  <p>
    Hopefully this has shed some light on the different possibilities you&#8217;ve to run or trigger callbacks and simulate user interaction with ScriptUI components. This appears to be a field where the <span itemprop="mentions">ExtendScript</span> implementation differs a lot among the host apps (ESTK, Photoshop, InDesign, etc…). I&#8217;ve tried out the above on ESTK and PS only, so please test your portings!
  </p>
  
  <p>
    <img class="aligncenter size-full wp-image-2150" alt="ScriptUI Events" src="http://localhost:8888/wp-content/uploads/2013/08/ScriptUI_Events.png" width="361" height="348" itemprop="image" srcset="http://localhost:8888/wp-content/uploads/2013/08/ScriptUI_Events.png 361w, http://localhost:8888/wp-content/uploads/2013/08/ScriptUI_Events-150x144.png 150w, http://localhost:8888/wp-content/uploads/2013/08/ScriptUI_Events-300x289.png 300w" sizes="(max-width: 361px) 100vw, 361px" />
  </p>
  
  <pre class="theme:classic font-size:13 line-height:16 toolbar:1 nums:false whitespace-before:1 whitespace-after:1 lang:js decode:true" title="ScriptUI Events - full script">// Davide Barranca - www.davidebarranca.com
// V1.0 - 2014.08.04

var windowRef = "dialog { \
    text: 'ScriptUI Events - www.davidebarranca.com', \
	orientation: 'column', \
	alignChildren: 'fill', \
	cbGroup: Group { \
		cbPanel: Panel { \
        preferredSize: ['155',''], \
			alignChildren: 'left', \
			cb1: Checkbox { text: 'CheckBox #1'}, \
			cb2: Checkbox { text: 'CheckBox #2'}, \
			cb3: Checkbox { text: 'CheckBox #3'}, \
			cb4: Checkbox { text: 'CheckBox #4'}, \
		}, \
		btGroup: Group { \
             preferredSize: ['170',''], \
			alignChildren: ['fill','center'], \
			orientation: 'column', \
			testCb1: Button { text: '#1 - onClick ()' }, \
			testCb2: Button { text: '#2 - notify ()' }, \
			testCb3: Button { text: '#2 - dispatchEvent ()' }, \
			testCb4: Button { text: '#1 - call (CB #3)' }, \
		}, \
	}, \
    spacer: Panel { size: [100,0]}, \
	rbGroup: Group { \
		rbPanel: Panel { \
        preferredSize: ['155',''], \
        alignChildren: 'left', \
			rb1: RadioButton { text: 'RadioButton #1', value: 'true'}, \
			rb2: RadioButton { text: 'RadioButton #2'}, \
		}, \
        btGroup: Group { \
             preferredSize: ['170',''], \
			alignChildren: ['fill','center'], \
			orientation: 'column', \
			testRb1: Button { text: 'Panel - dispatchEvent ()' }, \
			testRb2: Button { text: 'RB #2 - notify ()' }, \
		}, \
	}, \
    spacer: Panel { size: [100,0]}, \
    closeBtn: Button { text: 'Close' }\
}";

// Build the Window
var w = new Window(windowRef);

/* =======================================
   CheckBox listeners and callbacks
   ======================================= */

// Shortcuts for cb
var cb1 = w.cbGroup.cbPanel.cb1;
var cb2 = w.cbGroup.cbPanel.cb2;
var cb3 = w.cbGroup.cbPanel.cb3;

// CheckBox #1 onClick listener: no event is passed.
cb1.onClick = function(event) { 
	alert("Clicked checkbox: " + this.text + "\nValue: " + this.value);
	// uncomment the following alert to test that no event is passed:
	// alert("Using event.target.value: " + event.target.value); // Error: event is undefined
};

// CheckBox #2 'click' listener: an event is passed, 
// and the alert correctly shows the event.target.value
cb2.addEventListener('click', function(event) { 
	alert("Clicked checkbox: " + this.text + "\nValue: " + event.target.value);
});

// Button: test the CB#1 callback via direct cb1.onClick() call.
// Result: the callback's alert shows, but the checkbox isn't toggled
w.cbGroup.btGroup.testCb1.onClick = function() { cb1.onClick() };

// Button: test the CB#1 callback via cb1.notify() call.
// Result: the callback's alert shows, and the checkbox is toggled
w.cbGroup.btGroup.testCb2.onClick = function() { cb2.notify('onClick') };
//w.cbGroup.btGroup.testCb2.onClick = function() { cb1.notify("onClick") }; // works as well

// Button: test the CB#2 callback via dispatchEvent() call.
// Result: the callback's alert shows, but the checkbox isn't toggled
w.cbGroup.btGroup.testCb3.onClick = function() { cb2.dispatchEvent(new UIEvent ('click')) };

// Button: uses the onClick.call() method of CB#1 passing the CB#2 as parameter
// Result: the CB#2 'borrows' the callback from the CB#1
// (i.e. the CB#2 becomes the callback's 'this')
w.cbGroup.btGroup.testCb4.onClick = function() { cb1.onClick.call(cb3) };

/* =======================================
   RadioButton listeners and callback
   ======================================= */

// Shortcut for the RB panel
var rbPanel = w.rbGroup.rbPanel;

// RadioButton 'click' listener
// alerts the active RB text (works differently on PS and ESTK)
rbPanel.addEventListener('click', function(event) {
    for (var i = 0; i &lt; rbPanel.children.length; i++) {
        if (rbPanel.children[i].value == true) {
            alert("Active: " + rbPanel.children[i].text);
            return;
         }
     }
});

// Button: test the RadioButton Panel via dispatchEvent()
// Result: triggers the Panel's callback and fires the alert
w.rbGroup.btGroup.testRb1.onClick = function() { rbPanel.dispatchEvent(new UIEvent ('click')) }; 

// Button: test the RadioButton #2 via notify()
// Result: selects the second RadioButton and fires the Panel's callback
w.rbGroup.btGroup.testRb2.onClick = function() { w.rbGroup.rbPanel.rb2.notify() }; 

/* Close Button */
w.closeBtn.onClick = function() { w.close() };

w.show();</pre>
  
  <p>
    &nbsp;
  </p>
</div>

<!-- Share-Widget Button BEGIN --><a href="javascript:void(0);" myshare\_id="mys\_shareit" myshare\_url="http://localhost:8888/2013/08/extendscript-scriptui-events-call-notify-dispatchevent/" myshare\_title="ScriptUI Events: call(), dispatchEvent(), notify()" rel="nofollow" onclick=" return false;" style="text-decoration:none; color:#000000; font-size:11px; line-height:20px;"> 

<img src="http://localhost:8888/wp-content/plugins/share-widget/img/share-button-white-small.png" height="20" alt="Share" style="border:0" /> </a> <!-- Share-Widget Button END -->