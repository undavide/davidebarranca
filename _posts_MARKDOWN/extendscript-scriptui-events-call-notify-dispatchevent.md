---
title: " ScriptUI Events: call(), dispatchEvent(), notify() \t\t"
tags:
  - call()
  - callback
  - dispatchEvent()
  - Event
  - notify()
  - ScriptUI
url: 2136.html
id: 2136
category:
  - Coding
  - ExtendScript / Javascript
date: 2013-08-04 19:20:33
---

In your code you may need to run a ScriptUI component's callback, or simulate a user interaction, maybe as a part of a subroutine. There are few ways to do this, with slight differences in the behavior: directly, using Javascript methods `call()`, `notify()` or `dispatchEvent()` \- I've set up a commented demo Dialog that shows them all.

The Dialog
----------

I've used a resource string to build the initial dialog (please scroll down to the post's end to find the example completed with listeners, callbacks and button functions):

var windowRef = "dialog { \
	text: 'ScriptUI Events - www.davidebarranca.com', \
	orientation: 'column', \
	alignChildren: 'fill', \
	cbGroup: Group { \
		cbPanel: Panel { \
        preferredSize: \['155',''\], \
			alignChildren: 'left', \
			cb1: Checkbox { text: 'CheckBox #1'}, \
			cb2: Checkbox { text: 'CheckBox #2'}, \
			cb3: Checkbox { text: 'CheckBox #3'}, \
			cb4: Checkbox { text: 'CheckBox #4'}, \
		}, \
		btGroup: Group { \
             preferredSize: \['170',''\], \
			alignChildren: \['fill','center'\], \
			orientation: 'column', \
			testCb1: Button { text: '#1 - onClick ()' }, \
			testCb2: Button { text: '#2 - notify ()' }, \
			testCb3: Button { text: '#2 - dispatchEvent ()' }, \
			testCb4: Button { text: '#1 - call (CB #3)' }, \
		}, \
	}, \
    spacer: Panel { size: \[100,0\]}, \
	rbGroup: Group { \
		rbPanel: Panel { \
        preferredSize: \['155',''\], \
        alignChildren: 'left', \
			rb1: RadioButton { text: 'RadioButton #1', value: 'true'}, \
			rb2: RadioButton { text: 'RadioButton #2'}, \
		}, \
        btGroup: Group { \
             preferredSize: \['170',''\], \
			alignChildren: \['fill','center'\], \
			orientation: 'column', \
			testRb1: Button { text: 'Panel - dispatchEvent ()' }, \
			testRb2: Button { text: 'RB #2 - notify ()' }, \
		}, \
	}, \
    spacer: Panel { size: \[100,0\]}, \
    closeBtn: Button { text: 'Close' }\
}";

// Build the Window
var w = new Window(windowRef);
w.closeBtn.onClick = function() { w.close() };
w.show();

Regular Listeners and Callbacks
-------------------------------

Let's code for the CheckBoxes a response to the `'click'` event in two different ways:

// Shortcuts for cb
var cb1 = w.cbGroup.cbPanel.cb1;
var cb2 = w.cbGroup.cbPanel.cb2;
var cb3 = w.cbGroup.cbPanel.cb3;

// CheckBox #1 onClick listener: no event is passed.
cb1.onClick = function(event) { 
	alert("Clicked checkbox: " + this.text + "\\nValue: " + this.value);
	// uncomment the following alert to test that no event is passed:
	// alert("Using event.target.value: " + event.target.value); // Error: event is undefined
};

// CheckBox #2 'click' listener: an event is passed, 
// and the alert correctly shows the event.target.value
cb2.addEventListener('click', function(event) { 
	alert("Clicked checkbox: " + this.text + "\\nValue: " + event.target.value);
});

The **first CheckBox** registers an `onClick` event: mind you, there's no Event actually passed to the callback. If you uncomment the second alert, you see that `event.target.value` is undefined. The **second CheckBox** uses `addEventListener`, listening for the `'click'` event with an anonymous callback. Here, an actual Event is passed. For the **RadioButtons**, I've set up a callback triggered by clicks on the container (i.e. the Panel) level:

// Shortcut for the RB panel
var rbPanel = w.rbGroup.rbPanel;

// RadioButton 'click' listener
// alerts the active RB text (works differently on PS and ESTK)
rbPanel.addEventListener('click', function(event) {
    for (var i = 0; i < rbPanel.children.length; i++) {
        if (rbPanel.children\[i\].value == true) {
            alert("Active: " + rbPanel.children\[i\].text);
            return;
         }
     }
});

The loop finds the selected RadioButton and alerts its name: mind you, this works differently on Photoshop and ESTK because of the way they deal with event propagation.

*   **Photoshop** gets the `'click'` on the Panel, lets it go through it, down to the RadioButton, then fires the callback - which evaluates the RadioButtons status and returns the name of the selected one: that is, the one you've clicked.
*   **ESTK** gets the `'click'` on the Panel and immediately fires the callback, before allowing the `'click'` to reach the RadioButton: this way the alerted name isn't the one of RadioButton you've clicked on, but the one that was selected before. When the callback is done, the `'click'` is allowed to propagate to the RadioButton, that is finally selected.

Simulated events: CheckBox
--------------------------

This is the code for the four checkbox's buttons, which duty is to trigger the callbacks and sometimes simulate user interaction:

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

