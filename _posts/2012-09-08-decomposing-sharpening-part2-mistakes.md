---
id: 1057
title: 'Decomposing Sharpening #2 Mistakes'
date: 2012-09-08T10:53:13+01:00
author: Davide Barranca
excerpt: "Easy, intuitive solutions may not survive accurate tests, proving to be… simply wrong. That's true in coding, Photoshop and life in general!"
layout: post
guid: http://www.davidebarranca.com/?p=1057
permalink: /2012/09/decomposing-sharpening-part2-mistakes/
standard_seo_post_level_layout:
  - ""
standard_seo_post_meta_description:
  - 'Splitting Dark and Light halos of Photoshop Unsharp Mask filter seems easy - yet intuitive solutions may mislead you towards blatant errors.'
seo_post_level_layout:
  - ""
link_url_field:
  - ""
seo_post_meta_description:
  - ""
image: /wp-content/uploads/2012/09/Original.png
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
    In the <a title="Decomposing Sharpening #1 Introduction" href="/2012/09/decomposing_sharpening_part_1/" target="_blank">previous post</a> I cited a tool for Photoshop I&#8217;m working on &#8211; which is currently on its beta cycle, thanks to the people of <a title="Applied Color Theory in Photoshop - Yahoo Group" href="http://tech.groups.yahoo.com/group/colortheory/" target="_blank">Colortheory</a>. Basically, it&#8217;s a Photoshop script that lets you control separately both Amount and Radius for Unsharp Mask filter&#8217;s Dark and Light halos: that is, you can apply USM 500% and 1.5px for Dark Halos and 300%, 0.9px for Light ones, tweaking sliders on a single GUI.<br /> <!--more-->

    <br /> Being it a <em>Script</em> and not a <em>Filter</em> means that I have to drive Photoshop behind the scenes while you play with the interface (I don&#8217;t have pixel access, it would require a programming knowledge exceeding my skills). In other words I must find a way to replicate with layers and filters that very effect; not only, the scheme update must be easy, because I can&#8217;t afford the whole processing to be time consuming (otherwise the live-update performance of the tool would degrade).
  </p>

  <p>
    That is to say: I won&#8217;t follow the steps outlined in the <a title="Decomposing Sharpening #1 Introduction" href="/2012/09/decomposing_sharpening_part_1/" target="_blank">post #1</a> of the series &#8211; producing two halo maps via Calculations, which would require a new calculation each time the user changes a single value. I need on-the-fly updatable parameters &#8211; I&#8217;m afraid I need <strong>Smart Objects</strong> (SO). I&#8217;m not particularly fond of SO &#8211; yet they provide <strong>Smart Filters</strong> (SF) which fit perfectly my requirements: SF are Filters applied non-permanently to a SO, i.e. you can decide to change their parameters anytime, or switch them on/off.
  </p>

  <p>
    Given this framework, how would you deal with SO and SF to split USM dark and light halos? There are at least two easy and intuitive ways that may came to your Photoshop-driven mind (they came to mine, too) &#8211; yet they&#8217;re <em>wrong</em>; this post is just about this &#8211; how not to be fooled by appealing, easy paths.
  </p>

  <h2>
    Blending modes: Darken / Lighten
  </h2>

  <p>
    Let me use this exciting picture as an example:
  </p>

  <p style="text-align: center;">
    <img class="aligncenter size-full wp-image-1059" src="/wp-content/uploads/2012/09/Original.png" alt="Original" width="580" height="290" srcset="/wp-content/uploads/2012/09/Original.png 580w, /wp-content/uploads/2012/09/Original-150x75.png 150w, /wp-content/uploads/2012/09/Original-300x150.png 300w" sizes="(max-width: 580px) 100vw, 580px" />
  </p>

  <p>
    <em>Reminder</em>: to convert a layer in a Smart Object, right click on the layer&#8217;s name in the Layers palette and choose <em>Convert to Smart Object</em>. Be aware that if you right click on the layer&#8217;s icon the option does not show (there must be good reasons that I miss for that). Alternatively, via menu: <em>Layer &#8211; Smart Objects &#8211; Convert to Smart Object</em>.
  </p>

  <p>
    That said, if you duplicate the Background layer, convert it to SO, apply the Unsharp Mask Filter (USM, say 500% 10px, we&#8217;ll always leave Threshold untouched) and set the SO blending mode to <strong>Lighten</strong>, this is what you get:
  </p>

  <p style="text-align: center;">
    <a href="/wp-content/uploads/2012/09/Lighten.png" target="_blank"><img class="aligncenter size-full wp-image-1060" src="/wp-content/uploads/2012/09/Lighten.png" alt="Lighten blending mode" width="870" height="359" srcset="/wp-content/uploads/2012/09/Lighten.png 870w, /wp-content/uploads/2012/09/Lighten-150x61.png 150w, /wp-content/uploads/2012/09/Lighten-300x123.png 300w" sizes="(max-width: 870px) 100vw, 870px" /></a>
  </p>

  <p>
    Same concept but a different USM (500%, 30px) blended in <strong>Darken</strong>:
  </p>

  <p>
    <a href="/wp-content/uploads/2012/09/Darken.png" target="_blank"><img class="aligncenter size-full wp-image-1065" src="/wp-content/uploads/2012/09/Darken.png" alt="Darken blending mode" width="870" height="359" srcset="/wp-content/uploads/2012/09/Darken.png 870w, /wp-content/uploads/2012/09/Darken-150x61.png 150w, /wp-content/uploads/2012/09/Darken-300x123.png 300w" sizes="(max-width: 870px) 100vw, 870px" /></a>
  </p>

  <p>
    So what would we get if we mix the two?
  </p>

  <p>
    <a href="/wp-content/uploads/2012/09/ApparentlyWorking.png" target="_blank"><img class="size-full wp-image-1066 aligncenter" src="/wp-content/uploads/2012/09/ApparentlyWorking.png" alt="Apparently Working set" width="867" height="483" srcset="/wp-content/uploads/2012/09/ApparentlyWorking.png 867w, /wp-content/uploads/2012/09/ApparentlyWorking-150x83.png 150w, /wp-content/uploads/2012/09/ApparentlyWorking-300x167.png 300w" sizes="(max-width: 867px) 100vw, 867px" /></a>
  </p>

  <p>
    Exactly what you would have expected &#8211; that&#8217;s it, we&#8217;re done splitting dark and light halos. With a caveat: what if we reverse the layer&#8217;s order (Lighten top, Darken bottom)?
  </p>

  <p style="text-align: center;">
    <a href="/wp-content/uploads/2012/09/ApparentlyNotWorking.png" target="_blank"><img class="aligncenter size-full wp-image-1067" src="/wp-content/uploads/2012/09/ApparentlyNotWorking.png" alt="Apparently Not Working set" width="867" height="483" srcset="/wp-content/uploads/2012/09/ApparentlyNotWorking.png 867w, /wp-content/uploads/2012/09/ApparentlyNotWorking-150x83.png 150w, /wp-content/uploads/2012/09/ApparentlyNotWorking-300x167.png 300w" sizes="(max-width: 867px) 100vw, 867px" /></a>
  </p>

  <p>
    Uhm, it doesn&#8217;t work anymore. So the trick is to keep the SO with the bigger USM Radius on top of the other &#8211; easy peasy.
  </p>

  <p>
    Alas, I should have been more careful, because I&#8217;ve based a good deal of coding time on this paradigm (two SO in Darken / Lighten) just to discover that it is <strong>flawed</strong>. What a unhappy business! Not to mention what my wife has been repeating me (she used to work as a programmer): &#8220;<em>Did you test it? You did it not enough. Test it more or you&#8217;ll waste your time later. Don&#8217;t complain, I told you this before: test it!</em>&#8221; She&#8217;s been in charge of testing, as you may guess.
  </p>

  <p>
    But&#8230; why it doesn&#8217;t work? The example I&#8217;ve showed is misleading because as a test it is not exhaustive one: I&#8217;ve changed one parameter only &#8211; the Radius, keeping the Amount full blast at 500% What happens for instance if I lower back the Amount of Dark halos to 100%? No matter how I would set the layers order, the result does not match my expectations:
  </p>

  <p style="text-align: center;">
    <a href="/wp-content/uploads/2012/09/Comparison.png" target="_blank"><img class="aligncenter size-full wp-image-1069" src="/wp-content/uploads/2012/09/Comparison.png" alt="Comparison" width="869" height="1011" srcset="/wp-content/uploads/2012/09/Comparison.png 869w, /wp-content/uploads/2012/09/Comparison-150x174.png 150w, /wp-content/uploads/2012/09/Comparison-257x300.png 257w" sizes="(max-width: 869px) 100vw, 869px" /></a>
  </p>

  <p>
    First row (Lighten on top) is obviously flawed because it erases the Darken effect &#8211; the dark halo is wrong. Middle row (Darken on top) is not correct too, because the light halo is slightly darkened by the dark halo: it&#8217;s subtle because I&#8217;ve used a low Amount. Check out the last row to compare them with the right effect. Back to square one.
  </p>

  <h2>
    Multiple Smart Filters
  </h2>

  <p>
    A single SO can support more than one SF (how many, frankly I don&#8217;t know). A nice idea could be to apply <strong>two rounds</strong> of USM as SF to the same SO, fading them to Darken / Lighten. To fade a SF, click on the &#8220;lines-and-arrows&#8221; icon at the right of the &#8220;Unsharp Mask&#8221; label of the Smart Filter.
  </p>

  <p style="text-align: center;">
    <a href="/wp-content/uploads/2012/09/MultipleSF.png" target="_blank"><img class="aligncenter size-full wp-image-1070" src="/wp-content/uploads/2012/09/MultipleSF.png" alt="Multiple Smart Filters" width="1080" height="430" srcset="/wp-content/uploads/2012/09/MultipleSF.png 1080w, /wp-content/uploads/2012/09/MultipleSF-150x59.png 150w, /wp-content/uploads/2012/09/MultipleSF-300x119.png 300w, /wp-content/uploads/2012/09/MultipleSF-1024x407.png 1024w" sizes="(max-width: 1080px) 100vw, 1080px" /></a>
  </p>

  <p>
    There&#8217;s a detail that prevent this one to work, though. SFs don&#8217;t operate in parallel, but in <strong>sequence</strong>: that is, the first USM round acts on the original SO (as expected); the second USM round operates on the result of the first SF, i.e. upon an <em>already sharpened</em> version. No matter how you set the SF stack order (in the following image: first Darken top, second Lighten top), the result is wrong.
  </p>

  <p>
    <a href="/wp-content/uploads/2012/09/MultipleSFComparison.png" target="_blank"><img class="aligncenter size-full wp-image-1071" src="/wp-content/uploads/2012/09/MultipleSFComparison.png" alt="Smart Filters stack comparison" width="580" height="580" srcset="/wp-content/uploads/2012/09/MultipleSFComparison.png 580w, /wp-content/uploads/2012/09/MultipleSFComparison-150x150.png 150w, /wp-content/uploads/2012/09/MultipleSFComparison-300x300.png 300w" sizes="(max-width: 580px) 100vw, 580px" /></a>
  </p>

  <h2>
    So what?
  </h2>

  <p>
    As a consequence of these experiments and in a bad mood for the time wasted coding a flawed paradigm (lesson learned: test extensively! Don&#8217;t get excited when things apparently work!), I&#8217;ve had to resort to some&#8230; trickier solutions &#8211; which I&#8217;ll show you in the next post of the series.
  </p>

  <hr />

  <p>
    The <a title="Decomposing Sharpening" href="/category/photoshop/decomposing-sharpening/">Decomposing Sharpening</a> series has been written as a research project for my script <a title="DoubleUSM: multi-radius Sharpening" href="http://www.cs-extensions.com/products/doubleusm/" target="_blank">DoubleUSM: multi-radius sharpening</a>. You might be interested also in <a title="Fixel Detailizer 2 PS" href="http://www.cs-extensions.com/products/detailizer/" target="_blank">Fixel Detailizer 2PS: multi-frequency contrast booster</a>.
  </p>
</div>
