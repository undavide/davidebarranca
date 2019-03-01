---
id: 2614
title: Introducing Fixel Detailizer 2 PS
date: 2014-07-25T23:09:51+01:00
author: Davide Barranca
excerpt: "I'm glad to announce the release on Adobe Add-ons of a powerful contrast booster for Photoshop: Fixel Detailizer 2 PS. Let's see what is that all about!"
layout: post
guid: http://www.davidebarranca.com/?p=2614
permalink: /2014/07/introducing-fixel-detailizer/
seo_post_level_layout:
  - ""
link_url_field:
  - ""
seo_post_meta_description:
  - 'Fixel Detailizer 2 PS - a Photoshop CC filter that boosts the contrast of 5 spatial ranges separated using a proprietary wavelets algorithm.'
image: /wp-content/uploads/2014/06/Detailizer-GUI.jpg
categories:
  - Extensions and Scripts
  - Fixel Detailizer
tags:
  - filter
  - Fixel Algorithms
  - frequency
---
<div class="pf-content">
  <div>
    <p>
      I&#8217;m glad to announce the release on Adobe Add-ons of a powerful contrast booster for Photoshop: Fixel Detailizer 2 PS. Let&#8217;s see what is that all about!<!--more-->
    </p>
    
    <h2>
      Fixel Algorithms
    </h2>
    
    <p>
      I&#8217;ve recently started a partnership with Fixel Algorithms, a company founded by engineers and programmers specialized in Digital Image Processing &#8211; amazingly smart people. Fixel Algorithms has been creating <a title="Fixel Algorithms for After Effects" href="http://aescripts.com/authors/f-l/fixel-algorithms/" target="_blank">Filters for Adobe After Effects</a> (video processing), and Detailizer is our first shared effort porting to Photoshop their excellent work.
    </p>
    
    <h2>
      Detailizer at a glance
    </h2>
    
    <p>
      Detailizer 2 PS decomposes your image into discrete Frequency Ranges and allows you to separately control the Contrast Boost of each of them. It&#8217;s like Frequency Separation on steroids!
    </p>
    
    <div class="twentytwenty" class="twentytwenty-container" style="display: none; max-width: 100%; width: 700px; height: 467px">
      <img src="http://localhost:8888/wp-content/uploads/2014/06/detailizer-before1.jpg" /><img src="http://localhost:8888/wp-content/uploads/2014/06/detailizer-after1.jpg" />
    </div>
    
    <p>
      &nbsp;
    </p>
    
    <h3>
      Frequency explained
    </h3>
    
    <p>
      Do you need a primer on spacial frequency? Basically, it&#8217;s about the <em>size</em> of the detail in your pictures. For instance, say that you&#8217;ve shot a portrait &#8211; take a sample in:
    </p>
    
    <ul>
      <li>
        the <strong>hair</strong>: there&#8217;s plenty of fine detail, pixel&#8217;s values (dark/bright) vary with a high frequency.
      </li>
      <li>
        the <strong>lips</strong>: the variation is smoother compared to the hair, even if the transition between dark/bright values happens with a decent (say: middle-sized) frequency too.
      </li>
      <li>
        the <strong>cheek</strong>: here the tonal variation is really smooth, and covers a larger area: in the sample <em>things</em> change on a lower rate (low frequency).
      </li>
    </ul>
    
    <p>
      <img class="aligncenter size-full wp-image-2617" src="http://localhost:8888/wp-content/uploads/2014/06/3Freq.jpg" alt="Frequency" width="700" height="556" srcset="http://localhost:8888/wp-content/uploads/2014/06/3Freq.jpg 700w, http://localhost:8888/wp-content/uploads/2014/06/3Freq-150x119.jpg 150w, http://localhost:8888/wp-content/uploads/2014/06/3Freq-300x238.jpg 300w" sizes="(max-width: 700px) 100vw, 700px" />
    </p>
    
    <p>
      It might not be the most accurate description of what a spacial frequency is, but you&#8217;ve probably got the idea. In the real world the distinction is fuzzier: every area is made with all frequencies combined (within &#8220;low-freq&#8221; cheeks there is &#8220;high-freq&#8221; skin texture), the same way a complex signal such a sound or an electromagnetic wave is a combination of simpler signals:
    </p>
    
    <p>
      <img class="aligncenter size-full wp-image-2618" src="http://localhost:8888/wp-content/uploads/2014/06/multifreq.jpg" alt="Multi Frequency" width="700" height="175" srcset="http://localhost:8888/wp-content/uploads/2014/06/multifreq.jpg 700w, http://localhost:8888/wp-content/uploads/2014/06/multifreq-150x37.jpg 150w, http://localhost:8888/wp-content/uploads/2014/06/multifreq-300x75.jpg 300w" sizes="(max-width: 700px) 100vw, 700px" />
    </p>
    
    <h2>
      Features
    </h2>
    
    <p>
      Fixel Detailizer 2 PS peculiar frequency decomposition (the filter&#8217;s core) is performed with a fast, proprietary <strong>Wavelets algorithm</strong> developed by Fixel.
    </p>
    
    <p>
      The interface lets you boost separately <strong>5 Frequency Ranges</strong>, in order to target precisely the detail level that you want.
    </p>
    
    <p>
      <img class="aligncenter size-full wp-image-2620" src="http://localhost:8888/wp-content/uploads/2014/06/Detailizer-GUI.jpg" alt="Detailizer GUI" width="700" height="467" srcset="http://localhost:8888/wp-content/uploads/2014/06/Detailizer-GUI.jpg 700w, http://localhost:8888/wp-content/uploads/2014/06/Detailizer-GUI-150x100.jpg 150w, http://localhost:8888/wp-content/uploads/2014/06/Detailizer-GUI-300x200.jpg 300w" sizes="(max-width: 700px) 100vw, 700px" />
    </p>
    
    <p>
      Fixel Detailizer 2 PS works on 8bit, 16bit and <strong>32bit HDR</strong> files too! All the calculations are internally performed at 32bits to ensure the maximum precision.
    </p>
    
    <h2>
      How to get it
    </h2>
    
    <p>
      Fixel Detailizer 2 PS is a 5 stars product! This Photoshop Filter available on <a title="Fixel Detailizer PS on Adobe Add-ons" href="https://creative.adobe.com/addons/products/2757" target="_blank">Adobe Add-ons</a> (the brand new app-store for Photoshop extensions) for USD 19.99 and supports Photoshop CC and CC 2014 on both Mac and Windows (64bit).
    </p>
  </div>
</div>

<!-- Share-Widget Button BEGIN --><a href="javascript:void(0);" myshare\_id="mys\_shareit" myshare\_url="http://localhost:8888/2014/07/introducing-fixel-detailizer/" myshare\_title="Introducing Fixel Detailizer 2 PS" rel="nofollow" onclick=" return false;" style="text-decoration:none; color:#000000; font-size:11px; line-height:20px;"> 

<img src="http://localhost:8888/wp-content/plugins/share-widget/img/share-button-white-small.png" height="20" alt="Share" style="border:0" /> </a> <!-- Share-Widget Button END -->