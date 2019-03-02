---
id: 2219
title: BridgeTalk Export as Binary failure
date: 2013-09-16T22:53:37+01:00
author: Davide Barranca
excerpt: 'BridgeTalk, the system that enables messaging between applications in the Adobe scripting ecosystem, is prone to failure when evaluating functions via .toSource() - be aware and stringify them in advance!'
layout: post
guid: http://www.davidebarranca.com/?p=2219
permalink: /2013/09/bridgetalk-export-as-binary-failures/
standard_seo_post_level_layout:
  - ""
standard_link_url_field:
  - ""
standard_seo_post_meta_description:
  - BridgeTalk, the messaging system between Adobe apps, is prone to failure when evaluating functions via toSource(). Stringify them in advance
image: /wp-content/uploads/2013/09/Export_as_Binary.png
categories:
  - Coding
  - ExtendScript / Javascript
tags:
  - binary
  - BridgeTalk
  - export
  - Extendscript
  - toSource
---
<div class="pf-content">
  <p>
    BridgeTalk, the system that enables messaging between applications in the Adobe scripting ecosystem, is prone to failure when evaluating functions via <code>.toSource()</code> &#8211; be aware and stringify them in advance!
  </p>

  <h2>
    BridgeTalk 101
  </h2>

  <p>
    This simple script sends a message from Photoshop to Adobe Bridge &#8211; that is to say, Photoshop gives Bridge some code to execute:
  </p>

  <pre class="lang:default decode:true">#target photoshop
var bt = new BridgeTalk()
bt.target = 'bridge';
bt.timeout = 30;
bt.body = "$.writeln('Greetings from Bridge'); true;";
bt.onReceived = function(btObj) {
	return $.writeln('Message received.');
};
bt.onResult = function(btObj) {
	return $.writeln('BridgeTalk result: ' + btObj.body);
};
bt.onError = function(btObj) {
	return $.writeln(btObj.body);
};
bt.onTimeOut = function(btObj) {
	return $.writeln(btObj.body);
};
bt.send(31);</pre>

  <p>
    The Console outputs:
  </p>

  <pre class="lang:default highlight:0 decode:true ">Greetings from Bridge
BridgeTalk result: true
Result: true</pre>

  <p>
    It does the job configuring a <strong>BridgeTalk Object</strong>, which <code>.body</code> is the actual part of the script in charge of Bridge (here just a String).
  </p>

  <p>
    You can specify <strong>callbacks</strong> invoked by the target application (in the example Bridge) such as:
  </p>

  <ul>
    <li>
      <code>onResult()</code> &#8211; to return a response (to Photoshop), which is handled by the function;
    </li>
    <li>
      <code>onError()</code> &#8211; to return and handle an error;
    </li>
    <li>
      <code>onTimeout()</code> &#8211; when a timeout occurs;
    </li>
    <li>
      <code>onReceived()</code> &#8211; handler fired when the target app receives the message. For some reason, it&#8217;s triggered after, and not before, <code>onResult()</code>.
    </li>
  </ul>

  <p>
    Mind you, if the <code>.send()</code> method is called without any argument, the message is <strong>asynchronous</strong>, while if you specify the number of seconds to wait for before returning from the function, the message is sent <strong>synchronously</strong>. More info about BridgeTalk in the JavaScript Tools Guide CC, from page 170 onwards.
  </p>

  <h2>
    Using functions
  </h2>

  <p>
    When the message body is trivial, it can be assigned to a String literal; conversely, when you need the target app to perform some more elaborated tasks, it&#8217;s common practice to use a separate function. This is the same as the first example (I will omit some of the callbacks):
  </p>

  <pre class="lang:default decode:true">var bridgeFun = function () {
	$.writeln('Greetings from Bridge');
	return true;
}
var bt = new BridgeTalk()
bt.target = 'bridge';
bt.body = "var bridgeFun = " + bridgeFun.toSource() + "; bridgeFun();";
bt.onResult = function(btObj) {
	return $.writeln('BridgeTalk result: ' + btObj.body);
};
bt.send(31);</pre>

  <p>
    The <code>.toSource()</code> method returns a string representing the source code for the function. So the message body (<code>bt.body</code>) is:
  </p>

  <pre class="lang:default decode:true ">var bridgeFun = (function () { $.writeln('Greetings from Bridge'); return true; }); bridgeFun();</pre>

  <h2>
    Beware .toSource()
  </h2>

  <p>
    There are at least a couple of issues with <code>.toSource()</code>. First be careful when <strong>commenting lines</strong> in the separate function &#8211; always use <code>/* */</code> and not <code>//</code> because chances are that the latter will comment out everything after it.
  </p>

  <p>
    Second, if you <strong>Export as Binary</strong> the code from ESTK, it fails:
  </p>

  <pre class="lang:default decode:true ">eval("@JSXBIN@ES@2.0@MyBbyBn0AJJAnASzJjCjSjJjEjHjFiGjVjOByBNyBnAMAbyBn0ACJBnAEXzHjXjSj\
JjUjFjMjOCfjzBhEDfRBFeViHjSjFjFjUjJjOjHjThAjGjSjPjNhAiCjSjJjEjHjFffZCnAFct0DzAE\
CDnftJFnASzCjCjUFyBEjzKiCjSjJjEjHjFiUjBjMjLGfntnftJGnABXzGjUjBjSjHjFjUHfVFfyBne\
GjCjSjJjEjHjFfJHnABXzHjUjJjNjFjPjVjUIfVFfyBndgefJInABXzEjCjPjEjZJfVFfyBCzBhLKCK\
nEXzIjUjPiTjPjVjSjDjFLfVBfyBnfeQjWjBjShAjCjSjJjEjHjFiGjVjOhAhdhAnnneOhbhAjCjSjJ\
jEjHjFiGjVjOhIhJhbnfJJnABXzIjPjOiSjFjTjVjMjUMfVFfyBNyBnAMJbyBn0ABZKnAEXCfjDfRBC\
KnXJfVzFjCjUiPjCjKNfAeTiCjSjJjEjHjFiUjBjMjLhAjSjFjTjVjMjUhahAnffABN40BhAB0AECLn\
fJMnABXzHjPjOiFjSjSjPjSOfVFfyBNyBnAMMbyBn0ABZNnAEXCfjDfRBXJfVNfAffABN40BhAB0AEC\
OnfJPnABXzJjPjOiUjJjNjFiPjVjUPfVFfyBNyBnAMPbyBn0ABZQnAEXCfjDfRBXJfVNfAffABN40Bh\
AB0AECRnfJSnAEXzEjTjFjOjEQfVFfyBRBFdgfffACF4B0AiAB40BiAACAEByB");</pre>

  <p>
    (Remember to surround the binary chars blob with <code>eval(" ")</code> and close the lines with a <code>\</code>). The Console outputs:
  </p>

  <pre class="lang:default highlight:0 decode:true">Error in
