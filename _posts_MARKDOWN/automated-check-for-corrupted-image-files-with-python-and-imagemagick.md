---
title: " Automated check for corrupted image files with Python and ImageMagick\t\t"
tags:
  - corrupted
  - image python
  - imagemagick
  - integrity check
url: 3337.html
id: 3337
category:
  - Coding
  - Photoshop
date: 2018-05-08 01:20:19
---

How do you check if an image file (tiff, psd, psb) is corrupted, other than looking at its thumbnail with Bridge, or opening it on Photoshop? With a small Python script and ImageMagick! Read along.

Background
----------

The client of mine I work as a retoucher for had some problems with the so-called _Data Migration_ (the dull, time consuming, and error prone process of transferring a lifetime backup from old, once very expensive external drives to a set of new, somehow still equally expensive external drives). As a result, he got some corrupted files here and there in the destination drives – that's the reason why you _migrate_ data: the source has insufficient capacity, it has become unstable, obsolete, or both combined. Problem is that we're talking about several TB of data, mostly as .psb files (ranging from about 1 up to 20GB each), and it goes without saying that opening them all in Photoshop is not an option; nor you can trust Adobe Bridge thumbnails – provided that you've set the preferences to render previews for big files too – because it's a manual process anyway. Even if I'm paid by the hour, staring at thumbs is not my preferred way to get blind. After some research, I've found no way (other than the one I'm about to describe) to check for psd/psb files corruption in an automated fashion. Which seems to me quite odd – if you have better, i.e. faster and/or simpler, solutions, please do suggest them to me in the comments below.

What you need
-------------

[Python 3](https://www.python.org/download/releases/3.0/), and [ImageMagick](https://www.imagemagick.org/). Both will work either on Mac or Windows: I've no experience of them on the latter platform, so I will just assume that you will be successful in following the installation instruction provided in the official home pages. PC owners: read at least the part relative to ImageMagick. Mac users: read it all.

### Python 3

If you're on a Mac like me, you already have Python installed. Chances are that it is version 2.7, or another one but 3. Open the Terminal and type:

$ python --version
Python 2.7.10

This is what I still get after having installed Python 3.6 myself, via [Homebrew](https://brew.sh/index_it). I'm no Python expert, so it took me some Google time to understand that on a Mac you can still have the `python` command pointing to the System's old 2.7 version, even if you've freshly installed the new one. Solutions involve manually changing symlinks (power users advise against it), or using one of the available packages to create isolate Python environments (e.g. [virtualenv](https://virtualenv.pypa.io/en/stable/), [pyenv](https://github.com/pyenv/pyenv), etc. A list of them is found [here](https://stackoverflow.com/questions/41573587/what-is-the-difference-between-venv-pyvenv-pyenv-virtualenv-virtualenvwrappe)). I couldn't make neither of them to work in a reasonable amount of time, so I've resorted to simply use the `python3` command, e.g.

$ python3 --version
Python 3.6.2

### ImageMagick

[ImageMagick](https://www.imagemagick.org/) is a multiplatform, open source commandline utility that performs a huge amount of tasks on all kinds of image files. I've installed via Homebrew, but it turns out that, at least on the Mac, it doesn't come by default with the proper Delegates (aka Libraries) to deal with .psb files, which is what I needed the most. Finding the proper way to do so proved almost impossible to me: while reading the source code documentation (the last thing I wanted to do was to compile it from the source), I've discovered that via Homebrew you can list all the possible installation options for a package:

$ brew options imagemagick

--with-fftw
	Compile with FFTW support
--with-fontconfig
	Build with fontconfig support
--with-ghostscript
	Build with ghostscript support
--with-hdri
	Compile with HDRI support
--with-liblqr
	Build with liblqr support
--with-librsvg
	Build with librsvg support
--with-libwmf
	Build with libwmf support
--with-little-cms
	Build with little-cms support
--with-little-cms2
	Build with little-cms2 support
--with-opencl
	Compile with OpenCL support
--with-openexr
	Build with openexr support
--with-openjpeg
	Build with openjpeg support
--with-openmp
	Compile with OpenMP support
--with-pango
	Build with pango support
--with-perl
	Compile with PerlMagick
--with-webp
	Build with webp support
--with-x11
	Build with x11 support
--with-zero-configuration
	Disables depending on XML configuration files
--without-freetype
	Build without freetype support
--without-jpeg
	Build without jpeg support
--without-libpng
	Build without libpng support
--without-libtiff
	Build without libtiff support
--without-magick-plus-plus
	disable build/install of Magick++
--without-modules
	Disable support for dynamically loadable modules
--without-threads
	Disable threads support
--HEAD
	Install HEAD version

So, after a first installation (without psb support), with no clear hint about the proper option(s) to use in my case, and even less spare time to test, I've chained them all – at least the seemingly appropriate ones, with little worries about being redundant. At all events, no one was watching me, nor would have ever known :-) The embarrassing line I've used is:

brew reinstall imagemagick  --with-fftw --with-fontconfig --with-ghostscript --with-hdri --with-libde265 --with-liblqr --with-librsvg --with-libwmf --with-little-cms --with-little-cms2 --with-opencl --with-openexr --with-openjpeg --with-openmp --with-pango --with-perl --with-webp --with-x11

It worked, so I was a happy camper.

The Python Script
-----------------

Which is far from perfect, but it does the job – I'm sure that a proper Python developer can make it much better: it comes from surgical copy&paste from various Google Search result, plus a very light editing on my side.

import os
from subprocess import Popen, PIPE

folderToCheck = '/Volumes/16TB/whatever/path'
fileExtension = '.psb'

def checkImage(fn):
	proc = Popen(\['identify', '-verbose', fn\], stdout=PIPE, stderr=PIPE)
	out, err = proc.communicate()
	exitcode = proc.returncode
	return exitcode, out, err

for directory, subdirectories, files, in os.walk(folderToCheck):
	for file in files:
		if file.endswith(fileExtension):
			filePath = os.path.join(directory, file)
			code, output, error = checkImage(filePath)
			if str(code) !="0" or str(error, "utf-8") != "":
				print("ERROR " + filePath)
			else:
				print("OK " + filePath)

print("-------------- DONE --------------");

### How it works

The basic is the `identify` call (that comes from ImageMagick), which is set to `-verbose`. This is what performs the check: the rest is just looping through the filesystem, looking for the appropriate file extension, and logging a message.

### How to use it

Save this on a file with a `.py` extension, and then run it with the `python3` command on a terminal, e.g.

$ python3 check.py

Before doing so, do change the content of the `folderToCheck` variable with an actual folder on your disk (with absolute path), and the `fileExtension` too: I've used `.psb`, but you can change it to `.psd`, `.tif`, `.jpg`, etc. As a result you'll get a log in the Terminal; I've used a nifty, cheap application called [Code Runner](https://coderunnerapp.com/) for such tests, and this is the result: [![](http://localhost:8888/wp-content/uploads/2018/05/coderunner-700x426.png)](http://localhost:8888/wp-content/uploads/2018/05/coderunner.png) As you see, I'm just logging OK/ERROR with the path, very basic. What to do with this newly acquired piece of knowledge is up to you. Please note that:

*   The script processes nested folders too.
*   It is _awfully_ _slow_ and hungry: it eats CPU cycles and RAM. But it's automatic, so heck!
*   The file extension is case sensitive, so `".JPG"` is different from `".jpg"`.

### How to make it better

Few suggestions for the skilled Python developer (which I'm not, alas)

*   Write on a log file instead of the console
*   Keep track of the processing status and resume from there
*   Display the advancement status (say, "image 34 of 320")
*   Make the file extension case insensitive.

If you know how to do any of this, please share your knowledge in the comments! Thank you!