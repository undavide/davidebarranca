---
title: " Parametric Curves free script User Guide\t\t"
tags:
  - Adobe Exchange
  - Gradient
  - Parametric Curves
  - script
url: 1868.html
id: 1868
comments: false
category:
  - Coding
  - Extensions and Scripts
  - Parametric Curves
  - Photoshop
date: 2013-03-26 21:07:18
---

Parametric Curves is a free Photoshop script that lets you plot mathematically defined (Javascript) Curves Adjustment layers. If you wonder what they could be for, please read my previous post called ["Gradients and Parametric Curves in Photoshop"](http://localhost:8888/2013/03/gradients-and-parametric-curves-in-photoshop/ "Gradients and Parametric Curves in Photoshop"). Here I'll show you the script interface and I'll walk you through the creation of some interesting Curves, that will be the building blocks of advanced creative manipulations.

Where do I get it?
------------------

[![Adobe Exchange](http://localhost:8888/wp-content/uploads/2013/03/Exchange.png)](https://www.adobeexchange.com)Parametric Curves is available for **Photoshop CS6 (Mac/Win)** through [Adobe Exchange](http://www.adobeexchange.com "Adobe Exchange panel") \- the new in-app, app-store panel made by Adobe itself. Download and install Exchange if you don't already have it, then browse the **Free** extensions and look for Parametric Curves there. Install Parametric Curves, and find it in the Photoshop **Filter menu**.

How does it work?
-----------------

When you launch it, a Curves Adjustment layer is build behind the scene and the Parametric Curves window pops up: you can either pick up a ready made function from the Presets or write your own Javascript in the text area. Click Try to preview the curve, Apply to eventually confirm.

Interface
---------

![Parametric Curves interface](http://localhost:8888/wp-content/uploads/2013/03/ParametricCurves_interface.jpg)

### 1 - Curve

This is where the curve is graphed. By default the orientation is **Light** \- that is to say Black (0) is bottom-left, White (255) is top-right, which is the default of _RGB_ and _Lab_ modes; you can switch to **Pigment / Ink %** (the default for _Grayscale_ and _CMYK,_ White is bottom-left and Black is top-right) with the Display option drop-down menu.

### 2 - Presets

![Parametric Curves Presets](http://localhost:8888/wp-content/uploads/2013/03/ParametricCurves_presets.jpg)There are some ready-made Curves you can try in the drop-down menu "Select a preset...". When you choose one, the function is pasted in the text area (3) and the curve displayed (1). You're allowed to save your own Preset clicking the **New** button (you'll be asked to enter a label for it). Add as many presets as you want - you can always delete the one you don't like (but the Default ones) with the **Remove** button, or **Reset** them to the default group.

### 3 - Functions

In this text area you can write your own functions, or modify existing ones. Once you're done, click the **Try** button in order to test it either as a graph in the Curve panel and as an actual Curves Adjustment Layer in the file. 4If you're satisfied with the result, click the **Apply** button, otherwise you can tweak the function of **Cancel** back to Photoshop.

### 4 - Information

The **Math Ref.** button (which can be toggled on/off) shows a handy reference for the Math. Javascript Object (see below for more information): ![Parametric Curves Math reference](http://localhost:8888/wp-content/uploads/2013/03/ParametricCurves_interface_Math.jpg) The **Tutorial** button points your browser to the [Parametric Curves category page](http://localhost:8888/category/photoshop/extensions-and-scripts/parametric-curves/ "Parametric Curves category") of this website, while the **?** button shows copyright informations.

Functions Tutorial
------------------

This will help you starting with your own functions. Few basic information:

*   The **full range is 0-255**, where Black = 0 and White = 255 (this is independent on the Display option).
*   Functions are in the form: **y = f(x)** and you're supposed to write the f(x) part only: so the default Curve (a straight line 45°) is just "x".
*   Functions value is automatically **rounded**.
*   **Javascript** syntax is allowed.
*   More complete Javascript reference is [here](http://www.w3schools.com/jsref/jsref_obj_math.asp "Javascript Math Object Reference").

I'm not very good with geometry so let's start simple: \[caption id="attachment_1885" align="aligncenter" width="568"\]![Functions 1](http://localhost:8888/wp-content/uploads/2013/03/functions_01.png) x+127  
x-127  
2*x  
x/2\[/caption\] If you need **constants**, they must be written as in this example: `Math.PI * x + Math.LN2`. You can write **functions** as well of course: \[caption id="attachment_1886" align="aligncenter" width="568"\]![Functions 2](http://localhost:8888/wp-content/uploads/2013/03/functions_02.png) Math.pow(x,2) / 70  
40*Math.log(x)  
255*Math.random()  
x+30*Math.sin(x/5)\[/caption\] You'd better off **normalizing the output to fit the 0-255 range**, as in the following examples: \[caption id="attachment_1890" align="aligncenter" width="568"\]![Functions 3](http://localhost:8888/wp-content/uploads/2013/03/functions_03.png) Math.sin(x)  
127*Math.sin(x)  
127*Math.sin(x) + 127  
127\*Math.sin(2\*Math.PI*x/255) + 127\[/caption\] You can also use:

*   The so-called **ternary operator**, in the form of `(condition) ? (true) : (false)`.
*   The **modulo operator** %, for instance `x%10` (which gives the rest of the division by 10 and it's useful to repeat patterns)
*   **Logic operators** `&&` (and) `||` (or), besides `==` (equal to) and `!=` (not equal to), `<` and `>`
*   Nest them when needed, but be careful not to write bad code!

\[caption id="attachment_1896" align="aligncenter" width="568"\]![Functions 4](http://localhost:8888/wp-content/uploads/2013/03/functions_04.png) (x>128) ? x : 255 - x  
4 * x%256  
(((x>0) && (x<64)) || ((x>128) && (x<191))) ? x : 255-x  
x / a\[/caption\]

Anything a bit more inspirational on the Creative side?
-------------------------------------------------------

It's a bottomless pit as soon as you start experimenting - please [see this post full of examples](http://localhost:8888/2013/03/gradients-and-parametric-curves-in-photoshop/ "Gradients and Parametric Curves in Photoshop").

[![Parametric Curves Funky Stuff](http://localhost:8888/wp-content/uploads/2013/03/ParametricCurvesDesign.jpg)](http://localhost:8888/2013/03/gradients-and-parametric-curves-in-photoshop/)

Go get it!
----------

Find it on [Adobe Exchange](http://www.adobeexchange.com "Adobe Exchange panel"), it's free! And if you think that after all it's a nice piece of software, please leave a review in Exchange.