The **first button** directly calls the CheckBox #1 callback via `cb1.onClick()`. Mind you, `onClick()` is a regular method of the `cb1` component, so it can be called anytime like any other method of any other object. The callback is fired but the CheckBox is not toggled. Conversely, the **second button** uses `.notify()` to simulate the user interaction, as if the CheckBox #1 was actually clicked. As a result, its value (checked/unchecked, i.e. true/false) is toggled and the callback is fired. Mind you, the `notify()` parameter in this case is optional - not in other cases when you might want to simulate, say, a `Window.notify('onMove')` event. The **third button** acts on CheckBox #2, which registers the listener and the callback via `.addEventListener()`. To trigger its callback (since `.onClick()` doesn't exist) you've to make the component dispatch a new Event via `.dispatchEvent()`: as a result the callback is fired but the CheckBox is not toggled. If you want to simulate user interaction, you can always rely on `.notify()`. The **fourth button** uses `.call()` to borrow from the CheckBox #1 the callback, and temporarily apply it to the CheckBox #3 (that hasn't any `.onClick()` method). The `.call()` parameter `cb3` is used as a "`this`" reference: so when the function needs to evaluate this.value, it doesn't refer anymore to `cb1` (the actual "owner" of the callback) but to `cb3`. As a result the alert is fired but the CheckBox isn't toggled.

Simulated events: RadioButton
-----------------------------

Things are similar when it comes to the RadioButtons:

// Button: test the RadioButton Panel via dispatchEvent()
// Result: triggers the Panel's callback and fires the alert
w.rbGroup.btGroup.testRb1.onClick = function() { rbPanel.dispatchEvent(new UIEvent ('click')) };

// Button: test the RadioButton #2 via notify()
// Result: selects the second RadioButton and fires the Panel's callback
w.rbGroup.btGroup.testRb2.onClick = function() { w.rbGroup.rbPanel.rb2.notify('onClick') };

The **first button** uses `.dispatchEvent()` on the container level (i.e. it's the Panel that fires the `UIEvent`), so that the callback is triggered and the alert shown. The **second button** instead uses `.notify()` directly on the RadioButton simulating a user click. As a result, the RadioButton #2 is selected and the callback is fired (at least it is fired in Photoshop: ESTK ignores the callback for some reason). Mind you, `.addEventListener()` and `dispatchEvent()` uses `'click'` as the Event, while what you `notify()` is an `'onClick'`.

Conclusion and full script
--------------------------

Hopefully this has shed some light on the different possibilities you've to run or trigger callbacks and simulate user interaction with ScriptUI components. This appears to be a field where the ExtendScript implementation differs a lot among the host apps (ESTK, Photoshop, InDesign, etc…). I've tried out the above on ESTK and PS only, so please test your portings! ![ScriptUI Events](http://localhost:8888/wp-content/uploads/2013/08/ScriptUI_Events.png)

// Davide Barranca - www.davidebarranca.com
// V1.0 - 2014.08.04

var windowRef = "dialog { \
    text: 'ScriptUI Events - www.davidebarranca.com', \
	orientation: 'column', \
	alignChildren: 'fill', \
	cbGroup: Group { \
		cbPanel: Panel { \
        preferredSize: \['155',''\], \
			alignChildren: 'left', \
			cb1: Checkbox { text: 'CheckBox #1'}, \
			cb2: Checkbox { text: 'CheckBox #2'}, \
			cb3: Checkbox { text: 'CheckBox #3'}, \
			cb4: Checkbox { text: 'CheckBox #4'}, \
		}, \
		btGroup: Group { \
             preferredSize: \['170',''\], \
			alignChildren: \['fill','center'\], \
			orientation: 'column', \
			testCb1: Button { text: '#1 - onClick ()' }, \
			testCb2: Button { text: '#2 - notify ()' }, \
			testCb3: Button { text: '#2 - dispatchEvent ()' }, \
			testCb4: Button { text: '#1 - call (CB #3)' }, \
		}, \
	}, \
    spacer: Panel { size: \[100,0\]}, \
	rbGroup: Group { \
		rbPanel: Panel { \
        preferredSize: \['155',''\], \
        alignChildren: 'left', \
			rb1: RadioButton { text: 'RadioButton #1', value: 'true'}, \
			rb2: RadioButton { text: 'RadioButton #2'}, \
		}, \
        btGroup: Group { \
             preferredSize: \['170',''\], \
			alignChildren: \['fill','center'\], \
			orientation: 'column', \
			testRb1: Button { text: 'Panel - dispatchEvent ()' }, \
			testRb2: Button { text: 'RB #2 - notify ()' }, \
		}, \
	}, \
    spacer: Panel { size: \[100,0\]}, \
    closeBtn: Button { text: 'Close' }\
}";

// Build the Window
var w = new Window(windowRef);

/\* =======================================
   CheckBox listeners and callbacks
   ======================================= */

// Shortcuts for cb
var cb1 = w.cbGroup.cbPanel.cb1;
var cb2 = w.cbGroup.cbPanel.cb2;
var cb3 = w.cbGroup.cbPanel.cb3;

// CheckBox #1 onClick listener: no event is passed.
cb1.onClick = function(event) { 
	alert("Clicked checkbox: " + this.text + "\\nValue: " + this.value);
	// uncomment the following alert to test that no event is passed:
	// alert("Using event.target.value: " + event.target.value); // Error: event is undefined
};

// CheckBox #2 'click' listener: an event is passed, 
// and the alert correctly shows the event.target.value
cb2.addEventListener('click', function(event) { 
	alert("Clicked checkbox: " + this.text + "\\nValue: " + event.target.value);
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

/\* =======================================
   RadioButton listeners and callback
   ======================================= */

// Shortcut for the RB panel
var rbPanel = w.rbGroup.rbPanel;

// RadioButton 'click' listener
// alerts the active RB text (works differently on PS and ESTK)
rbPanel.addEventListener('click', function(event) {
    for (var i = 0; i < rbPanel.children.length; i++) {
        if (rbPanel.children\[i\].value == true) {
            alert("Active: " + rbPanel.children\[i\].text);
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

/\* Close Button */
w.closeBtn.onClick = function() { w.close() };

w.show();