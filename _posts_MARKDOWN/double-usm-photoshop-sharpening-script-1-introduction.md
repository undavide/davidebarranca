---
title: " Double USM Photoshop Sharpening Script #1: Introduction\t\t"
tags:
  - Double USM
  - halos
  - sharpening
  - unsharp mask
url: 1472.html
id: 1472
category:
  - Double USM
  - Extensions and Scripts
  - Photoshop
date: 2013-01-27 17:50:30
---

Double USM is the brand **new Sharpening script for Photoshop** that I've coded, let me introduce it to you in a three posts series! Post #1, this one, I'll be discussing **sharpening basics and how Double USM is different**. Post #2 is about the **interface, functionality and batch processing**. In post #3 I'll show you **real world examples**, for traditional, HiRaLoAm and mixed sharpening.

### UPDATE

This is a 3 part series on the **Double USM** Photoshop plugin.  
Even if the version used in this article is older than the current one, the sharpening concepts still apply! Find the [new version announcement here](http://localhost:8888/2017/02/double-usm-v2-for-photoshop-has-been-released/)

1.  Introduction (sharpening basics, Double USM) **<\- you're here**
2.  [Features (interface, functionality and Batch processing)](http://localhost:8888/2013/01/double-usm-photoshop-sharpening-script-2-features "Double USM - #2 Features ")
3.  [Examples (case studies)](http://localhost:8888/2013/01/double-usm-photoshop-sharpening-script-3-examples "Double USM - #3 Examples")

Double USM v2 is available for sale on my website [CC-Extensions](http://cc-extensions.com/products/doubleusm/).

Why bother with Sharpening at all?
----------------------------------

![Double USM - High Frequency Detail Sharpening](http://localhost:8888/wp-content/uploads/2013/01/HighFrequencyDetail-300x300.jpg)If you look away from the monitor for a moment, you'll see the **world you live in as a source of an _apparently_ endless detail**; things around you can be probed up to the microscopic level (provided you've got a superhero eyesight) and still reveal new features. Which is not a big news, as it's not a big news to discover that **digital captures** of the world (either a landscape, a portrait, a still life...) you've taken with some expensive equipment **don't contain such an infinite detail** \- just a portion of it. It's not CSI, zoom in 10.000% on your 16bit RAW file and you'll be staring just at a huge, single pixel. The fact you're not allowed by the laws of the universe to capture but a portion of the world's infinite detail, results in a **softer picture** \- if you compare what you perceive out there in the wild, with your digital images. Softness **you've got to compensate** for, especially if pictures are going to be printed (that is, they'll **undergo another reduction of information levels available**). Luckily, our perception is easily deceived by appearances - if you inject your pictures with a little bit of the right _drug_, they suddenly look "right", or "better". That drug is called **sharpening**.

Halos
-----

How is it that we're tricked into _perceiving_ things as more detailed? Short answer: by means of **halos around borders**, that's the little magic. Long answer would mention things like "_retina's receptive fields_", "_lateral inhibition_" and "_Mach bands_" \- but I won't go any further here, even though they're fascinating topics. ![Portrait With Halos](http://localhost:8888/wp-content/uploads/2013/01/PortraitWithHalos.png) So: **a border is found where two differently perceived areas meet**: it may be one darker the other lighter, different color, tone, etc. What sharpening as we know it in the Photoshop's implementation (aka Gaussian Sharpening) does, is to create **two tiny gradients** around borders. A _darker_ one spreading inside the darker area, a _lighter_ one spreading inside the lighter area. If those artifacts are small enough to be noticed as just artifacts, our perception blends them and we get **the impression of more detail** \- we're that easy as humans, you see. ![Photoshop UnSharpMask Filter](http://localhost:8888/wp-content/uploads/2013/01/UnSharpMaskFilter.jpg)

UnSharp Mask filter
-------------------

Photoshop owns one since version 1.0. _UnSharpMask_ (**USM** from now on) lets you specify three parameters to describe the sharpening artifacts (that is: halos) features.

1.  How strong are they (**Amount**)
2.  How widespread are they (**Radius**)
3.  Should they be applied everywhere (**Threshold**)?

The three are independent controls: as far as so called "traditional sharpening" is involved, you usually set large Amounts and relatively small Radii - as in the screenshot, which is a reasonable starting point for web-targeted images; while the contrary may give interesting results as well (read along about HiRaLoAm). Find below the controls' effect on a synthetic test file. \[caption id="attachment_1496" align="alignnone" width="570"\]![USM Settings: Amount](http://localhost:8888/wp-content/uploads/2013/01/USM_Settings_Amount.jpg) Amount: the **Halos' strength**\[/caption\] \[caption id="attachment_1498" align="alignnone" width="570"\]![USM Settings: Radius](http://localhost:8888/wp-content/uploads/2013/01/USM_Settings_Radius.jpg) Radius: the **Halos' thickness**\[/caption\] \[caption id="attachment_1499" align="alignnone" width="570"\]![Threshold: Halos'... threshold](http://localhost:8888/wp-content/uploads/2013/01/USM_Settings_Threshold.jpg) Threshold: the **Halos'... threshold**\[/caption\] I must mention one thing: we all know what **oversharpening** means - it's a delicate topic, photographers go nuts when retouchers oversharpen their images - i.e. they are strong handed with USM filter. \[caption id="attachment_1502" align="alignnone" width="630"\]![Oversharpening example](http://localhost:8888/wp-content/uploads/2013/01/oversharpened.jpg) What do you think about, is this picture oversharpened?\[/caption\] The _psychological threshold_ (how much USM is too much?) is to some extent a subjective choice. As a retoucher, I admit I've kind of a higher tolerance (I know for instance that printed images require, and can stand, much more USM that you'd consider fair just looking at them in a monitor).

So what's wrong with USM?
-------------------------

![Photoshop Maestro Dan Margulis](http://localhost:8888/wp-content/uploads/2013/01/DanMargulis.jpg)My color correction maestro **Dan Margulis** did study the topic of sharpening (among the rest) extensively, and came up with some experimental discoveries worth mentioning. What is that we're bothered the most with, when we look at oversharpened pictures? _Halos_ of course, but he's been the first one to ask the simple question: "Ok, but _which kind_?". **Sharpening introduces at the same time Light Halos and Dark Halos - do we respond to them in the same way?** Take a look at the following picture. \[caption id="attachment_1505" align="alignnone" width="570"\]![Sharpening blending modes](http://localhost:8888/wp-content/uploads/2013/01/Sharpening_BlendingModes.jpg) **A**: original image; **B**: sharpened image (Light + Dark Halos); **C**: Light Halos only; **D**: Dark Halos only.\[/caption\] We may agree that the original version is way too soft, while the sharpened's gone too far. But... what about the rest? Light Halos, as well, may seem too much, while we may be perfectly fine with Dark halos. That is to say: **we're easily disturbed by Light halos, while Dark ones are less annoying**. Whether it is their _strength (Amount)_, or their _size (Radius)_ that bother us, we're perhaps not sure about - but Light Halos should be toned down, somehow. So far, an easy and well known way to do this was to apply USM on a layer, duplicate it, set one layer to Darken (Dark Halos only) and the other to Lighten (Light Halos only) and lower the latter's opacity. Which is somehow equivalent to lower the Amount of the Light Halos altogether. But what about if Light Halos could be made smaller (_Radius_) too? \[caption id="attachment_1515" align="alignnone" width="570"\]![Double USM Sharpening](http://localhost:8888/wp-content/uploads/2013/01/Double_Sharpening_ef.jpg) **E**: image sharpened traditionally. F: sharpening where Light Halos are **half the strength** of, and **two third thinner** than, the Dark Halos. (heavy handed correction just to show you more clearly the difference!)\[/caption\] The picture will benefit from a sharpening that is more robust on the dark side (_stronger in Amount, wider in Radius_), and less on the light side (_weaker Amount, narrower Radius_), as you can see in the above comparison. \[caption id="attachment_1512" align="alignleft" width="295"\]![Double USM gui](http://localhost:8888/wp-content/uploads/2013/01/DoubleUSM_gui-295x300.png) Double USM user interface: **Amount and Radius controls for both Dark and Light Halos**\[/caption\]

Double USM
----------

Since drum scanners' softwares did have this feature decades ago, why not implementing it back in Photoshop now? **Double USM lets you tweak both Amount and Radius for Dark and Light halos separately, at the same time.** A double set of sliders can be useful not only for **traditional sharpening**, but it gives unprecedented control also for **HiRaLoAm** (as Dan Margulis calls it: sharpening with unusual _High Radius and Low Amount_, used as a local contrast enhancer), or as a tool for **mixed sharpening** (Dark Halos: traditional, Light Halos: HiRaLoAm). _You may be tempted to do the trick with Darken and Lighten layers but save your time: alas it can't work, as [I've demonstrated here](http://localhost:8888/2012/09/decomposing-sharpening-part2-mistakes/ "Decomposing Sharpening #2 Mistakes")._

### UPDATE

This is a 3 part series on the **Double USM** Photoshop plugin.  
Even if the version used in this article is older than the current one, the sharpening concepts still apply! Find the [new version announcement here](http://localhost:8888/2017/02/double-usm-v2-for-photoshop-has-been-released/)

1.  Introduction (sharpening basics, Double USM) **<\- you're here**
2.  [Features (interface, functionality and Batch processing)](http://localhost:8888/2013/01/double-usm-photoshop-sharpening-script-2-features "Double USM - #2 Features ")
3.  [Examples (case studies)](http://localhost:8888/2013/01/double-usm-photoshop-sharpening-script-3-examples "Double USM - #3 Examples")

Double USM v2 is available for sale on my website [CC-Extensions](http://cc-extensions.com/products/doubleusm/).