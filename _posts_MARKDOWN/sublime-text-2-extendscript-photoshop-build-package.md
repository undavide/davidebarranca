---
title: " Sublime Text 2 and 3 ExtendScript Photoshop package (Updated)\t\t"
tags:
  - build
  - Extendscript
  - package
  - Sublime Text 2
url: 2019.html
id: 2019
category:
  - Coding
  - ExtendScript / Javascript
date: 2013-05-05 23:16:54
---

This post contains a [Sublime Text 2](http://www.sublimetext.com/ "Sublime Text website") custom build package that lets you run ExtendScript code targeting Adobe Photoshop. I've coded it applying few tweaks and customization to an existing package made by [Sébastien Lavoie](http://seblavoie.com "Sebastien Lavoie website") for After Effects scripting. 

What is that for?
-----------------

When you code Photoshop scripting, chances are that you do it with the **ExtendScript ToolKit** (aka ESTK). While this Adobe provided piece of software has some unique features that make it very efficient while debugging and looking for DOM documentation, as an editor it really sucks. I'm sorry about that, but compared to other modern editors it's barely usable. [Sublime Text 2](http://www.sublimetext.com/ "Sublime Text website"), (and the 3 beta) conversely, are an amazing editor with tons of features and plugins. Yet so far the only workflow for Photoshop scripters was to code ExtendScript (which is a superset of Javascript) in Sublime Text, then copy and paste it to ESTK and run the script from ESTK. This custom build package lets you run ExtendScript code targeting Photoshop in Sublime Text 2 and 3. Type ⌘B (or ctrl+B) and your code is being executed in the host app!

UPDATE (September 2016)
-----------------------

I've tweaked the Package described in this article, originally written in 2013. Important news:

*   The code is now hosted and maintained in a **GitHub repository** that you can [find here](https://github.com/undavide/sublime-ps-extendscript).
*   It's been updated to support Photoshop CC 2015.5.
*   Simplified the code a lot

**Please refer to the GitHub repository for updated instruction** – I keep the rest of the article as a document of the past.

Installation
------------

Download [ExtendScript-PS (V2.0 - April 2014, defaults to Photoshop CC, customizable for CS6)](https://github.com/undavide/sublime-ps-extendscript "ExtendScript-PS - Sublime Text 2 build package for Photoshop ExtendScript"), the Sublime Text 2 build package for Photoshop ExtendScript. Unzip it and move the **ExtendScript-PS** folder in:

> \[OSX\] ~/Library/Application Support/Sublime Text 2/Packages \[Win\] ~\\AppData\\Roaming\\Sublime Text 2\\Packages

If you're unsure where the Packages are located on your system, find them from the Sublime Text menu: _Preferences - Browse Packages..._ Do an app restart, just in case!

How to use it
-------------

Write your ExtendScript as usual, and **save** the JSX file you're working on (if you don't, it won't work). ![Build System](http://localhost:8888/wp-content/uploads/2013/05/BuildSystem-300x280.png)Make sure the _Tools - Build System - ExtendScript_PS_ is checked, then build the script with ⌘B (Mac), ctrl+B (PC) or the _Tools - Build_ menu item. As you build it the JSX script is copied to the Photoshop's Presets/Scripts/ folder, a confirmation message appears on the Sublime Text 2 console, then it's run in the host application. Depending on your platform and/or PS version, a couple of tweaks may be needed in order to make the it run in your system: please open the ExtendScript-PS folder (in the Sublime Text 2 packages) and read along.

Mac Hacks
---------

The build package, as it is, works on Photoshop CC, CS6 (and possibly CS5 too). In the `script.scpt` file make sure you are targeting the correct Photoshop version:

on run arg

  set theFile to arg's item 1

  open for access theFile
  set fileContents to (read theFile)
  close access theFile

  tell application "Adobe Photoshop CC"
    do javascript fileContents (* show debugger before running / never / on runtime error *)
    activate
  end tell

end run

Moreover (see line 10) you can uncomment **show debugger** to activate ExtendScript Toolkit: available options are either _before running_, _never_, _on runtime error_.

Windows Hacks
-------------

The build package, as it is, works on Photoshop CC (64 bit). I did test it for CS6 on a virtualized Windows 7, so feedback from actual PC users is welcome. Check that the `build.bat` file contains information that matches your system in these lines:

:: Change this accordingly to your CS/CC version
set version=CC

:: Adobe Photoshop folder location 64 bit versions:
set ps\_folder\_path=c:\\Program Files\\Adobe\\Adobe Photoshop %version% (64 Bit)

:: Adobe Photoshop folder location 32 bit versions:
:: set ps\_folder\_path=c:\\Program Files (x86)\\Adobe\\Adobe Photoshop %version%

Particularly, set the correct:

*   Photoshop version (CS5, CS6, CC...) which has to match the application's folder name.
*   Photoshop folder (in the comments there's the default 32 bit path if you need it) - change it accordingly if PS has been installed in a custom disk/directory.

Credits
-------

I've cloned from GitHub the original [Adobe After Effects package for Sublime Text 2](https://github.com/seblavoie/After-Effects-Scripting-Sublime-Text-Package "After Effects Sublime Text 2 package") made by [Sébastien Lavoie](http://seblavoie.com "Sebastien Lavoie") \- which I have tweaked to fit Photoshop's scripting needs (Version 1 in May 2013, Version 2 in April 2014). I'm a poor hacker, so if the package doesn't work properly it's my fault only. Feedback is welcome as usual, happy coding!