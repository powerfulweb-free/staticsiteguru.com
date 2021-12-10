---
author: "Sean Emerson"
title: Remote Resources
lead: Now you can pass remote resources into Hugo Pipes just like they are local resources
draft: true
date: 2021-12-11
description: ""
images: []
categories: [hugo-pipes]
tags: [resources, remote-resources, resources.get]
archives: [2021/12]
---
As of Hugo v0.90.1 you can now fetch a remote resource and process it with Hugo Pipes just like it was a local resource.

To fetch a local resource (in the assets folder) you would run the following code:
```HTML
<!-- fetch /assets/scss/main.scss -->
{{ $local := resources.Get "sass/main.scss" }}
```

Now you can fetch a file from a remote server, and still run it through Hugo Pipes like this:
```HTML
{{ $remote := resources.Get "https://www.example.com/styles.scss" }}
{{ $css := $remote | toCSS | postCSS | fingerprint }}
```

Luckily in Hugo v0.90.1 resources.Get has been patched so that if the resource it not available, resources.Get will return `nil`, rather than failing the build. You can then handle the `nil` return with a conditional stated for example:

```HTML
{{ with resources.Get "https://gohugo.io/images/gohugoio-card-1.png" }}
    <img src="{{ .RelPermalink }}" width="{{ .Width }}" height="{{ .Height }}" alt="">
  {{ else }}
    <!-- fall back local image -->
    <img src="{{ resources.Get "images/local-card-1.png" }}" width="{{ .Width }}" height="{{ .Height }}" alt="">
  {{ end }}
{{ end }}
```

If you want to return a warning message, if the resource is not available you will have to handle the error like this: (check for `.Err`)

```HTML
{{ with resources.Get "https://gohugo.io/images/gohugoio-card-1.png" }}
  {{ with .Err }}
    {{ warnf "%s" . }}
  {{ else }}
    <!-- image is only displayed if remote image is downloaded, otherwise warning above -->
    <img src="{{ .RelPermalink }}" width="{{ .Width }}" height="{{ .Height }}" alt="">
  {{ end }}
{{ end }}
```

If you want to the build to fail if the resource is not available, just `Get` the image, without the use of a conditional statement, Hugo will fail the build the first time that you start using the failed `Resource` object. e.g.

```HTML
{{ $img := resources.Get "https://gohugo.io/images/gohugoio-card-1.png" }}
<!-- build will fail if the image does not download -->
<img src="{{ $img.RelPermalink }}" width="{{ .Width }}" height="{{ .Height }}" alt="">

```
