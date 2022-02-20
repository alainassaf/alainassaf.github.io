# alainassaf.github.io

This repo contains the content of my website and blog.

*Copyright (c) 2022 Alain Assaf All Rights Reserved*

Any views expressed here are my own and in no way reflect on my current employer, organziations, businesses, or services I interact with.

# Blog Layout

Much of the layout and ideas on navigation for my blog came from [**Kevin Marquette's blog**](https://powershellexplained.com/). I recommend checking his blog out. It is a terrific resource for learning PowerShell.

# Theme Template is Beautiful Jekyll

> *Copyright (c) 2016 [Dean Attali](http://deanattali.com). Licensed under the MIT license.*

**Beautiful Jekyll** is a ready-to-use template to help you create an awesome website quickly. Perfect for personal blogs or simple project websites.  [Check out a demo](http://deanattali.com/beautiful-jekyll) of what you'll get after just two minutes.  You can also look at [Dean Attail's personal website](http://deanattali.com) to see it in use, or see examples of websites other people created using this theme [here](https://github.com/daattali/beautiful-jekyll#showcased-users-success-stories).

# Container Usage
To start the blog in development mode use the following command:
```termnial
docker-compose up -d
```
This will start up a local Jekyll server (you must have Docker desktop installed), build the site, and serve it on http://localhost:4000.

To follow the logs use the following command:
```terminal
docker-compose logs -f
```
If this error occurs:

 *"Bundler: You must use Bundler 2 or greater with this lockfile"*

 delete the Gemfile.lock and redo the `docker-compose up -d` command. This will re-create the Gemfile.lock file.

---
 ### Container Usage Sources
* https://takacsmark.com/how-to-set-up-docker-container-for-your-github-pages-jekyll-site/
* https://github.com/takacsmark/takacsmark.github.io/blob/master/docker-compose.yml
* https://www.youtube.com/watch?v=6UAf8b_2juk&t=3s