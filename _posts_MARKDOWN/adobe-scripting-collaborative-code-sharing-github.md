---
title: " Adobe Scripting: collaborative code sharing\t\t"
tags:
  - Adobe
  - Extendscript
  - extensions
  - GitHub
  - scripting
url: 2373.html
id: 2373
category:
  - Coding
date: 2013-12-08 15:21:23
---

An open letter to fellow devs
-----------------------------

If you're involved building third party solution for Adobe applications (mainly Scripts/Extensions) and you care about it, please invest 10 minutes of your time, read along and chime in.

Hi, I'm Davide
--------------

I've started scripting Adobe apps (mainly PS) back in 2008, with no prior programming experience whatsoever. I work full time as a post-production freelance since the year 2000, so my main focus can't be on coding; yet over the years I've learned, mostly from others in public forums, enough to start a small paid scripts/extensions business that so far has been contributing for about 1:5 on average to my total incomes. **As a senior PS professional I can't consider myself but as an amateur coder**, yet I'm very passionate (isn't passion what amateurs are about, after all?!) What follows are **my own and personal thoughts**, that I've shared privately with some folks in the industry: nobody put me down (on the contrary, they strangely seemed to sustain my point of view!) so I've decided to eventually write this one publicly.

The Scripting communities
-------------------------

