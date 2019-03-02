---
id: 2155
title: Testing minified JS Libraries in ExtendScript
date: 2013-08-29T15:01:46+01:00
author: Davide Barranca
excerpt: "It's not uncommon, when scripting for Adobe applications, to borrow JS libraries that have been originally written for web development. While the new generation of Adobe HTML Extensions will run on the Chromium Embedded Framework, traditional ExtendScript code is based upon a different, older engine. Besides ECMAScript unsupported features (i.e. ES 5) I've noticed that using minified JS code is a risky business - scripts can break or fail silently. I've set up a proper testing environment to inspect them."
layout: post
guid: http://www.davidebarranca.com/?p=2155
permalink: /2013/08/testing-minified-js-libraries-in-extendscript/
standard_seo_post_level_layout:
  - ""
standard_link_url_field:
  - ""
standard_seo_post_meta_description:
  - 'Minified JS libraries used in Web development can fail in the ExtendScript engine: proper testing shows the risks, and suggests right tools.'
image: /wp-content/uploads/2013/08/TestingJSMinify.png
categories:
  - Coding
  - ExtendScript / Javascript
tags:
  - Extendscript
  - Javascript
  - libraries
  - minify
  - testing
---
<div class="pf-content">
  <p>
    <span itemprop="headline">It&#8217;s not uncommon, when scripting for Adobe applications, to borrow JS libraries that have been originally written for web development. While the new generation of HTML Extensions will run on the <a title="Google Chromium Embedded Framework" href="http://en.wikipedia.org/wiki/Chromium_Embedded_Framework" target="_blank">Chromium Embedded Framework</a>, traditional <span itemprop= "about" itemscope itemtype="http://schema.org/SoftwareApplication">Adobe ExtendScript</span> code is based upon the implementation of a different, older Javascript engine. Besides ECMAScript unsupported features (i.e. ES 5) I&#8217;ve noticed that using <span itemprop="about">minified JS libraries</span> is a risky business &#8211; scripts can break or fail silently. I&#8217;ve set up a proper <span itemprop="about">testing</span> environment to inspect them.</span><!--more-->
  </p>

  <h2>
    Minified libraries
  </h2>

  <p>
    Web developers need to keep their data transfer footprints as light as possible so they minify their code &#8211; i.e. use engines such as <span itemprop="mentions" itemscope itemtype="http://schema.org/SoftwareApplication"><a title="Google Closure Compiler" href="http://closure-compiler.appspot.com/home" target="_blank" itemprop="url">Google Closure Compiler</a></span> or <span itemprop="mentions" itemscope itemtype="http://schema.org/SoftwareApplication"><a title="UglifyJS2" href="https://github.com/mishoo/UglifyJS2" target="_blank" itemprop="url">UglifyJS2</a></span> to transform eloquent, commented and nicely indented code into an unreadable blob of characters &#8211; for instance as follow is a minified version of a CryptoJS library for Base64 encoding:
  </p>

  <pre class="theme:twilight font-size:14 line-height:18 toolbar:2 striped:false nums:false nums-toggle:false scroll:false whitespace-before:1 whitespace-after:1 lang:js decode:true" title="Minified code">(function(){var h=CryptoJS,k=h.f.c;h.e.b={stringify:function(b){var e=b.h,f=b.g,c=this.a;b.d();b=[];for(var a=0;a&lt;f;a+=3)for(var d=(e[a&gt;&gt;&gt;2]&gt;&gt;&gt;24-8*(a%4)&255)&lt;&lt;16|(e[a+1&gt;&gt;&gt;2]&gt;&gt;&gt;24-8*((a+1)%4)&255)&lt;&lt;8|e[a+2&gt;&gt;&gt;2]&gt;&gt;&gt;24-8*((a+2)%4)&255,g=0;4&gt;g&&a+0.75*g&lt;f;g++)b.push(c.charAt(d&gt;&gt;&gt;6*(3-g)&63));if(e=c.charAt(64))for(;b.length%4;)b.push(e);return b.join("")},parse:function(b){var e=b.length,f=this.a,c=f.charAt(64);c&&(c=b.indexOf(c),-1!=c&&(e=c));for(var c=[],a=0,d=0;d&lt;e;d++)d%4&&(c[a&gt;&gt;&gt;2]|=(f.indexOf(b.charAt(d-
1))&lt;&lt;2*(d%4)|f.indexOf(b.charAt(d))&gt;&gt;&gt;6-2*(d%4))&lt;&lt;24-8*(a%4),a++);return k.create(c,a)},a:"ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/="}})();</pre>

  <p>
    Being generally installed on the machine the users work on, Adobe scripts don&#8217;t benefit from a reduction in filesize (which is pretty small per se) so developers don&#8217;t bother with minifiers; and ESTK can deploy a binary version, so code readability is not an issue.
  </p>

  <p>
    A couple of pretty neat libraries that I use &#8211; and will test in this post &#8211; are:
  </p>

  <ol>
    <li>
      <span itemprop="mentions" itemscope itemtype="http://schema.org/SoftwareApplication"><a title="ES5 Shim" href="https://github.com/kriskowal/es5-shim" target="_blank" itemprop="url">ES5-Shim</a></span> &#8211; monkey-patch a JavaScript context to contain all EcmaScript 5 methods that can be faithfully emulated with a legacy JavaScript engine.
    </li>
    <li>
      <span itemprop="mentions" itemscope itemtype="http://schema.org/SoftwareApplication"><a title="CryptoJS" href="http://code.google.com/p/crypto-js/" target="_blank" itemprop="url">CryptoJS</a></span> &#8211; a growing collection of standard and secure cryptographic algorithms implemented in JavaScript. I wrote <span itemprop="citation" itemscope itemtype="http://schema.org/BlogPosting"><a title="CryptoJS Tutorial For Dummies" href="/2012/10/crypto-js-tutorial-cryptography-for-dummies/" target="_blank" itemprop="url">a &#8220;CryptoJS For Dummies&#8221; tutorial</a></span> some time ago that appears to be quite popular.
    </li>
  </ol>

  <h2>
    Testing
  </h2>

  <p>
    Libraries that I will test are the actual <code>ES5-Shim.js</code> and the <code>core.js</code> + <code>enc-Base64.js</code> (routine for Base64 encoding, as you may guess) from CryptoJS. Below you can find a comparative view over the original files, the minified version provided by the libs authors and minified versions that I&#8217;ve compiled using UglifyJS (default params) and Closure Compiler (three versions: whitespaces only, simple and advanced &#8211; detailed info about their differences <a title="Closure Compiler minify options" href="https://developers.google.com/closure/compiler/docs/compilation_levels?hl=it&csw=1" target="_blank">here</a>).
  </p>

  <pre class="whitespace-before:0 whitespace-after:0 lang:default highlight:0 decode:true">============================================
