---
title: " How to open and retouch Hasselblad 3F scanner files in Photoshop\t\t"
tags:
  - 3F
  - Adobe Camera Raw
  - error
  - fff
  - FlexColor
  - Hasselblad
  - Imacon
  - Photoshop CS6
  - scanner
url: 1722.html
id: 1722
category:
  - Photoshop
date: 2013-02-23 00:56:00
---

If you try to open in Photoshop CS6 a 3F (a file with extension .fff, the raw format for Hasselblad and Imacon scanners) coming from the Imacon/Hasselblad FlexColor software, you're going to run into troubles (but there's plenty of workarounds!): [![AdobeCameraRaw 3F error](http://localhost:8888/wp-content/uploads/2013/02/AdobeCameraRaw_3F_error-300x208.jpg)](http://localhost:8888/wp-content/uploads/2013/02/AdobeCameraRaw_3F_error.jpg)**That's because the Adobe Camera Raw (ACR) that ships with CS6 expects the file to belong to a digital camera - which is not**. Actually, the 3F is just a plain, bitmap TIFF file with some extra proprietary tags, that ACR can't manage properly. This didn't happen back with CS5, unless you've installed some beta version of ACR. Check it out from the menu: _Photoshop - About Plug-In - Camera Raw..._ (CS5 version should be 6.x). There's of course a workaround, but first let me review briefly why you do want to open a 3F in Photoshop bypassing ACR.

Why opening a 3F in Photoshop?
------------------------------

In order to retouch it! Think about the 3F as a scanner raw file - you can tweak each parameter in the Hasselblad FlexColor software and finally export a TIFF. Alas, scanned film has to be retouched: dust, scratches, blotches, hairs, chemical halos, you know what I mean. Unless you're willing to retouch the TIFF each time you export it with different parameters from FlexColor (you aren't), **the best workflow is**:

1.  Open the 3F in Photoshop CS6 in order to retouch it.
2.  Save the retouched 3F.
3.  Open the retouched 3F in FlexColor (tweaking the parameters to your taste).
4.  Export the (usually) TIFF file from FlexColor.
5.  Open the TIFF in Photoshop and let the fun begin.

**Why should I export many time a 3F, isn't one time just enough? **Fair question, but consider that:

*   People get more experienced over time, and you may want to elaborate a second time a scan you've acquired and processed in the past (and you're not really satisfied with the old result).
*   You may want to sandwich two or more FlexColor versions (say, one corrected for the highlights, one for the mid-tones and shadows) in Photoshop.
*   You may be a service provider willing to return the client a flawless 3F scan.

