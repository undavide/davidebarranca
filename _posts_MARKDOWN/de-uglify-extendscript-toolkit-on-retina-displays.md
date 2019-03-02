---
title: " De-uglify ExtendScript Toolkit on Retina Displays\t\t"
tags:
  - ESTK
  - ExtendScript Toolkit
  - Retina
url: 2883.html
id: 2883
category:
  - Coding
  - ExtendScript / Javascript
date: 2015-09-20 22:06:47
---

I'm no big fan of ESTK for a variety of reasons; when I finally replaced my old MacBookPro with a newer model with Retina display I got even more disappointed: pixels everywhere! There's a quick fix/hack that I'd like to show you (also as a reminder for my future self, just in case ESTK is still going to be around in 2021 when I'll get a newer Mac). You may want to right-click the app, select Info and uncheck the "Open in Low Resolution" checkbox, alas it is grayed out. Don't worry.

*   Grab the ExtendScript Toolkit.app, right click it and select **"Show Package content"**.
*   Go to **Content** \> **Info.plist** and open it with the text editor of your choice.
*   Go to the line before the closing `</dict>` tag and add the following two highlighted lines:

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <!\-\- LOT OF STUFF HERE... -->
    <key>NSHighResolutionCapable</key>
    <true/>
</dict>
</plist>

*   Save and close the Info.plist file, then open the Terminal and:

touch /Applications/Adobe\ ExtendScript\ Toolkit\ CC/ExtendScript\ Toolkit.app

(hint: type `touch`  and then drag the ESTK.app in the Terminal window to save you typing the rest - we're all born lazy). Now voilà, you open ESTK: text is crisp and clear and confetti fall from above. Cheers!