---
title: " Decomposing Sharpening #5 Arithmetic matters\t\t"
tags:
  - blending modes
  - Lab
  - sharpening
  - smart filter
  - smart object
url: 1150.html
id: 1150
comments: false
category:
  - Decomposing Sharpening
  - Photoshop
date: 2012-09-15 16:59:47
---

The previous post of the series ([#4 The Lab way](http://localhost:8888/2012/09/decomposing-sharpening-part4-the-lab-way/ "Decomposing Sharpening #4 The Lab way")) introduced a different strategy for splitting in an updatable fashion Dark and Light halos of the Unsharp Mask filter (USM) via Smart Objects (SO) and Smart Filters (SF). Basically, I've been manipulating a Subtraction between USM and the original image (Grayscale, RGB and CMYK files) or using the High Pass filter to work around the lack of the Subtraction blending mode in Lab colorspace.

Guilty omissions
----------------

I must tell you that I've hit the post #4 "Publish" button with a bit of an uncomfortable feeling. I was, deliberately, passing over one important detail when I wrote the formula:

> Amount % = 2\*Base + 4\*Booster

Which regulates the total amount (in percent) of the USM on the following setup - where Base and Booster are the High Pass SO and Curves adjustment layers:

[![LinearLight Booster](http://localhost:8888/wp-content/uploads/2012/09/LinearLightBooster.jpg "LinearLight Booster")](http://localhost:8888/wp-content/uploads/2012/09/LinearLightBooster.jpg)

The missing information is that you can _not_ tweak the Base opacity, otherwise _the result isn't USM-like anymore_. If you switch off the Booster layer and lower the opacity of the High Pass (Base) layer only to, say, 50% this doesn't equal at all an USM of amount 2*50 = 100%:

[![Comparison High Pass 50% opacity / USM 100% Amount](http://localhost:8888/wp-content/uploads/2012/09/Comparison50.jpg "Comparison High Pass 50% opacity / USM 100% Amount")](http://localhost:8888/wp-content/uploads/2012/09/Comparison50.jpg)

So the Base opacity must stick to 100% - which binds the original setup (the one above is just a simplified example, please find the details in the [previous post](http://localhost:8888/2012/09/decomposing-sharpening-part4-the-lab-way/ "Decomposing Sharpening #4 The Lab way")) to work as a split Dark/Light halos USM simulator for Amounts in the range 200% - 600% only (no 100%, no 50%, etc). Just a negligible detail. [![Dimmer](http://localhost:8888/wp-content/uploads/2012/09/Dimmer-130x300.jpg "Dimmer")](http://localhost:8888/wp-content/uploads/2012/09/Dimmer.jpg) … At least as long as you don't throw a new player in that layers crowded arena: that would act as an inverted Booster, something like a **Dimmer**. We're talking about an adjustment layer: either a flat, horizontal Curves passing through the middle point (128,128) or better a Layers adjustment with both Output Levels at 128 - which you must clip to the High Pass SO. As a result, the adjustment makes everything as a nice 50% gray. Theoretically, you should use 16 bits files in order to avoid visible posterization, yet I'm biased to thing that 8 or 16 wouldn't make any remarkable difference: synthetic images are one thing, real world pictures another one (and we're talking about posterization in a mostly 50% gray layer with blurred borders here and there). When you need USM Amounts lower than 200%, switch off the Booster, turn on the Dimmer, modulating its opacity. Conversely, for Amounts higher than 200%, turn the Dimmer off, the Booster on and tune its opacity as you'd do usually. The math that regulates the Dimmer is easy:

> USM Amount % = 200 - 2*Dimmer opacity

Back at the time of post #4 writing I didn't know how to solve the problem, the solution came out experimenting later. So you may ask Davide, why don't you just add this piece of information alongside the original article? The answer lies in the original motivations of the whole analysis: to build a tool for Photoshop (via scripting). As a little researcher of image related topics, I should be fine, happy and done - the problem's fix has been expressed and it works. As a programmer, to implement such a stack of layers is far from being an elegant solution - and I'm the laziest around when it comes to write new code.

Subtraction, from a different point of view
-------------------------------------------

It's been playing around with blending modes math (which I'll be talking about in the next and last post of the series possibly - notes, errata, variations, etc) that I bumped into the obvious.

> a - b = a + (-b)

They call it the "_aha moment_". Translated in the image arithmetic that matters for us, it means that the following layers setups are just equivalent.

[![Images' arithmetic](http://localhost:8888/wp-content/uploads/2012/09/Arithmetic.jpg "Images' arithmetic")](http://localhost:8888/wp-content/uploads/2012/09/Arithmetic.jpg)

Which also means that post #4 about the Lab way and the first part of this very one are a just an interesting academic digression. I'm not going to implement it, relying to this (more efficient? less efficient? surely easier to code) setup:

[![Final setup](http://localhost:8888/wp-content/uploads/2012/09/Final.jpg "Final setup")](http://localhost:8888/wp-content/uploads/2012/09/Final.jpg)

In other terms (just in case you have't "_aha-ed_" yet) I can easily bypass the subtraction (forbidden in Lab) with an inversion (Invert adjustment layer - one or two depending on the cases) followed by an addition (Linear Dodge blending mode). A good news for Davide the scripter. If your eyes are as sharp as your post-produced pictures, you'd notice I'm still omitting one detail. The Grayscale/RGB/CMYK routine involves two other blending modes that are forbidden in Lab too, namely _Darken_ and _Lighten_. Their cousins _Darker Color_ and _Lighter Color_ will work there; the difference basically is that Darker/Lighten work on a channel basis, while Darker/Lighter Color work on the composite. You can visualize the difference here: \[caption id="attachment_1168" align="aligncenter" width="1787"\][![Darken / DarkerColor comparison](http://localhost:8888/wp-content/uploads/2012/09/DarkenDarkerColor_Comparison.jpg "Darken / DarkerColor comparison")](http://localhost:8888/wp-content/uploads/2012/09/DarkenDarkerColor_Comparison.jpg) From left to right: Original (base). Gradient (blend). Blend in Darken. Blend in Darker Color\[/caption\] Yet, if you resort to the old trick of the Advanced Blending Options, deselecting the a,b channels of the blend layer... and since I want the halos to be applied in Luminosity only, you get reasonably close to the equivalent of Darken/Lighten anyway. This post should end the series: given a problem, I've found and discussed in depth the solutions (even those that have been proved to be less efficient). Yet a closing post will follow, with errata, variations, experiments, and thorough extra examination of some of the topics that have been shown here - we need to decompress somehow, I guess :-). Thanks for having followed so far!

* * *

The [Decomposing Sharpening](http://localhost:8888/category/photoshop/decomposing-sharpening/ "Decomposing Sharpening") series has been written as a research project for my script [DoubleUSM: multi-radius sharpening](http://www.cs-extensions.com/products/doubleusm/ "DoubleUSM: multi-radius Sharpening"). You might be interested also in [Fixel Detailizer 2PS: multi-frequency contrast booster](http://www.cs-extensions.com/products/detailizer/ "Fixel Detailizer 2 PS").