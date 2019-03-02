---
id: 514
title: Creative Suite extensibility and Photoshop
date: 2011-12-11T21:52:01+01:00
author: Davide Barranca
excerpt: Personal thoughts about Adobe Creative Suite extensibility, focusing on Photoshop.
layout: post
guid: http://www.davidebarranca.com/?p=514
permalink: /2011/12/creative-suite-extensibility-and-photoshop/
category:
  - Coding
  - Photoshop
tags:
  - Creative Suite
  - CS Extension Builder
  - CS SDK
  - scripting
---
<div class="pf-content">
  <p>
    Having read the (InDesign developer) <a title="Thoughts on Extending the Creative Suite" href="http://in-tools.com/article/thoughts-on-extending-the-creative-suite/" target="_blank">Gabe Harbs&#8217; post on the subject</a>, I&#8217;d like to add here a couple of personal points focusing on Photoshop.
  </p>

  <h2>
    CS extensions basics
  </h2>

  <p>
    Few lines to frame the topic: Creative Suite extensibility as we know it today is far better than we could even dream few years ago.<br /> This happens because Macromedia technology has plugged into Creative Suite and mixed with Flex SDK, to finally mature and end up in a product called Extension Builder (a Flash Builder / Eclipse plugin) based on the new CS SDK.
  </p>

  <p>
    We&#8217;re able to build panels that:
  </p>

  <ol>
    <li>
      <p>
        (almost seamlessy) integrate with the host application UI;
      </p>
    </li>

    <li>
      <p>
        can drive Photoshop & C. just like scripting did;
      </p>
    </li>

    <li>
      <p>
        are based upon an OOP language such as ActionScript 3, and Flex services, to manage data;
      </p>
    </li>

    <li>
      <p>
        use Flash components for UI design;
      </p>
    </li>
  </ol>

  <p>
    Incredibly powerful tools.
  </p>

  <p>
    <!--more--> Moreover, the

    <a title="Adobe CS SDK official blog" href="http://blogs.adobe.com/cssdk/" target="_blank">CS SDK team</a> led by PM <a title="Gabriel Tavridis on Twitter" href="http://twitter.com/#!/gtavridis" target="_blank">Gabriel Tavridis</a> seems to be really motivated making the extensibility platform to evolve at a high rate, and they&#8217;ve a strong attitude to listen to developers suggestions: not only in terms of technical matters, but strategic ones too. Even though I&#8217;ve already mentioned all <a title="Is Adobe at a crossroads?" href="/2011/11/adobe-crossroads/" target="_blank">my doubts about Adobe as a company</a> elsewhere, I&#8217;d say: what not to like.
  </p>

  <p>
    Of course the path it&#8217;s not entirely lined with roses &#8211; ever since CS4 the flash support for panels got better (possibly because it couldn&#8217;t be worse 😉 ), but debugging customer&#8217;s troubles on a remote machine is still a ruthless job.
  </p>

  <ul>
    <li>
      <p>
        On a Mac, apparently unrelated issues like system Fonts may lead to a blank panel, as corrupted preferences frequently do.
      </p>
    </li>

    <li>
      <p>
        On a PC (from now on I&#8217;ll be referring to Photoshop as the host application for CS extensions), depending whether a script is embedded within a panel or called as a script tout-court, <a title="PS-Scripts forum" href="http://www.ps-scripts.com/bb/viewtopic.php?f=9&t=4489&sid=a77983ed5a8f6f8aebe91ed7790229c4" target="_blank">the behavior may be different</a>.
      </p>
    </li>

    <li>
      <p>
        Adobe Extension Manager seems to be particularly hostile to unexperienced users &#8211; my topmost support request subject is related to installation via AEM. Recent <a title="Adobe Extension Manager and OSX Lion issue" href="/2011/10/adobe-extension-manager-and-lion-issue/" target="_blank">troubles with OSX Lion</a>, finally solved with an official update, may confirm that there&#8217;s plenty of room for improvement.
      </p>
    </li>
  </ul>

  <p>
    That said, I know that the CS SDK team is working to address these technical issues and keep the CS extensibility a pleasing platform to work in.
  </p>

  <h2>
    Three ways
  </h2>

  <p>
    A developer willing to code for Creative Suite applications can follow three paths (that may also cross):
  </p>

  <ol>
    <li>
      <p>
        C++ (the old, steep way)
      </p>
    </li>

    <li>
      <p>
        Scripting (cross-platform JavaScript, mainly)
      </p>
    </li>

    <li>
      <p>
        CS SDK
      </p>
    </li>
  </ol>

  <p>
    The first one is for hardcore programmers (which I&#8217;m not). Since my friend <a title="Marco Olivotto" href="http://www.marcoolivotto.com" target="_blank">Marco Olivotto</a> is on his way to coding a Photoshop plugin, I&#8217;ve heard from him one of the largest collection of <del>cursing</del> complaints ever, mostly focused on the documentation: obscure, missing, misleading, out of date, etc.<br /> While JS scripting may be used either as a quick, procedural way to drive the application or as a tool to build more complex systems, I&#8217;ve found that CS SDK and a true OOP language such as ActionScript better fit the needs of a coder (like me) just averagely skilled in both the programming and the DOM side of the job.
  </p>

  <p>
    Take a course on Java, like the great introductory <a title="Stanford University - Programming methodology" href="http://see.stanford.edu/see/courseinfo.aspx?coll=824a47e1-135f-4508-a5aa-866adcae1111" target="_blank">Stanford&#8217;s &#8220;Programming Methodology&#8221;</a> (lectures by the volcanic professor Mehram Sahami) and you&#8217;re ready to start with AS and extension development.
  </p>

  <h2>
    Extending Photoshop
  </h2>

  <p>
    Personally, I see at least a couple of areas where CS (and especially Photoshop) extensibility may evolve in an interesting way.
  </p>

  <h3>
    Integration
  </h3>

  <p>
    Generally speaking the CS SDK provides, amongst other things, the access to Photoshop DOM from ActionScript (plus a way to embed Javascript code too); there are a couple of problems IMHO:
  </p>

  <ol>
    <li>
      <p>
        A minor one: Some functions simply don&#8217;t traslate in AS successfully. For instance, PS .suspendHistory() exists as an AS method (like in JS) but doesn&#8217;t work at all. <em>Yet</em>
      </p>
    </li>

    <li>
      <p>
        A crucial one: The CD SDK development is <em>unlinked</em> to the actual application&#8217;s scripting evolution.
      </p>
    </li>
  </ol>

  <p>
    The PS scripting community is constantly <a title="PS-Scripts features wishlist" href="http://www.ps-scripts.com/bb/viewtopic.php?f=36&t=3518&sid=986a4d430efdc3de891785c10bbd01a5" target="_blank">gathering feature requests</a> for Photoshop-next, even though scripting support doesn&#8217;t look like one of the highest priority for the PS development team: a good amount of tools are not within the reach of the DOM, and can be accessed only via unfriendly Action Manager code. Whereas for instance cursor position or eyedropper sample size (see my open project <a title="Power Info Palette" href="http://blog.rbg.bigano.com/2011/05/22/work-in-progress-power-info-palette/" target="_blank">Power Info Palette</a>), just to mention a couple of items, are out of the reach of any code at all, i.e. you can&#8217;t get that piece of information, period.
  </p>

  <blockquote>
    <p>
      I know it&#8217;s not easy, but in my opinion it would be <em>far more productive</em> if the CS SDK team could somehow collaborate with the application teams (PS, ID, etc) to develop and tightly integrate within the Actionscript layer new (and old) features that users are requesting. A bolder statement: if Adobe&#8217;s promoting the CS SDK as the preferred way to extend Creative Suite applications, it should give to the team the possibility <em>to drive</em> the evolution of each application&#8217;s scripting development, not only the license to embed current features into the CS SDK, which we could take for granted.
    </p>
  </blockquote>

  <p>
    I know that CS extensions are a huge leap forward themselves &#8211; however I believe that a mixed approach (with a personal bias to make the Tavridis&#8217; team the reference point for developers) would be the best way to exploit their power on the long run.<br /> <a name="low-level" id="low-level"></a>
  </p>

  <h3>
    Low level access
  </h3>

  <p>
    This is one of my ancient hobby horses.<br /> With InDesign scripting, which BTW I&#8217;m totally unexperienced of, a scripter can manipulate documents and all kind of objects (like frames) within the documents. And, as far as I know, any text that belongs to those frames. So, being InDesign (roughly speaking) a software about text management, you have a direct access to the smallest building block &#8211; i.e. characters.
  </p>

  <p>
    Translating to Photoshop, it&#8217;s like if we could access the image&#8217;s smallest building block, i.e. pixels.<br /> Which, sadly, we can&#8217;t.<br /> We&#8217;re allowed to duplicate documents, layers, apply PS filters to selections and channels &#8211; it&#8217;s fine &#8211; but the active bitmap layer in PS can&#8217;t (as far as I know) be directly grabbed and used by ActionScript as a BitmapImage. The only workaround, to the best of my knowledge is:
  </p>

  <ol>
    <li>
      <p>
        save a temporary copy on the disk as an image;
      </p>
    </li>

    <li>
      <p>
        make the panel load and elaborate it;
      </p>
    </li>

    <li>
      <p>
        save a copy back on the disk;
      </p>
    </li>

    <li>
      <p>
        tell PS to load and paste it in the current document.
      </p>
    </li>
  </ol>

  <p>
    A pain. I knew that a direct sharing between Photoshop and an &#8220;extension&#8221; (back and forth, as a JPG flattened image) is allowed &#8211; within the Photoshop Touch SDK, <em>suddenly disappeared from the Adobe Devnet?!</em> That is: mobile applications, those Touch apps Adobe is proud of. I also knew that <a title="Daniel Koestler Adobe blog" href="http://blogs.adobe.com/koestler/2011/05/using-the-photoshop-touch-sdk-creating-a-project.html" target="_blank">Daniel Koestler</a> and <a title="Renaun Erickson blog" href="http://renaun.com/blog/2011/04/photoshop-touch-sdk-contains-an-actionscript-3-library-too/" target="_blank">Renaun Erickson</a> wrote the Actionscript API (which were in the Touch SDK that I downloaded in May 2011) &#8211; I&#8217;m just wondering and desperately hoping if we&#8217;ll eventually see this little extra step in CS SDK as well.
  </p>

  <blockquote>
    <p>
      To let an extension have <em>direct access to a Photoshop layer as a BitmapImage</em> would be terrific: it would mean the possibility to write actual PS plugins with Flash UI without the headache of dealing with C code (and memory management). It would imply the use of (binary, that is: protected) Pixel Bender kernels, plus a whole bunch of ready made ActionScript shaders. It would possibly result in lower performances compared to C++ plugins but frankly&#8230; who cares.
    </p>
  </blockquote>

  <p>
    I thought to be the only one requesting this feature around, but a couple of emails from other developers pushed me to write this plea: Adobe, please, pretty please&#8230; 🙂
  </p>

  <h2>
    The future of CS Extensibility
  </h2>

  <p>
    While I was in the middle of this post&#8217;s writing, I mailed to Gabriel Tavridis some doubts that came to my mind &#8211; expecially after reading about the <a title="Adobe Flex blog" href="http://blogs.adobe.com/flex/2011/11/your-questions-about-flex.html" target="_blank">Flash/Flex affaire</a>. It&#8217;s clear that, even if Flex cannot be replaced by HTML5 in the short run, Adobe&#8217;s commitment to this newer technology is quite strongly advertised. So what&#8217;s the company&#8217;s roadmap for CS extensions, I was asking. Gabriel kindly wrote back that he was going to post detailed answers to this (and similar) questions in the CS SDK official blog &#8211; which is <a title="CS SDK future" href="http://blogs.adobe.com/cssdk/2011/12/cs_extensibility_and_flex_next.html" target="_blank">now online</a> and&#8230; I suggest you to read it!
  </p>

  <p>
    As a conclusion, no matter what technology we&#8217;ll be using in the years to come, it&#8217;s important that Adobe embraces a transparent attitude toward its users &#8211; as a developer, I&#8217;m happy to notice that the CS SDK group in this respect seems to be <em>avant-garde</em>.
  </p>
</div>
