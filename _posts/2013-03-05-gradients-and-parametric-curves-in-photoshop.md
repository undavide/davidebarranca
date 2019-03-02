---
id: 1788
title: Gradients and Parametric Curves in Photoshop
date: 2013-03-05T23:37:21+01:00
author: Davide Barranca
excerpt: "It's fascinating the complexity of designs you can get just mixing Gradients with parametric Curves built via scripting in Photoshop - that can be used for textures, bump maps or displacement maps, abstract designs or animations."
layout: post
guid: http://www.davidebarranca.com/?p=1788
permalink: /2013/03/gradients-and-parametric-curves-in-photoshop/
enclosure:
  - |
    /wp-content/uploads/2013/03/Gradient_madness.mp4
    3723168
    video/mp4

standard_seo_post_level_layout:
  - ""
standard_link_url_field:
  - ""
standard_seo_post_meta_description:
  - You can get Bump, Depth or Displacement Maps, Textures, abstract shapes just mixing Gradients and scripting parametric Curves in Photoshop.
image: /wp-content/uploads/2013/03/CurvesGradients.jpg
category:
  - Extensions and Scripts
  - Parametric Curves
  - Photoshop
tags:
  - 3D
  - bump map
  - Curves
  - depth map
  - displacement
  - Gradient
---
<div class="pf-content">
  <p>
    <meta itemprop="image" content="/wp-content/uploads/2013/03/CurvesGradients.jpg" />

    <meta itemprop="thumbnailUrl" content="/wp-content/uploads/2013/03/CurvesGradients-150x150.jpg" />
    I&#8217;m not a FX kind of guy, but
    <span itemprop="description">as a developer I&#8217;m fascinated by the astonishing complex results you can get with simple <span itemprop="name">Gradients and a Parametric Curves in Photoshop</span>. These designs can be used as building blocks for all kind of creative needs &#8211; whether bump maps, depth maps or displacement maps, abstract design, patterns, etc.</span>
  </p>

  <p>
    <!--more-->
  </p>

  <h2>
    Parametric Curves
  </h2>

  <p>
    I&#8217;ve started playing with the idea of <strong>drawing mathematically defined Curves via scripting</strong>. That is, not you adding/dragging points on a Curves window; but let ExtendScript code fetch user inputted formulas and draw the Curves accordingly. Let&#8217;s say it could be a prototype test for a future <span itemprop="mentions" itemscope itemtype="http://schema.org/SoftwareApplication"><span itemprop="name">Photoshop</span></span> panel.
  </p>

  <p>
    So I&#8217;ve coded a simple <em>script</em> and, after a good dose of trial and error, I came up with some pretty interesting shapes:
  </p>

  <div id="attachment_1792" style="width: 552px" class="wp-caption aligncenter">
    <img aria-describedby="caption-attachment-1792" class="size-full wp-image-1792" alt="Photoshop Curves" src="/wp-content/uploads/2013/03/curves.gif" width="542" height="542" />

    <p id="caption-attachment-1792" class="wp-caption-text">
      Curves (as Adjustment Layers) I&#8217;ve drawn via <strong>scripting</strong>.
    </p>
  </div>

  <p>
    Some of them may be replicable by hand (with a good dose of patience), others can be built via scripting only. These kind of curves are normally available in <span itemprop="mentions" itemscope itemtype="http://schema.org/SoftwareApplication"><a title="Filter Forge Photoshop plugin" href="http://www.filterforge.com/?affiliateid=200188983" target="_blank" itemprop="url"><span itemprop="name">Filter Forge</span></a>, a <span itemprop="description"><span itemprop="applicationSuite">Photoshop</span> plugin for texture creation</span></span> I&#8217;ve <a title="Introducing Filter Forge" href="/2013/02/introducing-filter-forge-for-photoshop/" target="_blank">enthusiastically written about here</a> &#8211; but are out of reach if you don&#8217;t own it.
  </p>

  <h2>
    Gradients
  </h2>

  <p>
    The above Curves have no use on real imaging but forensic and/or scientific ones (maybe &#8211; if there&#8217;s a forensic expert out there I&#8217;m all ears! The comment section is open for you), but if you put them above a <strong>Gradient adjustment layer</strong>, the fun begins. I&#8217;ll be using 16bit files (in order to avoid posterization, some of these transformations are pretty harsh), with all of the available Gradient shapes:
  </p>

  <div id="attachment_1791" style="width: 580px" class="wp-caption aligncenter">
    <img aria-describedby="caption-attachment-1791" class="size-full wp-image-1791" alt="Photoshop Gradients" src="/wp-content/uploads/2013/03/gradients.png" width="570" height="114" srcset="/wp-content/uploads/2013/03/gradients.png 570w, /wp-content/uploads/2013/03/gradients-150x30.png 150w, /wp-content/uploads/2013/03/gradients-300x60.png 300w" sizes="(max-width: 570px) 100vw, 570px" />

    <p id="caption-attachment-1791" class="wp-caption-text">
      Available gradients in Photoshop: <strong>Linear</strong>, <strong>Reflected</strong>, <strong>Radial</strong>, <strong>Diamond</strong>, <strong>Angle</strong>
    </p>
  </div>

  <h2>
    Gradient + Curves
  </h2>

  <p>
    Let&#8217;s start experimenting with a <strong>mix of Curves and Gradients</strong>. Be aware that you can mix multiple <span itemprop="about">Curves</span> and <span itemprop="about">Gradients</span>, and play with Blending modes too!
  </p>

  <h3>
    Linear
  </h3>

  <p>
    <img class="aligncenter size-full wp-image-1794" alt="Linear gradients and scripted curves" src="/wp-content/uploads/2013/03/Linear.jpg" width="570" height="140" srcset="/wp-content/uploads/2013/03/Linear.jpg 570w, /wp-content/uploads/2013/03/Linear-150x36.jpg 150w, /wp-content/uploads/2013/03/Linear-300x73.jpg 300w" sizes="(max-width: 570px) 100vw, 570px" />
  </p>

  <h3>
    Radial
  </h3>

  <p>
    <img class="aligncenter size-full wp-image-1796" alt="Radial gradients and scripted curves" src="/wp-content/uploads/2013/03/Radial.jpg" width="570" height="140" srcset="/wp-content/uploads/2013/03/Radial.jpg 570w, /wp-content/uploads/2013/03/Radial-150x36.jpg 150w, /wp-content/uploads/2013/03/Radial-300x73.jpg 300w" sizes="(max-width: 570px) 100vw, 570px" />
  </p>

  <h3>
    Angle
  </h3>

  <p>
    <img class="aligncenter size-full wp-image-1798" alt="Angle gradients and scripted curves" src="/wp-content/uploads/2013/03/Angle.jpg" width="570" height="140" srcset="/wp-content/uploads/2013/03/Angle.jpg 570w, /wp-content/uploads/2013/03/Angle-150x36.jpg 150w, /wp-content/uploads/2013/03/Angle-300x73.jpg 300w" sizes="(max-width: 570px) 100vw, 570px" />
  </p>

  <h3>
    Reflected
  </h3>

  <p>
    <img class="aligncenter size-full wp-image-1799" alt="Reflected gradients and scripted curves" src="/wp-content/uploads/2013/03/Reflected.jpg" width="570" height="140" srcset="/wp-content/uploads/2013/03/Reflected.jpg 570w, /wp-content/uploads/2013/03/Reflected-150x36.jpg 150w, /wp-content/uploads/2013/03/Reflected-300x73.jpg 300w" sizes="(max-width: 570px) 100vw, 570px" />
  </p>

  <h3>
    Diamond
  </h3>

  <p>
    <img class="aligncenter size-full wp-image-1800" alt="Diamond gradients and scripted curves" src="/wp-content/uploads/2013/03/Diamond.jpg" width="570" height="140" srcset="/wp-content/uploads/2013/03/Diamond.jpg 570w, /wp-content/uploads/2013/03/Diamond-150x36.jpg 150w, /wp-content/uploads/2013/03/Diamond-300x73.jpg 300w" sizes="(max-width: 570px) 100vw, 570px" />
  </p>

  <h2>
    Combined gradients
  </h2>

  <p>
    If you start adding <strong>multiple Gradients and Curves on top of each other</strong>, you may end up with some really interesting results:
  </p>

  <p>
    <img class="aligncenter size-full wp-image-1802" alt="Mixed gradients and scripted curves" src="/wp-content/uploads/2013/03/Mixed_01.jpg" width="570" height="570" srcset="/wp-content/uploads/2013/03/Mixed_01.jpg 570w, /wp-content/uploads/2013/03/Mixed_01-150x150.jpg 150w, /wp-content/uploads/2013/03/Mixed_01-300x300.jpg 300w" sizes="(max-width: 570px) 100vw, 570px" />
  </p>

  <p>
    It gets addictive when you start must I confess&#8230;
  </p>

  <p>
    <img class="aligncenter size-full wp-image-1803" alt="Mixed gradients and scripted curves" src="/wp-content/uploads/2013/03/Mixed_02.jpg" width="570" height="570" itemprop="image" srcset="/wp-content/uploads/2013/03/Mixed_02.jpg 570w, /wp-content/uploads/2013/03/Mixed_02-150x150.jpg 150w, /wp-content/uploads/2013/03/Mixed_02-300x300.jpg 300w" sizes="(max-width: 570px) 100vw, 570px" />
  </p>

  <h2>
    What for?
  </h2>

  <p>
    I&#8217;m not the creative one, I&#8217;m just owning the hardware store providing you with the tools 🙂
  </p>

  <p>
    That said, as the only untalented designer available on duty at the moment, I can show you just few examples to tease your creative skills. These are just building blocks, combining them the possibilities are endless.
  </p>

  <h3>
    Patterns
  </h3>

  <p>
    This is a very simple one, textured and ready to be tiled.
  </p>

  <div id="attachment_1810" style="width: 1010px" class="wp-caption aligncenter">
    <a href="/wp-content/uploads/2013/03/Texture_example.jpg" target="_blank"><img aria-describedby="caption-attachment-1810" class="size-full wp-image-1810" alt="Texture Example" src="/wp-content/uploads/2013/03/Texture_example.jpg" width="1000" height="1000" srcset="/wp-content/uploads/2013/03/Texture_example.jpg 1000w, /wp-content/uploads/2013/03/Texture_example-150x150.jpg 150w, /wp-content/uploads/2013/03/Texture_example-300x300.jpg 300w" sizes="(max-width: 1000px) 100vw, 1000px" /></a>

    <p id="caption-attachment-1810" class="wp-caption-text">
      <strong>A</strong>: Gradient + Curves. <strong>B</strong>: Perlin noise (Clouds) + Motion Blur. <strong>C</strong>: Perlin noise + Gaussian Noise. <strong>D</strong>: Combined result.
    </p>
  </div>

  <h3>
    Displacement maps
  </h3>

  <p>
    Pretty hidden in the menu, there&#8217;s the <strong>Distort &#8211; Displace filter</strong>. It requires you to open a second image (the <em><span itemprop="mentions">Displacement Map</span></em>), to be used to distort the first one according to the parameters you&#8217;ve set.
  </p>

  <div id="attachment_1813" style="width: 1010px" class="wp-caption aligncenter">
    <a href="/wp-content/uploads/2013/03/Displacement_example.jpg" target="_blank"><img aria-describedby="caption-attachment-1813" class="size-full wp-image-1813" alt="Displacement example" src="/wp-content/uploads/2013/03/Displacement_example.jpg" width="1000" height="1000" srcset="/wp-content/uploads/2013/03/Displacement_example.jpg 1000w, /wp-content/uploads/2013/03/Displacement_example-150x150.jpg 150w, /wp-content/uploads/2013/03/Displacement_example-300x300.jpg 300w" sizes="(max-width: 1000px) 100vw, 1000px" /></a>

    <p id="caption-attachment-1813" class="wp-caption-text">
      <strong>A</strong>: Gradient + Curves. <strong>B</strong>: Texture. <strong>C</strong>: Texture displaced. <strong>D</strong>: Combined result.
    </p>
  </div>

  <h3>
    3D <span itemprop="mentions">Depth Maps</span> and <span itemprop="mentions">Bump Maps</span>
  </h3>

  <p>
    Believe me, I rarely open any 3D panel in Photoshop because I&#8217;m a 2D chauvinist, so this one is really a basic example that may make the actual experts laugh, but it&#8217;s just to give you the idea of using gradient as Depth Maps.
  </p>

  <div id="attachment_1818" style="width: 1010px" class="wp-caption aligncenter">
    <a href="/wp-content/uploads/2013/03/Depth_map.jpg" target="_blank"><img aria-describedby="caption-attachment-1818" class="size-full wp-image-1818" alt="Depth map example" src="/wp-content/uploads/2013/03/Depth_map.jpg" width="1000" height="500" itemprop="image" srcset="/wp-content/uploads/2013/03/Depth_map.jpg 1000w, /wp-content/uploads/2013/03/Depth_map-150x75.jpg 150w, /wp-content/uploads/2013/03/Depth_map-300x150.jpg 300w" sizes="(max-width: 1000px) 100vw, 1000px" /></a>

    <p id="caption-attachment-1818" class="wp-caption-text">
      From the Photoshop CS6 menu<strong> 3D &#8211; New Mesh from Layer &#8211; Depth Map to &#8211; Plane</strong>
    </p>
  </div>

  <div id="attachment_1821" style="width: 1010px" class="wp-caption aligncenter">
    <a href="/wp-content/uploads/2013/03/Displacement_example_02.jpg"><img aria-describedby="caption-attachment-1821" class="size-full wp-image-1821" alt="Displacement example" src="/wp-content/uploads/2013/03/Displacement_example_02.jpg" width="1000" height="500" srcset="/wp-content/uploads/2013/03/Displacement_example_02.jpg 1000w, /wp-content/uploads/2013/03/Displacement_example_02-150x75.jpg 150w, /wp-content/uploads/2013/03/Displacement_example_02-300x150.jpg 300w" sizes="(max-width: 1000px) 100vw, 1000px" /></a>

    <p id="caption-attachment-1821" class="wp-caption-text">
      Another couple of Depth Map examples
    </p>
  </div>

  <p>
    Similarly (but with a subtler effect) you can apply those gradients as actual Bump Maps in 3D materials.
  </p>

  <h3>
    Animations
  </h3>

  <p>
    All kind of weird stuff&#8230; this is just an example of what happens if you shift Gradients (on selected channels only) when one or more parametric Curves are applied. <strong>Click on the image to launch the video</strong>.
  </p>

  <div id="attachment_1826" style="width: 570px" class="wp-caption aligncenter">
    <a href="/wp-content/uploads/2013/03/Gradient_madness.mp4" target="_blank"><img aria-describedby="caption-attachment-1826" class="size-full wp-image-1826 " alt="Gradient Madness" src="/wp-content/uploads/2013/03/Movie.jpg" width="560" height="374" itemprop="image" srcset="/wp-content/uploads/2013/03/Movie.jpg 560w, /wp-content/uploads/2013/03/Movie-150x100.jpg 150w, /wp-content/uploads/2013/03/Movie-300x200.jpg 300w" sizes="(max-width: 560px) 100vw, 560px" /></a>

    <p id="caption-attachment-1826" class="wp-caption-text">
      Few examples &#8211; scripting the gradient interaction would lead to even more interesting results
    </p>
  </div>

  <h2>
    Parametric Curves on Exchange!
  </h2>

  <p>
    <a href="https://www.adobeexchange.com" target="_blank"><img class="alignright size-full wp-image-1877" alt="Adobe Exchange" src="/wp-content/uploads/2013/03/Exchange.png" width="128" height="128" /></a>Parametric Curves is available as a free script for <strong><span itemprop="applicationSuite">Photoshop CS6</span> (Mac/Win)</strong> through <a title="Adobe Exchange panel" href="http://www.adobeexchange.com" target="_blank">Adobe Exchange</a> &#8211; the new in-app, app-store panel made by Adobe itself. Download and install Exchange if you don&#8217;t already have it, then browse the <strong>Free</strong> extensions and look for Parametric Curves there. After the installation, please find it in the <strong>Filter</strong> menu. More <a href="/2013/03/parametric-curves-script-for-photoshop-user-guide/" title="Parametric Curves free script User Guide" target="_blank">info about the panel here</a>.<br /> <a href="/2013/03/parametric-curves-script-for-photoshop-user-guide/" title="Parametric Curves free script User Guide" target="_blank"><img class="aligncenter size-full wp-image-1879" alt="Parametric Curves Math reference" src="/wp-content/uploads/2013/03/ParametricCurves_interface_Math.jpg" width="884" height="512" itemprop="screenshot" srcset="/wp-content/uploads/2013/03/ParametricCurves_interface_Math.jpg 884w, /wp-content/uploads/2013/03/ParametricCurves_interface_Math-150x86.jpg 150w, /wp-content/uploads/2013/03/ParametricCurves_interface_Math-300x173.jpg 300w" sizes="(max-width: 884px) 100vw, 884px" /></a>
  </p>
</div>
