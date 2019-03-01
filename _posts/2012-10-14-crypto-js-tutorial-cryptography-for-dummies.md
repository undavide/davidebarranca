---
id: 1232
title: CryptoJS Tutorial For Dummies
date: 2012-10-14T01:30:15+01:00
author: Davide Barranca
excerpt: "If you need cryptography in your Javascript or Extendscript files but your topic's knowledge ends with Alan Turing breaking the Enigma machine in WWII, read along. I'll try to save you some time showing basic operations with the great Crypto-JS package."
layout: post
guid: http://www.davidebarranca.com/?p=1232
permalink: /2012/10/crypto-js-tutorial-cryptography-for-dummies/
standard_seo_post_level_layout:
  - ""
standard_link_url_field:
  - ""
standard_seo_post_meta_description:
  - 'CryptoJS basic tutorial and practical tips - How to implement in your Extendscript / Javascript code this great free Cryptography library.'
seo_post_level_layout:
  - ""
link_url_field:
  - ""
seo_post_meta_description:
  - ""
image: /wp-content/uploads/2012/10/DB_-2012-10-13-at-11.05.08.png
categories:
  - Coding
tags:
  - Crypto-JS
  - cryptography
  - Extendscript
  - Javascript
  - tutorial
---
<div class="pf-content">
  <div>
    <p>
      <time datetime="2011-04-01"><em>[Updated October 16<sup>th</sup> 2012 with corrected information from CryptoJS author Jeff Mott &#8211; look for the <strong>UPDATED</strong> tag below]</em></time>
    </p>
    
    <p>
      The Google Code <a title="Crypto JS home page" href="http://code.google.com/p/crypto-js" target="_blank">Crypto-JS</a> page titles &#8220;<em>JavaScript implementations of standard and secure cryptographic algorithms</em>&#8221; and that&#8217;s exactly what it&#8217;s all about. I strongly suggest you to read the <a title="CryptoJS Quick Start Guide" href="http://code.google.com/p/crypto-js/#Quick-start_Guide" target="_blank">QuickStart Guide</a>: this CryptoJS Tutorial for Dummies is just made with comments in the margin of it &#8211; underlining important stuff I didn&#8217;t notice when I first approached it. The official documentation is precise yet kind of succint &#8211; it makes sense to those who master the topic, but it may disorient the newcomers (like me when I get there for the first time).
    </p>
    
    <p>
      I&#8217;m just on my way learning both cryptography theory and this nice JS library, the following is only a collection of basic advices and personal notes. Please forgive me for using a very&#8230; non-specialized jargon.
    </p>
    
    <blockquote>
      <p>
        <em>Caveat</em>: I use to write code in <strong>Extendscript</strong> &#8211; which is an Adobe made superset of Javascript adding nice extras, such as: file-system access, user interface development tools, external communication, preprocessing directives, XML integration, etc. I say so because it seems that my mindset is a bit shifted from the one of the pure Javascript developer (for instance, I&#8217;ve not to worry about privacy issues arising from readable code &#8211; my .jsx files are binary encrypted by default when I export them from the ESTK &#8211; ExtendScript ToolKit). I use to write <a title="Coffeescript home page" href="http://coffeescript.org" target="_blank">Coffeescript</a>, a nice smart language that compiles to Javascript, but I won&#8217;t us it here. Lot of extra resources on the Coffeescript language <a href="http://wiht.link/CoffeeScript">here</a>.
      </p>
    </blockquote>
    
    <h2>
      Files
    </h2>
    
    <p>
      First, <a title="Crypto JS download page" href="http://code.google.com/p/crypto-js/downloads/list" target="_blank">download</a> the CryptoJS package (3.0.2 at the time of this post). It contains two folders:
    </p>
    
    <ul>
      <li>
        <strong>components</strong> &#8211; with both minified and commented JS files.
      </li>
      <li>
        <strong>rollups</strong> &#8211; minified files (one for each algorithm) bundled with core code.
      </li>
    </ul>
    
    <p>
      Components files have dependencies: you have to link at least core.js, while rollups are quite self contained. In Extendscript, save a test.jsx file alongside (or within) the CryptoJS folder and set the preprocessing directives:
    </p>
    
    <pre class="brush:js">#include "components/core-min.js"
#include "rollups/sha1.js"
#include "rollups/aes.js"
#include "components/enc-base64-min.js"
#include "components/enc-utf16-min.js"</pre>
    
    <p>
      The #include (redundant here) are the equivalent of the following tag in HTML documents:
    </p>
    
    <pre class="brush:xml">&lt;script src="http://crypto-js.googlecode.com/svn/tags/3.0.2/build/rollups/aes.js"&gt;&lt;/script&gt;</pre>
    
    <p>
      Be aware that you need to save the file at least once otherwise the include can&#8217;t resolve the path.
    </p>
    
    <h2>
      Word Array and Encodings
    </h2>
    
    <p>
      CryptoJS makes large use of Word Array &#8211; that is, arrays of 32-bits words (instances of the CryptoJS.lib.WordArray); few useful functions:
    </p>
    
    <pre class="brush:js">// Creates a word array filled with random bytes.
// @param {number} nBytes The number of random bytes to generate.
var wordArray = CryptoJS.lib.WordArray.random(16);

// Converts a String to word array
var words = CryptoJS.enc.Utf16.parse('Hello, World!'); 
// 00480065006c006c006f002c00200057006f0072006c00640021

// Reverses the word array to a readable String
var utf8 = CryptoJS.enc.Utf16.stringify(words); // Hello World</pre>
    
    <p>
      Mind you, quoting the Guide: &#8220;<em>When you use a WordArray object in a string context</em>&#8220;, that is, an alert box or the Console, &#8220;<em>it&#8217;s automatically converted to a <strong>hex string</strong></em>&#8220;.
    </p>
    
    <p>
      CryptoJS can manage different encodings, such as Base64, Hex, Latin1, UTF-8 and UTF-16. If you wonder how data would look like converted to them, paste this script in ESTK (Adobe&#8217;s ExtendScriptToolKit):
    </p>
    
    <pre class="brush:js">#include "components/enc-base64-min.js"
#include "components/enc-utf16-min.js"

// Save at least once in the appropriate folder in order to 
// help the #include to resolve the path

var wordArray = CryptoJS.lib.WordArray.random(32);
alert("Random WordArray" +"\nHex: "+ wordArray + "\n\nBase 64: " + CryptoJS.enc.Base64.stringify(wordArray) +"\n\nLatin 1: " + CryptoJS.enc.Latin1.stringify(wordArray) + "\n\nUTF-8: " + CryptoJS.enc.Utf8.stringify(wordArray) + "\n\nUTF-16: " + CryptoJS.enc.Utf16.stringify(wordArray))</pre>
    
    <p>
      The output is as follows &#8211; I&#8217;ve used an Alert box because ESTK&#8217;s $.writeln() (as I suspect both console.log() and debug(), depending on the tool you use to run your JS code) can&#8217;t output but Hex and Base64:
    </p>
    
    <p style="text-align: center;">
      <img class="aligncenter size-full wp-image-1243" title="ESTK output" src="http://localhost:8888/wp-content/uploads/2012/10/DB_-2012-10-13-at-11.05.08.png" alt="ESTK output" width="534" height="410" srcset="http://localhost:8888/wp-content/uploads/2012/10/DB_-2012-10-13-at-11.05.08.png 534w, http://localhost:8888/wp-content/uploads/2012/10/DB_-2012-10-13-at-11.05.08-150x115.png 150w, http://localhost:8888/wp-content/uploads/2012/10/DB_-2012-10-13-at-11.05.08-300x230.png 300w" sizes="(max-width: 534px) 100vw, 534px" />
    </p>
    
    <h3>
      Conversion Functions
    </h3>
    
    <p>
      Include both <code>enc-base64-min.js</code> and <code>enc-utf16-min.js</code> in your code.
    </p>
    
    <p>
      The functions are in the form:
    </p>
    
    <pre class="brush:js">CryptoJS.enc./Encoding/.parse();
// interprets the param as /Encoded/												// and converts it to Word Array

CryptoJS.enc./Encoding/.stringify();
// interprets the param as Word Array												// and converts it to the /Encoded/ String

var wordArray = CryptoJS.enc.Utf16.parse('Hello, World!'); // Word Array
var utf16 = CryptoJS.enc.Utf16.stringify(wordArray); // String</pre>
    
    <p>
      Few examples as follows:
    </p>
    
    <pre class="brush:js">// Encodings:
var words  = CryptoJS.enc.Base64.parse('SGVsbG8sIFdvcmxkIQ==');
var base64 = CryptoJS.enc.Base64.stringify(words);

var words = CryptoJS.enc.Latin1.parse('Hello, World!');
var latin1 = CryptoJS.enc.Latin1.stringify(words);

var words = CryptoJS.enc.Hex.parse('48656c6c6f2c20576f726c6421');
var hex = CryptoJS.enc.Hex.stringify(words);

var words = CryptoJS.enc.Utf8.parse('♦') 
var utf8  = CryptoJS.enc.Utf8.stringify(words);

var words = CryptoJS.enc.Utf16.parse('Hello, World!');
var utf16 = CryptoJS.enc.Utf16.stringify(words);</pre>
    
    <h2>
      Hash Functions
    </h2>
    
    <p>
      So to speak, <em>hashers</em> are functions that take an input (no matter how large) and maps it to a fixed size, smaller one (the hash, or checksum). You can&#8217;t convert a hash back to the original input, yet you can check if the original data has been corrupted comparing the hashes.
    </p>
    
    <p>
      CryptoJS implements MD5, SHA-1 (used by Git) and its variant (2, 224, 384, 256 and 512).
    </p>
    
    <pre class="brush:js">var hash = CryptoJS.SHA1("Message");
// c4b0858dd7f14b154cac443b659bf9def034e01f</pre>
    
    <p>
      The input <code>"Message"</code> can either be a <em>WordArray</em> or a <em>String</em> (which will automatically be converted to the former, encoded UTF-8). Then you may:
    </p>
    
    <pre class="brush:js">// CONVERT WordArray to Latin1 or Base64 - better off using alert()!
var hash_Latin1 = hash.toString(CryptoJS.enc.Latin1);
var hash_Base64 = hash.toString(CryptoJS.enc.Base64);</pre>
    
    <p>
      Mind you the two forms are equivalent:
    </p>
    
    <pre class="brush:js">var hash_Base64 = hash.toString(CryptoJS.enc.Base64);
var hash_Base64 = CryptoJS.enc.Base64.stringify(hash);</pre>
    
    <h2>
      Ciphers
    </h2>
    
    <p>
      CryptoJS implements several Cipher Algorithms &#8211; in the following example AES:
    </p>
    
    <pre class="brush:js">#include "rollups/aes.js"

var encrypted = CryptoJS.AES.encrypt("Message", "Secret Passphrase");
$.writeln(encrypted); 
// AABsAABkAABiAAAAAAAAAABNAABlAABPAAC0AABHAAA=

var decrypted = CryptoJS.AES.decrypt(encrypted, "Secret Passphrase");
$.writeln(decrypted); 
// 4d657373616765</pre>
    
    <p>
      The encryption results in a Base64 string, while the decrypted string is Hex. To get back the &#8220;Message&#8221; you need to:
    </p>
    
    <pre class="brush:js">$.writeln(decrypted.toString(CryptoJS.enc.Utf8));
// Message</pre>
    
    <p>
      If you run the encryption code several times, you&#8217;ll notice that the result will change:
    </p>
    
    <pre class="brush:js">var encrypted = CryptoJS.AES.encrypt("Message", "Secret Passphrase");
$.writeln(encrypted); 
// AABsAABkAAA0AAAAAAAAAABfAADSAAC5AABcAACjAAA=

var encrypted = CryptoJS.AES.encrypt("Message", "Secret Passphrase");
$.writeln(encrypted); 
// AABsAABkAAA8AAAAAAAAAAA1AACIAAChAAAqAADMAAA=

var encrypted = CryptoJS.AES.encrypt("Message", "Secret Passphrase");
$.writeln(encrypted); 
// AABsAABkAAArAAAAAAAAAAA4AABGAAAkAACpAAB5AAA=</pre>
    
    <p>
      yet each one of them will always decrypt to &#8220;Message&#8221;. How come?
    </p>
    
    <h3>
      Key, Salt and Initialization Vector
    </h3>
    
    <p>
      I&#8217;m not a cryptography expert &#8211; my raw understanding of the matter (after digging Wikipedia and StackOverflow) is as follows.
    </p>
    
    <p>
      Human memorizable passphrase are known to be bad ones. In order to make them more secure you can add a bunch of random bits (the <strong>salt</strong>) so that the actual <code>key = function(Salt, Passphrase)</code>. An effect of Salt is that the same passphrase doesn&#8217;t always produce the same key.
    </p>
    
    <p>
      IV (initialization vector) is used, similarly, to ensure that the same plaintext (&#8220;Message&#8221;) doesn&#8217;t return the same ciphertext. It appears that Salt is used with passphrase to generate a key for encryption, then the resulting encryption is processed with IV. So to speak, the <code>encryption = function(plaintext, passphrase, salt, IV)</code>. So when you write:
    </p>
    
    <pre class="brush:js">var encrypted = CryptoJS.AES.encrypt("Message", "Secret Passphrase");</pre>
    
    <p>
      CryptoJS randomly generates for you what it need. [<strong>UPDATED</strong>] Alternatively, you can specify:
    </p>
    
    <pre class="brush:js">var key = CryptoJS.lib.WordArray.random(16);
var iv  = CryptoJS.lib.WordArray.random(16);
var encrypted = CryptoJS.AES.encrypt("Message", key, { iv: iv });
var decrypted = CryptoJS.AES.decrypt(encrypted, key, { iv: iv });</pre>
    
    <h3>
      Input and output
    </h3>
    
    <p>
      The encrypt function takes a plaintext input as a String or WordArray (the &#8220;Message&#8221;), and either a similar passphrase or Hex Key and IV.
    </p>
    
    <p>
      [<strong>UPDATED</strong>] It&#8217;s important to reaffirm that, if you use a String as a passphrase, CryptoJS uses it to generate a random key and IV:
    </p>
    
    <pre class="brush:js">var key = CryptoJS.enc.Hex.stringify(CryptoJS.lib.WordArray.random(16));
var iv  = CryptoJS.enc.Hex.stringify(CryptoJS.lib.WordArray.random(16));
// WRONG!!
var encrypted = CryptoJS.AES.encrypt("Message", key, {iv: iv});</pre>
    
    <p>
      The code above is <strong>not proper</strong>, for two reasons:
    </p>
    
    <ol>
      <li>
        Key and IV are Strings, not Word Arrays!
      </li>
      <li>
        Consequently, the key is used as a String passphrase from which to derive a random <em>actual</em> key + IV pair &#8211; so they&#8217;re not the <code>key</code> and <code>iv</code> variables you&#8217;ve declared.
      </li>
    </ol>
    
    <p>
      When it comes to the output, things are a bit different because if you:
    </p>
    
    <pre class="brush:js">alert (typeof encrypted); // object</pre>
    
    <p>
      Actually, the encryption output is an object called <strong>CipherParams</strong>, and you can access its properties:
    </p>
    
    <pre class="brush:js">var encrypted = CryptoJS.AES.encrypt("Message", "Secret Passphrase");

alert(encrypted); // U2FsdGVkX1+iX5Ey7GqLND5UFUoV0b7rUJ2eEvHkYqA=
alert(encrypted.key); // 74eb593087a982e2a6f5dded54ecd96d1fd0f3d44a58728cdcd40c55227522223
alert(encrypted.iv); // 7781157e2629b094f0e3dd48c4d786115
alert(encrypted.salt); // 7a25f9132ec6a8b34
alert(encrypted.ciphertext); // 73e54154a15d1beeb509d9e12f1e462a0</pre>
    
    <p>
      I&#8217;ve had some troubles understanding why they use this object as the vector of encrypted data &#8211; the pack of bits you and the other guy oversea securely swap. It looks like you&#8217;re putting your treasure map into a casket and ship it with the keys to open it (I would have used the ciphertext only). It puzzled me because you can successfully write:
    </p>
    
    <pre class="brush:js">var encrypted = CryptoJS.AES.encrypt("Message", "Secret Passphrase");
var decrypted = CryptoJS.AES.decrypt(encrypted, encrypted.key, { iv: encrypted.iv});</pre>
    
    <p>
      [<strong>UPDATED</strong>] The explanation came directly from CryptoJS author Jeff Moss who wrote me:
    </p>
    
    <blockquote>
      <p>
        Although the key is a property in the CipherParams object, the key is not included when that CipherParams object is serialized to a string. By default, CipherParams objects are serialized using a format from OpenSSL. Just do encrypted.toString(), and you can safely send that to the other side of the ocean.
      </p>
    </blockquote>
    
    <p>
      So the <code>alert(encrypted);</code> hex string you see in the last but one code block is definitely safe to use and share:
    </p>
    
    <pre class="brush:js">var encrypted = CryptoJS.AES.encrypt("Message", "Secret Passphrase");
var decrypted = CryptoJS.AES.decrypt("U2FsdGVkX1+iX5Ey7GqLND5UFUoV0b7rUJ2eEvHkYqA=", "Secret Passphrase") // or just encrypted.toString()</pre>
    
    <p>
      There&#8217;s definitely more to dig in this great CryptoJS library, I&#8217;m looking forward to explore it! This should be just enough to let you start implementing cryptography in your own projects.
    </p>
  </div>
</div>

<!-- Share-Widget Button BEGIN --><a href="javascript:void(0);" myshare\_id="mys\_shareit" myshare\_url="http://localhost:8888/2012/10/crypto-js-tutorial-cryptography-for-dummies/" myshare\_title="CryptoJS Tutorial For Dummies" rel="nofollow" onclick=" return false;" style="text-decoration:none; color:#000000; font-size:11px; line-height:20px;"> 

<img src="http://localhost:8888/wp-content/plugins/share-widget/img/share-button-white-small.png" height="20" alt="Share" style="border:0" /> </a> <!-- Share-Widget Button END -->