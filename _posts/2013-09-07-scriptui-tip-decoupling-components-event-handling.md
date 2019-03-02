---
id: 2181
title: 'ScriptUI tip: decoupling components&#8217; Event handling'
date: 2013-09-07T14:12:51+01:00
author: Davide Barranca
excerpt: "In a ScriptUI Window different components are usually registered for Events, and fire their own Handlers. You can build some interconnection, so for instance a Button's 'click' handler triggers a change in a ListBox, which in turn reacts to its own 'onChange' Event. It's quite easy to decouple this interaction, provided that you set up your code properly."
layout: post
guid: http://www.davidebarranca.com/?p=2181
permalink: /2013/09/scriptui-tip-decoupling-components-event-handling/
standard_seo_post_level_layout:
  - ""
standard_link_url_field:
  - ""
standard_seo_post_meta_description:
  - "How to use the addEventListener() capturePhase argument to control Event propagation and decouple chained components' handling."
image: /wp-content/uploads/2013/09/ScriptUI_Event_propagation.png
category:
  - Coding
  - ExtendScript / Javascript
tags:
  - bubbling
  - Event
  - Extendscript
  - ScriptUI
---
<div class="pf-content">
  <p>
    In a ScriptUI Window different components are usually registered for Events, and fire their own Handlers. You can build some interconnection, so for instance a Button&#8217;s &#8216;click&#8217; handler triggers a change in a ListBox, which in turn reacts to its own &#8216;onChange&#8217; Event. It&#8217;s quite easy to decouple this interaction, provided that you set up your code properly.<!--more-->
  </p>

  <h2>
    Example
  </h2>

  <p>
    As a <strong>starting point</strong>, let&#8217;s write some basic demo code to build a Window with just a ListBox and a Button. The ListBox contains five children items, and the Button randomly selects one of them.
  </p>

  <pre class="lang:default decode:true">var win = new Window('dialog');
win.lbx = win.add('listbox', undefined, undefined, {items:['First Item', 'Second Item', 'Third Item', 'Fourth Item', 'Fifth Item']});
win.btn = win.add('button', undefined, "Select Random");
// ListBox handler
win.lbx.onChange = function () {
	$.writeln("ListBox onChange fired.");
}
// Button handler
win.btn.onClick = function () {
	$.writeln("Button clicked.");
	var i = Math.floor( 5 * Math.random());
	win.lbx.children[i].selected = true;
}
win.show();</pre>

  <p>
    As it is the code now, I&#8217;ve set a button <code>.onClick()</code> function that makes the ListBox elements selection switch at random, and a ListBox <code>.onChange()</code> function just logging a message in the Console.
  </p>

  <p>
    If you click few times the button, the ListBox elements selection changes: the super-simple flowchart is:
  </p>

  <p style="text-align: center;">
    <img class="aligncenter" alt="Coupled Events" src="/wp-content/uploads/2013/09/Coupled_Events.png" width="627" height="287" />
  </p>

  <p>
    and the Console will contain something like:
  </p>

  <pre class="lang:default highlight:0 decode:true">Button clicked.
ListBox onChange fired.
Button clicked.
Button clicked.
Button clicked.
ListBox onChange fired.
Button clicked.
ListBox onChange fired.
Button clicked.
ListBox onChange fired.</pre>

  <p>
    Mind you: when it happens, randomly, that the selected item is the same, the &#8220;ListBox onChange fired&#8221; message isn&#8217;t output because there&#8217;s no actual change in the ListBox itself.
  </p>

  <p>
    While if you just manually select different items in the Listbox:
  </p>

  <p>
    <img class="aligncenter size-full wp-image-2184" alt="ListBox Event" src="/wp-content/uploads/2013/09/ListBox_Event.png" width="627" height="116" srcset="/wp-content/uploads/2013/09/ListBox_Event.png 627w, /wp-content/uploads/2013/09/ListBox_Event-150x27.png 150w, /wp-content/uploads/2013/09/ListBox_Event-300x55.png 300w" sizes="(max-width: 627px) 100vw, 627px" />
  </p>

  <p>
    This is what is logged then:
  </p>

  <pre class="lang:default highlight:0 decode:true">ListBox onChange fired.
ListBox onChange fired.
ListBox onChange fired.
ListBox onChange fired.</pre>

  <p>
    Say that, instead, <strong>you want the ListBox to respond to &#8216;change&#8217; events only when you directly interact with it</strong>. That is, the Button will keep to change the selection, but it doesn&#8217;t have to trigger the ListBox handler anymore, this way:
  </p>

  <p>
    <img class="aligncenter size-full wp-image-2185" alt="Decoupled Events" src="/wp-content/uploads/2013/09/Decoupled_Events.png" width="627" height="230" srcset="/wp-content/uploads/2013/09/Decoupled_Events.png 627w, /wp-content/uploads/2013/09/Decoupled_Events-150x55.png 150w, /wp-content/uploads/2013/09/Decoupled_Events-300x110.png 300w" sizes="(max-width: 627px) 100vw, 627px" />
  </p>

  <p>
    That is to say, the two Events must be decoupled. In order to make things work this way you&#8217;ve to rewrite some code.
  </p>

  <pre class="lang:default decode:true">var win = new Window('dialog');
