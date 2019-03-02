---
title: " Spectrum CSS Vue.js Component: DropDown\t\t"
tags:
  - CSS
  - spectrum
  - vue.js
url: 3378.html
id: 3378
category:
  - Coding
  - HTML Panels
  - VueJS
date: 2018-12-16 11:19:58
---

In a [previous post](http://localhost:8888/2018/11/adobe-spectrum-css-open-sourced/) I've introduced the recently open-sourced Spectrum CSS. Here, I'll be demonstrating how to use them to build a simple Vue.js component: a DropDown menu.

Spectrum CSS are really appealing, but as I've mentioned [earlier](http://localhost:8888/2018/11/adobe-spectrum-css-open-sourced/), they're not bundled with any JavaScript – besides what you may borrow from the [online documentation](http://opensource.adobe.com/spectrum-css/2.7.2/docs/) page. While elements such as Sliders are _plug & play_, for they actually do use a `<input type="range">` wrapped in a lot of finely styled `<div>` tags, others such as the DropDown are not.

![](http://localhost:8888/wp-content/uploads/2018/12/dropdown-558x700.gif)

Screenshot from the Spectrum CSS documentation

I.e. if you click on a Closed dropdown, it won't open (the above is just a screenshot so it's pointless to try, but the result would be the same). The reason being that a Spectrum DropDown is not really a `<select>` element, as you can see in the snippet below:

![](http://localhost:8888/wp-content/uploads/2018/12/code-700x581.png)

The Spectrum code that renders a DropDown

As far as I get, re-creating an element from scratch is a standard practice to avoid rendering differences between browsers (the `<select>` is a good example here).

Anyways: no `<select>` means no default behavior on click, change, etc. In other words, you are the one in charge of populating the `<div>` elements above and adding relevant event handlers so that the DropDown stops being a nice but still and useless object.

This is a perfect test case for encapsulating everything in a **Vue.js Component**! The only drawback is that I don't feel particularly comfortable with the idea of creating Components from scratch. All my CEP Panels use Vue.js now, that's true, but frankly in a very simple and not over-engineered fashion: I've always used one shared `data`, the farther I've ventured into fanciness was setting up an Event Hub.

Thanks god I've a PhD in _Copy&Paste_, so I've been able to adapt [this one](https://vuejsexamples.com/a-prettier-way-to-display-select-boxes/):

![](http://localhost:8888/wp-content/uploads/2018/12/display-select-boxes.gif)

... into a proper Vue Component that wraps the Spectrum DropDown original markup. The `.vue` file I came up with is as follows:

I won't go too much into the details here. If you look at the `<template>` tag and compare it with the original Spectrum html, you can spot the differences: I've added a couple of classes that depend on the selected item or the `showMenu`boolean, and used a `v-for` loop to populate the options.

The `<script>` tag is where the actual logic belongs: I've added two methods, one to show the opened dropdown and one to emit an event to the parent when something is selected. From the `props` you're able to tell how to consume such element, e.g.:

<drop-down :options="arrayOfObjects" v-on:updateOption="methodToRunOnSelect" placeholder="Select a thing..."/>

The result is this one:

![](http://localhost:8888/wp-content/uploads/2018/12/WorkingDropDown.gif)

The working DropDown Vue.js Component  
(wife was unimpressed)

It is quite bare, I may want to add transitions and such... but it works!

To tell you the truth, I thought I couldn't be able to make it: I had a look at proper Vue.js UI Kits (like [this one](https://vuikit.js.org/)) and they exceed, by far, my understanding. Luckily I've been able to borrow code and adapt it, so I may be doing it again in the future for other elements that I would need.

If you're up for the same thing, or you know how to make my code better, please let me know in the comments. Bye!