---
title: " Vue.js – Binding a Component in a v-for loop to the Parent model\t\t"
tags:
  - binding
  - component
  - parent
  - vue.js
  - vuejs
url: 3006.html
id: 3006
category:
  - Coding
  - HTML Panels
  - VueJS
date: 2016-08-01 23:21:10
---

Learning Vue.js is fun – if I run into a problem that has taken me some head scratching time to solve and/or and no easy Stack Overflow answer, why not writing a blog post for you and my future self? :-) Today's stumbling block is bi-directionally binding of a Component (v-model), to the root data object – being the Components generated in a v-for loop. Sounds unclear? Think about a lot of instances of a Component containing, say, checkboxes or radiobuttons, automatically generated from an array. It's a quite frequent scenario, at least in my projects, so let's have a look. **UPDATE** \[October 2017\]: The article refers to Vue 1.x – with version 2, things have slightly changed – I've updated the Checkbox section with code working with Vue 2.5.2.

Checkboxes
----------

**\[See the Vue 2.5.2 update at the end of this section\]** I'm going to show you a couple of different setups and solutions. First initial arrangement is as follows:

<body id="main-container">
   
    <input type="checkbox"
           value="0" 
           id="boxes\[0\].name"            
           v-model="boxes\[0\].status"
    /\> zero

    <input type="checkbox"
           value="1" 
           id="boxes\[1\].name"            
           v-model="boxes\[1\].status"
    /\> one

    <input type="checkbox"
           value="boxes\[2\].name" 
           id="two"            
           v-model="boxes\[2\].status"
    /\> two

    <!\-\- etc -->

</body>

While the Javascript and the Vue code is:

var vm = new Vue({
    el: '#main-container',
    data: {
        boxes: \[
            {'name' : 'zero',  'status': false},
            {'name' : 'one',   'status': false},
            {'name' : 'two',   'status': false},
            {'name' : 'three', 'status': false},
            {'name' : 'four',  'status': true },
            {'name' : 'five',  'status': false},
            {'name' : 'six',   'status': false},
            {'name' : 'seven', 'status': false},
            {'name' : 'eight', 'status': false},
            {'name' : 'nine',  'status': false},
        \]
    }
});

In this scenario it is particularly important for me that the `v-model` of each checkbox is bound to the `boxes` array of objects, so that other functions are able to refer to them (say, passing their values down to the Photoshop JSX layer). But clearly the checkboxes cry to be implemented as Vue Components! So let's do that – first in the HTML as a template – that, by now, is mostly an empty placeholder:

<template id="boxes-template">
  <div>
    <input type="checkbox"
           value="" 
           id="" 
    />
  </div>
</template>

We need to register the Component in the JS, so:

var BoxesChecks = Vue.extend({
  props: \['boxIndex', 'boxName', 'boxStatus'\],
  template: '#boxes-template'
});

Vue.component('boxes-checks', BoxesChecks);

var vm = new Vue({
// etc. etc.
});

Please note that I'm using both camelCase and kebab-case, as the Vue.js doc suggests. I've declared three props:

*   `boxIndex`: will be 0, 1, 2, 3... 9.
*   `boxName`: the checkbox string label, like `"one"`, `"two"`, etc.
*   `boxStatus`: will be the Checkbox checked status (either true or false), and will be bound to the `data.boxes` Object.

OK, how do we implement the `v-for loop`, so that all the ten Components instances are rendered on the page? A good starting point is:

<boxes-checks 
    v-for="box in boxes"
>
</boxes-checks>

So far so good, nothing that you can't figure by yourself. Here comes the tricky part, have a look.

<template id="boxes-template">
  <div>
    <input type="checkbox"
           value="{ {boxIndex}}" 
           id="{ {boxName}}" 
           checked="{ {boxStatus}}"
    />
    \[{ {boxIndex}}\] { {boxName}} is: { {boxStatus}}
  </div>
</template>


<boxes-checks 
    v-for="box in boxes"
    :box-index="$index"
    :box-name="box.name"
    :box-status="box.status"
    
>
</boxes-checks>

{ {boxes | json }}

The checkbox `value` is `{ {boxIndex}}`, that is passed as a prop using the shorthand syntax `:box-index` (which stands for `v-bind:box-index`) and assigned to the special property provided by the `v-for` loop `$index`. Note here camelCase and kebab-case. Similarly, `{ {boxName}}` and `{ {boxStatus}}` are passed in the loop using `:box-name` and `:box-status`, and the latter is used to fill the `checked` property of the input tag. This way, the component "knows" whether to be initialized as checked / unchecked depending on the `status` property of the respective object in the `boxes` array. I've also added as a label for each box component a string containing them all, and at the bottom the JSON version of the boxes object to check if binding works properly. ![](http://localhost:8888/wp-content/uploads/2016/08/DB-2016-08-01-at-22.40.52-300x185.png)We're slowly getting there: each checkbox shows its index, name and status, and the one labelled "four" correctly initializes itself as "checked". But if you try clicking them, neither their `status`, (true/false in the label) nor the root `boxes` object updates. This binding requires a couple of new lines of code, one in the Component's template – i.e. a click handler:

