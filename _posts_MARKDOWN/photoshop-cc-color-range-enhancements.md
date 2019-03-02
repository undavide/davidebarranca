---
title: " Photoshop CC Color Range Enhancements\t\t"
tags:
  - '14.1'
  - Color Range
  - Fuzziness
  - Photoshop CC
  - Range
url: 2195.html
id: 2195
category:
  - Photoshop
date: 2013-09-10 11:27:10
---

Latest Photoshop CC update (version 14.1) has introduced a good deal of new features. One of them is the refined Color Range Selection for Highlights, Midtones and Shadows, which now includes a proper Threshold called Range: let's see what is that all about.

Threshold
---------

As an Adjustment that is in Photoshop since early versions, Threshold hasn't gained much popularity so far, unless you're into Color Correction or forensic image processing. Have a look at what it does to a test image (click to enlarge).

![Threshold](http://localhost:8888/wp-content/uploads/2013/09/Threshold.jpg)

Basically it blows to White every pixel in the image which value that exceeds (but not include) the Threshold, and clips to Black everything else. You see it clearly in the third example (value: 128) where the image is split in two halves: from mid-grays to shadows they get to black, from mid-grays to highlights they get to white, so to speak. Another way to put it: Threshold makes your images 1bit (i.e. just Black or White, no grays allowed). Engineers have now built in the Photoshop CC Color Range Selection tool a Threshold in disguise - whose name has become **Range**.

Range and Fuzziness
-------------------

![Presets](http://localhost:8888/wp-content/uploads/2013/09/Presets.png)The Color Range Enhancement can be found if you pick from the drop down menu either the **Highlights**, **Midtones** or **Shadows** (see the screenshot), otherwise you won't see any new feature (if you're wondering, Skin Tones has been introduced in PS CS6). Mind you: we'll be dealing with B/W images because that's how Photoshop "thinks" about selections: what is going to get **white means selected**, what is going to get **black means not selected**. All the grays in the middle are, depending on their value, **halfway between selected and not selected** \- that is: the level of gray is tied with the opacity of the selection. So an Alpha Channel is just a way to visualize complex selections. The two sliders we'll be playing with are the **Fuzziness** and **Range**; the latter as I said is just a Threshold (plenty of screenshot below don't worry) while the former controls how much the actual selection adhere to the numbers you set in the Range. So to speak, zero Fuzziness means harsh, Threshold like, just-black-or-white selections; higher Fuzziness means smooth selections, that include more grays. Let's see.

### Highlights

I've run Select - Color Range, then chosen Highlights. I've set Fuzziness to zero: restricting to one variable at the time helps understanding what happens more clearly - playing with the Range only you get:

[![Highlights](http://localhost:8888/wp-content/uploads/2013/09/Highlights.png)](http://localhost:8888/wp-content/uploads/2013/09/Highlights.png)

As you see, it's very much like Threshold! With the apparent difference that Range at 255 still selects the pure whites (the pixels which value is 255), while actual Threshold doesn't include it. What happens if you raise the Fuzziness to, say, 50%?

[![Highlights_Fuzzy](http://localhost:8888/wp-content/uploads/2013/09/Highlights_Fuzzy.png)](http://localhost:8888/wp-content/uploads/2013/09/Highlights_Fuzzy.png)

As you see the selection "grows" through the blacks and gains more gray levels there - that is to say, you have a smoother selection that includes partially selected pixels.

### Shadows

Similarly, if you pick the Shadows from the drop-down menu and keep the Fuzziness at zero:

[![Shadows](http://localhost:8888/wp-content/uploads/2013/09/Shadows.png)](http://localhost:8888/wp-content/uploads/2013/09/Shadows.png)

Again you see the Range as a Threshold in disguise - with the difference that (compared to the actual Threshold) the resulting image is inverted, and you're allowed to input 0 as a value (while Threshold sticks with 1). Basically selecting Shadows is just the same as inverting the image first, then selecting the Highlights - hopefully this makes sense to you. Fuzziness does what you would expect:

[![Shadows_Fuzzy](http://localhost:8888/wp-content/uploads/2013/09/Shadows_Fuzzy.png)](http://localhost:8888/wp-content/uploads/2013/09/Shadows_Fuzzy.png)

### Midtones

Things get more interesting when it comes to the Midtones. There, you can simultaneously play with two Range sliders, and combine (so to speak) two Threshold at the same time, that restrict the _concept_ of "midtone" that you need:

[![Midtones](http://localhost:8888/wp-content/uploads/2013/09/Midtones.png)](http://localhost:8888/wp-content/uploads/2013/09/Midtones.png)

You can move the two sliders, shifting them to the left (the shadows) or to the right (the highlights), or make them more apart in order to select a bigger part of the grays. Fuzziness is handy here too:

[![Midtones_Fuzzy](http://localhost:8888/wp-content/uploads/2013/09/Midtones_Fuzzy.png)](http://localhost:8888/wp-content/uploads/2013/09/Midtones_Fuzzy.png)

As it happened before, you can refine the selection combining Fuzziness and Range, to get exactly where you want to. The higher the Fuzziness, the more the selection dilates and includes gray levels.

Curves
------

Mind you, the idea of Midtone selection isn't new - Photoshop engineers have been doing a nice enhancement to the Color Range dialog, which surely helps those unwilling to do it the old way. That is to say using Curves:

[![Curves](http://localhost:8888/wp-content/uploads/2013/09/Curves.png)](http://localhost:8888/wp-content/uploads/2013/09/Curves.png)

[Davide Barranca](https://plus.google.com/111682277377408685163?rel=author)