---
id: 1076
title: 'Decomposing Sharpening #3 Workaround'
date: 2012-09-08T15:05:21+01:00
author: Davide Barranca
excerpt: How to replicate Unsharp Mask with Smart Filters, allowing the separate control of Dark and Light halos? Read along one viable workaround.
layout: post
guid: http://www.davidebarranca.com/?p=1076
permalink: /2012/09/decomposing-sharpening-part3-workaround/
standard_seo_post_level_layout:
  - ""
standard_seo_post_meta_description:
  - 'An effective workaround that mixes Smart Filters & Blending Modes in order to replicate the Unsharp Mask Filter splitting Dark / Light halos'
seo_post_level_layout:
  - ""
link_url_field:
  - ""
seo_post_meta_description:
  - ""
image: /wp-content/uploads/2012/09/FinalResult.jpg
categories:
  - Decomposing Sharpening
  - Photoshop
tags:
  - blending modes
  - sharpening
  - smart filter
  - smart object
---
<div class="pf-content">
  <p>
    In the previous post I&#8217;ve approached appealing yet wrong solutions to the problem of splitting in a scriptable-friendly way Dark and Light halos of the Unsharp Mask filter (USM) using Smart Objects (SO). I&#8217;ve got to use Smart Filters (SF), which parameters can be easily modified anytime. To find a working answer to the problem (which is just a pretext for going into the topic of sharpening and blending modes), let&#8217;s go back to the basic of Halo Maps creation.
  </p>
  
  <p>
    <!--more-->
  </p>
  
  <p>
    If you&#8217;ve followed the <a title="Decomposing Sharpening #1 Introduction" href="http://localhost:8888/2012/09/decomposing_sharpening_part_1/">first post</a> of the series, this is what a Halo Map for USM Dark halos look like:
  </p>
  
  <div id="attachment_1078" style="width: 1610px" class="wp-caption aligncenter">
    <a href="http://localhost:8888/wp-content/uploads/2012/09/Dark-Halos.jpg" target="_blank"><img aria-describedby="caption-attachment-1078" class="size-full wp-image-1078 " title="Dark Halos" src="http://localhost:8888/wp-content/uploads/2012/09/Dark-Halos.jpg" alt="Dark Halos" width="1600" height="800" srcset="http://localhost:8888/wp-content/uploads/2012/09/Dark-Halos.jpg 1600w, http://localhost:8888/wp-content/uploads/2012/09/Dark-Halos-150x75.jpg 150w, http://localhost:8888/wp-content/uploads/2012/09/Dark-Halos-300x150.jpg 300w, http://localhost:8888/wp-content/uploads/2012/09/Dark-Halos-1024x512.jpg 1024w" sizes="(max-width: 1600px) 100vw, 1600px" /></a>
    
    <p id="caption-attachment-1078" class="wp-caption-text">
      My friend Santo is back &#8211; here with his peculiar Dark Halos map (USM 500%, 3px) coming from a Grayscale version
    </p>
  </div>
  
  <p>
    As a reminder, it&#8217;s been created fading to <strong>Darken</strong> the USM applied to a duplicate of the original, then <strong>subtracting</strong> the original layer from it. The appropriate Blending Mode to simulate USM is <strong>Linear Burn</strong>. How would you get there with SO, avoiding Calculations? Let&#8217;s split the problem into smaller pieces.
  </p>
  
  <h2>
    Fade to Darken / Lighten
  </h2>
  
  <p>
    Duplicate the Original layer, call it &#8220;<em>USM &#8211; Darken</em>&#8220;, then apply USM as a SF. We&#8217;ve a couple of options here, either:
  </p>
  
  <ul>
    <li>
      fade the SF in <strong>Darken</strong>
    </li>
    <li>
      fade the SF in <strong>Luminosity</strong> and set the SO to <strong>Darken</strong>
    </li>
  </ul>
  
  <p>
    <a href="http://localhost:8888/wp-content/uploads/2012/09/USMDarken.jpg" target="_blank"><img class="aligncenter size-full wp-image-1081" title="USM faded to Luminosity, SO set to Darken" src="http://localhost:8888/wp-content/uploads/2012/09/USMDarken.jpg" alt="USM faded to Luminosity, SO set to Darken" width="1139" height="357" srcset="http://localhost:8888/wp-content/uploads/2012/09/USMDarken.jpg 1139w, http://localhost:8888/wp-content/uploads/2012/09/USMDarken-150x47.jpg 150w, http://localhost:8888/wp-content/uploads/2012/09/USMDarken-300x94.jpg 300w, http://localhost:8888/wp-content/uploads/2012/09/USMDarken-1024x320.jpg 1024w" sizes="(max-width: 1139px) 100vw, 1139px" /></a>
  </p>
  
  <p>
    Pick up the one you like the most (I prefer the latter, that avoids chromatic shifts and it&#8217;s a nice way). Nothing new so far.
  </p>
  
  <h2>
    Subtract
  </h2>
  
  <p>
    Photoshop doesn&#8217;t allow you to <em>stack</em> blending modes &#8211; that is, you can&#8217;t set a layer to Overlay, then set the composite result to Luminosity, then set the composite result to Darken. Something you would write:
  </p>
  
  <blockquote>
    <p>
      Darken( Luminosity( Overlay (layer) ) )
    </p>
  </blockquote>
  
  <p>
    A trick that doesn&#8217;t work (it would be really nice if it would) is to use nested <em>Layer Sets</em> (aka Groups), changing the blending modes of each one. No luck, try yourself if you will. Why do I want such a &#8220;double&#8221; blending? Because I&#8217;d like to use Subtract on a SO that already has its own blending mode. A neat solution is to use <strong>Adjustment Layers</strong> as a support &#8211; for instance if you want to Subtract to layers:
  </p>
  
  <ol>
    <li>
      Put the two layers one on top of the other in the Layers palette
    </li>
    <li>
      Create a Curves Adjustment Layer (or Levels, it&#8217;s just the same) right in between. Leave it as it is.
    </li>
    <li>
      Set it to <strong>Subtraction</strong> blending mode
    </li>
    <li>
      Clip the upper layer to it
    </li>
  </ol>
  
  <p>
    <a href="http://localhost:8888/wp-content/uploads/2012/09/Subtraction.jpg" target="_blank"><img class="aligncenter size-full wp-image-1082" title="Subtraction via Adjustment Layers" src="http://localhost:8888/wp-content/uploads/2012/09/Subtraction.jpg" alt="Subtraction via Adjustment Layers" width="1175" height="445" srcset="http://localhost:8888/wp-content/uploads/2012/09/Subtraction.jpg 1175w, http://localhost:8888/wp-content/uploads/2012/09/Subtraction-150x56.jpg 150w, http://localhost:8888/wp-content/uploads/2012/09/Subtraction-300x113.jpg 300w, http://localhost:8888/wp-content/uploads/2012/09/Subtraction-1024x387.jpg 1024w" sizes="(max-width: 1175px) 100vw, 1175px" /></a>
  </p>
  
  <p>
    Pretty nice &#8211; the only limitation is that you can&#8217;t stack more than two blending modes (it would be a really cool feature if Photoshop supported nested, multiple clipping! But it doesn&#8217;t). Actually, I&#8217;m performing a <em>triple</em> blending (SF faded to Luminosity, SO set to Luminosity, clipped to Subtraction) but this is cheating because a SF is involved ;-).<br /> So, we&#8217;ve subtracted the faded-to-Luminosity, Darken USM and the original layer, let&#8217;s go ahead.
  </p>
  
  <h2>
    Invert
  </h2>
  
  <p>
    The Halo Map is almost done, we need to Invert it &#8211; and an Adjustment Layer will come handy. Almost done!
  </p>
  
  <p style="text-align: center;">
    <a href="http://localhost:8888/wp-content/uploads/2012/09/Invert.jpg" target="_blank"><img class="aligncenter size-full wp-image-1084" title="Invert the Halo Map" src="http://localhost:8888/wp-content/uploads/2012/09/Invert.jpg" alt="Invert the Halo Map" width="1173" height="533" srcset="http://localhost:8888/wp-content/uploads/2012/09/Invert.jpg 1173w, http://localhost:8888/wp-content/uploads/2012/09/Invert-150x68.jpg 150w, http://localhost:8888/wp-content/uploads/2012/09/Invert-300x136.jpg 300w, http://localhost:8888/wp-content/uploads/2012/09/Invert-1024x465.jpg 1024w" sizes="(max-width: 1173px) 100vw, 1173px" /></a>
  </p>
  
  <h2>
    Linear Burn
  </h2>
  
  <p>
    How would you set all that to Linear Burn as requested? Think about it: we already have a bunch of interacting Blending modes. The solution lies in the use of Layer Sets, with a caveat.
  </p>
  
  <p>
    If you just group the Subtract Curve, the clipped SO and the Invert Adjustment Layer you&#8217;ll see that no matter what group&#8217;s Blending mode you set, the result doesn&#8217;t change. In order for a Layer Set to effectively use its content for blending, it must have a <strong>bitmap</strong> base: that is, it can&#8217;t be based only upon an Adjustment Layer with clipped stuff on, it plus extra adjustments on top. To make it work, duplicate the Original layer and put it inside the Layer Set, so that you end up with the following setup:
  </p>
  
  <p style="text-align: center;">
    <a href="http://localhost:8888/wp-content/uploads/2012/09/LayerSet_DarkHalos.jpg" target="_blank"><img class="aligncenter size-full wp-image-1085" title="Final LayerSet for Dark Halos" src="http://localhost:8888/wp-content/uploads/2012/09/LayerSet_DarkHalos.jpg" alt="Final LayerSet for Dark Halos" width="1216" height="642" srcset="http://localhost:8888/wp-content/uploads/2012/09/LayerSet_DarkHalos.jpg 1216w, http://localhost:8888/wp-content/uploads/2012/09/LayerSet_DarkHalos-150x79.jpg 150w, http://localhost:8888/wp-content/uploads/2012/09/LayerSet_DarkHalos-300x158.jpg 300w, http://localhost:8888/wp-content/uploads/2012/09/LayerSet_DarkHalos-1024x540.jpg 1024w" sizes="(max-width: 1216px) 100vw, 1216px" /></a>
  </p>
  
  <p>
    Top-down:
  </p>
  
  <ul>
    <li>
      Linear Burn Layer Set, containing&#8230; <ul>
        <li>
          Invert Adjustment Layer <ul>
            <li>
              SO, set to Darken, with a USM SF faded to Luminosity &#8211; clipped to…
            </li>
          </ul>
        </li>
        
        <li>
          Curves Adjustment Layer set to Subtract
        </li>
        <li>
          a duplicate of the original layer
        </li>
      </ul>
    </li>
    
    <li>
      the Original, unsharpened layer
    </li>
  </ul>
  
  <p>
    Does this make sense to you? Within this setup, you can change the USM Dark Amount/Radius on the fly, tweaking the SF (double click on it and change the values)
  </p>
  
  <h2>
    Halfway!
  </h2>
  
  <p>
    We&#8217;re just in the middle of the task &#8211; we still need to create the Light Halos Map. It should be easy now, though! (I&#8217;m lying of course ;-)) One thing that you should keep in mind when trying to replicate this setup on your own before reading along, is that you don&#8217;t just need to fade to <strong>Lighten</strong> (instead of Darken).
  </p>
  
  <p>
    Think about how Subtraction works, say you&#8217;ve a Base layer and a Blend one on top, set to (wonder what) Subtraction. The actual operation carried along is:
  </p>
  
  <blockquote>
    <p>
      Base &#8211; Blend
    </p>
  </blockquote>
  
  <p>
    and not the reverse. If you go back to a simpler grayscale image, you&#8217;ll see that in order to extract the Light Halos the position of the layers in the subtraction must change, otherwise you&#8217;ll get a totally black layer:
  </p>
  
  <p style="text-align: center;">
    <a href="http://localhost:8888/wp-content/uploads/2012/09/SubtractionOrder.jpg" target="_blank"><img class="aligncenter size-full wp-image-1086" title="Subtraction Order" src="http://localhost:8888/wp-content/uploads/2012/09/SubtractionOrder.jpg" alt="Subtraction Order" width="908" height="980" srcset="http://localhost:8888/wp-content/uploads/2012/09/SubtractionOrder.jpg 908w, http://localhost:8888/wp-content/uploads/2012/09/SubtractionOrder-150x161.jpg 150w, http://localhost:8888/wp-content/uploads/2012/09/SubtractionOrder-277x300.jpg 277w" sizes="(max-width: 908px) 100vw, 908px" /></a>
  </p>
  
  <p>
    Eventually, your final setup will look like as follows:
  </p>
  
  <div id="attachment_1088" style="width: 1177px" class="wp-caption aligncenter">
    <a href="http://localhost:8888/wp-content/uploads/2012/09/FinalResult.jpg" target="_blank"><img aria-describedby="caption-attachment-1088" class="size-full wp-image-1088" title="Final Result" src="http://localhost:8888/wp-content/uploads/2012/09/FinalResult.jpg" alt="Final Result" width="1167" height="800" srcset="http://localhost:8888/wp-content/uploads/2012/09/FinalResult.jpg 1167w, http://localhost:8888/wp-content/uploads/2012/09/FinalResult-150x102.jpg 150w, http://localhost:8888/wp-content/uploads/2012/09/FinalResult-300x205.jpg 300w, http://localhost:8888/wp-content/uploads/2012/09/FinalResult-1024x701.jpg 1024w" sizes="(max-width: 1167px) 100vw, 1167px" /></a>
    
    <p id="caption-attachment-1088" class="wp-caption-text">
      Santo is over-sharpened on purpose &#8211; just in case you&#8217;re wondering
    </p>
  </div>
  
  <p>
    Phew. We made it! This is a working configuration that let you split Dark and Light Halos of USM in a non-destructive, updatable way (my script is based on this one), at least for RGB and CMYK documents.<br /> Yes, because in the <strong>Lab</strong> colorspace, <strong>it doesn&#8217;t work</strong> at all (why? Try yourself and find this out if you don&#8217;t imagine the why). Lab requires a completely different approach, that I will explore in the next post of the series.
  </p>
  
  <hr />
  
  <p>
    The <a title="Decomposing Sharpening" href="http://localhost:8888/category/photoshop/decomposing-sharpening/">Decomposing Sharpening</a> series has been written as a research project for my script <a title="DoubleUSM: multi-radius Sharpening" href="http://www.cs-extensions.com/products/doubleusm/" target="_blank">DoubleUSM: multi-radius sharpening</a>. You might be interested also in <a title="Fixel Detailizer 2 PS" href="http://www.cs-extensions.com/products/detailizer/" target="_blank">Fixel Detailizer 2PS: multi-frequency contrast booster</a>.
  </p>
</div>

<!-- Share-Widget Button BEGIN --><a href="javascript:void(0);" myshare\_id="mys\_shareit" myshare\_url="http://localhost:8888/2012/09/decomposing-sharpening-part3-workaround/" myshare\_title="Decomposing Sharpening #3 Workaround" rel="nofollow" onclick=" return false;" style="text-decoration:none; color:#000000; font-size:11px; line-height:20px;"> 

<img src="http://localhost:8888/wp-content/plugins/share-widget/img/share-button-white-small.png" height="20" alt="Share" style="border:0" /> </a> <!-- Share-Widget Button END -->