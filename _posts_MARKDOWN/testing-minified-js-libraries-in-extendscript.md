---
title: " Testing minified JS Libraries in ExtendScript\t\t"
tags:
  - Extendscript
  - Javascript
  - libraries
  - minify
  - testing
url: 2155.html
id: 2155
category:
  - Coding
  - ExtendScript / Javascript
date: 2013-08-29 15:01:46
---

It's not uncommon, when scripting for Adobe applications, to borrow JS libraries that have been originally written for web development. While the new generation of HTML Extensions will run on the [Chromium Embedded Framework](http://en.wikipedia.org/wiki/Chromium_Embedded_Framework "Google Chromium Embedded Framework"), traditional Adobe ExtendScript code is based upon the implementation of a different, older Javascript engine. Besides ECMAScript unsupported features (i.e. ES 5) I've noticed that using minified JS libraries is a risky business - scripts can break or fail silently. I've set up a proper testing environment to inspect them.

Minified libraries
------------------

Web developers need to keep their data transfer footprints as light as possible so they minify their code - i.e. use engines such as [Google Closure Compiler](http://closure-compiler.appspot.com/home "Google Closure Compiler") or [UglifyJS2](https://github.com/mishoo/UglifyJS2 "UglifyJS2") to transform eloquent, commented and nicely indented code into an unreadable blob of characters - for instance as follow is a minified version of a CryptoJS library for Base64 encoding:

(function(){var h=CryptoJS,k=h.f.c;h.e.b={stringify:function(b){var e=b.h,f=b.g,c=this.a;b.d();b=\[\];for(var a=0;a<f;a+=3)for(var d=(e\[a>>>2\]>>>24-8*(a%4)&255)<<16|(e\[a+1>>>2\]>>>24-8*((a+1)%4)&255)<<8|e\[a+2>>>2\]>>>24-8*((a+2)%4)&255,g=0;4>g&&a+0.75\*g<f;g++)b.push(c.charAt(d>>>6\*(3-g)&63));if(e=c.charAt(64))for(;b.length%4;)b.push(e);return b.join("")},parse:function(b){var e=b.length,f=this.a,c=f.charAt(64);c&&(c=b.indexOf(c),-1!=c&&(e=c));for(var c=\[\],a=0,d=0;d<e;d++)d%4&&(c\[a>>>2\]|=(f.indexOf(b.charAt(d-
1))<<2*(d%4)|f.indexOf(b.charAt(d))>>>6-2*(d%4))<<24-8*(a%4),a++);return k.create(c,a)},a:"ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/="}})();

Being generally installed on the machine the users work on, Adobe scripts don't benefit from a reduction in filesize (which is pretty small per se) so developers don't bother with minifiers; and ESTK can deploy a binary version, so code readability is not an issue. A couple of pretty neat libraries that I use - and will test in this post - are:

1.  [ES5-Shim](https://github.com/kriskowal/es5-shim "ES5 Shim") \- monkey-patch a JavaScript context to contain all EcmaScript 5 methods that can be faithfully emulated with a legacy JavaScript engine.
2.  [CryptoJS](http://code.google.com/p/crypto-js/ "CryptoJS") - a growing collection of standard and secure cryptographic algorithms implemented in JavaScript. I wrote [a "CryptoJS For Dummies" tutorial](http://localhost:8888/2012/10/crypto-js-tutorial-cryptography-for-dummies/ "CryptoJS Tutorial For Dummies") some time ago that appears to be quite popular.

Testing
-------

Libraries that I will test are the actual `ES5-Shim.js` and the `core.js` + `enc-Base64.js` (routine for Base64 encoding, as you may guess) from CryptoJS. Below you can find a comparative view over the original files, the minified version provided by the libs authors and minified versions that I've compiled using UglifyJS (default params) and Closure Compiler (three versions: whitespaces only, simple and advanced - detailed info about their differences [here](https://developers.google.com/closure/compiler/docs/compilation_levels?hl=it&csw=1 "Closure Compiler minify options")).

============================================
ES5-Shim
bytes - Library
\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-
45658 - es5-shim.js
16324 - es5-shim.min.js
16308 - es5-shim-min-uglify.js
16288 - es5-shim-min-closure-whitespace.js
11400 - es5-shim-min-closure-simple.js
11367 - es5-shim-min-closure-advanced.js

============================================
CryptoJS
bytes - Library
\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-
21468 - core.js
 3298 - core-min.js
 4973 - core-min-uglify.js
 4964 - core-min-closure-whitespace.js
 3176 - core-min-closure-simple.js
 2682 - core-min-closure-advanced.js
\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-
 3338 - enc-base64.js
  869 - enc-base64-min.js
 1251 - enc-base64-min-uglify.js
 1245 - enc-base64-min-closure-whitespace.js
  728 - enc-base64-min-closure-simple.js
  674 - enc-base64-min-closure-advanced.js

As you see the compression can be remarkable! (~21.000 -> ~3.000 bytes). Let's set up a test environment: I've created a Lib2Test folder in Photoshop CC/Presets/Scripts where I've put all the above listed files. In order to include a library in a jsx add the line:

$.evalFile("" + app.path + "/" + (localize("$$$/ScriptingSupport/InstalledScripts=Presets/Scripts")) + "/Libs2Test/<yourLibName>.js");

`$.evalFile` is more flexible than `#include` in my opinion, since you can load external code when you need it (i.e. testing a condition - if this happens, then I need this lib, else I don't or I need some other lib).

### ES5-Shim

I've written a simple evaluation test that cycle through all the original, provided minified version and custom made compression:

var scriptPath = "" + app.path + "/" + (localize("$$$/ScriptingSupport/InstalledScripts=Presets/Scripts")) + "/Libs2Test/";
var postfix = \["", "-min", "-min-uglify", "-min-closure-whitespace", "-min-closure-simple", "-min-closure-advanced"\];
var libKind = "es5-shim";
var libName = undefined;
for (var i = 0; i < postfix.length; i++) {
	libName = "" + libKind + postfix\[i\] + ".js";
	try {
		$.evalFile(scriptPath + libName);
		$.writeln("Testing " + libName + ": OK.");
	} catch (e) {
		$.writeln("Testing " + libName + ": ERROR " + e.number + "; " + e.message);
	}
}

As a result, I've found that:

Testing es5-shim.js: OK.
Testing es5-shim-min.js: ERROR 25; Expected: :
Testing es5-shim-min-uglify.js: ERROR 25; Expected: :
Testing es5-shim-min-closure-whitespace.js: ERROR 25; Expected: :
Testing es5-shim-min-closure-simple.js: ERROR 25; Expected: :
Testing es5-shim-min-closure-advanced.js: ERROR 25; Expected: :

**Problem**: for some reason - that I'm not willing to investigate - each and every compressed versions for Photoshop is indigestible. Conclusion: keep the original version!

### CryptoJS

The test for the CryptoJS libraries is slightly more complex. First let's just evaluate the `core.js`

Testing core.js: OK.
Testing core-min.js: OK.
Testing core-min-uglify.js: OK.
Testing core-min-closure-whitespace.js: OK.
Testing core-min-closure-simple.js: OK.
Testing core-min-closure-advanced.js: OK.

Then (including `core.js` because it's a dependency) the `enc-Base64.js` file:

Testing enc-Base64.js: OK.
Testing enc-Base64-min.js: OK.
Testing enc-Base64-min-uglify.js: OK.
Testing enc-Base64-min-closure-whitespace.js: OK.
Testing enc-Base64-min-closure-simple.js: OK.
Testing enc-Base64-min-closure-advanced.js: ERROR 21; undefined is not an object

**Problem**: apparently there are no evaluation errors, but for the Base64 module when compressed with Closure (advanced). Let's do a functional test on the minified versions of core.js - true, it's evaluated without errors, but this doesn't mean it works as expected. The following is not the most bulletproof test in the northern hemisphere, but should work. Basically I'm converting a Latin1 encoded String to a WordArray, then the reverse (WordArray to Latin1). If the two strings match, the test is passed. (If you have doubts about how CryptoJS works, please have a look at my [my tutorial](http://localhost:8888/2012/10/crypto-js-tutorial-cryptography-for-dummies/ "CryptoJS Tutorial For Dummies") to refresh your 007 skills).

var scriptPath = "" + app.path + "/" + (localize("$$$/ScriptingSupport/InstalledScripts=Presets/Scripts")) + "/Libs2Test/";
var postfix = \["", "-min", "-min-uglify", "-min-closure-whitespace", "-min-closure-simple", "-min-closure-advanced"\];
var libKind = "core";
var text = "My name is Bond, James Bond.";
var libName, Latin1ToWA, WAToLatin1, result;

for (var i = 0; i < postfix.length; i++) {
	libName = "" + libKind + postfix\[i\] + ".js";
	$.evalFile(scriptPath + libName);

	Latin1ToWA = CryptoJS.enc.Latin1.parse(text);		// Latin1 to WordArray
	WAToLatin1 = CryptoJS.enc.Latin1.stringify(Latin1ToWA);	// WordArray to Latin1

	result = text === WAToLatin1 ? "Passed" : "Failed";
	$.writeln("Testing " + libName + ": " + result);
}

The results are encouraging:

Testing core.js: Passed
Testing core-min.js: Passed
Testing core-min-uglify.js: Passed
Testing core-min-closure-whitespace.js: Passed
Testing core-min-closure-simple.js: Passed
Testing core-min-closure-advanced.js: Passed

Apparently - as long as this simple test is concerned - `core.js` has no functional problems. Let's involve the `enc-base64.js` library: this time the test will be a roundtrip:

*   from Latin1 to Word Array;
*   from WordArray to Base64;
*   from Base64 back to WordArray;
*   from WordArray back to Latin1.

If the two Latin1 strings are identical, the test is passed. I've removed the version compressed with Closure Compiler (advanced) because it gives evaluation errors.

var scriptPath = "" + app.path + "/" + (localize("$$$/ScriptingSupport/InstalledScripts=Presets/Scripts")) + "/Libs2Test/";
$.evalFile(scriptPath + "core.js"); // needed by the enc-Base64 library
var postfix = \["", "-min", "-min-uglify", "-min-closure-whitespace", "-min-closure-simple"\];
var libKind = "enc-Base64";
var text = "My name is Bond, James Bond.";
var libName, Latin1ToWA, WAToBase64, Base64ToWA, WAToLatin1, result;

for (var i = 0; i < postfix.length; i++) {
	libName = "" + libKind + postfix\[i\] + ".js";
	$.evalFile(scriptPath + libName);

	Latin1ToWA = CryptoJS.enc.Latin1.parse(text);           // Latin1 to WordArray
	WAToBase64 = CryptoJS.enc.Base64.stringify(Latin1ToWA); // WordArray to Base64
	Base64ToWA = CryptoJS.enc.Base64.parse(WAToBase64);     // Base64 to WordArray
	WAToLatin1 = CryptoJS.enc.Latin1.stringify(Base64ToWA); // WordArray to Latin1

	result = text === WAToLatin1 ? "Passed" : "Failed";
	$.writeln("Testing " + libName + ": " + result);
}

Mixed results:

Testing enc-Base64.js: Passed
Testing enc-Base64-min.js: Failed
Testing enc-Base64-min-uglify.js: Passed
Testing enc-Base64-min-closure-whitespace.js: Passed
Testing enc-Base64-min-closure-simple.js: Failed

**Problem**: the minified file provided by CryptoJS authors and a version compressed with Closure (simple) fail. If you're curious, their converted strings is `miBdJeBd`. Digression: you can't directly output in the console the "wrong" string, you've to loop through it this way (mind you the string length appears to be correct, 28):

if (result ==="Failed") {
    var str = ""
        for (var j = 0; j < WAToLatin1.length; j++) {
            str += WAToLatin1.charAt(j);
        }
    $.writeln("String is: " + "'" + str + "'");
}

Conclusions
-----------

These tests - even if trivially simple - have shown that there's a dangerous variability in the different output minified code can lead to.

*   2/3 of the minified versions provided by Libraries authors failed a simple evaluation test.
*   Closure Compiler (advanced) has the highest failure rate.
*   UglifyJS and Closure (whitespace) have the highest success rate, though not 100%. Returning more or less the same filesize, they can be used interchangeably.
*   Beware minified version, stick to uncompressed code if you can afford some extra bytes on the disk! You'll save yourself some useless debug time!

![table](http://localhost:8888/wp-content/uploads/2013/08/table.png) Mind you: this post is far from being an exhaustive review of code minification in ExtendScript- yet should suggest that an appropriate testing environment is a useful tool - so test your code!