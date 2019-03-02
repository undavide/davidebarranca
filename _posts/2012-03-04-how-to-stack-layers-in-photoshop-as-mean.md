---
id: 753
title: How to stack layers in Photoshop as a Mean
date: 2012-03-04T00:00:51+01:00
author: Davide Barranca
excerpt: 'How to stack layers in Photoshop as a Mean: correct opacities to assign, and the smart objects workaround'
layout: post
guid: http://www.davidebarranca.com/?p=753
permalink: /2012/03/how-to-stack-layers-in-photoshop-as-mean/
standard_seo_post_level_layout:
  - ""
standard_link_url_field:
  - ""
standard_seo_post_meta_description:
  - 'How to stack layers in Photoshop as a Mean: correct opacities to assign, and the smart objects workaround.'
image: /wp-content/uploads/2012/03/Layered-Stonehenge.jpg
category:
  - Photoshop
tags:
  - layers
  - Mean
  - Photoshop @en
  - smart object
  - stack
---
<div class="pf-content">
  <div id="attachment_754" style="width: 580px" class="wp-caption aligncenter">
    <a href="http://blog.rbg.bigano.com/2011/02/14/three-heads-are-better-than-one/" target="_blank"><img aria-describedby="caption-attachment-754" class=" wp-image-754  " style="border-style: initial; border-color: initial; border-width: 0px;" alt="Three_of_a_perfect_pair" src="/wp-content/uploads/2012/03/Three_of_a_perfect_pair.jpg" width="570" height="376" srcset="/wp-content/uploads/2012/03/Three_of_a_perfect_pair-150x99.jpg 150w, /wp-content/uploads/2012/03/Three_of_a_perfect_pair-300x199.jpg 300w" sizes="(max-width: 570px) 100vw, 570px" /></a>

    <p id="caption-attachment-754" class="wp-caption-text">
      Three versions of a single image stitched together. Left to right: MO, DB, GA.
    </p>
  </div>

  <p>
    In Color Correction it is a wise strategy to produce multiple versions of a single image (that is: to correct the same picture few times collecting different variations), then blend them together in a number of ways &#8211; to get the better of each one. This doesn&#8217;t necessarily involve that it must be the same person the one in charge, as my dear friend Marco Olivotto stated in a beautiful manifesto on collaborative color correction called <a title="Three Heads are Better than One" href="http://blog.rbg.bigano.com/2011/02/14/three-heads-are-better-than-one/" target="_blank">Three Heads are Better than One</a>.
  </p>

  <p>
    Although masks and blending modes may be required, the easiest blends of all (33-33-33 or maybe 25-25-25-25) can be difficult to obtain, unless you know the trick!
  </p>

  <p>
    <!--more-->
  </p>

  <p>
    The 50-50 blend of two layers is a piece of cake: you set the bottom to 100% opacity and the top to 50%. It&#8217;s a bit different if you&#8217;d like to blend three layers 33-33-33. You&#8217;d be tempted to set the background to 100% and the two above to 33% (or 50% perhaps?) &#8211; but you&#8217;d be wrong!
  </p>

  <p>
    Let&#8217;s say you&#8217;ve three versions to blend (i.e. three layers) and you want to give the same weight to everyone.
  </p>

  <h3>
    Smart Objects and Stack Mode
  </h3>

  <p>
    One method is to select them all, create a Smart Object containing the three layers, then (if you have Photoshop Extended), select in the menu <em>Layer > Smart Objects > Stack Mode > Mean</em>. This is doubtless a neat way, yet you may find this workflow a bit tedious (S.O. are a pain of slowness and add unnecessary complication for this task). And you need to have an Extended version of PS.
  </p>

  <h3>
    Opacity ramp
  </h3>

  <p>
    The fastest way (because we&#8217;re always in hurry and/or we may just want to give the blend a quick try) is to set each layer opacity.
  </p>

  <p>
    <img class="alignleft size-full wp-image-756" style="border-style: initial; border-color: initial; border-width: 0px;" alt="Layers palette" src="/wp-content/uploads/2012/03/layers.png" width="306" height="306" srcset="/wp-content/uploads/2012/03/layers.png 306w, /wp-content/uploads/2012/03/layers-150x150.png 150w, /wp-content/uploads/2012/03/layers-300x300.png 300w" sizes="(max-width: 306px) 100vw, 306px" />
  </p>

  <p>
    It&#8217;s not that obvious what percentage to assign to them. For three layers the numbers (bottom up) are 100%, 50%, 33% like in the screenshot at left &#8211; and the very easy general formula to use is
  </p>

  <p>
    <strong>OPACITY = 100 / LAYER POSITION</strong>
  </p>

  <p>
    That is, the background layer (at the very bottom) is in position one, so its opacity is 100/1 = 100. The second layer, above it, is in position two, so the opacity is 100/2 = 50. And the third, in position three as the topmost layer has an opacity of 100/3 = 33.
  </p>

  <p>
    To save you some time, the ramp for 10 layers goes like that: 100, 50, 33, 25, 20, 17, 14, (12), 11, 10.
  </p>

  <p>
    Actually 12 should be 13 (since 100/8 = 12.5), but I&#8217;ve chosen 12 to keep the slope smoother (up to you). Anyway: if you need mathematic precision and the layers&#8217; number you&#8217;ve got to deal with is about 8 or more, go with the Smart Object way; otherwise opacity it&#8217;s a fair compromise between speed and precision!
  </p>

  <p>
    Just a tip that may come handy from time to time. And if you want to know how Marco blended the three versions to reach the final picture, go read the <a title="Three Heads are Better than One" href="http://blog.rbg.bigano.com/2011/02/14/three-heads-are-better-than-one/" target="_blank">original post</a> and find out why 1+1+1 > 3.
  </p>

  <p>
    <a href="http://blog.rbg.bigano.com/2011/02/14/three-heads-are-better-than-one/" target="_blank"><img class="alignleft  wp-image-762" style="border-style: initial; border-color: initial; border-width: 0px;" alt="Layered Stonehenge" src="/wp-content/uploads/2012/03/Layered-Stonehenge.jpg" width="570" height="376" /></a>
  </p>

  <p>
    &nbsp;
  </p>
</div>
