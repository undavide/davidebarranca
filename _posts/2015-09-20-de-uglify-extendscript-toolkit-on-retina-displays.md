---
title: De-uglify ExtendScript Toolkit on Retina Displays
date: 2015-09-20T22:06:47+01:00
author: Davide Barranca
excerpt: Quick hack to get rid of pixelated text and finally get crisp fonts in Adobe ExtendScript Toolkit on Retina displays.
layout: post
permalink: /2015/09/de-uglify-extendscript-toolkit-on-retina-displays/
description: Quick hack to get rid of pixelated text and finally get crisp fonts in Adobe ExtendScript Toolkit on Retina displays.
image: /wp-content/uploads/2015/09/ESTK_retina.png
category:
  - Scripting
tags:
  - ESTK
---

I'm no big fan of ESTK for a variety of reasons; when I finally replaced my old MacBookPro with a newer model with Retina display I got even more disappointed: pixels everywhere! There's a quick fix/hack that I'd like to show you (also as a reminder for my future self, just in case ESTK is still going to be around in 2021 when I'll get a newer Mac). You may want to right-click the app, select Info and uncheck the "Open in Low Resolution" checkbox, alas it is grayed out. Don't worry.

*   Grab the ExtendScript Toolkit.app, right click it and select `Show Package content`
*   Go to `Content > Info.plist` and open it with the text editor of your choice.
*   Go to the line before the closing `</dict>` tag and add the following two highlighted lines:

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <!\-\- LOT OF STUFF HERE... -->
    <key>NSHighResolutionCapable</key>
    <true/>
</dict>
</plist>
{% endhighlight %}

*   Save and close the Info.plist file, then open the Terminal and:

{% highlight sh %}
touch /Applications/Adobe\ ExtendScript\ Toolkit\ CC/ExtendScript\ Toolkit.app
{% endhighlight %}

(hint: type `touch`  and then drag the `ExtendScript ToolKit.app` in the Terminal window to save you typing the rest - we're all born lazy). Now voilà, you open ESTK: text is crisp and clear and confetti fall from above. Cheers!