win.lbx = win.add('listbox', undefined, undefined, {items:['First Item', 'Second Item', 'Third Item', 'Fourth Item', 'Fifth Item']});
win.btn = win.add('button', undefined, "Select Random");

win.lbx.addEventListener('change', function(event) {
	$.writeln("ListBox Change fired");
}, true ); // Responds only in the capture phase, not bubbling

win.btn.addEventListener('click', function(event){
	var i = Math.floor( 5* Math.random() );
	win.lbx.children[i].selected = true;
    $.writeln("Clicked Button");
}, false );

win.show()</pre>

  <p>
    First, <code>.onClick()</code> and <code>.onChange()</code> don&#8217;t actually have any Event object available to the function (which may be handy), while <code>.addEventListener()</code> provides you with one.
  </p>

  <p>
    Second, using <code>.addEventListener()</code> lets you specify a third argument, which is a Boolean that controls whether the handler responds only in the <strong>Capture Phase</strong> rather than the <strong>Bubbling Phase</strong> (for details about Events propagation see pag. 84 of the JavaScript Tools Guide CC). Default is false.
  </p>

  <p>
    Bubbling is mostly used, as far as I understand, to diversify handlers in situations when, say, a container such as a Group has several Checkboxes in it. You can attach a handler to the Group, so that the click on every checkbox can &#8220;bubble&#8221; through it (and trigger a response), and attach a different handler on each checkbox, which fires its own peculiar function.
  </p>

  <p>
    Here, I&#8217;ve been using the bubbling boolean to control the Event propagation, particularly:
  </p>

  <pre class="lang:default mark:3 decode:true">win.lbx.addEventListener('change', function(event) {
	$.writeln("ListBox Change fired");
}, true ); // Responds only in the capture phase, not bubbling</pre>

  <p>
    That &#8216;true&#8217; means, so to speak, that the <code>'change'</code> event triggers the anonymous callback it&#8217;s associated with only when the user directly interact with the ListBox &#8211; i.e. selecting items. Conversely, the <code>'change'</code> event in the ListBox caused by the Button function is ignored. And finally the Console logs only:
  </p>

  <pre class="lang:default highlight:0 decode:true">Button clicked.
Button clicked.
Button clicked.
Button clicked.</pre>

  <h2>
    Update (and correction)
  </h2>

  <p>
    Apparently I&#8217;ve been either too lazy to check or completely drunk not to notice that the above example doesn&#8217;t entirely work. True, the Button click makes the ListBox selection change and doesn&#8217;t fires the <code>'change'</code> event, yet when you manually click items in the list (i.e. change them) the list&#8217;s handler doesn&#8217;t fire either. Which is a problem, because the whole rationale of this post is about decoupling the events (and to kill one isn&#8217;t quite the right way to do it).
  </p>

  <p>
    The workaround involves a deeper understanding of Events propagation. Quoting a short excerpt from the Documentation:
  </p>

  <blockquote>
    <p>
      <strong>Capture phase</strong> — When an event occurs in an object hierarchy, it is captured by the topmost ancestor object at which a handler is registered (the window, for example). If no handler is registered for the topmost ancestor, ScriptUI looks for a handler for the next ancestor (the dialog, for example), on down through the hierarchy to the direct parent of actual target. When ScriptUI finds a handler registered for any ancestor of the target, it executes that handler then proceeds to the next phase.<br /> <strong>At-target phase</strong> — ScriptUI calls any handlers that are registered with the actual target object.<br /> <strong>Bubble phase</strong> — The event bubbles back out through the hierarchy; ScriptUI again looks for handlers registered for the event with ancestor objects, starting with the immediate parent, and working back up the hierarchy to the topmost ancestor. When ScriptUI finds a handler, it executes it and the event propagation is complete.
    </p>
  </blockquote>

  <p>
    That said, I&#8217;ve worked around the problem nesting the ListBox in a Group (with its own handler):
  </p>

  <pre class="lang:default decode:true ">var win = new Window('dialog');
win.grp = win.add('group');
win.grp.lbx = win.grp.add('listbox', undefined, undefined, {items:['First Item', 'Second Item', 'Third Item', 'Fourth Item', 'Fifth Item']});
win.btn = win.add('button', undefined, "Select Random");

win.grp.addEventListener('click', function(event) {
    $.writeln("ListBox 'change' Handler [" + event.eventPhase + "]")
}, true );

win.btn.addEventListener('click', function(event){
    var i = Math.floor( 5* Math.random() );
    win.grp.lbx.children[i].selected = true;
    $.writeln("Clicked Button");
}, false );

win.show()</pre>

  <p>
    The Group listens for <code>'click'</code>, (if you set the third argument to true, it will report the event phase as <em>capture</em>, otherwise as <em>bubble</em>), so it intercepts the clicks first and fires the handler, which can&#8217;t be triggered by the button. It works, but even if the ListBox doesn&#8217;t change (i.e. you click the same item that is already selected), it fires the handler anyway.
  </p>

  <p>
    &nbsp;
  </p>
</div>

<!-- Share-Widget Button BEGIN --><a href="javascript:void(0);" myshare\_id="mys\_shareit" myshare\_url="/2013/09/scriptui-tip-decoupling-components-event-handling/" myshare\_title="ScriptUI tip: decoupling components&#8217; Event handling" rel="nofollow" onclick=" return false;" style="text-decoration:none; color:#000000; font-size:11px; line-height:20px;">
