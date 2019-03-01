---
id: 2654
title: PS-Panels-Boilerplate on GitHub!
date: 2014-06-22T00:09:28+01:00
author: Davide Barranca
excerpt: "I've created a new project on my GitHub account where I will collect both Boilerplate code for HTML Panels and other Demo extensions - it's called PS-Panels-Boilerplate."
layout: post
guid: http://www.davidebarranca.com/?p=2654
permalink: /2014/06/ps-panels-boilerplate-on-github/
seo_post_level_layout:
  - ""
link_url_field:
  - ""
seo_post_meta_description:
  - "I've started a new project on GitHub where I will gather Boilerplate / Template code for Adobe Photoshop CC and CC 2014 HTML Panels."
image: /wp-content/uploads/2014/06/Octocat.jpg
categories:
  - Coding
  - HTML Panels
tags:
  - Boilerplate
  - GitHub
---
<div class="pf-content">
  <p>
    I&#8217;ve created a new project on my GitHub account where I will collect both Boilerplate code for HTML Panels and other Demo extensions &#8211; it&#8217;s called <a title="PS-Panels-Boilerplate" href="https://github.com/undavide/PS-Panels-Boilerplate" target="_blank">PS-Panels-Boilerplate</a>.<!--more-->
  </p>
  
  <p>
    I&#8217;ve started with a revised version of my <strong>Topcoat CSS</strong> integration <a title="HTML Panels Tips: #6 integrating Topcoat CSS" href="http://localhost:8888/2014/02/html-panels-tips-6-integrating-topcoat-css/" target="_blank">post</a>. The code is now better because it doesn&#8217;t rely on the usual two version of topcoat:
  </p>
  
  <ul>
    <li>
      topcoat-desktop-light.css
    </li>
    <li>
      topcoat-desktop-dark.css
    </li>
  </ul>
  
  <p>
    Instead, I&#8217;ve tweaked the original <code>.styl</code> and the <code>Grunt.js</code> files &#8211; and following Garth Braithwaite&#8217;s <a style="color: #4183c4;" href="http://topcoat.io/posts/color-me-topcoat/">tutorial</a> I&#8217;ve eventually been able to output 4 CSS files that better match the Photoshop GUI, keeping the peculiar Topcoat personality:
  </p>
  
  <ul>
    <li>
      topcoat-desktop-lightlight.css
    </li>
    <li>
      topcoat-desktop-light.css
    </li>
    <li>
      topcoat-desktop-dark.css
    </li>
    <li>
      topcoat-desktop-darkdark.css
    </li>
  </ul>
  
  <p>
    The <code>themeManager.js</code> code is refactored too, also (thanks to David Deraedt&#8217;s idea) injecting app&#8217;s Font information in the CSS.
  </p>
  
  <h2>
    UPDATE &#8211; Photoshop Tools
  </h2>
  
  <p>
    I&#8217;ve added this HTML Panel, which has been fun to build &#8211; go grab it on <a title="Photoshop Tools HTML Panel" href="https://github.com/undavide/PS-Panels-Boilerplate/blob/master/src/com.undavide.tools2/README.md" target="_blank">my GitHub page</a>.
  </p>
  
  <p>
    <a href="https://github.com/undavide/PS-Panels-Boilerplate/blob/master/src/com.undavide.tools2/README.md"><img class="aligncenter size-full wp-image-2664" src="http://localhost:8888/wp-content/uploads/2014/06/screenshot.png" alt="Photoshop Tools Panel" width="812" height="410" srcset="http://localhost:8888/wp-content/uploads/2014/06/screenshot.png 812w, http://localhost:8888/wp-content/uploads/2014/06/screenshot-150x75.png 150w, http://localhost:8888/wp-content/uploads/2014/06/screenshot-300x151.png 300w" sizes="(max-width: 812px) 100vw, 812px" /></a>
  </p>
  
  <p>
    In the future &#8211; job, family and time permitting &#8211; I hope to be able to store in that repository the full code related to my entire <a title="HTML Panels Tips series" href="http://localhost:8888/category/code/html-panels/" target="_blank">HTML Panels tip series</a> (and more! Since I&#8217;ve started approaching Angular, I plan to cover that one too).
  </p>
</div>

<!-- Share-Widget Button BEGIN --><a href="javascript:void(0);" myshare\_id="mys\_shareit" myshare\_url="http://localhost:8888/2014/06/ps-panels-boilerplate-on-github/" myshare\_title="PS-Panels-Boilerplate on GitHub!" rel="nofollow" onclick=" return false;" style="text-decoration:none; color:#000000; font-size:11px; line-height:20px;"> 

<img src="http://localhost:8888/wp-content/plugins/share-widget/img/share-button-white-small.png" height="20" alt="Share" style="border:0" /> </a> <!-- Share-Widget Button END -->