ES5-Shim
bytes - Library
--------------------------------------------
45658 - es5-shim.js
16324 - es5-shim.min.js
16308 - es5-shim-min-uglify.js
16288 - es5-shim-min-closure-whitespace.js
11400 - es5-shim-min-closure-simple.js
11367 - es5-shim-min-closure-advanced.js

============================================
CryptoJS
bytes - Library
--------------------------------------------
21468 - core.js
 3298 - core-min.js
 4973 - core-min-uglify.js
 4964 - core-min-closure-whitespace.js
 3176 - core-min-closure-simple.js
 2682 - core-min-closure-advanced.js
--------------------------------------------
 3338 - enc-base64.js
  869 - enc-base64-min.js
 1251 - enc-base64-min-uglify.js
 1245 - enc-base64-min-closure-whitespace.js
  728 - enc-base64-min-closure-simple.js
  674 - enc-base64-min-closure-advanced.js</pre>

  <p>
    As you see the compression can be remarkable! (~21.000 -> ~3.000 bytes). Let&#8217;s set up a test environment: I&#8217;ve created a Lib2Test folder in Photoshop CC/Presets/Scripts where I&#8217;ve put all the above listed files. In order to include a library in a jsx add the line:
  </p>

  <pre class="lang:default decode:true">$.evalFile("" + app.path + "/" + (localize("$$$/ScriptingSupport/InstalledScripts=Presets/Scripts")) + "/Libs2Test/&lt;yourLibName&gt;.js");</pre>

  <p>
    <code>$.evalFile</code> is more flexible than <code>#include</code> in my opinion, since you can load external code when you need it (i.e. testing a condition &#8211; if this happens, then I need this lib, else I don&#8217;t or I need some other lib).
  </p>

  <h3>
    ES5-Shim
  </h3>

  <p>
    I&#8217;ve written a simple evaluation test that cycle through all the original, provided minified version and custom made compression:
  </p>

  <pre class="lang:default decode:true">var scriptPath = "" + app.path + "/" + (localize("$$$/ScriptingSupport/InstalledScripts=Presets/Scripts")) + "/Libs2Test/";
