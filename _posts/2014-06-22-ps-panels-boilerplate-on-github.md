---
title: PS-Panels-Boilerplate on GitHub!
date: 2014-06-22T00:09:28+01:00
author: Davide Barranca
excerpt: "A GitHub repository where I will gather Boilerplate and Template code for Adobe Photoshop CEP Panels."
layout: post
permalink: /2014/06/ps-panels-boilerplate-on-github/
description:
  - "A GitHub repository where I will gather Boilerplate and Template code for Adobe Photoshop CEP Panels."
image: /wp-content/uploads/2014/06/Octocat.jpg
categories:
  - CEP
tags:
  - GitHub
---

I've created a new project on my GitHub account where I will collect both Boilerplate code for HTML Panels and other Demo extensions - it's called [PS-Panels-Boilerplate](https://github.com/undavide/PS-Panels-Boilerplate "PS-Panels-Boilerplate"). I've started with a revised version of my **Topcoat CSS** integration [post](/2014/02/html-panels-tips-6-integrating-topcoat-css/ "HTML Panels Tips: #6 integrating Topcoat CSS"). The code is now better because it doesn't rely on the usual two version of topcoat:

*   topcoat-desktop-light.css
*   topcoat-desktop-dark.css

Instead, I've tweaked the original `.styl` and the `Grunt.js` files - and following Garth Braithwaite's [tutorial](http://topcoat.io/posts/color-me-topcoat/) I've eventually been able to output 4 CSS files that better match the Photoshop GUI, keeping the peculiar Topcoat personality:

*   topcoat-desktop-lightlight.css
*   topcoat-desktop-light.css
*   topcoat-desktop-dark.css
*   topcoat-desktop-darkdark.css

The `themeManager.js` code is refactored too, also (thanks to David Deraedt's idea) injecting app's Font information in the CSS.

## UPDATE - Photoshop Tools

I've added this HTML Panel, which has been fun to build - go grab it on [my GitHub page](https://github.com/undavide/PS-Panels-Boilerplate/blob/master/src/com.undavide.tools2/README.md "Photoshop Tools HTML Panel").

[![Photoshop Tools Panel](/wp-content/uploads/2014/06/screenshot.png)](https://github.com/undavide/PS-Panels-Boilerplate/blob/master/src/com.undavide.tools2/README.md)

In the future - job, family and time permitting - I hope to be able to store in that repository the full code related to my entire [HTML Panels tip series](/category/code/html-panels/ "HTML Panels Tips series") (and more! Since I've started approaching Angular, I plan to cover that one too).

{% include_relative cepBook.md %}
