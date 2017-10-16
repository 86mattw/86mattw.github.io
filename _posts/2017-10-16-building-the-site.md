---
title: Building the site
tags: blog jekyll
---

As previously mentioned, the site is built using Jekyll and is currently hosted on Github Pages.

My setup at home is Windows based which presented the first issue. Jekyll doesn't officially support Windows but does
have a pretty comprehensive set of instuctions on how to get 
[Jekyll working](https://jekyllrb.com/docs/windows/).

### Installing Jekyll

Initially I decided to try and get Jekyll running inside a [Docker](https://www.docker.com/) container. I've been using
Docker successfully on Linux and wanted to see what the Windows install would be like. This would have been cool
as I would've been able to mirror the environment I'd be deploying to. I seemed to encounter a few issues during setup
but after some trial and error Docker appeared to be working correctly. Around the same time though I started to run
into problems with my machine and ended up having to replace a few parts. I'm not sure the two were related but didn't
want to risk it the second time around.

Eventually I went with the "Installation via RubyInstaller" approach. This allowed me to use Ruby without having to do 
anything with Windows Subsystems. Once it was all set up getting Jekyll installed was as easy as running

```shell
gem install jekyll bundler
```

### Deploying to Github Pages

This was very straight-forward. All that was needed was to create a repo under my Github account with a name in the
format &lt;username&gt;.github.io. Github then monitors any changes made to the master branch and generates the site 
for you.

### Config

My site is pretty basic so far and I've been using default conventions where possible. All of my styles are inside the 
_sass folder. Here I've used the ITCSS method to manage the hierarchy. Additional settings have been added to the 
_config.yml, which currently looks like this:

```yaml
defaults:
  - scope: 
      path: ""
      type: "posts"
    values:
      layout: "post"
  - scope:
      path: ""
      type: "pages"
    values:
      layout: "default"
  - scope:
      path: "tag"
      type: "pages"
    values:
      layout: "tag"
  - scope:
      path: "cv"
    values:
      cv: true
description: "The thoughts and experiences of ..."
sass:
  style: compressed
permalink: pretty
```

The defaults allow me to set common variables for different page types. Here I'm setting which layout should be used for
each. I'm also adding the variable cv:true to anything inside the cv folder. This is how you get around not being
able to add front matter directly to static files. 

I'm using the description as a default meta description if one isn't added at the page lavel. 

The sass setting is pretty self-explanatory.

Setting permalink to pretty allows me to have URLs without file extensions.

This was enough to get me up-and-running.