---
author: "Sean Emerson"
title: "Module configuration merging in Hugo v0.84.0"
draft: false
date: 2021-11-17
description: ""
images: [hugo.png]
categories: ["Hugo Modules", "Hugo Themes"]
tags: ["config", "modules", "Themes"]
archives: ["2021/11"]
---

Since Hugo v0.84.0 you can now use the configuration folder when authoring modules.

In addition to this, Hugo now performs a deep merge of the parameters.

So what exactly is deep merging?

Consider the following example module configuration:

```TOML
# config.toml
[params]
[params.colours]
blue="#337DFF"
green="#68FF33"
red="#FF3358"
```

OR

```YAML
# /config.yaml
params:
  colours:
    blue: "#337DFF"
    green: "#68FF33"
    red: "#FF3358"
```

Before Hugo v0.84.0, to 1 of the theme/modules params in your local config, you needed to specify this whole block, and then change the 1 param. For example:

```YAML
# /config.yaml
params:
  colours:
    blue: "#337DFF"
    green: "#68FF33"
    # only the following line is changed
    red: "#FF3344"
```

Now you only need to copy and paste and modify the param that you wish to override e.g.

```YAML
# /config.yaml
params:
  colours:
    red: "#FF3344"
```

Much easier, and brings hugo in line with other languages!

Even better, you can now use the config directory. If you use the config directory for your website, you can then copy and paste the params directly across before you override the params.

e.g.

```YAML
# /config/_default/params.yaml
colours:
  blue: "#337DFF"
  green: "#68FF33"
  red: "#FF3358"
```
