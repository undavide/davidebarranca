---
title: 'HTML Panel Tips #24: Fixing ZXP Timestamping errors'
date: 2017-04-29T01:46:45+01:00
author: Davide Barranca
excerpt: Recently, running the ZXPSignCmd command line utility to sign and timestamp HTML Panels has proved to cause errors – also for users of Adobe Configurator 4, which relies on it behind the scenes to export the (Flash) panel as ZXP. Quick fix as follows.
layout: post
permalink: /2017/04/html-panel-tips-24-fixing-zxp-timestamping-errors/
description: Fix the recent ZXPSignCmd error when building ZXP packages using a different timestamp authority
image: /wp-content/uploads/2017/04/bug.png
category:
  - CEP
tags:
  - timestamp
  - ZXPSignCmd
  - HTML Panels Tips
---

Recently, running the `ZXPSignCmd` command line utility to sign and timestamp HTML Panels has proved to cause errors. The problem lies in the Timestamp Authority.

Adobe Configurator 4, which relies on it behind the scenes to export the (Flash) panel as ZXP is also affected. Quick fix as follows: hammer it! And if it doesn't work, change the timestamp authority. We've been using `https://timestamp.geotrust.com/tsa` for quite some time now. Is its recent failure linked to the SHA-1 deprecation? I can't say, but you can try a different service from this list – that comes from the small but always great CEP (HTML) Panels developers community.

*   `http://tsa.starfieldtech.com`
*   `http://sha256timestamp.ws.symantec.com/sha256/timestamp`
*   `http://timestamp.comodoca.com`
*   `http://timestamp.verisign.com/scripts/timstamp.dll`
*   `http://timestamp.digicert.com`
*   `http://time.certum.pl`
*   `http://timestamp.apple.com/ts01`
*   `http://timestamp.globalsign.com/scripts/timstamp.dll`
*   `http://timestamp.sectigo.com`

I assume you know how to use it. If this is not the case, check out my previous posts: [here](/2014/05/html-panels-tips-10-packaging-zxp-installers/) and also [here](/2014/08/html-panels-tips-13-automate-zxp-packaging-with-gulp-js/). Few notes: first, it might be that in the future Geotrust will be working again (I'll update the post, if/when).

Second, I've tried a mild Configurator 4 hack, but without any success – I guess the `ZXPSignCmd` call is somewhere out of my reach – if you find a way to change the timestamp authority there, or if you want to suggest other timestemp URLs, please let me know in the comments!

Third, make sure to have the latest version of the `ZXPSignCmd`, which you can download [here](https://github.com/Adobe-CEP/CEP-Resources/tree/master/ZXPSignCMD).

{% include_relative cepBook.md %}
