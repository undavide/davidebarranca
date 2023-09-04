---
title: "Professional Photoshop Scripting (2nd edition) is in the work!"
date: 2023-09-04T11:02:17+01:00
author: Davide Barranca
excerpt: "TL;DR I am writing the second edition of my book Professional Photoshop Scripting, UXP Scripts and Plugins. I'm super-happy, it's turning out great."
layout: post
permalink: 2023-09-04-photoshop-scripting-second-edition/
image: "/wp-content/uploads/2023/09/crane.png"
description: "TL;DR I am writing the second edition of my book Professional Photoshop Scripting, UXP Scripts and Plugins. I'm super-happy, it's turning out great."
category:
    - Scripting
tags:
    - Professional Photoshop Scripting
---

Hello, internet, this is a quick memo to inform you all that I am currently working on the **2nd edition of my book "Professional Photoshop Scripting"**, covering UXP Scripts and Plugins. It's a complete rewrite with brand new content, which I am super proud of—I shouldn't say, but I am. I can't wait to publish.[^0]

I'm posting here because I'm well past the halfway mark of the necessary work—otherwise, I wouldn't dare to say. I've completed (he checks) eight chapters out of ten/eleven, but the bulk of the content is there; then, it'll be the usual deadly wrestling match with LaTeX for the book design, illustrations and whatnot. I hope to be able to record videos too.[^1]

The **Table of Contents** (subject to changes) so far is this:

1. Photoshop Scripting in the Twenties
2. The Development Environment
3. Fundamentals of UXP DOM API and JavaScript
4. The DOM
5. ActionJSON
6. The Imaging API
7. Rust WASM and Hybrid C++ plugins
8. The FileSystem
9. (User Interfaces) TBD
10. (Metadata) TBD
11. (TBD)

I can't say how many pages I've written because I've forced myself to use MarkDown first. According to Obsidian (a [fantastic application](https://obsidian.md) for MD I couldn't recommend more), I've typed 482.020 chars so far—whatever it means, it feels like a lot.

![Obsidian](/wp-content/uploads/2023/09/obsidian.png)

This post also answers anyone who's written to me asking about the 1st edition and whether it would be helpful to get started with Photoshop Scripting.

No, I'm afraid it wouldn't. While it possibly makes sense to stick to ExtendScript and CEP in the short term for Premiere Pro and After Effects (maybe), Photoshop has committed to UXP: anyone starting a PS extensibility project now should use it, period.

As an educator, my long-term plan is to offer (at least) two courses that complement each other:

-   Professional Photoshop Scripting, 2ed: About the **PS Scripting API**, building UXP plugins and scripts. Touches on the UXP API.
-   [Adobe UXP plugins development with ReactJS](https://www.ps-scripting.com/uxp-react.html): About the **UXP API**, building UXP plugins with ReactJS. Touches on the Photoshop API.

There is some overlap, but Adobe applications' extensibility has always required knowledge of the host application's scripting layer and its own layer. That is, how you script Photoshop, and how you wrap the code with products with an interface, a lifecycle, shared capabilities of the UXP platform, a spot in the Creative Cloud Marketplace, etc.

I should have started with Pro PS Scripting 2ed first, but the DOM API wasn't complete enough. Not that it is now, but at least my dear friend [Jaroslav Bereza](https://bereza.cz) and I have had the opportunity to temporarily join the Adobe ranks as contractors to help in 2022—which has been _amazing_, to say the least.

In any case, I've written all I wanted to say. Thanks for your time and take care!

---

[^0]: When you read this, it means that the author is exhausted and wants to move on to other things. And/or their daughter is in Ireland to study, and they desperately need to start monetising all this time they've <s>wasted</s> invested sitting on a desk writing for months on end while their spouse expressed (valid) concerns about the current state of the bank account's balance vs. the mortgage. Hypothetically, I mean.
[^1]: I don't want to/can't further delay the publication, because of Ireland and stuff. I might add them later.
