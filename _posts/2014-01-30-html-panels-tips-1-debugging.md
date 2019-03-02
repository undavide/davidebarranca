---
title: 'HTML Panels Tips: #1 Debugging'
date: 2014-01-30T22:37:32+01:00
author: Davide Barranca
excerpt: "Since latest updates, Photoshop CC (and possibly other Adobe apps too) need remote connecting for debugging"
layout: post
permalink: /2014/01/html-panels-tips-1-debugging/
description: "Since latest updates, Photoshop CC (and possibly other Adobe apps too) need remote connecting for debugging"
image: /wp-content/uploads/2014/01/ChromeDebuggingTool.jpg
categories:
  - CEP
tags:
  - HTML Panels Tips
---

Since latest updates, Photoshop (and possibly other Adobe apps too) need remote connecting for debugging HTML Extensions - here's a quick how-to.

![.debug file](/wp-content/uploads/2014/01/debug.png)

In the root folder of your extension, create a blank `.debug` file (you may need to make your OS show hidden files - [download here](/wp-content/uploads/2014/01/Toggle-Hidden-Files.app_.zip) an Automator app for OSX that toggles the visibility on/off). This file needs to be formatted as an XML and should contain information about the ID of your extension, and the host/port (in the range 1024 - 65534) you want to connect through.

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<ExtensionList>
  <Extension Id="com.example.communication">
    <HostList>

      <!-- Comment Host tags according to the apps you want your panel to support -->

      <!-- Photoshop -->
      <Host Name="PHXS" Port="8088"/>

      <!-- Illustrator -->
      <Host Name="ILST" Port="8088"/>

      <!-- InDesign -->
      <Host Name="IDSN" Port="8088"/>

      <!-- Premiere -->
      <Host Name="PPRO" Port="8088"/>

      <!-- AfterEffects -->
      <Host Name="AEFT" Port="8088"/>

      <!-- PRELUDE -->
      <Host Name="PRLD" Port="8088"/>

      <!-- FLASH Pro -->
      <Host Name="FLPR" Port="8088"/>

    </HostList>
  </Extension>
</ExtensionList>
{% endhighlight %}

In my case the ID is `com.example.communication` and the port `8088`.

![localhost](/wp-content/uploads/2014/01/localhost.png)

Now, while your Photoshop extension is open, point your browser (either Chrome, Safari, Firefox...) to `http://localhost:8088` (of course use the port you've set in the XML file). You should see something like the screenshot on the left. If you click the index.html link the Chrome Developer Tools or the Safari/Firefox equivalent will open and you'll be able to inspect your extension like any other webpage on the internet. Mind you, as far as I know this debug method doesn't reach the JSX level - it's just for the HTML panel and its JS; if you're wondering a `debugger;` call in the JSX doesn't fire up ESTK. 

![Chrome Debugging Tool](/wp-content/uploads/2014/01/ChromeDebuggingTool.jpg)

{% include_relative cepBook.md %}
