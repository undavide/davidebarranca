---
title: "Adobe UXP: Things you need to know! #12 React JS"
date: 2021-12-17
author: "Davide Barranca"
excerpt: "It's time to approach the most popular JavaScript library: React JS, and my Adobe UXP plugins development with React JS brand new course!"
layout: post
description: "It's time to approach the most popular JavaScript library: React JS, and my Adobe UXP plugins development with React JS brand new course!"
image: "/wp-content/uploads/2020/10/TYNTN.png"
media_card: /wp-content/uploads/2021/11/TYNTN-twitter12.png
category:
  - Development
tags:
  - UXP
---

In this instalment I'd like to tell you about ReactJS, and I'll take the chance to announce a brand new course of mine: **Adobe UXP plugins development with ReactJS - Build products and Create a Business in the Creative Cloud Marketplace and Beyond**.

<iframe width="560" height="315" src="https://www.youtube.com/embed/ll84YImfMP4" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

So far in this series I've only used Vanilla JavaScript to keep it simple, but in the real world a large part of plugin developers don't like to reinvent the wheel themselves and end up using one of the available frameworks. And guess what, ReactJS is the most popular JavaScript library according to the StackOverflow 2021 survey. Even more than jQuery, check that out!

![](/wp-content/uploads/2021/11/frameworks.png)

By the way, React JS is a Library and not a Framework (such Vue.js or Angular), do you know the difference? A **Library** is something that you _"plug in your code"_, whereas a **Framework** is something _"you plug your code into"_... subtle but meaningful difference! In other words, _your code uses a Library_, whereas _your code is used by a Framework_. This is what is generally referred to as _"Inversion of Control"_.

According to this definition, it turns out that React JS is in fact a library, i.e. some code that enhances "regular" JavaScript adding useful features, but it doesn't require you to buy into some opinionated way of structuring your source code, as Frameworks generally do.

## React feats

So, what kind of feats made React JS so popular over the years? First of all, **Components**, a handy way to bundle into a custom element bits of user interface and logic.

{% highlight html %}
<div>
  <Heading></Heading>
  <Slider></Slider>
  <Buttons></Buttons>
</div>
{% endhighlight %}

Creating a component is as easy as writing a function: that in React can return markup, i.e. HTML elements.

{% highlight jsx %}
const Buttons = () => {
  return (
    <div className="buttons">
      <sp-button variant="secondary">Cancel</sp-button>
      <sp-button>Run</sp-button>
    </div>
  );
}
{% endhighlight %}

This is referred to as JSX: JavaScript Syntax Extension, or JavaScript XML. Those components can be passed **Properties** by the parent: they can make use of them, e.g. to display values via curly braces `{}` interpolation or to do whatever they have to do.

{% highlight html %}
<!-- In the Parent -->
<ToggleButton message="Click me" bigger="true"/>
{% endhighlight %}

Here we're passing a `message` and a `bigger` properties, that are then destructured and used in the component to set the Spectrum Button text and assign classes.

{% highlight jsx %}
// In the Component
const ToggleButton = ({message, bigger}) => {
  return (
    <sp-button class={bigger === "true" ? "larger highlight" : "normal"}>
      {message}
    </sp-button>
  );
}
{% endhighlight %}

Thanks to **Hooks**, special React functions that give Components superpowers, you can e.g. create a **State**: a _reactive_ property (alongside with its setter) that is able to trigger a component re-render when the property changes.

{% highlight jsx %}
import { useState } from "react";

const ToggleButton = () => {
  const [ isOn, setIsOn ] = useState(false); // <- State
  return (
    <sp-action-button onClick={ () => setIsOn(!isOn) }>
      { isOn ? "Toggled ON" : "Toggled OFF" }
    </sp-action-button>
  );
}
{% endhighlight %}

In the example above, `isOn` is a boolean stored in the State; depending on its value, a different string is displayed in the Spectrum Action Button. The boolean is flipped in the button's `onClick` handler via its setter `setIsOn()`.

**Reactivity** is key here: you're building _declarative_ interfaces based on data, that are going to reflect changes in data as they happen. On the other side, with _imperative_ code (e.g. jQuery) you have got to explicitly impart instructions to the UI on what to do, and when.

Obviously there's much more to React than Components and Hooks, but thanks to them and to JSX (the possibility to use JavaScript to compute HTML code) you're able to build remarkably powerful User Interfaces.

For all your needs there is a large ecosystem of plugins: take State Management as an example—i.e. a unique place to store reactive data that is accessible to all components no matter their parent/child relation. React provides you with solid foundations only, but you can easily plug-in the library you like the most among the available ones: Redux, Recoil, MobX, Zustand, etc.

## Why React

And not Vue, Angular, Svelte...? Because: Adobe! You might not like this, but let me explain.

