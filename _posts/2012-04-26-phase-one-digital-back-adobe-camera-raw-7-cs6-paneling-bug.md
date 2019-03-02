---
id: 916
title: 'Phase One digital backs and Adobe Camera Raw &#8211; Paneling &#8220;bug&#8221;'
date: 2012-04-26T10:07:01+01:00
author: Davide Barranca
excerpt: |
  Phase One digital back raw files are not fully supported by Adobe Camera Raw (all versions) because the sensor calibration data embedded in the .IIQ files is not interpreted. This leads to a so-called paneling "bug", affecting the sensor's response uniformity - I came up with a workaround.
layout: post
guid: http://www.davidebarranca.com/?p=916
permalink: /2012/04/phase-one-digital-back-adobe-camera-raw-7-cs6-paneling-bug/
standard_seo_post_level_layout:
  - ""
standard_link_url_field:
  - ""
standard_seo_post_meta_description:
  - Phase One digital back .IIQ raw are not fully supported by ACR because some sensor calibration data embedded in the files is not interpreted
image: /wp-content/uploads/2012/04/PhaseOne_IQ180_paneling_bug3.jpg
category:
  - Photography Post-Production
tags:
  - Adobe Camera Raw
  - bug
  - Capture One
  - IQ180
  - P65+
  - Phase One
  - Photoshop CS6
