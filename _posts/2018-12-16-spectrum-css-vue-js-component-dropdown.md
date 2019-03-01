---
title: "Spectrum CSS VueJS Component: DropDown"
date: 2018-12-16T11:19:58+01:00
author: Davide Barranca
excerpt: In a previous post I have introduced the recently open-sourced Spectrum CSS. Here, I'll be demonstrating how to use them to build a simple VueJS DropDown component
layout: post
permalink: /2018/12/spectrum-css-vue-js-component-dropdown/
description: How to use the open-sourced Spectrum CSS to build a simple VueJS DropDown component
image: /wp-content/uploads/2018/12/vuespectrum.png
categories:
  - CEP
tags:
  - CEP
  - Vue.js
---

In a [previous post](/2018/11/adobe-spectrum-css-open-sourced/) I've introduced the recently open-sourced Spectrum CSS. Here, I'll be demonstrating how to use them to build a simple Vue.js component: a DropDown menu.

Spectrum CSS are really appealing, but as I've mentioned [earlier](/2018/11/adobe-spectrum-css-open-sourced/), they're not bundled with any JavaScript – besides what you may borrow from the [online documentation](http://opensource.adobe.com/spectrum-css) page. While elements such as Sliders are _plug & play_, for they actually do use a `<input type="range">` wrapped in a lot of finely styled `<div>` tags, others such as the DropDown are not.

<figure>
	<img width="300" src="/wp-content/uploads/2018/12/dropdown.gif" />
	<figcaption>Screenshot from the Spectrum CSS documentation</figcaption>
</figure>

I.e. if you click on a Closed dropdown, it won't open (the above is just a screenshot so it's pointless to try, but the result would be the same). The reason being that a Spectrum DropDown is not really a `<select>` element, as you can see in the snippet below:

![The Spectrum code that renders a DropDown](/wp-content/uploads/2018/12/code.png)

As far as I get, re-creating an element from scratch is a standard practice to avoid rendering differences between browsers (the `<select>` is a good example here).

Anyways: no `<select>` means no default behavior on click, change, etc. In other words, you are the one in charge of populating the `<div>` elements above and adding relevant event handlers so that the DropDown stops being a nice but still and useless object.

This is a perfect test case for encapsulating everything in a **Vue.js Component**! The only drawback is that I don't feel particularly comfortable with the idea of creating Components from scratch. All my CEP Panels use Vue.js now, that's true, but frankly in a very simple and not over-engineered fashion: I've always used one shared `data`, the farther I've ventured into fanciness was setting up an Event Hub.

Thanks god I've a PhD in _Copy&Paste_, so I've been able to adapt [this one](https://vuejsexamples.com/a-prettier-way-to-display-select-boxes/):

![](/wp-content/uploads/2018/12/display-select-boxes.gif)

... into a proper Vue Component that wraps the Spectrum DropDown original markup. The `.vue` file I came up with is as follows:

{% gist ec5c7ef0d83b5d14252a963e761896f7 %}

I won't go too much into the details here. If you look at the `<template>` tag and compare it with the original Spectrum html, you can spot the differences: I've added a couple of classes that depend on the selected item or the `showMenu`boolean, and used a `v-for` loop to populate the options.

The `<script>` tag is where the actual logic belongs: I've added two methods, one to show the opened dropdown and one to emit an event to the parent when something is selected. From the `props` you're able to tell how to consume such element, e.g.:

{% highlight html %}
<drop-down :options="arrayOfObjects" v-on:updateOption="methodToRunOnSelect" placeholder="Select a thing..."/>
{% endhighlight %}

The result is this one:

<figure>
	<img width="300" src="/wp-content/uploads/2018/12/WorkingDropDown.gif" />
	<figcaption>The working DropDown Vue.js Component! (Wife was unimpressed)</figcaption>
</figure>

It is quite bare, I may want to add transitions and such... but it works!

To tell you the truth, I thought I couldn't be able to make it: I had a look at proper Vue.js UI Kits (like [this one](https://vuikit.js.org/)) and they exceed, by far, my understanding. Luckily I've been able to borrow code and adapt it, so I may be doing it again in the future for other elements that I would need.

If you're up for the same thing, or you know how to make my code better, please let me know in the comments. Bye!
