---
title: " A New Course on Adobe CEP Panels: Native Installers and Automation!\t\t"
tags:
  - Adobe
  - automation
  - CEP
  - course
  - html panels
  - Native Installer
url: 3195.html
id: 3195
category:
  - Coding
  - HTML Panels
date: 2018-01-23 14:37:47
---

After [Adobe Photoshop HTML Panels Development](http://htmlpanelsbook.com/), I've now published a new Course, titled "The Ultimate Guide to Native Installers and Automated Build Systems"! ![](http://localhost:8888/wp-content/uploads/2018/01/Book_Cover_Blog.jpg) For which title the gods of software development and copywriting will forgive me. Curious? In the business of Adobe HTML Panels, sooner or later you'll ask yourself: "how the heck do I do... _whatever it must be done_ to let my customers run my software on their Computers?" The typical stream of consciousness is as follows. I know I know: it's just a matter of moving a folder to the right place, isn't it? And perhaps few extra assets here and there... of course, I don't want other developers to reverse engineer my own code, so it must be protected somehow. OK, I forgot signing and timestamping. And Code Certificates: should I buy one? Which one? Do they all work the same? I've heard that some classy people build Native Installers, like Apple and Windows programs with nice graphic interfaces, that defeat Gatekeeper and SmartScreen security warnings like a pro! Gee, do I have to go through all this, by hand, all the time? Each time I change my code, on each product? For real? Stream of consciousness ends on a sad note. Hey, look at that! (Spring from Vivaldi's The Four Seasons plays in background) ![](http://localhost:8888/wp-content/uploads/2018/01/UG.gif) This is what the cool guys are doing. Type a command, and look at things that build themselves. What is looping above is a sample project, including:

*   **Input**:
    *   HTML Panel
    *   Multiplatform Photoshop Plugin as `.plugin` and `.8bf` files (optional).
    *   Readme documentation as a `.pdf` file (optional).
*   **Output**
    *   Signed and Timestamped Panel folder with:
        *   `.js` files protected with advanced obfuscation algorithms.
        *   `.jsx` files exported as `.jsxbin` with different advanced obfuscation algorithms.
    *   Zipped version for convenience.
    *   Hybrid Extension as a `.zxp` file, ready for submission on Adobe Add-ons.
    *   Mac `.dmg` package, that in turn contains:
        *   a `.pkg` native installer,
        *   an uninstaller application,
        *   the `.pdf` documentation.
    *   Windows `.exe` native installer that deploys Panel, Photoshop Plugin and Documentation
        *   Uninstaller provided in Windows' Control Panel.
    *   Both platforms' native installers are signed with paid Code Certificates to comply with Windows and macOS security policies.

After many years of trial and errors, I've finally settled to the above setup for my own commercial products, which takes 20 seconds of computer work to automatically build all the above. The final result looks like that, first on Mac: \[video width="600" height="400" mp4="http://localhost:8888/wp-content/uploads/2018/01/UGMac.mp4" poster="http://localhost:8888/wp-content/uploads/2018/01/Poster.jpg"\]\[/video\] Then on Windows: \[video width="600" height="400" mp4="http://localhost:8888/wp-content/uploads/2018/01/Windows.mp4"\]\[/video\]

The Course
----------

How to get there? I've explained it all, step by step, in **The Ultimate Guide to Native Installers and Automated Build Systems**, my new Course! ![](http://localhost:8888/wp-content/uploads/2018/01/shot.jpg) It comes as a **PDF ebook, bundled with the entire Sample Project code** – both the dummy Panel and assets, plus the automation. The course draws the _big picture_ first, aka what needs to be done; then shows you how to manually perform each step, why, suggesting best practice and which software to use; finally, it teaches you how to write and customize the code to build the entire automation pipeline. 

[Loading...](https://gumroad.com/l/swCoT/)

 

### FAQ

**Does the Course apply to Photoshop only?** I've used PS as my Adobe app of choice, but no, the course applies to _every_ CEP-enabled app you're developing extensions for. **Mac, Windows or both?** I'm on Mac: it's the only platform that allows you to build both Mac _and_ Windows installers; yet the automation code can run on Windows as well, with the exception of the Mac installers part. **Does the bundle include Code Certificates?** No. On the one side, paid Code Certificates are strongly suggested but optional. On the other side, I could not buy them in your place! In fact, it took me quite some time and effort to acquire them just for myself. **Does the Course cover Panels development?** This one is about installers and automation only. If you want the Big One (300 pages, 3 hours of videotutorials, 28 sample Panels code), head to the [Adobe Photoshop HTML Panels Development](http://htmlpanelsbook.com/) Course. **Why there's a 1 in the cover?** You have a sharp eye! I plan to expand the collection with more specific and smaller Courses like this one, in the future. Feel free to suggest topics you'd like to see covered in the comments! And yes, I have a 🦊 as the Terminal prompt, isn't it cute?

Thank you!
----------

Over the years, I've always posted articles on this blog as a way to give back to the developers' community what I've picked up myself in the first place – from Forums, chats, etc. Alas, we live in a (very) material and a (very) demanding world: projects like this one are fundamental for me to sustain my business and my family – and as a consequence maintain and update this blog too. Thank you in advance for your support! 🙏🏻It is really appreciated.