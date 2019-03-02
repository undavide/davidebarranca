---
id: 1150
title: 'Decomposing Sharpening #5 Arithmetic matters'
date: 2012-09-15T16:59:47+01:00
author: Davide Barranca
excerpt: "When you miss the obvious, bumping into it makes you hyper and lose heart at the same time. I've found a better way to deal with Lab split Dark/Light halos Unsharp Mask and I'm ready to show it to you! "
layout: post
guid: http://www.davidebarranca.com/?p=1150
permalink: /2012/09/decomposing-sharpening-part5-arithmetic-matters/
standard_seo_post_level_layout:
  - ""
standard_seo_post_meta_description:
  - "Working around the lack of Subtraction blending mode in Lab colorspace. If you can't subtract, add the inverse!"
seo_post_level_layout:
  - ""
link_url_field:
  - ""
seo_post_meta_description:
  - ""
image: /wp-content/uploads/2012/09/Arithmetic.jpg
category:
  - Decomposing Sharpening
  - Photoshop
tags:
  - blending modes
  - Lab
  - sharpening
  - smart filter
  - smart object
---
<div class="pf-content">
  <p>
    The previous post of the series (<a title="Decomposing Sharpening #4 The Lab way" href="/2012/09/decomposing-sharpening-part4-the-lab-way/">#4 The Lab way</a>) introduced a different strategy for splitting in an updatable fashion Dark and Light halos of the Unsharp Mask filter (USM) via Smart Objects (SO) and Smart Filters (SF). Basically, I&#8217;ve been manipulating a Subtraction between USM and the original image (Grayscale, RGB and CMYK files) or using the High Pass filter to work around the lack of the Subtraction blending mode in Lab colorspace.<!--more-->
  </p>

  <h2>
    Guilty omissions
  </h2>

  <p>
    I must tell you that I&#8217;ve hit the post #4 &#8220;Publish&#8221; button with a bit of an uncomfortable feeling. I was, deliberately, passing over one important detail when I wrote the formula:
  </p>

  <blockquote>
    <p style="margin: 0px 0px 20px; font-size: 16px; line-height: 22.5px;">
      Amount % = 2*Base + 4*Booster
    </p>
  </blockquote>

  <p>
    Which regulates the total amount (in percent) of the USM on the following setup &#8211; where Base and Booster are the High Pass SO and Curves adjustment layers:
  </p>

  <p style="text-align: center;">
    <a href="/wp-content/uploads/2012/09/LinearLightBooster.jpg" target="_blank"><img class="aligncenter size-full wp-image-1121" title="LinearLight Booster" src="/wp-content/uploads/2012/09/LinearLightBooster.jpg" alt="LinearLight Booster" width="903" height="374" srcset="/wp-content/uploads/2012/09/LinearLightBooster.jpg 903w, /wp-content/uploads/2012/09/LinearLightBooster-150x62.jpg 150w, /wp-content/uploads/2012/09/LinearLightBooster-300x124.jpg 300w" sizes="(max-width: 903px) 100vw, 903px" /></a>
  </p>

  <p>
    The missing information is that you can <em>not</em> tweak the Base opacity, otherwise <em>the result isn&#8217;t USM-like anymore</em>. If you switch off the Booster layer and lower the opacity of the High Pass (Base) layer only to, say, 50% this doesn&#8217;t equal at all an USM of amount 2*50 = 100%:
  </p>

  <p style="text-align: center;">
    <a href="/wp-content/uploads/2012/09/Comparison50.jpg" target="_blank"><img class="aligncenter size-full wp-image-1154" title="Comparison High Pass 50% opacity / USM 100% Amount" src="/wp-content/uploads/2012/09/Comparison50.jpg" alt="Comparison High Pass 50% opacity / USM 100% Amount" width="965" height="585" srcset="/wp-content/uploads/2012/09/Comparison50.jpg 965w, /wp-content/uploads/2012/09/Comparison50-150x90.jpg 150w, /wp-content/uploads/2012/09/Comparison50-300x181.jpg 300w" sizes="(max-width: 965px) 100vw, 965px" /></a>
  </p>

  <p>
    So the Base opacity must stick to 100% &#8211; which binds the original setup (the one above is just a simplified example, please find the details in the <a title="Decomposing Sharpening #4 The Lab way" href="/2012/09/decomposing-sharpening-part4-the-lab-way/" target="_blank">previous post</a>) to work as a split Dark/Light halos USM simulator for Amounts in the range 200% &#8211; 600% only (no 100%, no 50%, etc). Just a negligible detail.
  </p>

  <p>
    <a href="/wp-content/uploads/2012/09/Dimmer.jpg" target="_blank"><img class="alignleft size-medium wp-image-1157" title="Dimmer" src="/wp-content/uploads/2012/09/Dimmer-130x300.jpg" alt="Dimmer" width="130" height="300" /></a> … At least as long as you don&#8217;t throw a new player in that layers crowded arena: that would act as an inverted Booster, something like a <strong>Dimmer</strong>.
  </p>

  <p>
    We&#8217;re talking about an adjustment layer: either a flat, horizontal Curves passing through the middle point (128,128) or better a Layers adjustment with both Output Levels at 128 &#8211; which you must clip to the High Pass SO. As a result, the adjustment makes everything as a nice 50% gray. Theoretically, you should use 16 bits files in order to avoid visible posterization, yet I&#8217;m biased to thing that 8 or 16 wouldn&#8217;t make any remarkable difference: synthetic images are one thing, real world pictures another one (and we&#8217;re talking about posterization in a mostly 50% gray layer with blurred borders here and there).
  </p>

  <p>
    When you need USM Amounts lower than 200%, switch off the Booster, turn on the Dimmer, modulating its opacity. Conversely, for Amounts higher than 200%, turn the Dimmer off, the Booster on and tune its opacity as you&#8217;d do usually. The math that regulates the Dimmer is easy:
  </p>

  <blockquote>
    <p>
      USM Amount % = 200 &#8211; 2*Dimmer opacity
    </p>
  </blockquote>

  <p>
    Back at the time of post #4 writing I didn&#8217;t know how to solve the problem, the solution came out experimenting later. So you may ask Davide, why don&#8217;t you just add this piece of information alongside the original article? The answer lies in the original motivations of the whole analysis: to build a tool for Photoshop (via scripting). As a little researcher of image related topics, I should be fine, happy and done &#8211; the problem&#8217;s fix has been expressed and it works. As a programmer, to implement such a stack of layers is far from being an elegant solution &#8211; and I&#8217;m the laziest around when it comes to write new code.
  </p>

  <h2>
    Subtraction, from a different point of view
  </h2>

  <p>
    It&#8217;s been playing around with blending modes math (which I&#8217;ll be talking about in the next and last post of the series possibly &#8211; notes, errata, variations, etc) that I bumped into the obvious.
  </p>

  <blockquote>
    <p>
      a &#8211; b = a + (-b)
    </p>
  </blockquote>

  <p>
    They call it the &#8220;<em>aha moment</em>&#8220;. Translated in the image arithmetic that matters for us, it means that the following layers setups are just equivalent.
  </p>

  <p style="text-align: center;">
    <a href="/wp-content/uploads/2012/09/Arithmetic.jpg" target="_blank"><img class="aligncenter size-full wp-image-1166" title="Images' arithmetic" src="/wp-content/uploads/2012/09/Arithmetic.jpg" alt="Images' arithmetic" width="1070" height="800" srcset="/wp-content/uploads/2012/09/Arithmetic.jpg 1070w, /wp-content/uploads/2012/09/Arithmetic-150x112.jpg 150w, /wp-content/uploads/2012/09/Arithmetic-300x224.jpg 300w, /wp-content/uploads/2012/09/Arithmetic-1024x765.jpg 1024w" sizes="(max-width: 1070px) 100vw, 1070px" /></a>
  </p>

  <p>
    Which also means that post #4 about the Lab way and the first part of this very one are a just an interesting academic digression. I&#8217;m not going to implement it, relying to this (more efficient? less efficient? surely easier to code) setup:
  </p>

  <p style="text-align: center;">
    <a href="/wp-content/uploads/2012/09/Final.jpg" target="_blank"><img class="aligncenter size-full wp-image-1167" title="Final setup" src="/wp-content/uploads/2012/09/Final.jpg" alt="Final setup" width="1222" height="797" srcset="/wp-content/uploads/2012/09/Final.jpg 1222w, /wp-content/uploads/2012/09/Final-150x97.jpg 150w, /wp-content/uploads/2012/09/Final-300x195.jpg 300w, /wp-content/uploads/2012/09/Final-1024x667.jpg 1024w" sizes="(max-width: 1222px) 100vw, 1222px" /></a>
  </p>

  <p>
    In other terms (just in case you have&#8217;t &#8220;<em>aha-ed</em>&#8221; yet) I can easily bypass the subtraction (forbidden in Lab) with an inversion (Invert adjustment layer &#8211; one or two depending on the cases) followed by an addition (Linear Dodge blending mode). A good news for Davide the scripter.
  </p>

  <p>
    If your eyes are as sharp as your post-produced pictures, you&#8217;d notice I&#8217;m still omitting one detail. The Grayscale/RGB/CMYK routine involves two other blending modes that are forbidden in Lab too, namely <em>Darken</em> and <em>Lighten</em>. Their cousins <em>Darker Color</em> and <em>Lighter Color</em> will work there; the difference basically is that Darker/Lighten work on a channel basis, while Darker/Lighter Color work on the composite. You can visualize the difference here:
  </p>

  <div id="attachment_1168" style="width: 1797px" class="wp-caption aligncenter">
    <a href="/wp-content/uploads/2012/09/DarkenDarkerColor_Comparison.jpg" target="_blank"><img aria-describedby="caption-attachment-1168" class="size-full wp-image-1168" title="Darken / DarkerColor comparison" src="/wp-content/uploads/2012/09/DarkenDarkerColor_Comparison.jpg" alt="Darken / DarkerColor comparison" width="1787" height="800" srcset="/wp-content/uploads/2012/09/DarkenDarkerColor_Comparison.jpg 1787w, /wp-content/uploads/2012/09/DarkenDarkerColor_Comparison-150x67.jpg 150w, /wp-content/uploads/2012/09/DarkenDarkerColor_Comparison-300x134.jpg 300w, /wp-content/uploads/2012/09/DarkenDarkerColor_Comparison-1024x458.jpg 1024w" sizes="(max-width: 1787px) 100vw, 1787px" /></a>

    <p id="caption-attachment-1168" class="wp-caption-text">
      From left to right: Original (base). Gradient (blend). Blend in Darken. Blend in Darker Color
    </p>
  </div>

  <p>
    Yet, if you resort to the old trick of the Advanced Blending Options, deselecting the a,b channels of the blend layer&#8230; and since I want the halos to be applied in Luminosity only, you get reasonably close to the equivalent of Darken/Lighten anyway.
  </p>

  <p>
    This post should end the series: given a problem, I&#8217;ve found and discussed in depth the solutions (even those that have been proved to be less efficient). Yet a closing post will follow, with errata, variations, experiments, and thorough extra examination of some of the topics that have been shown here &#8211; we need to decompress somehow, I guess :-). Thanks for having followed so far!
  </p>

  <hr />

  <p>
    The <a title="Decomposing Sharpening" href="/category/photoshop/decomposing-sharpening/">Decomposing Sharpening</a> series has been written as a research project for my script <a title="DoubleUSM: multi-radius Sharpening" href="http://www.cs-extensions.com/products/doubleusm/" target="_blank">DoubleUSM: multi-radius sharpening</a>. You might be interested also in <a title="Fixel Detailizer 2 PS" href="http://www.cs-extensions.com/products/detailizer/" target="_blank">Fixel Detailizer 2PS: multi-frequency contrast booster</a>.
  </p>
</div>
