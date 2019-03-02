---
title: " iBooks Author - Image compression\t\t"
tags:
  - iBooks
  - image
  - iPad
url: 847.html
id: 847
comments: false
category:
  - Digital Publishing
  - iBooksAuthor
date: 2012-04-06 10:01:58
---

![IBooksAuthor opening](http://localhost:8888/wp-content/uploads/2012/04/iBooksAuthor-opening1.jpg) If you're serious about image quality, you want to know what happens to your pictures when iBooks Author exports a .iBook file: not only you're interested in the ICC profile to use, the allowed filetypes (JPG and PNG), but also what kind of color conversion, JPG compression and resizing is applied to pictures on their way to the iPad. I've run some test and results are… somehow surprising.  The iBook format is a proprietary flavor of e-books made by Apple. It's not a standard (standards advocates argue), but it's exceptionally versatile, easy to produce and fun. May I tell you? I love it. Yet iBooks Author, Apple's free tool for authoring iBooks, is not bug-free; some actions aren't straightforward (read my previous post about [fullscreen, borderless images](http://localhost:8888/2012/04/ibooks-author-fullscreen-images/ "iBooks Author fullscreen images")), but it's only version 1.1 and we're allowed to expect for it a bright future. Let's start the analysis.

### File Format, ICC profile and size

To make a long story short: use JPG or PNG (which support transparency), convert everything to sRGB, and be aware that all the images (with an exception, read along) will be resized to 2048 x 1536 pixels, the new retina display iPad size. This is what [Apple recommends](http://support.apple.com/kb/PH2838).

### How to inspect .iba and .ibook files

[![iBook Assets](http://localhost:8888/wp-content/uploads/2012/04/iBookAssets-132x300.png "iBook Assets")](http://localhost:8888/wp-content/uploads/2012/04/iBookAssets.png)iBooks Author saves projects as .iba files. When you export for the iPad, it outputs an .ibook file. The .iba package contains a copy of the original assets you've imported, while in the .ibook package things are transformed: resized, converted, compressed. Either ones can be inspected this way:

1.  Duplicate the .iba and/or the .ibook files (just to be safe…)
2.  Rename them to .zip
3.  Unzip them in a folder (be aware that OSX doesn't expand them with a double click, I've got to use Stuffit Expander)

That's it. As you see, you end up with a folder structure with xhtml, css and all the needed assets.

### Image dimension comparison

For all the following tests I've used a 2048 x 1536 pixel image, saved as JPG maximum quality, sRGB. When you import a picture in iBooks Author you resize and move it to fit the page's design, deciding whether to allow the image to pop up fullscreen or not (more details about the three ways you can choose to do it in my previous post). If the image is not allowed to go fullscreen, the .ibooks package will only contain an exact sized version of the original imported file: that is, if the page holds a 600 x 800 pixels JPG, the .ibook asset will be automatically rescaled as a 600 x 800 pixel JPG. On the contrary, if the image is part of a widget (_Image_ or _Gallery_) things get weird:

*   Images with a stroke (default is a 1pt line) end up as a **2040 x 1530** px JPG.
*   Images without a stroke end up as a **2048 x 1536** px JPG.

![](http://localhost:8888/wp-content/uploads/2012/04/InspectorFullscreen1.png "InspectorFullscreen.png")So applying a stroke actually shrink the .ibooks image dimension. Mind you, the **_Full-screen only option_ has no effect whatsoever** on image dimension (on the iPad, a widget image will always go fullscreen), so I suggest you to keep it unchecked. Moreover, an _Image_ widget will only expand fullscreen to maximum **2008 x 1319** pixels ([see here for the details and examples](http://localhost:8888/2012/04/ibooks-author-fullscreen-images/ "iBooks Author fullscreen images")), while _Gallery_ images are allowed to go both fullscreen and _borderless_ (2048 x 1536). As a last option, if the image is part of an _Interactive Image_ widget (i.e. not only it goes fullscreen, but you're allowed to zoom in and explore preset details at higher magnification), the final size will depend on the zoom percentage - that is, you can feed iBooks Author with a 5000 x 5000 pixels image and expect to have it in the .ibooks assets.

### Profile comparison

\[caption id="attachment_866" align="alignleft" width="180"\][![iPad1/iPad2 over iPad3](http://localhost:8888/wp-content/uploads/2012/04/ipad1-2overiPad3-300x290.jpg "iPad1/iPad2 over iPad3")](http://cdtobie.wordpress.com/2012/03/21/more-answers-about-the-new-ipad-and-color/) Gamut comparison of iPad1/iPad2 against the new iPad © C. David Tobie\[/caption\] If you're into color management and don't know yet [C. David Tobie's blog](http://cdtobie.wordpress.com/) (he's _Global Product Technology Manager - Imaging Color Solutions, [Datacolor inc.](http://www.datacolor.com/)_) I strongly suggest you to read at least his post series about Color Management and the iPad ([More answers about the new iPad and Color](http://cdtobie.wordpress.com/2012/03/21/more-answers-about-the-new-ipad-and-color/) contains the links to them all). He's very competent and you'll read a lot of original content that you won't find elsewhere. To my tests I've confirmed that, even though both the .iba and .ibooks assets contain sRGB tagged images, the exported one appears lighter (hence some secret Apple processing has been going on). I've tested a [synthetic ColorChecker image](http://www.babelcolor.com/main_level/ColorChecker.htm#ColorChecker_images), measuring these differences:

*   about 4 points in the b of Lab in blues and cyans (original has an higher saturation)
*   about 1 point in the a of Lab for yellow and orange
*   a shift towards green in the base color (.ibooks compressed JPG)

![ColorChecker sRGB Comparison](http://localhost:8888/wp-content/uploads/2012/04/ColorChecker_sRGB_COMPARISON.jpg "ColorChecker sRGB Comparison")Left is original, right is .ibooks asset - almost unnoticeable. Not something to be obsessed with - actually the ColorChecker comparison alone is not very much significative, if you test real world photographs the .ibooks JPG version appears constantly lighter.

![iBooksAuthor Profile Comparison](http://localhost:8888/wp-content/uploads/2012/04/iBooksAuthor-ProfileComparison.jpg "iBooksAuthor Profile Comparison")

Again, left is original, right is .ibooks asset - now you see the difference! To simulate the effect, add a Curves adjustment layer to the original image and raise the middle point of the master curve about +20 points (input: 127, output: 147), this is more or less how the .ibooks file will look like.

### Compression comparison

I've imported into iBooks Author a JPG saved for the web in Photoshop CS6, maximum quality: then I've checked the .ibooks JPG against the .iba (which is identical to the original). As you can see by yourself, there's a lot of JPG compression and artifacts in the .ibooks file: and it isn't shocking, because the .iba JPG is 1.2MB, while the .ibooks JPG is 560KB (less than half of the filesize!). What kind of JPG compression has been used? To test it, I've put together a simple Photoshop script to save for the web 101 JPGs from the same original, with Quality ranging from 0 to 100. To the best of my eyesight, I've identified the compression just below 50: yet it isn't identical - for instance, in the .ibook version there's a lot more dithering around the edges, therefore that it may be that some quality filter based on image content has been used - just like when in Photoshop you set an alpha channel to determine the compression range. \[caption id="attachment_876" align="aligncenter" width="570"\][![iBooksAuthor - JPG Compression Comparison](http://localhost:8888/wp-content/uploads/2012/04/iBooksAuthor-CompressionComparison.jpg "iBooksAuthor - JPG Compression Comparison")](http://localhost:8888/wp-content/uploads/2012/04/iBooksAuthor-CompressionComparison.jpg) Left: the original imported JPG (quality 100); middle, a JPG quality 48; right, the JPG from .ibooks assets (unknown JPG compression). Click the image to get a larger version.\[/caption\] Yet, if you zoom in the .ibook image, it looks like Frankenstein compared to the immaculate beauty of the original, max quality JPG.

### Conclusions

Be happy and build your iBooks. Stick to 2048 x 1536 pixels, sRGB, max quality JPGs (unless you plan to use Interactive Image widgets, that may require higher resolution images). Don't worry about the conversion, there's nothing you can do - proof your images on the iPad and tweak them in Photoshop accordingly, if they don't look good to you. Yes, they're horribly compressed and if you peep them 400% it's like a sea of JPG artifacts, but you know what? On the iPad retina display they're not that much obvious, you can live with them. And by the way you could always export the .ibooks, unzip it, substitute the JPGs with higher quality ones, zip it again, cross the fingers and submit the file for approval to Apple - if you succeed, please let me know! For sure I won't be the first one to hack iBooks and risk to make the big A angry ;-)