var postfix = ["", "-min", "-min-uglify", "-min-closure-whitespace", "-min-closure-simple", "-min-closure-advanced"];
var libKind = "es5-shim";
var libName = undefined;
for (var i = 0; i &lt; postfix.length; i++) {
	libName = "" + libKind + postfix[i] + ".js";
	try {
		$.evalFile(scriptPath + libName);
		$.writeln("Testing " + libName + ": OK.");
	} catch (e) {
		$.writeln("Testing " + libName + ": ERROR " + e.number + "; " + e.message);
	}
}</pre>

  <p>
    As a result, I&#8217;ve found that:
  </p>

  <pre class="lang:default highlight:0 decode:true">Testing es5-shim.js: OK.
Testing es5-shim-min.js: ERROR 25; Expected: :
Testing es5-shim-min-uglify.js: ERROR 25; Expected: :
Testing es5-shim-min-closure-whitespace.js: ERROR 25; Expected: :
Testing es5-shim-min-closure-simple.js: ERROR 25; Expected: :
Testing es5-shim-min-closure-advanced.js: ERROR 25; Expected: :</pre>

  <p>
    <strong>Problem</strong>: for some reason &#8211; that I&#8217;m not willing to investigate &#8211; each and every compressed versions for Photoshop is indigestible. Conclusion: keep the original version!
  </p>

  <h3>
    CryptoJS
  </h3>

  <p>
    The test for the CryptoJS libraries is slightly more complex. First let&#8217;s just evaluate the <code>core.js</code>
  </p>

  <pre class="lang:default highlight:0 decode:true">Testing core.js: OK.
