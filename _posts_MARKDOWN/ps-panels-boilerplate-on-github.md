---
title: " PS-Panels-Boilerplate on GitHub!\t\t"
tags:
  - Boilerplate
  - GitHub
url: 2654.html
id: 2654
category:
  - Coding
  - HTML Panels
date: 2014-06-22 00:09:28
---

I've created a new project on my GitHub account where I will collect both Boilerplate code for HTML Panels and other Demo extensions - it's called [PS-Panels-Boilerplate](https://github.com/undavide/PS-Panels-Boilerplate "PS-Panels-Boilerplate"). I've started with a revised version of my **Topcoat CSS** integration [post](http://localhost:8888/2014/02/html-panels-tips-6-integrating-topcoat-css/ "HTML Panels Tips: #6 integrating Topcoat CSS"). The code is now better because it doesn't rely on the usual two version of topcoat:

*   topcoat-desktop-light.css
*   topcoat-desktop-dark.css

Instead, I've tweaked the original `.styl` and the `Grunt.js` files - and following Garth Braithwaite's [tutorial](http://topcoat.io/posts/color-me-topcoat/) I've eventually been able to output 4 CSS files that better match the Photoshop GUI, keeping the peculiar Topcoat personality:

*   topcoat-desktop-lightlight.css
*   topcoat-desktop-light.css
*   topcoat-desktop-dark.css
*   topcoat-desktop-darkdark.css

The `themeManager.js` code is refactored too, also (thanks to David Deraedt's idea) injecting app's Font information in the CSS.

UPDATE - Photoshop Tools
------------------------

I've added this HTML Panel, which has been fun to build - go grab it on [my GitHub page](https://github.com/undavide/PS-Panels-Boilerplate/blob/master/src/com.undavide.tools2/README.md "Photoshop Tools HTML Panel"). [![Photoshop Tools Panel](http://localhost:8888/wp-content/uploads/2014/06/screenshot.png)](https://github.com/undavide/PS-Panels-Boilerplate/blob/master/src/com.undavide.tools2/README.md) In the future - job, family and time permitting - I hope to be able to store in that repository the full code related to my entire [HTML Panels tip series](http://localhost:8888/category/code/html-panels/ "HTML Panels Tips series") (and more! Since I've started approaching Angular, I plan to cover that one too).