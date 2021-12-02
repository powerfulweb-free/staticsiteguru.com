---
author: "Sean Emerson"
title: Hugo Module replacements
lead:
draft: false
date: 2021-12-02
description: ""
images: [7904.png]
categories: ["Hugo Modules", "Hugo Themes"]
tags: ["config", "modules", "Themes"]
archives: [2021/11]
---
One of the challenges of working with modules whether it’s a  component or a theme is local development. Committing and then updating the dependency is a very time consuming workflow, and so is creating a local copy and changing your configuration.

There is an option of environment variables - the downside to this approach is you need to configure this locally, and if cross platform support is required, you need to use npm packages to set the NODE_ENV.

Luckily with Hugo 0.77.0 there is now a module replacements option.

The format is as follows

```YAML
# config.yaml
module:
  # replacement dir is relative to the theme folder
  # this example is a project which sits along side the current project
  replacements: github.com/user/repo -> ../../project
  # import your repo here
  imports: github.com/user/repo
```

The problem with this config file, is that if you push this site to netlify, it will also look in the local folder for the module and your build will fail.

The solution to this is using environment specific config folders

```YAML
# config/_default/config.yaml
module:
  # import your repo here
  imports: github.com/user/repo
```

```YAML
# config/development/config.yaml
# this config file will only run in the development 
# environment (when your running hugo server)
module:
  # replacement dir is relative to the theme folder
  replacements: github.com/user/repo -> ../../project
```

Remember to initialise your base project with `hugo mod init github.com/user/baseproject`, `hugo mod tidy`.  

I also have posts on [setting up modules]({{< relref "modules/index.md" >}}), and [creating your own modules]({{< relref "own-modules/index.md" >}}).

Happy coding

Further reading

https://gohugo.io/hugo-modules/configuration/

https://gohugo.io/getting-started/configuration/#configuration-directory
