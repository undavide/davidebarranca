---
title: " BridgeTalk Export as Binary failure\t\t"
tags:
  - binary
  - BridgeTalk
  - export
  - Extendscript
  - toSource
url: 2219.html
id: 2219
category:
  - Coding
  - ExtendScript / Javascript
date: 2013-09-16 22:53:37
---

BridgeTalk, the system that enables messaging between applications in the Adobe scripting ecosystem, is prone to failure when evaluating functions via `.toSource()` \- be aware and stringify them in advance!

BridgeTalk 101
--------------

This simple script sends a message from Photoshop to Adobe Bridge - that is to say, Photoshop gives Bridge some code to execute:

#target photoshop
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
bt.send(31);

The Console outputs:

Greetings from Bridge
BridgeTalk result: true
Result: true

It does the job configuring a **BridgeTalk Object**, which `.body` is the actual part of the script in charge of Bridge (here just a String). You can specify **callbacks** invoked by the target application (in the example Bridge) such as:

*   `onResult()` \- to return a response (to Photoshop), which is handled by the function;
*   `onError()` \- to return and handle an error;
*   `onTimeout()` \- when a timeout occurs;
*   `onReceived()` \- handler fired when the target app receives the message. For some reason, it's triggered after, and not before, `onResult()`.

Mind you, if the `.send()` method is called without any argument, the message is **asynchronous**, while if you specify the number of seconds to wait for before returning from the function, the message is sent **synchronously**. More info about BridgeTalk in the JavaScript Tools Guide CC, from page 170 onwards.

Using functions
---------------

When the message body is trivial, it can be assigned to a String literal; conversely, when you need the target app to perform some more elaborated tasks, it's common practice to use a separate function. This is the same as the first example (I will omit some of the callbacks):

var bridgeFun = function () {
	$.writeln('Greetings from Bridge');
	return true;
}
var bt = new BridgeTalk()
bt.target = 'bridge';
bt.body = "var bridgeFun = " + bridgeFun.toSource() + "; bridgeFun();";
bt.onResult = function(btObj) {
	return $.writeln('BridgeTalk result: ' + btObj.body);
};
bt.send(31);

The `.toSource()` method returns a string representing the source code for the function. So the message body (`bt.body`) is:

var bridgeFun = (function () { $.writeln('Greetings from Bridge'); return true; }); bridgeFun();

Beware .toSource()
------------------

There are at least a couple of issues with `.toSource()`. First be careful when **commenting lines** in the separate function - always use `/* */` and not `//` because chances are that the latter will comment out everything after it. Second, if you **Export as Binary** the code from ESTK, it fails:

eval("@JSXBIN@ES@2.0@MyBbyBn0AJJAnASzJjCjSjJjEjHjFiGjVjOByBNyBnAMAbyBn0ACJBnAEXzHjXjSj\
JjUjFjMjOCfjzBhEDfRBFeViHjSjFjFjUjJjOjHjThAjGjSjPjNhAiCjSjJjEjHjFffZCnAFct0DzAE\
CDnftJFnASzCjCjUFyBEjzKiCjSjJjEjHjFiUjBjMjLGfntnftJGnABXzGjUjBjSjHjFjUHfVFfyBne\
GjCjSjJjEjHjFfJHnABXzHjUjJjNjFjPjVjUIfVFfyBndgefJInABXzEjCjPjEjZJfVFfyBCzBhLKCK\
nEXzIjUjPiTjPjVjSjDjFLfVBfyBnfeQjWjBjShAjCjSjJjEjHjFiGjVjOhAhdhAnnneOhbhAjCjSjJ\
jEjHjFiGjVjOhIhJhbnfJJnABXzIjPjOiSjFjTjVjMjUMfVFfyBNyBnAMJbyBn0ABZKnAEXCfjDfRBC\
KnXJfVzFjCjUiPjCjKNfAeTiCjSjJjEjHjFiUjBjMjLhAjSjFjTjVjMjUhahAnffABN40BhAB0AECLn\
fJMnABXzHjPjOiFjSjSjPjSOfVFfyBNyBnAMMbyBn0ABZNnAEXCfjDfRBXJfVNfAffABN40BhAB0AEC\
OnfJPnABXzJjPjOiUjJjNjFiPjVjUPfVFfyBNyBnAMPbyBn0ABZQnAEXCfjDfRBXJfVNfAffABN40Bh\
AB0AECRnfJSnAEXzEjTjFjOjEQfVFfyBRBFdgfffACF4B0AiAB40BiAACAEByB");

(Remember to surround the binary chars blob with `eval(" ")` and close the lines with a `\`). The Console outputs:

Error in 
Line 2: var bridgeFun = (function anonymous() {  \[compiled code\] } ); bridgeFun();
Expected: \]

Result: true

That's because the purpose of binary encoding is hiding the code, so the function reads `[compiled code]` and is not stringified nor interpreted correctly.

Workarounds
-----------

You have a couple of choices to make it work. Either you can **move the separate function out** of the binary encoding and leave it readable:

var bridgeFun = function () {
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
FjOjEQfVBfyBRBFdgfffABB40BiAABANByB");

(Mind you: I've cut the function out, then exported the binary, then pasted the function back above the binary). Or you can **stringify the function** and insert it as a String literal: for instance calling `$.writeln(bt.body)` on the original version and copy/paste the result from the Console back to the message body. The following version exports as binary without a glitch!

var bt = new BridgeTalk()
bt.target = 'bridge';
bt.body = "var bridgeFun = (function () { $.writeln('Greetings from Bridge'); return true; }); bridgeFun();"
bt.onResult = function(btObj) {
	return $.writeln('BridgeTalk result: ' + btObj.body);
};
bt.send(31);