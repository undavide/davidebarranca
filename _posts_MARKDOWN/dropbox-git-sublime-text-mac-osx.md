---
title: " DropBox, Git and SublimeText on the Mac\t\t"
tags:
  - DropBox
  - Git
  - repository
  - Sublime Text
url: 1968.html
id: 1968
comments: false
category:
  - Coding
date: 2013-05-01 13:25:30
---

This post will describe how to setup a local Git repository, sync it with a private DropBox repository for cloud backup (as it were a GitHub hosted one) and use the Sublime Text Git plugin to manage it.

Get ready with Git
------------------

Download the latest Git version from the [official website](http://git-scm.com/downloads "Git download"). To date, the latest package available is called `git-1.8.2.1-intel-universal-snow-leopard.dmg` (I'm running it successfully on OSX 10.8.3 Mountain Lion). Launch the installer, and when it's done open the **Terminal** (Applications/Utilities/Terminal.app) and type `which git` ![which git](http://localhost:8888/wp-content/uploads/2013/04/which_git.png) This is what I got, meaning that the path for the git executable is `/usr/local/bin/git` (or whatever it is on your machine) which is something you've to remember. [![GitHub app](http://localhost:8888/wp-content/uploads/2013/04/github.jpg)](http://mac.github.com)If you want (I do), you can download as well a **GUI Client** \- providing you with a nice graphic interface. There are plenty of them, I'll stick with the free original [GitHub app](http://mac.github.com "GitHub app for Mac") for the Mac. Depending on your skills (I'm not a very seasoned Git user, to put it mildly) and preferences, you'll be able to drive your version control system either via command-line and/or visually through the GitHub app.

Get ready with Sublime Text
---------------------------

[Sublime Text](http://www.sublimetext.com "Sublime Text official website") is a really powerful text editor (TextMate is another one I'd suggest): among the plethora of available plugins, install the free [Sublime Package Control](http://wbond.net/sublime_packages/package_control "Sublime Package Control") first and you'll never regret it (installation instruction [here](http://wbond.net/sublime_packages/package_control/installation "Installation ")). It will help you look for, and install, all the (other) plugins you may ever need. Then, in Sublime Text do the usual ⌘⇧P to open the **Command Palette** and start typing "install" - as you type, the actual command will appear, which is `Package Control: Install Package`. Then look for `Git` and Enter to install it - visit also at the [plugin official page](https://github.com/kemayo/sublime-text-2-git/wiki "Sublime Text Git plugin") for more information. So far so good - yet the Git plugin isn't working yet because Sublime Text needs to know the Git's **path** (the one you've found before). Open with a text editor of your choice (guess which one!) the file called `Git.sublime-settings` found here: `/Users/<yourUser>/Library/Application Support/Sublime Text 2/Packages/Git/` And look for the following lines:

  // if present, use this command instead of plain "git"
  // e.g. "/Users/kemayo/bin/git" or "C:\\bin\\git.exe"
  ,"git_command": "/usr/local/git/bin/git"

Of course be careful to type the correct path for the `git_command`, (recall what was the output of the `which git` command) then save and close (this issue is covered [here](https://github.com/kemayo/sublime-text-2-git/issues/96 "Git plugin issue")).

Set a DropBox "remote" repository
---------------------------------

One thing you could do (I did it - but it's not what I'll be suggesting) is to create a Local Repository in the [Dropbox](http://www.dropbox.com "Dropbox") folder on your HD: here both your files and the repository are backed-up on DropBox servers. Unfortunately, this way you're forced to keep all your dev stuff within the local DropBox folder - i.e. all your files - and this is not what you want. As a more efficient alternative, you'll keep a **local repository** in your usual development folder, then **push it to a DropBox repository** as it were a GitHub hosted one. Let's assume that your DropBox folder is `~/Dropbox` and the folder where you do your development is `~/dev/photoshop`. You're going to work on a job called `psProject01`. Open the Terminal app.

### 1\. Create the DropBox repo

$ cd /users/Davide/Dropbox
$ mkdir psProject01.git
$ cd psProject01.git
$ git init --bare

You've made and initialized a [bare](http://www.gitguys.com/topics/shared-repositories-should-be-bare-repositories/ "Bare repositories") psProject01.git repository in the DropBox folder.

### 2\. Create the Dev repo

This is on your usual development folder.

$ cd /users/Davide/dev/photoshop
$ mkdir psProject01
$ cd psProject01
$ git init

As a result, a hidden `.git` folder is created inside `psProject01`. Now there are two empty repos, one on the DropBox folder, one on the dev folder.

### 3\. Link the repositories

The following command will link the DropBox repository to the dev one (you're still in `/users/Davide/Dev/photoshop/psProject01`)

$ git remote add origin /users/Davide/Dropbox/psProject01.git

Someone is using a different syntax, but apparently the former one works too:

$ git remote add origin file:///users/Davide/Dropbox/psProject01.git

### 4\. Add files to the Dev repo, Commit and Push

For some reason, I haven't been able to use the Sublime Text Git plugin to `Git: Add Current File` (i.e. create a new file in Sublime Text and add it to the repo, I get this error: "git branch: fatal: ambituous argument head unknow revision or path not in the working tree") at least until a first file has been added via command line, the change committed and the repository pushed:

$ touch lib.jsx
$ git add lib.jsx
$ git commit -m "Initial commit, lib.jsx added"
$ git push origin master
  Counting objects: 3, done.
  Writing objects: 100% (3/3), 225 bytes, done.
  Total 3 (delta 0), reused 0 (delta 0)
  To /Users/Davide/Dropbox/repo.git
  \* \[new branch\] master -> master

The above creates a `lib.jsx` file and adds it to the dev repository. Then the change is **committed**, and the repo is **pushed** to the DropBox "remote" repository (which is on the HD, but it's the same command you'd write for an actual remotely hosted repo). Mind you, if you look into the `/users/Davide/Dropbox/psProject01.git` folder you won't see any `lib.jsx` file: **the Dropbox repository only contains the Git's guts**. If you want to double-check that everything went fine (and the file is actually stored in the Git database), you can clone the repo to a different location, for instance:

$ cd /users/Davide/dev/
$ git clone /users/Davide/Dropbox/psProject01.git testRepository
  Cloning into 'testRepository'...
  done.

Now if you inspect the `/users/Davide/dev/testRepository` folder you'll find the `lib.jsx` file there! Does it make sense? Articles I've used as a reference for these commands are found [here](http://blog.maurizioturatti.com/2012/11/creating-your-private-git-repository-on.html "Creating private Git repository") and [here](http://jetheis.com/blog/2013/02/17/using-dropbox-as-a-private-github/ "Using dropbox as a private GitHub").

Using Sublime Text Git plugin
-----------------------------

Now start doing something with your newly created `lib.jsx` file, like:

// Test file
#target photoshop

alert("Hello World");

and save.

### Commit

You can **commit** with ⌘⇧P to open the Command Palette and then typing `Git: Quick Commit`: ![git_quick_commit_01](http://localhost:8888/wp-content/uploads/2013/05/git_quick_commit_01.png) Then enter a short description: ![git_quick_commit_02](http://localhost:8888/wp-content/uploads/2013/05/git_quick_commit_02.png) And the following is the correct output: ![git_quick_commit_03](http://localhost:8888/wp-content/uploads/2013/05/git_quick_commit_03.png)

### Log and Diff

⌘⇧P and then `Git: Log Current File` shows you the commit history: ![git_log_01](http://localhost:8888/wp-content/uploads/2013/05/git_log_01.png) Choosing one item shows you the details: ![git_log_02](http://localhost:8888/wp-content/uploads/2013/05/git_log_02.png)

### Push

⌘⇧P and then `Git: Push Current Branch`: ![git_push](http://localhost:8888/wp-content/uploads/2013/05/git_push.png) A list of **supported commands** is found in the [Sublime Text Git plugin page](https://github.com/kemayo/sublime-text-2-git/wiki "Git plugin").

Using the GitHub app
--------------------

If you feel more comfortable with a graphic user interface, start the GitHub application you've downloaded before and add a **Local Repository** (pointing to the **dev folder**) with the little plus button in the bottom bar: ![GitHub_01](http://localhost:8888/wp-content/uploads/2013/05/GitHub_01.png) Double click on the psProject repository to see the History list: ![GitHub_02](http://localhost:8888/wp-content/uploads/2013/05/GitHub_02.png) And eventually double click on a single item to view the commit's details: ![GitHub_03](http://localhost:8888/wp-content/uploads/2013/05/GitHub_03.png) From the app you can Publish/Sync the branch, add new files, etc - the very same things Git command line allows you. I'm still finding my way around all the options, so as a very basic user I won't suggest you anything but the initial setup.

Caveat
------

True, DropBox sync may let you run into troubles if you and someone else are **pushing stuff at the very same time**, but as a single developer this won't be a problem. Be aware, just in case you use DropBox to share your Git repository to others.