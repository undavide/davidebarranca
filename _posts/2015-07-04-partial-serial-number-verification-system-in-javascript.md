---
id: 2843
title: Partial Serial Number Verification System in Javascript
date: 2015-07-04T23:59:25+01:00
author: Davide Barranca
excerpt: |
  Back in 2007, developer Brandon Staggs wrote a brilliant article about software licensing, showing how to implement what he calls a Partial Serial Number Verification System using the Delphi language.
  Apparently the remarkable technique first appeared around 2003 in a set of slides by Chris Thornton, developer of the ClipMate software.
  
  Years have passed, cryptography is more affordable, yet I would say that this approach to the Software Licensing problem is still valid in some businesses - besides the fact it's fascinatingly clever.
  
  In a nutshell, Brandon shows how to build a serial number (seed, keys, checksum), which verification in the final product is partial - so that a cracker will never be able to build a long lasting keygen.
  
  I've ported the original code (written in Delphi) to Javascript.
layout: post
guid: http://www.davidebarranca.com/?p=2843
permalink: /2015/07/partial-serial-number-verification-system-in-javascript/
seo_post_level_layout:
  - ""
link_url_field:
  - ""
seo_post_meta_description:
  - "Implementing a Partial Serial Number Verification System in Javascript - a port of Brandon Staggs' original code in Delphi"
image: /wp-content/uploads/2015/07/rsa_encrypt_decrypt.png
categories:
  - Coding
  - ExtendScript / Javascript
tags:
  - Javascript
  - Licensing
  - Partial Serial Number Verification
