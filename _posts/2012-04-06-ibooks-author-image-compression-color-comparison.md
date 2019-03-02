---
id: 847
title: 'iBooks Author &#8211; Image compression'
date: 2012-04-06T10:01:58+01:00
author: Davide Barranca
excerpt: 'iBooks Author outputs .ibooks files, a zipped package of assets: there, images undergo resizing, a strong JPG compression and some kind of color conversion.'
layout: post
guid: http://www.davidebarranca.com/?p=847
permalink: /2012/04/ibooks-author-image-compression-color-comparison/
standard_seo_post_level_layout:
  - ""
standard_link_url_field:
  - ""
standard_seo_post_meta_description:
  - What kind of JPG compression does iBooksAuthor apply to iBooks images? What happens to color, tone and detail of iBooks embedded pictures?
image: /wp-content/uploads/2012/04/iBooksAuthor-opening1.jpg
categories:
  - Digital Publishing
  - iBooksAuthor
tags:
  - iBooks
  - image
  - iPad
---
<div class="pf-content">
  <p>
    <img style="margin-left: auto; margin-right: auto; border-width: 0px;" alt="IBooksAuthor opening" src="/wp-content/uploads/2012/04/iBooksAuthor-opening1.jpg" width="570" height="450" border="0" />
  </p>

  <p>
    If you&#8217;re serious about image quality, you want to know what happens to your pictures when iBooks Author exports a .iBook file: not only you&#8217;re interested in the ICC profile to use, the allowed filetypes (JPG and PNG), but also what kind of color conversion, JPG compression and resizing is applied to pictures on their way to the iPad. I&#8217;ve run some test and results are… somehow surprising.
  </p>

  <p>
    <!--more-->
  </p>

  <p>
    The iBook format is a proprietary flavor of e-books made by Apple. It&#8217;s not a standard (standards advocates argue), but it&#8217;s exceptionally versatile, easy to produce and fun. May I tell you? I love it. Yet iBooks Author, Apple&#8217;s free tool for authoring iBooks, is not bug-free; some actions aren&#8217;t straightforward (read my previous post about <a title="iBooks Author fullscreen images" href="/2012/04/ibooks-author-fullscreen-images/" target="_blank">fullscreen, borderless images</a>), but it&#8217;s only version 1.1 and we&#8217;re allowed to expect for it a bright future. Let&#8217;s start the analysis.
  </p>

  <h3>
    File Format, ICC profile and size
  </h3>

  <p>
    To make a long story short: use JPG or PNG (which support transparency), convert everything to sRGB, and be aware that all the images (with an exception, read along) will be resized to 2048 x 1536 pixels, the new retina display iPad size. This is what <a href="http://support.apple.com/kb/PH2838" target="_blank">Apple recommends</a>.
  </p>

  <h3>
    How to inspect .iba and .ibook files
  </h3>

  <p>
    <a href="/wp-content/uploads/2012/04/iBookAssets.png" target="_blank"><img class="alignright size-medium wp-image-849" title="iBook Assets" alt="iBook Assets" src="/wp-content/uploads/2012/04/iBookAssets-132x300.png" width="132" height="300" srcset="/wp-content/uploads/2012/04/iBookAssets-132x300.png 132w, /wp-content/uploads/2012/04/iBookAssets.png 341w" sizes="(max-width: 132px) 100vw, 132px" /></a>iBooks Author saves projects as .iba files. When you export for the iPad, it outputs an .ibook file. The .iba package contains a copy of the original assets you&#8217;ve imported, while in the .ibook package things are transformed: resized, converted, compressed. Either ones can be inspected this way:
  </p>

  <ol>
    <li>
      Duplicate the .iba and/or the .ibook files (just to be safe…)
    </li>
    <li>
      Rename them to .zip
    </li>
    <li>
      Unzip them in a folder (be aware that OSX doesn&#8217;t expand them with a double click, I&#8217;ve got to use Stuffit Expander)
    </li>
  </ol>

  <p>
    That&#8217;s it. As you see, you end up with a folder structure with xhtml, css and all the needed assets.
  </p>

  <h3>
    Image dimension comparison
  </h3>

  <p>
    For all the following tests I&#8217;ve used a 2048 x 1536 pixel image, saved as JPG maximum quality, sRGB. When you import a picture in iBooks Author you resize and move it to fit the page&#8217;s design, deciding whether to allow the image to pop up fullscreen or not (more details about the three ways you can choose to do it in my previous post).
  </p>

  <p>
    If the image is not allowed to go fullscreen, the .ibooks package will only contain an exact sized version of the original imported file: that is, if the page holds a 600 x 800 pixels JPG, the .ibook asset will be automatically rescaled as a 600 x 800 pixel JPG.
  </p>

  <p>
    On the contrary, if the image is part of a widget (<em>Image</em> or <em>Gallery</em>) things get weird:
  </p>

  <ul>
    <li>
      Images with a stroke (default is a 1pt line) end up as a <strong>2040 x 1530</strong> px JPG.
    </li>
    <li>
      Images without a stroke end up as a <strong>2048 x 1536</strong> px JPG.
    </li>
  </ul>

  <p>
    <img class="alignleft size-full wp-image-832" title="InspectorFullscreen.png" alt="" src="/wp-content/uploads/2012/04/InspectorFullscreen1.png" width="230" height="110" srcset="/wp-content/uploads/2012/04/InspectorFullscreen1.png 230w, /wp-content/uploads/2012/04/InspectorFullscreen1-150x71.png 150w" sizes="(max-width: 230px) 100vw, 230px" />So applying a stroke actually shrink the .ibooks image dimension. Mind you, the <strong><em>Full-screen only option</em> has no effect whatsoever</strong> on image dimension (on the iPad, a widget image will always go fullscreen), so I suggest you to keep it unchecked. Moreover, an <em>Image</em> widget will only expand fullscreen to maximum <strong>2008 x 1319</strong> pixels (<a title="iBooks Author fullscreen images" href="/2012/04/ibooks-author-fullscreen-images/" target="_blank">see here for the details and examples</a>), while <em>Gallery</em> images are allowed to go both fullscreen and <em>borderless</em> (2048 x 1536).
  </p>

  <p>
    As a last option, if the image is part of an <em>Interactive Image</em> widget (i.e. not only it goes fullscreen, but you&#8217;re allowed to zoom in and explore preset details at higher magnification), the final size will depend on the zoom percentage &#8211; that is, you can feed iBooks Author with a 5000 x 5000 pixels image and expect to have it in the .ibooks assets.
  </p>

  <h3>
    Profile comparison
  </h3>

  <div id="attachment_866" style="width: 190px" class="wp-caption alignleft">
    <a href="http://cdtobie.wordpress.com/2012/03/21/more-answers-about-the-new-ipad-and-color/" target="_blank"><img aria-describedby="caption-attachment-866" class=" wp-image-866 " title="iPad1/iPad2 over iPad3" alt="iPad1/iPad2 over iPad3" src="/wp-content/uploads/2012/04/ipad1-2overiPad3-300x290.jpg" width="180" height="174" srcset="/wp-content/uploads/2012/04/ipad1-2overiPad3-300x290.jpg 300w, /wp-content/uploads/2012/04/ipad1-2overiPad3-150x145.jpg 150w, /wp-content/uploads/2012/04/ipad1-2overiPad3.jpg 476w" sizes="(max-width: 180px) 100vw, 180px" /></a>

    <p id="caption-attachment-866" class="wp-caption-text">
      Gamut comparison of iPad1/iPad2 against the new iPad © C. David Tobie
    </p>
  </div>

  <p>
    If you&#8217;re into color management and don&#8217;t know yet <a href="http://cdtobie.wordpress.com/">C. David Tobie&#8217;s blog</a> (he&#8217;s <em>Global Product Technology Manager &#8211; Imaging Color Solutions, <a href="http://www.datacolor.com/">Datacolor inc.</a></em>) I strongly suggest you to read at least his post series about Color Management and the iPad (<a href="http://cdtobie.wordpress.com/2012/03/21/more-answers-about-the-new-ipad-and-color/">More answers about the new iPad and Color</a> contains the links to them all). He&#8217;s very competent and you&#8217;ll read a lot of original content that you won&#8217;t find elsewhere.
  </p>

  <p>
    To my tests I&#8217;ve confirmed that, even though both the .iba and .ibooks assets contain sRGB tagged images, the exported one appears lighter (hence some secret Apple processing has been going on). I&#8217;ve tested a <a href="http://www.babelcolor.com/main_level/ColorChecker.htm#ColorChecker_images">synthetic ColorChecker image</a>, measuring these differences:
  </p>

  <ul>
    <li>
      about 4 points in the b of Lab in blues and cyans (original has an higher saturation)
    </li>
    <li>
      about 1 point in the a of Lab for yellow and orange
    </li>
    <li>
      a shift towards green in the base color (.ibooks compressed JPG)
    </li>
  </ul>

  <p>
    <img class="alignleft size-full wp-image-870" style="border-style: initial; border-color: initial; border-width: 0px; margin-bottom: 10px;" title="ColorChecker sRGB Comparison" alt="ColorChecker sRGB Comparison" src="/wp-content/uploads/2012/04/ColorChecker_sRGB_COMPARISON.jpg" width="570" height="201" srcset="/wp-content/uploads/2012/04/ColorChecker_sRGB_COMPARISON.jpg 570w, /wp-content/uploads/2012/04/ColorChecker_sRGB_COMPARISON-150x52.jpg 150w, /wp-content/uploads/2012/04/ColorChecker_sRGB_COMPARISON-300x105.jpg 300w" sizes="(max-width: 570px) 100vw, 570px" />Left is original, right is .ibooks asset &#8211; almost unnoticeable. Not something to be obsessed with &#8211; actually the ColorChecker comparison alone is not very much significative, if you test real world photographs the .ibooks JPG version appears constantly lighter.
  </p>

  <p style="text-align: center;">
    <img class="aligncenter size-full wp-image-872" style="border-style: initial; border-color: initial; border-width: 0px; margin-bottom: 10px;" title="iBooksAuthor Profile Comparison" alt="iBooksAuthor Profile Comparison" src="/wp-content/uploads/2012/04/iBooksAuthor-ProfileComparison.jpg" width="570" height="211" srcset="/wp-content/uploads/2012/04/iBooksAuthor-ProfileComparison.jpg 570w, /wp-content/uploads/2012/04/iBooksAuthor-ProfileComparison-150x55.jpg 150w, /wp-content/uploads/2012/04/iBooksAuthor-ProfileComparison-300x111.jpg 300w" sizes="(max-width: 570px) 100vw, 570px" />
  </p>

  <p>
    Again, left is original, right is .ibooks asset &#8211; now you see the difference! To simulate the effect, add a Curves adjustment layer to the original image and raise the middle point of the master curve about +20 points (input: 127, output: 147), this is more or less how the .ibooks file will look like.
  </p>

  <h3>
    Compression comparison
  </h3>

  <p>
    I&#8217;ve imported into iBooks Author a JPG saved for the web in Photoshop CS6, maximum quality: then I&#8217;ve checked the .ibooks JPG against the .iba (which is identical to the original). As you can see by yourself, there&#8217;s a lot of JPG compression and artifacts in the .ibooks file: and it isn&#8217;t shocking, because the .iba JPG is 1.2MB, while the .ibooks JPG is 560KB (less than half of the filesize!).
  </p>

  <p>
    What kind of JPG compression has been used? To test it, I&#8217;ve put together a simple Photoshop script to save for the web 101 JPGs from the same original, with Quality ranging from 0 to 100. To the best of my eyesight, I&#8217;ve identified the compression just below 50: yet it isn&#8217;t identical &#8211; for instance, in the .ibook version there&#8217;s a lot more dithering around the edges, therefore that it may be that some quality filter based on image content has been used &#8211; just like when in Photoshop you set an alpha channel to determine the compression range.
  </p>

  <div id="attachment_876" style="width: 580px" class="wp-caption aligncenter">
    <a href="/wp-content/uploads/2012/04/iBooksAuthor-CompressionComparison.jpg" target="_blank"><img aria-describedby="caption-attachment-876" class=" wp-image-876 " title="iBooksAuthor - JPG Compression Comparison" alt="iBooksAuthor - JPG Compression Comparison" src="/wp-content/uploads/2012/04/iBooksAuthor-CompressionComparison.jpg" width="570" height="333" srcset="/wp-content/uploads/2012/04/iBooksAuthor-CompressionComparison.jpg 907w, /wp-content/uploads/2012/04/iBooksAuthor-CompressionComparison-150x87.jpg 150w, /wp-content/uploads/2012/04/iBooksAuthor-CompressionComparison-300x175.jpg 300w" sizes="(max-width: 570px) 100vw, 570px" /></a>

    <p id="caption-attachment-876" class="wp-caption-text">
      Left: the original imported JPG (quality 100); middle, a JPG quality 48; right, the JPG from .ibooks assets (unknown JPG compression). Click the image to get a larger version.
    </p>
  </div>

  <p>
    Yet, if you zoom in the .ibook image, it looks like Frankenstein compared to the immaculate beauty of the original, max quality JPG.
  </p>

  <h3>
    Conclusions
  </h3>

  <p>
    Be happy and build your iBooks. Stick to 2048 x 1536 pixels, sRGB, max quality JPGs (unless you plan to use Interactive Image widgets, that may require higher resolution images). Don&#8217;t worry about the conversion, there&#8217;s nothing you can do &#8211; proof your images on the iPad and tweak them in Photoshop accordingly, if they don&#8217;t look good to you. Yes, they&#8217;re horribly compressed and if you peep them 400% it&#8217;s like a sea of JPG artifacts, but you know what? On the iPad retina display they&#8217;re not that much obvious, you can live with them. And by the way you could always export the .ibooks, unzip it, substitute the JPGs with higher quality ones, zip it again, cross the fingers and submit the file for approval to Apple &#8211; if you succeed, please let me know! For sure I won&#8217;t be the first one to hack iBooks and risk to make the big A angry 😉
  </p>
</div>
