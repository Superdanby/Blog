---
title: "Create a New Hugo Site"
date: 2018-03-24T17:46:50+08:00
categories: [Hugo]
tags: [Hugo, Fedora]
toc: true
math: false
---

# Prerequisites

`hugo`

## Install Hugo on Fedora

~~`sudo dnf install hugo`~~
1.	`sudo dnf install snap`
2.	`snap install hugo`

# Initial Setup

1.	`hugo new site` ***PROJECT NAME***
2.	`cd` ***PROJECT NAME***
3.	`git init`

# Setup a Github Repository

1.	Create a new repository
2.	Repository settings > Source: master branch /docs folder
3.	`git remote add origin` ***REPO ADDRESS***: usually `git@github.com:...git`
4.	`git pull origin master`

# Setup Custom Theme: [zmd](https://github.com/amoshyc/hugo-zmd-theme)

1.	Fork the Github repository.
2.	`git submodule add https://github.com/amoshyc/hugo-zmd-theme themes/zmd`
3.	Backup `config.toml` and replace it with `themes/zmd/exampleSite/config.toml`
4.	Backup `archetypes/default.md` and replace it with`themes/zmd/exampleSite/archetypes/default.md`
5.	`mkdir content/abouts`
6.	Place your profile picture in `content/abouts`

## There are some settings needs to be modified:

-	`config.toml`: blog base URL
-	`themes/zmd/layouts/_default/baseof.html`: profile picture `abouts/me.jpg`
-	`themes/zmd/layouts/partials/header.html`: profile picture `abouts/me.jpg`

# Reduce Upload Process

Paste the following into a makefile:

{{< highlight sh "noclasses=false" >}}
MSG = "Build at $(shell /bin/date '+%Y-%m-%d %H-%M-%S')"
upload:
	rm -rf ./docs && hugo
	git add -A
	git commit -m $(MSG)
	git push origin master
{{< / highlight >}}

`make upload`: build and upload all content to Github.

# Original Post

[https://amoshyc.github.io/blog/abouts/site.html](https://amoshyc.github.io/blog/abouts/site.html)