---
<div class="pf-content">
  <p>
    Back in 2007, developer <strong>Brandon Staggs</strong> wrote a <a href="http://www.brandonstaggs.com/2007/07/26/implementing-a-partial-serial-number-verification-system-in-delphi/">brilliant article</a> about software licensing, showing how to implement what he calls a Partial Serial Number Verification System using the Delphi language.<br /> Apparently the remarkable technique first appeared around 2003 in a set of <a href="http://www.thornsoft.com/sic/2004/keys2004.ppt">slides</a> by Chris Thornton, developer of the ClipMate software.
  </p>
  
  <p>
    Years have passed, cryptography is more <em>affordable</em>, yet I would say that this approach to the Software Licensing problem is still valid in some businesses &#8211; besides the fact it&#8217;s fascinatingly clever.
  </p>
  
  <p>
    In a nutshell, Staggs shows how to build a serial number (seed, keys, checksum), which keys verification in the final product is partial &#8211; so that a cracker would hardly be able to build a long lasting keygen.
  </p>
  
  <p>
    <strong>I&#8217;ve ported the code to Javascript</strong> (originally written in Delphi). Two caveats:
  </p>
  
  <ol>
    <li>
      My understanding of Delphi is <em>very limited</em>: in fact I&#8217;ve never seen any Delphi code before, the port is entirely based on searching the Language Reference and a bit of common sense.
    </li>
    <li>
      As a translation to a different language, mine is a pretty literal one. I&#8217;ve also avoided ES5 / ES6 features on purpose: I wanted the code to run on the Photoshop ExtendScript interpreter (which is very unfortunately stuck to ES3).
    </li>
  </ol>
  
  <p>
    As follows, Brandon Staggs introduction quoted with his permission from the <a href="http://www.brandonstaggs.com/2007/07/26/implementing-a-partial-serial-number-verification-system-in-delphi/">original article</a> &#8211; which I highly recommend you to read since he discusses the concept and details his code.<br /> Then you can find my Javascript version (frankly, I thought it would have been slightly easier to write).<br /> Eventually, a link section with some reference URLs that I visited while studying the code and that might help you as well.
  </p>
  
  <div class="well" style="margin-top: 20px">
    <h3 style="margin-top: 0">
      Brandon Staggs original Introduction
    </h3>
    
    <p>
      Most micro-ISVs <em>[Independent Software Vendors]</em> use a serial number/registration code system to allow end users to unlock or activate their purchase.  The problem most of us have run into is that a few days or weeks after our software is released, someone has developed a keygen, a crack, or has leaked a serial number across the internet.
    </p>
    
    <p>
      There are several possible solutions to this problem. You could license a system like Armadillo/Software Passport or ASProtect, or you could distribute a separate full version as a download for your paying customers. Each option has advantages and disadvantages. What I am going to show you is a way to keep “rolling your own” license key system while making working cracks harder for crackers to produce, and working keygens a thing of the past.
    </p>
    
    <p>
      Aside: If you think it’s crazy to post this publicly where crackers can see it, don’t worry about that. I’m not posting anything they haven’t seen before. The entire point of partial key verification is that your code never includes enough information to reverse engineer a key generation algorithm. Also, I offer no warranty of any kind — this is for your information only! Now, on with things.
    </p>
    
    <p>
      Our license key system must meet some basic requirements.
    </p>
    
    <ol type="1">
      <li>
        License keys must be easy enough to type in.
      </li>
      <li>
        We must be able to blacklist (revoke) a license key in the case of chargebacks or purchases with stolen credit cards.
      </li>
      <li>
        No “phoning home” to test keys.  Although this practice is becoming more and more prevalent, I still do not appreciate it as a user, so will not ask my users to put up with it.
      </li>
      <li>
        It should not be possible for a cracker to disassemble our released application and produce a working “keygen” from it. This means that our application will <em>not</em> fully test a key for verification. Only <em>some</em> of the key is to be tested. Further, each release of the application should test a <em>different</em> portion of the key, so that a phony key based on an earlier release will not work on a later release of our software.
      </li>
      <li>
        Important: it should not be possible for a legitimate user to accidentally type in an invalid key that will appear to work but fail on a future version due to a typographical error.
      </li>
    </ol>
    
    <p>
      The solution is called a <em>Partial Key Verification System</em> because your software never tests the full key. Since your application does not include the code to test every portion of the key, it is impossible for a cracker to build a working valid key generator just by disassembling your executable code.
    </p>
    
    <p>
      This system is <em>not</em> a way to prevent cracks entirely. It will still be possible for a cracker to edit your executable to jump over verification code. But such cracks only work on one specific release, and I’ll suggest a couple of tricks to make their job harder to complete successfully. [&#8230;]<br /> <em>[Quoted with permission]</em>
    </p>
  </div>
  
  <h2>
    Javascript port of the original Delphi code
  </h2>
  
  <pre class="lang:js decode:true ">/**
 * Generates a Key Byte
 * @param {32bit integer} seed e.g. 0xA2791717
 * @param {8bit integer} a    
 * @param {8bit integer} b    
 * @param {8bit integer} c    
 * @return {8bit hex string} 
 */
function PKV_GetKeyByte(seed, a, b, c) {
	var result;
	a = a%25;
	b = b%3;
	if (a%2 == 0) {
		result = ((seed &gt;&gt; a) & 0x000000FF) ^ ((seed &gt;&gt; b) | c);
	} else {
		result = ((seed &gt;&gt; a) & 0x000000FF) ^ ((seed &gt;&gt; b) & c);
	}
	result = result & 0xFF; /* mask 255 values! */
    return result.toString(16).toUpperCase();
} 

/**
 * Generates the checksum
 * @param {string} s Seed + Keys
 * @return {16bit hex string} 
 */
function PKV_GetChecksum(s) {
	var left,	/* unsigned 16bit integer */
		right,	/* unsigned 16bit integer */
		sum;	/* unsigned 16bit integer */
	left  = 0x0056; /* 101 */
	right = 0x00AF; /* 175 */
	if (s.length) {
		for (var i = 0; i &lt; s.length; i++) {
			right += s.charCodeAt(i);
			if (right &gt; 0x00FF) { 
				right -= 0x00FF;
			}
			left += right;
			if (left &gt; 0x00FF) {
				left -= 0x00FF;
			}
		};
	}
	sum = (left &lt;&lt; 8) + right;
	return sum.toString(16);
}

/**
 * Generates a serial number
 * @param {32bit integer} seed 
 * @return {20 chars String}
 */
function PKV_MakeKey(seed) {
	var keyBytes = [],
		result = "", 
		serial;

	/* Fill keyBytes with values derived from Seed.
	The parameters used here must be extactly the same
	as the ones used in the PKV_CheckKey function.
	A real key system should use more than four bytes. */

	keyBytes[0] = PKV_GetKeyByte(seed, 24, 3, 200);
	keyBytes[1] = PKV_GetKeyByte(seed, 10, 0, 56);
	keyBytes[2] = PKV_GetKeyByte(seed, 1, 2, 91);
	keyBytes[3] = PKV_GetKeyByte(seed, 7, 1, 100);

	/* the key string begins with a hexidecimal string of the seed */
	result += seed.toString(16).toUpperCase();

	/* then is followed by hexidecimal strings of each byte in the key */
	for (var i = 0; i &lt; keyBytes.length; i++) {
		result += keyBytes[i].toUpperCase();
	};

	/* Add checksum to key string */
	result += PKV_GetChecksum(result).toUpperCase();
	
	/* Add some hyphens to make it easier to type */
	serial = result.split("");
	var j = serial.length - 4;
	while (j &gt; 1) {
		serial.splice(j, 0, "-");
		j = j -4;
	}
	return serial.join("");

}

var KEY_GOOD = 0,
	KEY_INVALID = 1,
	KEY_BLACKLISTED = 2,
	KEY_PHONY = 3,
	BL = [
		"11111111"
	];

/**
 * Checks if the checksum in the serial is correct
 * @param {String} key The 20 chars serial
 * @return {boolean} 
 */
function PKV_CheckKeyChecksum(key) {
	var s, c,
		result = false;
	s = key.replace(/-/g,"").toUpperCase();
	if (s.length !== 20) { return result }
	c = s.slice(s.length - 4);
	s = s.slice(0, 16);
	result = ( c === PKV_GetChecksum(s).toUpperCase());
	return result;
}

/**
 * Check if the Serial is valid
 * @param {String} s 20 chars serial
 * @return {integer} 
 * KEY_GOOD = 0,
 * KEY_INVALID = 1,
 * KEY_BLACKLISTED = 2,
 * KEY_PHONY = 3,
 */
function PKV_CheckKey(s){
	var key,	/* String */
		kb,		/* String */
		seed,	/* Int32 */
		b,		/* Byte */
		result = KEY_INVALID;
	
	if (!PKV_CheckKeyChecksum(s)) {
		/* bad checksum or wrong number of characters */
		return result;
	}
	/* remove cosmetic hypens and normalize case */
	key = s.replace(/-/g,"").toUpperCase();
	
  	/* test against blacklist */
  	var bl = BL.length
	if (bl) {
		for (var i = 0; i &lt; bl; i++) {
			if( key.indexOf(BL[i].toUpperCase()) &gt; -1 ) {
				result = KEY_BLACKLISTED;
				return result;
			}
		}
	}

	/* At this point, the key is either valid or forged,
	 * because a forged key can have a valid checksum.
	 * We now test the "bytes" of the key to determine if it is
	 * actually valid.
	 * When building your release application, use conditional defines
	 * or comment out most of the byte checks!  This is the heart
	 * of the partial key verification system. By not compiling in
	 * each check, there is no way for someone to build a keygen that
	 * will produce valid keys.  If an invalid keygen is released, you
	 * simply change which byte checks are compiled in, and any serial
	 * number built with the fake keygen no longer works.
	 * Note that the parameters used for PKV_GetKeyByte calls MUST
	 * MATCH the values that PKV_MakeKey uses to make the key in the
	 * first place! 
	 */
	
	result = KEY_PHONY;

	/* extract the Seed from the supplied key string */
	seed = key.substr(0,8);
	/* test whether the seed is a valid HEX */
	if (seed.match(/[A-F0-9]{8}/) === null) { return result; }

	/* Keys test - never test them all! */

	/* Testing K1 */
	kb = key.substr(8,2);
	b = PKV_GetKeyByte(parseInt(seed, 16), 24, 3, 200);
	if (kb !== b.toUpperCase()) { return result; }

	/* Testing K2 */
	kb = key.substr(10,2);
	b = PKV_GetKeyByte(parseInt(seed, 16), 10, 0, 56);
	if (kb !== b.toUpperCase()) { return result; }

	/* Testing K3 */
	kb = key.substr(12,2);
	b = PKV_GetKeyByte(parseInt(seed, 16), 1, 2, 91);
	if (kb !== b.toUpperCase()) { return result; }

	/* Testing K4 */
	kb = key.substr(14,2);
	b = PKV_GetKeyByte(parseInt(seed, 16), 7, 1, 100);
	if (kb !== b.toUpperCase()) { return result; }

	/* If we get this far, then it means the key is either good, or was made
	 * with a keygen derived from "this" release. */ 

	result = KEY_GOOD;
	return result;
}</pre>
  
  <h2>
    Reference Links
  </h2>
  
  <ul>
    <li>
      Brandon Staggs: <a href="http://www.brandonstaggs.com/2007/07/26/implementing-a-partial-serial-number-verification-system-in-delphi/">Implementing Partial Serial Number Verification System</a>.
    </li>
    <li>
      Patrick McKenzie: <a href="http://www.kalzumeus.com/2006/09/05/everything-you-need-to-know-about-registration-systems/">Everything You Need To Know About Registration Systems</a>.
    </li>
    <li>
      Allan Odgaard [TextMate developer]: <a href="http://sigpipe.macromates.com/2004/09/05/using-openssl-for-license-keys/">OpenSSL for License Keys</a>.
    </li>
    <li>
      Chris Thornton: <a href="http://www.thornsoft.com/sic/2004/keys2004.ppt">Keygens, Protection, Encryption Panel Software Protection Methods</a> slides.
    </li>
    <li>
      <a href="http://discuss.joelonsoftware.com/default.asp?ixDiscussGroup=5&pg=pgSearchDiscussGroup&qDiscuss=partial+key+verification">The Business of Software</a> community.
    </li>
    <li>
      GitHub&#8217;s JS encryption-related repos: <a href="https://github.com/brix/crypto-js">crypto-js</a>, <a href="https://github.com/digitalbazaar/forge">forge</a>, <a href="https://github.com/travist/jsencrypt">jsencrypt</a> (RSA), <a href="https://github.com/agnoster/base32-js">base32-js</a>.
    </li>
    <li>
      Youtube: <a href="https://www.youtube.com/watch?v=wXB-V_Keiu8">RSA Encryption Algorithm</a>.
    </li>
    <li>
      Dan Vanderkam: <a href="http://www.danvk.org/hex2dec.html">Arbitrary precision Hex <-> Dec converter</a>.
    </li>
    <li>
      MDN: <a href="https://developer.mozilla.org/it/docs/Web/JavaScript/Reference/Operators/Bitwise_Operators">Bitwise Operators</a>.
    </li>
    <li>
      <a href="http://www.delphibasics.co.uk/index.html">Delphi Basics</a> Reference Language.
    </li>
    <li>
      <a href="http://fmarcia.info/jsmin/test.html">JS Minifier</a> (use &#8220;Conservative&#8221; then strip newlines &#8211; <em>aggressive</em> minifiers will break ExtendScript code, see <a href="http://localhost:8888/2013/08/testing-minified-js-libraries-in-extendscript/">this article</a>)
    </li>
  </ul>
  
  <p>
    Hope this helps!<br /> Thanks for reading and if you feel dandy there&#8217;s always that yellow &#8220;Donate&#8221; button in the top-left corner 🙂
  </p>
  
  <h3>
    Credits
  </h3>
  
  <p>
    I would like to thank Brandon Staggs for the kind permission of quoting his original article &#8211; pay a visit to <a href="http://brandonstaggs.com">his blog</a> and the <a href="http://www.studylamp.com">StudyLamp Software LLC</a> website.
  </p>
  
  <p>
    &nbsp;
  </p>
</div>

<!-- Share-Widget Button BEGIN --><a href="javascript:void(0);" myshare\_id="mys\_shareit" myshare\_url="http://localhost:8888/2015/07/partial-serial-number-verification-system-in-javascript/" myshare\_title="Partial Serial Number Verification System in Javascript" rel="nofollow" onclick=" return false;" style="text-decoration:none; color:#000000; font-size:11px; line-height:20px;"> 

<img src="http://localhost:8888/wp-content/plugins/share-widget/img/share-button-white-small.png" height="20" alt="Share" style="border:0" /> </a> <!-- Share-Widget Button END -->