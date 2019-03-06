---
title: Migrating from WordPress to Jekyll on Netlify
date: 2019-03-05T11
author: Davide Barranca
excerpt: Bye-bye Wordpress.
layout: post
permalink: /2019/03/migrating-from-wordpress-to-jekyll-on-netlify/
description: How to migrate from WordPress to Jekyll on Netlify
image: /wp-content/uploads/2019/03/jekyll.png
draft: true
category:
  - Personal
tags:
  - Jekyll
  - Netlify
---

After years, I've been able to kiss WordPress bye-bye and migrate to a fully static site build with Jekyll and deployed to Netlify. In this post I'll tell you why, and show you how.

## Why not WordPress?

I've nothing against WP in principle, it's not the right tool for me. I blogpost on average once a month, it makes no sense for me to be bound to a Linux hosting with mySQL access, a BackupBuddy subscription plan and a dozen of other WP plugins to run this site. Have you seen my posts? A couple of images, few snippets of code, some text. Really, WP is plain overkill; plus, I don't want to worry about WP updates, PhpMyAdmin, DB access errors, log-ins, plugin incompatibility and fancy dashboards anymore.

Now I have a simpler, static site which I update writing text files on my disk, committing to a free git repository, which Jekyll files are automatically built, hosted and served over https for free by Netlify.

## Static vs. Dynamic

As opposed to WP, where each PHP _template_[^template] is filled on-demand – i.e. when a user requests a page, fetching the content from a database and returning the processed data as html – a Static site is _pre-compiled_ so to speak, and simply made available online all at once.

[^template]: For a lack of better word.

As a consequence, a WP site needs a machine running some server-side scripting language such as PHP, a database like mySQL, and some processing resources; a static site is happy when it is hosted on a server that is (a) turned on, and (b) connected to the net.

## SSGs

If you're not familiar with the concept of Static Site Generators, they're command-line tools that get a bunch of HTML/JS/CSS with template code and markdown files as input, and output a full static website. Your job is then to move the files on the server.

