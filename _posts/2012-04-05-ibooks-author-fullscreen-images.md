---
id: 811
title: 'iBooks Author &#8211; Fullscreen images'
date: 2012-04-05T14:26:22+01:00
author: Davide Barranca
excerpt: "To make a picture zoom fullscreen in iBooks Author v1.1 isn't as easy as it could be - there are at least three ways to do it, each one with its own drawbacks."
layout: post
guid: http://www.davidebarranca.com/?p=811
permalink: /2012/04/ibooks-author-fullscreen-images/
standard_seo_post_level_layout:
  - ""
standard_link_url_field:
  - ""
standard_seo_post_meta_description:
  - There are three ways to make a fullscreen image in iBooks (made with iBooksAuthor), read along to find pros and cons of each one.
image: /wp-content/uploads/2012/04/iBooksAuthor-fullscreen.jpg
category:
  - Digital Publishing
  - iBooksAuthor
tags:
  - borderless
  - gallery widget
  - iBooks
  - image
  - image widget
  - interactive image widget
---
<div class="pf-content">
  <p>
    <img style="border-width: 0px;" alt="IBooksAuthor fullscreen" src="/wp-content/uploads/2012/04/iBooksAuthor-fullscreen.jpg" width="570" height="447" /> One of the most interesting yet obvious features of iBooks is the possibility to make an image to pop up fullscreen, so that any small illustration may be appreciated as big as the iPad display allows &#8211; and even more, zooming in. Alas, there are at least three ways to do this in iBooks Author, each one with its own drawbacks: because fullscreen and borderless aren&#8217;t synonyms.
  </p>

  <p>
    <!--more-->
  </p>

  <p>
    <img style="float: left; border-width: 0px;" alt="IBooksAuthor - Insert Image" src="/wp-content/uploads/2012/04/iBooksAuthor-insertImage.jpg" width="300" height="427" />
  </p>

  <h3>
    Images
  </h3>

  <p>
    You&#8217;ve probably noticed that there&#8217;s no menu item for <em>Insert &#8211; Image</em>. You&#8217;re allowed to drag and drop a JPG or PNG (if the transparency matters to you) either from the Finder or the Media panel, or<em> Choose…</em> one (Command Shift V). Just like in <em>Pages</em>, you can resize it, move it, etc. Yet the picture won&#8217;t allow any kind of zoom/pinch: it will be a static image, merged within the background, when exported to the iPad. Users may tap or double tap on it without any effect. In order to add interactivity, the image has to be turned into a <em>Widget</em>.
  </p>

  <h3>
    Image Widget
  </h3>

  <p>
    <img class="alignright size-full wp-image-828" style="border-width: 0px;" alt="Inspector - Widget" src="/wp-content/uploads/2012/04/InspectorWidget.png" width="230" height="422" srcset="/wp-content/uploads/2012/04/InspectorWidget.png 230w, /wp-content/uploads/2012/04/InspectorWidget-150x275.png 150w, /wp-content/uploads/2012/04/InspectorWidget-163x300.png 163w" sizes="(max-width: 230px) 100vw, 230px" />Back in iBooks Author, select your image and open the <em>Widget</em> tab in the <em>Inspector</em> (if the Inspector is not visible as a floating palette, click on the white &#8220;i&#8221; on the blue circle icon, which is in the Toolbar).
  </p>

  <p>
    Check either the <em>Title</em> or the <em>Caption</em> checkbox (or both) to transform the Image into an <em>Image Widget</em>. If you are willing to, you may <em>Label</em> the <em>Title</em> via its own drop-down menu and choose from a long list the right definition (Diagram, Figure, Gallery, Illustration, Image, Interactive, Movie, Review), enabling the automatic numbering (Fig 1.1 for instance).
  </p>

  <p>
    If you&#8217;re not interesting in having a Title or a Caption, just delete the text in the editor and leave them blank. Mind you, the Label isn&#8217;t related to the kind of widget (you may have an Image Widget labeled Movie and everything will be ok anyway)
  </p>

  <p>
    <img style="float: right;" alt="InspectorFullscreen" src="/wp-content/uploads/2012/04/InspectorFullscreen1.png" width="230" height="110" border="0" />
  </p>

  <p>
    The Interaction tab (just besides the Layout one) shows a promising <em>Full-screen only</em> checkbox. Each and every tutorial or post I&#8217;ve read suggest you to check it: do it, it makes no difference at all. Actually I&#8217;ve not understood what is it for, since it appears to have no effect in the exported iBook.
  </p>

  <p>
    On the iPad the image will pop up fullscreen when double-tapped:
  </p>

  <p>
    <img style="margin-left: auto; margin-right: auto; border-width: 0px;" alt="IBooksAuthor semiFullscreen" src="/wp-content/uploads/2012/04/iBooksAuthor-semiFullscreen.jpg" width="570" height="447" border="0" />
  </p>

  <p>
    Yet, you see that while the image is <em>fullscreen</em>, it&#8217;s not <em>borderless</em>. Actually, if for some reason you don&#8217;t want your image to be rescaled in any way, the maximum dimension it will have when displayed fullscreen are <strong>2008 x 1319 px</strong>. A border of 20 px each side is always kept, and the image is put on a white background. I haven&#8217;t found a way to change its color yet.
  </p>

  <h3>
    Gallery widget
  </h3>

  <p>
    An actual fullscreen image can be obtained via the <em>Gallery</em> widget. Click on the <em>Widget</em> button on the toolbar and select <em>Gallery</em>, or from the menu <em>Insert &#8211; Widget &#8211; Gallery</em>. It&#8217;s a collection of pictures with one placeholder within the text (thumbnails for the other images are optional), that you can swipe with one finger either normal size or fullscreen. Images share the same title, while customized captions are optional. Sounds great right? You may build a <em>Gallery</em> with one image only, this is how it looks like (a true borderless fullscreen!)
  </p>

  <p>
    <img style="margin-left: auto; margin-right: auto; border-width: 0px;" alt="IBooksAuthor GalleryFullscreen" src="/wp-content/uploads/2012/04/iBooksAuthor-GalleryFullscreen.jpg" width="570" height="447" border="0" />
  </p>

  <p>
    One tap on the image and both <em>Title</em> and <em>Caption</em> will disappear. So what&#8217;s wrong with the <em>Gallery</em> widget?
  </p>

  <p>
    <img style="float: left; border-width: 0px;" alt="IBooksAuthor GalleryWidget" src="/wp-content/uploads/2012/04/iBooksAuthor-GalleryWidget.jpg" width="300" height="285" border="0" />Not the fact that the title is <em>Gallery</em> even if it contains just one image (it can be changed via the Label drop-down menu). The screenshot at the left is from iBooks Author, and shows two grayed out arrows that are for navigating the pictures.
  </p>

  <p>
    They don&#8217;t appear on the iPad if the <em>Gallery</em> contains a single image, yet the whole widget ends up having a wasted, empty, extra white space at the bottom that ruins the overall look of the page. At least to me, and I&#8217;ve got to live with me.
  </p>

  <p>
    So far, this is the best solution to display a borderless fullscreen image with optional title and caption, but with one design drawback on the book page.
  </p>

  <h3>
    Interactive Image widget
  </h3>

  <p>
    This one is for zooming the image into predefined hot-spots, that usually contains textual captions: just like you can build an <em>Gallery</em> with a single image, you&#8217;re allowed to add an <em>Interactive Image</em> widget without any extra view or hot-spot. So add the widget and delete the views in the <em>Widget</em> tab of the <em>Inspector</em> panel. This is the fullscreen outcome:
  </p>

  <p>
    <img style="margin-left: auto; margin-right: auto; border-width: 0px;" alt="IBooksAuthor InteractiveImage" src="/wp-content/uploads/2012/04/iBooksAuthor-InteractiveImage.jpg" width="570" height="447" border="0" />
  </p>

  <p>
    It shows no title and no caption &#8211; that may be useful to have.
  </p>

  <p>
    <img style="float: left; border-width: 0px;" alt="IBooksAuthor InteractiveImageProblem" src="/wp-content/uploads/2012/04/iBooksAuthor-InteractiveImageProblem.jpg" width="300" height="327" border="0" />
  </p>

  <p>
    Yet the biggest issue to me is that, in iBooks Author, it&#8217;s almost impossible to make the small image to fit the widget&#8217;s box (tweaking the zoom), avoiding the ugly white bar to appear above and below the image. With the same picture I&#8217;ve been able to do it successfully once, then in order to grab a screenshot for this blogpost, there&#8217;s no way I could do it again. As a suggestion, remember to click the blue <em>Set View</em> button, otherwise the widget will zoom the image back to its default value.
  </p>

  <p>
    I&#8217;ve been playing with iBooks Author for few days, and I admit it&#8217;s a beautiful piece of software, being at the version 1.1. It has a lot of bugs: the most annoying one, in my opinion, is the <del datetime="2012-04-08T14:47:34+00:00">broken media placeholder</del> (<strong>UPDATE</strong>: in order to successfully transform a picture/movie within a master page into a placeholder you must select it, then 1) <em>Format &#8211; Advanced &#8211; Define as Media Placeholder</em>. 2) From the <em>Inspector</em> panel, select the <em>Layout inspector</em> and below Layout Object select the <em>Editable on pages using this layout</em> checkbox. If you forget this checkbox, the placeholder won&#8217;t work).
  </p>

  <p>
    I hope future releases will fix the issues, and above all, I hope we&#8217;ll see point updates to appear frequently &#8211; for iBooks Author is truly an amazing software!
  </p>

  <p>
    &nbsp;
  </p>

  <p>
    <em>[The image is courtesy <a href="http://www.bigano.com" target="_blank">Roberto Bigano</a>]</em>
  </p>
</div>
