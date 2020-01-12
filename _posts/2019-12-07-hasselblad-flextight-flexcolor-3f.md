---
title: The state of Hasselblad Flextight scanners, FlexColor and the 3F format
date: 2019-12-07
author: Davide Barranca
excerpt: "In case you missed it, Flextight scanners have vanished from the Hasselblad website around May 2019 and macOS Catalina doesn't run FlexColor anymore. No software updates are planned, plus I've run into problems dealing with 3F files in Photoshop 2020. Heavens!"
layout: post
permalink: /2019/12/2019-12-07/2019-12-07-hasselblad-flextight-flexcolor-3f/
description: "In case you missed it, Flextight scanners have vanished from the Hasselblad website around May 2019 and macOS Catalina doesn't run FlexColor anymore. No software updates are planned, plus I've run into problems dealing with 3F files in Photoshop 2020. Heavens!"
image: /wp-content/uploads/2019/12/flexcolor.png
media_card: /wp-content/uploads/2019/12/media.jpg
category:
  - Photoshop
tags:
  - Hasselblad
  - Imacon
  - Flextight
  - FlexColor
  - 3F
---

In case you missed it, Flextight scanners have vanished from the Hasselblad website around May 2019 and macOS Catalina doesn't run FlexColor anymore. No software updates are planned, plus I've run into problems dealing with `fff` files in Photoshop 2020. Heavens!

## Flextight scanners

Flextight X1 and X5, the flagship (and only left) models in Hasselblad's lineup have been discontinued: EOL, kaputt. No official announcement has been published, but a number of users around the world have had the news confirmed either from dealers or servicing shops.

It's pointless to ask the reason why, scanners have always been a low priority asset to Hasselblad. Flextight know-how and hardware were acquired in 2014 when the company merged with Imacon: reliable sources told me that the engineering team has always been quite small. My best bet is that the 2017 acquisition of Hasselblad by DJI, a Chinese drone-making company, gave scanners the _coup de grace_.

The good news is that servicing will be available for the foreseeable future, as customer service wrote me:

> The Flextight scanners are full service supported in our factory service in Gothenburg, Sweden and a selected number of regional scanner partners are able to perform local service and preventive maintenance. We will continue to perform service for these scanners as long as we will have spare parts, therefore it is difficult to appreciate for how long.

In fact, this is what you can read when logging in the private area of the Hasselblad website.

![](/wp-content/uploads/2019/12/servicing.jpg)

## FlexColor software

Unclear information has been given to users in the past, when many of us pinged Hasselblad about the pressing issue of macOS 10.15 Catalina blocking 32bit applications. Any hope to see a 64bit FlexColor rewrite has been definitely flushed away when customer support eventually started to reply that:

> [...] we currently have no plans to release an updated version of Flexcolor.

As expected, the software has been EOLed very much like the hardware. Not that it has ever received much care: the last available versions are 4.8.13 (2011, for Mac) and 4.8.9 (2012, for Windows).

<figure>
<img src="/wp-content/uploads/2019/12/fc-error.jpg" style="width: 357px; height: 180px">
</figure>  

To the best of my knowledge, the Windows version still runs on whatever version is Windows at now, but with macOS you're capped to 10.14 Mojave.

Neither FlexColor nor the scanners do require a high-performance computer, they're probably going to work fine with old dedicated hardware for years to come. I think it's possible to run Catalina and virtualize (with Parallels or VMWare) Mojave so that you can still run 32bit apps. Let's hope that by the time the whole process becomes impractical or the spare parts are out of stock, the film we want to scan has been eroded away by chemical remains and fungi – checkmate, problem solved[^1980].

[^1980]: Please bear with me but I'm right in the middle of the postproduction of a batch of large format, color negative scans from the early 1980, and it's a mess. Are you nostalgic of the old days of analogic photography? Bloody hell, no.


Finally, due to legal reasons (very likely the use of some internal libraries that prevent it) FlexColor can't be open-sourced. There are rumors about VueScan willing (or having been asked) to implement a module that would drive the Flextight hardware, but I couldn't been able to find any official statement.

So this is the state of affairs for the hardware, what about existing archives?

## 3F files

Here things are getting slightly more complex, please bear with me.

As we all know, FlexColor is used by archivists and photoshoppers alike to browse through, and extract `.tif` files from, huge collections of `.fff` (aka 3F) files. You may or may not know that the 3F format is nothing but a `.tif` file with some proprietary tags: to prove it, try renaming a `.fff` to `.tif` and open it in Photoshop. The extra stuff is:

- the compressed/low-res bitmap preview that FlexColor uses to _temporarily_ display the 3F while waiting to read the actual data from the file (only if the 3F isn't too large, in which case you're _always_ only shown the preview)
- metadata related to the scan acquisition and FlexColor settings (e.g. curves, FlexTouch, saturation, snapshots, etc.)

