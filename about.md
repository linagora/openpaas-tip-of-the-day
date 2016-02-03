---
layout: page
title: About
permalink: /about/
---

Here you will find a great daily OpenPaas tip that will learn you everything about this very big project.

### How to contribute?
Do not hesitate to send a **beautiful PR**!
Follow those [instructions](https://github.com/linagora/openpaas-tip-of-the-day/blob/gh-pages/CONTRIBUTING.md).

### How to setup a local jekyll and see your post!
This blog is powered by the awesome [jekyll-now](https://github.com/barryclark/jekyll-now).
You need to install `github-pages`:

```sh
gem install github-pages
```

Then add a **Gemfile** file at the root of the repository with this content:

```
source 'https://rubygems.org'
gem 'pygments.rb'
gem 'github-pages'
```

Finally run:

```sh
gem install bundler
bundle install
jekyll serve
```

And browse at *http://127.0.0.1:4000/*

>*Don't forget to Star the repository!*
