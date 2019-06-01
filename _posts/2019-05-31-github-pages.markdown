---
layout: post
title:  "Github Pages - Custom Themes"
permalink: "github-pages"
date:   2019-05-31 01:00:00 +0000
categories: github, jekyll, ruby
---
My first problem post involves setting up this blog.
&nbsp;

# Introduction

I'm using [Jekyll][jekyll] with [jekyll-theme-console][jekyll-theme-console], which is unsupported by GitHub Pages.

See the supported themes [here][supported-themes].
&nbsp;

On pushing a Jekyll site to: `github.com/<username>/<username>.github.io`, GitHub will host the site for you within the next 5-10 minutes at: `<username>.github.io`.

# Issue

After my pushes, I was just seeing the content as it is in the markdown files, but with no theme. The homepage was mostly blank, like I was viewing a text file.
At first, I thought it was because I was missing the [github-pages gem][github-pages-gem], but that didn't seem to help.

Had I checked my emails sooner...:
&nbsp;

> You are attempting to use a Jekyll theme, "jekyll-theme-console", which is not supported by GitHub Pages...

# Resolution

To get around this, you'll need to use the [remote-theme][remote-theme] gem. This loads in the theme remotely from a GitHub repository.
&nbsp;

1) Add it to your Gemfile
2) Remove/uncomment your current theme
3) Add `remote_theme: <username>/<repository>` (e.g. `remote_theme: b2a3e8/jekyll-theme-console`)
4) Update and test
&nbsp;

Check the [source][source] for this site if you get stuck, or give me a shout.
&nbsp;

Thanks,
&nbsp;

l

[jekyll]: jekyll-theme-console

[jekyll-theme-console]: https://github.com/b2a3e8/jekyll-theme-console/

[supported-themes]: https://pages.github.com/themes/

[github-pages-gem]: https://jekyllrb.com/docs/github-pages/

[remote-theme]: https://github.com/benbalter/jekyll-remote-theme/

[source]: https://github.com/eledw/eledw.github.io/
