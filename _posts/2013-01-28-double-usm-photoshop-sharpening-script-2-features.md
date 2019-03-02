---
id: 1521
title: 'Double USM #2: Features'
date: 2013-01-28T19:08:06+01:00
author: Davide Barranca
excerpt: "Double USM is the brand new Sharpening script for Photoshop that I've coded; in this second post of the series I'll cover the user interface, functionality and how to record it into an Action for batch processing."
layout: post
guid: http://www.davidebarranca.com/?p=1521
permalink: /2013/01/double-usm-photoshop-sharpening-script-2-features/
standard_seo_post_level_layout:
  - ""
standard_link_url_field:
  - ""
standard_seo_post_meta_description:
  - ' Double USM Sharpening script for Photoshop; post #2 covers the user interface, functionality and Batch processing.'
seo_post_level_layout:
  - ""
link_url_field:
  - ""
seo_post_meta_description:
  - ""
image: /wp-content/uploads/2013/01/DoubleUSM_interface.jpg
categories:
  - Double USM
  - Extensions and Scripts
  - Photoshop
tags:
  - batch
  - Double USM
  - halos
  - sharpening
  - unsharp mask
---
<div class="pf-content">
  <p>
    Double USM is the brand new Sharpening script for Photoshop that I&#8217;ve coded; in this second post of the series I&#8217;ll cover the user interface, functionality and how to record it into an Action for batch processing.
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
          <a title="Double USM - #1 Introduction" href="/2013/01/double-usm-photoshop-sharpening-script-1-introduction" target="_blank">Introduction (sharpening basics, Double USM)</a>
        </li>
        <li>
          Features (interface, functionality and Batch processing) <strong><- you&#8217;re here!</strong>
        </li>
        <li>
          <a title="Double USM - #3 Examples" href="/2013/01/double-usm-photoshop-sharpening-script-3-examples" target="_blank">Examples (case studies)</a>
        </li>
      </ol>

      <p>
        Double USM v2 is available for sale on my website <a href="http://cc-extensions.com/products/doubleusm/">CC-Extensions</a>. </div> </div>

        <p>
          If you&#8217;re not familiar with Sharpening or want to recap the basics, I&#8217;ve covered the topic <a title="Double USM - #1 Introduction" href="/2013/01/double-usm-photoshop-sharpening-script-1-introduction" target="_blank">extensively here</a>.
        </p>

        <h2>
          Interface
        </h2>

        <p>
          First, <strong>access Double USM from the Photoshop&#8217;s Filter menu</strong>. The User Interface It is quite straightforward, have a look at it:
        </p>

        <p>
          <img class="aligncenter size-full wp-image-1535" src="/wp-content/uploads/2013/01/DoubleUSM_interface.jpg" alt="Double USM Interface" width="519" height="483" srcset="/wp-content/uploads/2013/01/DoubleUSM_interface.jpg 519w, /wp-content/uploads/2013/01/DoubleUSM_interface-150x139.jpg 150w, /wp-content/uploads/2013/01/DoubleUSM_interface-300x279.jpg 300w" sizes="(max-width: 519px) 100vw, 519px" />
        </p>

        <p>
          Basically the upper part of the panel is where <em>sharpening controls</em> belong (1, 2, 3), while in the lower part you can find <em>preview tools</em> (4, 5) and <em>confirmation buttons</em> (6). Details as follows:
        </p>

        <ol>
          <li>
            <strong>Dark Group</strong>: these two sliders let you control Amount and Radius for the sharpening&#8217;s <em>Dark Halos only</em> (equivalent of the Photoshop&#8217;s UnSharp Mask Filter, but darkening only). Input in the text field is allowed too.
          </li>
          <li>
            <strong>Light Group</strong>: same as above, Amount and Radius but for the sharpening&#8217;s <em>Light Halos only</em> (text input allowed).
          </li>
          <li>
            <strong>Threshold</strong>: equivalent of the UnSharp Mask Filter &#8211; a <em>single control</em> for both Dark and Light Group.
          </li>
          <li>
            <strong>Zoom</strong>: Zoom In, Zoom Out and Fit to Screen buttons. Resize the preview accordingly and inform you of the current zoom level in percent.
          </li>
          <li>
            <strong>Preview</strong>: radio buttons that set the preview either <em>On</em> (you see the effect of both Dark and Light sharpening Halos), <em>Off</em> (unsharpened original), <em>Dark</em> (Dark Halos only), <em>Light</em> (Light Halos only).
          </li>
          <li>
            <strong>Confirmation buttons</strong>: <em>Cancel</em> the dialog and switch back to the original image, or <em>Apply</em> the sharpening effect.
          </li>
        </ol></div>

        <h2>
          Functionality
        </h2>

        <p>
          Double USM runs on both on <strong>8bit</strong> and <strong>16bit</strong> files, in <strong>Grayscale</strong>, <strong>RGB</strong>, <strong>Lab</strong> and <strong>CMYK</strong> working spaces. Open a picture, find Double USM in the Photoshop&#8217;s <strong>Filter menu</strong>:
        </p>

        <p>
          <img class="aligncenter size-full wp-image-1538" src="/wp-content/uploads/2013/01/DoubleUSM_screenshot_01.jpg" alt="DoubleUSM screenshot 01" width="570" height="350" srcset="/wp-content/uploads/2013/01/DoubleUSM_screenshot_01.jpg 570w, /wp-content/uploads/2013/01/DoubleUSM_screenshot_01-150x92.jpg 150w, /wp-content/uploads/2013/01/DoubleUSM_screenshot_01-300x184.jpg 300w" sizes="(max-width: 570px) 100vw, 570px" />
        </p>

        <p>
          Mind you, you&#8217;ve got to select a <em>bitmap</em> layer or a <em>Smart Object</em> in the Layers palette (that is: not a text layer, not a vector shape, just something with raster pixels in it &#8211; if you&#8217;re not sure what this mean, you have see in the Layers&#8217; palette layer thumbnail your picture, and that&#8217;s ok).
        </p>

        <p>
          <img class="alignleft size-full wp-image-1545" src="/wp-content/uploads/2013/01/DoubleUSM_screenshot_02.jpg" alt="DoubleUSM screenshot 02" width="344" height="150" srcset="/wp-content/uploads/2013/01/DoubleUSM_screenshot_02.jpg 344w, /wp-content/uploads/2013/01/DoubleUSM_screenshot_02-150x65.jpg 150w, /wp-content/uploads/2013/01/DoubleUSM_screenshot_02-300x130.jpg 300w" sizes="(max-width: 344px) 100vw, 344px" />Now Photoshop goes <strong>fullscreen</strong> to hide disturbing interface elements and a small <strong>progress bar window pops up</strong>: Double USM must calculate a bunch of Smart Objects with Smart Filters and blending modes in order to operate properly (if you&#8217;re curious, the routine is publicly available, I&#8217;ve written extensively about it and published a 5 posts series called <a title="Decomposing Sharpening series" href="/category/photoshop/decomposing-sharpening/" target="_blank">Decomposing Sharpening</a>) &#8211; it should be pretty fast, but it eventually depends on your computer performance.
        </p>

        <p>
          When Double USM is done with its calculation the main window appears (you can drag it along the screen freely) and you can start tweaking the values.
        </p>

        <p>
          <img class="aligncenter size-full wp-image-1547" src="/wp-content/uploads/2013/01/DoubleUSM_screenshot_03.jpg" alt="DoubleUSM screenshot 03" width="570" height="353" srcset="/wp-content/uploads/2013/01/DoubleUSM_screenshot_03.jpg 570w, /wp-content/uploads/2013/01/DoubleUSM_screenshot_03-150x92.jpg 150w, /wp-content/uploads/2013/01/DoubleUSM_screenshot_03-300x185.jpg 300w" sizes="(max-width: 570px) 100vw, 570px" />
        </p>

        <p>
          Use the <strong>Zoom buttons</strong> to reach the zoom level you&#8217;re most comfortable with when evaluating sharpening, <strong>pan the image</strong> (the hand tool is selected by default) to inspect the result in significant areas. You can either <strong>drag sliders</strong> or <strong>input values</strong> directly in the text field (press tab to switch between them). The routine&#8217;s update takes about half of a second to complete (may be even faster on some computers), so please wait the preview&#8217;s refresh of one control, before moving to another one.
        </p>

        <p>
          The <strong>Preview</strong> radio buttons are crucial to evaluate the sharpening effect. You can switch it On and Off, but you&#8217;re allowed to inspect Light Halos only (<em>Light</em> radio button) or <em>Dark</em> Halos only (Dark radio button): this is particularly useful because you can concentrate on the Dark/Light values separately, then judge their combination (preview: On) before clicking the <strong>Apply</strong> button.
        </p>

        <p>
          <img class="aligncenter size-full wp-image-1549" src="/wp-content/uploads/2013/01/DoubleUSM_screenshot_04.jpg" alt="DoubleUSM screenshot 04" width="570" height="337" srcset="/wp-content/uploads/2013/01/DoubleUSM_screenshot_04.jpg 570w, /wp-content/uploads/2013/01/DoubleUSM_screenshot_04-150x88.jpg 150w, /wp-content/uploads/2013/01/DoubleUSM_screenshot_04-300x177.jpg 300w" sizes="(max-width: 570px) 100vw, 570px" />
        </p>

        <p>
          Mind you: <strong>the preview setting that is active when you press Apply will determine the final result</strong>. That is to say: if the preview is set to <em>Light</em> when clicking Apply, only Light Halos will be applied &#8211; conversely, if the preview is set to Dark, only Dark Halos will survive. So, unless you want it expressly, remember to switch the preview to On before clicking Apply.
        </p>

        <p>
          <img class="alignleft size-full wp-image-1551" src="/wp-content/uploads/2013/01/DoubleUSM_screenshot_05.jpg" alt="DoubleUSM screenshot 05" width="300" height="206" srcset="/wp-content/uploads/2013/01/DoubleUSM_screenshot_05.jpg 300w, /wp-content/uploads/2013/01/DoubleUSM_screenshot_05-150x103.jpg 150w" sizes="(max-width: 300px) 100vw, 300px" />As you see in the Layer&#8217;s palette screenshot, <strong>the resulting elaboration</strong> of the Double USM routine <strong>belongs to a new layer</strong> (i.e. the original layer is left untouched), <strong>named according to the used parameters</strong>. In the example, &#8220;Double USM: D(300, 1.5), L(150, 0.7), T(0)&#8221; means that I&#8217;ve applied Amount 300% and Radius 1.5px for the Dark Halos, Amount 150% and Radius 0.7px for the Light Halos, and Threshold has been left to its default, zero. In case you want to apply Double USM to a Smart Object (yes you can), the resulting layer will be a new, raster one anyway &#8211; scripts can&#8217;t be tied as smart filters (this is a technical limit of the platform).
        </p>

        <p>
          Lastly, I&#8217;d like to mention that no matter whether the layer you&#8217;re applying Double USM to is beneath other layers (adjustments, texts, vectors, whatever), the preview and the result will be based on that very layer only &#8211; this is consistent with Photoshop&#8217;s Filters general behavior.
        </p>

        <h2>
          <img class="alignleft size-full wp-image-1554" src="/wp-content/uploads/2013/01/DoubleUSM_screenshot_06.jpg" alt="DoubleUSM screenshot 06" width="273" height="221" srcset="/wp-content/uploads/2013/01/DoubleUSM_screenshot_06.jpg 273w, /wp-content/uploads/2013/01/DoubleUSM_screenshot_06-150x121.jpg 150w" sizes="(max-width: 273px) 100vw, 273px" />Actions
        </h2>

        <p>
          Double USM can be recorded into Actions, so that you&#8217;re allowed to apply it on batch via the Photoshop&#8217;s File &#8211; Automate &#8211; Batch command. This is particularly appropriate when you&#8217;ve to process <em>blindly</em> large amount of images with output sharpening for instance (before sending them to some kind of printing process).
        </p>

        <p>
          Just create a new Action and, while recording, run Double USM. You can check later in the Actions palette the used parameters.
        </p>

        <p>
          That&#8217;s basically it for Double USM interface, functionality and the Actions feature. The next post will be about real world examples &#8211; traditional, HiRaLoAm and mixed sharpening.
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
                <a title="Double USM - #1 Introduction" href="/2013/01/double-usm-photoshop-sharpening-script-1-introduction" target="_blank">Introduction (sharpening basics, Double USM)</a>
              </li>
              <li>
                Features (interface, functionality and Batch processing) <strong><- you&#8217;re here!</strong>
              </li>
              <li>
                <a title="Double USM - #3 Examples" href="/2013/01/double-usm-photoshop-sharpening-script-3-examples" target="_blank">Examples (case studies)</a>
              </li>
            </ol>

            <p>
              Double USM v2 is available for sale on my website <a href="http://cc-extensions.com/products/doubleusm/">CC-Extensions</a>. </div> </div> </div> <!-- Share-Widget Button BEGIN --><a href="javascript:void(0);" myshare_id="mys_shareit" myshare_url="/2013/01/double-usm-photoshop-sharpening-script-2-features/" myshare_title="Double USM #2: Features" rel="nofollow" onclick=" return false;" style="text-decoration:none; color:#000000; font-size:11px; line-height:20px;">
            
