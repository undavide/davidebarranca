---
id: 2195
title: Photoshop CC Color Range Enhancements
date: 2013-09-10T11:27:10+01:00
author: Davide Barranca
excerpt: "Latest Photoshop CC update (version 14.1) has introduced a good deal of new features. One of them is the refined Color Range Selection for Highlights, Midtones and Shadows which now includes Threshold: let's see what is that all about."
layout: post
guid: http://www.davidebarranca.com/?p=2195
permalink: /2013/09/photoshop-cc-color-range-enhancements/
standard_seo_post_level_layout:
  - ""
standard_link_url_field:
  - ""
standard_seo_post_meta_description:
  - 'Photoshop CC (version 14.1) introduces Color Range Selection enhancements: Highlights, Midtones and Shadows Range. A Threshold in disguise.'
image: /wp-content/uploads/2013/09/Photoshop_CC_Color_Range.png
categories:
  - Photoshop
tags:
  - "14.1"
  - Color Range
  - Fuzziness
  - Photoshop CC
  - Range
---
<div class="pf-content">
  <p>
    <span itemprop="description">Latest Photoshop CC update (version 14.1) has introduced a good deal of new features. One of them is the refined Color Range Selection for Highlights, Midtones and Shadows, which now includes a proper Threshold called Range</span>: let&#8217;s see what is that all about.
  </p>

  <h2>
    Threshold
  </h2>

  <p>
    As an Adjustment that is in Photoshop since early versions, Threshold hasn&#8217;t gained much popularity so far, unless you&#8217;re into Color Correction or forensic image processing. Have a look at what it does to a test image (click to enlarge).
  </p>

  <p style="text-align: center;">
    <img class="aligncenter size-full wp-image-2196" alt="Threshold" src="/wp-content/uploads/2013/09/Threshold.jpg" width="1176" height="680" srcset="/wp-content/uploads/2013/09/Threshold.jpg 1176w, /wp-content/uploads/2013/09/Threshold-150x86.jpg 150w, /wp-content/uploads/2013/09/Threshold-300x173.jpg 300w, /wp-content/uploads/2013/09/Threshold-1024x592.jpg 1024w" sizes="(max-width: 1176px) 100vw, 1176px" />
  </p>

  <p>
    Basically it blows to White every pixel in the image which value that exceeds (but not include) the Threshold, and clips to Black everything else. You see it clearly in the third example (value: 128) where the image is split in two halves: from mid-grays to shadows they get to black, from mid-grays to highlights they get to white, so to speak. Another way to put it: Threshold makes your images 1bit (i.e. just Black or White, no grays allowed).
  </p>

  <p>
    Engineers have now built in the <span itemprop="about" itempscope itemtype="http://schema.org/SoftwareApplication"><span itemprop="name">Photoshop CC</span> <span itemprop="about">Color Range Selection</span> tool</span> a Threshold in disguise &#8211; whose name has become <strong itemprop="mentions">Range</strong>.
  </p>

  <h2>
    Range and Fuzziness
  </h2>

  <p>
    <img class="alignleft size-full wp-image-2197" alt="Presets" src="/wp-content/uploads/2013/09/Presets.png" width="223" height="312" srcset="/wp-content/uploads/2013/09/Presets.png 223w, /wp-content/uploads/2013/09/Presets-150x209.png 150w, /wp-content/uploads/2013/09/Presets-214x300.png 214w" sizes="(max-width: 223px) 100vw, 223px" />The Color Range Enhancement can be found if you pick from the drop down menu either the <strong itemprop="mentions">Highlights</strong>, <strong itemprop="mentions">Midtones</strong> or <strong itemprop="mentions">Shadows</strong> (see the screenshot), otherwise you won&#8217;t see any new feature (if you&#8217;re wondering, Skin Tones has been introduced in PS CS6).
  </p>

  <p>
    Mind you: we&#8217;ll be dealing with B/W images because that&#8217;s how Photoshop &#8220;thinks&#8221; about selections: what is going to get <strong>white means selected</strong>, what is going to get <strong>black means not selected</strong>. All the grays in the middle are, depending on their value, <strong>halfway between selected and not selected</strong> &#8211; that is: the level of gray is tied with the opacity of the selection. So an Alpha Channel is just a way to visualize complex selections.
  </p>

  <p>
    The two sliders we&#8217;ll be playing with are the <strong>Fuzziness</strong> and <strong>Range</strong>; the latter as I said is just a Threshold (plenty of screenshot below don&#8217;t worry) while the former controls how much the actual selection adhere to the numbers you set in the Range. So to speak, zero Fuzziness means harsh, Threshold like, just-black-or-white selections; higher Fuzziness means smooth selections, that include more grays. Let&#8217;s see.
  </p>

  <h3>
    Highlights
  </h3>

  <p>
    I&#8217;ve run Select &#8211; Color Range, then chosen Highlights. I&#8217;ve set Fuzziness to zero: restricting to one variable at the time helps understanding what happens more clearly &#8211; playing with the Range only you get:
  </p>

  <p style="text-align: center;">
    <a href="/wp-content/uploads/2013/09/Highlights.png" target="_blank"><img class="size-full wp-image-2205 aligncenter" alt="Highlights" src="/wp-content/uploads/2013/09/Highlights.png" width="1263" height="419" itemprop="image" srcset="/wp-content/uploads/2013/09/Highlights.png 1263w, /wp-content/uploads/2013/09/Highlights-150x49.png 150w, /wp-content/uploads/2013/09/Highlights-300x99.png 300w, /wp-content/uploads/2013/09/Highlights-1024x339.png 1024w" sizes="(max-width: 1263px) 100vw, 1263px" /></a>
  </p>

  <p>
    As you see, it&#8217;s very much like Threshold! With the apparent difference that Range at 255 still selects the pure whites (the pixels which value is 255), while actual Threshold doesn&#8217;t include it.
  </p>

  <p>
    What happens if you raise the Fuzziness to, say, 50%?
  </p>

  <p style="text-align: center;">
    <a href="/wp-content/uploads/2013/09/Highlights_Fuzzy.png" target="_blank"><img class="aligncenter size-full wp-image-2204" alt="Highlights_Fuzzy" src="/wp-content/uploads/2013/09/Highlights_Fuzzy.png" width="1263" height="419" itemprop="image" srcset="/wp-content/uploads/2013/09/Highlights_Fuzzy.png 1263w, /wp-content/uploads/2013/09/Highlights_Fuzzy-150x49.png 150w, /wp-content/uploads/2013/09/Highlights_Fuzzy-300x99.png 300w, /wp-content/uploads/2013/09/Highlights_Fuzzy-1024x339.png 1024w" sizes="(max-width: 1263px) 100vw, 1263px" /></a>
  </p>

  <p>
    As you see the selection &#8220;grows&#8221; through the blacks and gains more gray levels there &#8211; that is to say, you have a smoother selection that includes partially selected pixels.
  </p>

  <h3>
    Shadows
  </h3>

  <p>
    Similarly, if you pick the Shadows from the drop-down menu and keep the Fuzziness at zero:
  </p>

  <p style="text-align: center;">
    <a href="/wp-content/uploads/2013/09/Shadows.png" target="_blank"><img class="aligncenter size-full wp-image-2203" alt="Shadows" src="/wp-content/uploads/2013/09/Shadows.png" width="1263" height="419" itemprop="image" srcset="/wp-content/uploads/2013/09/Shadows.png 1263w, /wp-content/uploads/2013/09/Shadows-150x49.png 150w, /wp-content/uploads/2013/09/Shadows-300x99.png 300w, /wp-content/uploads/2013/09/Shadows-1024x339.png 1024w" sizes="(max-width: 1263px) 100vw, 1263px" /></a>
  </p>

  <p>
    Again you see the Range as a Threshold in disguise &#8211; with the difference that (compared to the actual Threshold) the resulting image is inverted, and you&#8217;re allowed to input 0 as a value (while Threshold sticks with 1). Basically selecting Shadows is just the same as inverting the image first, then selecting the Highlights &#8211; hopefully this makes sense to you.
  </p>

  <p>
    Fuzziness does what you would expect:
  </p>

  <p style="text-align: center;">
    <a href="/wp-content/uploads/2013/09/Shadows_Fuzzy.png" target="_blank"><img class="aligncenter size-full wp-image-2202" alt="Shadows_Fuzzy" src="/wp-content/uploads/2013/09/Shadows_Fuzzy.png" width="1263" height="419" itemprop="image" srcset="/wp-content/uploads/2013/09/Shadows_Fuzzy.png 1263w, /wp-content/uploads/2013/09/Shadows_Fuzzy-150x49.png 150w, /wp-content/uploads/2013/09/Shadows_Fuzzy-300x99.png 300w, /wp-content/uploads/2013/09/Shadows_Fuzzy-1024x339.png 1024w" sizes="(max-width: 1263px) 100vw, 1263px" /></a>
  </p>

  <h3>
    Midtones
  </h3>

  <p>
    Things get more interesting when it comes to the Midtones. There, you can simultaneously play with two Range sliders, and combine (so to speak) two Threshold at the same time, that restrict the <em>concept</em> of &#8220;midtone&#8221; that you need:
  </p>

  <p style="text-align: center;">
    <a href="/wp-content/uploads/2013/09/Midtones.png" target="_blank"><img class="aligncenter size-full wp-image-2201" alt="Midtones" src="/wp-content/uploads/2013/09/Midtones.png" width="1263" height="419" itemprop="image" srcset="/wp-content/uploads/2013/09/Midtones.png 1263w, /wp-content/uploads/2013/09/Midtones-150x49.png 150w, /wp-content/uploads/2013/09/Midtones-300x99.png 300w, /wp-content/uploads/2013/09/Midtones-1024x339.png 1024w" sizes="(max-width: 1263px) 100vw, 1263px" /></a>
  </p>

  <p>
    You can move the two sliders, shifting them to the left (the shadows) or to the right (the highlights), or make them more apart in order to select a bigger part of the grays. Fuzziness is handy here too:
  </p>

  <p style="text-align: center;">
    <a href="/wp-content/uploads/2013/09/Midtones_Fuzzy.png" target="_blank"><img class="aligncenter size-full wp-image-2200" alt="Midtones_Fuzzy" src="/wp-content/uploads/2013/09/Midtones_Fuzzy.png" width="1263" height="419" itemprop="image" srcset="/wp-content/uploads/2013/09/Midtones_Fuzzy.png 1263w, /wp-content/uploads/2013/09/Midtones_Fuzzy-150x49.png 150w, /wp-content/uploads/2013/09/Midtones_Fuzzy-300x99.png 300w, /wp-content/uploads/2013/09/Midtones_Fuzzy-1024x339.png 1024w" sizes="(max-width: 1263px) 100vw, 1263px" /></a>
  </p>

  <p>
    As it happened before, you can refine the selection combining Fuzziness and Range, to get exactly where you want to. The higher the Fuzziness, the more the selection dilates and includes gray levels.
  </p>

  <h2>
    Curves
  </h2>

  <p>
    Mind you, the idea of Midtone selection isn&#8217;t new &#8211; Photoshop engineers have been doing a nice enhancement to the Color Range dialog, which surely helps those unwilling to do it the old way. That is to say using Curves:
  </p>

  <p style="text-align: center;">
    <a href="/wp-content/uploads/2013/09/Curves.png" target="_blank"><img class="aligncenter size-full wp-image-2199" alt="Curves" src="/wp-content/uploads/2013/09/Curves.png" width="850" height="364" srcset="/wp-content/uploads/2013/09/Curves.png 850w, /wp-content/uploads/2013/09/Curves-150x64.png 150w, /wp-content/uploads/2013/09/Curves-300x128.png 300w" sizes="(max-width: 850px) 100vw, 850px" /></a>
  </p>

  <p>
    <a href="https://plus.google.com/111682277377408685163?rel=author">Davide Barranca</a><br />

    <meta itemprop="datePublished" content="2013-09-10" />

    <br />

    <meta itemprop="timeRequired" content="P15M" />

    <br />

    <meta itemprop="learningResourceType" content="tutorial" />

    <br />

    <meta itemprop="thumbnailUrl" content="/wp-content/uploads/2013/09/Photoshop_CC_Color_Range.png" />
  </p>
</div>
