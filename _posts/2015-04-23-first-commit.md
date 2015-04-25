---
layout: post
title: First Commit
description: "Its ON, baby"
headline: "Let's Fire up the Engines"
categories: personal
tags: [blog, Jekyll]
imagefeature: "website-speed.jpg"
imagecredit: spreadeffect.com
imagecreditlink: "http://www.spreadeffect.com/blog/improve-website-speed/"
comments: true
mathjax: null
featured: true
published: true
---

>&quot;The beginning is the most important part of the work.&quot;
><small><cite title="Plato">Plato</cite></small>

Recently, I met across this beautiful site [http://gaocegege.com/Blog/cpp/cppclass/](http://gaocegege.com/Blog/cpp/cppclass/) when I was looking for some materials about **KMP** algorithm. 
I was attracted by the layout, the fonts and the colors immediately before I decided to build one for myself.

This website is static. It leverages an open source project on GitHub: [Notepad](https://github.com/hmfaysal/Notepad).

I spent almost a whole day figuring out all the details and specifics and finally made it. During the process, these two links helped me a lot:

1. [http://allandenot.com/development/2015/01/11/blogging-like-a-dev-jekyll-github-prose-io.html](http://allandenot.com/development/2015/01/11/blogging-like-a-dev-jekyll-github-prose-io.html)
1. [http://hmfaysal.github.io/Notepad/theme/documentation/](http://hmfaysal.github.io/Notepad/theme/documentation/)

Simply summarising:

1. Clone [Notepad](https://github.com/hmfaysal/Notepad) to your own repo. Then **Switch to gh-pages branch!**
1. Open _config.yml. Add **url: http://user_name.github.io/project_name** in it. Here user_name is your GitHub user name, project_name is your repo.
1. Type http://user_name.github.io/project_name in your brower. OK.

**Be aware: if you want to run your website locally, don't add url or assign url an empty string!**