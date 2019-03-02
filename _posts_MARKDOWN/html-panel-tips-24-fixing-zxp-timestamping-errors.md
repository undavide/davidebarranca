---
title: " HTML Panel Tips #24: Fixing ZXP Timestamping errors\t\t"
tags:
  - signing
  - timestamp
  - ZXP
  - ZXPSignCmd
url: 3137.html
id: 3137
category:
  - Coding
  - HTML Panels
date: 2017-04-29 01:46:45
---

Recently, running the `ZXPSignCmd` command line utility to sign and timestamp HTML Panels has proved to cause errors – also for users of Adobe Configurator 4, which relies on it behind the scenes to export the (Flash) panel as ZXP. Quick fix as follows. Hammer it! And if it doesn't work, change the timestamp authority. We've been using `https://timestamp.geotrust.com/tsa` for quite some time now. Is its recent failure linked to the SHA-1 deprecation? I can't say, but you can try a different service from this list – that comes from the small but always great CEP (HTML) Panels developers community.

*   `http://tsa.starfieldtech.com`
*   `http://sha256timestamp.ws.symantec.com/sha256/timestamp`
*   `http://timestamp.comodoca.com`
*   `http://timestamp.verisign.com/scripts/timstamp.dll`
*   `http://timestamp.digicert.com`
*   `http://time.certum.pl`

I assume you know how to use it. If this is not the case, check out my previous posts: [here](http://localhost:8888/2014/05/html-panels-tips-10-packaging-zxp-installers/) and also [here](http://localhost:8888/2014/08/html-panels-tips-13-automate-zxp-packaging-with-gulp-js/). Few notes: first, it might be that in the future Geotrust will be working again (I'll update the post, if/when). Second, I've tried a mild Configurator 4 hack, but without any success – I guess the `ZXPSignCmd` call is somewhere out of my reach – if you find a way to change the timestamp authority there, or if you want to suggest other timestemp URLs, please let me know in the comments! Third, make sure to have the latest version of the `ZXPSignCmd`, which you can download [here](https://github.com/Adobe-CEP/CEP-Resources/tree/master/ZXPSignCMD).

* * *

### The Photoshop HTML Panels Development Course

[![Photoshop HTML Panels Development course](http://localhost:8888/wp-content/uploads/2016/03/BookVideo-300x193.jpg)](http://htmlpanelsbook.com "Photoshop HTML Panels Development course") If you're reading here, you might be interested in Photoshop Panels development – so let me inform you that I've authored a full course:

*   **300 pages** PDF
*   **3 hours** of HD screencasts
*   **28 custom Panels** with commented code

[Check it out!](http://htmlpanelsbook.com) Please help me spreading the news – I'll be able to keep producing more exclusive content on this blog. Thank you!