---
id: 1472
title: 'Double USM Photoshop Sharpening Script #1: Introduction'
date: 2013-01-27T17:50:30+01:00
author: Davide Barranca
excerpt: "Double USM is the brand new Sharpening Photoshop's script I've coded! Let me introduce it to you in a three posts series: in this first one I'll be discussing why it's useful and how it works. Post #2 will be about the interface and how it works. In Post #3 I'll show you examples and we'll try to find a place for it in your own image processing workflow."
layout: post
guid: http://www.davidebarranca.com/?p=1472
permalink: /2013/01/double-usm-photoshop-sharpening-script-1-introduction/
standard_seo_post_level_layout:
  - ""
standard_link_url_field:
  - ""
standard_seo_post_meta_description:
  - "Double USM is a Photoshop script for Sharpening; in this Introduction I'll show you why it is different from traditional UnSharpMask filter."
seo_post_level_layout:
  - ""
link_url_field:
  - ""
seo_post_meta_description:
  - ""
image: /wp-content/uploads/2013/01/HighFrequencyDetail.jpg
category:
  - Double USM
  - Extensions and Scripts
  - Photoshop
tags:
  - Double USM
  - halos
  - sharpening
  - unsharp mask
---
<div class="pf-content">
  <p>
    Double USM is the brand <strong>new Sharpening script for Photoshop</strong> that I&#8217;ve coded, let me introduce it to you in a three posts series! Post #1, this one, I&#8217;ll be discussing <strong>sharpening basics and how Double USM is different</strong>. Post #2 is about the <strong>interface, functionality and batch processing</strong>. In post #3 I&#8217;ll show you <strong>real world examples</strong>, for traditional, HiRaLoAm and mixed sharpening.
  </p>

  <p>
    <!--more-->
  </p>

  <div class="panel panel-primary">
    <div class="panel-heading">
      <h3 class="panel-title">
        UPDATE
      </h3></p>
    </div>

    <div class="panel-body">
      <p>
        This is a 3 part series on the <strong>Double USM</strong> Photoshop plugin.<br /> Even if the version used in this article is older than the current one, the sharpening concepts still apply! Find the <a href="/2017/02/double-usm-v2-for-photoshop-has-been-released/">new version announcement here</a>
      </p>

      <ol>
        <li>
          Introduction (sharpening basics, Double USM) <strong><- you&#8217;re here</strong>
        </li>
        <li>
          <a title="Double USM - #2 Features " href="/2013/01/double-usm-photoshop-sharpening-script-2-features" target="_blank">Features (interface, functionality and Batch processing)</a>
        </li>
        <li>
          <a title="Double USM - #3 Examples" href="/2013/01/double-usm-photoshop-sharpening-script-3-examples" target="_blank">Examples (case studies)</a>
        </li>
      </ol>

      <p>
        Double USM v2 is available for sale on my website <a href="http://cc-extensions.com/products/doubleusm/">CC-Extensions</a>. </div> </div>

        <h2>
          Why bother with Sharpening at all?
        </h2>

        <p>
          <img class="alignleft size-medium wp-image-1479" alt="Double USM - High Frequency Detail Sharpening" src="/wp-content/uploads/2013/01/HighFrequencyDetail-300x300.jpg" width="300" height="300" />If you look away from the monitor for a moment, you&#8217;ll see the <strong>world you live in as a source of an <em>apparently</em> endless detail</strong>; things around you can be probed up to the microscopic level (provided you&#8217;ve got a superhero eyesight) and still reveal new features.<br /> Which is not a big news, as it&#8217;s not a big news to discover that <strong>digital captures</strong> of the world (either a landscape, a portrait, a still life&#8230;) you&#8217;ve taken with some expensive equipment <strong>don&#8217;t contain such an infinite detail</strong> &#8211; just a portion of it. It&#8217;s not CSI, zoom in 10.000% on your 16bit RAW file and you&#8217;ll be staring just at a huge, single pixel.
        </p>

        <p>
          The fact you&#8217;re not allowed by the laws of the universe to capture but a portion of the world&#8217;s infinite detail, results in a <strong>softer picture</strong> &#8211; if you compare what you perceive out there in the wild, with your digital images. Softness <strong>you&#8217;ve got to compensate</strong> for, especially if pictures are going to be printed (that is, they&#8217;ll <strong>undergo another reduction of information levels available</strong>).
        </p>

        <p>
          Luckily, our perception is easily deceived by appearances &#8211; if you inject your pictures with a little bit of the right <em>drug</em>, they suddenly look &#8220;right&#8221;, or &#8220;better&#8221;. That drug is called <strong>sharpening</strong>.
        </p>

        <h2>
          Halos
        </h2>

        <p>
          How is it that we&#8217;re tricked into <em>perceiving</em> things as more detailed? Short answer: by means of <strong>halos around borders</strong>, that&#8217;s the little magic. Long answer would mention things like &#8220;<em>retina&#8217;s receptive fields</em>&#8220;, &#8220;<em>lateral inhibition</em>&#8221; and &#8220;<em>Mach bands</em>&#8221; &#8211; but I won&#8217;t go any further here, even though they&#8217;re fascinating topics.
        </p>

        <p>
          <img class="alignnone size-full wp-image-1482" alt="Portrait With Halos" src="/wp-content/uploads/2013/01/PortraitWithHalos.png" width="570" height="367" srcset="/wp-content/uploads/2013/01/PortraitWithHalos.png 570w, /wp-content/uploads/2013/01/PortraitWithHalos-150x96.png 150w, /wp-content/uploads/2013/01/PortraitWithHalos-300x193.png 300w" sizes="(max-width: 570px) 100vw, 570px" /><br /> So: <strong>a border is found where two differently perceived areas meet</strong>: it may be one darker the other lighter, different color, tone, etc. What sharpening as we know it in the Photoshop&#8217;s implementation (aka Gaussian Sharpening) does, is to create <strong>two tiny gradients</strong> around borders. A <em>darker</em> one spreading inside the darker area, a <em>lighter</em> one spreading inside the lighter area. If those artifacts are small enough to be noticed as just artifacts, our perception blends them and we get <strong>the impression of more detail</strong> &#8211; we&#8217;re that easy as humans, you see.
        </p>

        <p>
          <img class="size-full wp-image-1484 alignright" alt="Photoshop UnSharpMask Filter" src="/wp-content/uploads/2013/01/UnSharpMaskFilter.jpg" width="200" height="264" srcset="/wp-content/uploads/2013/01/UnSharpMaskFilter.jpg 200w, /wp-content/uploads/2013/01/UnSharpMaskFilter-150x198.jpg 150w" sizes="(max-width: 200px) 100vw, 200px" />
        </p>

        <h2>
          UnSharp Mask filter
        </h2>

        <p>
          Photoshop owns one since version 1.0. <em>UnSharpMask</em> (<strong>USM</strong> from now on) lets you specify three parameters to describe the sharpening artifacts (that is: halos) features.
        </p>

        <ol>
          <li>
            How strong are they (<strong>Amount</strong>)
          </li>
          <li>
            How widespread are they (<strong>Radius</strong>)
          </li>
          <li>
            Should they be applied everywhere (<strong>Threshold</strong>)?
          </li>
        </ol>

        <p>
          The three are independent controls: as far as so called &#8220;traditional sharpening&#8221; is involved, you usually set large Amounts and relatively small Radii &#8211; as in the screenshot, which is a reasonable starting point for web-targeted images; while the contrary may give interesting results as well (read along about HiRaLoAm).
        </p>

        <p>
          Find below the controls&#8217; effect on a synthetic test file.
        </p>

        <div id="attachment_1496" style="width: 580px" class="wp-caption alignnone">
          <img aria-describedby="caption-attachment-1496" class="size-full wp-image-1496" alt="USM Settings: Amount" src="/wp-content/uploads/2013/01/USM_Settings_Amount.jpg" width="570" height="190" srcset="/wp-content/uploads/2013/01/USM_Settings_Amount.jpg 570w, /wp-content/uploads/2013/01/USM_Settings_Amount-150x50.jpg 150w, /wp-content/uploads/2013/01/USM_Settings_Amount-300x100.jpg 300w" sizes="(max-width: 570px) 100vw, 570px" />

          <p id="caption-attachment-1496" class="wp-caption-text">
            Amount: the <b>Halos&#8217; strength</b>
          </p>
        </div>

        <div id="attachment_1498" style="width: 580px" class="wp-caption alignnone">
          <img aria-describedby="caption-attachment-1498" class="size-full wp-image-1498" alt="USM Settings: Radius" src="/wp-content/uploads/2013/01/USM_Settings_Radius.jpg" width="570" height="190" srcset="/wp-content/uploads/2013/01/USM_Settings_Radius.jpg 570w, /wp-content/uploads/2013/01/USM_Settings_Radius-150x50.jpg 150w, /wp-content/uploads/2013/01/USM_Settings_Radius-300x100.jpg 300w" sizes="(max-width: 570px) 100vw, 570px" />

          <p id="caption-attachment-1498" class="wp-caption-text">
            Radius: the <b>Halos&#8217; thickness</b>
          </p>
        </div>

        <div id="attachment_1499" style="width: 580px" class="wp-caption alignnone">
          <img aria-describedby="caption-attachment-1499" class="size-full wp-image-1499" alt="Threshold: Halos'... threshold" src="/wp-content/uploads/2013/01/USM_Settings_Threshold.jpg" width="570" height="190" srcset="/wp-content/uploads/2013/01/USM_Settings_Threshold.jpg 570w, /wp-content/uploads/2013/01/USM_Settings_Threshold-150x50.jpg 150w, /wp-content/uploads/2013/01/USM_Settings_Threshold-300x100.jpg 300w" sizes="(max-width: 570px) 100vw, 570px" />

          <p id="caption-attachment-1499" class="wp-caption-text">
            Threshold: the <b>Halos&#8217;&#8230; threshold</b>
          </p>
        </div>

        <p>
          I must mention one thing: we all know what <strong>oversharpening</strong> means &#8211; it&#8217;s a delicate topic, photographers go nuts when retouchers oversharpen their images &#8211; i.e. they are strong handed with USM filter.
        </p>

        <div id="attachment_1502" style="width: 640px" class="wp-caption alignnone">
          <img aria-describedby="caption-attachment-1502" class="size-full wp-image-1502" alt="Oversharpening example" src="/wp-content/uploads/2013/01/oversharpened.jpg" width="630" height="499" srcset="/wp-content/uploads/2013/01/oversharpened.jpg 630w, /wp-content/uploads/2013/01/oversharpened-150x118.jpg 150w, /wp-content/uploads/2013/01/oversharpened-300x237.jpg 300w" sizes="(max-width: 630px) 100vw, 630px" />

          <p id="caption-attachment-1502" class="wp-caption-text">
            What do you think about, is this picture oversharpened?
          </p>
        </div>

        <p>
          The <em>psychological threshold</em> (how much USM is too much?) is to some extent a subjective choice. As a retoucher, I admit I&#8217;ve kind of a higher tolerance (I know for instance that printed images require, and can stand, much more USM that you&#8217;d consider fair just looking at them in a monitor).
        </p>

        <h2>
          So what&#8217;s wrong with USM?
        </h2>

        <p>
          <img class="alignleft size-full wp-image-1503" alt="Photoshop Maestro Dan Margulis" src="/wp-content/uploads/2013/01/DanMargulis.jpg" width="168" height="168" srcset="/wp-content/uploads/2013/01/DanMargulis.jpg 168w, /wp-content/uploads/2013/01/DanMargulis-150x150.jpg 150w" sizes="(max-width: 168px) 100vw, 168px" />My color correction maestro <strong>Dan Margulis</strong> did study the topic of sharpening (among the rest) extensively, and came up with some experimental discoveries worth mentioning.<br /> What is that we&#8217;re bothered the most with, when we look at oversharpened pictures? <em>Halos</em> of course, but he&#8217;s been the first one to ask the simple question: &#8220;Ok, but <em>which kind</em>?&#8221;.
        </p>

        <p>
          <strong>Sharpening introduces at the same time Light Halos and Dark Halos &#8211; do we respond to them in the same way?</strong>
        </p>

        <p>
          Take a look at the following picture.
        </p>

        <div id="attachment_1505" style="width: 580px" class="wp-caption alignnone">
          <img aria-describedby="caption-attachment-1505" class="size-full wp-image-1505" alt="Sharpening blending modes" src="/wp-content/uploads/2013/01/Sharpening_BlendingModes.jpg" width="570" height="570" srcset="/wp-content/uploads/2013/01/Sharpening_BlendingModes.jpg 570w, /wp-content/uploads/2013/01/Sharpening_BlendingModes-150x150.jpg 150w, /wp-content/uploads/2013/01/Sharpening_BlendingModes-300x300.jpg 300w" sizes="(max-width: 570px) 100vw, 570px" />

          <p id="caption-attachment-1505" class="wp-caption-text">
            <b>A</b>: original image; <b>B</b>: sharpened image (Light + Dark Halos); <b>C</b>: Light Halos only; <b>D</b>: Dark Halos only.
          </p>
        </div>

        <p>
          We may agree that the original version is way too soft, while the sharpened&#8217;s gone too far. But&#8230; what about the rest? Light Halos, as well, may seem too much, while we may be perfectly fine with Dark halos.
        </p>

        <p>
          That is to say: <strong>we&#8217;re easily disturbed by Light halos, while Dark ones are less annoying</strong>.<br /> Whether it is their <em>strength (Amount)</em>, or their <em>size (Radius)</em> that bother us, we&#8217;re perhaps not sure about &#8211; but Light Halos should be toned down, somehow.<br /> So far, an easy and well known way to do this was to apply USM on a layer, duplicate it, set one layer to Darken (Dark Halos only) and the other to Lighten (Light Halos only) and lower the latter&#8217;s opacity. Which is somehow equivalent to lower the Amount of the Light Halos altogether.
        </p>

        <p>
          But what about if Light Halos could be made smaller (<em>Radius</em>) too?
        </p>

        <div id="attachment_1515" style="width: 580px" class="wp-caption alignnone">
          <img aria-describedby="caption-attachment-1515" class="size-full wp-image-1515" alt="Double USM Sharpening" src="/wp-content/uploads/2013/01/Double_Sharpening_ef.jpg" width="570" height="285" />

          <p id="caption-attachment-1515" class="wp-caption-text">
            <b>E</b>: image sharpened traditionally. F: sharpening where Light Halos are <strong>half the strength</strong> of, and <strong>two third thinner</strong> than, the Dark Halos. (heavy handed correction just to show you more clearly the difference!)
          </p>
        </div>

        <p>
          The picture will benefit from a sharpening that is more robust on the dark side (<em>stronger in Amount, wider in Radius</em>), and less on the light side (<em>weaker Amount, narrower Radius</em>), as you can see in the above comparison.
        </p>

        <div id="attachment_1512" style="width: 305px" class="wp-caption alignleft">
          <img aria-describedby="caption-attachment-1512" class="size-medium wp-image-1512" alt="Double USM gui" src="/wp-content/uploads/2013/01/DoubleUSM_gui-295x300.png" width="295" height="300" srcset="/wp-content/uploads/2013/01/DoubleUSM_gui-295x300.png 295w, /wp-content/uploads/2013/01/DoubleUSM_gui-150x152.png 150w, /wp-content/uploads/2013/01/DoubleUSM_gui.png 424w" sizes="(max-width: 295px) 100vw, 295px" />

          <p id="caption-attachment-1512" class="wp-caption-text">
            Double USM user interface: <b>Amount and Radius controls for both Dark and Light Halos</b>
          </p>
        </div>

        <h2>
          Double USM
        </h2>

        <p>
          Since drum scanners&#8217; softwares did have this feature decades ago, why not implementing it back in Photoshop now? <strong>Double USM lets you tweak both Amount and Radius for Dark and Light halos separately, at the same time.</strong>
        </p>

        <p>
          A double set of sliders can be useful not only for <strong>traditional sharpening</strong>, but it gives unprecedented control also for <strong>HiRaLoAm</strong> (as Dan Margulis calls it: sharpening with unusual <em>High Radius and Low Amount</em>, used as a local contrast enhancer), or as a tool for <strong>mixed sharpening</strong> (Dark Halos: traditional, Light Halos: HiRaLoAm).
        </p>

        <p>
          <em>You may be tempted to do the trick with Darken and Lighten layers but save your time: alas it can&#8217;t work, as <a title="Decomposing Sharpening #2 Mistakes" href="/2012/09/decomposing-sharpening-part2-mistakes/" target="_blank">I&#8217;ve demonstrated here</a>.</em>
        </p>

        <div class="panel panel-primary">
          <div class="panel-heading">
            <h3 class="panel-title">
              UPDATE
            </h3></p>
          </div>

          <div class="panel-body">
            <p>
              This is a 3 part series on the <strong>Double USM</strong> Photoshop plugin.<br /> Even if the version used in this article is older than the current one, the sharpening concepts still apply! Find the <a href="/2017/02/double-usm-v2-for-photoshop-has-been-released/">new version announcement here</a>
            </p>

            <ol>
              <li>
                Introduction (sharpening basics, Double USM) <strong><- you&#8217;re here</strong>
              </li>
              <li>
                <a title="Double USM - #2 Features " href="/2013/01/double-usm-photoshop-sharpening-script-2-features" target="_blank">Features (interface, functionality and Batch processing)</a>
              </li>
              <li>
                <a title="Double USM - #3 Examples" href="/2013/01/double-usm-photoshop-sharpening-script-3-examples" target="_blank">Examples (case studies)</a>
              </li>
            </ol>

            <p>
              Double USM v2 is available for sale on my website <a href="http://cc-extensions.com/products/doubleusm/">CC-Extensions</a>. </div> </div> </div> <!-- Share-Widget Button BEGIN --><a href="javascript:void(0);" myshare_id="mys_shareit" myshare_url="/2013/01/double-usm-photoshop-sharpening-script-1-introduction/" myshare_title="Double USM Photoshop Sharpening Script #1: Introduction" rel="nofollow" onclick=" return false;" style="text-decoration:none; color:#000000; font-size:11px; line-height:20px;">
            
