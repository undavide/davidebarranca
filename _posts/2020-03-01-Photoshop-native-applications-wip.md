---
title: "Photoshop JS Native Applications Course (work-in-progress)"
date: 2020-03-01
author: Davide Barranca
excerpt: "I've been working on my next project, a video-course on Photoshop JS Native Applications development. The Demo app is 98% done, I have to start recording the screencasts now; let's see that what this is all about."
layout: post
description: "I've been working on my next project, a video-course on Photoshop JS Native Applications development. The Demo app is 98% done, I have to start recording the screencasts now; let's see that what this is all about."
image: /wp-content/uploads/2020/03/NWJS.png
media_card: /wp-content/uploads/2020/03/media.jpg
category:
  - Development
tags:
  - Native App
  - JavaScript
  - Machine Learning
---

## TL;DR

- I am working on a new series of **video-tutorials** on Photoshop Development.
- The subject is going to be **Native Applications** (`.app` for Mac and `.exe` for Windows, entirely written in Javascript) that connect to Photoshop.
- Think CEP (aka HTML) Panels, but transplanted outside Photoshop.
- The Demo App is 98% complete: for fun, it also implements a simple _Machine Learning_ pre-trained model for image classification.
- **Availability**: approx. Q3/2020, updates in the blog.
- **Pricing**: $149 for individuals, $249 for companies; early bird discounts available.

## Long time no see

I've slowed down a bit with blog-posting in the last year due to a mix of personal reasons (burnout recovery, house restoration, work, fighting with a neighbour-from-hell, this kind of stuff) plus a general lack of relevant things to say.

In the meantime, since late Q2/2019 I've been playing with the idea of building independent, native apps that can connect to Photoshop and act very much like CEP Panels would. As I'm slooooow and distracted, I've got to the point of having a working Demo App in my hands around March 2020 – not bad for a coding-sloth.

## Why Native Apps?

I got a bit tired of CEP: I wanted to allow myself the luxury of _some more control_ over the development environment; if you're a CEP developer I think you can relate. As anybody who's been around in this business long enough knows, we've had the possibility to connect to PS _from the outside_ for like 10 years (now you can call me a true sloth); reliable Native apps with JavaScript are here, so it was a no-brainer for me to give it a try, finally.

**Pros**:

- You're _totally_ in charge over the technologies to use – no more "This has been fixed in PS 2029, but older PS versions are still out-of-luck", or the cyclic "you've been doing X for a jolly bunch of years, guess what from now on it's done the Y way" kind of troubles.
- Still JS/Node.js stuff, easy transition if you come from a CEP background.
- You can update to Chromium/Node-latest whenever you want for whatever PS version you want.
- Native means independent, separate, dock-bouncing apps, with access to OS-level stuff like tray icons, notifications, FileSystem, you name it; Mac and Windows alike.

**Cons**:

- Not all projects are a good fit for an independent app: if you need a tighter, super-fast integration with PS, stick to CEP or whatever _default_ technology Adobe provides.
- There's a bit of a learning curve, but I'm here to ease the process.

## How does it work, technically speaking?

NW.js, Vue.js, Vuex, Socket.io, Adobe Generator Plugin, MobileNet, Node.js stuff, trial and error. For now you'll make do with that.

## What will the Video Course cover?

I'm going to build a demo app from scratch over the course of some hours of recording. Features are totally arbitrary, and it have been chosen to serve just one purpose: showing how things can be done in the context of a native app connecting to PS, i.e. solving specific problems that you're likely to run into (e.g. passing data from/to PS, executing scripts, synchronous/asynchronous operations, storing permanent data between sessions, etc.)

![](/wp-content/uploads/2020/03/demo-app.jpg)

**What does the demo app do?**

- the user selects a bunch of files from the disk
- the user also toggles few Image Classification tags
- the app retrieves ActionSets/Actions loaded in Photoshop
- if each file's _subject_ matches the ones selected by the user (that one document actually _depicts_ a tiger), Photoshop will process it with the selected Action
- when the batch ends, a System Notification pops up some info

As simple as it looks, it's enough to build a rather structured NW.js/Vue.js architecture that can serve as a blueprint for more complex projects of yours.

Everything's based on a solid build system, so 99% of the tasks are completely automated.

## Course's goals

Like [everything](https://www.ps-scripting.com/bundles.html) I've ever published, the main goal is to give you a substantial head-start, and save you all the time I've spent stuck trying to solve specific technical problems. As in life, there are many more ways to mess up than to get it right: this course will provide you with a selected range of working options to build commercial-grade applications.

## Pricing and Availability

I'm bugfixing the app, which functionally speaking is done. Knowing myself, it will take me some more months to record the actual content, so friendly advice: don't hold your breath.

It'll be priced according to previous courses of mine:

- **$149** for individuals, with lifetime updates and HD videos for streaming
- **$249** for enterprise licenses up to 10 seats, with lifetime updates and HD videos for download

Early bird discounts available. If you're interested, please enter your email in the form below (no spam at all); you'll receive a notification with a discounted offer before the course goes public.

<iframe width="540" height="650" src="https://3dc77a80.sibforms.com/serve/MUIEAHsKpwIq1yNWbj_lxLqna87ra1XJHISVSYzDNKhSsYsYb4rLHME8ojWOr2YC4crxEuI6GRaX1pYUKI5bLh6Knr5sKmIwW1kvScZISpzmqLc1EUaREv-6OVq9ff5bRhJEKnM4Qi-2WIBNuk1Q4IlyEGw5jxOj1iiVa3KqHUw8x2V0NyAwNEZec3NIfkETDoIDPbaN-DPhUG86" frameborder="0" scrolling="auto" allowfullscreen style="display: block;margin-left: auto;margin-right: auto;max-width: 100%;"></iframe>

## Questions?

Unless they're along the lines of _"Sir, why X instead of Y?"_, I encourage you to post in the comments below. Thanks for reading and have a good day!
