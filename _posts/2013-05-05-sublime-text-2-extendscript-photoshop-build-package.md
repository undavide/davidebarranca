---
id: 2019
title: Sublime Text 2 and 3 ExtendScript Photoshop package (Updated)
date: 2013-05-05T23:16:54+01:00
author: Davide Barranca
excerpt: "If you aren't in love with Adobe's ExtendScript Toolkit, welcome to the club. I've come up with a Sublime Text 2 build system package that lets you run JSX scripts directly from Sublime Text targeting Photoshop, Command+B or CTRL+B and voilà!"
layout: post
guid: http://www.davidebarranca.com/?p=2019
permalink: /2013/05/sublime-text-2-extendscript-photoshop-build-package/
standard_seo_post_level_layout:
  - ""
standard_link_url_field:
  - ""
standard_seo_post_meta_description:
  - This custom build package lets you run ExtendScript code targeting Photoshop from Sublime Text 2, minimizing the ExtendScript Toolkit use
seo_post_level_layout:
  - ""
link_url_field:
  - ""
seo_post_meta_description:
  - 'Sublime Text 2/3 Build Package: runs ExtendScript code in Photoshop without the need to copy/paste code in ExtendScript ToolKit (ESTK)'
image: /wp-content/uploads/2013/05/logo.jpg
categories:
  - Coding
  - ExtendScript / Javascript
tags:
  - build
  - Extendscript
  - package
  - Sublime Text 2
