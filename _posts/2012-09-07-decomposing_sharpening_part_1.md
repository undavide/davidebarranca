---
id: 1034
title: 'Decomposing Sharpening #1 Introduction'
date: 2012-09-07T23:01:46+01:00
author: Davide Barranca
excerpt: First part of a series about the Photoshop sharpening algorithm and ways to decompose it in order to create new detail enhancing tools.
layout: post
guid: http://www.davidebarranca.com/?p=1034
permalink: /2012/09/decomposing_sharpening_part_1/
standard_seo_post_level_layout:
  - ""
standard_seo_post_meta_description:
  - First part of a series about the Photoshop sharpening algorithm and ways to decompose it in order to create new detail enhancing tools.
seo_post_level_layout:
  - ""
link_url_field:
  - ""
seo_post_meta_description:
  - ""
image: /wp-content/uploads/2012/09/HighPass.jpg
category:
  - Decomposing Sharpening
  - Photoshop
tags:
  - blending modes
  - Calculations
  - high pass
  - sharpening
---
<div class="pf-content">
  <p>
    I&#8217;m currently working on a project of mine (Photoshop script) that involves sharpening. This has been giving me the opportunity to err (quite a bit) and discover some interesting combinations that may lead to new tools. In this first post of a small multipart series, I&#8217;ll review basic concepts like Halo Maps, Subtractions and Blending Modes applied to sharpening.<!--more-->
  </p>

  <p>
    I&#8217;ve always been a great fan of Dan Margulis&#8217; PPW (<a title="Dan Margulis PPW panel" href="http://www.ledet.com/margulis/ppw" target="_blank">Picture Postcard Workflow</a>) panel, coded by my dear friend and talented scripter <a title="Giuly Abbiati" href="http://www.cromaline.net" target="_blank">Giuly Abbiati</a>. If you don&#8217;t know it, I&#8217;m talking about a Photoshop extension filled up with great tools that make Dan&#8217;s color correction workflow really speedy and automated &#8211; provided that you know the theory behind it. Over the years Margulis has freely distributed updated actions that target very specific topics &#8211; of course sharpening is one of them and this is what I&#8217;d like to go into.
  </p>

  <p>
    <img class="alignleft size-medium wp-image-1039" title="PPW Layers" src="/wp-content/uploads/2012/09/PPW-Layers-154x300.png" alt="PPW Layers" width="154" height="300" srcset="/wp-content/uploads/2012/09/PPW-Layers-154x300.png 154w, /wp-content/uploads/2012/09/PPW-Layers-150x291.png 150w, /wp-content/uploads/2012/09/PPW-Layers.png 274w" sizes="(max-width: 154px) 100vw, 154px" />The last evolution stage of such action (at the time of this post in September 2012, but new versions may appear for this is a very active project) provides you with several layers &#8211; each one aiming to a peculiar target: among the rest there are two separate high-frequency sharpening layers, one for Light Halos and one for Dark Halos. If you&#8217;re interested in the exact way they&#8217;re produced, applied and masked within the context of the PPW workflow, please refer to Dan&#8217;s original action and follow each step.
  </p>

  <p>
    Let&#8217;s first approach the idea of creating a <em>Halo Map</em> &#8211; that is, a single layer that contains just what is needed in order to sharpen and is applied on top of the original image with an appropriate blending mode. The purpose of such layer is to separate the effect (a detail enhancing filter) from its substrate (the original image).
  </p>

  <p>
    A trivial and well known Halo Map is easily built with the <strong>High Pass</strong> filter:
  </p>

  <div id="attachment_1036" style="width: 1034px" class="wp-caption aligncenter">
    <a href="/wp-content/uploads/2012/09/HighPass.jpg" target="_blank"><img aria-describedby="caption-attachment-1036" class="size-large wp-image-1036 " title="High Pass sample" src="/wp-content/uploads/2012/09/HighPass-1024x768.jpg" alt="High Pass Filter" width="1024" height="768" srcset="/wp-content/uploads/2012/09/HighPass-1024x768.jpg 1024w, /wp-content/uploads/2012/09/HighPass-150x112.jpg 150w, /wp-content/uploads/2012/09/HighPass-300x225.jpg 300w, /wp-content/uploads/2012/09/HighPass.jpg 1066w" sizes="(max-width: 1024px) 100vw, 1024px" /></a>

    <p id="caption-attachment-1036" class="wp-caption-text">
      Please welcome my friend Santo &#8211; here showing his trekking outfit and the result of the High Pass filter
    </p>
  </div>

  <p>
    Switching the blending mode to some contrast enhancing one (such Overlay, Soft Light, Hard Light, etc.).
  </p>

  <p>
    Strangely enough, at least in my personal opinion, High Pass is mainly used for <em>HiRaLoAm</em> (High-Radius-Low-Amount) sharpening &#8211; that is, not targeting high frequency detail but as a local contrast enhancer (speaking of which, let me remind you <a title="ALCE - Advanced Local Contrast Enhancer" href="http://www.bigano.com/ALCE" target="_blank">ALCE</a> as my favorite tool ;-)) Bizarre, because there is nothing to prevent you using High Pass to create a traditional sharpening layer.
  </p>

  <p>
    In a nutshell, the High Pass filter is a quick way to cut every image frequency up to the radius you&#8217;ve set: that is, radius 10px replaces with a nice 50% gray everything in the image but frequencies higher than 10px &#8211; if this sounds mysterious, dig the topic <a title="Notes on Sharpening - Image Decomposition" href="http://bigano.com/index.php/en/consulting/40-davide-barranca/90-davide-barranca-notes-on-sharpening.html?start=5" target="_blank">here</a>. I&#8217;m sure you&#8217;re willing to know that the result of High Pass Filter is exactly equal to a <strong>Subtraction</strong>: between the original image and a Gaussian Blurred one (with the same radius).
  </p>

  <p style="text-align: center;">
    <a href="/wp-content/uploads/2012/09/SubtractionHighPass128.png" target="_blank"><img class="aligncenter size-full wp-image-1103" title="Subtraction - HighPass" src="/wp-content/uploads/2012/09/SubtractionHighPass128.png" alt="Subtraction - HighPass" width="613" height="489" srcset="/wp-content/uploads/2012/09/SubtractionHighPass128.png 613w, /wp-content/uploads/2012/09/SubtractionHighPass128-150x119.png 150w, /wp-content/uploads/2012/09/SubtractionHighPass128-300x239.png 300w" sizes="(max-width: 613px) 100vw, 613px" /></a>
  </p>

  <p>
    The only extra detail that you should keep in mind is that <strong>Offset is 128</strong>. The actual operation performed is then (speaking of values of each pixel, ranging from 0 to 255 alias black to white):
  </p>

  <blockquote>
    <p>
      High Pass = 128 + ( Original &#8211; Blurred )
    </p>
  </blockquote>

  <p>
    Incidentally, 128 is 50% gray, which is the <strong>neutral color</strong> of contrast enhancing blending modes (i.e. it does not affect the image below)
  </p>

  <p>
    <em>Be aware that if you like to test this yourself using RGB images, you must set the Photoshop Grayscale Default (Color Settings, Command + Shift + K for Mac or use Ctrl instead of Command for PC &#8211; I&#8217;ll be using Mac shortcuts throughout the post) to an ICC with a gamma that matches the one of the RGB file you&#8217;re working on. If you&#8217;re about to ask why, the reason is in the way Photoshop displays channels; as a rule of thumb if your file is ProPhoto RGB, set the Grayscale ICC to Gray Gamma 1.8, if you&#8217;re using Adobe RGB or sRGB set it to Gray Gamma 2.2 (more on colorspaces gamma in <a title="Bruce Lindbloom - RGB Working Space Information" href="http://brucelindbloom.com/WorkingSpaceInfo.html" target="_blank">Bruce Lindbloom&#8217;s website</a>)</em>
  </p>

  <p>
    Let&#8217;s move towards more interesting Halo Maps:
  </p>

  <p>
    <a href="/wp-content/uploads/2012/09/PPW_Halos.jpg" target="_blank"><img class="aligncenter size-large wp-image-1047" title="PPW Halo Maps" src="/wp-content/uploads/2012/09/PPW_Halos-1024x767.jpg" alt="PPW Halo Maps" width="1024" height="767" srcset="/wp-content/uploads/2012/09/PPW_Halos-1024x767.jpg 1024w, /wp-content/uploads/2012/09/PPW_Halos-150x112.jpg 150w, /wp-content/uploads/2012/09/PPW_Halos-300x224.jpg 300w, /wp-content/uploads/2012/09/PPW_Halos.jpg 1062w" sizes="(max-width: 1024px) 100vw, 1024px" /></a>
  </p>

  <p>
    The screenshot above shows the actual high frequency Dark Halos (left) and Light Halos (right) layers from Dan&#8217;s PPW sharpening action, without masks. The former is mostly white because its blending mode is set to <strong>Multiply</strong> &#8211; that is, any value is multiplied with the one from below, resulting in darker pixels. How come? Because for the operation the values used are defined in the range 0 to 1 and not 0 to 255 (either way, black to white): this way, if you multiply two 50% grays you end up with 0.5 * 0.5 = 0.25 which is 75% gray (definitely darker). White, which is what Dark Halos map layer is mainly made of, is the neutral color of Multiply.
  </p>

  <p>
    Not surprisingly, the Light Halos layer looks mostly black, being in <strong>Screen</strong> blending mode (the inverse of Multiply) &#8211; where black is the neutral color.
  </p>

  <p>
    To sum up, Multiply darkens (neutral: White), Screen lightens (neutral: Black), <strong>Overlay</strong> is a combination of Screen and Multiply (Gray 50% is the neutral color). It&#8217;s contrast enhancing just because it darkens and lightens at the same time, depending on the layer content: darker gray darkens, lighter gray lightens, pivoting on middle gray. Citing Paul Dunn, who has a <a title="Paul Dunn - Photoshop Blending modes" href="http://dunnbypaul.net/blends/" target="_blank">very informative page</a> about blending modes:
  </p>

  <blockquote>
    <p>
      [Multiply] = Base * Blend<br /> [Screen] = 1 &#8211; (1-Base) × (1-Blend)<br /> [Overlay, if (Base > ½)] = 1 &#8211; (1-2×(Base-½)) × (1-Blend).<br /> [Overlay, If (Base <= ½)] = (2×Base) × Blend
    </p>
  </blockquote>

  <p>
    This is what you need to know in order to replicate halo maps such as the ones found in the PPW action. Yet Dan applies them in a peculiar way (I&#8217;ll let you the pleasure to decipher the action in order to get the details) &#8211; I&#8217;ll be describing a more orthodox path, leading to halo maps that emulate the Unsharp Mask filter (USM)
  </p>

  <ol>
    <li>
      On a duplicate layer, apply USM, the settings you like the most
    </li>
    <li>
      [for Light Halos] <a title="Fade to Lighten" href="/wp-content/uploads/2012/09/FadeToLighten.png" target="_blank">Fade to Lighten</a> (Command + Shift + F)
    </li>
    <li>
      Use Calculation and Subtract the original to the USM version
    </li>
  </ol>

  <div>
    <a href="/wp-content/uploads/2012/09/Calculations.png"><img class="aligncenter size-full wp-image-1051" title="Calculations" src="/wp-content/uploads/2012/09/Calculations.png" alt="Calculations" width="613" height="489" srcset="/wp-content/uploads/2012/09/Calculations.png 613w, /wp-content/uploads/2012/09/Calculations-150x119.png 150w, /wp-content/uploads/2012/09/Calculations-300x239.png 300w" sizes="(max-width: 613px) 100vw, 613px" /></a>
  </div>

  <p>
    That&#8217;s it. To get the Dark Halos map instead, just Fade to Darken, subtract the USM faded Darken from the Original and <strong>invert</strong> the result of the Calculation. A useful shortcut is to use <strong>Subtraction blending mode</strong> on the USM layer to skip the Calculation step.
  </p>

  <p>
    Now that you&#8217;ve built dark and light layers, set them to the appropriate blending mode. Which one? Dan&#8217;s been choosing Multiply and Screen because his workflow involves masks and a slightly different process, yet if you want to get the exact result of the USM filter with the layers we&#8217;ve built together you should use <strong>Linear Burn</strong> and <strong>Linear Dodge</strong>.
  </p>

  <blockquote>
    <p>
      [Linear Burn] = Base + Blend<br /> [Linear Dodge] = Base + Blend &#8211; 1
    </p>
  </blockquote>

  <p>
    If you look at the math, we&#8217;ve first performed a subtraction between Original and Sharpened, to extract the Difference (Halos); then we&#8217;re adding back that Difference (Halos) to the Original in order to get back the Sharpened. That is to say:
  </p>

  <blockquote>
    <p>
      Sharpened &#8211; Original = Halos<br /> Original + Halos = Sharpened
    </p>
  </blockquote>

  <p>
    Trivial. But… What for?! Many interesting little things, for instance driving separately the Amount and Radius of Dark and Light Halos in Sharpening &#8211; a common feature in Drum Scanners&#8217; softwares in the past, missing in Photoshop today &#8211; as you&#8217;ll see in the next post of the series.
  </p>

  <hr />

  <p>
    The <a title="Decomposing Sharpening" href="/category/photoshop/decomposing-sharpening/">Decomposing Sharpening</a> series has been written as a research project for my script <a title="DoubleUSM: multi-radius Sharpening" href="http://www.cs-extensions.com/products/doubleusm/" target="_blank">DoubleUSM: multi-radius sharpening</a>. You might be interested also in <a title="Fixel Detailizer 2 PS" href="http://www.cs-extensions.com/products/detailizer/" target="_blank">Fixel Detailizer 2PS: multi-frequency contrast booster</a>.
  </p>
</div>