So **your goal is not to open a 3F with Adobe Camera Raw** (which can't export nor save a 3F), **just to open it as a TIFF-like file** to retouch it in Photoshop.

1\. The easy and smooth way
---------------------------

I've discovered this one very recently. Provided you've installed the  **Imacon 3F Plug-In for Photoshop** (see the section 2, below), just **File - Open** in Photoshop, and select a 3F:

[![Photoshop open 3F 01](http://localhost:8888/wp-content/uploads/2013/02/Photoshop_open3F_1.jpg)](http://localhost:8888/wp-content/uploads/2013/02/Photoshop_open3F_1.jpg)

Now, just select the Imacon 3F format as outlined below:

[![Photoshop open 3F 2](http://localhost:8888/wp-content/uploads/2013/02/Photoshop_open3F_2.jpg)](http://localhost:8888/wp-content/uploads/2013/02/Photoshop_open3F_2.jpg)

And voilà it will open smoothly and easily!

2\. The old tricky way
----------------------

[![Imacon plug-in version](http://localhost:8888/wp-content/uploads/2013/02/ImaconPlugin_version-150x68.jpg)](http://localhost:8888/wp-content/uploads/2013/02/ImaconPlugin_version.jpg)That's how I did it before discovering the above method. First thing is the **Imacon 3F Plug-In for Photoshop**, that you can [download for free here](http://bigano.com/index.php/en/consulting/57-hasselblad/155-il-sistema-3f-hasselblad-flexible-file-format.html?start=9 "Imacon 3F Plug-in for Photoshop"). It should be installed by default by FlexColor, so you can check if you have it already. The right place to look into (and/or to copy the downloaded plugin) is the following folder:

> **\[MAC\]** Photoshop CS6/Plug-ins/File Formats/**imacon3f.plugin** **\[PC\]** Photoshop CS6/Plug-ins/File Formats/**Imacon 3f.8bi**

Second thing is to **temporarily get rid of Adobe Camera Raw**. It's ACR that intercepts the 3F opening and fires the "Photoshop cannot open this file" error. You do this by closing Photoshop and moving away the following file:

> **\[MAC\]** Macintosh HD/Library/Application Support/Adobe/Plug-Ins/CS6/File Formats/**Camera Raw.plugin** **\[PC\]** Program Files/Common Files/Adobe/Plug-Ins/CS6/File Formats/**Camera Raw.8bi**

You can move it to the Desktop or some other folder - then restart Photoshop CS6. If you look in the menu _Photoshop - About Plug-In _there shouldn't be any mention of Camera Raw anymore. Now, if you've correctly installed the Imacon 3F plugin and got rid of Camera Raw, open a 3F and it should open correctly. Remember to move it back in the correct position when you're done!

### Automatize file moving

[![Apple Automator](http://localhost:8888/wp-content/uploads/2013/02/Apple-Automator.jpg)](http://localhost:8888/wp-content/uploads/2013/02/Apple-Automator.jpg)If you're on a Mac (maybe there's a Windows equivalent, please suggest it in the comments) you can use the OSX built-in Apple **Automator** application. Open it and choose **File - New - Application**. Then, from the Library column select **Utilities**, then drag the **Quit Application** in the workspace. There, select the Adobe Photoshop CS6.app from the dropdown menu - just to make sure Photoshop isn't open when you move files around. Next, from the Library column select **Files & Folders**, and drag first the **Get Specified Finder Items**, then the **Move Finder Items** components, selecting respectively the camera raw plugin file and the destination folder (the Desktop or a temp folder, for instance).

[![AppleAutomator moving](http://localhost:8888/wp-content/uploads/2013/02/AppleAutomator_moving-1024x487.jpg)](http://localhost:8888/wp-content/uploads/2013/02/AppleAutomator_moving.jpg)

Now **File - Save** the application to the Desktop. If you double click on it, the camera raw plugin file moves from its original location to the temp folder. You can build in the very same way an Automator application that moves it back into place. Keep both apps handy, so **with a double-click you can move files without having to remember who needs to go where!**

Retouching a 3F in Photoshop
----------------------------

If you're new to 3F, I suggest you to visit the excellent [Roberto Bigano's 3F resources page](http://bigano.com/index.php/en/consulting/57-hasselblad/155-il-sistema-3f-hasselblad-flexible-file-format.html?showall=1 "Roberto Bigano Hasselblad 3F page") first. My current workflow, when I'm in need of 3F retouching (which is basically... always), is as follows. Click on the thumbnails for a bigger version. [![3F Retouching 01](http://localhost:8888/wp-content/uploads/2013/02/3f_Retouching_01-150x94.jpg)](http://localhost:8888/wp-content/uploads/2013/02/3f_Retouching_01.jpg)

### 1\. Open the 3F and save a .PSB

Say that you've a **myScan.fff** file. Open it in Photoshop and immediately save a copy as a .psb (Photoshop Large Format document), **myScan.psb**. Leave alone the original .fff file, you'll be dealing with it again when the retouching is done.

 [![3F Retouching 02](http://localhost:8888/wp-content/uploads/2013/02/3f_Retouching_02-150x93.jpg)](http://localhost:8888/wp-content/uploads/2013/02/3f_Retouching_02.jpg) [ ![3F Retouching 03](http://localhost:8888/wp-content/uploads/2013/02/3f_Retouching_03-150x93.jpg) ](http://localhost:8888/wp-content/uploads/2013/02/3f_Retouching_03.jpg) [![](http://localhost:8888/wp-content/uploads/2013/02/3f_Retouching_04-150x66.jpg)](http://localhost:8888/wp-content/uploads/2013/02/3f_Retouching_04.jpg) 

### 2\. Retouch the PSB

One thing to keep in mind is that you're not (at this stage) dealing with noise reduction or color correction - here you just do the always-boring, zen-like practice of manual dust and scratches removal. **Clone tool, Healing Brush, time and patience will be your friends**. Duplicate the background layer, call it "Retouching" or something else, zoom in and start retouching. If you happen to have a negative scan (which opens as a caramel pudding), I suggest you to add an Invert adjustment layer (which turns everything into a cyan flat-land) and above it a set of Curves adjustment layer (rough curves, just to restore a decent tone and color and make the retouching easier). Mind you, do not make the Clone tool and the Healing brush sampling "All Layers". Either "Current Layer" or "Current & Below" will be ok - if you wonder why, try it yourself. When you're done, remove the now useless adjustment layers and **flatten the myScan.psb**.

### [![3F Retouching 02](http://localhost:8888/wp-content/uploads/2013/02/3f_Retouching_02-150x93.jpg)](http://localhost:8888/wp-content/uploads/2013/02/3f_Retouching_02.jpg)

### 3\. Paste the retouched layer in the 3F

Open both the original **myScan.fff** and the retouched **myScan.psb**. Drag the Background layer from the .psb and shift-drop it (i.e. do that pressing the shift key in order to auto-align it) into the 3F. You should have two layers in the **myScan.fff** now: the original below and the retouched above.

### [![3F Retouching 01](http://localhost:8888/wp-content/uploads/2013/02/3f_Retouching_01-150x94.jpg)](http://localhost:8888/wp-content/uploads/2013/02/3f_Retouching_01.jpg)4\. Save a copy of the retouched 3F

Flatten the **myScan.fff**. Leave it **without any embedded ICC profile** (remove it if necessary: _Edit - Assign Profile - Don't Color Manage This Document_) and double check that it has a **single Background layer** only. Now **save the file as 3F** (if you don't find the _Format: Imacon 3F_ in the saving options the 3F plugin hasn't been installed correctly) with a different name, say **myScan_clean.fff**. If you have FlexColor already opened and pointing to the folder where the files are, close and reopen it in order to let the software read them again. ![FlexColor Detail Window](http://localhost:8888/wp-content/uploads/2013/02/FlexColor_DetailWindow.jpg)

### Caveat

FlexColor preview (at least when your 3Fs are very big - say a 6x12 cm at full resolution) isn't based upon the actual file reading, but upon an embedded low-res image. Which is not affected by your retouching any way, so the 3F will keep showing dust and scratches. If you want to make sure your retouching has been applied, check it opening the Detail window (it's the button with the magnifying glass icon): this forces FlexColor to read and show a portion of the actual file. Anyway, when you'll export and the TIFF, it'll be clean!