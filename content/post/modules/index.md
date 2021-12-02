---
author: "Sean Emerson"
title: Getting started with Hugo Modules
lead: Hugo modules will save you from replicating code, and help you manage dependencies across project much easier. 
draft: true
date: 2021-11-17
description: ""
images: [hugo056.png]
categories: ["Hugo Modules", "Hugo Themes"]
tags: ["config", "modules", "Themes"]
archives: [2021/11]
---
Many users find Hugo Modules confusing, its a question that I have had quite a few times over on {{< channel />}}.

Many users have followed the instructions over on the hugo docs, and it simply won't work, with Hugo throwing an error stating that the module cannot be find.

The most common mistake is failing to initialise the base project as a module.

## Base project operations

### Initialise your base project with Hugo Modules

To do this, in the root directory of the base project run the following command:

`hugo mod init github.com/user/baseproject`

### Configure Module Imports

You then need to configure your base project to import the module, with the following configuration:

```YAML
# config/_default/config.yaml or config.yaml
modules:
# import your repo here
imports: github.com/user/repo
```

### Fetch/Get the remote module

You then need to "get" the module with the following command.

`hugo mod get`

If you have removed modules from your config file, you can tidy it up with 'hugo mod tidy'.

At many points along this process hugo will advise you on what commands to use e.g. "run go mod tidy to tidy your go.mod file" - you need to replace `go` with `hugo` as we are running the commands via hugo.

### Update Modules

Later on you can update modules with 'hugo mod get -u'

By now everything should be working well, unless the module hasn't been versioned correctly. More on that in my next post {{< next-page >}}.

Futher reading...

https://gohugo.io/hugo-modules/configuration/

https://gohugo.io/hugo-modules/use-modules/
