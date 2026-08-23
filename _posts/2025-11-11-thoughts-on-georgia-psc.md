---
layout: post
title: "cgit"
date: 2025-11-28
permalink: "/blog/cgit"
author: Benjamin Sanders
catagories: cgit
---
Since I've moved my website(s) over to a dedicated server I started using I've been meaning to configure my own host for git repos. I ran
into some trouble getting it to work properly but thanks to [Luke Smith's](https://lukesmith.xyz) [landchad.net](https://landchad.net) somebody had written an article how to get a basic one
working.

Turns out the main thing I was missing was this line from my nginx site config file `try_files $uri @cgit ;`. The error that fixed was a 403 Forbidden error I kept getting when attempting to visit [git.sanders.life](https://git.sanders.life/). 

I did use the article to make sure HTTPS cloning would work too though and cloned a couple repos I'm interested in having a mirror.

Now, though, I can freely push my repos to my own server with a read-only frontend to it for other netizens!

Anyway, thanks for reading this little update to my site and services.