I have grown accustomed to skip FlexColor altogether for anything but driving the scanner (acquiring the 3F), and browsing (i.e. visualizing the files). That is to say: I **do not use FlexColor anymore for any 3F processing** nor `.tif` extraction. I've never liked FlexColor rudimental, limited features – they were sort-of OK in the mid-2000s, or for large files batches quick post-processing. My client's work today is more focused on a smaller selection of scans, to be post-produced very accurately, so I only use Photoshop.

**To briefly sum up the way I use to work** (which I can expand in a separate blogpost – leave a comment below in case it is of any interest):

- I open the 3F in Photoshop (thanks to the `Imacon3F.plugin` or its `.8bf` Windows version), which in my case is going to appear as a color negative.
- I apply an Invert adjustment layer and a couple of Curves adjustment layers to make it look more or less _correct_ to my eyes – no need to be precise here.
- I duplicate the background layer and perform all sorts of retouching voodoo (against dust, scratches, chemicals stains, fingerprints, semen, etc.), saving this intermediate layered file as a separate, temporary `.psb`.
- When I'm happy, I flatten all the bitmap layers and discard all the adjustment layers, so that I'm back with a one-layer, color negative file again.
- I re-open the original `.fff` file, drag and drop the retouched layer from the `.psb`, flatten, and save as `.CLEAN.fff` – this because one never knows what the future brings... I then trash the intermediate `.psb`.
- I keep the `.CLEAN.fff` open: assign the _FlexColor Input_ ICC, convert to Adobe RGB, Invert, and save my working file as a `.psb`. This is my starting point, to which I apply the usual Curves, and whatever it takes to make the image work.

Enter another stray bullet in the Hasselblad game: Adobe. It turns out that **Photoshop 2020 doesn't want to open 3Fs anymore**, popping up this new errors:

![image proportion has changed the imacon 3f plugin can't save the file](/wp-content/uploads/2019/12/ps-error.jpg)

In recent versions, you already had to set the Open dialog's settings to _Enable: All Documents_ (and of course the _Format: Imacon 3f_) otherwise Adobe Camera Raw kicked in trying to open the file, mistaking it as coming from a digital back.

![](/wp-content/uploads/2019/12/open.jpg)

The last working version (at least on a Mac) is Photoshop CC 2019 – which is a **big concern**. Why? You may or may not have heard that in the last months Adobe has aggressively restricted the licensed versions range for Photoshop – e.g. you cannot, theoretically, install or run, say, Photoshop CC 2015 anymore. Without entering into yet another rabbit hole, it all boils down to royalties with third-parties, very likely Dolby[^dolby].

[^dolby]: Read [here](https://www.plagiarismtoday.com/2019/05/15/adobe-dolby-and-the-battle-over-your-software/) for more info about the Adobe-Dolby lawsuit.

In the end, Adobe's versions policy is (and it is fair to expect that it will be their default from now on) to license N-1 versions – in other words: you'll be allowed to download and install the latest Photoshop, plus the version before it, period. At the time I'm writing this, it is PS 2020 + PS CC 2019. In one year, CC 2019 will be dropped, and it'll be PS 2021 + PS 2020.

Which means: one year from now, the _officially available_ Photoshop versions won't open 3F files anymore, and working versions' installers won't be available for download either. LMAO.

There is also the tangentially related problem of macOS notarization/digital signature, from macOS Catalina onwards: `.plugin` files are requested to be at least[^sign] signed by the developer, otherwise macOS GateKeeper will prevent to run them. This is not too much of a big deal, I have an active Apple Developer membership and I can sign the file myself if needed.

[^sign]: It is unclear to me, at the moment, whether a digital signature is enough or I would be required to notarize the `.plugin` as well – not a big deal either, but not running Catalina myself I cannot test it.

## TL;DR

- Hasselblad Flextight X1 and X5 scanners (previously known as Imacon) are discontinued. Scanner servicing will be provided as usual _as long as spare parts will be available_.
- FlexColor is abandonware: it doesn't run on any macOS > 10.14.
- 3F files don't open anymore in Photoshop 2020; very likely the CC 2019 installer is not going to be available one year from now, and even if you have one, it's definitely possible that macOS Catalina will prevent `Imacon3F.plugin` from working because it's not signed/notarized.

## Workarounds (if any)

The following is partly subjective.

- **Acquiring new scans**, for the time being, requires FlexColor; so either you keep dedicated hardware running macOS Mojave, or you switch to Windows.
- **Browsing 3F collections** is, and will keep being, a pain in the butt: FlexColor is the _lesser evil_, Adobe Bridge seems unable to load just the low-res preview that is embedded in `.fff` files and it takes forever to read the file content to create a thumbnail. The same holds true with Finder. I'm not aware of any other viable options for the Mac.
- Extracting `.tif` files for **3F post-processing** is not a problem, as long as the Photoshop plugin keeps working 🤞🏻.

Feel free to comment below if you have updated news, workflow suggestions, or you just want to share your point of view as a Hasselblad user.
  Thanks!

<hr />
