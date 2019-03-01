---
title: 'HTML Panel Tips #20: Javascript Frameworks'
date: 2016-06-10T00:07:42+01:00
author: Davide Barranca
excerpt: "I've tackled the issue of JS frameworks in a dedicated chapter of my Photoshop HTML Panels Development course, but I'd like to add some new thoughts here as well."
layout: post
permalink: /2016/06/html-panel-tips-20-javascript-frameworks-angularjs-vuejs/
description: "What is the 'best' Javascript framework for Adobe Photoshop HTML Panels? I review AngularJS and Vue.js as two of the options that I've used."
image: /wp-content/uploads/2016/06/logo.png
categories:
  - CEP
tags:
  - HTML Panels Tips
  - Angular
  - "Vue.js"
---

Javascript definitely has a problem: too many frameworks! But as my Color Correction maestro Dan Margulis would put it: _"the opposite problem might be much worse"_. I've tackled the issue of JS frameworks in a dedicated chapter of my [Photoshop HTML Panels Development course](http://htmlpanelsbook.com), but I'd like to add some new thoughts here as well.

This is an _atypical Tip:_ the topic needs more than a quick coverage so bear with me and read along. Also, note that I'm not, strictly speaking, a front-end JS expert: I know how to put together _some_ stuff, but I'm not really into Webpack, Babel, EcmaScript 2015, etc. so I've a pragmatic approach. Coming from Flash Panels and the Olav Kvern school of [blogposts](http://blogs.adobe.com/cssdk/author/olav-kvern), I've been using MVC patterns in ActionScript; like many others, I've found myself stuck in a dead end when wind changed and turned to HTML. As Matthias Petter Johansson predicts in an entertaining video appropriately called [Too many tools and frameworks](https://www.youtube.com/watch?v=DFP6UDgVJtE) I chose my framework based on the combination of **Popularity** (how much it is widespread and adopted) and **Authority** (who's build it) – you can't fail this way, can you? So I went with Google AngularJS.

## AngularJS

Being a self-taught JS developer, the framework learning curve was quite steep: so I bought courses, books etc. and started climbing mount Angular. I've used it in some of my commercial products and, as I've mentioned, I've also built a dedicated demo panel in AngularJS for my [Photoshop HTML Panels Development course](http://htmlpanelsbook.com). Per se, that Panel is not rocket science: it scans your PS open document and seeks for Text layers, retrieving the text content and letting you update it. It features three tabs (just a pretext to use routing) and looks like that:

[![AngularPanel](/wp-content/uploads/2016/06/AngularPanel.png)](http://htmlpanelsbook.com)

Yet I've built it in AngularJS trying to use all the best practices I knew: separation of concerns, modularity, you name it. I ended up with a code by all means exaggerated for that little panel, but which architecture can scale and support much more complex and elaborated projects. You can have a look at the dependencies schema used in this image.

[![schema](/wp-content/uploads/2016/06/schema.png)](http://htmlpanelsbook.com)

If you're familiar with AngularJS you can understand what's in there – I won't get through it in this very post: the full Panel's code, with detailed explanations, is available for download alongside in a **28 custom made panels** bundle with the [Panels course](http://htmlpanelsbook.com) – you can borrow it and use it in your own projects if you want.

Now, after having written that chapter, I ended up using the very same architecture in an actual, commercial product: a contract job for a Panel that looks apparently simple (but it's not 😶). Compared to other jobs I did in the past, this one features a great amount of **modularity**: possibly it wasn't totally needed _when I build it_, but being a contract job I've preferred to overdo when coding it, and then ensure hopefully easier updates in the future. As a compulsive new technologies addicted, I have now to face the advent of **AngularJS v2**, which scrambles all the rules of v1 and is an entirely different beast. Mind you, I went with Angular from the beginning mostly because I like learning new stuff, and for knowing a framework such as AngularJS might be useful for my career apart from HTML Panels (wishful thinking).

Mind you, **I can stay with v1 for a long long time!** Stuff evolves so rapidly in JS land that by the time Angular v1 is ultimately unsupported, new players will have hit the field: so basically... why bother now (besides, Adobe _isn't the fastest company_ in updating SDK). Also, I'm not a full-time developer and another steep learning curve will eat time that (a) I don't have, and (b) that I can spend more profitably, i.e. writing another book. Angular v2 is better experienced using TypeScript – I can barely stand CoffeeScript as a language to compile to JS, I'm sorry but another strongly typed language is not in my plans. So... I've taken the chance to get accustomed to a valid alternative (at least for HTML Panels) that I'd like to suggest you.

## VueJS

Let's start with downsides: ~~it isn't as widespread as AngularJS~~ (note by my future self from 2019: it really is now!!) or the other framework that everybody's excited about (**React** – which frankly I don't know, but the article "[Your Timeline for Learning React](https://daveceddia.com/timeline-for-learning-react/)" is enough to make me skip it altogether – if you really think I should get into React instead, please let me know in the comments the reason), nonetheless [Vue.js](http://vuejs.org/) has some respectable 20K stars on [Github](https://github.com/vuejs). Another concern may be that Vue.js is a personal project: yes, it's mainly built by [Evan You](http://blog.evanyou.me/), not an enterprise backed developers team. If you can live with that... Vue.js has:

*   an amazing community: apparently it's very popular among Laravel developers
*   very good documentation: IMHO uber-important factor when choosing a framework
*   reactive data binding: rendered templates are always up-to-date
*   components
*   routing

and much more, that I won't cover here 'cause frankly I can't understand it yet – I've just started approaching the framework... How sad is truth, isn't it?! Vue.js implements a **MVVM** (Model - View - ViewModel) pattern, which is considered a good choice for user interfaces (like... Panels!).

One of the most understandable articles on MVVM is by [Addy Osmani](https://addyosmani.com/blog/understanding-mvvm-a-guide-for-javascript-developers/), and I don't know why, this doesn't surprise me. Let's have a look at a dead simple example: In this case, the **View** is the templating in the HTML, the **Model** is the `model` object (a regular, plain JS object) and the **ViewModel** is the Vue instance that binds the View (here the `"app"` element – think about it as the Vue's version of Angular `ng-app`), and binds the Model.

_Reactivity_ means that there's no need to update the view if/when the model changes – as soon as `model` is updated, the view will reflect the new value. This leads to two-way data binding, such as: Here you see `v-model` (equivalent to Angular `ng-model`) and the use of a JSON filter with `$data`. Refactoring the same example with modular ViewModel (usually called just vm) you can see that it's really the data that drives everything, so if you need two Vue instances – and their parent – to share the same model, just **share it by identity**: In the above example, the `globalModel` var contains _reactive_ data that is used by both `vmRadius` and `vmOpacity`. Extending further the idea, each vm can bind model data that is both shared and private, like: If you need it, you can always use a PubSub pattern with `$on`, `$emit` and `$broadcast`, similarly to Angular.

Vue.js also has all the directive that you're used to in Angular, so `v-if`, `v-show`, `v-for`, `v-on`, etc. I haven't talked about **methods** (i.e. functions that you define in the `vm`), but I'm not really writing a Vue tutorial here – the official documentation is remarkably good and there's also [this videoseries](https://laracasts.com/series/learning-vue-step-by-step) (mostly free) by the always great Jeffrey Way – let me just briefly mention **components**: Everything I've shown here is really basic, there's so much more to discover in Vue.js! Hopefully I've raised to your attention what seems to me a valuable alternative to more known and opinionated frameworks that, in the context of HTML Panels, for some among us are either overkill and/or difficult to master and maintain.

{% include_relative cepBook.md %}
