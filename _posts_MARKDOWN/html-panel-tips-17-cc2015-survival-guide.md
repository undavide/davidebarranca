---
title: " HTML Panel Tips #17: CC2015 Survival Guide\t\t"
tags:
  - Adobe Extension Manager
  - CC2015
  - Creative Cloud
  - PlayerDebugMode
url: 2831.html
id: 2831
category:
  - Coding
  - HTML Panels
date: 2015-06-16 22:05:54
---

Superstitious people in Italy think that seventeen is a bad luck number - yet I'm not gullible and I think it must be _by chance_ that this Tip #17 is about the apparently, ehm, _troublesome_ update to Creative Cloud 2015. As follows is a **checklist of common problems** (both from the developer's and user's point of view - I've been in touch with quite a number of them both) and my own suggestions to [stop worrying and love the bomb](https://en.wikipedia.org/wiki/Dr._Strangelove "Dr. Strangelove"). **(UPDATED Jul 5th, 2015)**

1\. My \[insert name here\] Product doesn't work anymore!
---------------------------------------------------------

Why should it? I tell you why, it's a matter of mismatching expectations. The Creative Cloud app tells you it will port your Preferences: you would expect that this means Scripts-Plugins too but alas it's not the case. Moreover, by default (what a tumble!) the CC app **removes old versions** of Photoshop CC, so that you can't just copy Scripts and Plugins from CC2014 on CC2015. **Developer**: advise your customers to look in the Advanced properties of the CC update application and uncheck "Remove previous versions" so that they have access to stuff installed within the Photoshop CC2014 folder. **User**: look in the `Photoshop CC2014/Presets/Scripts/` for Script products, and/or `Photoshop CC2014/Plug-ins/` and manually copy sensible stuff from there into the corresponding Photoshop CC2015/ paths - ask your developer for help.

2\. Adobe Extension Manager says the product is installed but it isn't!
-----------------------------------------------------------------------

You might have not heard about this, but **Adobe Extension Manager** (AEM) has reached his [EOL](https://www.adobeexchange.com/resources/27 "Adobe Extension Manager funeral party") (End Of Life) - that is, it's not going to be supported by Adobe anymore. AEM doesn't know about CC2015, **it's of no use to install CC2015 products** \- more about it below. **Developer**: it's somehow _by chance_ (or by design, depending on your POV) that your Panel shows up in CC2015, because the path for CC2014 and CC2015 is shared: Mac: `~/Library/Application Support/Adobe/CEP/extensions` Win: `C:\<username>\AppData\Roaming\Adobe\CEP\extensions` Mind you: if your panel is self-contained (doesn't require files such as scripts installed within the Photoshop folder, like JSXs in `Presets/Scripts/` or plugins in `Plug-ins/`) it will work on CC2015. Otherwise, you need to move the required files manually in order for the panel to be functional. The same applies for Plug-ins and Scripts. **User**: ask your developer if you think the Panel is not working properly even if it shows up, and ask for CC2015 installers (yes, you need to install them on the new version.

3\. The \[insert name here\] Panel says it's not properly signed and/or timestamped!
------------------------------------------------------------------------------------

My friend, have you signed and timestamped your panel? Chances are you've not - or possibly you are used to keep the debug flag on (CC2014) on your machine and your users don't - and you don't have it either on CC2015. CEP panels need to be signed/timestamped - I personally do this via `ZXPSignCmd` commandline tool, you'll find all the excruciating details [here](http://localhost:8888/2014/05/html-panels-tips-10-packaging-zxp-installers/ "HTML Panels Tips: #10 Packaging / ZXP Installers")). **Developer**: Build a signed/timestamped ZXP, rename it as ZIP, unZip it (OSX won't let you do this because his inner soul is mean, download any third party free app such as [The Unarchiver](https://itunes.apple.com/it/app/the-unarchiver/id425424353?mt=12 "The Unarchiver (OSX)")) and now distribute the folder for manual installation in the usual paths (see #2 above). You'll notice that a signed/timestamped panel contains a `META-INF` folder and a `mimetype` file: take care of these guys. Please notice that:

*   You modify a single file in a signed/timestamped panel (such as the manifest.xml to add compatibility to CC2015) and the signing/timestamping is screwed, error will follow and the panel won't show up anymore.
*   You let the uncompressed panel lie in a Google Drive folder shared with somebody else, and the signing/timestamping is screwed as well (true! It took me forever to get this one, still I don't know why it happens).
*   Perhaps if you stare at a panel long enough, the signing and timestamping will break into tears.

**User**: ask the developer for a manual installation Panel (see #2 for the installation paths).

4\. Do I have to modify the Panel's manifest.xml?
-------------------------------------------------

It depends. I never explicitly set an upper limit, so my panels will hopefully load in CC2015 as well:

<HostList>
    <Host Name="PHXS" Version="14.0" />
    <Host Name="PHSP" Version="14.0" />
</HostList>

The above means: from 14 upwards. Same story if you use a MXI file for hybrid panels:

<products>
    <product familyname="Photoshop" version="14.0"/>
</products>

5\. How do I debug on CC2015?
-----------------------------

Set the **PlayerDebugMode** as a String with value "1" in: Win: `regedit > HKEY_CURRENT_USER/Software/Adobe/CSXS.6` Mac: `/Users/<username>/Library/Preferences/com.adobe.CSXS.6.plist` Let me read your mind: `com.adobe.CSXS.6` doesn't exist. Brave developer, clone it from CSXS.5 and rename it, you'll be forgiven. Mind you: Starting with Mac 10.9, Apple introduced a caching mechanism for plist files. Your modifications to plist files do not take effect until the cache gets updated (on a periodic basis, you cannot know exactly when the update will happen). To make sure your modifications take effect, kill all `cfprefsd` processes. They will restart automatically.

6\. How do I let users install \[insert name here\] if Adobe Extension Manager is gone?
---------------------------------------------------------------------------------------

I've had this thought myself too, right after the eleventh glass of wine at the AEM Funeral Party. The developer community is facing this problem and to date several possibilities are on the table:

*   Manual installation: you let users move files around - it's not needed that I list pros and cons.
*   Native installers: you're braver than me. If you have reliable info on this process, please let me know in the comments
*   Scripted installers: I've built one (free and opensource, please find it [here](http://github.com/undavide/PS-Installer "PS Installer")). The idea is simple: ExtendScript knows File Management, so Photoshop itself can deploy the needed files. You let users download a ZIP, which contains an Assets folder and an Installer.jsx file - they drag and drop it into Photoshop and it'll do the magic.
*   Extension Manager CommandLine utility: not ready for public download when I' writing this but it will in a short while. It's basically the operational core of AEM in all the beauty and appeal of a CLI. You're allowed to send users a ZIP packed with your usual product's ZXP, the Adobe CLI plus a Installer.sh (OSX) or a Installer.bat (Windows) which runs the CLI with the right params.
*   Other opensource projects still under development, like [this one](http://unhurdle/creative-installer "UnHurdle Creative Installer").

My own preference goes (for obvious reasons) to Scripted installer, then Manual installation, then AEM CLI, then horse shoeing.

7\. How do I sell through Adobe Add-ons if Extension Manager is gone?
---------------------------------------------------------------------

Adobe Add-ons relies on the Creative Cloud app to deploy files, via the File Synch feature (let users check it's enabled) - they've somehow transplanted the AEM core into the CC app. So keep building your ZXP files and life will be good.

8\. My \[insert name here\] Script behaves badly
------------------------------------------------

From Photoshop CC2015 onwards, Adobe's engineers have introduced the new **Mondo** rendering engine for ScriptUI dialogs. Previously it was Flash, and... you know. Mondo is the same framework used in Photoshop actual Filters and Plug-ins, so we're allowed to guess that engineers will take care of it. I've to tell you that, while theoretically Mondo implements the same set of features that the one before it, actually it isn't completely so (I'll be blogposting in the near future about this). So:

*   The appearance of your Scripted Dialogs will be different
*   The functionality of your Scripted Dialogs might be different

And by functionality I mean: ScriptUI can't load JPG images now, only PNGs - so if your dialog relies on JPGs, it will fail on CC2015. (This is going to be fixed in the future).

9\. Some of my users say that their Adobe Extension Manager lists CC2015 as well!
---------------------------------------------------------------------------------

Cool down the enthusiasm, it's a false positive - Adobe said that:

> The fact that it still seems to work for CC 2015 in some cases but not others, is because the Exchange Plugin, the component in the Creative Cloud desktop app that contains the ”brains” of Extension Manager and allows extensions to be automatically installed via Add-ons, automatically updates the Extension Manager Database. In some cases this ”just works” as if Extension Manager fully supported CC 2015. It should work more often on Windows than Mac. Having said that, **it’s not a supported workflow**.

(Bold is mine)

10. Timestamp failure when building the ZXP
-------------------------------------------

You run into "Error - the timestamp returned from the chosen TSA could not be verified, so the ZXP created is likely to be rejected by other tools. Please create your ZXP with a different trusted TSA.", right? Please make sure you're using CC2015 version of the ZXPSignCmd, grab the updated version from the CEP Github Repo ([MAC](https://github.com/Adobe-CEP/CEP-Resources/blob/master/ZXPSignCMD/ZXPSignCmd.dmg?raw=true) and [PC](https://github.com/Adobe-CEP/CEP-Resources/blob/master/ZXPSignCMD/ZXPSignCmd.exe?raw=true)).

11\. Extension Manager Opensource replacement
---------------------------------------------

Developer Max Penson has come up with the free [ZXP Installer](http://zxpinstaller.com), a native app (Mac/PC) which runs under the hood the new ExManCmd commandline utility. Drag and Drop your ZXP and voilà! It still under development, but it works and you can point your customers to it. (if, for some reason out of your control, you happen to have problems with your Extension Manager \*and\* Creative Cloud database files, it's going to be bad times. Been there, seen things, wasted an awful amount of time).

12. After installing CC2015, my \[insert name here\] doesn't work anymore on CC2014
-----------------------------------------------------------------------------------

I don't know how on earth this can happen, but I've seen it myself (in my own case, an entire folder in the Presets/Script disappeared causing an HTML Panel failure).

13\. More to come
-----------------

I'm adding stuff to the list (already 4 extra topic since the original draft). Take care and have fun keeping up to date :-)
