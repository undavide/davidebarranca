---
title: " Version Control in Flash Builder: installation and setup\t\t"
tags:
  - Flash Builder
  - Subclipse
  - Subversion
  - svn
  - Version Control
url: 543.html
id: 543
comments: false
category:
  - Coding
date: 2011-12-21 23:19:55
---

![Folders](http://localhost:8888/wp-content/uploads/2011/12/Folders.png)"_Save As_" is the very first, basic, form of Version Control - the business of tracking and/or reverting the changes you made to (code, images, files) over time. You know:

*   BlogPost.txt
*   BlogPost_V1.txt
*   BlogPost\_V2\_Links.txt
*   BlogPost_Final.txt
*   BlogPost\_Final\_OK.txt

Etc. The image at the right is a screenshot of my actual [ALCE](http://www.bigano.com/ALCE "ALCE - Advanced Local Contrast Enhancer for Photoshop") development main folder, so I've decided to adopt some kind of Version Control, namely: [Subclipse](http://subclipse.tigris.org/ "Subclipse") (a [Subversion](http://subversion.apache.org/ "Subversion") support provider plugin) for Flash Builder 4.6. Since I'm still learning, I will show here my findings from the point of view of a single developer, hoping that this first post (about Installation and setup of a local repository) will save some time to those who, like me, are beginning to put some order to the chaos.

Subversion and Subclipse
------------------------

First if you're new to the topic (as I am), please have a look to this well done and super clear [Visual Guide to Version Control System (VCS)](http://betterexplained.com/articles/a-visual-guide-to-version-control/ "A Visual Guide to Version Control"). It explains the concept of revision control and its terminology.

> Basically, in the centralized (and not distributed) paradigm you set a _Repository: _i.e. a folder (either local or on a remote server) that contains a reference copy of your code. You _Check Out_ (download it) on your machine in order to be able to edit/change it; you _Synchronize with the Repository_ and then _Commit_ (or _Check In_, i.e. upload) the changes. The system keeps track of the changes (as a sum of differences), sets revision numbers, can backup and restore; moreover, you may _Branch_ the code - that is: duplicate the _Trunk_ to experiment new implementations - and eventually _Merge_ them back on the main line. Plus a whole bunch of advanced features that I don't understand yet.

A very common Version Control System is [Subversion](http://subversion.apache.org/ "Subversion"), also known as svn (the command line tool). If you wanna hear bad bad things about Subversion, [Linus Torvalds is a great source](http://www.youtube.com/watch?v=4XpnKHJAok8 "Linus Torvalds on Git"). And if you wonder why choose Subclipse and not Subversive as an Eclipse (and Flash Builder) plugin, there's a [Q&A worth reading here](http://stackoverflow.com/questions/61320/svn-plugins-for-eclipse-subclipse-vs-subversive "SVN plugins for Eclipse").

Installation
------------

In Flash Builder (I use 4.6 but afaik the same applies for 4.5 and 4.0), go to the menu: _Help - Eclipse Marketplace_ and in the following window search for "Subclipse": ![Eclipse Marketplace](http://localhost:8888/wp-content/uploads/2011/12/EclipseMarketplace.png) Click the Install button and restart Flash Builder. It seems enough to consider Subclipse installed.

Setup of a Local Repository
---------------------------

Many of the tutorials I've read at this point show you how to setup a _repo_ (we know the _lingo_ now ;-)) on a remote server. Thanks, but I don't collaborate with people who are in need to sync, and I don't even know how to setup a remote server to put my small repo into. I just need a folder on my local disk. After a couple of unsuccessful tries (one of which involved MAMP), I've understood that the process is:

1.  Locate a folder and tell svn (not Subclipse: svn, the command line tool) to create a new repository.
2.  Point Subclipse to it.

It makes sense - but if you're new to Version Control Systems you'd stare at Flash Builder (as I did) thinking: how on earth I'm supposed to do it?

### Create a repository

First, Subclipse cannot create a repository. Let's say that I want my "repo" folder (that will contain repositories) to be in my Home - I'll use Finder to create that "repo" new folder as I would do for any other folder I need. Then, I must open the Terminal:

![Terminal](http://localhost:8888/wp-content/uploads/2011/12/Terminal.png)

And type a couple of commands:

cd repo

This first line is to enter into the "repo" folder I've just created in Finder.

svnadmin create ALCE_repo

This second line tells svn (the command line tool) to create, within the "repo" folder, an actual repository called "ALCE\_repo" (that will contain my ALCE project). \[caption id="attachment\_567" align="alignright" width="302"\]![Repository structure](http://localhost:8888/wp-content/uploads/2011/12/Repository.png) This is what a repository looks like\[/caption\] That is: "repo" will contain as many different repositories as the projects I need to take control of. If you look at the command's output, you'll see several new files.

### Fill a repository with things

We now have a brand new repo: the second step is to fill it with a Flash Builder project (either a CS extension, or any other Flash/AIR thing you're coding). \[caption id="attachment_573" align="alignleft" width="107"\][![SVN Repository Exploring Perspective](http://localhost:8888/wp-content/uploads/2011/12/Perspective-150x209.png)](http://localhost:8888/wp-content/uploads/2011/12/Perspective.png) SVN Perspective\[/caption\] In Flash Builder, from the menu _Window - Open Perspective - Other..._ browse to _SVN Repository Exploring_. This perspective gives you access to the SVN Repositories panel, which should be empty. To make Flash Builder aware that a repository exists, right click inside the panel and select _New - Repository Location..._ then in the following window, please enter:

file:///Users/\[your user\]/repo/ALCE_repo

![Add SVN Repository](http://localhost:8888/wp-content/uploads/2011/12/AddSVNRepository.png) Please notice that "_file:///"_ has a triple forward slash. ![Created Repository](http://localhost:8888/wp-content/uploads/2011/12/CreatedRepo.png) A new location should be now set, as you see in the screenshot on the right - that is: Flash Builder knows about a repository location. [![Share Project](http://localhost:8888/wp-content/uploads/2011/12/ShareProject-150x186.png)](http://localhost:8888/wp-content/uploads/2011/12/ShareProject.png)The next and final step is to link a project to this repo. The Subclipse commands in the Flash Builder IDE belongs to a menu item called _Team_. To access it, switch back to your usual Flex or Extension Builder perspective, in the _Package Explorer_ panel right click on the project you want to add to the repository, then select: _Team - Share_ In the following window select _SVN_ as the repository type: ![Share Project - SVN](http://localhost:8888/wp-content/uploads/2011/12/ShareProject_SVN.png) then _Use existing repository location:_ ![Share Project - select repository](http://localhost:8888/wp-content/uploads/2011/12/ShareProject_SelRep.png) and finally _Use project name as folder name:_ ![Share Project - select name](http://localhost:8888/wp-content/uploads/2011/12/ShareProject_SelName.png) Mind you, the Console will report a red warning saying that:

Filesystem has no item svn: URL 'file:///Users/Davide/repo/ALCE_repo/myTest' non-existent in that revision

Which you can safely ignore (the following command, mkdir, creates the missing myTest folder). According to the documentation, now the whole project should be committed - that is: all the files copied to the repository. [![Commit](http://localhost:8888/wp-content/uploads/2011/12/Commit-150x150.png)](http://localhost:8888/wp-content/uploads/2011/12/Commit.png)To me, this doesn't happen, so I've to do it manually; right click on the project (myTest in this case, see the screenshot on the right) in the _Package Explorer_ panel and choose: _Team - Commit_ In the window that will pop up, you're able to select which resource send to the repository - setting a custom comment too: ![Commit and comment](http://localhost:8888/wp-content/uploads/2011/12/CommitComment.png) That's all! To check that everything went fine, you should notice several log messages in the console. ![Repository content](http://localhost:8888/wp-content/uploads/2011/12/RepositoryContent.png)Also, go to the _SVN repositories_ panel and click the refresh button (the two yellow arrows) in order to display the changes. The repository now contains the myTest folder and all the project files. Now that the binding is done, you can start using the Version Control System - that is, code freely and _Commit_ each time you want to set a new revision; revisions can be found later in the _History_ panel. I hope this summary has been of some help for you. In the next post of this series, I'll try to dig a little bit deeper (I'm still learning!) around Tags, Branches, revisions, etc. so stay tuned!

Useful links
------------

I've collected a good amount of links while writing this post. Some of them may help you too:

*   [Version Control with Subversion](http://svnbook.red-bean.com/en/1.7/svn-book.pdf "SVN book"), pdf version of the Subversion manual (also published by O'Reilly).
*   Guy Rutemberg on [Creating Local SVN Repository](http://www.guyrutenberg.com/2007/10/29/creating-local-svn-repository-home-repository/ "Creating Local SVN Repository") (with svn command line tools).
*   IBM howto about [Subversion and Eclipse](http://www.ibm.com/developerworks/opensource/library/os-ecl-subversion/#resources "Subversion and Eclipse").
*   North Carolina State University on [Subclipse for Configuration Management](http://agile.csc.ncsu.edu/SEMaterials/tutorials/subclipse/index.html#section1_0 "Subclipse for Configuration Management").