<input type="checkbox"
           value="{ {boxIndex}}" 
           id="{ {boxName}}" 
           checked="{ {boxStatus}}"
           @click="changeStatus"/>

and the corresponding function in the Components declaration in the JS:

var BoxesChecks = Vue.extend({
  props: \['boxIndex', 'boxName', 'boxStatus'\],
  template: '#boxes-template',
  methods: {
    changeStatus: function() {
      this.boxStatus = !this.boxStatus;
    }
  }
});

![](http://localhost:8888/wp-content/uploads/2016/08/DB-2016-08-01-at-22.51.30-300x185.png)This triggers a change in the Component's `boxStatus` property, so that when you click each checkbox, you see that its own `status` (logged in the label) changes accordingly. Which is cool, but there's just one piece of the puzzle missing – can you find it? The Component's `boxStatus` is updated, but the Component (or better: each Component) has an **isolated scope of its own**! In fact, the boxes object, logged as JSON is not changed (every status is false but one). In order to make a two way binding, you need to add the binding type modifier .sync to the prop, like:

<boxes-checks 
    v-for="box in boxes"
    :box-index="$index"
    :box-name="box.name"
    :box-status.sync="box.status">
</boxes-checks>

This correctly set everything: each Component is initialized with the boxes object, and two-ways bound to it – the same way it was just before refactoring with Components. See the full code in the JsBin below: [JS Bin on jsbin.com](http://jsbin.com/simuzi/13/embed?html,js,output) (link to the bin on a larger page [here](http://jsbin.com/simuzi/13/edit?html,js,output)).

* * *

**Vue 2.x October 2017 UPDATE** With the new version, `.sync` has been first deprecated, then re-introduced (see [here](https://vuejs.org/v2/guide/components.html#sync-Modifier)). Yet, letting a child component modify parent data is considered an anti pattern in Vue: in this case, you should emit events and use a new Vue instance as a hub. See the following JSBin as an example: [JS Bin on jsbin.com](http://jsbin.com/niloxid/2/embed?html,js,output) (link to the bin on a larger page [here](https://jsbin.com/niloxid/2/edit?html,js,output)).

* * *

RadioButtons
------------

Following the same idea, let me show a different solution for RadioButtons, using events. The component looks like that:

<template id="channel-radio">
	<label class="topcoat-radio-button" style="display:block">
		<input type="radio" name="topcoat" 
               checked={ {channelChecked}}
               @click="changeRadio">
	  	<div class="topcoat-radio-button__checkmark" style="margin-bottom:4px"></div>
	    { {channelName}} \[{ {channelIndex}}\]
    </label>
</template>

<channel-radio
    v-for="channel in channels"
    :channel-name="channel.name"
    :channel-index="channel.index"
    :channel-checked="channel.checked"
    >
</channel-radio>

You see that there are `channelName`, `channelIndex` and `channelChecked` props, with a similar `changeRadio` function for the click handler. The loop is based upon a `channels` array, see the JS side:

var ChannelRadio = Vue.extend({
    props: \['channelName', 'channelIndex','channelChecked'\],
    template: '#channel-radio',
    methods: {
        changeRadio: function() {
            this.$dispatch('radioClick', this.channelName);
        }
    }
});

Vue.component('channel-radio', ChannelRadio);

var vm = new Vue({
    el: '#main-container',
    data: {
        channels: \[
            { 'name': 'Luminosity', 'index': 0, 'checked': false },
            { 'name': 'Red',        'index': 1, 'checked': true },
            { 'name': 'Green',      'index': 2, 'checked': false },
            { 'name': 'Blue',       'index': 3, 'checked': false }
        \]
    },
    events: {
        radioClick: 'handleRadioClick'
    },
    methods: {
        handleRadioClick: function(radioName) {
          for (var i = 0, \_len = this.channels.length; i < \_len; i++) {
            if (this.channels\[i\].name === radioName) {
              this.channels\[i\].checked = true;
            } else {
              this.channels\[i\].checked = false;
            }
          }
        }
    }
});

Clicking on each radiobutton component now dispatches a `'radioClick'` message (defined in the component's `methods` object, and carrying the `channelName` as the payload), handled by the root Vue instance (see both `events` and `methods` objects). The handleRadioClick then adjust the `checked` properties on each object within the `channels` array, that the radiobuttons are bound to – please note that because of this, there's no need to add `.sync` in the template now. See it in action here: [JS Bin on jsbin.com](http://jsbin.com/belele/embed?html,js,output) (link to the bin on a larger page [here](http://jsbin.com/belele/edit?html,js,output)). That's it! It took me a while to figure this out. Possibly the second example – where the parent (root, here) Vue instance is in charge of updating the data object, to which the Component is bound – is "more proper" than the first one – where the Component did it by itself. Hope this helps! Ciao.