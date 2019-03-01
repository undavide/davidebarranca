---
title: "HTML Panel Tips #21: Photoshop CC 2015.5 survival guide"
date: 2016-06-22T00:53:14+01:00
author: Davide Barranca
excerpt: 'Today Photoshop CC2015.5 has been released even if everybody wanted CC2016. This will cause you some headaches: let me hand you a painkiller'
layout: post
permalink: /2016/06/html-panel-tips-21-photoshop-cc2015-5-2016-survival-guide/
description: 'Today Photoshop CC2015.5 has been released even if everybody wanted CC2016. This will cause you some headaches: let me hand you a painkiller'
image: /wp-content/uploads/2016/06/CC2015.png
categories:
  - CEP
tags:
  - CC 2015.5
  - HTML Panels Tips
---

Today Photoshop CC 2015.5 has been released even if everybody wanted CC 2016. This will cause you some headaches: let me hand you a painkiller. First thing to know: CC 2015.5 is, for some _inscrutable_ reason, a **major version**.

*   CC internal version was 14
*   CC 2014 was 15
*   CC 2015 was 16
*   **CC2015.5 is 17**.

This means that if you've set an upper version in your manifest.xml like:

{% highlight xml %}
<HostList>
  <Host Name="PHXS" Version="[14.0, 16.9]" />
  <Host Name="PHSP" Version="[14.0, 16.9]" />
</HostList>
{% endhighlight %}

Your panel **will just stop working**. Myself, I prefer not to set any range, like:

{% highlight xml %}
<HostList>
  <Host Name="PHXS" Version="14.0" />
  <Host Name="PHSP" Version="14.0" />
</HostList>
{% endhighlight %}

Which means: from 14.0 onwards. Mind you, the installation paths for extensions are thankfully always the same:

(Win User): `C:\Users\<username>\AppData\Roaming\Adobe\CEP\extensions\`  
(Win System): `C:\Program Files (x86)\Common Files\Adobe\CEP\extensions\`  
(Mac User): `∼/Library/Application Support/Adobe/CEP/extensions`  
(Mac System): `/Library/Application Support/Adobe/CEP/extensions/`

Second thing to know: in Photoshop at least, **CEP jumped to (major) version 7**. Documentation and new features should be [available soon](https://github.com/Adobe-CEP/CEP-Resources/issues/60#issuecomment-227469836). Please note that CEP7 means that for debugging purposes you need to set a new debug flag! On Mac, open the Terminal and enter:

{% highlight sh %}
defaults write com.adobe.CSXS.7 PlayerDebugMode 1
{% endhighlight %}

On Windows run Regedit, browse to: `HKEY\_CURRENT\_USER/Software/Adobe/CSXS.7` then add a new `PlayerDebugMode` key of type String with value `1`.

Third thing to know: according to this Adobe's official [blogpost](http://blogs.adobe.com/crawlspace/2016/06/faq-photoshop-cc-2015-5-now-available.html#important), there is now a suggested **path for shared, third-party plugins** (in case your Panel uses them). What happens when your users migrate to the new CC 2015.5 major version is that, by default and unless they know [how to avoid it](http://blogs.adobe.com/crawlspace/files/2015/06/AdvancedView.png), **old CC 2015 is going to be removed**.

So all your Scripts belonging to `/Presets/Scripts` and your plugins in `/Plug-ins` are gone too. The paths for versions persistent plugins (from CC up to current, and hopefully newer versions too) [according to Adobe](https://helpx.adobe.com/photoshop/kb/plug-ins-photoshop-troubleshooting.html#topic-5) is:

(Win): `C:\Program Files\Common Files\Adobe\Plug-Ins\CC`  
(Mac): `/Library/Application Support/Adobe/Plug-Ins/CC`

Fourth thing to know (hey, I can count!): if you rely on `.zxp` for installation, you need to download the **new version of ExManCmd command-line tool** from [this link](https://www.adobeexchange.com/resources/28). If/when something new emerges, I will update this post.

* * *

_X-Files corner: Photoshop went from version 5 to 5.5, then from CS5 to CS5.5 (actually 5.1 but within a Creative Suite version 5.5), and now from CC 2015 to CC 2015.5. There must be something really weird going on in Adobe with the number five – conspiracy theorists, let me know!_ 😁

{% include_relative cepBook.md %}
