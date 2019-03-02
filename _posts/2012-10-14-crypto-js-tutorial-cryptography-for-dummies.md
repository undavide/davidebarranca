---
title: CryptoJS Tutorial For Dummies
date: 2012-10-14T01:30:15+01:00
author: Davide Barranca
excerpt: "CryptoJS basic tutorial and practical tips. How to implement in your Extendscript / Javascript code this great free Cryptography library."
layout: post
permalink: /2012/10/crypto-js-tutorial-cryptography-for-dummies/
description: "CryptoJS basic tutorial and practical tips. How to implement in your Extendscript / Javascript code this great free Cryptography library."
image: /wp-content/uploads/2012/10/DB_-2012-10-13-at-11.05.08.png
categories:
  - Scripting
tags:
  - Crypto-JS
---

_[Updated October 16th 2012 with corrected information from CryptoJS author Jeff Mott - look for the **UPDATED** tag below]_  
The Google Code [Crypto-JS](http://code.google.com/p/crypto-js "Crypto JS home page") page titles "_JavaScript implementations of standard and secure cryptographic algorithms_" and that's exactly what it's all about. I strongly suggest you to read the [QuickStart Guide](http://code.google.com/p/crypto-js/#Quick-start_Guide "CryptoJS Quick Start Guide"): this CryptoJS Tutorial for Dummies is just made with comments in the margin of it - underlining important stuff I didn't notice when I first approached it. The official documentation is precise yet kind of succint - it makes sense to those who master the topic, but it may disorient the newcomers (like me when I get there for the first time). I'm just on my way learning both cryptography theory and this nice JS library, the following is only a collection of basic advices and personal notes. Please forgive me for using a very... non-specialized jargon.

> _Caveat_: I use to write code in **Extendscript** \- which is an Adobe made superset of Javascript adding nice extras, such as: file-system access, user interface development tools, external communication, preprocessing directives, XML integration, etc. I say so because it seems that my mindset is a bit shifted from the one of the pure Javascript developer (for instance, I've not to worry about privacy issues arising from readable code - my .jsx files are binary encrypted by default when I export them from the ESTK - ExtendScript ToolKit). I use to write [Coffeescript](http://coffeescript.org "Coffeescript home page"), a nice smart language that compiles to Javascript, but I won't us it here. Lot of extra resources on the Coffeescript language [here](http://wiht.link/CoffeeScript).

## Files

First, [download](http://code.google.com/p/crypto-js/downloads/list "Crypto JS download page") the CryptoJS package (3.0.2 at the time of this post). It contains two folders:

*   **components** \- with both minified and commented JS files.
*   **rollups** \- minified files (one for each algorithm) bundled with core code.

Components files have dependencies: you have to link at least core.js, while rollups are quite self contained. In Extendscript, save a test.jsx file alongside (or within) the CryptoJS folder and set the preprocessing directives:

{% highlight js %}
#include "components/core-min.js"
#include "rollups/sha1.js"
#include "rollups/aes.js"
#include "components/enc-base64-min.js"
#include "components/enc-utf16-min.js"
{% endhighlight %}

The #include (redundant here) are the equivalent of the following tag in HTML documents:

{% highlight html %}
<script src="http://crypto-js.googlecode.com/svn/tags/3.0.2/build/rollups/aes.js"></script>
{% endhighlight %}

Be aware that you need to save the file at least once otherwise the include can't resolve the path.

## Word Array and Encodings

CryptoJS makes large use of Word Array - that is, arrays of 32-bits words (instances of the `CryptoJS.lib.WordArray`); few useful functions:

{% highlight js %}
// Creates a word array filled with random bytes.
// @param {number} nBytes The number of random bytes to generate.
var wordArray = CryptoJS.lib.WordArray.random(16);

// Converts a String to word array
var words = CryptoJS.enc.Utf16.parse('Hello, World!');
// 00480065006c006c006f002c00200057006f0072006c00640021

// Reverses the word array to a readable String
var utf8 = CryptoJS.enc.Utf16.stringify(words); // Hello World
{% endhighlight %}

Mind you, quoting the Guide: "_When you use a WordArray object in a string context_", that is, an alert box or the Console, "_it's automatically converted to a **hex string**_". CryptoJS can manage different encodings, such as Base64, Hex, Latin1, UTF-8 and UTF-16. If you wonder how data would look like converted to them, paste this script in ESTK (Adobe's ExtendScriptToolKit):

{% highlight js %}
#include "components/enc-base64-min.js"
#include "components/enc-utf16-min.js"

// Save at least once in the appropriate folder in order to
// help the #include to resolve the path

var wordArray = CryptoJS.lib.WordArray.random(32);
alert("Random WordArray" +"\\nHex: "+ wordArray + "\\n\\nBase 64: " + CryptoJS.enc.Base64.stringify(wordArray) +"\\n\\nLatin 1: " + CryptoJS.enc.Latin1.stringify(wordArray) + "\\n\\nUTF-8: " + CryptoJS.enc.Utf8.stringify(wordArray) + "\\n\\nUTF-16: " + CryptoJS.enc.Utf16.stringify(wordArray))
{% endhighlight %}

The output is as follows - I've used an Alert box because ESTK's $.writeln() (as I suspect both console.log() and debug(), depending on the tool you use to run your JS code) can't output but Hex and Base64:

![ESTK output](/wp-content/uploads/2012/10/DB_-2012-10-13-at-11.05.08.png "ESTK output")

### Conversion Functions

Include both `enc-base64-min.js` and `enc-utf16-min.js` in your code. The functions are in the form:

{% highlight js %}
CryptoJS.enc./Encoding/.parse();
// interprets the param as /Encoded/												// and converts it to Word Array

CryptoJS.enc./Encoding/.stringify();
// interprets the param as Word Array												// and converts it to the /Encoded/ String

var wordArray = CryptoJS.enc.Utf16.parse('Hello, World!'); // Word Array
var utf16 = CryptoJS.enc.Utf16.stringify(wordArray); // String
{% endhighlight %}

Few examples as follows:

{% highlight js %}
// Encodings:
var words  = CryptoJS.enc.Base64.parse('SGVsbG8sIFdvcmxkIQ==');
var base64 = CryptoJS.enc.Base64.stringify(words);

var words = CryptoJS.enc.Latin1.parse('Hello, World!');
var latin1 = CryptoJS.enc.Latin1.stringify(words);

var words = CryptoJS.enc.Hex.parse('48656c6c6f2c20576f726c6421');
var hex = CryptoJS.enc.Hex.stringify(words);

var words = CryptoJS.enc.Utf8.parse('♦')
var utf8  = CryptoJS.enc.Utf8.stringify(words);

var words = CryptoJS.enc.Utf16.parse('Hello, World!');
var utf16 = CryptoJS.enc.Utf16.stringify(words);
{% endhighlight %}

## Hash Functions

So to speak, _hashers_ are functions that take an input (no matter how large) and maps it to a fixed size, smaller one (the hash, or checksum). You can't convert a hash back to the original input, yet you can check if the original data has been corrupted comparing the hashes. CryptoJS implements MD5, SHA-1 (used by Git) and its variant (2, 224, 384, 256 and 512).

{% highlight js %}
var hash = CryptoJS.SHA1("Message");
// c4b0858dd7f14b154cac443b659bf9def034e01f
{% endhighlight %}

The input `"Message"` can either be a _WordArray_ or a _String_ (which will automatically be converted to the former, encoded UTF-8). Then you may:

{% highlight js %}
// CONVERT WordArray to Latin1 or Base64 - better off using alert()!
var hash_Latin1 = hash.toString(CryptoJS.enc.Latin1);
var hash_Base64 = hash.toString(CryptoJS.enc.Base64);
{% endhighlight %}

Mind you the two forms are equivalent:

{% highlight js %}
var hash_Base64 = hash.toString(CryptoJS.enc.Base64);
var hash_Base64 = CryptoJS.enc.Base64.stringify(hash);
{% endhighlight %}

## Ciphers

CryptoJS implements several Cipher Algorithms - in the following example AES:

{% highlight js %}
#include "rollups/aes.js"

var encrypted = CryptoJS.AES.encrypt("Message", "Secret Passphrase");
$.writeln(encrypted);
// AABsAABkAABiAAAAAAAAAABNAABlAABPAAC0AABHAAA=

var decrypted = CryptoJS.AES.decrypt(encrypted, "Secret Passphrase");
$.writeln(decrypted);
// 4d657373616765
{% endhighlight %}

The encryption results in a Base64 string, while the decrypted string is Hex. To get back the "Message" you need to:

{% highlight js %}
$.writeln(decrypted.toString(CryptoJS.enc.Utf8));
// Message
{% endhighlight %}

If you run the encryption code several times, you'll notice that the result will change:

{% highlight js %}
var encrypted = CryptoJS.AES.encrypt("Message", "Secret Passphrase");
$.writeln(encrypted);
// AABsAABkAAA0AAAAAAAAAABfAADSAAC5AABcAACjAAA=

var encrypted = CryptoJS.AES.encrypt("Message", "Secret Passphrase");
$.writeln(encrypted);
// AABsAABkAAA8AAAAAAAAAAA1AACIAAChAAAqAADMAAA=

var encrypted = CryptoJS.AES.encrypt("Message", "Secret Passphrase");
$.writeln(encrypted);
// AABsAABkAAArAAAAAAAAAAA4AABGAAAkAACpAAB5AAA=
{% endhighlight %}

yet each one of them will always decrypt to "Message". How come?

### Key, Salt and Initialization Vector

I'm not a cryptography expert - my raw understanding of the matter (after digging Wikipedia and StackOverflow) is as follows. Human memorizable passphrase are known to be bad ones. In order to make them more secure you can add a bunch of random bits (the **salt**) so that the actual `key = function(Salt, Passphrase)`. An effect of Salt is that the same passphrase doesn't always produce the same key. IV (initialization vector) is used, similarly, to ensure that the same plaintext ("Message") doesn't return the same ciphertext. It appears that Salt is used with passphrase to generate a key for encryption, then the resulting encryption is processed with IV. So to speak, the `encryption = function(plaintext, passphrase, salt, IV)`. So when you write:

{% highlight js %}
var encrypted = CryptoJS.AES.encrypt("Message", "Secret Passphrase");
{% endhighlight %}

CryptoJS randomly generates for you what it need. [**UPDATED**] Alternatively, you can specify:

{% highlight js %}
var key = CryptoJS.lib.WordArray.random(16);
var iv  = CryptoJS.lib.WordArray.random(16);
var encrypted = CryptoJS.AES.encrypt("Message", key, { iv: iv });
var decrypted = CryptoJS.AES.decrypt(encrypted, key, { iv: iv });
{% endhighlight %}

### Input and output

The encrypt function takes a plaintext input as a String or WordArray (the "Message"), and either a similar passphrase or Hex Key and IV. [**UPDATED**] It's important to reaffirm that, if you use a String as a passphrase, CryptoJS uses it to generate a random key and IV:

{% highlight js %}
var key = CryptoJS.enc.Hex.stringify(CryptoJS.lib.WordArray.random(16));
var iv  = CryptoJS.enc.Hex.stringify(CryptoJS.lib.WordArray.random(16));
// WRONG!!
var encrypted = CryptoJS.AES.encrypt("Message", key, {iv: iv});
{% endhighlight %}

The code above is **not proper**, for two reasons:

1.  Key and IV are Strings, not Word Arrays!
2.  Consequently, the key is used as a String passphrase from which to derive a random _actual_ key + IV pair - so they're not the `key` and `iv` variables you've declared.

When it comes to the output, things are a bit different because if you:

{% highlight js %}
alert (typeof encrypted); // object
{% endhighlight %}

Actually, the encryption output is an object called **CipherParams**, and you can access its properties:

{% highlight js %}
var encrypted = CryptoJS.AES.encrypt("Message", "Secret Passphrase");

alert(encrypted); // U2FsdGVkX1+iX5Ey7GqLND5UFUoV0b7rUJ2eEvHkYqA=
alert(encrypted.key); // 74eb593087a982e2a6f5dded54ecd96d1fd0f3d44a58728cdcd40c55227522223
alert(encrypted.iv); // 7781157e2629b094f0e3dd48c4d786115
alert(encrypted.salt); // 7a25f9132ec6a8b34
alert(encrypted.ciphertext); // 73e54154a15d1beeb509d9e12f1e462a0
{% endhighlight %}

I've had some troubles understanding why they use this object as the vector of encrypted data - the pack of bits you and the other guy oversea securely swap. It looks like you're putting your treasure map into a casket and ship it with the keys to open it (I would have used the ciphertext only). It puzzled me because you can successfully write:

{% highlight js %}
var encrypted = CryptoJS.AES.encrypt("Message", "Secret Passphrase");
var decrypted = CryptoJS.AES.decrypt(encrypted, encrypted.key, { iv: encrypted.iv});
{% endhighlight %}

[**UPDATED**] The explanation came directly from CryptoJS author Jeff Moss who wrote me:

> Although the key is a property in the CipherParams object, the key is not included when that CipherParams object is serialized to a string. By default, CipherParams objects are serialized using a format from OpenSSL. Just do encrypted.toString(), and you can safely send that to the other side of the ocean.

So the `alert(encrypted);` hex string you see in the last but one code block is definitely safe to use and share:

{% highlight js %}
var encrypted = CryptoJS.AES.encrypt("Message", "Secret Passphrase");
var decrypted = CryptoJS.AES.decrypt("U2FsdGVkX1+iX5Ey7GqLND5UFUoV0b7rUJ2eEvHkYqA=", "Secret Passphrase") // or just encrypted.toString()
{% endhighlight %}

There's definitely more to dig in this great CryptoJS library, I'm looking forward to explore it! This should be just enough to let you start implementing cryptography in your own projects.
