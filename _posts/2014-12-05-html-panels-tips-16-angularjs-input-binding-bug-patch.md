---
id: 2792
title: 'HTML Panels Tips #16 AngularJS Binding bug patch'
date: 2014-12-05T16:13:36+01:00
author: Davide Barranca
excerpt: "CEP 5.2 has a bug (affecting Macs with non US keyboards) that prevents AngularJS to realize binding in HTML Panels when input fields are involved. New Zealand developer Kris Coppieters has been able to find a successful way to patch Angular and make it work - with his kind permission I'm going to share the instruction."
layout: post
guid: http://www.davidebarranca.com/?p=2792
permalink: /2014/12/html-panels-tips-16-angularjs-input-binding-bug-patch/
seo_post_level_layout:
  - ""
link_url_field:
  - ""
seo_post_meta_description:
  - "CEP5.2 has a bug affecting MacOS and non-US keyboards that may prevent AngularJS binding to work. Here's how to apply a patch to angular.js"
image: /wp-content/uploads/2014/12/AngularJS.png
categories:
  - Coding
  - HTML Panels
tags:
  - AngularJS
  - binding
  - CEP 5.2
  - html panels
  - tip
---
<div class="pf-content">
  <p>
    CEP 5.2 has a bug (affecting Macs with non US keyboards) that prevents AngularJS to realize binding in HTML Panels when input fields are involved. New Zealand developer <a title="Rorohiko Workflow Resources" href="https://www.rorohiko.com" target="_blank">Kris Coppieters</a> has been able to find a successful way to patch Angular and make it work &#8211; with his kind permission I&#8217;m going to share the instruction.
  </p>
  
  <h2>
    The input bug
  </h2>
  
  <p>
    First, what am I talking about? See it yourself with this code:
  </p>
  
  <!-- iframe plugin v.4.4 wordpress.org/plugins/iframe/ -->
  
  <p>
    In the browser, the text input and the h2 are bound to the same ng-model, so what you type in the input area is immediately reflected in the h2, as you&#8217;d expect. First thing that you learn in Angular tutorials: binding.
  </p>
  
  <p>
    Now, it happens that in <strong>Mac OSX with non-US keyboards</strong> (you can change that in the Preferences and see it yourself) <strong>the keypress / input event is fired only when a backspace is typed</strong>, so that:
  </p>
  
  <p>
    1. You type &#8220;Ciao&#8221; here and nothing happens there.<br /> 2. You hit Backspace, so: &#8220;Cia&#8221; &#8211; this triggers the binding and the text appears in the h2.
  </p>
  
  <p>
    The above is utterly annoying imho, and verified in several Adobe application (PS included).
  </p>
  
  <p>
    Kris Coppieters has been able to patch the angular.js file so that binding works again &#8211; please visit his <a title="Kris Coppieters website" href="https://www.rorohiko.com" target="_blank">Rorohiko Workflow Resources</a> website as a way to say thanks (he&#8217;s an InDesign automation developer).
  </p>
  
  <blockquote>
    <p>
      I&#8217;ll be quoting Kris extensively [my comments are inline in square brackets].
    </p>
  </blockquote>
  
  <h2>
    Angular Patch
  </h2>
  
  <p>
    [Basically it&#8217;s a matter of modifying two files in the AngularJS source, then compile the patched build]
  </p>
  
  <p>
    Check out the desired Angular source code. It can be found at: <a title="angular.js on GitHub" href="https://github.com/angular/angular.js" target="_blank">https://github.com/angular/angular.js</a>
  </p>
  
  <p>
    Use the repository URL: <a title="AngularJS repository on GitHub" href="https://github.com/angular/angular.js.git" target="_blank">https://github.com/angular/angular.js.git</a>
  </p>
  
  <p>
    In SourceTree or whatever your favorite git client is, clone the repo to your local hard disk.
  </p>
  
  <p>
    Find the desired version &#8211; e.g. for version 1.2.15, the commit is <em>a9b5a10</em>, dated Mar 22, 2014. I use the &#8216;Jump to&#8217; functionality [top right corned in the interface] in SourceTree to jump to the 1.2.15 label.
  </p>
  
  <p>
    Create a local branch from that label and name your local branch to something descriptive.
  </p>
  
  <p>
    The patched version of Angular will detect:
  </p>
  
  <ul>
    <li>
      That it is running on CEP5 in a Creative Cloud product.
    </li>
    <li>
      What platform (Mac vs. non-Mac) it is running on.
    </li>
  </ul>
  
  <p>
    If it is running on CEP5, and it is on a Mac, then the <code>$SnifferProvider</code> in the Angular source code is set to avoid using the &#8216;input&#8217; event. The input event is currently (December, 2014) broken, and only fires when the backspace key is hit; it does not fire for other events.
  </p>
  
  <p>
    Oddly enough, this seems like a reversal of the broken behavior of IE9 (which does not emit the &#8216;input&#8217; event when backspace is used, whereas CEP seems to suppress the &#8216;input&#8217; event for everything except backspace).
  </p>
  
  <p>
    The standard $<code>SnifferProvider</code> avoids using &#8216;input&#8217; when running on IE9. The code I&#8217;ve added also avoids using &#8216;input&#8217; when running on Macintosh CEP5/CC.
  </p>
  
  <p>
    This cures the &#8216;keyboard language dependency&#8217; issue with panels ignoring the field contents on Macs with non-US keyboards.
  </p>
  
  <p>
    I changed two files:
  </p>
  
  <ol>
    <li>
      <code>src/Angular.js</code>
    </li>
    <li>
      <code>src/ng/sniffer.js</code>
    </li>
  </ol>
  
  <p>
    The patch consists of:
  </p>
  
  <h3>
    Step 1
  </h3>
  
  <p>
    Near the very top of the <code>src/Angular.js</code> file, find the jsLint info for <code>msie</code>. Add two new variables to the list: <code>isCreativeCloudCEP</code> and <code>isMacintosh</code>. Mimic the syntax that is used for <code>msie</code> for these two variables: this is where version Angular 1.2.x and 1.3.x look somewhat different.
  </p>
  
  <p>
    Angular <strong>1.2.x</strong>
  </p>
  
  <pre class="nums:true lang:js decode:true" title="Angular 1.2.x">'use strict';

/* We need to tell jshint what variables are being exported */
/* global
    -angular,
    -msie,
    -isCreativeCloudCEP,
    -isMacintosh,
    -jqLite,
...
*/</pre>
  
  <p>
    Angular <strong>1.3.x</strong>
  </p>
  
  <pre class="nums:true lang:js decode:true" title="Angular 1.3.x">'use strict';

/* We need to tell jshint what variables are being exported */
/* global angular: true,
  msie: true,
  isCreativeCloudCEP: true,
  isMacintosh: true,
  jqLite: true,
...
*/</pre>
  
  <h3>
    Step 2
  </h3>
  
  <p>
    2) Find <code>msie</code> in the list of variables being defined, and again add similar declarations for variables <code>isCreativeCloudCEP</code> and <code>isMacintosh</code> right behind it.
  </p>
  
  <p>
    For Angular <strong>1.2.x</strong> (around line 136):
  </p>
  
  <pre class="nums:true start-line:136 lang:js decode:true">var /** holds major version number for IE or NaN for real browsers */
    msie,
    isCreativeCloudCEP,
    isMacintosh,
    jqLite,           // delay binding since jQuery could be loaded after us.</pre>
  
  <p>
    For Angular <strong>1.3.x</strong> (around line 167):
  </p>
  
  <pre class="nums:true start-line:167 lang:js decode:true">var
    msie,             // holds major version number for IE, or NaN if UA is not IE.
    isCreativeCloudCEP,
    isMacintosh,
    jqLite,           // delay binding since jQuery could be loaded after us.</pre>
  
  <h3>
    Step 3
  </h3>
  
  <p>
    A little bit further find the lines where <code>msie</code> is assigned and add the following statements for <code>isCreativeCloudCEP</code> and <code>isMacintosh</code>.
  </p>
  
  <p>
    For Angular <strong>1.2.x</strong> (around line 155):
  </p>
  
  <pre class="nums:true start-line:155 lang:js decode:true">/**
 * IE 11 changed the format of the UserAgent string.
 * See http://msdn.microsoft.com/en-us/library/ms537503.aspx
 */
msie = int((/msie (\d+)/.exec(lowercase(navigator.userAgent)) || [])[1]);
if (isNaN(msie)) {
  msie = int((/trident\/.*; rv:(\d+)/.exec(lowercase(navigator.userAgent)) || [])[1]);
}

/* CEP Patch */
isCreativeCloudCEP = "object" == typeof window.__adobe_cep__;
isMacintosh = navigator.userAgent.indexOf("Macintosh") &gt;= 0;</pre>
  
  <p>
    For Angular <strong>1.3.x</strong> (around line 184):
  </p>
  
  <pre class="nums:true start-line:184 lang:js decode:true">/**
 * documentMode is an IE-only property
 * http://msdn.microsoft.com/en-us/library/ie/cc196988(v=vs.85).aspx
 */
msie = document.documentMode;

/* CEP Patch */
isCreativeCloudCEP = "object" == typeof window.__adobe_cep__;
isMacintosh = navigator.userAgent.indexOf("Macintosh") &gt;= 0;</pre>
  
  <h3>
    Step 4
  </h3>
  
  <p>
    In the file <code>src/ng/sniffer.js</code> find these lines and add right behind it a conditional.
  </p>
  
  <p>
    For Angular <strong>1.2.x</strong> (around line 71):
  </p>
  
  <pre class="nums:true start-line:71 lang:js decode:true">// IE9 implements 'input' event it's so fubared that we rather pretend that it doesn't have
// it. In particular the event is not fired when backspace or delete key are pressed or
// when cut operation is performed.
if (event == 'input' && msie == 9) return false;

// CEP Patch
if (event == 'input' && isMacintosh && isCreativeCloudCEP) return false;</pre>
  
  <p>
    For Angular <strong>1.3.x</strong> (around line 67):
  </p>
  
  <pre class="nums:true start-line:67 lang:js decode:true">// IE9 implements 'input' event it's so fubared that we rather pretend that it doesn't have
// it. In particular the event is not fired when backspace or delete key are pressed or
// when cut operation is performed.
if (event == 'input' && msie == 9) return false;

// CEP Patch
if (event == 'input' && isMacintosh && isCreativeCloudCEP) return false;</pre>
  
  <h3>
    Step 5
  </h3>
  
  <p>
    A little bit further, find the last returned objects and add <code>isMacintosh</code> and <code>isCreativeCloudCEP</code>.
  </p>
  
  <p>
    For Angular <strong>1.2.x</strong> (around line 89):
  </p>
  
  <pre class="nums:true start-line:89 lang:js decode:true">msie : msie,

// CEP Patch
isMacintosh: isMacintosh,
isCreativeCloudCEP: isCreativeCloudCEP</pre>
  
  <p>
    For Angular <strong>1.3.x</strong> (around line 82):
  </p>
  
  <pre class="nums:true start-line:82 lang:js decode:true">csp: csp(),
vendorPrefix: vendorPrefix,
transitions: transitions,
animations: animations,
android: android,

// CEP Patch
isMacintosh: isMacintosh,
isCreativeCloudCEP: isCreativeCloudCEP</pre>
  
  <h3>
    Step 6
  </h3>
  
  <p>
    Compile a new version of Angular: you will need to install node.js, grunt, bower&#8230; I won&#8217;t explain that here &#8211; it&#8217;s a matter of visiting <a title="AngularJS contribute page" href="https://docs.angularjs.org/misc/contribute" target="_blank">Angular Contribute page</a> and the node.js web site and following instructions.
  </p>
  
  <p>
    Build angular as documented on the Angular &#8216;contribute&#8217; page, then go into the build folder, and extract <code>angular.min.js</code> from the final .zip file. This is your patched version &#8211; it&#8217;s a drop-in replacement for the standard <code>angular.min.js</code> version that you branched off from.
  </p>
  
  <h2>
    Follow-up &#8211; building Angular
  </h2>
  
  <p>
    Let&#8217;s thank again Kris Coppieters for the instruction so far &#8211; I will add a brief section about building Angular, which hasn&#8217;t been as straightforward as I assumed first.
  </p>
  
  <h3>
    Installing Dependencies
  </h3>
  
  <p>
    Quoting from Angular website:
  </p>
  
  <blockquote style="font-size: 100%;">
    <p>
      Before you can build AngularJS, you must install and configure the following dependencies on your machine:
    </p>
    
    <p>
      <a href="http://git-scm.com/">Git</a>: The <a href="https://help.github.com/articles/set-up-git">Github Guide to Installing Git</a> is a good source of information.
    </p>
    
    <p>
      <a href="http://nodejs.org/">Node.js</a>: We use Node to generate the documentation, run a development web server, run tests, and generate distributable files. Depending on your system, you can install Node either from source or as a pre-packaged bundle.
    </p>
    
    <p>
      <a href="http://www.java.com/">Java</a>: We minify JavaScript using our <a href="https://developers.google.com/closure/">Closure Tools</a> jar. Make sure you have Java (version 7 or higher) installed and included in your <a href="http://docs.oracle.com/javase/tutorial/essential/environment/paths.html">PATH</a> variable.
    </p>
    
    <p>
      <a href="http://gruntjs.com/">Grunt</a>: We use Grunt as our build system. Install the grunt command-line tool globally with:
    </p>
    
    <p>
      <code>npm install -g grunt-cli</code>
    </p>
    
    <p>
      <a href="http://bower.io/">Bower</a>: We use Bower to manage client-side packages for the docs. Install the bower command-line tool globally with:
    </p>
    
    <p>
      <code>npm install -g bower</code>
    </p>
    
    <p>
      <strong>Note</strong>: You may need to use sudo (for OSX, *nix, BSD etc) or run your command shell as Administrator (for Windows) to install Grunt & Bower globally.
    </p>
  </blockquote>
  
  <p>
    This has been easy, no problem to me &#8211; by the way I already had everything needed, chances are you too already rely on those tools.
  </p>
  
  <p>
    Things get slightly worse when:
  </p>
  
  <pre class="lang:js highlight:0 decode:true"># Install node.js dependencies:
npm install

# Install bower components:
bower install

# Build AngularJS:
grunt package</pre>
  
  <p>
    First <strong>npm</strong>: I&#8217;ve had lots of unmet dependencies with npm (Mac users should <code>&lt;strong>sudo&lt;/strong> npm install</code>):
  </p>
  
  <p>
    <img class="aligncenter size-full wp-image-2812" src="http://localhost:8888/wp-content/uploads/2014/12/unmet-dependencies.png" alt="unmet-dependencies" width="570" height="344" srcset="http://localhost:8888/wp-content/uploads/2014/12/unmet-dependencies.png 570w, http://localhost:8888/wp-content/uploads/2014/12/unmet-dependencies-150x90.png 150w, http://localhost:8888/wp-content/uploads/2014/12/unmet-dependencies-300x181.png 300w" sizes="(max-width: 570px) 100vw, 570px" />
  </p>
  
  <p>
    Which I ignored. No apparent problem with <strong>bower</strong>, but when it came to <code>grunt package</code> the command stopped because of some missing module, like:
  </p>
  
  <p>
    <img class="aligncenter size-full wp-image-2813" src="http://localhost:8888/wp-content/uploads/2014/12/missing-packages.png" alt="missing-packages" width="570" height="344" srcset="http://localhost:8888/wp-content/uploads/2014/12/missing-packages.png 570w, http://localhost:8888/wp-content/uploads/2014/12/missing-packages-150x90.png 150w, http://localhost:8888/wp-content/uploads/2014/12/missing-packages-300x181.png 300w" sizes="(max-width: 570px) 100vw, 570px" />
  </p>
  
  <p>
    In this case as you can see in the first Error line, the &#8216;clone-stats&#8217; package couldn&#8217;t be found, so went on with <code>npm install clone-stats</code>, then <code>grunt package</code> again. I run into several missing modules, so I kept  <code>npm install this-and-that</code> and then <code>grunt package</code> on and on. Until the missing package appeared to be a local module such as:
  </p>
  
  <p>
    <img class="aligncenter size-full wp-image-2814" src="http://localhost:8888/wp-content/uploads/2014/12/local-lib.png" alt="local-lib" width="570" height="344" srcset="http://localhost:8888/wp-content/uploads/2014/12/local-lib.png 570w, http://localhost:8888/wp-content/uploads/2014/12/local-lib-150x90.png 150w, http://localhost:8888/wp-content/uploads/2014/12/local-lib-300x181.png 300w" sizes="(max-width: 570px) 100vw, 570px" />
  </p>
  
  <p>
    Then I gave up &#8211; by the way, this happened while running the &#8220;docs&#8221; task (which I&#8217;m not interested into), the compilation of both regular and minified version of <code>angular.js</code> was already done so I could grab the file and use it.
  </p>
  
  <p>
    And, good news, it works 🙂
  </p>
  
  <h2>
    TL;DR
  </h2>
  
  <p>
    Are you having problems with AngularJS binding the model to an input field? Chances are you&#8217;re on a Mac with non-US keyboard (and if you&#8217;re not, your users can be). This is a known issue of CEP5.2. <a title="Rorohiko Workflow Resources" href="https://www.rorohiko.com" target="_blank">Kris Coppieters</a> came up with an Angular patch that you can apply &#8211; if you&#8217;re lazy, just grab the patched version of <a href="http://localhost:8888/wp-content/uploads/2014/12/angular-1.2.9.zip">Angular-1.2.9</a> and <a href="http://localhost:8888/wp-content/uploads/2014/12/angular-1.3.5.zip">Angular-1.3.5</a>.
  </p>
</div>

<!-- Share-Widget Button BEGIN --><a href="javascript:void(0);" myshare\_id="mys\_shareit" myshare\_url="http://localhost:8888/2014/12/html-panels-tips-16-angularjs-input-binding-bug-patch/" myshare\_title="HTML Panels Tips #16 AngularJS Binding bug patch" rel="nofollow" onclick=" return false;" style="text-decoration:none; color:#000000; font-size:11px; line-height:20px;"> 

<img src="http://localhost:8888/wp-content/plugins/share-widget/img/share-button-white-small.png" height="20" alt="Share" style="border:0" /> </a> <!-- Share-Widget Button END -->