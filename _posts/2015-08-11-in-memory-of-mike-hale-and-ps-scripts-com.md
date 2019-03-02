---
title: In memory of Mike Hale and PS-Scripts.com
date: 2015-08-11T17:04:54+01:00
author: Davide Barranca
excerpt: "PS-Scripts, the independent forum devoted to Photoshop scripting, is offline since May 2015 due to the passing of the domain owner and maintainer, the very missed Mike Hale."
layout: post
permalink: /2015/08/in-memory-of-mike-hale-and-ps-scripts-com/
description: "PS-Scripts, the independent forum devoted to Photoshop scripting, is offline since May 2015 due to the passing of the domain owner and maintainer, the very missed Mike Hale."
image: /wp-content/uploads/2015/08/1398164_profile_pic.jpg
category:
  - Scripting
tags:
  - Mike Hale
  - ps-scripts.com
---

As you might have noticed, [PS-Scripts](http://www.ps-scripts.com) – the independent forum devoted to Photoshop scripting - is offline since May 2015 due to the passing of the domain owner and maintainer, the very missed Mike Hale.

People (like me) who have been spending countless hours there looking for, and finding, valuable information, posting and chatting with other experienced developers, have been deeply touched by the loss of Mike. And, I'd say, by the sudden disappearance of a decade old forum. Bits of information can be found in this Adobe Photoshop Scripting official forum [thread](https://forums.adobe.com/thread/1847411), but I've always liked to tell the whole story (as I know it) here. It's not a timely post, but somehow I feel I had to write it anyway.

As a regular visitor of PS-Scripts, I noticed in 2014 a lack of posts from Mike: I checked his user information and saw that he kept logging, but his contributions were just fading off. At first I thought that he could have just lost interest: in the recent past, other great developers who wrote fundamental chapters of the history of Scripting such as Paul Riggott and xbytor announced to be definitely fed up with Adobe's lack of commitment in fixing bugs and evolving the platform, and quit. Logs from Mike definitely stopped in late 2014, and around May 2015 a placeholder appeared instead of the Forum's landing page. Slightly nervous, I asked for information in the official forum and started digging the internet looking for Mike's traces: I've been in touch with him few times over the years, but couldn't find his email anywhere - most of our chats happened as private messages on PS-Scripts. Apparently he took privacy in high regard - very little can be found about him online - and as a introvert I'm not the kind of person who emails from time to time just to keep in touch.

Googling for him, one night I eventually run into [his obituary](http://www.charlestonfunerals.com/home/index.cfm/obituaries/view/fh_id/15235/id/3817879), which left no doubts since he's portrayed this way:

> [...] he then went on to pursue a career in the photography lab industry. He was a contributor to ADOBE Photoshop and led an international photo scripting forum. His passion in life was restoring old damaged photos and spoiling his nieces, nephews, grandnieces and nephews. Michael L. Hale was a kind, compassionate, caring gentleman who is admired and respected by many and will be greatly missed by all.

So Mike passed away in September 2014 and the Forum's placeholder page was just GoDaddy's way to tell the world the domain was about to be vacant. Saddened by the news I got in touch with some of the people I know in the business, and we started discussing a viable strategy to get back online the invaluable 10 years worth PS-Scripts archive.

Among those who kindly offered their help, either logistic and/or financial, xbytor, Chuck Uebele, Jeffrey Tranberry, Tom Ruark, Pete Green, Sandra Voelker and others. xbytor found the [PS-Script snapshot](http://web.archive.org/web/20150314191545/http://ps-scripts.com/) on archive.org (sort of the internet time capsule), which is available for consultation yet not searchable (as far as I know) and offered to reach out for Mike's relatives in order to retrieve his own backups. xbytor was one of the PS-Scripts administrators, and has given a substantial contribution over the years to the forum. He also tried to talk with GoDaddy support, as a plan B. Both plans haven't been as successful as we hoped: no backup from the Hales, and lot of bureaucracy just to take over the domain.

I volunteered to try again with GoDaddy, and found additional information: while the domain was somehow pre-paid and still reserved to Mike Hale until mid-2016, the hosting expired and - given a grace period of few months - GoDaddy eventually deleted all the servers content by the end of May 2015 - we arrived too late. Being PS-Scripts based on phpBB (a very common PHP forum platform), it's not possible as far as I've been told by PHP programmers, to restore it unless you have a proper backup (that is, some sort of mySQL database + phpBB data), such as the one from Mike - who by the way would have been dated September 2014 at best. Conversely, Archive.org snapshot (dated May 2015, so 8 months more up to date), is just a plain, flat version: unsuitable for posting, logging, searching, etc.

So, paradoxical as it seems in the era of digital permanence, 10 years of shared knowledge, efforts and care have more or less vanished in a snap. All those who've been in touch with Mike describe him as a helpful and kind person, and I could not agree more. He was not only knowledgeable, but also willing to share, and as a maintainer of the forum he has demonstrated with facts his own commitment in a field in which frustrations, dare I to say, are far more common than satisfactions. Trying to preserve his, and others' work, proved to be hard too.

Downloading Archive.org entire websites is explicitely forbidden by their policies, yet somebody I'm, ehm, close to paid a small sum to a company which does retrieve sites for offline viewing - it's [not a complete dump](https://dl.dropboxusercontent.com/u/23243188/ps-scripts.zip) (about 12K files, lot of which are useless duplicates of the login page and the like), to view it you need to set up a local server on your machine (with [MAMP](http://mamp.info) or [Ampps](http://www.ampps.com) – copy also the .htaccess file), otherwise just search for a topic on Finder and use that folder as a source.  You can also try yourself with apps such as [Site Sucker](http://ricks-apps.com/osx/sitesucker/index.html), but the output might be very large - I've not been able to restrict to the latest Archive.org snapshot (the one from May 2014), if you know how to do it write in the comments below.

Chuck Uebele, who is an Adobe Community Manager, volunteered to set up a dedicated section in the Adobe Photoshop Scripting forum, to collect relevant threads. Sandra Voelker offered the photoshop-scripting.com domain she owns. In order to avoid the issue of linking too tightly a website to somebody, I suggested GitHub (which by the way can host for free static websites) and started this experimental, early alpha [PS-Scripting Cookbook](http://undavide.github.io/Photoshop-Scripting-Cookbook/) – which is based on GitHub Pages (plain text Markdown files, "compiled" on the fly with [Jekyll](http://www.jekyllrb.com), a Liquid Templates based Generator), linking [Gists](http://www.labnol.org/internet/github-gist-tutorial/28499/) (version managed snippets).

<figure class="alignleft">
<img width="300" src="/wp-content/uploads/2015/08/1398164_profile_pic.jpg" />
<figcaption>Mike Hale</figcaption>
</figure>

A mixed approach might be the right one, in which code is available, version managed, and discussed too - the greater plus that PS-Scripts offered was the interaction between contributors, which usually led to new and interesting solutions. Needless to say, contributions are needed and more than welcome so please join in - this community needs you too. I'd like to thank everybody who's spent his own time trying to build first, then recover the PS-Scripts community: a place I personally and professionaly owe a lot to.

So long Mike, and thanks for all the fish.
