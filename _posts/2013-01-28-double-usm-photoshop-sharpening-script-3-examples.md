---
id: 1524
title: 'Double USM #3: Examples'
date: 2013-01-28T19:10:22+01:00
author: Davide Barranca
excerpt: "Double USM is the brand new Sharpening script for Photoshop coded by Davide Barranca; in this third post of the series I'll show you examples for traditional sharpening, HiRaLoAm (High Radius Low Amount) and mixed sharpening."
layout: post
guid: http://www.davidebarranca.com/?p=1524
permalink: /2013/01/double-usm-photoshop-sharpening-script-3-examples/
standard_seo_post_level_layout:
  - ""
standard_link_url_field:
  - ""
standard_seo_post_meta_description:
  - ' Double USM Sharpening script for Photoshop; post #3 shows real world examples for traditional, HiRaLoAm and mixed sharpening'
seo_post_level_layout:
  - ""
link_url_field:
  - ""
seo_post_meta_description:
  - ""
image: /wp-content/uploads/2013/01/Sharpening_example_03_DoubleUSM.jpg
categories:
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
    Double USM is a brand new Sharpening script for Photoshop; in this third post of the series I&#8217;ll show you examples for <strong>traditional sharpening</strong>, <strong>HiRaLoAm</strong> (High Radius Low Amount) and <strong>mixed sharpening</strong>.
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
          <a title="Double USM - #2 Features " href="/2013/01/double-usm-photoshop-sharpening-script-2-features" target="_blank">Features (interface, functionality and Batch processing)</a>
        </li>
        <li>
          Examples (case studies) <strong><- you&#8217;re here!</strong>
        </li>
      </ol>

      <p>
        Double USM v2 is available for sale on my website <a href="http://cc-extensions.com/products/doubleusm/">CC-Extensions</a>. </div> </div>

        <h2>
          Warning!
        </h2>

        <p>
          If you&#8217;re not familiar with Sharpening or want to recap the basics, I&#8217;ve covered the topic <a title="Double USM - #1 Introduction" href="/2013/01/double-usm-photoshop-sharpening-script-1-introduction" target="_blank">extensively here</a>. We&#8217;re going to explore three kind of sharpening you can apply with Double USM: <em>traditional</em> (fine detail), <em>HiRaLoAm</em> (shape and 3D look), <em>mixed</em> (a combination of the former ones). I&#8217;m going to show an example for each case, but there&#8217;s of course a lot of room for your personal experimentation, which I strongly encourage (you can read a <a title="Sharpening case study" href="/2011/11/sharpening-case-study/" target="_blank">step-by-step Sharpening Case Study</a> I&#8217;ve published some time ago as an example of a possible advanced application).
        </p>

        <p>
          <img class="alignleft size-full wp-image-1580" src="/wp-content/uploads/2013/01/grappa_150.jpg" alt="Grappa" width="150" height="242" />Mind you: <strong>I&#8217;ll be a bit heavy-handed</strong> in the examples &#8211; I&#8217;d like you to spot halos easily; moreover, we&#8217;ll be studying not only &#8220;<em>how much</em>&#8221; sharpening to apply (which can be a matter of taste), but specifically &#8220;<em>which kind</em>&#8220;. That is to say, the <strong>Dark / Light Halos ratios</strong>, both in terms of Amount and Radius. <strong>Don&#8217;t mind too much absolute values</strong> (why 345% Amount and not 350%?), <strong>but instead think to the relation between Dark and Light sharpening, this way</strong>: Light Halos are, say, <em>one third smaller</em> (Radius) and <em>a half weaker</em> (Amount) than Dark Halos.
        </p>

        <p>
          I&#8217;m interested in finding the &#8220;<em>right kind</em>&#8221; of sharpening; as once my Maestro <a title="Dan Margulis" href="http://www.ledet.com/margulis" target="_blank">Dan Margulis</a> during one of is italian trip said &#8220;one liter of <a title="Grappa" href="http://en.wikipedia.org/wiki/Grappa" target="_blank" rel="nofollow">Grappa</a> is too much of a good idea&#8221;, so if you think it&#8217;s over-sharpened, just reduce the opacity of the layer to your taste and it&#8217;ll be OK.
        </p>

        <h2>
          Traditional Sharpening
        </h2>

        <p>
          This is what we&#8217;re mostly used to: high frequency detail boosting (i.e. small features and borders). Standard sharpening values include <strong>high Amounts</strong> (up to 500% if you&#8217;re heavy-handed) and <strong>low Radii</strong> (it depends on the image size and the output &#8211; display, print/press &#8211; but we talk about values ranging from 0.2/0.4px for the web, up to about 2.0px or more). Threshold is really up to you, and must be evaluate on each picture: sometimes you&#8217;d be better off masking the sharpening level with some kind of luminosity mask or border mask, instead to tweaking the Threshold.
        </p>

        <div id="attachment_1585" style="width: 580px" class="wp-caption aligncenter">
          <img aria-describedby="caption-attachment-1585" class="size-full wp-image-1585" src="/wp-content/uploads/2013/01/Sharpening_example_01a_orig.jpg" alt="Sharpening example 01 original" width="570" height="428" srcset="/wp-content/uploads/2013/01/Sharpening_example_01a_orig.jpg 570w, /wp-content/uploads/2013/01/Sharpening_example_01a_orig-150x112.jpg 150w, /wp-content/uploads/2013/01/Sharpening_example_01a_orig-300x225.jpg 300w" sizes="(max-width: 570px) 100vw, 570px" />

          <p id="caption-attachment-1585" class="wp-caption-text">
            Still-life shot of a Manfrotto leather photo bag by <a title="Roberto Bigano photographer" href="http://www.bigano.com" target="_blank">Roberto Bigano</a>
          </p>
        </div>

        <p>
          This first picture comes as a pretty high resolution file &#8211; it&#8217;s been captured using an <strong>Hasselblad 528 Multi Shot</strong> (four shots recording real non-interpolated color) with a <strong>HC4-120 Macro</strong> lens.
        </p>

        <p>
          <img class="alignleft size-full wp-image-1587" src="/wp-content/uploads/2013/01/Sharpening_example_01_orig_detail.jpg" alt="Sharpening example 01 orig detail" width="300" height="300" srcset="/wp-content/uploads/2013/01/Sharpening_example_01_orig_detail.jpg 300w, /wp-content/uploads/2013/01/Sharpening_example_01_orig_detail-150x150.jpg 150w" sizes="(max-width: 300px) 100vw, 300px" />
        </p>

        <p>
          Roberto masters a particular technique called <strong>Focus Stacking</strong> (that is, he takes several multi-shot captures focusing at progressively distant planes, then blends the files into a amazingly detailed picture).  Anyway, at left there is a 100% crop of a detail so you can get the idea of the image size and quality. I&#8217;m going to <strong>downsample the file</strong> to 50% so that the values refers to a picture sized: 2720 x 2040 px. Let&#8217;s pretend we need to sharpen it for a printed ad.
        </p>

        <p>
          We can easily predict that a traditional sharpening (high Amount, Low Radius would emphasize too much the fabric texture, which highlights are going to &#8220;eat&#8221; detail, resulting in a polka-dots-like white noise, a bad scenario. On the other hand, sheet-fed presses and high quality coated papers can stand quite a bit of sharpening, so we&#8217;d like to boost the existing detail &#8211; it&#8217;s a finely crafted technical product after all, and the texture should be prominent in my personal opinion.
        </p>

        <p>
          So, as follows there&#8217;s a comparison between the original file, the Photoshop&#8217;s UnSharp Mask filter (Amount 300%, Radius 1.5px) and the Double USM default (which Dark Halos are the very same 300% and 1.5px, but <strong>Light Halos are 50% thinner and 50% weaker</strong> &#8211; that is to say: Amount 150% and Radius 0.7)
        </p>

        <div style="padding: 10px; border: 1px solid #ededed; background-color: #f8f8f8; border-radius: 4px; margin-bottom: 20px;">
          [widgetkit id=1591]
        </div>

        <h2>
          HiRaLoAm
        </h2>

        <p>
          This is how <a title="Dan Margulis" href="http://www.ledet.com/margulis" target="_blank">Dan Margulis</a> first called a sharpening applied with <strong>High Radius, Low Amount</strong> (the reverse of traditional sharpening). Instead of enhancing high fine detail, a high Radius (in the range of about 15/50px or more, depending on the image size and the subject&#8217;s features) affects wider frequencies and basically shapes the subject: it makes it appear more three-dimensional.
        </p>

        <p>
          We&#8217;ll be using the following image, a Texas Tea hybrid rose (see the caption for full details &#8211; the original file is about 4400x3600px).
        </p>

        <div id="attachment_1603" style="width: 580px" class="wp-caption aligncenter">
          <img aria-describedby="caption-attachment-1603" class="size-full wp-image-1603" src="/wp-content/uploads/2013/01/Sharpening_example_02_original.jpg" alt="Sharpening example 02 original" width="570" height="475" srcset="/wp-content/uploads/2013/01/Sharpening_example_02_original.jpg 570w, /wp-content/uploads/2013/01/Sharpening_example_02_original-150x125.jpg 150w, /wp-content/uploads/2013/01/Sharpening_example_02_original-300x250.jpg 300w" sizes="(max-width: 570px) 100vw, 570px" />

          <p id="caption-attachment-1603" class="wp-caption-text">
            <strong>Plaublel 4&#215;5</strong> camera with <strong>Sinar Vario</strong> film holder and <strong>Schneider Symmar Macro 180</strong> lens.<br />Fujichrome <strong>Velvia50 6x7cm</strong>, scanner <strong>Hasselblad Flextight X5</strong> &#8211; © <a title="Roberto Bigano photographer" href="http://www.bigano.com" target="_blank">Roberto Bigano</a>
          </p>
        </div>

        <p>
          If you&#8217;re not familiar with HiRaLoAm, practice with it this way:
        </p>

        <ol>
          <li>
            Open a picture and duplicate the background layer (precautionary measure).
          </li>
          <li>
            Run Photoshop&#8217;s own UnSharp Mask filter and pump up the Amount slider to the max (500%), leave Threshold alone and start raising the Radius up to very large numbers (say 15, 35, 70 pixels or more).
          </li>
        </ol>

        <p>
          Oh yes, the picture looks like crap but it&#8217;s OK:
        </p>

        <div id="attachment_1604" style="width: 580px" class="wp-caption aligncenter">
          <img aria-describedby="caption-attachment-1604" class="size-full wp-image-1604" src="/wp-content/uploads/2013/01/Sharpening_example_02_comparison.jpg" alt="Sharpening example 02 comparison" width="570" height="474" srcset="/wp-content/uploads/2013/01/Sharpening_example_02_comparison.jpg 570w, /wp-content/uploads/2013/01/Sharpening_example_02_comparison-150x124.jpg 150w, /wp-content/uploads/2013/01/Sharpening_example_02_comparison-300x249.jpg 300w" sizes="(max-width: 570px) 100vw, 570px" />

          <p id="caption-attachment-1604" class="wp-caption-text">
            Testing high Radii at crazy Amount &#8211; just to assess the right Radius for HiRaLoAm
          </p>
        </div>

        <p>
          You see that, even though the Amount is absurd, HiRaLoAm shapes the yellow rose&#8217;s volume &#8211; which Radius does the job best in your opinion?
        </p>

        <p>
          <em>(I haven&#8217;t bothered to fade the UnSharp Mask filter to Luminosity with &#8220;Edit -> Fade&#8230;&#8221; menu item &#8211; in order to avoid chromatic aberration as you see &#8211; we don&#8217;t care about them now)</em>
        </p>

        <ol start="3">
          <li>
            Tweak Threshold if necessary, to suppress noise in areas where it may be a problem.
          </li>
          <li>
            Finally, reduce Amount to pleasing levels &#8211; something like 30% up to 70% (again, these are just suggested values).
          </li>
        </ol>

        <p>
          Double USM has the power to let you control both Light and Dark shaping Halos separately (Amount and Radius), so you&#8217;ve unprecedented  control on the 3D effect you&#8217;re after. Please see in the following comparison the original image, Dark Halos only (available in Double USM checking the Dark preview radio-button), Light Halos only (ibid.) and the combination of <strong>Double USM Dark + Light Halos</strong> (the final result).
        </p>

        <div style="padding: 10px; border: 1px solid #ededed; background-color: #f8f8f8; border-radius: 4px; margin-bottom: 20px;">
          [widgetkit id=1608]
        </div>

        <p>
          Applied values on the original sized image are: Dark Halos (Amount: 40%, Radius: 60px), Light Halos (Amount 80%, Radius 80px), Threshold zero. So this time <strong>Dark Halos are half weaker and three quarters the size of Light Halos</strong>.
        </p>

        <h2>
          Mixed Sharpening
        </h2>

        <p>
          So far we&#8217;ve seen how to tweak Dark and Light Halos in traditional sharpening to boost fine detail, and how to shape a picture with broader halos in HiRaLoAm to get a three-dimensional effect.
        </p>

        <p>
          Besides advanced topics (masking and iterative sharpening for instance), an easy yet very effective technique that Double USM only allows is:
        </p>

        <ol>
          <li>
            <span style="line-height: 13px;">Use <strong>Dark Halos</strong> at high Amount and low Radius to boost the structure as you&#8217;d do with <strong>traditional sharpening</strong>.</span>
          </li>
          <li>
            Use <strong>Light Halos</strong> to add volume, with low Amount and High Radius as you&#8217;d do with <strong>HiRaLoAm</strong>.
          </li>
        </ol>

        <p>
          The following is just one example, and it comes from Roberto Bigano&#8217;s <a title="Plastic Girls" href="http://bigano.com/index.php/en/personal-works/106-personal-works/302-roberto-bigano-plastic-girls-1978-2011.html" target="_blank">Plastic Girls</a> project (a 30 years long study on mannequins around the world). This &#8220;girl&#8221; has been shot with a <strong>Canon G12</strong> compact camera, which at 80 ISO delivers an amazing quality, and it&#8217;s just a low-res crop 1500px wide.
        </p>

        <p>
          Applied <strong>Double USM values are as follows</strong>: Dark Halos (Amount: 500%, Radius: 0.5px), Light Halos (Amount 40%, Radius 40px), Threshold (3 levels).
        </p>

        <div style="padding: 10px; border: 1px solid #ededed; background-color: #f8f8f8; border-radius: 4px; margin-bottom: 20px;">
          [widgetkit id= 1615]
        </div>

        <h2>
          Experiment!
        </h2>

        <p>
          Sharpening is a topic with quite many ramification &#8211; so there&#8217;s a lot of room for personal research; your skills will grow as you get more experienced. Have a look to this <a title="Sharpening case study" href="/2011/11/sharpening-case-study/" target="_blank">step-by-step Sharpening Case Study</a> I&#8217;ve published some time ago, then start experimenting on your own.
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
                <a title="Double USM - #2 Features " href="/2013/01/double-usm-photoshop-sharpening-script-2-features" target="_blank">Features (interface, functionality and Batch processing)</a>
              </li>
              <li>
                Examples (case studies) <strong><- you&#8217;re here!</strong>
              </li>
            </ol>

            <p>
              Double USM v2 is available for sale on my website <a href="http://cc-extensions.com/products/doubleusm/">CC-Extensions</a>. </div> </div> </div> <!-- Share-Widget Button BEGIN --><a href="javascript:void(0);" myshare_id="mys_shareit" myshare_url="/2013/01/double-usm-photoshop-sharpening-script-3-examples/" myshare_title="Double USM #3: Examples" rel="nofollow" onclick=" return false;" style="text-decoration:none; color:#000000; font-size:11px; line-height:20px;">
              
