---
title: Partial Serial Number Verification System in Javascript
date: 2015-07-04T23:59:25+01:00
author: Davide Barranca
excerpt: "Implementing a Partial Serial Number Verification System in Javascript - a port of Brandon Staggs' original code in Delphi"
layout: post
permalink: /2015/07/partial-serial-number-verification-system-in-javascript/
description: "Implementing a Partial Serial Number Verification System in Javascript - a port of Brandon Staggs' original code in Delphi"
image: /wp-content/uploads/2015/07/rsa_encrypt_decrypt.png
category:
  - Scripting
tags:
  - Licensing
  - Partial Serial Number Verification
---

Back in 2007, developer **Brandon Staggs** wrote a [brilliant article](http://www.brandonstaggs.com/2007/07/26/implementing-a-partial-serial-number-verification-system-in-delphi/) about software licensing, showing how to implement what he calls a Partial Serial Number Verification System using the Delphi language. Apparently the remarkable technique first appeared around 2003 in a set of [slides](http://www.thornsoft.com/sic/2004/keys2004.ppt) by Chris Thornton, developer of the ClipMate software. Years have passed, cryptography is more _affordable_, yet I would say that this approach to the Software Licensing problem is still valid in some businesses - besides the fact it's fascinatingly clever. In a nutshell, Staggs shows how to build a serial number (seed, keys, checksum), which keys verification in the final product is partial - so that a cracker would hardly be able to build a long lasting keygen. **I've ported the code to Javascript** (originally written in Delphi). Two caveats:

1.  My understanding of Delphi is _very limited_: in fact I've never seen any Delphi code before, the port is entirely based on searching the Language Reference and a bit of common sense.
2.  As a translation to a different language, mine is a pretty literal one. I've also avoided ES5 / ES6 features on purpose: I wanted the code to run on the Photoshop ExtendScript interpreter (which is very unfortunately stuck to ES3).

As follows, Brandon Staggs introduction quoted with his permission from the [original article](http://www.brandonstaggs.com/2007/07/26/implementing-a-partial-serial-number-verification-system-in-delphi/) \- which I highly recommend you to read since he discusses the concept and details his code. Then you can find my Javascript version (frankly, I thought it would have been slightly easier to write). Eventually, a link section with some reference URLs that I visited while studying the code and that might help you as well.

> ### Brandon Staggs original Introduction
>
> Most micro-ISVs _\[Independent Software Vendors\]_ use a serial number/registration code system to allow end users to unlock or activate their purchase.  The problem most of us have run into is that a few days or weeks after our software is released, someone has developed a keygen, a crack, or has leaked a serial number across the internet. There are several possible solutions to this problem. You could license a system like Armadillo/Software Passport or ASProtect, or you could distribute a separate full version as a download for your paying customers. Each option has advantages and disadvantages. What I am going to show you is a way to keep “rolling your own” license key system while making working cracks harder for crackers to produce, and working keygens a thing of the past. Aside: If you think it’s crazy to post this publicly where crackers can see it, don’t worry about that. I’m not posting anything they haven’t seen before. The entire point of partial key verification is that your code never includes enough information to reverse engineer a key generation algorithm. Also, I offer no warranty of any kind — this is for your information only! Now, on with things. Our license key system must meet some basic requirements.
>
> 1.  License keys must be easy enough to type in.
> 2.  We must be able to blacklist (revoke) a license key in the case of chargebacks or purchases with stolen credit cards.
> 3.  No “phoning home” to test keys.  Although this practice is becoming more and more prevalent, I still do not appreciate it as a user, so will not ask my users to put up with it.
> 4.  It should not be possible for a cracker to disassemble our released application and produce a working “keygen” from it. This means that our application will _not_ fully test a key for verification. Only _some_ of the key is to be tested. Further, each release of the application should test a _different_ portion of the key, so that a phony key based on an earlier release will not work on a later release of our software.
> 5.  Important: it should not be possible for a legitimate user to accidentally type in an invalid key that will appear to work but fail on a future version due to a typographical error.
>
> The solution is called a _Partial Key Verification System_ because your software never tests the full key. Since your application does not include the code to test every portion of the key, it is impossible for a cracker to build a working valid key generator just by disassembling your executable code. This system is _not_ a way to prevent cracks entirely. It will still be possible for a cracker to edit your executable to jump over verification code. But such cracks only work on one specific release, and I’ll suggest a couple of tricks to make their job harder to complete successfully. [...] _[Quoted with permission]_

## Javascript port of the original Delphi code

{% gist 0a23024ac85fa560593597b26166b81b%}

## Reference Links

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
*   [JS Minifier](http://fmarcia.info/jsmin/test.html) (use "Conservative" then strip newlines - _aggressive_ minifiers will break ExtendScript code, see [this article](/2013/08/testing-minified-js-libraries-in-extendscript/))

Hope this helps! Thanks for reading and if you feel dandy there's always that yellow "Donate" button in the top-left corner :-)

### Credits

I would like to thank Brandon Staggs for the kind permission of quoting his original article - pay a visit to [his blog](http://brandonstaggs.com) and the [StudyLamp Software LLC](http://www.studylamp.com) website.