Testing core-min.js: OK.
Testing core-min-uglify.js: OK.
Testing core-min-closure-whitespace.js: OK.
Testing core-min-closure-simple.js: OK.
Testing core-min-closure-advanced.js: OK.</pre>

  <p>
    Then (including <code>core.js</code> because it&#8217;s a dependency) the <code>enc-Base64.js</code> file:
  </p>

  <pre class="lang:default highlight:0 decode:true">Testing enc-Base64.js: OK.
Testing enc-Base64-min.js: OK.
Testing enc-Base64-min-uglify.js: OK.
Testing enc-Base64-min-closure-whitespace.js: OK.
Testing enc-Base64-min-closure-simple.js: OK.
Testing enc-Base64-min-closure-advanced.js: ERROR 21; undefined is not an object</pre>

  <p>
    <strong>Problem</strong>: apparently there are no evaluation errors, but for the Base64 module when compressed with Closure (advanced).
  </p>

  <p>
    Let&#8217;s do a functional test on the minified versions of core.js &#8211; true, it&#8217;s evaluated without errors, but this doesn&#8217;t mean it works as expected. The following is not the most bulletproof test in the northern hemisphere, but should work. Basically I&#8217;m converting a Latin1 encoded String to a WordArray, then the reverse (WordArray to Latin1). If the two strings match, the test is passed. (If you have doubts about how CryptoJS works, please have a look at my <a title="CryptoJS Tutorial For Dummies" href="/2012/10/crypto-js-tutorial-cryptography-for-dummies/" target="_blank">my tutorial</a> to refresh your 007 skills).
  </p>

  <pre class="lang:default decode:true">var scriptPath = "" + app.path + "/" + (localize("$$$/ScriptingSupport/InstalledScripts=Presets/Scripts")) + "/Libs2Test/";
var postfix = ["", "-min", "-min-uglify", "-min-closure-whitespace", "-min-closure-simple", "-min-closure-advanced"];
var libKind = "core";
var text = "My name is Bond, James Bond.";
var libName, Latin1ToWA, WAToLatin1, result;

for (var i = 0; i &lt; postfix.length; i++) {
	libName = "" + libKind + postfix[i] + ".js";
	$.evalFile(scriptPath + libName);

	Latin1ToWA = CryptoJS.enc.Latin1.parse(text);		// Latin1 to WordArray
	WAToLatin1 = CryptoJS.enc.Latin1.stringify(Latin1ToWA);	// WordArray to Latin1

	result = text === WAToLatin1 ? "Passed" : "Failed";
	$.writeln("Testing " + libName + ": " + result);
}</pre>

  <p>
    The results are encouraging:
  </p>

  <pre class="lang:default highlight:0 decode:true">Testing core.js: Passed
Testing core-min.js: Passed
Testing core-min-uglify.js: Passed
Testing core-min-closure-whitespace.js: Passed
Testing core-min-closure-simple.js: Passed
Testing core-min-closure-advanced.js: Passed</pre>

  <p>
    Apparently &#8211; as long as this simple test is concerned &#8211; <code>core.js</code> has no functional problems.
  </p>

  <p>
    Let&#8217;s involve the <code>enc-base64.js</code> library: this time the test will be a roundtrip:
  </p>

  <ul>
    <li>
      from Latin1 to Word Array;
    </li>
    <li>
      from WordArray to Base64;
    </li>
    <li>
      from Base64 back to WordArray;
    </li>
    <li>
      from WordArray back to Latin1.
    </li>
  </ul>

  <p>
    If the two Latin1 strings are identical, the test is passed. I&#8217;ve removed the version compressed with Closure Compiler (advanced) because it gives evaluation errors.
  </p>

  <pre class="lang:default decode:true">var scriptPath = "" + app.path + "/" + (localize("$$$/ScriptingSupport/InstalledScripts=Presets/Scripts")) + "/Libs2Test/";
$.evalFile(scriptPath + "core.js"); // needed by the enc-Base64 library
var postfix = ["", "-min", "-min-uglify", "-min-closure-whitespace", "-min-closure-simple"];
var libKind = "enc-Base64";
var text = "My name is Bond, James Bond.";
var libName, Latin1ToWA, WAToBase64, Base64ToWA, WAToLatin1, result;

for (var i = 0; i &lt; postfix.length; i++) {
	libName = "" + libKind + postfix[i] + ".js";
	$.evalFile(scriptPath + libName);

	Latin1ToWA = CryptoJS.enc.Latin1.parse(text);           // Latin1 to WordArray
	WAToBase64 = CryptoJS.enc.Base64.stringify(Latin1ToWA); // WordArray to Base64
	Base64ToWA = CryptoJS.enc.Base64.parse(WAToBase64);     // Base64 to WordArray
	WAToLatin1 = CryptoJS.enc.Latin1.stringify(Base64ToWA); // WordArray to Latin1

	result = text === WAToLatin1 ? "Passed" : "Failed";
	$.writeln("Testing " + libName + ": " + result);
}</pre>

  <p>
    Mixed results:
  </p>

  <pre class="lang:default highlight:0 decode:true">Testing enc-Base64.js: Passed
Testing enc-Base64-min.js: Failed
Testing enc-Base64-min-uglify.js: Passed
Testing enc-Base64-min-closure-whitespace.js: Passed
Testing enc-Base64-min-closure-simple.js: Failed</pre>

  <p>
    <strong>Problem</strong>: the minified file provided by CryptoJS authors and a version compressed with Closure (simple) fail. If you&#8217;re curious, their converted strings is <code>miBdJeBd</code>.<br /> Digression: you can&#8217;t directly output in the console the &#8220;wrong&#8221; string, you&#8217;ve to loop through it this way (mind you the string length appears to be correct, 28):
  </p>

  <pre class="lang:default decode:true">if (result ==="Failed") {
    var str = ""
        for (var j = 0; j &lt; WAToLatin1.length; j++) {
            str += WAToLatin1.charAt(j);
        }
    $.writeln("String is: " + "'" + str + "'");
}</pre>

  <h2>
    Conclusions
  </h2>

  <p>
    These tests &#8211; even if trivially simple &#8211; have shown that there&#8217;s a dangerous variability in the different output minified code can lead to.
  </p>

  <ul>
    <li>
      2/3 of the minified versions provided by Libraries authors failed a simple evaluation test.
    </li>
    <li>
      Closure Compiler (advanced) has the highest failure rate.
    </li>
    <li>
      UglifyJS and Closure (whitespace) have the highest success rate, though not 100%. Returning more or less the same filesize, they can be used interchangeably.
    </li>
    <li>
      Beware minified version, stick to uncompressed code if you can afford some extra bytes on the disk! You&#8217;ll save yourself some useless debug time!
    </li>
  </ul>

  <p>
    <img class="aligncenter size-full wp-image-2168" alt="table" src="/wp-content/uploads/2013/08/table.png" width="562" height="225" itemprop="image" srcset="/wp-content/uploads/2013/08/table.png 562w, /wp-content/uploads/2013/08/table-150x60.png 150w, /wp-content/uploads/2013/08/table-300x120.png 300w" sizes="(max-width: 562px) 100vw, 562px" />
  </p>

  <p>
    Mind you: this post is far from being an exhaustive review of code minification in ExtendScript- yet should suggest that an appropriate testing environment is a useful tool &#8211; so test your code!<br />

    <meta itemprop="dateCreated" content="2013-08-31" />

    <meta itemprop="isFamilyFriendly" content="true" />

    <meta itemprop="learningResourceType" content="testing" />

    <meta itemprop="timeRequired" content="P20M" />

    <meta itemprop="interactivityType" content="expositive" />
  </p>
</div>
