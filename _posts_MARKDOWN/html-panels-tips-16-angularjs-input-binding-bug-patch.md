---
title: " HTML Panels Tips #16 AngularJS Binding bug patch\t\t"
tags:
  - AngularJS
  - binding
  - CEP 5.2
  - html panels
  - tip
url: 2792.html
id: 2792
category:
  - Coding
  - HTML Panels
date: 2014-12-05 16:13:36
---

CEP 5.2 has a bug (affecting Macs with non US keyboards) that prevents AngularJS to realize binding in HTML Panels when input fields are involved. New Zealand developer [Kris Coppieters](https://www.rorohiko.com "Rorohiko Workflow Resources") has been able to find a successful way to patch Angular and make it work - with his kind permission I'm going to share the instruction.

The input bug
-------------

First, what am I talking about? See it yourself with this code: \[iframe width="100%" height="160" src="http://jsfiddle.net/01k8u5fw/4/embedded/result,js,html" frameborder="0"\] In the browser, the text input and the h2 are bound to the same ng-model, so what you type in the input area is immediately reflected in the h2, as you'd expect. First thing that you learn in Angular tutorials: binding. Now, it happens that in **Mac OSX with non-US keyboards** (you can change that in the Preferences and see it yourself) **the keypress / input event is fired only when a backspace is typed**, so that: 1. You type "Ciao" here and nothing happens there. 2. You hit Backspace, so: "Cia" - this triggers the binding and the text appears in the h2. The above is utterly annoying imho, and verified in several Adobe application (PS included). Kris Coppieters has been able to patch the angular.js file so that binding works again - please visit his [Rorohiko Workflow Resources](https://www.rorohiko.com "Kris Coppieters website") website as a way to say thanks (he's an InDesign automation developer).

> I'll be quoting Kris extensively \[my comments are inline in square brackets\].

Angular Patch
-------------

\[Basically it's a matter of modifying two files in the AngularJS source, then compile the patched build\] Check out the desired Angular source code. It can be found at: [https://github.com/angular/angular.js](https://github.com/angular/angular.js "angular.js on GitHub") Use the repository URL: [https://github.com/angular/angular.js.git](https://github.com/angular/angular.js.git "AngularJS repository on GitHub") In SourceTree or whatever your favorite git client is, clone the repo to your local hard disk. Find the desired version - e.g. for version 1.2.15, the commit is _a9b5a10_, dated Mar 22, 2014. I use the 'Jump to' functionality \[top right corned in the interface\] in SourceTree to jump to the 1.2.15 label. Create a local branch from that label and name your local branch to something descriptive. The patched version of Angular will detect:

*   That it is running on CEP5 in a Creative Cloud product.
*   What platform (Mac vs. non-Mac) it is running on.

If it is running on CEP5, and it is on a Mac, then the `$SnifferProvider` in the Angular source code is set to avoid using the 'input' event. The input event is currently (December, 2014) broken, and only fires when the backspace key is hit; it does not fire for other events. Oddly enough, this seems like a reversal of the broken behavior of IE9 (which does not emit the 'input' event when backspace is used, whereas CEP seems to suppress the 'input' event for everything except backspace). The standard $`SnifferProvider` avoids using 'input' when running on IE9. The code I've added also avoids using 'input' when running on Macintosh CEP5/CC. This cures the 'keyboard language dependency' issue with panels ignoring the field contents on Macs with non-US keyboards. I changed two files:

1.  `src/Angular.js`
2.  `src/ng/sniffer.js`

The patch consists of:

### Step 1

Near the very top of the `src/Angular.js` file, find the jsLint info for `msie`. Add two new variables to the list: `isCreativeCloudCEP` and `isMacintosh`. Mimic the syntax that is used for `msie` for these two variables: this is where version Angular 1.2.x and 1.3.x look somewhat different. Angular **1.2.x**

'use strict';

/\* We need to tell jshint what variables are being exported */
/\* global
    -angular,
    -msie,
    -isCreativeCloudCEP,
    -isMacintosh,
    -jqLite,
...
*/

Angular **1.3.x**

'use strict';

/\* We need to tell jshint what variables are being exported */
/\* global angular: true,
  msie: true,
  isCreativeCloudCEP: true,
  isMacintosh: true,
  jqLite: true,
...
*/

### Step 2

2) Find `msie` in the list of variables being defined, and again add similar declarations for variables `isCreativeCloudCEP` and `isMacintosh` right behind it. For Angular **1.2.x** (around line 136):

var /** holds major version number for IE or NaN for real browsers */
    msie,
    isCreativeCloudCEP,
    isMacintosh,
    jqLite,           // delay binding since jQuery could be loaded after us.

For Angular **1.3.x** (around line 167):

var
    msie,             // holds major version number for IE, or NaN if UA is not IE.
    isCreativeCloudCEP,
    isMacintosh,
    jqLite,           // delay binding since jQuery could be loaded after us.

### Step 3

A little bit further find the lines where `msie` is assigned and add the following statements for `isCreativeCloudCEP` and `isMacintosh`. For Angular **1.2.x** (around line 155):

/\*\*
 \* IE 11 changed the format of the UserAgent string.
 \* See http://msdn.microsoft.com/en-us/library/ms537503.aspx
 */
msie = int((/msie (\\d+)/.exec(lowercase(navigator.userAgent)) || \[\])\[1\]);
if (isNaN(msie)) {
  msie = int((/trident\\/.*; rv:(\\d+)/.exec(lowercase(navigator.userAgent)) || \[\])\[1\]);
}

