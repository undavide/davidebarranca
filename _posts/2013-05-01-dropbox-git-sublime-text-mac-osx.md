---
id: 1968
title: DropBox, Git and SublimeText on the Mac
date: 2013-05-01T13:25:30+01:00
author: Davide Barranca
excerpt: "As a freelance developer I've been using my DropBox account as the projects cloud backup - yet if you're willing to setup it as a private GitHub repository for version control, possibly in conjunction with a SublimeText Git plugin... chances are that you'll spend some time looking for issue workarounds. Why bother, I've gathered them here for you!"
layout: post
guid: http://www.davidebarranca.com/?p=1968
permalink: /2013/05/dropbox-git-sublime-text-mac-osx/
standard_seo_post_level_layout:
  - ""
standard_link_url_field:
  - ""
standard_seo_post_meta_description:
  - How to setup a local Git repository, sync it with a private DropBox repository for backup, and use the Sublime Text Git plugin to manage it.
image: /wp-content/uploads/2013/05/Git_DropBox_SublimeText.jpg
category:
  - Coding
tags:
  - DropBox
  - Git
  - repository
  - Sublime Text
---
<div class="pf-content">
  <p>
    <span itemprop="description">This post will describe how to setup a local Git repository, sync it with a private DropBox repository for cloud backup (as it were a GitHub hosted one) and use the Sublime Text Git plugin to manage it.</span>
  </p>

  <h2>
    Get ready with Git
  </h2>

  <p>
    Download the latest <span itemprop="about" itempscope itemtype="http://schema.org/SoftwareApplication"><span itemprop="name">Git</span> version from the <a title="Git download" href="http://git-scm.com/downloads" target="_blank" itemprop="url">official website</a></span>. To date, the latest package available is called <code>git-1.8.2.1-intel-universal-snow-leopard.dmg</code> (I&#8217;m running it successfully on OSX 10.8.3 Mountain Lion). Launch the installer, and when it&#8217;s done open the <strong>Terminal</strong> (Applications/Utilities/Terminal.app) and type <code>which git</code>
  </p>

  <p>
    <img class="aligncenter size-full wp-image-1970" alt="which git" src="/wp-content/uploads/2013/04/which_git.png" width="523" height="146" srcset="/wp-content/uploads/2013/04/which_git.png 523w, /wp-content/uploads/2013/04/which_git-150x41.png 150w, /wp-content/uploads/2013/04/which_git-300x83.png 300w" sizes="(max-width: 523px) 100vw, 523px" />
  </p>

  <p>
    This is what I got, meaning that the path for the git executable is <code>/usr/local/bin/git</code> (or whatever it is on your machine) which is something you&#8217;ve to remember.
  </p>

  <p>
    <a href="http://mac.github.com" target="_blank"><img class="alignleft size-full wp-image-1971" alt="GitHub app" src="/wp-content/uploads/2013/04/github.jpg" width="200" height="200" srcset="/wp-content/uploads/2013/04/github.jpg 200w, /wp-content/uploads/2013/04/github-150x150.jpg 150w" sizes="(max-width: 200px) 100vw, 200px" /></a>If you want (I do), you can download as well a <strong>GUI Client</strong> &#8211; providing you with a nice graphic interface. There are plenty of them, I&#8217;ll stick with the free original <span itemprop="mentions" itemscope itemtype="http://schema.org/SoftwareApplication"><a title="GitHub app for Mac" href="http://mac.github.com" target="_blank" itemprop="url"><span itemprop="name">GitHub</span> app</a></span> for the Mac.
  </p>

  <p>
    Depending on your skills (I&#8217;m not a very seasoned Git user, to put it mildly) and preferences, you&#8217;ll be able to drive your version control system either via command-line and/or visually through the GitHub app.
  </p>

  <h2>
    Get ready with Sublime Text
  </h2>

  <p>
    <span itemprop="about" itemscope itemtype="http://schema.org/SoftwareApplication"><a title="Sublime Text official website" href="http://www.sublimetext.com" target="_blank" itemprop="url"><span itemprop="name">Sublime Text</span></a></span> is a really powerful text editor (TextMate is another one I&#8217;d suggest): among the plethora of available plugins, install the free <a title="Sublime Package Control" href="http://wbond.net/sublime_packages/package_control" target="_blank">Sublime Package Control</a> first and you&#8217;ll never regret it (installation instruction <a title="Installation " href="http://wbond.net/sublime_packages/package_control/installation" target="_blank">here</a>). It will help you look for, and install, all the (other) plugins you may ever need.
  </p>

  <p>
    Then, in Sublime Text do the usual ⌘⇧P to open the <strong>Command Palette</strong> and start typing &#8220;install&#8221; &#8211; as you type, the actual command will appear, which is <code>Package Control: Install Package</code>. Then look for <code>Git</code> and Enter to install it &#8211; visit also at the <a title="Sublime Text Git plugin" href="https://github.com/kemayo/sublime-text-2-git/wiki" target="_blank">plugin official page</a> for more information.
  </p>

  <p>
    So far so good &#8211; yet the Git plugin isn&#8217;t working yet because Sublime Text needs to know the Git&#8217;s <strong>path</strong> (the one you&#8217;ve found before). Open with a text editor of your choice (guess which one!) the file called <code>Git.sublime-settings</code> found here:
  </p>

  <p>
    <code>/Users/&lt;yourUser&gt;/Library/Application Support/Sublime Text 2/Packages/Git/</code>
  </p>

  <p>
    And look for the following lines:
  </p>

  <pre class="theme:twilight font-size:14 line-height:18 toolbar:2 striped:false whitespace-before:1 whitespace-after:1 lang:js decode:true">  // if present, use this command instead of plain "git"
  // e.g. "/Users/kemayo/bin/git" or "C:\bin\git.exe"
  ,"git_command": "/usr/local/git/bin/git"</pre>

  <p>
    Of course be careful to type the correct path for the <code>git_command</code>, (recall what was the output of the <code>which git</code> command) then save and close (this issue is covered <a title="Git plugin issue" href="https://github.com/kemayo/sublime-text-2-git/issues/96" target="_blank">here</a>).
  </p>

  <h2>
    Set a DropBox &#8220;remote&#8221; repository
  </h2>

  <p>
    One thing you could do (I did it &#8211; but it&#8217;s not what I&#8217;ll be suggesting) is to create a Local Repository in the <span itemprop="about" itemscope itemtype="http://schema.org/SoftwareApplication"><a title="Dropbox" href="http://www.dropbox.com" target="_blank" itemprop="url"><span itemprop="name">Dropbox</span></a></span> folder on your HD: here both your files and the <span itemprop="mentions">repository</span> are backed-up on DropBox servers. Unfortunately, this way you&#8217;re forced to keep all your dev stuff within the local DropBox folder &#8211; i.e. all your files &#8211; and this is not what you want.
  </p>

  <p>
    As a more efficient alternative, you&#8217;ll keep a <strong>local repository</strong> in your usual development folder, then <strong>push it to a DropBox repository</strong> as it were a GitHub hosted one.
  </p>

  <p>
    Let&#8217;s assume that your DropBox folder is <code>~/Dropbox</code> and the folder where you do your development is <code>~/dev/photoshop</code>. You&#8217;re going to work on a job called <code>psProject01</code>. Open the Terminal app.
  </p>

  <h3>
    <span style="line-height: 13px;">1. Create the DropBox repo</span>
  </h3>

  <pre class="theme:twilight font-size:14 line-height:18 toolbar:2 striped:false nums:false whitespace-before:1 whitespace-after:1 lang:default highlight:0 decode:true">$ cd /users/Davide/Dropbox
$ mkdir psProject01.git
$ cd psProject01.git
$ git init --bare</pre>

  <p>
    You&#8217;ve made and initialized a <a title="Bare repositories" href="http://www.gitguys.com/topics/shared-repositories-should-be-bare-repositories/" target="_blank">bare</a> psProject01.git repository in the DropBox folder.
  </p>

  <h3>
    2. Create the Dev repo
  </h3>

  <p>
    This is on your usual development folder.
  </p>

  <pre class="theme:twilight font-size:14 line-height:18 toolbar:2 striped:false nums:false whitespace-before:1 whitespace-after:1 lang:default highlight:0 decode:true">$ cd /users/Davide/dev/photoshop
$ mkdir psProject01
$ cd psProject01
$ git init</pre>

  <p>
    As a result, a hidden <code>.git</code> folder is created inside <code>psProject01</code>. Now there are two empty repos, one on the DropBox folder, one on the dev folder.
  </p>

  <h3>
    3. Link the repositories
  </h3>

  <p>
    The following command will link the DropBox repository to the dev one (you&#8217;re still in <code>/users/Davide/Dev/photoshop/psProject01</code>)
  </p>

  <pre class="theme:twilight font-size:14 line-height:18 toolbar:2 striped:false nums:false whitespace-before:1 whitespace-after:1 lang:default highlight:0 decode:true">$ git remote add origin /users/Davide/Dropbox/psProject01.git</pre>

  <p>
    Someone is using a different syntax, but apparently the former one works too:
  </p>

  <pre class="theme:twilight font-size:14 line-height:18 toolbar:2 striped:false nums:false whitespace-before:1 whitespace-after:1 lang:default highlight:0 decode:true">$ git remote add origin file:///users/Davide/Dropbox/psProject01.git</pre>

  <h3>
    4. Add files to the Dev repo, Commit and Push
  </h3>

  <p>
    For some reason, I haven&#8217;t been able to use the Sublime Text Git plugin to <code>Git: Add Current File</code> (i.e. create a new file in Sublime Text and add it to the repo, I get this error: &#8220;git branch: fatal: ambituous argument head unknow revision or path not in the working tree&#8221;) at least until a first file has been added via command line, the change committed and the repository pushed:
  </p>

  <pre class="theme:twilight font-size:14 line-height:18 toolbar:2 striped:false nums:false whitespace-before:1 whitespace-after:1 lang:default highlight:0 decode:true">$ touch lib.jsx
$ git add lib.jsx
$ git commit -m "Initial commit, lib.jsx added"
$ git push origin master
  Counting objects: 3, done.
  Writing objects: 100% (3/3), 225 bytes, done.
  Total 3 (delta 0), reused 0 (delta 0)
  To /Users/Davide/Dropbox/repo.git
  * [new branch] master -&gt; master</pre>

  <p>
    The above creates a <code>lib.jsx</code> file and adds it to the dev repository. Then the change is <strong>committed</strong>, and the repo is <strong>pushed</strong> to the DropBox &#8220;remote&#8221; repository (which is on the HD, but it&#8217;s the same command you&#8217;d write for an actual remotely hosted repo).
  </p>

  <p>
    Mind you, if you look into the <code>/users/Davide/Dropbox/psProject01.git</code> folder you won&#8217;t see any <code>lib.jsx</code> file: <strong>the Dropbox repository only contains the Git&#8217;s guts</strong>. If you want to double-check that everything went fine (and the file is actually stored in the Git database), you can clone the repo to a different location, for instance:
  </p>

  <pre class="theme:twilight font-size:14 line-height:18 toolbar:2 striped:false nums:false whitespace-before:1 whitespace-after:1 lang:default highlight:0 decode:true">$ cd /users/Davide/dev/
$ git clone /users/Davide/Dropbox/psProject01.git testRepository
  Cloning into 'testRepository'...
  done.</pre>

  <p>
    Now if you inspect the <code>/users/Davide/dev/testRepository</code> folder you&#8217;ll find the <code>lib.jsx</code> file there! Does it make sense? Articles I&#8217;ve used as a reference for these commands are found <a title="Creating private Git repository" href="http://blog.maurizioturatti.com/2012/11/creating-your-private-git-repository-on.html" target="_blank">here</a> and <a title="Using dropbox as a private GitHub" href="http://jetheis.com/blog/2013/02/17/using-dropbox-as-a-private-github/" target="_blank">here</a>.
  </p>

  <h2>
    Using Sublime Text Git plugin
  </h2>

  <p>
    Now start doing something with your newly created <code>lib.jsx</code> file, like:
  </p>

  <pre class="theme:twilight font-size:14 line-height:18 toolbar:2 whitespace-before:1 whitespace-after:1 lang:js decode:true">// Test file
#target photoshop

alert("Hello World");</pre>

  <p>
    and save.
  </p>

  <h3>
    Commit
  </h3>

  <p>
    You can <strong>commit</strong> with ⌘⇧P to open the Command Palette and then typing <code>Git: Quick Commit</code>:
  </p>

  <p>
    <img class="aligncenter size-full wp-image-1999" alt="git_quick_commit_01" src="/wp-content/uploads/2013/05/git_quick_commit_01.png" width="570" height="277" srcset="/wp-content/uploads/2013/05/git_quick_commit_01.png 570w, /wp-content/uploads/2013/05/git_quick_commit_01-150x72.png 150w, /wp-content/uploads/2013/05/git_quick_commit_01-300x145.png 300w" sizes="(max-width: 570px) 100vw, 570px" />
  </p>

  <p>
    Then enter a short description:
  </p>

  <p>
    <img class="aligncenter size-full wp-image-1997" alt="git_quick_commit_02" src="/wp-content/uploads/2013/05/git_quick_commit_02.png" width="593" height="276" srcset="/wp-content/uploads/2013/05/git_quick_commit_02.png 593w, /wp-content/uploads/2013/05/git_quick_commit_02-150x69.png 150w, /wp-content/uploads/2013/05/git_quick_commit_02-300x139.png 300w" sizes="(max-width: 593px) 100vw, 593px" />
  </p>

  <p>
    And the following is the correct output:
  </p>

  <p>
    <img class="aligncenter size-full wp-image-1998" alt="git_quick_commit_03" src="/wp-content/uploads/2013/05/git_quick_commit_03.png" width="673" height="308" srcset="/wp-content/uploads/2013/05/git_quick_commit_03.png 673w, /wp-content/uploads/2013/05/git_quick_commit_03-150x68.png 150w, /wp-content/uploads/2013/05/git_quick_commit_03-300x137.png 300w" sizes="(max-width: 673px) 100vw, 673px" />
  </p>

  <h3>
    Log and Diff
  </h3>

  <p>
    ⌘⇧P and then <code>Git: Log Current File</code> shows you the commit history:
  </p>

  <p>
    <img class="aligncenter size-full wp-image-2003" alt="git_log_01" src="/wp-content/uploads/2013/05/git_log_01.png" width="620" height="240" srcset="/wp-content/uploads/2013/05/git_log_01.png 620w, /wp-content/uploads/2013/05/git_log_01-150x58.png 150w, /wp-content/uploads/2013/05/git_log_01-300x116.png 300w" sizes="(max-width: 620px) 100vw, 620px" />
  </p>

  <p>
    Choosing one item shows you the details:
  </p>

  <p>
    <img class="aligncenter size-full wp-image-2002" alt="git_log_02" src="/wp-content/uploads/2013/05/git_log_02.png" width="811" height="555" itemprop="image" srcset="/wp-content/uploads/2013/05/git_log_02.png 811w, /wp-content/uploads/2013/05/git_log_02-150x102.png 150w, /wp-content/uploads/2013/05/git_log_02-300x205.png 300w" sizes="(max-width: 811px) 100vw, 811px" />
  </p>

  <h3>
    Push
  </h3>

  <p>
    ⌘⇧P and then <code>Git: Push Current Branch</code>:
  </p>

  <p>
    <img class="aligncenter size-full wp-image-2004" alt="git_push" src="/wp-content/uploads/2013/05/git_push.png" width="699" height="317" srcset="/wp-content/uploads/2013/05/git_push.png 699w, /wp-content/uploads/2013/05/git_push-150x68.png 150w, /wp-content/uploads/2013/05/git_push-300x136.png 300w" sizes="(max-width: 699px) 100vw, 699px" />
  </p>

  <p>
    A list of <strong>supported commands</strong> is found in the <a title="Git plugin" href="https://github.com/kemayo/sublime-text-2-git/wiki" target="_blank">Sublime Text Git plugin page</a>.
  </p>

  <h2>
    Using the GitHub app
  </h2>

  <p>
    If you feel more comfortable with a graphic user interface, start the GitHub application you&#8217;ve downloaded before and add a <strong>Local Repository</strong> (pointing to the <strong>dev folder</strong>) with the little plus button in the bottom bar:
  </p>

  <p>
    <img class="aligncenter size-full wp-image-2007" alt="GitHub_01" src="/wp-content/uploads/2013/05/GitHub_01.png" width="791" height="349" srcset="/wp-content/uploads/2013/05/GitHub_01.png 791w, /wp-content/uploads/2013/05/GitHub_01-150x66.png 150w, /wp-content/uploads/2013/05/GitHub_01-300x132.png 300w" sizes="(max-width: 791px) 100vw, 791px" />
  </p>

  <p>
    Double click on the psProject repository to see the History list:
  </p>

  <p>
    <img class="aligncenter size-full wp-image-2006" alt="GitHub_02" src="/wp-content/uploads/2013/05/GitHub_02.png" width="819" height="419" srcset="/wp-content/uploads/2013/05/GitHub_02.png 819w, /wp-content/uploads/2013/05/GitHub_02-150x76.png 150w, /wp-content/uploads/2013/05/GitHub_02-300x153.png 300w" sizes="(max-width: 819px) 100vw, 819px" />
  </p>

  <p>
    And eventually double click on a single item to view the commit&#8217;s details:
  </p>

  <p>
    <img class="aligncenter size-full wp-image-2005" alt="GitHub_03" src="/wp-content/uploads/2013/05/GitHub_03.png" width="819" height="419" srcset="/wp-content/uploads/2013/05/GitHub_03.png 819w, /wp-content/uploads/2013/05/GitHub_03-150x76.png 150w, /wp-content/uploads/2013/05/GitHub_03-300x153.png 300w" sizes="(max-width: 819px) 100vw, 819px" />
  </p>

  <p>
    From the app you can Publish/Sync the branch, add new files, etc &#8211; the very same things Git command line allows you. I&#8217;m still finding my way around all the options, so as a very basic user I won&#8217;t suggest you anything but the initial setup.
  </p>

  <h2>
    Caveat
  </h2>

  <p>
    True, DropBox sync may let you run into troubles if you and someone else are<strong> pushing stuff at the very same time</strong>, but as a single developer this won&#8217;t be a problem. Be aware, just in case you use DropBox to share your Git repository to others.<br />

    <meta itemprop="image" content="/wp-content/uploads/2013/05/Git_DropBox_SublimeText.jpg" />
  </p>
</div>