---
<div class="pf-content">
  <p>
    This post contains a <a title="Sublime Text website" href="http://www.sublimetext.com/" target="_blank">Sublime Text 2</a> custom build package that lets you run ExtendScript code targeting Adobe Photoshop. I&#8217;ve coded it applying few tweaks and customization to an existing package made by <a title="Sebastien Lavoie website" href="http://seblavoie.com" target="_blank">Sébastien Lavoie</a> for After Effects scripting. <!--more-->
  </p>
  
  <h2>
    What is that for?
  </h2>
  
  <p>
    When you code Photoshop scripting, chances are that you do it with the <strong>ExtendScript ToolKit</strong> (aka ESTK). While this Adobe provided piece of software has some unique features that make it very efficient while debugging and looking for DOM documentation, as an editor it really sucks. I&#8217;m sorry about that, but compared to other modern editors it&#8217;s barely usable.
  </p>
  
  <p>
    <a title="Sublime Text website" href="http://www.sublimetext.com/" target="_blank">Sublime Text 2</a>, (and the 3 beta) conversely, are an amazing editor with tons of features and plugins. Yet so far the only workflow for Photoshop scripters was to code ExtendScript (which is a superset of Javascript) in Sublime Text, then copy and paste it to ESTK and run the script from ESTK.
  </p>
  
  <p>
    This custom build package lets you run ExtendScript code targeting Photoshop in Sublime Text 2 and 3. Type ⌘B (or ctrl+B) and your code is being executed in the host app!
  </p>
  
  <h2>
    UPDATE (September 2016)
  </h2>
  
  <p>
    I&#8217;ve tweaked the Package described in this article, originally written in 2013. Important news:
  </p>
  
  <ul>
    <li>
      The code is now hosted and maintained in a <strong>GitHub repository </strong>that you can <a href="https://github.com/undavide/sublime-ps-extendscript">find here</a>.
    </li>
    <li>
      It&#8217;s been updated to support Photoshop CC 2015.5.
    </li>
    <li>
      Simplified the code a lot
    </li>
  </ul>
  
  <p>
    <strong>Please refer to the GitHub repository for updated instruction</strong> – I keep the rest of the article as a document of the past.
  </p>
  
  <h2>
    Installation
  </h2>
  
  <p>
    Download <a title="ExtendScript-PS - Sublime Text 2 build package for Photoshop ExtendScript" href="https://github.com/undavide/sublime-ps-extendscript" target="_blank">ExtendScript-PS (V2.0 &#8211; April 2014, defaults to Photoshop CC, customizable for CS6)</a>, the Sublime Text 2 build package for Photoshop ExtendScript. Unzip it and move the <strong>ExtendScript-PS</strong> folder in:
  </p>
  
  <blockquote>
    <p>
      [OSX] ~/Library/Application Support/Sublime Text 2/Packages
    </p>
    
    <p>
      [Win] ~\AppData\Roaming\Sublime Text 2\Packages
    </p>
  </blockquote>
  
  <p>
    If you&#8217;re unsure where the Packages are located on your system, find them from the Sublime Text menu: <em>Preferences &#8211; Browse Packages&#8230;</em> Do an app restart, just in case!
  </p>
  
  <h2>
    How to use it
  </h2>
  
  <p>
    Write your ExtendScript as usual, and <strong>save</strong> the JSX file you&#8217;re working on (if you don&#8217;t, it won&#8217;t work).
  </p>
  
  <p>
    <img class="alignleft size-medium wp-image-2030" src="http://localhost:8888/wp-content/uploads/2013/05/BuildSystem-300x280.png" alt="Build System" width="300" height="280" srcset="http://localhost:8888/wp-content/uploads/2013/05/BuildSystem-300x280.png 300w, http://localhost:8888/wp-content/uploads/2013/05/BuildSystem-150x140.png 150w, http://localhost:8888/wp-content/uploads/2013/05/BuildSystem.png 420w" sizes="(max-width: 300px) 100vw, 300px" />Make sure the <em>Tools &#8211; Build System &#8211; ExtendScript_PS</em> is checked, then build the script with ⌘B (Mac), ctrl+B (PC) or the <em>Tools &#8211; Build</em> menu item.
  </p>
  
  <p>
    As you build it <span style="text-decoration: line-through;">the JSX script is copied to the Photoshop&#8217;s Presets/Scripts/ folder</span>, a confirmation message appears on the Sublime Text 2 console, then it&#8217;s run in the host application.
  </p>
  
  <p>
    Depending on your platform and/or PS version, a couple of tweaks may be needed in order to make the it run in your system: please open the ExtendScript-PS folder (in the Sublime Text 2 packages) and read along.
  </p>
  
  <h2>
    Mac Hacks
  </h2>
  
  <p>
    The build package, as it is, works on Photoshop CC, CS6 (and possibly CS5 too).<br /> In the <code>script.scpt</code> file make sure you are targeting the correct Photoshop version:
  </p>
  
  <pre class="theme:twilight font-size:14 line-height:18 toolbar:2 striped:false whitespace-before:1 whitespace-after:1 lang:applescript mark:10 decode:true">on run arg

  set theFile to arg's item 1

  open for access theFile
  set fileContents to (read theFile)
  close access theFile

  tell application "Adobe Photoshop CC"
    do javascript fileContents (* show debugger before running / never / on runtime error *)
    activate
  end tell

end run</pre>
  
  <p>
    Moreover (see line 10) you can uncomment <strong>show debugger</strong> to activate ExtendScript Toolkit: available options are either <em>before running</em>, <em>never</em>, <em>on runtime error</em>.
  </p>
  
  <h2>
    Windows Hacks
  </h2>
  
  <p>
    The build package, as it is, works on Photoshop CC (64 bit). I did test it for CS6 on a virtualized Windows 7, so feedback from actual PC users is welcome. Check that the <code>build.bat</code> file contains information that matches your system in these lines:
  </p>
  
  <pre class="theme:twilight font-size:14 line-height:18 toolbar:2 striped:false whitespace-before:1 whitespace-after:1 lang:default mark:3,6 highlight:0 decode:true ">:: Change this accordingly to your CS/CC version
set version=CC

:: Adobe Photoshop folder location 64 bit versions:
set ps_folder_path=c:\Program Files\Adobe\Adobe Photoshop %version% (64 Bit)

:: Adobe Photoshop folder location 32 bit versions:
:: set ps_folder_path=c:\Program Files (x86)\Adobe\Adobe Photoshop %version%</pre>
  
  <p>
    Particularly, set the correct:
  </p>
  
  <ul>
    <li>
      <span style="line-height: 13px;">Photoshop version (CS5, CS6, CC&#8230;) which has to match the application&#8217;s folder name.</span>
    </li>
    <li>
      Photoshop folder (in the comments there&#8217;s the default 32 bit path if you need it) &#8211; change it accordingly if PS has been installed in a custom disk/directory.
    </li>
  </ul>
  
  <h2>
    Credits
  </h2>
  
  <p>
    I&#8217;ve cloned from GitHub the original <a title="After Effects Sublime Text 2 package" href="https://github.com/seblavoie/After-Effects-Scripting-Sublime-Text-Package" target="_blank">Adobe After Effects package for Sublime Text 2</a> made by <a title="Sebastien Lavoie" href="http://seblavoie.com" target="_blank">Sébastien Lavoie</a> &#8211; which I have tweaked to fit Photoshop&#8217;s scripting needs (Version 1 in May 2013, Version 2 in April 2014).
  </p>
  
  <p>
    I&#8217;m a poor hacker, so if the package doesn&#8217;t work properly it&#8217;s my fault only. Feedback is welcome as usual, happy coding!
  </p>
</div>

<!-- Share-Widget Button BEGIN --><a href="javascript:void(0);" myshare\_id="mys\_shareit" myshare\_url="http://localhost:8888/2013/05/sublime-text-2-extendscript-photoshop-build-package/" myshare\_title="Sublime Text 2 and 3 ExtendScript Photoshop package (Updated)" rel="nofollow" onclick=" return false;" style="text-decoration:none; color:#000000; font-size:11px; line-height:20px;"> 

<img src="http://localhost:8888/wp-content/plugins/share-widget/img/share-button-white-small.png" height="20" alt="Share" style="border:0" /> </a> <!-- Share-Widget Button END -->