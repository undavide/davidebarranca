---
title: " Creative Suite extensibility and Photoshop\t\t"
tags:
  - Creative Suite
  - CS Extension Builder
  - CS SDK
  - scripting
url: 514.html
id: 514
comments: false
category:
  - Coding
  - Photoshop
date: 2011-12-11 21:52:01
---

Having read the (InDesign developer) [Gabe Harbs' post on the subject](http://in-tools.com/article/thoughts-on-extending-the-creative-suite/ "Thoughts on Extending the Creative Suite"), I'd like to add here a couple of personal points focusing on Photoshop.

CS extensions basics
--------------------

Few lines to frame the topic: Creative Suite extensibility as we know it today is far better than we could even dream few years ago. This happens because Macromedia technology has plugged into Creative Suite and mixed with Flex SDK, to finally mature and end up in a product called Extension Builder (a Flash Builder / Eclipse plugin) based on the new CS SDK. We're able to build panels that:

1.  (almost seamlessy) integrate with the host application UI;
    
2.  can drive Photoshop & C. just like scripting did;
    
3.  are based upon an OOP language such as ActionScript 3, and Flex services, to manage data;
    
4.  use Flash components for UI design;
    

Incredibly powerful tools.  Moreover, the [CS SDK team](http://blogs.adobe.com/cssdk/ "Adobe CS SDK official blog") led by PM [Gabriel Tavridis](http://twitter.com/#!/gtavridis "Gabriel Tavridis on Twitter") seems to be really motivated making the extensibility platform to evolve at a high rate, and they've a strong attitude to listen to developers suggestions: not only in terms of technical matters, but strategic ones too. Even though I've already mentioned all [my doubts about Adobe as a company](http://localhost:8888/2011/11/adobe-crossroads/ "Is Adobe at a crossroads?") elsewhere, I'd say: what not to like. Of course the path it's not entirely lined with roses - ever since CS4 the flash support for panels got better (possibly because it couldn't be worse ;-) ), but debugging customer's troubles on a remote machine is still a ruthless job.

*   On a Mac, apparently unrelated issues like system Fonts may lead to a blank panel, as corrupted preferences frequently do.
    
*   On a PC (from now on I'll be referring to Photoshop as the host application for CS extensions), depending whether a script is embedded within a panel or called as a script tout-court, [the behavior may be different](http://www.ps-scripts.com/bb/viewtopic.php?f=9&t=4489&sid=a77983ed5a8f6f8aebe91ed7790229c4 "PS-Scripts forum").
    
*   Adobe Extension Manager seems to be particularly hostile to unexperienced users - my topmost support request subject is related to installation via AEM. Recent [troubles with OSX Lion](http://localhost:8888/2011/10/adobe-extension-manager-and-lion-issue/ "Adobe Extension Manager and OSX Lion issue"), finally solved with an official update, may confirm that there's plenty of room for improvement.
    

That said, I know that the CS SDK team is working to address these technical issues and keep the CS extensibility a pleasing platform to work in.

Three ways
----------

A developer willing to code for Creative Suite applications can follow three paths (that may also cross):

1.  C++ (the old, steep way)
    
2.  Scripting (cross-platform JavaScript, mainly)
    
3.  CS SDK
    

The first one is for hardcore programmers (which I'm not). Since my friend [Marco Olivotto](http://www.marcoolivotto.com "Marco Olivotto") is on his way to coding a Photoshop plugin, I've heard from him one of the largest collection of cursing complaints ever, mostly focused on the documentation: obscure, missing, misleading, out of date, etc. While JS scripting may be used either as a quick, procedural way to drive the application or as a tool to build more complex systems, I've found that CS SDK and a true OOP language such as ActionScript better fit the needs of a coder (like me) just averagely skilled in both the programming and the DOM side of the job. Take a course on Java, like the great introductory [Stanford's "Programming Methodology"](http://see.stanford.edu/see/courseinfo.aspx?coll=824a47e1-135f-4508-a5aa-866adcae1111 "Stanford University - Programming methodology") (lectures by the volcanic professor Mehram Sahami) and you're ready to start with AS and extension development.

Extending Photoshop
-------------------

Personally, I see at least a couple of areas where CS (and especially Photoshop) extensibility may evolve in an interesting way.

### Integration

Generally speaking the CS SDK provides, amongst other things, the access to Photoshop DOM from ActionScript (plus a way to embed Javascript code too); there are a couple of problems IMHO:

1.  A minor one: Some functions simply don't traslate in AS successfully. For instance, PS .suspendHistory() exists as an AS method (like in JS) but doesn't work at all. _Yet_
    
2.  A crucial one: The CD SDK development is _unlinked_ to the actual application's scripting evolution.
    

The PS scripting community is constantly [gathering feature requests](http://www.ps-scripts.com/bb/viewtopic.php?f=36&t=3518&sid=986a4d430efdc3de891785c10bbd01a5 "PS-Scripts features wishlist") for Photoshop-next, even though scripting support doesn't look like one of the highest priority for the PS development team: a good amount of tools are not within the reach of the DOM, and can be accessed only via unfriendly Action Manager code. Whereas for instance cursor position or eyedropper sample size (see my open project [Power Info Palette](http://blog.rbg.bigano.com/2011/05/22/work-in-progress-power-info-palette/ "Power Info Palette")), just to mention a couple of items, are out of the reach of any code at all, i.e. you can't get that piece of information, period.

> I know it's not easy, but in my opinion it would be _far more productive_ if the CS SDK team could somehow collaborate with the application teams (PS, ID, etc) to develop and tightly integrate within the Actionscript layer new (and old) features that users are requesting. A bolder statement: if Adobe's promoting the CS SDK as the preferred way to extend Creative Suite applications, it should give to the team the possibility _to drive_ the evolution of each application's scripting development, not only the license to embed current features into the CS SDK, which we could take for granted.

I know that CS extensions are a huge leap forward themselves - however I believe that a mixed approach (with a personal bias to make the Tavridis' team the reference point for developers) would be the best way to exploit their power on the long run.

### Low level access

This is one of my ancient hobby horses. With InDesign scripting, which BTW I'm totally unexperienced of, a scripter can manipulate documents and all kind of objects (like frames) within the documents. And, as far as I know, any text that belongs to those frames. So, being InDesign (roughly speaking) a software about text management, you have a direct access to the smallest building block - i.e. characters. Translating to Photoshop, it's like if we could access the image's smallest building block, i.e. pixels. Which, sadly, we can't. We're allowed to duplicate documents, layers, apply PS filters to selections and channels - it's fine - but the active bitmap layer in PS can't (as far as I know) be directly grabbed and used by ActionScript as a BitmapImage. The only workaround, to the best of my knowledge is:

1.  save a temporary copy on the disk as an image;
    
2.  make the panel load and elaborate it;
    
3.  save a copy back on the disk;
    
4.  tell PS to load and paste it in the current document.
    

A pain. I knew that a direct sharing between Photoshop and an "extension" (back and forth, as a JPG flattened image) is allowed - within the Photoshop Touch SDK, _suddenly disappeared from the Adobe Devnet?!_ That is: mobile applications, those Touch apps Adobe is proud of. I also knew that [Daniel Koestler](http://blogs.adobe.com/koestler/2011/05/using-the-photoshop-touch-sdk-creating-a-project.html "Daniel Koestler Adobe blog") and [Renaun Erickson](http://renaun.com/blog/2011/04/photoshop-touch-sdk-contains-an-actionscript-3-library-too/ "Renaun Erickson blog") wrote the Actionscript API (which were in the Touch SDK that I downloaded in May 2011) - I'm just wondering and desperately hoping if we'll eventually see this little extra step in CS SDK as well.

> To let an extension have _direct access to a Photoshop layer as a BitmapImage_ would be terrific: it would mean the possibility to write actual PS plugins with Flash UI without the headache of dealing with C code (and memory management). It would imply the use of (binary, that is: protected) Pixel Bender kernels, plus a whole bunch of ready made ActionScript shaders. It would possibly result in lower performances compared to C++ plugins but frankly... who cares.

I thought to be the only one requesting this feature around, but a couple of emails from other developers pushed me to write this plea: Adobe, please, pretty please... :-)

The future of CS Extensibility
------------------------------

While I was in the middle of this post's writing, I mailed to Gabriel Tavridis some doubts that came to my mind - expecially after reading about the [Flash/Flex affaire](http://blogs.adobe.com/flex/2011/11/your-questions-about-flex.html "Adobe Flex blog"). It's clear that, even if Flex cannot be replaced by HTML5 in the short run, Adobe's commitment to this newer technology is quite strongly advertised. So what's the company's roadmap for CS extensions, I was asking. Gabriel kindly wrote back that he was going to post detailed answers to this (and similar) questions in the CS SDK official blog - which is [now online](http://blogs.adobe.com/cssdk/2011/12/cs_extensibility_and_flex_next.html "CS SDK future") and... I suggest you to read it! As a conclusion, no matter what technology we'll be using in the years to come, it's important that Adobe embraces a transparent attitude toward its users - as a developer, I'm happy to notice that the CS SDK group in this respect seems to be _avant-garde_.