---
id: 1868
title: Parametric Curves free script User Guide
date: 2013-03-26T21:07:18+01:00
author: Davide Barranca
excerpt: "Parametric Curves is a free Photoshop script that lets you plot mathematically defined (Javascript) Curves Adjustment layers. Here I'll show you the script interface and I'll walk you through the creation of some interesting Curves, that will be the building blocks of advanced creative manipulations."
layout: post
guid: http://www.davidebarranca.com/?p=1868
permalink: /2013/03/parametric-curves-script-for-photoshop-user-guide/
standard_seo_post_level_layout:
  - ""
standard_link_url_field:
  - ""
standard_seo_post_meta_description:
  - Parametric Curves is a free Photoshop CS6 script that lets you plot mathematically defined (Javascript) Curves Adjustment layers.
image: /wp-content/uploads/2013/03/ParametricCurves_Featured.jpg
categories:
  - Coding
  - Extensions and Scripts
  - Parametric Curves
  - Photoshop
tags:
  - Adobe Exchange
  - Gradient
  - Parametric Curves
  - script
---
<div class="pf-content">
  <div itemprop="about" itemscope itemtype="http://schema.org/SoftwareApplication">
    <span itemprop="description"><span itemprop="name">Parametric Curves</span> is a free <span itemprop="applicationCategory">Photoshop script</span> that lets you plot mathematically defined (Javascript) Curves Adjustment layers.</span> If you wonder what they could be for, please read my previous post called <a title="Gradients and Parametric Curves in Photoshop" href="/2013/03/gradients-and-parametric-curves-in-photoshop/" target="_blank">&#8220;Gradients and Parametric Curves in Photoshop&#8221;</a>.</p>

    <p>
      Here I&#8217;ll show you the script interface and I&#8217;ll walk you through the creation of some interesting Curves, that will be the building blocks of advanced creative manipulations.
    </p>

    <p>
      <!--more-->
    </p>

    <h2>
      Where do I get it?
    </h2>

    <p>
      <a href="https://www.adobeexchange.com" target="_blank"><img class="alignright size-full wp-image-1877" alt="Adobe Exchange" src="/wp-content/uploads/2013/03/Exchange.png" width="128" height="128" /></a>Parametric Curves is available for <strong><span itemprop="applicationSuite">Photoshop CS6</span> (Mac/Win)</strong> through <a title="Adobe Exchange panel" href="http://www.adobeexchange.com" target="_blank">Adobe Exchange</a> &#8211; the new in-app, app-store panel made by Adobe itself. Download and install Exchange if you don&#8217;t already have it, then browse the <strong>Free</strong> extensions and look for Parametric Curves there.
    </p>

    <p>
      Install Parametric Curves, and find it in the Photoshop <strong>Filter menu</strong>.
    </p>

    <h2>
      How does it work?
    </h2>

    <p>
      When you launch it, a Curves Adjustment layer is build behind the scene and the Parametric Curves window pops up: you can either pick up a ready made function from the Presets or write your own Javascript in the text area. Click Try to preview the curve, Apply to eventually confirm.
    </p>

    <h2>
      Interface
    </h2>

    <p>
      <img class="aligncenter size-full wp-image-1875" alt="Parametric Curves interface" src="/wp-content/uploads/2013/03/ParametricCurves_interface.jpg" width="685" height="512" itemprop="screenshot" srcset="/wp-content/uploads/2013/03/ParametricCurves_interface.jpg 685w, /wp-content/uploads/2013/03/ParametricCurves_interface-150x112.jpg 150w, /wp-content/uploads/2013/03/ParametricCurves_interface-300x224.jpg 300w" sizes="(max-width: 685px) 100vw, 685px" />
    </p>

    <h3>
      1 &#8211; Curve
    </h3>

    <p>
      This is where the curve is graphed. By default the orientation is <strong>Light</strong> &#8211; that is to say Black (0) is bottom-left, White (255) is top-right, which is the default of <em>RGB</em> and <em>Lab</em> modes; you can switch to <strong>Pigment / Ink %</strong> (the default for <em>Grayscale</em> and <em>CMYK,</em> White is bottom-left and Black is top-right) with the Display option drop-down menu.
    </p>

    <h3>
      2 &#8211; Presets
    </h3>

    <p>
      <img class="alignleft size-full wp-image-1872" alt="Parametric Curves Presets" src="/wp-content/uploads/2013/03/ParametricCurves_presets.jpg" width="237" height="289" itemprop="screenshot" srcset="/wp-content/uploads/2013/03/ParametricCurves_presets.jpg 237w, /wp-content/uploads/2013/03/ParametricCurves_presets-150x182.jpg 150w" sizes="(max-width: 237px) 100vw, 237px" />There are some ready-made Curves you can try in the drop-down menu &#8220;Select a preset&#8230;&#8221;. When you choose one, the function is pasted in the text area (3) and the curve displayed (1).
    </p>

    <p>
      You&#8217;re allowed to save your own Preset clicking the <strong>New</strong> button (you&#8217;ll be asked to enter a label for it). Add as many presets as you want &#8211; you can always delete the one you don&#8217;t like (but the Default ones) with the <strong>Remove</strong> button, or <strong>Reset</strong> them to the default group.
    </p>

    <h3>
      3 &#8211; Functions
    </h3>

    <p>
      In this text area you can write your own functions, or modify existing ones. Once you&#8217;re done, click the <strong>Try</strong> button in order to test it either as a graph in the Curve panel and as an actual Curves Adjustment Layer in the file.
    </p>

    <p>
      4If you&#8217;re satisfied with the result, click the <strong>Apply</strong> button, otherwise you can tweak the function of <strong>Cancel</strong> back to Photoshop.
    </p>

    <h3>
      4 &#8211; Information
    </h3>

    <p>
      The <strong>Math Ref.</strong> button (which can be toggled on/off) shows a handy reference for the Math. Javascript Object (see below for more information):
    </p>

    <p>
      <img class="aligncenter size-full wp-image-1879" alt="Parametric Curves Math reference" src="/wp-content/uploads/2013/03/ParametricCurves_interface_Math.jpg" width="884" height="512" itemprop="screenshot" srcset="/wp-content/uploads/2013/03/ParametricCurves_interface_Math.jpg 884w, /wp-content/uploads/2013/03/ParametricCurves_interface_Math-150x86.jpg 150w, /wp-content/uploads/2013/03/ParametricCurves_interface_Math-300x173.jpg 300w" sizes="(max-width: 884px) 100vw, 884px" />
    </p>

    <p>
      The <strong>Tutorial</strong> button points your browser to the <a title="Parametric Curves category" href="/category/photoshop/extensions-and-scripts/parametric-curves/" target="_blank">Parametric Curves category page</a> of this website, while the <strong>?</strong> button shows copyright informations.
    </p>

    <h2>
      Functions Tutorial
    </h2>

    <p>
      This will help you starting with your own functions. Few basic information:
    </p>

    <ul>
      <li>
        The <strong>full range is 0-255</strong>, where Black = 0 and White = 255 (this is independent on the Display option).
      </li>
      <li>
        Functions are in the form: <strong>y = f(x)</strong> and you&#8217;re supposed to write the f(x) part only: so the default Curve (a straight line 45°) is just &#8220;x&#8221;.
      </li>
      <li>
        Functions value is automatically <strong>rounded</strong>.
      </li>
      <li>
        <strong>Javascript</strong> syntax is allowed.
      </li>
      <li>
        More complete Javascript reference is <a title="Javascript Math Object Reference" href="http://www.w3schools.com/jsref/jsref_obj_math.asp" target="_blank">here</a>.
      </li>
    </ul>

    <p>
      I&#8217;m not very good with geometry so let&#8217;s start simple:
    </p>

    <div id="attachment_1885" style="width: 578px" class="wp-caption aligncenter">
      <img aria-describedby="caption-attachment-1885" class="size-full wp-image-1885" alt="Functions 1" src="/wp-content/uploads/2013/03/functions_01.png" width="568" height="142" srcset="/wp-content/uploads/2013/03/functions_01.png 568w, /wp-content/uploads/2013/03/functions_01-150x37.png 150w, /wp-content/uploads/2013/03/functions_01-300x75.png 300w" sizes="(max-width: 568px) 100vw, 568px" />

      <p id="caption-attachment-1885" class="wp-caption-text">
        x+127<br />x-127<br />2*x<br />x/2
      </p>
    </div>

    <p>
      If you need <strong>constants</strong>, they must be written as in this example: <code>Math.PI * x + Math.LN2</code>.
    </p>

    <p>
      You can write <strong>functions</strong> as well of course:
    </p>

    <div id="attachment_1886" style="width: 578px" class="wp-caption aligncenter">
      <img aria-describedby="caption-attachment-1886" class="size-full wp-image-1886" alt="Functions 2" src="/wp-content/uploads/2013/03/functions_02.png" width="568" height="142" srcset="/wp-content/uploads/2013/03/functions_02.png 568w, /wp-content/uploads/2013/03/functions_02-150x37.png 150w, /wp-content/uploads/2013/03/functions_02-300x75.png 300w" sizes="(max-width: 568px) 100vw, 568px" />

      <p id="caption-attachment-1886" class="wp-caption-text">
        Math.pow(x,2) / 70<br />40*Math.log(x)<br />255*Math.random()<br />x+30*Math.sin(x/5)
      </p>
    </div>

    <p>
      You&#8217;d better off <strong>normalizing the output to fit the 0-255 range</strong>, as in the following examples:
    </p>

    <div id="attachment_1890" style="width: 578px" class="wp-caption aligncenter">
      <img aria-describedby="caption-attachment-1890" class="size-full wp-image-1890" alt="Functions 3" src="/wp-content/uploads/2013/03/functions_03.png" width="568" height="142" srcset="/wp-content/uploads/2013/03/functions_03.png 568w, /wp-content/uploads/2013/03/functions_03-150x37.png 150w, /wp-content/uploads/2013/03/functions_03-300x75.png 300w" sizes="(max-width: 568px) 100vw, 568px" />

      <p id="caption-attachment-1890" class="wp-caption-text">
        Math.sin(x)<br />127*Math.sin(x)<br />127*Math.sin(x) + 127<br />127*Math.sin(2*Math.PI*x/255) + 127
      </p>
    </div>

    <p>
      You can also use:
    </p>

    <ul>
      <li>
        The so-called <strong>ternary operator</strong>, in the form of <code>(condition) ? (true) : (false)</code>.
      </li>
      <li>
        The <strong>modulo operator</strong> %, for instance <code>x%10</code> (which gives the rest of the division by 10 and it&#8217;s useful to repeat patterns)
      </li>
      <li>
        <strong>Logic operators</strong> <code>&&</code> (and) <code>||</code> (or), besides <code>==</code> (equal to) and <code>!=</code> (not equal to), <code>&lt;</code> and <code>&gt;</code>
      </li>
      <li>
        Nest them when needed, but be careful not to write bad code!
      </li>
    </ul>

    <div id="attachment_1896" style="width: 578px" class="wp-caption aligncenter">
      <img aria-describedby="caption-attachment-1896" class="size-full wp-image-1896" alt="Functions 4" src="/wp-content/uploads/2013/03/functions_04.png" width="568" height="142" srcset="/wp-content/uploads/2013/03/functions_04.png 568w, /wp-content/uploads/2013/03/functions_04-150x37.png 150w, /wp-content/uploads/2013/03/functions_04-300x75.png 300w" sizes="(max-width: 568px) 100vw, 568px" />

      <p id="caption-attachment-1896" class="wp-caption-text">
        (x>128) ? x : 255 &#8211; x<br />4 * x%256<br />(((x>0) && (x<64)) || ((x>128) && (x<191))) ? x : 255-x<br />x / a
      </p>
    </div>

    <h2>
      Anything a bit more inspirational on the Creative side?
    </h2>

    <p>
      It&#8217;s a bottomless pit as soon as you start experimenting &#8211; please <a title="Gradients and Parametric Curves in Photoshop" href="/2013/03/gradients-and-parametric-curves-in-photoshop/" target="_blank">see this post full of examples</a>.
    </p>

    <p style="text-align: center;">
      <a href="/2013/03/gradients-and-parametric-curves-in-photoshop/" target="_blank"><img class="aligncenter size-full wp-image-1901" alt="Parametric Curves Funky Stuff" src="/wp-content/uploads/2013/03/ParametricCurvesDesign.jpg" width="570" height="570" itemprop="image" srcset="/wp-content/uploads/2013/03/ParametricCurvesDesign.jpg 570w, /wp-content/uploads/2013/03/ParametricCurvesDesign-150x150.jpg 150w, /wp-content/uploads/2013/03/ParametricCurvesDesign-300x300.jpg 300w" sizes="(max-width: 570px) 100vw, 570px" /></a>
    </p>

    <h2>
      Go get it!
    </h2>

    <p>
      Find it on <a title="Adobe Exchange panel" href="http://www.adobeexchange.com" target="_blank">Adobe Exchange</a>, it&#8217;s free! And if you think that after all it&#8217;s a nice piece of software, please leave a review in Exchange.</div>

      <p>
        <meta itemprop="image" content="/wp-content/uploads/2013/03/ParametricCurves_Featured.jpg" />

        <br />

        <meta itemprop="thumbnailUrl" content="/wp-content/uploads/2013/03/ParametricCurves_Featured-150x104.jpg" />
      </p></div>
      
