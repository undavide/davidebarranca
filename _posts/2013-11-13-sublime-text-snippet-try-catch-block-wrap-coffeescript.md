---
id: 2327
title: 'Sublime Text snippet: try/catch wrapping in CoffeeScript'
date: 2013-11-13T13:54:02+01:00
author: Davide Barranca
excerpt: Ever wanted a shortcut-bound, Sublime Text snippet that wraps a selected portion of CoffeeScript code in a properly indented try/catch block? Me too, so I wrote it, here it is!
layout: post
guid: http://www.davidebarranca.com/?p=2327
permalink: /2013/11/sublime-text-snippet-try-catch-block-wrap-coffeescript/
standard_seo_post_level_layout:
  - ""
standard_link_url_field:
  - ""
standard_seo_post_meta_description:
  - A Sublime Text snippet that wraps selected CoffeeScript code in a properly indented try/catch block, with instruction on shortcut binding.
image: /wp-content/uploads/2013/11/wrap_try_catch_coffeescript.png
category:
  - Coding
  - CoffeeScript
tags:
  - CoffeeScript
  - indentation
  - Snippet
  - Sublime Text
  - try catch
---
<div class="pf-content">
  <p>
    One of the <span itemprop="mentions" itemscope="" itemtype="http://schema.org/SoftwareApplication"><a itemprop="url" title="Sublime Text" href="http://www.sublimetext.com"><span itemprop="name">Sublime Text</span></a></span> editor&#8217;s cool features are <span itemprop="mentions">Snippets</span> (bits of code you use very often and want to keep handy). The CoffeeScript package that I use, <a title="Better CoffeeScript for Sublime Text on GitHub" href="https://github.com/aponxi/sublime-better-coffeescript" target="_blank">Better-CoffeeScript</a>, already has a snippet for <span itemprop="mentions">try/catch</span> block which creates some empty code to be filled:
  </p>

  <pre class="lang:coffee decode:true">try
	# ...
catch e
	# ...</pre>

  <p>
    Yet, I&#8217;d like to select some text, then have a shortcut-bound Snippet that <strong>acts</strong> on selected code and <strong>wraps it</strong> in a try/catch block. From this:
  </p>

  <pre class="lang:default decode:true">w = (s) -&gt;
	$.writeln s
w app.activeDocument.name</pre>

  <p>
    To this:
  </p>

  <pre class="lang:coffee decode:true">try
	w = (s) -&gt;
		$.writeln s
	w app.activeDocument.name
catch e
	alert "Error: #{e.message}\nLine: #{e.line}"
	# ...</pre>

  <p>
    (Don&#8217;t worry if the code is weird, it&#8217;s what ExtendScript, a Javascript superset made by Adobe for scripting purposes, looks like). It seems trivial &#8211; yet I&#8217;ve had issues with proper indentation unless I&#8217;ve eventually turned to Regular Expressions.
  </p>

  <p>
    There are plenty of tutorial about Sublime Text Snippet creation, so I&#8217;ll keep it as short as possible.
  </p>

  <h2>
    1. Create a New Snippet
  </h2>

  <p>
    In Sublime, go to the <strong>Tools > New Snippet&#8230;</strong> menu and replace the boilerplate XML-like code with the following:
  </p>

  <pre class="lang:xhtml decode:true">&lt;snippet&gt;
&lt;content&gt;&lt;![CDATA[
try
${SELECTION/([^\n]*)/\t$0/g}
catch e
	alert "Error: #{e.message}\nLine: #{e.line}"
	${1:# ...}
]]&gt;&lt;/content&gt;
	&lt;scope&gt;source.coffee&lt;/scope&gt;
	&lt;description&gt;Try/Catch wrap&lt;/description&gt;
&lt;/snippet&gt;</pre>

  <p>
    The RegEx basically acts on the selected text <code>${SELECTION</code> (mind you, there&#8217;s no tab in the snippet before it) and adds tabs to each line. Everything is wrapped in the try/catch block, and the caret is set in the last catch line, a comment <code>${1: #...}</code>. Feel free to customize the error handling to your needs.
  </p>

  <p>
    The <code>&lt;scope&gt;</code> tag instruct Sublime not to use this snippet unless the file is CoffeeScript, while the <code>&lt;description&gt;</code> is the label that will appear in the Snippets menu. You can follow the <a title="Sublime Text Documentation" href="http://docs.sublimetext.info/en/sublime-text-3/extensibility/snippets.html" target="_blank">Sublime Text Documentation</a> to learn more about Snippet creation, and this summary for <a title="Perl Regular Expressions" href="http://www.cs.tut.fi/~jkorpela/perl/regexp.html" target="_blank">Regular Expressions</a> in Perl.
  </p>

  <h2>
    2. Save the Snippet
  </h2>

  <p>
    Save to <strong>Sublime Text > Packages > User</strong> folder, with a <code>.sublime-snippet</code> extension. I&#8217;ve used <code>TryCatch_Wrap.sublime-snippet</code>.
  </p>

  <h2>
    3. Bind to a Shortcut
  </h2>

  <p>
    Go to the <strong>Sublime Text > Preferences > Key-Bindings &#8211; User</strong> menu. A <code>.sublime-keymap</code> file will open, mine was:
  </p>

  <pre class="lang:default decode:true">[
	{ "keys": ["alt+shift+j"], "command": "js_coffee", "args":{"new_file": true}}
]</pre>

  <p>
    In case (like me) you already have a line, add a comma and the following:
  </p>

  <pre class="lang:default decode:true">[
	{ "keys": ["alt+shift+j"], "command": "js_coffee", "args":{"new_file": true}},
	{ "keys": ["ctrl+super+t"], "command": "insert_snippet", "args": {"name": "Packages/User/TryCatch_Wrap.sublime-snippet"} }
]</pre>

  <p>
    As I&#8217;ve written it, the Snippet is bound to <code>Command+Control+t</code>, but you can use the combination you like the most (have a look at <strong>Key-Bindings &#8211; Default</strong> to check whether you&#8217;re overriding some shortcut already in use). If you&#8217;re wondering, accepted modifiers are:
  </p>

  <ul>
    <li>
      shift
    </li>
    <li>
      ctrl
    </li>
    <li>
      alt
    </li>
    <li>
      super (Windows key, Command key)
    </li>
  </ul>

  <p>
    Be sure to refer to the same <code>.sublime-snippet</code> filename, save and close the keymap.
  </p>

  <h2>
    4. Enjoy!
  </h2>

  <p>
    You&#8217;ve now built <span itemprop="about">a Sublime Text Snippet for CoffeeScript that wraps and indent selected code in a try/catch block</span>. You can select a portion of your code, hit ctrl+command+t and voilà! Hope this helps, you coffee fellows out there.
  </p>
</div>
