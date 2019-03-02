---
id: 1370
title: 'Capture One 7 DB &#8211; Lens Correction bug'
date: 2012-11-08T12:37:52+01:00
author: Davide Barranca
excerpt: Capture One 7 looks like a good improvement over the previous version, but shows a nasty bug when you use a particular combination of lens corrections (with raw files coming from P1 digital back at least). Be aware!
layout: post
guid: http://www.davidebarranca.com/?p=1370
permalink: /2012/11/capture-one-7-db-lens-correction-bug/
standard_seo_post_level_layout:
  - ""
standard_link_url_field:
  - ""
standard_seo_post_meta_description:
  - Capture One 7 DB shows a bug when you couple Sharpness Falloff with Distortion Correction in the Lens Correction tab (IIQ raw from P1 back)
image: /wp-content/uploads/2012/11/PhaseOneLensCorrection.png
category:
  - Photography Post-Production
tags:
  - bug
  - Capture One
  - issue
  - Lens Correction
  - Phase One
---
<div class="pf-content">
  <p>
    <img class="alignleft size-full wp-image-1371" title="PhaseOne LensCorrection" src="/wp-content/uploads/2012/11/PhaseOneLensCorrection.png" alt="PhaseOne LensCorrection" width="339" height="202" srcset="/wp-content/uploads/2012/11/PhaseOneLensCorrection.png 339w, /wp-content/uploads/2012/11/PhaseOneLensCorrection-150x89.png 150w, /wp-content/uploads/2012/11/PhaseOneLensCorrection-300x178.png 300w" sizes="(max-width: 339px) 100vw, 339px" />The recently released Capture One Pro 7.0 software (from PhaseOne) shows a bug when dealing with Lens Correction &#8211; particularly when you couple<strong> Sharpness Falloff</strong> with <strong>Distortion Correction</strong> only when the Process engine is the latest, version 7 (at least with raws coming from P1 digital backs &#8211; I&#8217;ve tested this on IQ180).
  </p>

  <p>
    This has been confirmed by the (incredibly fast, kudos to them!) Phase One tech support, so be aware of it. You need to inspect your images (the exported TIFF) looking for artifacts such as the following white stripes that appear near the corners, up to a third of the image&#8217;s width.
  </p>

  <p>
    The issue may or may not shows even in the Capture One preview, depending on the OpenCL settings and your video card (but who cares, the important thing is that the exported files are affected).
  </p>

  <p>
    Waiting for a bugfix update!
  </p>

  <p style="text-align: center;">
    <a href="/wp-content/uploads/2012/11/CaptureOne7_Issue.jpg" target="_blank"><img class="aligncenter size-full wp-image-1372" title="CaptureOne7 Lens Correction Issue" src="/wp-content/uploads/2012/11/CaptureOne7_Issue.jpg" alt="CaptureOne7 Lens Correction Issue" width="584" height="748" srcset="/wp-content/uploads/2012/11/CaptureOne7_Issue.jpg 584w, /wp-content/uploads/2012/11/CaptureOne7_Issue-150x192.jpg 150w, /wp-content/uploads/2012/11/CaptureOne7_Issue-234x300.jpg 234w" sizes="(max-width: 584px) 100vw, 584px" /></a>
  </p>
</div>