I can't speak but about PS people; yet I've been in touch with many InDesign coders lately - from whom I got the personal, possibly incorrect impression that their community is _stronger_: in terms of available resources produced (forum threads, free and paid products, documentation), vitality, and, well, collaboration and _sense of community_. As a result I got the personal, possibly incorrect impression that they have **more influence on Adobe** as a software company. Mind you, **developers** (no matter whether PS-ID-IL-AE-centric) **understand they are just a tiny fraction of the company's users base** and it's not from them the cash flow comes - hence their needs aren't top priority, so to speak. We know. The Photoshop scripting community, and by scripting I mean basically everything in the extensibility layer but C/C++ plugins (**ExtendScript** and **Flash/HTML Panels**, **Generator/Node.js**) do have some essential and well known keystones: preferences may vary, but I confidently list first the independent [ps-scripts.com](http://www.ps-scripts.com "PS Scripts forum") forum moderated by **Mike Hall**, which is where **Ross Huitt** (aka XBytor) has been sharing his great _XTools_ and many skilled and very talented people constantly keep contributing. The official [PS Scripting forum by Adobe](http://forums.adobe.com/community/photoshop/photoshop_scripting "Adobe PS Scripting") (where in my opinion the signal/noise ratio is a bit lower). And lots of other personal websites devoted to PS scripting: Adobe's [Jeffrey Tranberry](http://www.tranberry.com/photoshop/photoshop_scripting/index.html "Jeffrey Tranberry"), [Russell Preston Brown](http://russellbrown.com/scripts.html "Dr. Brown Scripts") (aka. Dr. Brown), [Tom Krcha](http://tomkrcha.com "Tom Krcha blog"), [David Deraedt](http://www.dehats.com "David Deraedt blog"); [Paul Riggott](http://www.ps-bridge-scripts.talktalk.net "Paul Riggott"), [Michel Mariani](http://www.tonton-pixel.com/blog/ "Michel Mariani (aka Mikaeru) "), [Trevor Morris](http://morris-photographics.com/photoshop/scripts/ "Trevor Morris Scripts"), [John J. McAssey](http://www.mouseprints.net/Photoshop.html "John J. McAssey") and many others  - I try to do my little part too.

Bad times?
----------

Undoubtedly the PS Scripting community is undergoing _some change_. **Paul Riggott**, among the most skilled and helpful contributors, [left forums altogether](http://forums.adobe.com/message/5394706 "Adobe Photoshop Forums") in protest with Adobe. **XBytor** declared [to be burned out](http://www.ps-scripts.com/bb/viewtopic.php?f=2&t=4466&p=26717#p26623 "PS-Scripts") and not willing to start any major new project. In an official forum's thread called "[Will scripting be supported in future?](http://forums.adobe.com/message/5707211 "Will scripting be supported in future?")" (worth reading too) **John McAssey** collected bits of different posts showing (some influent) community members' _bad mood_ \- so to speak. Private conversations I've had with people in this business confirmed that, generally speaking, there's **a lot of disaffection** going on among developers: recent technologies Adobe rolls out of its hat are promising ([HTML Extensions](http://blogs.adobe.com/cssdk/2013/09/introducing-html5-extensions.html "HTML Extensions"), [Generator/Node.js](https://github.com/adobe-photoshop/ "Adobe Generator on GitHub")) yet the past has taught us not to embrace them too enthusiastically nor too soon, because they might end unsupported in the mid-term. In a dog-chasing-its-tail loop that could even accelerate the process: take as an example [Kevin Goldsmith](http://blogs.adobe.com/kevin-goldsmith "Kevin Goldsmith blog")'s [PixelBender](http://blogs.adobe.com/pixel-bender/ "Pixel Bender"), which never left the Labs and drop discontinued. Was that because Adobe judged the coders' involvement too low to justify the development cost (as it's been declared) or are the developers, who weren't embracing the technology because Adobe didn't put enough resources on it in order to make it really profitable? That was well before the [Flash thing](http://arstechnica.com/business/2011/11/adobe-donates-flex-to-foundation-in-community-friendly-exit-strategy/ "Adobe donates Flex to Apache foundations"), which is by the way going to put an end to Flex Panels and Configurator very soon. True, IT world is constantly moving on, but as someone who's learned ActionScript from scratch "just" to code Flex panels back in CS4, I now approach HTML Panels in CC (three generations afterwards) and Generator with a tad of cold blood - I'm working full-time, married, in my late thirties and time is not exactly what I'm plenty of.

Cahiers de doléances
--------------------

Bear with me and let me express some **personal criticism** before getting into some more pro-active, constructive comments. Here are my own [Cahiers de doléances](http://en.wikipedia.org/wiki/Cahiers_de_doléances "Cahiers de doléances"), provided that I assume engineers don't like not to fix bugs, nor avoid implementing new features just to liven up the otherwise boring life of the third party developer - it's management that decides where to put (or not to put) development resources.

1.  Scripting as we know it (i.e. **ExtendScript**) needs some love. It's a language with its own true gems - as pointed out by the ID developer [Marc Autret](http://www.indiscripts.com "Marc Autret's Indiscripts") (who proved to be among the finest scripting connoisseurs): the JSXBIN compiler, File/Folder management, Reflection and Dictionary interfaces, Debugging/tracking services, preprocessor directives, native XML support, etc. - and some nightmares. Now that scripters are supposed to _interbreed_ with web-devs for HTML panels (running on a [Google Chromium](http://www.chromium.org "Google Chromium") implementation / V8 engine) at least in the long run the way data is passed/evaluated by the two engines should be unified - and ExtendScript upgraded to latest ECMAScript standards without losing its peculiarities.
2.  **ScriptUI is a mess**. Hopefully the switch to an HTML adapter that [Tom Ruark's envisioned](http://forums.adobe.com/message/5706861#5706861 "Tom Ruark on HTML adapters") will end up fixing the undisguised rendering differences and behavior discrepancies we're drowning into now: inter-app (PS/ID/IL etc.), inter-platform (PS Mac/Win), inter-version (PS CSx/CC), it's a minefield. I've been told that while ExtendScript specs are the Standard to refer to, each app's team (PS/ID/IL/...) deals with its implementation independently: as a result, the consistency one would expect for a set of applications formerly knows as Creative _Suite_ vanishes.
3.  **ESTK (ExtendScript ToolKit)** is... rather _raw_ as an IDE for 2014 standards, suffers from many (old) bugs, code hinting is often inaccurate, it doesn't provide any ScriptUI visual tooling, not to mention the fact its own ScriptUI implementation differs from all the other apps' (aspect/behavior) implementation, which makes the whole thing quite pointless, and sometimes dangerous.
4.  PS scripting would greatly benefit from the implementation of some kind of **Image Processing language** available through scripting. It's quite paradoxical that HTML Canvas element + JS (that is: HTML Panels!) can do image processing yet there's no straightforward way to pass back and forth the open Document's BitmapData. As far as I know, one could even port [Processing](http://processingjs.org "Processing JS") code that way - wouldn't that be _just awesome_?
5.  **ActionManager code** and the ScriptingListener plugin are a godsend, but first: there are many useful things beyond the AM reach, second: AM isn't an excuse not to extend the DOM support anymore.
6.  **HTML Panels** are a wise move in the long run for sure - provided their implementation will overcome several teething problems and lack of documentation (I'm among the ones who remember the amazing [series of articles](http://blogs.adobe.com/cssdk/author/okvern "Olav Kvern on CS SDK topics") written by the great Olav Kvern on CS SDK). Which is OK to deal with, unless the deadline for the Flex panels support in PS is [Mid 2014](http://forums.adobe.com/thread/1298733 "Photoshop Flex panel support stops in mid 2014") (that is: tomorrow morning, so to speak). Extensions cannot replace scripted windows (that are [action recordable](http://localhost:8888/2012/11/action-recordable-scripts-in-photoshop/ "Action recordable scripts in Photoshop") for instance), but are _interfaces_ to, and evaluates, "traditional" scripting.

Others might have a different list of doléances, the above ones are just my favorite refrain.

Call for (some) action!
-----------------------

Notwithstanding, the PS scripting community is doing some interesting stuff and is spread over the internet under many forms. Yet **all the available resources I've mentioned earlier, while undoubtedly valuable, appears very sparse**. They're able to answer to the question: "_How do I...?_" with snippets and explanation or just exposing source code, but mostly it's _just_ a matter of personal efforts. Disconnected from other personal efforts - by someone else - and for that reason less effective from the educational point of view. Forums somehow fill the gap: it's where you go asking questions and interacting with people, who are either likely to have run into your problem, or just more experienced to suggest a solution. Forums are an excellent _archive_, collecting the community's accumulated knowledge, providing historically sorted references and discussions. Yet, as a code repository, Forums cannot beat other tools that have been specifically designed to be exactly that: **code repositories**. I have nothing revolutionary to propose: I've noticed how people involved in PS coding are sometimes very talented but either isolated, or interacting in ways that could be more profitable for the whole community - **specifically for those who are _not yet in the number_, but will enter the community in the future**: willing to learn and to make use of our accumulated knowledge.

Open Source Code Repositories
-----------------------------

Over the years I've collected tons of Scripting related material, and you're likely to have done the very same thing yourself. Why don't we create a set of **collaboratively maintained, open sourced GitHub repositories**, to store:

*   Snippets (functions, AM code, etc).
*   Boilerplate code.
*   Demo code and Tutorials/HOWTOs.
*   Libraries and Frameworks.
*   Links to notable Forum Threads, valuable external resources, etc.
*   Official and Unofficial Documentation.
*   Community's feature requests.
*   You name it.

**I have no intention to create the umpteenth PS-website, nor to substitute Forums **(which keep being the best place for discussion to happen and ideas to be shared): I'd like to have a place to go to when I'm in need to borrow or learn code from (a-ehm, what [Adobe's DevNet](http://adobe.com/devnet/photoshop/scripting.html "Adobe Photoshop Scripting DevNet") should have been). I'm not exactly the smartest [Git](http://git-scm.com "Git") user in town, but I know that [GitHub](http://www.github.com "GitHub") repos:

1.  Are **forkable** (you can clone it locally).
2.  There is a group of **maintainers** who is in charge of the not obvious problem of their design/structure.
3.  Allow **Pull requests**: that is, you can submit your own tweaks in order to have them integrated into the main line of development.
4.  Can contain not only sheer code but HTML/markdown documents - i.e. all sort of **Documentation**, Tutorials, etc.
5.  Provide collaborative coding tools such as **Wiki**, **Issues tracking**, etc.
6.  Are **free and open source** \- you give bits of your time and knowledge for the whole community's benefit.
7.  Can **scale** easily to include other apps (ID/IL/AE/...)
8.  Because they have been designed as a repositories, code there is **version tracked** and can be maintained more effectively (compared to Forums, where not very often old threads get revamped).

Chime in!
---------

When I've happened to code commercial scripts I've tried to build tools that I would have liked to use myself - and this is the spirit I'm asking you for support with. Looks like a complex thing to build from the ground up, yet it would help not only ourselves but those approaching scripting in the future too (and it doesn't need to be built overnight); not to mention the fact that there could be a tiny chance this **might strengthen the Devs Community to Adobe's eyes** \- I don't want to create a lobby, nor find a place to fill with narcissism: I'm a happy sociopath who lives in the countryside. I would like to consider Adobe Scripting a nice and profitable platform to develop for in the next, say, 5 years. To date, I'm not sure I would, frankly. I'm still in a phase where I'm evaluating whether this project can arouse other people's interest - **it can't be but a collaborative effort** \- or not (in case I'll easily turn it into a personal hobby). I'm skating over the next big problem on the list, i.e. the design of such repository. **Would you like to chime in?** Do you have _any_ idea, feedback, suggestion along these lines? Can you provide GitHub/social coding experience and skills? Just want to high five and disappear? The comment section below is for you. Thank you for your time, Davide