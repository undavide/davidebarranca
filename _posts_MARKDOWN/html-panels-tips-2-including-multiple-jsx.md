---
title: " HTML Panels Tips: #2 Including multiple JSX\t\t"
tags:
  - html panels
  - include
  - jsx
  - tip
url: 2444.html
id: 2444
category:
  - Coding
  - HTML Panels
date: 2014-01-30 23:54:23
---

A simple way to dynamically evaluate multiple JSX files in the Scripting context of Photoshop within HTML Panels. Back in the Flash world, I used to dynamically include external JSX files via:

$.evalFile("" + (File($.fileName).path) + "/libs/external.jsx");

Being `$.fileName` the current JSX file within the above line is written and the `File.path` call meaning the actual path. Alas, this doesn't seem to work anymore when HTML panels are involved - the `File($.fileName).path` once resulted to me, don't ask me why, as "33" -  which is not a bad approximation to the answer to the [Ultimate Question of Life, the Universe, and Everything](http://simple.wikipedia.org/wiki/42_(answer) "42") after all ;-) Anyway, what I personally use now is the following function (in the `main.js` file):

// fileName is a String (with the .jsx extension included)
function loadJSX(fileName) {
    var csInterface = new CSInterface();
    var extensionRoot = csInterface.getSystemPath(SystemPath.EXTENSION) + "/jsx/";
    csInterface.evalScript('$.evalFile("' + extensionRoot + fileName + '")');
}

The key here is the above highlighted line, which is the JS equivalent of the JSX line I've been using. To load a JSX - which must belong to the root/jsx folder since it's hardwired in the function, you would write:

loadJSX("myFile.jsx");

Borrowing code from the Extension Builder 3 boilerplate you can get fancier. The first chunk goes in the main JSX, i.e. the one listed in the `manifest.xml`, enclosed in the `ScriptPath` tag:

if(typeof($)=='undefined')
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
            for (var i = 0; i < jsxFiles.length; i++) {
                var jsxFile = jsxFiles\[i\];
                $._ext.evalFile(jsxFile);
            }
        }
    }
};

This instead is what goes in the `main.js`

/\*\*
 \* Load JSX file into the scripting context of the product. All the jsx files in 
 \* folder \[ExtensionRoot\]/jsx will be loaded. 
 */
function loadJSX() {
    var csInterface = new CSInterface();
    var extensionRoot = csInterface.getSystemPath(SystemPath.EXTENSION) + "/jsx/";
    csInterface.evalScript('$._ext.evalFiles("' + extensionRoot + '")');
}

`loadJSX()` (for instance called in the `init()` function) will evaluate all the JSX files in the predefined folder.

### Bonus - System Paths

You may be interested in these ones too (from CSInterface-4.0.0.js)

/\*\* Identifies the path to user data.  */	
SystemPath.USER_DATA = "userData";

/\*\* Identifies the path to common files for Adobe applications.  */	
SystemPath.COMMON_FILES = "commonFiles";			

/\*\* Identifies the path to the user's default document folder.  */
SystemPath.MY_DOCUMENTS = "myDocuments";

/\*\* Identifies the path to current extension.  */
/\*\* DEPRECATED, PLEASE USE EXTENSION INSTEAD.  */	
SystemPath.APPLICATION = "application";

/\*\* Identifies the path to current extension.  */	
SystemPath.EXTENSION = "extension";

/\*\* Identifies the path to hosting application's executable.  */	
SystemPath.HOST_APPLICATION = "hostApplication";