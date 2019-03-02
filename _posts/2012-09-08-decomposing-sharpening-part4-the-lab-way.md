---
id: 1099
title: 'Decomposing Sharpening #4 The Lab way'
date: 2012-09-08T18:47:12+01:00
author: Davide Barranca
excerpt: "Lab colorspace is picky and doesn't allow many useful Blending Modes: a different routine (compared to the RGB one) is needed in order to split Dark and Light Halos in the Unsharp Mask Photoshop filter."
layout: post
guid: http://www.davidebarranca.com/?p=1099
permalink: /2012/09/decomposing-sharpening-part4-the-lab-way/
standard_seo_post_level_layout:
  - ""
standard_seo_post_meta_description:
  - 'How to split Dark and Light Halos in Photoshop Unsharp Mask filter - a different sharpening routine is required for Lab, compared to RGB one'
seo_post_level_layout:
  - ""
link_url_field:
  - ""
seo_post_meta_description:
  - ""
image: /wp-content/uploads/2012/09/FinalResult1.jpg
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
    If you&#8217;ve followed this series, you know that I&#8217;m involved in a scripting project for Photoshop that deals with sharpening, splitting Dark and Light Halos control. So far, I&#8217;ve been able to build Layer Sets that mix Smart Objects (SO), Smart Filters (SF) and several Blending modes with good results &#8211; at least if I restrict myself to RGB and CMYK documents.
  </p>

  <p>
    <!--more-->
  </p>

  <p>
    Because when you approach Lab with that very workflow in mind, you hit a notable wall. That is to say, <strong>Subtract</strong> blending mode is not allowed: no Subtraction, no party.
  </p>

  <p>
    In the Lab colorspace, Blending modes act in peculiar ways (some would say: arbitrary). For instance, if you set a layer to <strong>Multiply</strong>, only the <em>L</em> channel is actually multiplied, while <em>a</em> and <em>b</em> behave as in <strong>Overlay</strong>. Why? Go figure. For the same reason (that&#8217;s my own personal opinion) <em>Difference, Exclusion, Subtract, Divide,</em> not to mention <em>Darken</em> and <em>Color Burn, Lighten</em> and<em> Color Dodge</em> are grayed out.
  </p>

  <p>
    One option when you feel stuck in a dead end is to ask for help to friends &#8211; better if they&#8217;re talented, with a solid scientific background, Photoshop experts and coding wizards. Most of the following content is here because of <a title="Jacob Rus" href="http://www.hcs.harvard.edu/~jrus/" target="_blank">Jacob Rus</a>&#8216; kind suggestions and comments. Luckily he also knows how to support the afflicted
  </p>

  <blockquote>
    <p>
      [Davide] I&#8217;ve found out that my biggest efforts have been spent dealing with unexpected behaviors<br /> [Jacob] This is called &#8220;programming&#8221; 😉
    </p>
  </blockquote>

  <p>
    So: a different plan is needed, back to the basics.
  </p>

  <h2>
    Work around Subtraction
  </h2>

  <p>
    If you recall <a title="Decomposing Sharpening #1 Introduction" href="/2012/09/decomposing_sharpening_part_1/" target="_blank">post #1</a> of the series, when I&#8217;ve introduced the concept of the Halo Map I&#8217;ve briefly cited <strong>High Pass</strong> as one of the ways to extract detail up to a certain frequency threshold into a separate layer. High Pass is deeply linked to <strong>Gaussian Blur</strong>, being nothing but a Subtraction between the original image and a blurred one (with <em>Offset 128</em>).
  </p>

  <p>
    You&#8217;ve read at least one interesting word: <em>subtraction</em> &#8211; yes, we&#8217;ll be using High Pass to workaround the Subtraction lack as a blending mode in Lab. To refine the correct setup let&#8217;s switch to grayscale for a quick review of the concepts I&#8217;ve shown throughout the past posts of the series.
  </p>

  <h2>
    Unsharp Mask uses Gaussian Blur
  </h2>

  <p>
    Indeed.
  </p>

  <p style="text-align: center;">
    <a href="/wp-content/uploads/2012/09/GB_DoubleUSM.jpg" target="_blank"><img class="aligncenter size-full wp-image-1113" title="Unsharp Mask with Gaussian Blur" src="/wp-content/uploads/2012/09/GB_DoubleUSM.jpg" alt="Unsharp Mask with Gaussian Blur" width="880" height="524" srcset="/wp-content/uploads/2012/09/GB_DoubleUSM.jpg 880w, /wp-content/uploads/2012/09/GB_DoubleUSM-150x89.jpg 150w, /wp-content/uploads/2012/09/GB_DoubleUSM-300x178.jpg 300w" sizes="(max-width: 880px) 100vw, 880px" /></a>
  </p>

  <h2>
    High Pass is made with Gaussian Blur
  </h2>

  <p>
    That is, <em>High Pass = Original &#8211; Blurred</em>. You can get there in several ways, using Calculations, Blending modes or (as in the following screenshot) with Apply Image &#8211; I&#8217;ve applied the Original in Subtract, Offset 128 on a Gaussian Blurred layer.
  </p>

  <p style="text-align: center;">
    <a href="/wp-content/uploads/2012/09/GB_HP.jpg" target="_blank"><img class="aligncenter size-full wp-image-1105" title="High Pass" src="/wp-content/uploads/2012/09/GB_HP.jpg" alt="High Pass" width="993" height="795" srcset="/wp-content/uploads/2012/09/GB_HP.jpg 993w, /wp-content/uploads/2012/09/GB_HP-150x120.jpg 150w, /wp-content/uploads/2012/09/GB_HP-300x240.jpg 300w" sizes="(max-width: 993px) 100vw, 993px" /></a>
  </p>

  <h2>
     So Unsharp Mask can use High Pass
  </h2>

  <p>
    Which is not a very big news, but few have ever shown the exact setup that replicate USM precisely &#8211; and of course I&#8217;m going to do it here with SO and SF, splitting Dark and Light Halos.
  </p>

  <p>
    Using Linear Light makes the High Pass layer look like USM:<span style="text-align: center;"> </span>
  </p>

  <p style="text-align: center;">
    <a href="/wp-content/uploads/2012/09/LinearLightUSM.jpg" target="_blank"><img class="aligncenter size-full wp-image-1119" title="High Pass USM" src="/wp-content/uploads/2012/09/LinearLightUSM.jpg" alt="High Pass USM" width="857" height="310" srcset="/wp-content/uploads/2012/09/LinearLightUSM.jpg 857w, /wp-content/uploads/2012/09/LinearLightUSM-150x54.jpg 150w, /wp-content/uploads/2012/09/LinearLightUSM-300x108.jpg 300w" sizes="(max-width: 857px) 100vw, 857px" /></a>
  </p>

  <p>
    Question: how much USM is this? Besides the fact that Radius is equal to 20px (the same as the High Pass filter), what about the Amount? Let&#8217;s have a look to Linear Light blending mode:
  </p>

  <blockquote>
    <p>
      [Linear Light] if (Base > ½) = Base + 2*(Blend-½)<br /> [Linear Light] if (Base < ½) = Base + 2*Blend &#8211; 1
    </p>
  </blockquote>

  <p>
    That is to say:
  </p>

  <blockquote>
    <p>
      [Linear Light] = Base + 2*Blend &#8211; 1
    </p>
  </blockquote>

  <p>
    In other words, as Jacob&#8217;s suggested me, &#8220;<em>Linear Light mode just (if you define the midpoint of any channel to be zero) adds twice the top layer to the bottom layer</em>&#8220;. Which should suggest that the above Amount for the High Pass sharpening is equal to 200%.
  </p>

  <p>
    How would you modulate the High Pass layer in order to get to 500%? A neat trick is to use Adjustment Layers in a similar way as we&#8217;ve been doing to <a title="Decomposing Sharpening #3 Workaround" href="/2012/09/decomposing-sharpening-part3-workaround/">stack blending modes</a>, this time clipping a blank Curves layer (Linear Light too) on top of the High Pass. It will act as a blending multiplier (mind you: not Multiply as the blending mode, but like a Linear Light booster):
  </p>

  <p style="text-align: center;">
    <a href="/wp-content/uploads/2012/09/LinearLightBooster.jpg" target="_blank"><img class="aligncenter size-full wp-image-1121" title="LinearLight Booster" src="/wp-content/uploads/2012/09/LinearLightBooster.jpg" alt="LinearLight Booster" width="903" height="374" srcset="/wp-content/uploads/2012/09/LinearLightBooster.jpg 903w, /wp-content/uploads/2012/09/LinearLightBooster-150x62.jpg 150w, /wp-content/uploads/2012/09/LinearLightBooster-300x124.jpg 300w" sizes="(max-width: 903px) 100vw, 903px" /></a>
  </p>

  <p>
     The following formula regulates the interaction between the two Linear Light layers:
  </p>

  <blockquote>
    <p>
      Amount % = 2*Base + 4*Booster
    </p>
  </blockquote>

  <p>
    Where Base and Booster are the layer opacites of High Pass and Curves respectively. In other words the above screenshot shows a USM of Radius 20px and Amount 600% (2*100 + 4*100, pretty cool). Amount 500% requires 75% Booster opacity, Amount 300% requires 25% opacity and so on.
  </p>

  <h2>
    Splitting Halos
  </h2>

  <p>
    The business of splitting Dark and Light sides of USM here is pretty simple: you just need a way to obliterate darker-than-Gray 50% or lighter-than-Gray 50% shades from the High Pass layer. Which you can do with Curves or Layers adjustment layers, as follows the two options for Dark Halos:
  </p>

  <p style="text-align: center;">
    <a href="/wp-content/uploads/2012/09/DarkHalosOnly.jpg" target="_blank"><img class="aligncenter size-full wp-image-1125" title="Dark Halos Only in High Pass" src="/wp-content/uploads/2012/09/DarkHalosOnly.jpg" alt="Dark Halos Only in High Pass" width="1160" height="700" srcset="/wp-content/uploads/2012/09/DarkHalosOnly.jpg 1160w, /wp-content/uploads/2012/09/DarkHalosOnly-150x90.jpg 150w, /wp-content/uploads/2012/09/DarkHalosOnly-300x181.jpg 300w, /wp-content/uploads/2012/09/DarkHalosOnly-1024x617.jpg 1024w" sizes="(max-width: 1160px) 100vw, 1160px" /></a>
  </p>

  <p>
     And the same two options for Light Halos:
  </p>

  <p style="text-align: center;">
    <a href="/wp-content/uploads/2012/09/LightHalosOnly.jpg" target="_blank"><img class="aligncenter size-full wp-image-1126" title="Light Halos Only in High Pass" src="/wp-content/uploads/2012/09/LightHalosOnly.jpg" alt="Light Halos Only in High Pass" width="1160" height="700" srcset="/wp-content/uploads/2012/09/LightHalosOnly.jpg 1160w, /wp-content/uploads/2012/09/LightHalosOnly-150x90.jpg 150w, /wp-content/uploads/2012/09/LightHalosOnly-300x181.jpg 300w, /wp-content/uploads/2012/09/LightHalosOnly-1024x617.jpg 1024w" sizes="(max-width: 1160px) 100vw, 1160px" /></a>
  </p>

  <h2>
     Is there anything missing?
  </h2>

  <p>
    Of course there is, namely the third (often less crucial) parameter: <strong>Threshold</strong>. In order to implement it, I&#8217;m going to leave this gray flatland and switch to color mountains &#8211; where my friend Santo shows his brand new trekking outfit (it was pretty cold in the Alps that day in spite of the sun &#8211; our wives may confirm). Let&#8217;s merge everything I&#8217;ve shown so far (SO, SF, High Pass, split halos, etc):
  </p>

  <p style="text-align: center;">
    <a href="/wp-content/uploads/2012/09/AlmostDoneSanto.jpg" target="_blank"><img class="aligncenter size-full wp-image-1128" title="Unsharp Mask with High Pass - almost done" src="/wp-content/uploads/2012/09/AlmostDoneSanto.jpg" alt="Unsharp Mask with High Pass - almost done" width="1175" height="663" srcset="/wp-content/uploads/2012/09/AlmostDoneSanto.jpg 1175w, /wp-content/uploads/2012/09/AlmostDoneSanto-150x84.jpg 150w, /wp-content/uploads/2012/09/AlmostDoneSanto-300x169.jpg 300w, /wp-content/uploads/2012/09/AlmostDoneSanto-1024x577.jpg 1024w" sizes="(max-width: 1175px) 100vw, 1175px" /></a>
  </p>

  <p>
    Since we&#8217;re in Lab, Linear Light will act wildly on chromatic channels (<em>a, b</em>) too, so you&#8217;d be better switching them off in the two High Pass SO <em>Advanced Blending</em> dialog (right click the SO name and choose <em>Blending Options</em>):
  </p>

  <p style="text-align: center;">
    <a href="/wp-content/uploads/2012/09/Advanced-Blending-Options.png" target="_blank"><img class="aligncenter size-full wp-image-1131" title="Advanced Blending Options" src="/wp-content/uploads/2012/09/Advanced-Blending-Options.png" alt="Advanced Blending Options" width="810" height="660" srcset="/wp-content/uploads/2012/09/Advanced-Blending-Options.png 810w, /wp-content/uploads/2012/09/Advanced-Blending-Options-150x122.png 150w, /wp-content/uploads/2012/09/Advanced-Blending-Options-300x244.png 300w" sizes="(max-width: 810px) 100vw, 810px" /></a>
  </p>

  <p>
    <a href="/wp-content/uploads/2012/09/Threshold.png" target="_blank"><img class="alignleft size-thumbnail wp-image-1135" title="Threshold" src="/wp-content/uploads/2012/09/Threshold-150x292.png" alt="Threshold" width="150" height="292" srcset="/wp-content/uploads/2012/09/Threshold-150x292.png 150w, /wp-content/uploads/2012/09/Threshold.png 409w" sizes="(max-width: 150px) 100vw, 150px" /></a>
  </p>

  <p>
    Threshold rolls in as a new Layers adjustment layer, right above the two High Pass SO. You can use Curves as well, but I prefer Levels because by default they come ranged 0-255 like the actual Threshold is (by the way, while the Amount is expressed in percent, Radius in pixels, Threshold&#8217;s units are Levels, which is quite the case here).
  </p>

  <p>
    In order to nicely visualize how Threshold affects the USM, switch off one side of the Halos (say, the Light Halos High Pass SO) and keep only the Background layer, the Dark Halos High Pass SO and its clipped friends on top. Switch the SO blending mode back to Normal. You should be looking at a gray, high pass stuff with dark halos on it.
  </p>

  <p>
    Click on the <em>Threshold</em> named Layers adjustment and start to move the white Input cursor to the left (the value starts from 255 and lowers). You&#8217;ll see how the dark halos in the High Pass layer are slowly <em>eroded</em> by the threshold &#8211; that is, only strong darker halos resist, while the rest goes to 50% Gray &#8211; this is another way to look at what happens inside USM.
  </p>

  <p style="text-align: center;">
    <a href="/wp-content/uploads/2012/09/ThresholdExplained.jpg" target="_blank"><img class="aligncenter size-full wp-image-1140" title="Visualize USM Threshold" src="/wp-content/uploads/2012/09/ThresholdExplained.jpg" alt="Visualize USM Threshold" width="1510" height="980" srcset="/wp-content/uploads/2012/09/ThresholdExplained.jpg 1510w, /wp-content/uploads/2012/09/ThresholdExplained-150x97.jpg 150w, /wp-content/uploads/2012/09/ThresholdExplained-300x194.jpg 300w, /wp-content/uploads/2012/09/ThresholdExplained-1024x664.jpg 1024w" sizes="(max-width: 1510px) 100vw, 1510px" /></a>
  </p>

  <p>
    You&#8217;ll move the White cursor left in the Dark Halos Threshold, the Black cursor right in the Light Halos Threshold Layers adjustment.
  </p>

  <p>
    The final setup for Lab colorspace is eventually as follows:
  </p>

  <p style="text-align: center;">
    <a href="/wp-content/uploads/2012/09/FinalResult1.jpg" target="_blank"><img class="aligncenter size-full wp-image-1141" title="Final Result for Lab" src="/wp-content/uploads/2012/09/FinalResult1.jpg" alt="Final Result for Lab" width="1203" height="791" srcset="/wp-content/uploads/2012/09/FinalResult1.jpg 1203w, /wp-content/uploads/2012/09/FinalResult1-150x98.jpg 150w, /wp-content/uploads/2012/09/FinalResult1-300x197.jpg 300w, /wp-content/uploads/2012/09/FinalResult1-1024x673.jpg 1024w" sizes="(max-width: 1203px) 100vw, 1203px" /></a>
  </p>

  <p>
    This is the end of our journey decomposing the Unsharp Mask filter: it is something I do because <del>I like it!</del> I&#8217;m at work on a Photoshop script (on this very topic); yet I hope that to break the toy, look in its belly and put together the pieces in new ways have been an interesting learning experience for you &#8211; as it&#8217;s been for me as well writing about it.
  </p>

  <p>
    A last, fifth post will follow (and definitely conclude the series) with notes, errata, variations, additions and what else, goodies perhaps. Thanks for having followed so far, you&#8217;ve earned the right to feel like a Sharpening geek now! 😉
  </p>

  <hr />

  <p>
    The <a title="Decomposing Sharpening" href="/category/photoshop/decomposing-sharpening/">Decomposing Sharpening</a> series has been written as a research project for my script <a title="DoubleUSM: multi-radius Sharpening" href="http://www.cs-extensions.com/products/doubleusm/" target="_blank">DoubleUSM: multi-radius sharpening</a>. You might be interested also in <a title="Fixel Detailizer 2 PS" href="http://www.cs-extensions.com/products/detailizer/" target="_blank">Fixel Detailizer 2PS: multi-frequency contrast booster</a>.
  </p>
</div>