---
<div class="pf-content">
  <div itemscope="" itemtype="http://schema.org/BlogPosting">
    <meta itemprop="author" content="Davide Barranca" />

    <br />

    <meta itemprop="dateCreated" />
    <time datetime="2012-04-26"></time>&nbsp;</p>

    <p>
      <img class="alignnone size-full wp-image-922" style="border-style: initial; border-color: initial; border-width: 0px;" title="PhaseOne IQ180 paneling bug" alt="PhaseOne IQ180 paneling bug" src="/wp-content/uploads/2012/04/PhaseOne_IQ180_paneling_bug.jpg" width="570" height="428" srcset="/wp-content/uploads/2012/04/PhaseOne_IQ180_paneling_bug.jpg 570w, /wp-content/uploads/2012/04/PhaseOne_IQ180_paneling_bug-150x112.jpg 150w, /wp-content/uploads/2012/04/PhaseOne_IQ180_paneling_bug-300x225.jpg 300w" sizes="(max-width: 570px) 100vw, 570px" />
    </p>

    <p>
      If you&#8217;re a Phase One digital back owner (IQ and P+ series) you&#8217;ve been told that <span itemprop="about" itemscope="" itemtype="http://schema.org/SoftwareApplication"><span itemprop="name">Phase One Capture One Pro</span></span> is the main choice for raw processing. Finest quality, and above all they make the hardware, so they&#8217;re supposed to code the best software to drive it.
    </p>

    <p>
      I&#8217;m afraid I don&#8217;t share the same confidence of Capture One passionate supporters: ever since Adobe distributed the new <span itemprop="mentions" itemscope="" itemtype="http://schema.org/SoftwareApplication"><span itemprop="name">Adobe Camera Raw 7.0</span></span> (shipped with <span itemprop="mentions" itemscope="" itemtype="http://schema.org/SoftwareApplication"><span itemprop="name">Photoshop CS6</span></span>, which technology is embedded in Lightroom 4) we finally have an excellent alternative &#8211; I would say a far better choice under many key aspects. However, <strong>there&#8217;s a hidden, annoying pitfall that you must be aware of</strong> &#8211; friendly called the <em><span itemprop="name">IIQ paneling &#8220;bug&#8221;</span>.</em>
    </p>

    <p>
      <!--more-->
    </p>

    <h3>
      What is the part of <em>&#8220;not officially supported&#8221;</em> that you don&#8217;t understand?
    </h3>

    <p>
      <img class="alignleft  wp-image-924" itemprop="thumbnailUrl" style="border-style: initial; border-color: initial; border-width: 0px;" title="IIQ icon" alt="IIQ icon" src="/wp-content/uploads/2012/04/IIQ_icon.png" width="135" height="169" />That&#8217;s what I&#8217;ve been told once, and the man was right somehow. To open Phase One .IIQ raw files with Adobe Camera Raw (ACR from now on)is a bad thing to do, theoretically, because they were: <em>&#8220;not officially supported&#8221;.</em> What does this mean, it&#8217;s a fair question to ask, you may wonder, since ACR seems to read and open them flawlessly just like any other CR2, NEF, DNG file.
    </p>

    <p>
      Wrong: it happens that <strong>.IIQ files contain sensor calibration data that is not correctly interpreted by ACR</strong>, or not interpreted at all. This data, as far as I can see, is used to <strong>balance the response of the sensor</strong>, which is geometrically divided into 8 sections (2 rows, 4 columns). Whether the sensor is controlled by different pieces of electronics, or it is made with smaller sensors glued together I don&#8217;t know and frankly I don&#8217;t care.
    </p>

    <p>
      &nbsp;
    </p>

    <div id="attachment_929" style="width: 560px" class="wp-caption aligncenter">
      <img aria-describedby="caption-attachment-929" class="size-full wp-image-929" title="PhaseOne P65+ paneling bug" alt="PhaseOne P65+ paneling bug" src="/wp-content/uploads/2012/04/PhaseOne_P65_paneling_bug.jpg" width="550" height="363" srcset="/wp-content/uploads/2012/04/PhaseOne_P65_paneling_bug.jpg 550w, /wp-content/uploads/2012/04/PhaseOne_P65_paneling_bug-150x98.jpg 150w, /wp-content/uploads/2012/04/PhaseOne_P65_paneling_bug-300x197.jpg 300w" sizes="(max-width: 550px) 100vw, 550px" />

      <p id="caption-attachment-929" class="wp-caption-text">
        PhaseOne P65+ test shot. Left: RGB version. Right: the &#8220;a&#8221; channel from Lab equalized shows paneling.
      </p>
    </div>

    <p>
      &nbsp;
    </p>

    <div id="attachment_932" style="width: 560px" class="wp-caption aligncenter">
      <img aria-describedby="caption-attachment-932" class="size-full wp-image-932" title="PhaseOne IQ180 paneling bug" alt="PhaseOne IQ180 paneling bug" src="/wp-content/uploads/2012/04/PhaseOne_IQ180_paneling_bug21.jpg" width="550" height="831" srcset="/wp-content/uploads/2012/04/PhaseOne_IQ180_paneling_bug21.jpg 550w, /wp-content/uploads/2012/04/PhaseOne_IQ180_paneling_bug21-150x226.jpg 150w, /wp-content/uploads/2012/04/PhaseOne_IQ180_paneling_bug21-198x300.jpg 198w" sizes="(max-width: 550px) 100vw, 550px" />

      <p id="caption-attachment-932" class="wp-caption-text">
        PhaseOne IQ180 test shot. Up: RGB version, weak green/magenta color casts in the asphalt. Bottom: the &#8220;a&#8221; channel from Lab equalized shows it more clearly
      </p>
    </div>

    <p>
      The crucial fact is that if this calibration is lost (and ACR 7.0 and earlier versions lose it), the raw data will be translated into a faulty, demosaicized image. The differences between each zone can be easily boosted by the usual Photoshop color/contrast enhancing routines that are in your repertoire as a retoucher: and believe me, you won&#8217;t like to end up with an image which has both magenta and green casts displaced in a chessboard fashion: it&#8217;s bad times.
    </p>

    <blockquote>
      <p>
        Mind you: this problem doesn&#8217;t show up using Capture One, which reads and interprets correctly the proprietary tags embedded within the .IIQ files.
      </p>
    </blockquote>

    <p>
      Right now both Photoshop CS6 and Lightroom 4 <a title="Supported cameras for the Camera Raw plug-in and Lightroom" href="http://www.adobe.com/products/photoshop/extend.html" target="_blank">officially support the whole range of Phase One backs</a> yet the problem is there anyway. There&#8217;s an easy way to check whether your raw files are correctly managed by ACR, or they show a so-called <strong>paneling &#8220;bug&#8221;</strong>. It&#8217;s worth to try this one at least once on some of your files (a white wall test shot and a couple of real world pictures):
    </p>

    <ol>
      <li>
        Open the .IIQ file in Adobe Camera Raw, everything zeroed but the White Balance. Bit depth and RGB profile do not matter, your default values are fine.
      </li>
      <li>
        In Photoshop, convert the file to the Lab colorspace (<em>Image &#8211; Mode &#8211; Lab Color</em>). You know Lab, don&#8217;t you?
      </li>
      <li>
        In the <em>Channels</em> palette, make the &#8220;a&#8221; channel only active (Command + 4 on the Mac, CTRL + 4 for PCs, Photoshop CS5 to CS6)
      </li>
      <li>
        <em>Image &#8211; Adjustments &#8211; Equalize</em>.
      </li>
      <li>
        Make the &#8220;b&#8221; channel active (Command + 5 on the Mac, CTRL + 5 for PCs) and repeat the step #4.
      </li>
    </ol>

    <p>
      If either the &#8220;a&#8221; and/or the &#8220;b&#8221; channels show some evident grid, your files are <em>unofficially unsupported</em> in ACR! Sometimes the grid is not so obvious, yet you may find some oddities like the one below, which are bad times anyway.
    </p>

    <p>
      &nbsp;
    </p>

    <div id="attachment_936" style="width: 560px" class="wp-caption aligncenter">
      <img aria-describedby="caption-attachment-936" class="size-full wp-image-936" itemprop="image" title="PhaseOne IQ180 paneling bug" alt="PhaseOne IQ180 paneling bug" src="/wp-content/uploads/2012/04/PhaseOne_IQ180_paneling_bug3.jpg" width="550" height="531" srcset="/wp-content/uploads/2012/04/PhaseOne_IQ180_paneling_bug3.jpg 550w, /wp-content/uploads/2012/04/PhaseOne_IQ180_paneling_bug3-150x144.jpg 150w, /wp-content/uploads/2012/04/PhaseOne_IQ180_paneling_bug3-300x289.jpg 300w" sizes="(max-width: 550px) 100vw, 550px" />

      <p id="caption-attachment-936" class="wp-caption-text">
        Phase One IQ180. Up: sky shot with noticeable color paneling (magenta cast in the left side). Bottom: the &#8220;a&#8221; channel from Lab equalized shows a weird paneling pattern
      </p>
    </div>

    <p>
      &nbsp;
    </p>

    <div id="attachment_938" style="width: 117px" class="wp-caption alignleft">
      <a href="/wp-content/uploads/2012/04/PhaseOne_IQ180_paneling_bug3_PART.jpg" target="_blank"><img aria-describedby="caption-attachment-938" class="size-medium wp-image-938 " style="border-style: initial; border-color: initial; border-width: 0px;" title="PhaseOne IQ180 paneling bug - particular" alt="PhaseOne IQ180 paneling bug - particular" src="/wp-content/uploads/2012/04/PhaseOne_IQ180_paneling_bug3_PART-107x300.jpg" width="107" height="300" srcset="/wp-content/uploads/2012/04/PhaseOne_IQ180_paneling_bug3_PART-107x300.jpg 107w, /wp-content/uploads/2012/04/PhaseOne_IQ180_paneling_bug3_PART-150x419.jpg 150w, /wp-content/uploads/2012/04/PhaseOne_IQ180_paneling_bug3_PART.jpg 257w" sizes="(max-width: 107px) 100vw, 107px" /></a>

      <p id="caption-attachment-938" class="wp-caption-text">
        Unedited crop, click to enlarge
      </p>
    </div>

    <p>
      As a side note, I know that the above mentioned moves are an extreme way to brutalize an image for testing purposes &#8211; yet Lab channel stretching is fairly used among retouchers, and the <em>Equalize</em> command is actually employed in the so-called <em>Modern Man from Mars</em> technique by Dan Margulis, a fundamental step in his <a title="The Picture Postcard Workflow Panel" href="http://www.ledet.com/margulis/ppw" target="_blank">Picture Postcard Workflow</a>, aka PPW).
    </p>

    <p>
      I&#8217;ve been personally reported by a Phase One user that the same paneling may appear even using Capture One as the one and only raw developer. That is, your own digital back&#8217;s internal calibration data doesn&#8217;t correspond to the actual sensor&#8217;s response no more, which sadly means: send it to Denmark for revision. (That is what he did and luckily the problem has been solved flawlessly)
    </p>

    <h3 style="font-size: 1.17em;">
      Do open .IIQ files with Adobe Camera Raw anyway
    </h3>

    <p>
      I sincerely do not like Capture One Pro (also known as C1 Pro). It isn&#8217;t a polite thing to write in some forums (people seem to polarize when it comes to religion, food and raw processors), but nobody hears me now so I can tell you: it&#8217;s a bad piece of software, clumsy, so slow, involved, unnecessarily complex, UI and controls are badly designed (try to move a point on a channel&#8217;s curve while keeping self control), even from the sheer algorithms point of view ACR 7.0 surpasses it without any doubt. That&#8217;s my own, personal point of view of course &#8211; and I&#8217;m not a big Adobe&#8217;s fan, usually.
    </p>

    <p>
      &nbsp;
    </p>

    <div id="attachment_947" style="width: 230px" class="wp-caption alignright">
      <a href="/wp-content/uploads/2012/04/CaptureOne_Curves.png"><img aria-describedby="caption-attachment-947" class="size-full wp-image-947" title="CaptureOne Curves" alt="CaptureOne Curves" src="/wp-content/uploads/2012/04/CaptureOne_Curves.png" width="220" height="165" srcset="/wp-content/uploads/2012/04/CaptureOne_Curves.png 220w, /wp-content/uploads/2012/04/CaptureOne_Curves-150x112.png 150w" sizes="(max-width: 220px) 100vw, 220px" /></a>

      <p id="caption-attachment-947" class="wp-caption-text">
        Capture One curves dialog
      </p>
    </div>

    <p>
      Mind you: C1 image quality is quite good (yet I still prefer ACR most of the times) but it comes with a price &#8211; way too high: in terms of usability, mainly. If I had no alternatives, I would patience and adapt to C1 cumbersome structure. But ACR 7.0 delivers a competitive, impressive image quality: noise reduction is superior, general usability is orders of magnitude superior, Local Laplacian filtering (the revised algorithm behind Clarity) is so nice, now that Adobe has implemented R, G, B single channel curves (it&#8217;s been a long wait: hurray!) there&#8217;s no reason not to use it.
    </p>

    <p>
      So I do open .IIQ files with ACR (because I&#8217;m a retoucher, photographers are used to work with Lightroom: that, technically speaking, is the same engine within a different car). How?
    </p>

    <h3>
      Avoid DNG conversions
    </h3>

    <p>
      <strong><img class="alignleft size-full wp-image-955" style="border-style: initial; border-color: initial; border-width: 0px;" title="DNG icon" alt="DNG icon" src="/wp-content/uploads/2012/04/DNG.png" width="150" height="150" />Do not try to export the .IIQ as DNG in CaptureOne</strong> (a task performed at unusual speed compared to C1 standards, I shall say). It&#8217;s been reported that earlier version of C1 messed with some metadata and filesize (inflated as much as the IIQ file&#8217;s double) &#8211; whether it&#8217;s been fixed or not (not so, at least for the filesize), even using an updated Capture One version those <strong>DNG are still not calibrated</strong>, so when you open them in ACR the very same paneling bug shows! This is a serious problem, that from my personal standpoint should be taken into account by Phase One: when users convert to a standard format like DNG, they should be given correct data. I can understand that some proprietary pieces of information may not fit into the DNG specs (the actual spots used by the camera to focus the image for instance, something you could get from Canon DPP as far as I remember), but here we talk about image channels that are intentionally left uncorrected!
    </p>

    <p>
      Just in case you&#8217;re wondering, Adobe DNG converter couldn&#8217;t care less about Phase One sensor calibration data. The only workaround I came up with, well&#8230; it isn&#8217;t exciting, but it works, provided that you&#8217;re ready to lose a little something.
    </p>

    <h3>
      Using Layers and Blending Modes
    </h3>

    <p>
      I&#8217;ve had the opportunity to work extensively with two P65+ and one IQ180 Phase One digital back units (60 and 80 megapixels respectively) in the last three years, so these models are what I have a direct experience of; the following should apply to every P1 back that shows the paneling &#8220;bug&#8221; (please notice that I use quotes for I know it&#8217;s <em>not officially</em> a bug &#8211; yet the fact that a C1 exported DNG file shows paneling makes me call it bug anyway; surrounding the &#8220;bug&#8221; word with quotes sounds like a fair compromise to me).
    </p>

    <p>
      Since the paneling affects &#8220;a&#8221; and &#8220;b&#8221; channels of Lab (more the former, green-magenta, and a bit less the latter, blue-yellow axis, i.e. the chromatic component of the image) and not the L, I came up with a decent workaround processing the raw twice and blend the results as a Luminosity and Color layers &#8211; as follows:
    </p>

    <ol>
      <li>
        Open the .IIQ file in ACR 7.0 (or any ACR version you legally own).
      </li>
      <li>
        Tweak all the parameters to your taste &#8211; don&#8217;t spend too much time on chromatic noise reduction (it will be lost) &#8211; and open it in Photoshop as a Smart Object (shift-click on the <em>Open Image</em> button).
      </li>
    </ol>

    <div>
      <a href="/wp-content/uploads/2012/04/ACR_open_as_SmartObject.png"><img class="aligncenter size-full wp-image-950" style="margin-bottom: 15px;" title="ACR - open as SmartObject" alt="ACR - open as SmartObject" src="/wp-content/uploads/2012/04/ACR_open_as_SmartObject.png" width="483" height="108" srcset="/wp-content/uploads/2012/04/ACR_open_as_SmartObject.png 483w, /wp-content/uploads/2012/04/ACR_open_as_SmartObject-150x33.png 150w, /wp-content/uploads/2012/04/ACR_open_as_SmartObject-300x67.png 300w" sizes="(max-width: 483px) 100vw, 483px" /></a>
    </div>

    <ol start="3">
      <li>
        Open the very same .IIQ file in Capture One.
      </li>
      <li>
        In Capture One, mimic the curves, white balance and everything else you did in ACR except for luminosity noise reduction and sharpening, cross the fingers and save a TIFF (same bit depth and output ICC profile you used in ACR).
      </li>
      <li>
        Open the TIFF in Photoshop, drag & drop on it the Smart Object coming from ACR and set its blending mode as Luminosity. If you keep the Shift key pressed while doing the drag & drop, the imported layer will automatically align.
      </li>
      <li>
        [Optional] Select the two layers and convert them to a single Smart Object (good idea if you need to correct the perspective)
      </li>
    </ol>

    <p>
      <img class="alignleft size-full wp-image-951" style="border-width: 0px;" title="Layers" alt="Layers" src="/wp-content/uploads/2012/04/Layers.png" width="351" height="262" srcset="/wp-content/uploads/2012/04/Layers.png 351w, /wp-content/uploads/2012/04/Layers-150x111.png 150w, /wp-content/uploads/2012/04/Layers-300x223.png 300w" sizes="(max-width: 351px) 100vw, 351px" />So you end up with a two layers document: a background layer which is the Capture One interpretation of your file, and a upper layer which is the ACR version in Luminosity. That is: you use the color from Capture One (input ICC profile, curves, chromatic aberration correction and chromatic noise reduction, etc. etc) and the luminosity from Adobe Camera Raw (with its own noise reduction, Clarity, powerful shadows and highlights recovery, etc).
    </p>

    <p>
      Please note that you can invert the layer&#8217;s order (the ACR below and the CaptureOne above, but in Color blending mode!), it gives the same net result.
    </p>

    <p>
      If you are a color management geek, you may be either happy to choose from Capture One input ICC profiles; or unhappy because it&#8217;s harder to use your ColorChecker Passport as a profiling tool in C1. In both situations, don&#8217;t bother too much. I haven&#8217;t found either profiles as particularly accurate &#8211; and like Dan Margulis once said: if there&#8217;s a problem in a picture, just correct it and the problem disappears.
    </p>

    <h3>
      Conclusions
    </h3>

    <p>
      Adobe Camera Raw 7.0 is an impressive leap forward compared to 6.x version. Its usability is far, far higher compared to Capture One Pro, it delivers first class image quality and cutting edges algorithms. Yet many Phase One digital backs are <em>unofficially unsupported</em> because the sensor calibration data embedded in .IIQ files is not correctly interpreted by ACR; nor the DNGs exported from Capture One undergo any kind of normalization (bug!). As a result, to open a Phase One raw file in Adobe Camera Raw means ending up with an image that contains a geometric, chessboard pattern of opposite color casts, mainly in the green-magenta axis: an almost unnoticeable pattern (sometimes but not always), that may become more apparent easily if the retouch involves some color boosting steps.
    </p>

    <p>
      As a workaround (waiting for Capture One to correct DNG exporting, and/or Adobe to fully support Phase One raw files) I suggest to <strong>process the raw twice</strong> (one with C1, one with ACR) and <strong>merge the two files in Photoshop as Color and Luminosity layers</strong>. It isn&#8217;t a particularly elegant solution, but I&#8217;ve found no alternatives and it works &#8211; possibly&#8230; it takes the best of two worlds. Possibly. 😉
    </p>
  </div>
</div>