UXP, as we've seen over and over again throughout this series, is heavily inspired by standards, but **it shouldn't be considered a Browser environment**. Hence, not everything that works in a Browser is to be expected to automatically work in a UXP plugin. As an example, until very (very!) recently, there wasn't a `document.createEvent()` method, which is what Vue.js version 3 relies upon. Why was it missing? Because the UXP team has to willingly implement features, and that one didn't happen to be high priority to them. As a result, for almost a year after the UXP announcement you couldn't even load Vue.js v3.[^vuejs]

It also happens that **React JS is exactly what Adobe uses for their own first-party features**, like for instance the Welcome screen, the New File dialog, the Neural Filters interfaces: all UXP plugins. And you can bet that if React-next-version is going to need some Browser API that didn't make into UXP yet, that very feature is going to be implemented at once. Because they won't risk Neural Filters to crash because of it, right? On the other hand, any other feature might take months to be implemented—as it's happened with `document.createEvent()`

> In other words, not only React is a solid choice thanks to the popularity of the library in the JavaScript ecosystem and the amount of available resources; given its adoption by Adobe's engineering teams it has to be considered the _safest choice_, support-wise.

With any other framework, although they might work, you're... more on your own. The [insert your framework of choice] might or might not work now, and might stop working when its newer version is released. ¯\\_(ツ)\_/¯

## The UXP and React JS course

I was chatting with a developer friend once, and they said: "I'll start with Vanilla JS plugins, and when I'm familiar with the environment I'll approach React JS. You know, I don't want too much on my plate".
Although it may sound considerate at first, I don't buy it, IMHO there's a **way better approach**.

On the one side, learning a tool[^react] such as React for React's sake, in some abstract/generic environment _à la_ "let's build a Todo app, or a Reddit clone" is always going to be an inferior way, compared to **learning it in the actual context in which you're gonna use it**: a UXP plugin. Not only you're gonna save a huge lot of time, but it sucks way less.

[^react]: To me, React is just a tool to get the job done: I'm not a zealot, I'm not interested in JavaScript frameworks religion wars.

Provided that somebody (i.e. yours truly) has figured out **a learning path that combines UXP and React JS** in such a way that you're presented with a UXP problem, and given a gradually more complex React tool to solve it. So, hopefully, next time you'll run into the same kind of problem in your own UXP plugins, it'll come natural for you to reach out for that same React tool to hammer it. Much better than trying to fit some abstract example into the case at hand.

Moreover, there's a small(ish) subset of React features that allow you to accomplish a great deal of good in the UXP environment. Everybody love Venn Diagrams, isn't that right?

![](/wp-content/uploads/2021/11/venn.png)

The overlapping area is evident _after_ you've drawn the two circles, i.e. after you've learned the two topics separately. But _while_ you're drawing (i.e. learning) them on your own? You've no idea whether the React JS topic you're approaching is going to be of fundamental importance, marginal, or just a waste of time. Wouldn't it be better to start from this relatively approachable core of super-important topics instead, dealt with in the UXP plugins context to optimize your energies and time?

That's precisely the Adobe UXP plugins development with React JS course's agenda.

![](/wp-content/uploads/2021/11/UXP_Hero.jpg)

Subtitled: "Build products and Create a Business in the Creative Cloud Marketplace and Beyond". There's a fat section on the business of UXP plugins, from mundane but fundamental topics such as VAT, ecommerce providers etc. that I wish somebody told me when I started many years ago, up to a comparison of marketing strategies. With examples from a barely average marketeer and a very successful one (and by very successful I mean in the six-figures range) such as Micheal Bruny-Groth, the author of the Logo Package for Adobe Illustrator.

The course consists of a **PDF ebook, ~290 pages** lusciously typeset in LaTeX (a pain in the ass, but still luscious).

![](/wp-content/uploads/2021/11/book.jpg)

Then there are **16 demo plugins** with fully commented code, from the ubiquitous "Action buttons" plugin, up more advanced topics such as React custom hooks, server-side communication, etc.

I've also recorded **5 and a half hours of videotutorials**, with theory, live coding, bug fixing and whatnot.

![](/wp-content/uploads/2021/11/SiB_videos.jpg)

I definitely plan to add more content as UXP evolves. The course is offered in three different _packages_:

- [Basic](https://gumroad.com/l/FrlXB?variant=Basic): Book + Code samples
- [Complete](https://gumroad.com/l/FrlXB?variant=Complete): Book + Code samples + Videos (streaming only)
- [Enterprise](https://gumroad.com/l/YNFVJ): Book + Code samples + Videos (downloadable, licensed for teams up to 10 seats strong)

For more info and FAQ check out the course home at [PS-Scripting.com](https://www.ps-scripting.com/uxp-react.html).

[^vuejs]: Vue.js version 2 worked fine out of the box, instead.

---

{% include SUPPORT.md %}

Stay safe, get the Nth vaccine shot if/when you can – bye!

{% include UXP_TYNTN.md %}

---
