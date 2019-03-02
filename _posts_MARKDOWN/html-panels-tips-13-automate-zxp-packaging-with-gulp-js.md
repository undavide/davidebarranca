---
title: " HTML Panels Tips: #13 Automate ZXP Packaging with Gulp.js\t\t"
tags:
  - build tool
  - CC Extension
  - gulp
  - task runner
  - ZXP
url: 2695.html
id: 2695
category:
  - Coding
  - HTML Panels
date: 2014-08-12 23:33:57
---

As a [HTML Panels Tips: #10 Packaging / ZXP Installers](http://localhost:8888/2014/05/html-panels-tips-10-packaging-zxp-installers/ "HTML Panels Tips: #10 Packaging / ZXP Installers") follow-up, in this tip I will show you how to automate (i.e. make less error prone) the utterly annoying job of building ZXP installers: i.e. with the help of [Gulp.js](http://gulpjs.com "Gulp.js"), a task runner based on Node.js.

Gulp.js
-------

If you come from Photoshop scripting like me, you might be pretty green when it comes to the plethora of tools that web developers are used to. Since they often need to perform repetitive tasks (such as compile LESS/SASS stylesheets, minify CSS, lint, uglify and concatenate JS, rename and move files, etc.) some tools known as build tools or **task runners** appeared - a couple of remarkable ones being [Grunt](http://gruntjs.com "Grunt.js") and [Gulp](http://gulpjs.com "Gulp.js"). They do basically the same thing: Grunt is based on configuration, Gulp is (newer, and) code based. If the previous sentence makes no sense to you it doesn't matter too much - if you want to learn more about task runners I strongly suggest you to watch [this screencast](http://build-podcast.com/gulp/ "Gulp on Build Podcast") made by the great and prolific [@sayanee_](https://twitter.com/sayanee_ "Sayanee on Twitter") from the [Build Podcast](http://build-podcast.com "Build Podcasts") series (**60 free videos** about all kind of tech tools). Amazing resource, really. I won't dig deeper on Gulp here (also because I'm not a task runner expert myself), I will just share the particular code I'm using and that proved to work, explaining what each part does and trying to show a glimpse of the big picture. Hopefully, my workflow is simple enough for you to tweak it to fit your own needs.

Requisites
----------

Gulp - like tons of useful stuff - is based on **Node.js** (a platform based on Google Chrome runtimes), so you'd better install it first downloading from the [Node website](http://nodejs.org "Node.js") if you haven't already done that. Then, you need to know that there are about _88 thousands_ packages extending Node - which are managed by [npm](http://npmjs.org "Node Package Manager"), the **Node Package Manager.** Npm is automatically installed alongside Node.js. You must have Gulp installed _globally_ (i.e. not only in your project directory but available everywhere), so fire the Terminal and:

sudo npm install -g gulp

Enter your user password when requested (`sudo` is a mandatory step on OSX as far as I know - I can't say if it's needed on Windows shell too).

Folders' structure
------------------

![Folder structure](http://localhost:8888/wp-content/uploads/2014/08/gulp-dirs.png) I will package a hybrid extension - the very same I showed in [HTML Panels Tips: #10 Packaging / ZXP Installers](http://localhost:8888/2014/05/html-panels-tips-10-packaging-zxp-installers/ "HTML Panels Tips: #10 Packaging / ZXP Installers"). That is, an installer that deploys an HTML Panel, a Mac and Windows specific plugin files and a shared Photoshop script. Please refer to the original post for information about the manual process - here I'll just automate it. The folder hierarchy is as follows:

*   `src/` contains all the source code; `build/` is empty and will contain just the final installer.
*   The source for the HTML panel is the `src/com.example.myExtension` folder.
*   The `src/MXI` folder gathers all the assets needed to pack the hybrid extension:
    *   `src/MXI/com.example.myExtension.mxi` is the configuration file ([info here](http://localhost:8888/2014/05/html-panels-tips-10-packaging-zxp-installers/ "HTML Panels Tips: #10 Packaging / ZXP Installers")).
    *   `src/MXI/HTML` is empty but will contain the ZXP of the HTML panel.
    *   `src/MXI/MAC`, `src/MXI/WIN` and `src/MXI/SCRIPT` are the plugins and scripts needed by the extension.
*   Finally, `ucf.jar`, `ZXPSignCmd` and `myCertificate.p12` are the command line tools and the signing certificate that you should well know about.

I've left alone the `package.json` and `gulpfile.js` files, which I will disclose in a moment.

Setting up the project
----------------------

You can either create yourself the `package.json` file with the following content:

{
  "name": "Gulp-package-demo",
  "version": "0.1.0",
  "description": "CC Extension packaging with Gulp - DEMO",
  "author": "Davide Barranca",
  "license": "MIT"
}

Or let npm do it for you: `cd` into the project directory (the root), and

npm init

The Node Package Manager will initialise a project asking you few questions; as follows my answers (when there's none, I just hit enter to confirm the suggested defaults):

This utility will walk you through creating a package.json file.
It only covers the most common items, and tries to guess sane defaults.

See \`npm help json\` for definitive documentation on these fields
and exactly what they do.

Use \`npm install <pkg> --save\` afterwards to install a package and
save it as a dependency in the package.json file.

Press ^C at any time to quit.
name: (Gulp) Gulp-package-demo
version: (0.0.0) 0.1.0
description: CC Extension packaging with Gulp - DEMO
entry point: (index.js) 
test command: 
git repository: 
keywords: 
author: Davide Barranca
license: (BSD-2-Clause) MIT
About to write to /Users/Davide/Google Drive/PACK/Gulp/package.json:

{
  "name": "Gulp-package-demo",
  "version": "0.1.0",
  "description": "CC Extension packaging with Gulp - DEMO",
  "main": "index.js",
  "scripts": {
    "test": "echo \\"Error: no test specified\\" && exit 1"
  },
  "author": "Davide Barranca",
  "license": "MIT"
}


Is this ok? (yes)

One way or the other, a `package.json` file should be there. Now let's install (_locally_ \- i.e. in this very folder) gulp:

npm install --save-dev gulp

We need two gulp plugins: [gulp-clean](https://www.npmjs.org/package/gulp-clean "gulp-clean") is for deleting files:

npm install --save-dev gulp-clean

while [gulp-shell](https://www.npmjs.org/package/gulp-shell "gulp-shell") is a command line interface:

npm install --save-dev gulp-shell

The `--save-dev` flag instructs npm to add these ones as project's dependencies, automatically modifying the json file for you:

{
  "name": "Gulp-package-demo",
  "version": "0.1.0",
  "description": "CC Extension packaging with Gulp - DEMO",
  "author": "Davide Barranca",
  "license": "MIT",
  "devDependencies": {
    "gulp": "~3.8.7",
    "gulp-shell": "~0.2.9",
    "gulp-clean": "~0.3.1"
  }
}

You'll see that npm has installed these dependencies in a `node_modules` folder.

gulpfile.js
-----------

Here comes the fun: the `gulpfile.js` is where you start defining tasks, that you'll be calling from the Terminal. Before digging into the actual, entire workflow, let's familiarize with **tasks** \- here's a simple one:

var gulp = require('gulp'); 
var clean = require('gulp-clean');

// clean all ZXP files found everywhere
gulp.task('clean-all', function () {
	return gulp.src('./**/*.zxp', { read: false })
	.pipe(clean());
});

You first `require` gulp, then call the `gulp.task` function. The first parameter is the task name as a string (here `'clean-all'`), the second is the function that will return a _Stream_ (if you feel inclined, find [here](https://github.com/substack/stream-handbook "Node.js Stream handbook") info about Streams). Don't worry too much about that: what you need to know is just that you usually define a gulp source (here every zxp file from any subdirectory) and you pipe that to the `clean` function - which deletes files. The `{ read: false }` is a configuration object that instructs gulp not to read them, to speed up the process. How do you call that task? In the Terminal (root folder):

gulp clean-all

And presto! all the ZXPs that might have been around are gone. Here is another useful task - it uses the `gulp-shell` plugin to call the `ZXPSignCmd` command line and actually pack files:

var shell = require('gulp-shell');

// pack the HTML panel using ZXPSignCmd
gulp.task('pack-html',  
	shell.task('./ZXPSignCmd -sign src/com.example.myExtension src/com.example.myExtension.zxp myCertificate.p12 myPassword -tsa https://timestamp.geotrust.com/tsa') // don't put the semicolon here or it will break!
);

If you run in the Terminal:

gulp pack-html

it is like if you had typed the whole, long ZXPSignCmd string yourself. Hopefully you start appreciating the power of task runners now.

Combining tasks...
------------------

A task can rely on other tasks - **dependencies** are defined into an array of tasks:

gulp.task('pack-html', \['clean-all'\]. function () {
	shell.task('./ZXPSignCmd -sign src/com.example.myExtension src/com.example.myExtension.zxp myCertificate.p12 myPassword -tsa https://timestamp.geotrust.com/tsa')
}

In the above example, the `'clean-all'` task is performed _first_, then the `'pack-html'` task runs. If you have multiple dependencies (that is: your task needs to run after _several_ other tasks) you can define them this way:

gulp.task('test-task', \['first-dep', 'second-dep'\], function () {
    //...
}

Mind you: while the `'test-task'` actually runs after `'first-dep'` and `'second-dep'`, you cannot be sure that `'first-dep'` and `'second-dep'` are executed in this very order! Dependencies are processed **asynchronously**: `'second-dep'` may finish before `'first-dep'`.

... synchronously!
------------------

But in the ZXP automation we need to be sure that a task is done _before_ the next one starts - i.e. the whole thing must be **synchronous**. There are few ways to trick gulp into this behaviour: returning a stream, defining a callback, using promises and - the easiest one and therefore my preferred - **chaining dependencies**:

*   Task #4 depends on Task #3
*   Task #3 depends on Task #2
*   Task #2 depends on Task #1

You run Task #4 and in fact the whole sequence is #1 > #2 > #3 > #4. There might be more elegant ways to get there, but this approach has the advantage of being easily testable: if the result isn't what you'd expect, just run Task #3 to test the gulpfile up to that point; if it's still broken, try Task #2, and so on, until you find the bugged task.

Complete gulpfile.js
--------------------

Given the folder structure I've outlined, the entire gulpfile is as follows:

var gulp = require('gulp'); 
var clean = require('gulp-clean');
var shell = require('gulp-shell');

// 0\. Clean all ZXPs found everywhere
gulp.task('clean-all', function () {
	return gulp.src('./**/*.zxp', { read: false })
	.pipe(clean());
});

// 1\. Build the HTML Panel ZXP in src/
gulp.task('pack-html',  
	shell.task('./ZXPSignCmd -sign src/com.example.myExtension src/com.example.myExtension.zxp myCertificate.p12 myPassword -tsa https://timestamp.geotrust.com/tsa')
);

// 2\. Copy the ZXP in src/ to src/MXI/HTML/
gulp.task('move-html-zxp', \['pack-html'\], function (){
	return gulp.src('./src/*.zxp', { base: './src/' })
	.pipe(gulp.dest('./src/MXI/HTML'));
});

// 3\. Delete the HTML Panel ZXP in src/
gulp.task('clean-html-zxp', \['move-html-zxp'\], function () {
	return gulp.src('./src/*.zxp', { read: false })
	.pipe(clean());
});

// 4\. Build the hybrid ZXP in src/
gulp.task('pack-hybrid-zxp', \['clean-html-zxp'\], 
	shell.task('java -jar ucf.jar -package -storetype PKCS12 -keystore ./myCertificate.p12 -storepass myPassword -tsa http://timestamp.entrust.net/TSS/JavaHttpTS com.example.myExtension.zxp -C "./src/MXI/" .')
);

// 5\. Copy the hybrid ZXP to build/
gulp.task('move-zxp-to-build', \['pack-hybrid-zxp'\], function () {
	return gulp.src('./*.zxp', { base: './' })
	.pipe(gulp.dest('build'));
});

// 6\. Delete the hybrid ZXP in src/
gulp.task('clean-hybrid-zxp', \['move-zxp-to-build'\], function () {
	return gulp.src('./*.zxp', { read: false })
	.pipe(clean());
});

// 7\. Define the default task
gulp.task('default', \['clean-all'\], function () {
	gulp.start('clean-hybrid-zxp');
});

Hopefully the code and the comments are self-explanatory. Few caveats that might save you some search time on Google:

*   `'./**/*.zxp'` is a way to mean: every zxp file in every subfolder of the root (being the root `'./'`).
*   For a reason that I don't know, a semicolon after the `shell.task()` breaks gulp.
*   `{ read: false }` speeds up the process when you don't need gulp to read files.
*   `{ base: './src/' }` (as used in task #2) tells gulp to use `./src/` as a base for the relative paths. If you omit it or set it to the root `'./'`, you'll end up with `/src/MXI/HTML/src/com.example.myExtension.zxp` (and extra `/src/`) and not `/src/MXI/HTML/com.example.myExtension.zxp` which is what you actually want.

This tip just scratches the surface of gulp.js as a task runner for CC Extensions - I hope to have time to dig deeper and share my findings again - feel free to add yours in the comments!