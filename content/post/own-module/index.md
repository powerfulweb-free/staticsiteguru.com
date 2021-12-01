---
author: "Sean Emerson"
title: Creating Your Own Hugo Modules
lead: Creating your own modules is a great way to avoid code duplication and make updating code easy.
draft: true
date: 2021-12-05
description: ""
images: []
categories: ["Hugo Modules", "Hugo Themes"]
tags: ["config", "modules", "Themes"]
archives: [2021/12]
---
<!-- add screen shots from github desktop -->
<!-- create shortcode for displaying images throughout post -->

Many users find Hugo Modules confusing, its a question that I have had quite a few times over on [my youtube channel](#).

Many users have followed the instructions over on the hugo docs, and it simply won't work, with Hugo throwing an error stating that the module cannot be find. 

Before you start make sure you know how to set up your base project to import modules correctly - see my previous post {{ < prev-post > }}.

## Initialise your sub-project as a module

Run `hugo mod init gihub.com/user/module-repo` in the root directory of the project which will become a module. This is the same step which is used for the base project. Don't get confused!

## Initialise your sub-project as a git repository

Run `git init` in the root directory of the project. 

## Add your new git repository to github (desktop) and publish. 

Ensure you have excluded files that you don't want to commit to git with a `.gitignore` file. 

For example:

```
# /.gitignore
node_modules/
.hugo_build.lock
resources/
```

Via the Github Desktop GUI (which I reccomend for beginners) go to File > Add local repository (ctrl/command + o). 

From here you need to make a commit on the left hand side (you will have to write a summary message) and then publish the repository on the top right. You will need to make the repository public for ease of use as a module, especially if building remotely with a service like netlify.

## Create a semantic version tag

Hugo modules need to be versioned using the [go semantic version standard](https://go.dev/doc/modules/version-numbers). 

For a version less than 2, its easy just provide a tag such as v0.0.1. 

Reaching version 2 is complicated. Version 0 (non-stable release) to version 1 is not considered a breaking change, as no guarantees are given when moving from version 0 to version 1. However when moving from version 1 to version 2 you are implying that there are breaking changes, and thus you need to copy your new version into a v2 folder in the root of your project. For more information see the [go semantic versioning page](https://go.dev/doc/modules/version-numbers).

The easist way to create a tag, is on github desktop, click on the history tab, and then right click on the most recent release at the top and click "create tag". Please not that once you have pushed this update to the remote repository, you cannot delete the tag with github desktop, you need to do a 2 step process (local and remote) [with the command line](https://devconnected.com/how-to-delete-local-and-remote-tags-on-git/).

## Get the module in your base project

Run the command `hugo mod get` to pull/get the module in your base project. Make sure you base project has been set up properly. See my previous post {{ < post-prev > }}.

## Update your module dependency after a new version has been published

Run `Hugo mod get -u` to update dependencies if they have changed.

## Replace your module with a local version for development. 

See my next post on module replacements {{ < next-post > }}.


