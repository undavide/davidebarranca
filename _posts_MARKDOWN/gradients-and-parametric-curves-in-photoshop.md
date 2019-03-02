---
title: " Gradients and Parametric Curves in Photoshop\t\t"
tags:
  - 3D
  - bump map
  - Curves
  - depth map
  - displacement
  - Gradient
url: 1788.html
id: 1788
comments: false
category:
  - Extensions and Scripts
  - Parametric Curves
  - Photoshop
date: 2013-03-05 23:37:21
---

I'm not a FX kind of guy, but as a developer I'm fascinated by the astonishing complex results you can get with simple Gradients and a Parametric Curves in Photoshop. These designs can be used as building blocks for all kind of creative needs - whether bump maps, depth maps or displacement maps, abstract design, patterns, etc. 

Parametric Curves
-----------------

I've started playing with the idea of **drawing mathematically defined Curves via scripting**. That is, not you adding/dragging points on a Curves window; but let ExtendScript code fetch user inputted formulas and draw the Curves accordingly. Let's say it could be a prototype test for a future Photoshop panel. So I've coded a simple _script_ and, after a good dose of trial and error, I came up with some pretty interesting shapes: \[caption id="attachment_1792" align="aligncenter" width="542"\]![Photoshop Curves](http://localhost:8888/wp-content/uploads/2013/03/curves.gif) Curves (as Adjustment Layers) I've drawn via **scripting**.\[/caption\] Some of them may be replicable by hand (with a good dose of patience), others can be built via scripting only. These kind of curves are normally available in [Filter Forge](http://www.filterforge.com/?affiliateid=200188983 "Filter Forge Photoshop plugin"), a Photoshop plugin for texture creation I've [enthusiastically written about here](http://localhost:8888/2013/02/introducing-filter-forge-for-photoshop/ "Introducing Filter Forge") \- but are out of reach if you don't own it.

Gradients
---------

The above Curves have no use on real imaging but forensic and/or scientific ones (maybe - if there's a forensic expert out there I'm all ears! The comment section is open for you), but if you put them above a **Gradient adjustment layer**, the fun begins. I'll be using 16bit files (in order to avoid posterization, some of these transformations are pretty harsh), with all of the available Gradient shapes: \[caption id="attachment_1791" align="aligncenter" width="570"\]![Photoshop Gradients](http://localhost:8888/wp-content/uploads/2013/03/gradients.png) Available gradients in Photoshop: **Linear**, **Reflected**, **Radial**, **Diamond**, **Angle**\[/caption\]

Gradient + Curves
-----------------

Let's start experimenting with a **mix of Curves and Gradients**. Be aware that you can mix multiple Curves and Gradients, and play with Blending modes too!

### Linear

![Linear gradients and scripted curves](http://localhost:8888/wp-content/uploads/2013/03/Linear.jpg)

### Radial

![Radial gradients and scripted curves](http://localhost:8888/wp-content/uploads/2013/03/Radial.jpg)

### Angle

![Angle gradients and scripted curves](http://localhost:8888/wp-content/uploads/2013/03/Angle.jpg)

### Reflected

![Reflected gradients and scripted curves](http://localhost:8888/wp-content/uploads/2013/03/Reflected.jpg)

### Diamond

![Diamond gradients and scripted curves](http://localhost:8888/wp-content/uploads/2013/03/Diamond.jpg)

Combined gradients
------------------

If you start adding **multiple Gradients and Curves on top of each other**, you may end up with some really interesting results: ![Mixed gradients and scripted curves](http://localhost:8888/wp-content/uploads/2013/03/Mixed_01.jpg) It gets addictive when you start must I confess... ![Mixed gradients and scripted curves](http://localhost:8888/wp-content/uploads/2013/03/Mixed_02.jpg)

What for?
---------

I'm not the creative one, I'm just owning the hardware store providing you with the tools :-) That said, as the only untalented designer available on duty at the moment, I can show you just few examples to tease your creative skills. These are just building blocks, combining them the possibilities are endless.

### Patterns

This is a very simple one, textured and ready to be tiled. \[caption id="attachment_1810" align="aligncenter" width="1000"\][![Texture Example](http://localhost:8888/wp-content/uploads/2013/03/Texture_example.jpg)](http://localhost:8888/wp-content/uploads/2013/03/Texture_example.jpg) **A**: Gradient + Curves. **B**: Perlin noise (Clouds) + Motion Blur. **C**: Perlin noise + Gaussian Noise. **D**: Combined result.\[/caption\]

### Displacement maps

Pretty hidden in the menu, there's the **Distort - Displace filter**. It requires you to open a second image (the _Displacement Map_), to be used to distort the first one according to the parameters you've set. \[caption id="attachment_1813" align="aligncenter" width="1000"\][![Displacement example](http://localhost:8888/wp-content/uploads/2013/03/Displacement_example.jpg)](http://localhost:8888/wp-content/uploads/2013/03/Displacement_example.jpg) **A**: Gradient + Curves. **B**: Texture. **C**: Texture displaced. **D**: Combined result.\[/caption\]

### 3D Depth Maps and Bump Maps

Believe me, I rarely open any 3D panel in Photoshop because I'm a 2D chauvinist, so this one is really a basic example that may make the actual experts laugh, but it's just to give you the idea of using gradient as Depth Maps. \[caption id="attachment_1818" align="aligncenter" width="1000"\][![Depth map example](http://localhost:8888/wp-content/uploads/2013/03/Depth_map.jpg)](http://localhost:8888/wp-content/uploads/2013/03/Depth_map.jpg) From the Photoshop CS6 menu **3D - New Mesh from Layer - Depth Map to - Plane**\[/caption\] \[caption id="attachment_1821" align="aligncenter" width="1000"\][![Displacement example](http://localhost:8888/wp-content/uploads/2013/03/Displacement_example_02.jpg)](http://localhost:8888/wp-content/uploads/2013/03/Displacement_example_02.jpg) Another couple of Depth Map examples\[/caption\] Similarly (but with a subtler effect) you can apply those gradients as actual Bump Maps in 3D materials.

### Animations

All kind of weird stuff... this is just an example of what happens if you shift Gradients (on selected channels only) when one or more parametric Curves are applied. **Click on the image to launch the video**. \[caption id="attachment_1826" align="aligncenter" width="560"\][![Gradient Madness](http://localhost:8888/wp-content/uploads/2013/03/Movie.jpg)](http://localhost:8888/wp-content/uploads/2013/03/Gradient_madness.mp4) Few examples - scripting the gradient interaction would lead to even more interesting results\[/caption\]

Parametric Curves on Exchange!
------------------------------

[![Adobe Exchange](http://localhost:8888/wp-content/uploads/2013/03/Exchange.png)](https://www.adobeexchange.com)Parametric Curves is available as a free script for **Photoshop CS6 (Mac/Win)** through [Adobe Exchange](http://www.adobeexchange.com "Adobe Exchange panel") \- the new in-app, app-store panel made by Adobe itself. Download and install Exchange if you don't already have it, then browse the **Free** extensions and look for Parametric Curves there. After the installation, please find it in the **Filter** menu. More [info about the panel here](http://localhost:8888/2013/03/parametric-curves-script-for-photoshop-user-guide/ "Parametric Curves free script User Guide"). [![Parametric Curves Math reference](http://localhost:8888/wp-content/uploads/2013/03/ParametricCurves_interface_Math.jpg)](http://localhost:8888/2013/03/parametric-curves-script-for-photoshop-user-guide/ "Parametric Curves free script User Guide")