---
title: " 'PS Project' for Photoshop CC / CS6 User Manual\t\t"
tags:
  - Photoshop CC
  - Photoshop CS6
  - PS Projects
  - User Manual
url: 2257.html
id: 2257
category:
  - Extensions and Scripts
  - Photoshop
  - PS Projects
date: 2013-10-04 11:05:10
---

PS Projects is a Photoshop CC / CS6 script that implements the idea of Project Files (a lightweight container of links to images on your disk, that you need to open frequently). This 'User Manual' post will go into every detail of it.

Introduction
------------

If you haven't seen them already, please go over this one minute presentation and read the companion [Introducing PS Project](http://localhost:8888/2013/09/introducing-ps-projects-for-photoshop-cc-cs6/ "Introducing PS Projects for Photoshop CC / CS6") blogpost to get a brief, visual overview about what the product is all about. http://www.youtube.com/watch?v=5EYZNU6gAZw

Installation
------------

You can get PS Projects from [Adobe Exchange](http://www.adobeexchange.com "Adobe Exchange") or [Bigano e-store](https://store.bigano.com "Bigano e-Store"). In order to streamline the installation process, please make sure you have the latest version of Adobe Extension Manager (download here: [CC](http://www.adobe.com/exchange/em_download/ "Adobe Extension Manager CC"), [CS6](http://www.adobe.com/exchange/em_download/em6_download.html "Adobe Extension Manager CS6")). If you run into problems with the installation, ask for support: Exchange users can [post here](http://www.cs-extensions.com/support/ "CS Extensions support"), Bigano customers will [ask here](mailto:support@bigano.com "Bigano e-Store support"). [![File - Automate - PS Projects](http://localhost:8888/wp-content/uploads/2013/09/File_Automate_PSProjects-150x172.jpg)](http://localhost:8888/wp-content/uploads/2013/09/File_Automate_PSProjects.jpg)

Where is it?
------------

PS Projects belongs to the **File - Automate** menu ->

Main Dialog
-----------

PS Projects has three modules that you can access from the Main Dialog, this User Manual will cover them all. Just below the buttons, the \[INFO\] link points to this very page. ![PS Projects - Main Dialog](http://localhost:8888/wp-content/uploads/2013/09/PSProjects_MainDialog.png)

*   **Create New** \- select files from any folder in your hard drive and Save a new PS Project.
*   **Load** \- load a PS Project from the disk to open the associated images in Photoshop.
*   **Modify** \- load a PS Project from the disk and modify the list of the included images, then Save the updated PS Project.

Create New
----------

Clicking the Create New button opens the Save a Project dialog: [![PSProjects - Create New](http://localhost:8888/wp-content/uploads/2013/09/PSProjects_CreateNew.jpg)](http://localhost:8888/wp-content/uploads/2013/09/PSProjects_CreateNew.jpg) First click the **Browse...** button to run the Photoshop "Open dialog" and choose images from your hard disks. You can include in a PS Project all formats Photoshop is able to deal with (PSD, TIFF, JPG, DNG... etc.). If you happen to have files already open in Photoshop, provided that they have been saved at least once (and they haven't been modified afterwards), you can include them with the **Add Open Files** button - which is grayed out in the screenshot above since there are no open files at all. The **Remove** button does what you guess - it removes from the list all the selected files. [![PS Projects - Bridge Warning](http://localhost:8888/wp-content/uploads/2013/09/PSProjects_BridgeWarning-150x135.png)](http://localhost:8888/wp-content/uploads/2013/09/PSProjects_BridgeWarning.png)The **Files** list shows filenames of the included assets. You can click on them (multiple selection allowed): the **Path** will appear below and you'll get a **Preview** thumbnail. File formats other than JPG and PNG require an instance of Adobe Bridge open to display the preview - the same way MiniBridge requires his bigger brother to work properly. No problem if you haven't it or don't want to run it - these preferences can be set (the window at left pops up the first time) and you'll get a generic thumbnail as the preview. When you click the **Save** button, you'll be prompted with the usual dialog: choose a meaningful name (without extension: the script will add _.psproject_ automatically) and save the file wherever you like. As soon as the save dialog is closed, an alert will ask you if you're willing to immediately open the images of the Project file just saved or not.

Load...
-------

Select **Load...**  to locate an existing _.psproject_ file on your disk. All the images that are referenced in the Project will be opened in Photoshop. In case the Project contains missing images (say you've moved, or deleted them from the disk after saving the Project) an alert will log all the lost entries. [![PS Projects - Modify Dialog](http://localhost:8888/wp-content/uploads/2013/09/PSProjects_ModifyDialog-273x300.png)](http://localhost:8888/wp-content/uploads/2013/09/PSProjects_ModifyDialog.png)

Modify...
---------

The **Modify...** button is a combination of the other two. You've first to select an existing _.psproject_ file; then a dialog identical to the **Create New** one will appear, automatically filled in with the list of images referenced in the Project file you've just loaded. In case of missing images you'll get both a warning pop-up and a red icon besides the filenames, showing an exclamation mark thumbnail.

Requirements
------------

PS Projects runs on Photoshop CC and CS6, on both Mac and Windows.