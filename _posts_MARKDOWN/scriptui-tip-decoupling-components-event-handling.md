---
title: " ScriptUI tip: decoupling components' Event handling\t\t"
tags:
  - bubbling
  - Event
  - Extendscript
  - ScriptUI
url: 2181.html
id: 2181
category:
  - Coding
  - ExtendScript / Javascript
date: 2013-09-07 14:12:51
---

In a ScriptUI Window different components are usually registered for Events, and fire their own Handlers. You can build some interconnection, so for instance a Button's 'click' handler triggers a change in a ListBox, which in turn reacts to its own 'onChange' Event. It's quite easy to decouple this interaction, provided that you set up your code properly.

Example
-------

As a **starting point**, let's write some basic demo code to build a Window with just a ListBox and a Button. The ListBox contains five children items, and the Button randomly selects one of them.

var win = new Window('dialog');
win.lbx = win.add('listbox', undefined, undefined, {items:\['First Item', 'Second Item', 'Third Item', 'Fourth Item', 'Fifth Item'\]});
win.btn = win.add('button', undefined, "Select Random");
// ListBox handler
win.lbx.onChange = function () {
	$.writeln("ListBox onChange fired.");
}
// Button handler
win.btn.onClick = function () {
	$.writeln("Button clicked.");
	var i = Math.floor( 5 * Math.random());
	win.lbx.children\[i\].selected = true;
}
win.show();

As it is the code now, I've set a button `.onClick()` function that makes the ListBox elements selection switch at random, and a ListBox `.onChange()` function just logging a message in the Console. If you click few times the button, the ListBox elements selection changes: the super-simple flowchart is:

![Coupled Events](http://localhost:8888/wp-content/uploads/2013/09/Coupled_Events.png)

and the Console will contain something like:

Button clicked.
ListBox onChange fired.
Button clicked.
Button clicked.
Button clicked.
ListBox onChange fired.
Button clicked.
ListBox onChange fired.
Button clicked.
ListBox onChange fired.

Mind you: when it happens, randomly, that the selected item is the same, the "ListBox onChange fired" message isn't output because there's no actual change in the ListBox itself. While if you just manually select different items in the Listbox: ![ListBox Event](http://localhost:8888/wp-content/uploads/2013/09/ListBox_Event.png) This is what is logged then:

ListBox onChange fired.
ListBox onChange fired.
ListBox onChange fired.
ListBox onChange fired.

Say that, instead, **you want the ListBox to respond to 'change' events only when you directly interact with it**. That is, the Button will keep to change the selection, but it doesn't have to trigger the ListBox handler anymore, this way: ![Decoupled Events](http://localhost:8888/wp-content/uploads/2013/09/Decoupled_Events.png) That is to say, the two Events must be decoupled. In order to make things work this way you've to rewrite some code.

var win = new Window('dialog');
win.lbx = win.add('listbox', undefined, undefined, {items:\['First Item', 'Second Item', 'Third Item', 'Fourth Item', 'Fifth Item'\]});
win.btn = win.add('button', undefined, "Select Random");

win.lbx.addEventListener('change', function(event) {
	$.writeln("ListBox Change fired");
}, true ); // Responds only in the capture phase, not bubbling

win.btn.addEventListener('click', function(event){
	var i = Math.floor( 5* Math.random() );
	win.lbx.children\[i\].selected = true;
    $.writeln("Clicked Button");
}, false );

win.show()

First, `.onClick()` and `.onChange()` don't actually have any Event object available to the function (which may be handy), while `.addEventListener()` provides you with one. Second, using `.addEventListener()` lets you specify a third argument, which is a Boolean that controls whether the handler responds only in the **Capture Phase** rather than the **Bubbling Phase** (for details about Events propagation see pag. 84 of the JavaScript Tools Guide CC). Default is false. Bubbling is mostly used, as far as I understand, to diversify handlers in situations when, say, a container such as a Group has several Checkboxes in it. You can attach a handler to the Group, so that the click on every checkbox can "bubble" through it (and trigger a response), and attach a different handler on each checkbox, which fires its own peculiar function. Here, I've been using the bubbling boolean to control the Event propagation, particularly:

win.lbx.addEventListener('change', function(event) {
	$.writeln("ListBox Change fired");
}, true ); // Responds only in the capture phase, not bubbling

That 'true' means, so to speak, that the `'change'` event triggers the anonymous callback it's associated with only when the user directly interact with the ListBox - i.e. selecting items. Conversely, the `'change'` event in the ListBox caused by the Button function is ignored. And finally the Console logs only:

Button clicked.
Button clicked.
Button clicked.
Button clicked.

Update (and correction)
-----------------------

Apparently I've been either too lazy to check or completely drunk not to notice that the above example doesn't entirely work. True, the Button click makes the ListBox selection change and doesn't fires the `'change'` event, yet when you manually click items in the list (i.e. change them) the list's handler doesn't fire either. Which is a problem, because the whole rationale of this post is about decoupling the events (and to kill one isn't quite the right way to do it). The workaround involves a deeper understanding of Events propagation. Quoting a short excerpt from the Documentation:

> **Capture phase** — When an event occurs in an object hierarchy, it is captured by the topmost ancestor object at which a handler is registered (the window, for example). If no handler is registered for the topmost ancestor, ScriptUI looks for a handler for the next ancestor (the dialog, for example), on down through the hierarchy to the direct parent of actual target. When ScriptUI finds a handler registered for any ancestor of the target, it executes that handler then proceeds to the next phase. **At-target phase** — ScriptUI calls any handlers that are registered with the actual target object. **Bubble phase** — The event bubbles back out through the hierarchy; ScriptUI again looks for handlers registered for the event with ancestor objects, starting with the immediate parent, and working back up the hierarchy to the topmost ancestor. When ScriptUI finds a handler, it executes it and the event propagation is complete.

That said, I've worked around the problem nesting the ListBox in a Group (with its own handler):

var win = new Window('dialog');
win.grp = win.add('group');
win.grp.lbx = win.grp.add('listbox', undefined, undefined, {items:\['First Item', 'Second Item', 'Third Item', 'Fourth Item', 'Fifth Item'\]});
win.btn = win.add('button', undefined, "Select Random");

win.grp.addEventListener('click', function(event) { 
    $.writeln("ListBox 'change' Handler \[" + event.eventPhase + "\]") 
}, true ); 

win.btn.addEventListener('click', function(event){
    var i = Math.floor( 5* Math.random() );
    win.grp.lbx.children\[i\].selected = true;
    $.writeln("Clicked Button");
}, false );

win.show()

The Group listens for `'click'`, (if you set the third argument to true, it will report the event phase as _capture_, otherwise as _bubble_), so it intercepts the clicks first and fires the handler, which can't be triggered by the button. It works, but even if the ListBox doesn't change (i.e. you click the same item that is already selected), it fires the handler anyway.