There are several SSGs available: to the best of my knowledge, the most popular ones are [Jekyll](https://jekyllrb.com/) (written in Ruby), [Hugo](https://gohugo.io) (written in Go) and [Hexo](https://hexo.io/) (Javascript). Each one of them has its peculiar templating system and folders structure.

<figure>
<a href="https://www.staticgen.com/" target="_blank"><img src="/wp-content/uploads/2019/03/staticGen.jpg"></a>
<figcaption>If you feel inclined, a way too big list of SSGs is found <a href="https://www.staticgen.com/">here</a>.</figcaption>
</figure>  

All of them share the sublime idea that you compose your writing (both pages and blog-posts) in MarkDown: a text file with a basic set of formatting rules, such as \*\***bold**\*\*, \__italic_\_ etc. Hence, all your website content is not hidden into some resources-hungry DataBase, but exposed to you as plain text files with a `.md` extension.

### Which SSG to pick

If you start fresh (i.e., you don't run a SSG already) or you don't have plenty of time on your hands, I suggest you to look at the available templates for Jekyll ([free](https://jekyllthemes.io) or [paid](https://jekyllthemes.io/premium)), for [Hugo](https://themes.gohugo.io/) and for [Hexo](https://hexo.io/themes/). Beware high expectations: they're all quite _bare_. Pick the one you like the most, and then learn that templating language to customize it.

On a superficial level, that's all you need. To me – and I'm not really into SSGs enough to get all the nuances – besides the templating language, the only other difference is compilation speed. Being written in Go, **Hugo goes like a rocket**. My website is compiled by Hugo in 1.5 seconds, whereas Jekyll takes ~14 seconds, and Hexo is not that much better.

![Hugo vs. Jekyll](/wp-content/uploads/2019/03/hugo.png)

Keep in mind that each time you modify a thing and save, the process re-generates (if you want, incrementally) the whole website, so 14 seconds to see whether a CSS rule or a template tweak really do what you originally meant them to do, may be a long time to wait – the fifth time in a row that you hit save.

I am on Jekyll, using a mildly customized version of the [Steve theme](https://themeforest.net/item/steve-a-minimal-blog-theme-for-jekyll/15601096), which costed me like $15. I already run [CC-Extensions](https://cc-extensions.com/) on Jekyll, I'm mildly familiar with it, so I'll keep being patient if it takes seconds to compile.

## WordPress migration

I've followed this very checklist myself. Perhaps things can be made simpler, but this has worked fine for me – feel free to google stuff if you need more detailed information.

1. Migrate all your comments to [Disqus](https://disqus.com): sign up and follow the instruction to install Disqus on WordPress (you'll need to get [this WP Plugin](https://wordpress.org/plugins/disqus-comment-system/)).
2. Do a full backup: I've always used [BackupBuddy](https://ithemes.com/purchase/backupbuddy/), which isn't cheap but works like a charm – perhaps also the built-in WP Export is OK. You need to backup both the WP assets and the DB.
3. Install [MAMP](https://www.mamp.info/) or a similar software and restore your WordPress installation on a local server (e.g. on your laptop). This will make the following step faster.
4. On your local WP, install both [WP to Hugo](https://github.com/SchumacherFM/wordpress-to-hugo-exporter) and [Jekyll Exporter](https://wordpress.org/plugins/jekyll-exporter/) migration tools, and perform both Exports. You'll get two `.zip` files with a bunch of MarkDown in them. I've found that the Hugo version of the posts returns a better MarkDown conversion – but the files aren't named as they should (i.e., prefixed with the date, like `2018-11-29-something.md`).

## Jekyll import

5. I have then manually reviewed the markdown files of the majority of my posts (~150) coming from the export, deleting items in the YAML FrontMatter[^frontmatter] that aren't meaningful, and fixing the markdown.
6. Make sure you keep the original `permalink` (the post URL): this way each post will have the same URL of your old WP site. This way people who get to you from other sites' links don't get 404'ed, and you keep analytics intact.
7. Remove in the MarkDown all the links to `http://localhost:8888/`. For instance, my Hugo export (the one with nicer markup) has all the posts' assets with urls like `http://localhost:8888/wp-content/uploads/2018/03/logo.png`. Do a batch search and replace and turn them to `/wp-content/uploads/2018/03/logo.png`.
8. Move the markdown posts in Jekyll's `_posts` folder, and also move in the website root the exported `/wp-content` folder, which contains all the images coming from WP[^redundant]

An example of the YAML FrontMatter for this very post:

{% highlight yaml %}
---
title: Migrating from WordPress to Jekyll on Netlify
date: 2019-03-05T11
author: Davide Barranca
excerpt: Bye-bye Wordpress.
layout: post
permalink: /2019/03/migrating-from-wordpress-to-jekyll-on-netlify/
description: How to migrate from WordPress to Jekyll on Netlify
image: /wp-content/uploads/2019/03/jekyll.png
draft: true
category:
  - Personal
tags:
  - Jekyll
  - Netlify
---

{% endhighlight %}

[^frontmatter]: It's the _header_ content of each `.md` file, which is wrapped with three dashes `---`. It contains the post's metadata (e.g. the title, the excerpt), that is used by Jekyll to display it.

[^redundant]: There is a lot of redundant stuff in there, for WP creates several versions of all images you've imported at different resolutions.

## GitHub

The whole idea around this SSG thing is that both the site build and the site updates must be easy. I've created a git repository on [GitHub](https://www.github.com) and pushed my Jekyll website there. Be aware that GitHub itself provides you for free with [GitHub Pages](https://pages.github.com/), a service based on Jekyll that automatically builds your site each time you push a commit to a `gh-pages` branch, and hosts the result on https for free.

There are some limitations in terms of Jekyll plugins (see the [whitelist here](https://help.github.com/en/articles/configuring-jekyll-plugins#default-plugins)), so I've decided to try a different approach.

## Netlify

Go create a free account on [Netlify](https://www.netlify.com/), which provides an amazing service similar to GitHub pages. Then link your GitHub/Bitbucket repository, define a build command (mine is simply `jekyll build`), and they will serve the Jekyll output on a dedicated, public subdomain – that you can use to check the site or point collaborators/clients to.

At this point, you can link your existing domain (the process is quite easy): Netlify will give you few domain name servers to set e.g. on GoDaddy, or wherever your domain is hosted. You'll be asked also to add to Netlify all the existing CNAME and MX records from your host (copy them from GoDaddy – they are for email, FTP and such).

Then you'll have to wait some hours for the DNS propagation, during which your website won'be served through https – the free [Let's Encrypt](https://letsencrypt.org/) certificate will be issued shortly, and your static site will finally be on SSL.

![Hugo vs. Jekyll](/wp-content/uploads/2019/03/netlify.png)

## Conclusions

I am genuinely happy to have streamlined my blogging workflow. There's lot of room for improvement – e.g. on the theme, SEO, social cards etc. – but given the little time it took:

- I have gotten rid of WordPress and related expenses (pricey hosting and plugins).
- Posts are easier to access, create and edit.
- I generally feel more in control, and less subject to random, time consuming issues.
- I haven't lost anything relevant in terms of functionality.

If I had more time I'd explore Jekyll and Netlify features more in depth, or even consider adapting my theme to Hugo to save some building time in the future. Luckily, I haven't got any spare time left 😅 So I'll just call quit and feel good.  
Thanks for reading!