Line 2: var bridgeFun = (function anonymous() {  [compiled code] } ); bridgeFun();
Expected: ]

Result: true</pre>

  <p>
    That&#8217;s because the purpose of binary encoding is hiding the code, so the function reads <code>[compiled code]</code> and is not stringified nor interpreted correctly.
  </p>

  <h2>
    Workarounds
  </h2>

  <p>
    You have a couple of choices to make it work. Either you can <strong>move the separate function out</strong> of the binary encoding and leave it readable:
  </p>

  <pre class="lang:default decode:true">var bridgeFun = function () {
	$.writeln('Greetings from Bridge');
	return true;
}

eval("@JSXBIN@ES@2.0@MyBbyBn0AIJAnASzCjCjUByBEjzKiCjSjJjEjHjFiUjBjMjLCfntnftJBnABXzGjU\
jBjSjHjFjUDfVBfyBneGjCjSjJjEjHjFfJCnABXzHjUjJjNjFjPjVjUEfVBfyBndgefJDnABXzEjCjP\
jEjZFfVBfyBCzBhLGCGnEXzIjUjPiTjPjVjSjDjFHfjzJjCjSjJjEjHjFiGjVjOIfnfeQjWjBjShAjC\
jSjJjEjHjFiGjVjOhAhdhAnnneOhbhAjCjSjJjEjHjFiGjVjOhIhJhbnfJEnABXzIjPjOiSjFjTjVjM\
jUJfVBfyBNyBnAMEbyBn0ABZFnAEXzHjXjSjJjUjFjMjOKfjzBhELfRBCGnXFfVzFjCjUiPjCjKMfAe\
TiCjSjJjEjHjFiUjBjMjLhAjSjFjTjVjMjUhahAnffABM40BhAB0AzANCGnfJHnABXzHjPjOiFjSjSj\
PjSOfVBfyBNyBnAMHbyBn0ABZInAEXKfjLfRBXFfVMfAffABM40BhAB0ANCJnfJKnABXzJjPjOiUjJj\
NjFiPjVjUPfVBfyBNyBnAMKbyBn0ABZLnAEXKfjLfRBXFfVMfAffABM40BhAB0ANCMnfJNnAEXzEjTj\
FjOjEQfVBfyBRBFdgfffABB40BiAABANByB");</pre>

  <p>
    (Mind you: I&#8217;ve cut the function out, then exported the binary, then pasted the function back above the binary).
  </p>

  <p>
    Or you can <strong>stringify the function</strong> and insert it as a String literal: for instance calling <code>$.writeln(bt.body)</code> on the original version and copy/paste the result from the Console back to the message body. The following version exports as binary without a glitch!
  </p>

  <pre class="lang:default decode:true">var bt = new BridgeTalk()
bt.target = 'bridge';
bt.body = "var bridgeFun = (function () { $.writeln('Greetings from Bridge'); return true; }); bridgeFun();"
bt.onResult = function(btObj) {
	return $.writeln('BridgeTalk result: ' + btObj.body);
};
bt.send(31);</pre>

  <p>
    &nbsp;
  </p>
</div>
