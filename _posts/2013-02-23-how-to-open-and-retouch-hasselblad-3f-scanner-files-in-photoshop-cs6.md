---
id: 1722
title: How to open and retouch Hasselblad 3F scanner files in Photoshop
date: 2013-02-23T00:56:00+01:00
author: Davide Barranca
excerpt: "If you try to open in Photoshop CS6 a 3F (a file with extension .fff, the raw format for Hasselblad and Imacon scanners) coming from the FlexColor software, you're going to run into troubles: here it is the workaround! And I'll show you my 3F retouching workflow."
layout: post
guid: http://www.davidebarranca.com/?p=1722
permalink: /2013/02/how-to-open-and-retouch-hasselblad-3f-scanner-files-in-photoshop-cs6/
standard_seo_post_level_layout:
  - ""
standard_link_url_field:
  - ""
standard_seo_post_meta_description:
  - Here it is, the workaround to open 3F Hasselblad / Imacon scanner raw files in CS6 avoiding the "Photoshop cannot open this file" error.
seo_post_level_layout:
  - ""
link_url_field:
  - ""
seo_post_meta_description:
  - ""
image: /wp-content/uploads/2013/02/FCIconApp.jpg
categories:
  - Photoshop
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
---
<div class="pf-content">
  <p>
    If you try to open in Photoshop CS6 a 3F (a file with extension .fff, the raw format for Hasselblad and Imacon scanners) coming from the Imacon/Hasselblad FlexColor software, you&#8217;re going to run into troubles (but there&#8217;s plenty of workarounds!):<br /> <a href="http://localhost:8888/wp-content/uploads/2013/02/AdobeCameraRaw_3F_error.jpg" target="_blank" rel="noopener"><img class="alignleft size-medium wp-image-1723" src="http://localhost:8888/wp-content/uploads/2013/02/AdobeCameraRaw_3F_error-300x208.jpg" alt="AdobeCameraRaw 3F error" width="300" height="208" srcset="http://localhost:8888/wp-content/uploads/2013/02/AdobeCameraRaw_3F_error-300x208.jpg 300w, http://localhost:8888/wp-content/uploads/2013/02/AdobeCameraRaw_3F_error-150x104.jpg 150w, http://localhost:8888/wp-content/uploads/2013/02/AdobeCameraRaw_3F_error.jpg 519w" sizes="(max-width: 300px) 100vw, 300px" /></a><strong>That&#8217;s because the Adobe Camera Raw (ACR) that ships with CS6 expects the file to belong to a digital camera &#8211; which is not</strong>. Actually, the 3F is just a plain, bitmap TIFF file with some extra proprietary tags, that ACR can&#8217;t manage properly.
  </p>
  
  <p>
    This didn&#8217;t happen back with CS5, unless you&#8217;ve installed some beta version of ACR. Check it out from the menu: <em>Photoshop &#8211; About Plug-In &#8211; Camera Raw&#8230;</em> (CS5 version should be 6.x).
  </p>
  
  <p>
    There&#8217;s of course a workaround, but first let me review briefly why you do want to open a 3F in Photoshop bypassing ACR.
  </p>
  
  <h2>
    Why opening a 3F in Photoshop?
  </h2>
  
  <p>
    In order to retouch it! Think about the 3F as a scanner raw file &#8211; you can tweak each parameter in the Hasselblad FlexColor software and finally export a TIFF. Alas, scanned film has to be retouched: dust, scratches, blotches, hairs, chemical halos, you know what I mean. Unless you&#8217;re willing to retouch the TIFF each time you export it with different parameters from FlexColor (you aren&#8217;t), <strong>the best workflow is</strong>:
  </p>
  
  <ol>
    <li>
      Open the 3F in Photoshop CS6 in order to retouch it.
    </li>
    <li>
      Save the retouched 3F.
    </li>
    <li>
      Open the retouched 3F in FlexColor (tweaking the parameters to your taste).
    </li>
    <li>
      Export the (usually) TIFF file from FlexColor.
    </li>
    <li>
      Open the TIFF in Photoshop and let the fun begin.
    </li>
  </ol>
  
  <p>
    <strong>Why should I export many time a 3F, isn&#8217;t one time just enough? </strong>Fair question, but consider that:
  </p>
  
  <ul>
    <li>
      People get more experienced over time, and you may want to elaborate a second time a scan you&#8217;ve acquired and processed in the past (and you&#8217;re not really satisfied with the old result).
    </li>
    <li>
      You may want to sandwich two or more FlexColor versions (say, one corrected for the highlights, one for the mid-tones and shadows) in Photoshop.
    </li>
    <li>
      You may be a service provider willing to return the client a flawless 3F scan.
    </li>
  </ul>
  
  <p>
    So <strong>your goal is not to open a 3F with Adobe Camera Raw</strong> (which can&#8217;t export nor save a 3F), <strong>just to open it as a TIFF-like file</strong> to retouch it in Photoshop.
  </p>
  
  <h2>
    1. The easy and smooth way
  </h2>
  
  <p>
    I&#8217;ve discovered this one very recently. Provided you&#8217;ve installed the  <strong>Imacon 3F Plug-In for Photoshop</strong> (see the section 2, below), just <strong>File &#8211; Open</strong> in Photoshop, and select a 3F:
  </p>
  
  <p style="text-align: center;">
    <a href="http://localhost:8888/wp-content/uploads/2013/02/Photoshop_open3F_1.jpg" target="_blank" rel="noopener"><img class="aligncenter size-full wp-image-1758" src="http://localhost:8888/wp-content/uploads/2013/02/Photoshop_open3F_1.jpg" alt="Photoshop open 3F 01" width="829" height="529" srcset="http://localhost:8888/wp-content/uploads/2013/02/Photoshop_open3F_1.jpg 829w, http://localhost:8888/wp-content/uploads/2013/02/Photoshop_open3F_1-150x95.jpg 150w, http://localhost:8888/wp-content/uploads/2013/02/Photoshop_open3F_1-300x191.jpg 300w" sizes="(max-width: 829px) 100vw, 829px" /></a>
  </p>
  
  <p>
    Now, just select the Imacon 3F format as outlined below:
  </p>
  
  <p style="text-align: center;">
    <a href="http://localhost:8888/wp-content/uploads/2013/02/Photoshop_open3F_2.jpg" target="_blank" rel="noopener"><img class="aligncenter size-full wp-image-1759" src="http://localhost:8888/wp-content/uploads/2013/02/Photoshop_open3F_2.jpg" alt="Photoshop open 3F 2" width="829" height="529" srcset="http://localhost:8888/wp-content/uploads/2013/02/Photoshop_open3F_2.jpg 829w, http://localhost:8888/wp-content/uploads/2013/02/Photoshop_open3F_2-150x95.jpg 150w, http://localhost:8888/wp-content/uploads/2013/02/Photoshop_open3F_2-300x191.jpg 300w" sizes="(max-width: 829px) 100vw, 829px" /></a>
  </p>
  
  <p>
    And voilà it will open smoothly and easily!
  </p>
  
  <h2>
    2. The old tricky way
  </h2>
  
  <p>
    <a href="http://localhost:8888/wp-content/uploads/2013/02/ImaconPlugin_version.jpg" target="_blank" rel="noopener"><img class="alignleft size-thumbnail wp-image-1729" src="http://localhost:8888/wp-content/uploads/2013/02/ImaconPlugin_version-150x68.jpg" alt="Imacon plug-in version" width="150" height="68" srcset="http://localhost:8888/wp-content/uploads/2013/02/ImaconPlugin_version-150x68.jpg 150w, http://localhost:8888/wp-content/uploads/2013/02/ImaconPlugin_version-300x137.jpg 300w, http://localhost:8888/wp-content/uploads/2013/02/ImaconPlugin_version.jpg 619w" sizes="(max-width: 150px) 100vw, 150px" /></a>That&#8217;s how I did it before discovering the above method. First thing is the <strong>Imacon 3F Plug-In for Photoshop</strong>, that you can <a title="Imacon 3F Plug-in for Photoshop" href="http://bigano.com/index.php/en/consulting/57-hasselblad/155-il-sistema-3f-hasselblad-flexible-file-format.html?start=9" target="_blank" rel="noopener">download for free here</a>. It should be installed by default by FlexColor, so you can check if you have it already. The right place to look into (and/or to copy the downloaded plugin) is the following folder:
  </p>
  
  <blockquote>
    <p>
      <strong>[MAC]</strong> Photoshop CS6/Plug-ins/File Formats/<strong>imacon3f.plugin</strong><br /> <strong>[PC]</strong> Photoshop CS6/Plug-ins/File Formats/<strong>Imacon 3f.8bi</strong>
    </p>
  </blockquote>
  
  <p>
    Second thing is to <strong>temporarily get rid of Adobe Camera Raw</strong>. It&#8217;s ACR that intercepts the 3F opening and fires the &#8220;Photoshop cannot open this file&#8221; error. You do this by closing Photoshop and moving away the following file:
  </p>
  
  <blockquote>
    <p>
      <strong>[MAC]</strong> Macintosh HD/Library/Application Support/Adobe/Plug-Ins/CS6/File Formats/<strong>Camera Raw.plugin</strong><br /> <strong>[PC]</strong> Program Files/Common Files/Adobe/Plug-Ins/CS6/File Formats/<strong>Camera Raw.8bi</strong>
    </p>
  </blockquote>
  
  <p>
    You can move it to the Desktop or some other folder &#8211; then restart Photoshop CS6. If you look in the menu <em>Photoshop &#8211; About Plug-In </em>there shouldn&#8217;t be any mention of Camera Raw anymore.
  </p>
  
  <p>
    Now, if you&#8217;ve correctly installed the Imacon 3F plugin and got rid of Camera Raw, open a 3F and it should open correctly. Remember to move it back in the correct position when you&#8217;re done!
  </p>
  
  <h3>
    Automatize file moving
  </h3>
  
  <p>
    <a href="http://localhost:8888/wp-content/uploads/2013/02/Apple-Automator.jpg"><img class="alignleft size-full wp-image-1736" src="http://localhost:8888/wp-content/uploads/2013/02/Apple-Automator.jpg" alt="Apple Automator" width="128" height="128" /></a>If you&#8217;re on a Mac (maybe there&#8217;s a Windows equivalent, please suggest it in the comments) you can use the OSX built-in Apple <strong>Automator</strong> application.
  </p>
  
  <p>
    Open it and choose <strong>File &#8211; New &#8211; Application</strong>. Then, from the Library column select <strong>Utilities</strong>, then drag the <strong>Quit Application</strong> in the workspace. There, select the Adobe Photoshop CS6.app from the dropdown menu &#8211; just to make sure Photoshop isn&#8217;t open when you move files around.
  </p>
  
  <p>
    Next, from the Library column select<strong> Files & Folders</strong>, and drag first the <strong>Get Specified Finder Items</strong>, then the <strong>Move Finder Items</strong> components, selecting respectively the camera raw plugin file and the destination folder (the Desktop or a temp folder, for instance).
  </p>
  
  <p style="text-align: center;">
    <a href="http://localhost:8888/wp-content/uploads/2013/02/AppleAutomator_moving.jpg" target="_blank" rel="noopener"><img class="aligncenter size-large wp-image-1735" src="http://localhost:8888/wp-content/uploads/2013/02/AppleAutomator_moving-1024x487.jpg" alt="AppleAutomator moving" width="1024" height="487" srcset="http://localhost:8888/wp-content/uploads/2013/02/AppleAutomator_moving-1024x487.jpg 1024w, http://localhost:8888/wp-content/uploads/2013/02/AppleAutomator_moving-150x71.jpg 150w, http://localhost:8888/wp-content/uploads/2013/02/AppleAutomator_moving-300x142.jpg 300w, http://localhost:8888/wp-content/uploads/2013/02/AppleAutomator_moving.jpg 1566w" sizes="(max-width: 1024px) 100vw, 1024px" /></a>
  </p>
  
  <p>
    Now <strong>File &#8211; Save</strong> the application to the Desktop. If you double click on it, the camera raw plugin file moves from its original location to the temp folder. You can build in the very same way an Automator application that moves it back into place. Keep both apps handy, so <strong>with a double-click you can move files without having to remember who needs to go where!</strong>
  </p>
  
  <h2>
    Retouching a 3F in Photoshop
  </h2>
  
  <p>
    If you&#8217;re new to 3F, I suggest you to visit the excellent <a title="Roberto Bigano Hasselblad 3F page" href="http://bigano.com/index.php/en/consulting/57-hasselblad/155-il-sistema-3f-hasselblad-flexible-file-format.html?showall=1" target="_blank" rel="noopener">Roberto Bigano&#8217;s 3F resources page</a> first. My current workflow, when I&#8217;m in need of 3F retouching (which is basically&#8230; always), is as follows. Click on the thumbnails for a bigger version.
  </p>
  
  <p>
    <a href="http://localhost:8888/wp-content/uploads/2013/02/3f_Retouching_01.jpg" target="_blank" rel="noopener"><img class="alignleft size-thumbnail wp-image-1741" src="http://localhost:8888/wp-content/uploads/2013/02/3f_Retouching_01-150x94.jpg" alt="3F Retouching 01" width="150" height="94" srcset="http://localhost:8888/wp-content/uploads/2013/02/3f_Retouching_01-150x94.jpg 150w, http://localhost:8888/wp-content/uploads/2013/02/3f_Retouching_01-300x189.jpg 300w, http://localhost:8888/wp-content/uploads/2013/02/3f_Retouching_01.jpg 681w" sizes="(max-width: 150px) 100vw, 150px" /></a>
  </p>
  
  <h3>
    1. Open the 3F and save a .PSB
  </h3>
  
  <p>
    Say that you&#8217;ve a <strong style="line-height: 13px;">myScan.fff</strong> file. Open it in Photoshop and immediately save a copy as a .psb (Photoshop Large Format document), <strong style="line-height: 13px;">myScan.psb</strong>. Leave alone the original .fff file, you&#8217;ll be dealing with it again when the retouching is done.
  </p>
  
  <div style="width: 170px; float: left;">
    <a href="http://localhost:8888/wp-content/uploads/2013/02/3f_Retouching_02.jpg" target="_blank" rel="noopener"><br /> <img class="alignleft size-thumbnail wp-image-1738" src="http://localhost:8888/wp-content/uploads/2013/02/3f_Retouching_02-150x93.jpg" alt="3F Retouching 02" width="150" height="93" srcset="http://localhost:8888/wp-content/uploads/2013/02/3f_Retouching_02-150x93.jpg 150w, http://localhost:8888/wp-content/uploads/2013/02/3f_Retouching_02-300x186.jpg 300w, http://localhost:8888/wp-content/uploads/2013/02/3f_Retouching_02.jpg 665w" sizes="(max-width: 150px) 100vw, 150px" /><br /> </a><br /> <a href="http://localhost:8888/wp-content/uploads/2013/02/3f_Retouching_03.jpg" target="_blank" rel="noopener"><br /> <img class="alignleft size-thumbnail wp-image-1739" src="http://localhost:8888/wp-content/uploads/2013/02/3f_Retouching_03-150x93.jpg" alt="3F Retouching 03" width="150" height="93" srcset="http://localhost:8888/wp-content/uploads/2013/02/3f_Retouching_03-150x93.jpg 150w, http://localhost:8888/wp-content/uploads/2013/02/3f_Retouching_03-300x186.jpg 300w, http://localhost:8888/wp-content/uploads/2013/02/3f_Retouching_03.jpg 665w" sizes="(max-width: 150px) 100vw, 150px" /><br /> </a><br /> <a href="http://localhost:8888/wp-content/uploads/2013/02/3f_Retouching_04.jpg" target="_blank" rel="noopener"><br /> <img class="alignleft size-thumbnail wp-image-1740" src="http://localhost:8888/wp-content/uploads/2013/02/3f_Retouching_04-150x66.jpg" alt="" width="150" height="66" srcset="http://localhost:8888/wp-content/uploads/2013/02/3f_Retouching_04-150x66.jpg 150w, http://localhost:8888/wp-content/uploads/2013/02/3f_Retouching_04-300x132.jpg 300w, http://localhost:8888/wp-content/uploads/2013/02/3f_Retouching_04.jpg 909w" sizes="(max-width: 150px) 100vw, 150px" /><br /> </a>
  </div>
  
  <h3>
    2. Retouch the PSB
  </h3>
  
  <p>
    One thing to keep in mind is that you&#8217;re not (at this stage) dealing with noise reduction or color correction &#8211; here you just do the always-boring, zen-like practice of manual dust and scratches removal. <strong>Clone tool, Healing Brush, time and patience will be your friends</strong>.<br /> Duplicate the background layer, call it &#8220;Retouching&#8221; or something else, zoom in and start retouching. If you happen to have a negative scan (which opens as a caramel pudding), I suggest you to add an Invert adjustment layer (which turns everything into a cyan flat-land) and above it a set of Curves adjustment layer (rough curves, just to restore a decent tone and color and make the retouching easier).
  </p>
  
  <p>
    Mind you, do not make the Clone tool and the Healing brush sampling &#8220;All Layers&#8221;. Either &#8220;Current Layer&#8221; or &#8220;Current & Below&#8221; will be ok &#8211; if you wonder why, try it yourself.
  </p>
  
  <p>
    When you&#8217;re done, remove the now useless adjustment layers and <strong>flatten the myScan.psb</strong>.
  </p>
  
  <h3 style="line-height: 25px;">
    <a href="http://localhost:8888/wp-content/uploads/2013/02/3f_Retouching_02.jpg"><img class="alignleft size-thumbnail wp-image-1738" src="http://localhost:8888/wp-content/uploads/2013/02/3f_Retouching_02-150x93.jpg" alt="3F Retouching 02" width="150" height="93" srcset="http://localhost:8888/wp-content/uploads/2013/02/3f_Retouching_02-150x93.jpg 150w, http://localhost:8888/wp-content/uploads/2013/02/3f_Retouching_02-300x186.jpg 300w, http://localhost:8888/wp-content/uploads/2013/02/3f_Retouching_02.jpg 665w" sizes="(max-width: 150px) 100vw, 150px" /></a>
  </h3>
  
  <h3>
    3. Paste the retouched layer in the 3F
  </h3>
  
  <p>
    Open both the original <strong>myScan.fff</strong> and the retouched<strong> myScan.psb</strong>. Drag the Background layer from the .psb and shift-drop it (i.e. do that pressing the shift key in order to auto-align it) into the 3F. You should have two layers in the <strong>myScan.fff</strong> now: the original below and the retouched above.
  </p>
  
  <h3>
    <a href="http://localhost:8888/wp-content/uploads/2013/02/3f_Retouching_01.jpg"><img class="alignleft size-thumbnail wp-image-1741" src="http://localhost:8888/wp-content/uploads/2013/02/3f_Retouching_01-150x94.jpg" alt="3F Retouching 01" width="150" height="94" srcset="http://localhost:8888/wp-content/uploads/2013/02/3f_Retouching_01-150x94.jpg 150w, http://localhost:8888/wp-content/uploads/2013/02/3f_Retouching_01-300x189.jpg 300w, http://localhost:8888/wp-content/uploads/2013/02/3f_Retouching_01.jpg 681w" sizes="(max-width: 150px) 100vw, 150px" /></a>4. Save a copy of the retouched 3F
  </h3>
  
  <p>
    Flatten the <strong>myScan.fff</strong>. Leave it <strong>without any embedded ICC profile</strong> (remove it if necessary: <em>Edit &#8211; Assign Profile &#8211; Don&#8217;t Color Manage This Document</em>) and double check that it has a <strong>single Background layer</strong> only.
  </p>
  
  <p>
    Now <strong>save the file as 3F</strong> (if you don&#8217;t find the <em>Format: Imacon 3F</em> in the saving options the 3F plugin hasn&#8217;t been installed correctly) with a different name, say <strong>myScan_clean.fff</strong>. If you have FlexColor already opened and pointing to the folder where the files are, close and reopen it in order to let the software read them again.
  </p>
  
  <p>
    <img class="alignright size-full wp-image-1751" src="http://localhost:8888/wp-content/uploads/2013/02/FlexColor_DetailWindow.jpg" alt="FlexColor Detail Window" width="300" height="280" srcset="http://localhost:8888/wp-content/uploads/2013/02/FlexColor_DetailWindow.jpg 300w, http://localhost:8888/wp-content/uploads/2013/02/FlexColor_DetailWindow-150x140.jpg 150w" sizes="(max-width: 300px) 100vw, 300px" />
  </p>
  
  <h3>
    Caveat
  </h3>
  
  <p>
    FlexColor preview (at least when your 3Fs are very big &#8211; say a 6&#215;12 cm at full resolution) isn&#8217;t based upon the actual file reading, but upon an embedded low-res image. Which is not affected by your retouching any way, so the 3F will keep showing dust and scratches. If you want to make sure your retouching has been applied, check it opening the Detail window (it&#8217;s the button with the magnifying glass icon): this forces FlexColor to read and show a portion of the actual file. Anyway, when you&#8217;ll export and the TIFF, it&#8217;ll be clean!
  </p>
</div>

<!-- Share-Widget Button BEGIN --><a href="javascript:void(0);" myshare\_id="mys\_shareit" myshare\_url="http://localhost:8888/2013/02/how-to-open-and-retouch-hasselblad-3f-scanner-files-in-photoshop-cs6/" myshare\_title="How to open and retouch Hasselblad 3F scanner files in Photoshop" rel="nofollow" onclick=" return false;" style="text-decoration:none; color:#000000; font-size:11px; line-height:20px;"> 

<img src="http://localhost:8888/wp-content/plugins/share-widget/img/share-button-white-small.png" height="20" alt="Share" style="border:0" /> </a> <!-- Share-Widget Button END -->