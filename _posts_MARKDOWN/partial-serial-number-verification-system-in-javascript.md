---
title: " Partial Serial Number Verification System in Javascript\t\t"
tags:
  - Javascript
  - Licensing
  - Partial Serial Number Verification
url: 2843.html
id: 2843
category:
  - Coding
  - ExtendScript / Javascript
date: 2015-07-04 23:59:25
---

Back in 2007, developer **Brandon Staggs** wrote a [brilliant article](http://www.brandonstaggs.com/2007/07/26/implementing-a-partial-serial-number-verification-system-in-delphi/) about software licensing, showing how to implement what he calls a Partial Serial Number Verification System using the Delphi language. Apparently the remarkable technique first appeared around 2003 in a set of [slides](http://www.thornsoft.com/sic/2004/keys2004.ppt) by Chris Thornton, developer of the ClipMate software. Years have passed, cryptography is more _affordable_, yet I would say that this approach to the Software Licensing problem is still valid in some businesses - besides the fact it's fascinatingly clever. In a nutshell, Staggs shows how to build a serial number (seed, keys, checksum), which keys verification in the final product is partial - so that a cracker would hardly be able to build a long lasting keygen. **I've ported the code to Javascript** (originally written in Delphi). Two caveats:

1.  My understanding of Delphi is _very limited_: in fact I've never seen any Delphi code before, the port is entirely based on searching the Language Reference and a bit of common sense.
2.  As a translation to a different language, mine is a pretty literal one. I've also avoided ES5 / ES6 features on purpose: I wanted the code to run on the Photoshop ExtendScript interpreter (which is very unfortunately stuck to ES3).

As follows, Brandon Staggs introduction quoted with his permission from the [original article](http://www.brandonstaggs.com/2007/07/26/implementing-a-partial-serial-number-verification-system-in-delphi/) \- which I highly recommend you to read since he discusses the concept and details his code. Then you can find my Javascript version (frankly, I thought it would have been slightly easier to write). Eventually, a link section with some reference URLs that I visited while studying the code and that might help you as well.

### Brandon Staggs original Introduction

Most micro-ISVs _\[Independent Software Vendors\]_ use a serial number/registration code system to allow end users to unlock or activate their purchase.  The problem most of us have run into is that a few days or weeks after our software is released, someone has developed a keygen, a crack, or has leaked a serial number across the internet. There are several possible solutions to this problem. You could license a system like Armadillo/Software Passport or ASProtect, or you could distribute a separate full version as a download for your paying customers. Each option has advantages and disadvantages. What I am going to show you is a way to keep “rolling your own” license key system while making working cracks harder for crackers to produce, and working keygens a thing of the past. Aside: If you think it’s crazy to post this publicly where crackers can see it, don’t worry about that. I’m not posting anything they haven’t seen before. The entire point of partial key verification is that your code never includes enough information to reverse engineer a key generation algorithm. Also, I offer no warranty of any kind — this is for your information only! Now, on with things. Our license key system must meet some basic requirements.

1.  License keys must be easy enough to type in.
2.  We must be able to blacklist (revoke) a license key in the case of chargebacks or purchases with stolen credit cards.
3.  No “phoning home” to test keys.  Although this practice is becoming more and more prevalent, I still do not appreciate it as a user, so will not ask my users to put up with it.
4.  It should not be possible for a cracker to disassemble our released application and produce a working “keygen” from it. This means that our application will _not_ fully test a key for verification. Only _some_ of the key is to be tested. Further, each release of the application should test a _different_ portion of the key, so that a phony key based on an earlier release will not work on a later release of our software.
5.  Important: it should not be possible for a legitimate user to accidentally type in an invalid key that will appear to work but fail on a future version due to a typographical error.

The solution is called a _Partial Key Verification System_ because your software never tests the full key. Since your application does not include the code to test every portion of the key, it is impossible for a cracker to build a working valid key generator just by disassembling your executable code. This system is _not_ a way to prevent cracks entirely. It will still be possible for a cracker to edit your executable to jump over verification code. But such cracks only work on one specific release, and I’ll suggest a couple of tricks to make their job harder to complete successfully. \[...\] _\[Quoted with permission\]_

Javascript port of the original Delphi code
-------------------------------------------

/\*\*
 \* Generates a Key Byte
 \* @param {32bit integer} seed e.g. 0xA2791717
 \* @param {8bit integer} a    
 \* @param {8bit integer} b    
 \* @param {8bit integer} c    
 \* @return {8bit hex string} 
 */
function PKV_GetKeyByte(seed, a, b, c) {
	var result;
	a = a%25;
	b = b%3;
	if (a%2 == 0) {
		result = ((seed >> a) & 0x000000FF) ^ ((seed >> b) | c);
	} else {
		result = ((seed >> a) & 0x000000FF) ^ ((seed >> b) & c);
	}
	result = result & 0xFF; /* mask 255 values! */
    return result.toString(16).toUpperCase();
} 

/\*\*
 \* Generates the checksum
 \* @param {string} s Seed + Keys
 \* @return {16bit hex string} 
 */
function PKV_GetChecksum(s) {
	var left,	/* unsigned 16bit integer */
		right,	/* unsigned 16bit integer */
		sum;	/* unsigned 16bit integer */
	left  = 0x0056; /* 101 */
	right = 0x00AF; /* 175 */
	if (s.length) {
		for (var i = 0; i < s.length; i++) {
			right += s.charCodeAt(i);
			if (right > 0x00FF) { 
				right -= 0x00FF;
			}
			left += right;
			if (left > 0x00FF) {
				left -= 0x00FF;
			}
		};
	}
	sum = (left << 8) + right;
	return sum.toString(16);
}

/\*\*
 \* Generates a serial number
 \* @param {32bit integer} seed 
 \* @return {20 chars String}
 */
function PKV_MakeKey(seed) {
	var keyBytes = \[\],
		result = "", 
		serial;

	/\* Fill keyBytes with values derived from Seed.
	The parameters used here must be extactly the same
	as the ones used in the PKV_CheckKey function.
	A real key system should use more than four bytes. */

	keyBytes\[0\] = PKV_GetKeyByte(seed, 24, 3, 200);
	keyBytes\[1\] = PKV_GetKeyByte(seed, 10, 0, 56);
	keyBytes\[2\] = PKV_GetKeyByte(seed, 1, 2, 91);
	keyBytes\[3\] = PKV_GetKeyByte(seed, 7, 1, 100);

	/\* the key string begins with a hexidecimal string of the seed */
	result += seed.toString(16).toUpperCase();

	/\* then is followed by hexidecimal strings of each byte in the key */
	for (var i = 0; i < keyBytes.length; i++) {
		result += keyBytes\[i\].toUpperCase();
	};

	/\* Add checksum to key string */
	result += PKV_GetChecksum(result).toUpperCase();
	
	/\* Add some hyphens to make it easier to type */
	serial = result.split("");
	var j = serial.length - 4;
	while (j > 1) {
		serial.splice(j, 0, "-");
		j = j -4;
	}
	return serial.join("");

}

var KEY_GOOD = 0,
	KEY_INVALID = 1,
	KEY_BLACKLISTED = 2,
	KEY_PHONY = 3,
	BL = \[
		"11111111"
	\];

/\*\*
 \* Checks if the checksum in the serial is correct
 \* @param {String} key The 20 chars serial
 \* @return {boolean} 
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

/\*\*
 \* Check if the Serial is valid
 \* @param {String} s 20 chars serial
 \* @return {integer} 
 \* KEY_GOOD = 0,
 \* KEY_INVALID = 1,
 \* KEY_BLACKLISTED = 2,
 \* KEY_PHONY = 3,
 */
function PKV_CheckKey(s){
	var key,	/* String */
		kb,		/* String */
		seed,	/* Int32 */
		b,		/* Byte */
		result = KEY_INVALID;
	
	if (!PKV_CheckKeyChecksum(s)) {
		/\* bad checksum or wrong number of characters */
		return result;
	}
	/\* remove cosmetic hypens and normalize case */
	key = s.replace(/-/g,"").toUpperCase();
	
  	/\* test against blacklist */
  	var bl = BL.length
	if (bl) {
		for (var i = 0; i < bl; i++) {
			if( key.indexOf(BL\[i\].toUpperCase()) > -1 ) {
				result = KEY_BLACKLISTED;
				return result;
			}
		}
	}

	/\* At this point, the key is either valid or forged,
	 \* because a forged key can have a valid checksum.
	 \* We now test the "bytes" of the key to determine if it is
	 \* actually valid.
	 \* When building your release application, use conditional defines
	 \* or comment out most of the byte checks!  This is the heart
	 \* of the partial key verification system. By not compiling in
	 \* each check, there is no way for someone to build a keygen that
	 \* will produce valid keys.  If an invalid keygen is released, you
	 \* simply change which byte checks are compiled in, and any serial
	 \* number built with the fake keygen no longer works.
	 \* Note that the parameters used for PKV_GetKeyByte calls MUST
	 \* MATCH the values that PKV_MakeKey uses to make the key in the
	 \* first place! 
	 */
	
	result = KEY_PHONY;

	/\* extract the Seed from the supplied key string */
	seed = key.substr(0,8);
	/\* test whether the seed is a valid HEX */
	if (seed.match(/\[A-F0-9\]{8}/) === null) { return result; }

	/\* Keys test - never test them all! */

	/\* Testing K1 */
	kb = key.substr(8,2);
	b = PKV_GetKeyByte(parseInt(seed, 16), 24, 3, 200);
	if (kb !== b.toUpperCase()) { return result; }

	/\* Testing K2 */
	kb = key.substr(10,2);
	b = PKV_GetKeyByte(parseInt(seed, 16), 10, 0, 56);
	if (kb !== b.toUpperCase()) { return result; }

	/\* Testing K3 */
	kb = key.substr(12,2);
	b = PKV_GetKeyByte(parseInt(seed, 16), 1, 2, 91);
	if (kb !== b.toUpperCase()) { return result; }

	/\* Testing K4 */
	kb = key.substr(14,2);
	b = PKV_GetKeyByte(parseInt(seed, 16), 7, 1, 100);
	if (kb !== b.toUpperCase()) { return result; }

	/\* If we get this far, then it means the key is either good, or was made
	 \* with a keygen derived from "this" release. */ 

	result = KEY_GOOD;
	return result;
}

Reference Links
---------------

*   Brandon Staggs: [Implementing Partial Serial Number Verification System](http://www.brandonstaggs.com/2007/07/26/implementing-a-partial-serial-number-verification-system-in-delphi/).
*   Patrick McKenzie: [Everything You Need To Know About Registration Systems](http://www.kalzumeus.com/2006/09/05/everything-you-need-to-know-about-registration-systems/).
*   Allan Odgaard \[TextMate developer\]: [OpenSSL for License Keys](http://sigpipe.macromates.com/2004/09/05/using-openssl-for-license-keys/).
*   Chris Thornton: [Keygens, Protection, Encryption Panel Software Protection Methods](http://www.thornsoft.com/sic/2004/keys2004.ppt) slides.
*   [The Business of Software](http://discuss.joelonsoftware.com/default.asp?ixDiscussGroup=5&pg=pgSearchDiscussGroup&qDiscuss=partial+key+verification) community.
*   GitHub's JS encryption-related repos: [crypto-js](https://github.com/brix/crypto-js), [forge](https://github.com/digitalbazaar/forge), [jsencrypt](https://github.com/travist/jsencrypt) (RSA), [base32-js](https://github.com/agnoster/base32-js).
*   Youtube: [RSA Encryption Algorithm](https://www.youtube.com/watch?v=wXB-V_Keiu8).
*   Dan Vanderkam: [Arbitrary precision Hex <-> Dec converter](http://www.danvk.org/hex2dec.html).
*   MDN: [Bitwise Operators](https://developer.mozilla.org/it/docs/Web/JavaScript/Reference/Operators/Bitwise_Operators).
*   [Delphi Basics](http://www.delphibasics.co.uk/index.html) Reference Language.
*   [JS Minifier](http://fmarcia.info/jsmin/test.html) (use "Conservative" then strip newlines - _aggressive_ minifiers will break ExtendScript code, see [this article](http://localhost:8888/2013/08/testing-minified-js-libraries-in-extendscript/))

Hope this helps! Thanks for reading and if you feel dandy there's always that yellow "Donate" button in the top-left corner :-)

### Credits

I would like to thank Brandon Staggs for the kind permission of quoting his original article - pay a visit to [his blog](http://brandonstaggs.com) and the [StudyLamp Software LLC](http://www.studylamp.com) website.