---
title: How to open and retouch Hasselblad 3F scanner files in Photoshop
date: 2013-02-23T00:56:00+01:00
author: Davide Barranca
excerpt: "How to open and retouch Hasselblad 3F scanner files in Photoshop"
layout: post
permalink: /2013/02/how-to-open-and-retouch-hasselblad-3f-scanner-files-in-photoshop-cs6/
description: "How to open and retouch Hasselblad 3F scanner files in Photoshop"
image: /wp-content/uploads/2013/02/FCIconApp.jpg
category:
  - Photoshop
tags:
  - 3F
  - FlexColor
  - Hasselblad
  - Imacon
---

[**Updated** on March 2019]  
If you try to open in Photoshop a 3F (a file with extension `.fff`, the raw format for Hasselblad and Imacon scanners) coming from the Imacon/Hasselblad FlexColor software, you're going to run into troubles (but there's plenty of workarounds!):

![AdobeCameraRaw 3F error](/wp-content/uploads/2013/02/AdobeCameraRaw_3F_error.jpg)

**That's because the Adobe Camera Raw (ACR) that ships with CS6 expects the file to belong to a digital camera - which is not**. Actually, the 3F is just a plain, bitmap TIFF file with some extra proprietary tags, that ACR can't manage properly. This didn't happen back with CS5, unless you've installed some beta version of ACR. There's of course a workaround, but first let me review briefly why you do want to open a 3F in Photoshop bypassing ACR.

# Why opening a 3F in Photoshop?

In order to retouch it! Think about the 3F as a scanner raw file - you can tweak each parameter in the Hasselblad FlexColor software and finally export a TIFF. Alas, scanned film has to be retouched: dust, scratches, blotches, hairs, chemical halos, you know what I mean. Unless you're willing to retouch the TIFF each time you export it with different parameters from FlexColor (you aren't), **the best workflow is**:

1.  Open the 3F in Photoshop CS6 in order to retouch it.
2.  Save the retouched 3F.
3.  Open the retouched 3F in FlexColor (tweaking the parameters to your taste).
4.  Export the (usually) TIFF file from FlexColor.
5.  Open the TIFF in Photoshop and let the fun begin.

**Why should I export many time a 3F, isn't one time just enough?** Fair question, but consider that:

*   People get more experienced over time, and you may want to elaborate a second time a scan you've acquired and processed in the past (and you're not really satisfied with the old result).
*   You may want to sandwich two or more FlexColor versions (say, one corrected for the highlights, one for the mid-tones and shadows) in Photoshop.
*   You may be a service provider willing to return the client a flawless 3F scan.

So **your goal is not to open a 3F with Adobe Camera Raw** (which can't export nor save a 3F), **just to open it as a TIFF-like file** to retouch it in Photoshop.

## How to

First, you need to install the  **Imacon 3F Plug-In for Photoshop**: create an account on "My Hasselblad" and find it in the download section. Move the file either in:

**[MAC]** `Photoshop CC 2019/Plug-ins/File Formats/imacon3f.plugin`  
**[PC]** `Photoshop CC 2019/Plug-ins/File Formats/Imacon 3f.8bi`

(or whatever Photoshop version you own). Please note that you can't just **File - Open**. You must click the **Options** button on the bottom-left corner of the open dialog to show the options. Under **Format** you must select **Imacon 3F**; then, click one time in the file to open; finally, due to a Photoshop bug, you also need to set **All Documents** under **Enable** (by default it selects "All Readable Documents"). This is important, and in this exact sequence, otherwise it won't work.

![Photoshop open 3F 01](/wp-content/uploads/2013/02/Photoshop_open3F_2.jpg)

And voilà it will open smoothly and easily!

## Retouching a 3F in Photoshop

If you're new to 3F, I suggest you to visit the excellent [Roberto Bigano's 3F resources page]("https://www.knowhowtransfer.com/how-to/resources/3f-system-revolutionary-pro-scanning-system-by-hasselblad/") first. My current workflow, when I'm in need of 3F retouching (which is basically... always), is as follows. Click on the thumbnails for a bigger version.

### 1. Open the 3F and save a .PSB

Say that you've a **myScan.fff** file. Open it in Photoshop and immediately save a copy as a .psb (Photoshop Large Format document), **myScan.psb**. Leave alone the original .fff file, you'll be dealing with it again when the retouching is done.

![3F Retouching 01](/wp-content/uploads/2013/02/3f_Retouching_01.jpg)




### 2. Retouch the PSB

Duplicate the background layer, call it "Retouching" or something else.

![3F Retouching 02](/wp-content/uploads/2013/02/3f_Retouching_02.jpg)

I suggest you to add an Invert adjustment layer (which turns everything into a cyan flat-land) and above it a set of Curves adjustment layer (rough curves, just to restore a decent tone and color and make the retouching easier).

![3F Retouching 03](/wp-content/uploads/2013/02/3f_Retouching_03.jpg)

![3F Retouching 04](/wp-content/uploads/2013/02/3f_Retouching_04.jpg)

One thing to keep in mind is that you're not (at this stage) dealing with noise reduction or color correction - here you just do the always-boring, zen-like practice of manual dust and scratches removal. **Clone tool, Healing Brush, time and patience will be your friends**.

Mind you, do not make the Clone tool and the Healing brush sampling "All Layers". Either "Current Layer" or "Current & Below" will be ok - if you wonder why, try it yourself. When you're done, remove the now useless adjustment layers and **flatten the myScan.psb**.

![3F Retouching 02](/wp-content/uploads/2013/02/3f_Retouching_02.jpg)

### 3. Paste the retouched layer in the 3F

Open both the original **myScan.fff** and the retouched **myScan.psb**. Drag the Background layer from the .psb and shift-drop it (i.e. do that pressing the shift key in order to auto-align it) into the 3F. You should have two layers in the **myScan.fff** now: the original below and the retouched above.

![3F Retouching 01](/wp-content/uploads/2013/02/3f_Retouching_01.jpg)

Flatten the **myScan.fff**. Leave it **without any embedded ICC profile** (remove it if necessary: _Edit - Assign Profile - Don't Color Manage This Document_) and double check that it has a **single Background layer** only. Now **save the file as 3F** (if you don't find the _Format: Imacon 3F_ in the saving options the 3F plugin hasn't been installed correctly) with a different name, say **myScan_clean.fff**. If you have FlexColor already opened and pointing to the folder where the files are, close and reopen it in order to let the software read them again.

![FlexColor Detail Window](/wp-content/uploads/2013/02/FlexColor_DetailWindow.jpg)

### Caveat

FlexColor preview (at least when your 3Fs are very big - say a 6x12 cm at full resolution) isn't based upon the actual file reading, but upon an embedded low-res image. Which is not affected by your retouching any way, so the 3F will keep showing dust and scratches. If you want to make sure your retouching has been applied, check it opening the Detail window (it's the button with the magnifying glass icon): this forces FlexColor to read and show a portion of the actual file. Anyway, when you'll export and the TIFF, it'll be clean!
