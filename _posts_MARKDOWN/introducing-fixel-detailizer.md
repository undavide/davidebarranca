---
title: " Introducing Fixel Detailizer 2 PS\t\t"
tags:
  - filter
  - Fixel Algorithms
  - frequency
url: 2614.html
id: 2614
category:
  - Extensions and Scripts
  - Fixel Detailizer
date: 2014-07-25 23:09:51
---

I'm glad to announce the release on Adobe Add-ons of a powerful contrast booster for Photoshop: Fixel Detailizer 2 PS. Let's see what is that all about!

Fixel Algorithms
----------------

I've recently started a partnership with Fixel Algorithms, a company founded by engineers and programmers specialized in Digital Image Processing - amazingly smart people. Fixel Algorithms has been creating [Filters for Adobe After Effects](http://aescripts.com/authors/f-l/fixel-algorithms/ "Fixel Algorithms for After Effects") (video processing), and Detailizer is our first shared effort porting to Photoshop their excellent work.

Detailizer at a glance
----------------------

Detailizer 2 PS decomposes your image into discrete Frequency Ranges and allows you to separately control the Contrast Boost of each of them. It's like Frequency Separation on steroids! \[twentytwenty\] ![Detailizer (before)](http://localhost:8888/wp-content/uploads/2014/06/detailizer-before1.jpg) ![Detailizer (after)](http://localhost:8888/wp-content/uploads/2014/06/detailizer-after1.jpg) \[/twentytwenty\]  

### Frequency explained

Do you need a primer on spacial frequency? Basically, it's about the _size_ of the detail in your pictures. For instance, say that you've shot a portrait - take a sample in:

*   the **hair**: there's plenty of fine detail, pixel's values (dark/bright) vary with a high frequency.
*   the **lips**: the variation is smoother compared to the hair, even if the transition between dark/bright values happens with a decent (say: middle-sized) frequency too.
*   the **cheek**: here the tonal variation is really smooth, and covers a larger area: in the sample _things_ change on a lower rate (low frequency).

![Frequency](http://localhost:8888/wp-content/uploads/2014/06/3Freq.jpg) It might not be the most accurate description of what a spacial frequency is, but you've probably got the idea. In the real world the distinction is fuzzier: every area is made with all frequencies combined (within "low-freq" cheeks there is "high-freq" skin texture), the same way a complex signal such a sound or an electromagnetic wave is a combination of simpler signals: ![Multi Frequency](http://localhost:8888/wp-content/uploads/2014/06/multifreq.jpg)

Features
--------

Fixel Detailizer 2 PS peculiar frequency decomposition (the filter's core) is performed with a fast, proprietary **Wavelets algorithm** developed by Fixel. The interface lets you boost separately **5 Frequency Ranges**, in order to target precisely the detail level that you want. ![Detailizer GUI](http://localhost:8888/wp-content/uploads/2014/06/Detailizer-GUI.jpg) Fixel Detailizer 2 PS works on 8bit, 16bit and **32bit HDR** files too! All the calculations are internally performed at 32bits to ensure the maximum precision.

How to get it
-------------

Fixel Detailizer 2 PS is a 5 stars product! This Photoshop Filter available on [Adobe Add-ons](https://creative.adobe.com/addons/products/2757 "Fixel Detailizer PS on Adobe Add-ons") (the brand new app-store for Photoshop extensions) for USD 19.99 and supports Photoshop CC and CC 2014 on both Mac and Windows (64bit).