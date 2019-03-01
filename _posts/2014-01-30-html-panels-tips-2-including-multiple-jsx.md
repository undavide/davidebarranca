---
id: 2444
title: 'HTML Panels Tips: #2 Including multiple JSX'
date: 2014-01-30T23:54:23+01:00
author: Davide Barranca
excerpt: A simple way to dynamically evaluate multiple JSX files in the Scripting context of Photoshop within HTML Panels.
layout: post
guid: http://www.davidebarranca.com/?p=2444
permalink: /2014/01/html-panels-tips-2-including-multiple-jsx/
standard_seo_post_level_layout:
  - ""
standard_link_url_field:
  - ""
standard_seo_post_meta_description:
  - A simple way to dynamically evaluate multiple JSX files in the Scripting context of Photoshop within HTML Panels.
seo_post_level_layout:
  - ""
link_url_field:
  - ""
seo_post_meta_description:
  - ""
image: /wp-content/uploads/2014/01/jsx.jpg
categories:
  - Coding
  - HTML Panels
tags:
  - html panels
  - include
  - jsx
  - tip
---
<div class="pf-content">
  <p>
    A simple way to dynamically evaluate multiple JSX files in the Scripting context of Photoshop within HTML Panels.
  </p>
  
  <p>
    <!--more-->
    
    <br /> Back in the Flash world, I used to dynamically include external JSX files via:
  </p>
  
  <pre class="lang:js decode:true">$.evalFile("" + (File($.fileName).path) + "/libs/external.jsx");</pre>
  
  <p>
    Being <code>$.fileName</code> the current JSX file within the above line is written and the <code>File.path</code> call meaning the actual path. Alas, this doesn&#8217;t seem to work anymore when HTML panels are involved &#8211; the <code>File($.fileName).path</code> once resulted to me, don&#8217;t ask me why, as &#8220;33&#8221; &#8211;  which is not a bad approximation to the answer to the <a title="42" href="http://simple.wikipedia.org/wiki/42_(answer)" target="_blank">Ultimate Question of Life, the Universe, and Everything</a> after all 😉
  </p>
  
  <p>
    Anyway, what I personally use now is the following function (in the <code>main.js</code> file):
  </p>
  
  <pre class="lang:js mark:4 decode:true">// fileName is a String (with the .jsx extension included)
function loadJSX(fileName) {
    var csInterface = new CSInterface();
    var extensionRoot = csInterface.getSystemPath(SystemPath.EXTENSION) + "/jsx/";
    csInterface.evalScript('$.evalFile("' + extensionRoot + fileName + '")');
}</pre>
  
  <p>
    The key here is the above highlighted line, which is the JS equivalent of the JSX line I&#8217;ve been using. To load a JSX &#8211; which must belong to the root/jsx folder since it&#8217;s hardwired in the function, you would write:
  </p>
  
  <pre class="lang:default decode:true">loadJSX("myFile.jsx");</pre>
  
  <p>
    Borrowing code from the Extension Builder 3 boilerplate you can get fancier. The first chunk goes in the main JSX, i.e. the one listed in the <code>manifest.xml</code>, enclosed in the <code>ScriptPath</code> tag:
  </p>
  
  <pre class="lang:js decode:true" title="In the JSX">if(typeof($)=='undefined')
	$={};

$._ext = {
    //Evaluate a file and catch the exception.
    evalFile : function(path) {
        try {
            $.evalFile(path);
        } catch (e) {alert("Exception:" + e);}
    },
    // Evaluate all the files in the given folder 
    evalFiles: function(jsxFolderPath) {
        var folder = new Folder(jsxFolderPath);
        if (folder.exists) {
            var jsxFiles = folder.getFiles("*.jsx");
            for (var i = 0; i &lt; jsxFiles.length; i++) {
                var jsxFile = jsxFiles[i];
                $._ext.evalFile(jsxFile);
            }
        }
    }
};</pre>
  
  <p>
    This instead is what goes in the <code>main.js</code>
  </p>
  
  <pre class="lang:js decode:true" title="In the main.js ">/**
 * Load JSX file into the scripting context of the product. All the jsx files in 
 * folder [ExtensionRoot]/jsx will be loaded. 
 */
function loadJSX() {
    var csInterface = new CSInterface();
    var extensionRoot = csInterface.getSystemPath(SystemPath.EXTENSION) + "/jsx/";
    csInterface.evalScript('$._ext.evalFiles("' + extensionRoot + '")');
}</pre>
  
  <p>
    <code>loadJSX()</code> (for instance called in the <code>init()</code> function) will evaluate all the JSX files in the predefined folder.
  </p>
  
  <h3>
    Bonus &#8211; System Paths
  </h3>
  
  <p>
    You may be interested in these ones too (from CSInterface-4.0.0.js)
  </p>
  
  <pre class="lang:js decode:true ">/** Identifies the path to user data.  */	
SystemPath.USER_DATA = "userData";

/** Identifies the path to common files for Adobe applications.  */	
SystemPath.COMMON_FILES = "commonFiles";			

/** Identifies the path to the user's default document folder.  */
SystemPath.MY_DOCUMENTS = "myDocuments";

/** Identifies the path to current extension.  */
/** DEPRECATED, PLEASE USE EXTENSION INSTEAD.  */	
SystemPath.APPLICATION = "application";

/** Identifies the path to current extension.  */	
SystemPath.EXTENSION = "extension";

/** Identifies the path to hosting application's executable.  */	
SystemPath.HOST_APPLICATION = "hostApplication";</pre>
  
  <p>
    &nbsp;
  </p>
</div>

<!-- Share-Widget Button BEGIN --><a href="javascript:void(0);" myshare\_id="mys\_shareit" myshare\_url="http://localhost:8888/2014/01/html-panels-tips-2-including-multiple-jsx/" myshare\_title="HTML Panels Tips: #2 Including multiple JSX" rel="nofollow" onclick=" return false;" style="text-decoration:none; color:#000000; font-size:11px; line-height:20px;"> 

<img src="http://localhost:8888/wp-content/plugins/share-widget/img/share-button-white-small.png" height="20" alt="Share" style="border:0" /> </a> <!-- Share-Widget Button END -->