/\* CEP Patch */
isCreativeCloudCEP = "object" == typeof window.\_\_adobe\_cep__;
isMacintosh = navigator.userAgent.indexOf("Macintosh") >= 0;

For Angular **1.3.x** (around line 184):

/\*\*
 \* documentMode is an IE-only property
 \* http://msdn.microsoft.com/en-us/library/ie/cc196988(v=vs.85).aspx
 */
msie = document.documentMode;

/\* CEP Patch */
isCreativeCloudCEP = "object" == typeof window.\_\_adobe\_cep__;
isMacintosh = navigator.userAgent.indexOf("Macintosh") >= 0;

### Step 4

In the file `src/ng/sniffer.js` find these lines and add right behind it a conditional. For Angular **1.2.x** (around line 71):

// IE9 implements 'input' event it's so fubared that we rather pretend that it doesn't have
// it. In particular the event is not fired when backspace or delete key are pressed or
// when cut operation is performed.
if (event == 'input' && msie == 9) return false;

// CEP Patch
if (event == 'input' && isMacintosh && isCreativeCloudCEP) return false;

For Angular **1.3.x** (around line 67):

// IE9 implements 'input' event it's so fubared that we rather pretend that it doesn't have
// it. In particular the event is not fired when backspace or delete key are pressed or
// when cut operation is performed.
if (event == 'input' && msie == 9) return false;

// CEP Patch
if (event == 'input' && isMacintosh && isCreativeCloudCEP) return false;

### Step 5

A little bit further, find the last returned objects and add `isMacintosh` and `isCreativeCloudCEP`. For Angular **1.2.x** (around line 89):

msie : msie,

// CEP Patch
isMacintosh: isMacintosh,
isCreativeCloudCEP: isCreativeCloudCEP

For Angular **1.3.x** (around line 82):

csp: csp(),
vendorPrefix: vendorPrefix,
transitions: transitions,
animations: animations,
android: android,

// CEP Patch
isMacintosh: isMacintosh,
isCreativeCloudCEP: isCreativeCloudCEP

### Step 6

Compile a new version of Angular: you will need to install node.js, grunt, bower... I won't explain that here - it's a matter of visiting [Angular Contribute page](https://docs.angularjs.org/misc/contribute "AngularJS contribute page") and the node.js web site and following instructions. Build angular as documented on the Angular 'contribute' page, then go into the build folder, and extract `angular.min.js` from the final .zip file. This is your patched version - it's a drop-in replacement for the standard `angular.min.js` version that you branched off from.

Follow-up - building Angular
----------------------------

Let's thank again Kris Coppieters for the instruction so far - I will add a brief section about building Angular, which hasn't been as straightforward as I assumed first.

### Installing Dependencies

Quoting from Angular website:

> Before you can build AngularJS, you must install and configure the following dependencies on your machine: [Git](http://git-scm.com/): The [Github Guide to Installing Git](https://help.github.com/articles/set-up-git) is a good source of information. [Node.js](http://nodejs.org/): We use Node to generate the documentation, run a development web server, run tests, and generate distributable files. Depending on your system, you can install Node either from source or as a pre-packaged bundle. [Java](http://www.java.com/): We minify JavaScript using our [Closure Tools](https://developers.google.com/closure/) jar. Make sure you have Java (version 7 or higher) installed and included in your [PATH](http://docs.oracle.com/javase/tutorial/essential/environment/paths.html) variable. [Grunt](http://gruntjs.com/): We use Grunt as our build system. Install the grunt command-line tool globally with: `npm install -g grunt-cli` [Bower](http://bower.io/): We use Bower to manage client-side packages for the docs. Install the bower command-line tool globally with: `npm install -g bower` **Note**: You may need to use sudo (for OSX, *nix, BSD etc) or run your command shell as Administrator (for Windows) to install Grunt & Bower globally.

This has been easy, no problem to me - by the way I already had everything needed, chances are you too already rely on those tools. Things get slightly worse when:

\# Install node.js dependencies:
npm install

\# Install bower components:
bower install

\# Build AngularJS:
grunt package

First **npm**: I've had lots of unmet dependencies with npm (Mac users should `**sudo** npm install`): ![unmet-dependencies](http://localhost:8888/wp-content/uploads/2014/12/unmet-dependencies.png) Which I ignored. No apparent problem with **bower**, but when it came to `grunt package` the command stopped because of some missing module, like: ![missing-packages](http://localhost:8888/wp-content/uploads/2014/12/missing-packages.png) In this case as you can see in the first Error line, the 'clone-stats' package couldn't be found, so went on with `npm install clone-stats`, then `grunt package` again. I run into several missing modules, so I kept  `npm install this-and-that` and then `grunt package` on and on. Until the missing package appeared to be a local module such as: ![local-lib](http://localhost:8888/wp-content/uploads/2014/12/local-lib.png) Then I gave up - by the way, this happened while running the "docs" task (which I'm not interested into), the compilation of both regular and minified version of `angular.js` was already done so I could grab the file and use it. And, good news, it works :-)

TL;DR
-----

Are you having problems with AngularJS binding the model to an input field? Chances are you're on a Mac with non-US keyboard (and if you're not, your users can be). This is a known issue of CEP5.2. [Kris Coppieters](https://www.rorohiko.com "Rorohiko Workflow Resources") came up with an Angular patch that you can apply - if you're lazy, just grab the patched version of [Angular-1.2.9](http://localhost:8888/wp-content/uploads/2014/12/angular-1.2.9.zip) and [Angular-1.3.5](http://localhost:8888/wp-content/uploads/2014/12/angular-1.3.5.zip).