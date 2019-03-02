---
title: " Sublime Text snippet: try/catch wrapping in CoffeeScript\t\t"
tags:
  - CoffeeScript
  - indentation
  - Snippet
  - Sublime Text
  - try catch
url: 2327.html
id: 2327
category:
  - Coding
  - CoffeeScript
date: 2013-11-13 13:54:02
---

One of the [Sublime Text](http://www.sublimetext.com "Sublime Text") editor's cool features are Snippets (bits of code you use very often and want to keep handy). The CoffeeScript package that I use, [Better-CoffeeScript](https://github.com/aponxi/sublime-better-coffeescript "Better CoffeeScript for Sublime Text on GitHub"), already has a snippet for try/catch block which creates some empty code to be filled:

try
	# ...
catch e
	# ...

Yet, I'd like to select some text, then have a shortcut-bound Snippet that **acts** on selected code and **wraps it** in a try/catch block. From this:

w = (s) ->
	$.writeln s
w app.activeDocument.name

To this:

try
	w = (s) ->	
		$.writeln s	
	w app.activeDocument.name	
catch e
	alert "Error: #{e.message}\\nLine: #{e.line}"
	# ...

(Don't worry if the code is weird, it's what ExtendScript, a Javascript superset made by Adobe for scripting purposes, looks like). It seems trivial - yet I've had issues with proper indentation unless I've eventually turned to Regular Expressions. There are plenty of tutorial about Sublime Text Snippet creation, so I'll keep it as short as possible.

1\. Create a New Snippet
------------------------

In Sublime, go to the **Tools > New Snippet...** menu and replace the boilerplate XML-like code with the following:

<snippet>
<content><!\[CDATA\[
try
${SELECTION/(\[^\\n\]*)/\\t$0/g}
catch e
	alert "Error: #{e.message}\\nLine: #{e.line}"
	${1:# ...}
\]\]></content>
	<scope>source.coffee</scope>
	<description>Try/Catch wrap</description>
</snippet>

The RegEx basically acts on the selected text `${SELECTION` (mind you, there's no tab in the snippet before it) and adds tabs to each line. Everything is wrapped in the try/catch block, and the caret is set in the last catch line, a comment `${1: #...}`. Feel free to customize the error handling to your needs. The `<scope>` tag instruct Sublime not to use this snippet unless the file is CoffeeScript, while the `<description>` is the label that will appear in the Snippets menu. You can follow the [Sublime Text Documentation](http://docs.sublimetext.info/en/sublime-text-3/extensibility/snippets.html "Sublime Text Documentation") to learn more about Snippet creation, and this summary for [Regular Expressions](http://www.cs.tut.fi/~jkorpela/perl/regexp.html "Perl Regular Expressions") in Perl.

2\. Save the Snippet
--------------------

Save to **Sublime Text > Packages > User** folder, with a `.sublime-snippet` extension. I've used `TryCatch_Wrap.sublime-snippet`.

3\. Bind to a Shortcut
----------------------

Go to the **Sublime Text > Preferences > Key-Bindings - User** menu. A `.sublime-keymap` file will open, mine was:

\[
	{ "keys": \["alt+shift+j"\], "command": "js\_coffee", "args":{"new\_file": true}}
\]

In case (like me) you already have a line, add a comma and the following:

\[
	{ "keys": \["alt+shift+j"\], "command": "js\_coffee", "args":{"new\_file": true}},
	{ "keys": \["ctrl+super+t"\], "command": "insert\_snippet", "args": {"name": "Packages/User/TryCatch\_Wrap.sublime-snippet"} }
\]

As I've written it, the Snippet is bound to `Command+Control+t`, but you can use the combination you like the most (have a look at **Key-Bindings - Default** to check whether you're overriding some shortcut already in use). If you're wondering, accepted modifiers are:

*   shift
*   ctrl
*   alt
*   super (Windows key, Command key)

Be sure to refer to the same `.sublime-snippet` filename, save and close the keymap.

4\. Enjoy!
----------

You've now built a Sublime Text Snippet for CoffeeScript that wraps and indent selected code in a try/catch block. You can select a portion of your code, hit ctrl+command+t and voilà! Hope this helps, you coffee fellows out there.