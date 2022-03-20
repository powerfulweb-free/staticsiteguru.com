---
author: "Sean Emerson"
title: Remote Resources
lead: Now you can pass remote resources into Hugo Pipes just like they are local resources
draft: false
date: 2021-12-10
description: ""
images: [remote.png]
categories: [hugo-pipes]
tags: [resources, remote]
archives: [2021/12]
---
As of Hugo v0.90.1 you can now fetch a remote resource and process it with Hugo Pipes just like it was a local resource.

To **fetch** a **local** resource (in the assets folder) and optionally run it through hugo pipes you would run the following code:

```go-html-template
<!-- fetch /assets/scss/main.scss -->
{{ $local := resources.Get "sass/main.scss" }}
{{ $css := $local | toCSS | postCSS | fingerprint }}
```

Now you can **fetch** a file from a **remote** server, and still run it through Hugo Pipes like this:

```go-html-template
{{ $remote := resources.Get "https://www.example.com/styles.scss" }}
{{ $css := $remote | toCSS | postCSS | fingerprint }}
```

Luckily in Hugo v0.90.1 resources.Get has been patched so that if the resource it not available, the resource will return `nil`. This will still create an error, but the error can be caught.

If you want to return a **warning** message, if the resource is not available you will have to handle the error like this: (check for `.Err` and catch it)

```go-html-template
{{ with resources.Get "https://gohugo.io/images/gohugoio-card-1.png" }}
  {{ with .Err }}
    {{ warnf "%s" . }}
    {{/* optionally provide a fallback local resource here if that's the behaviour that you want. */}}
  {{ else }}
    <!-- image is only displayed if remote image is downloaded, otherwise warning above -->
    <img src="{{ .RelPermalink }}" width="{{ .Width }}" height="{{ .Height }}" alt="">
  {{ end }}
{{ end }}
```

If you want to the build to fail with an **error** if the resource is not available, just `Get` the remote resource. Hugo will fail the build as you are not catching the `.Err` error that is generated.

```go-html-template
{{ with resources.Get "https://gohugo.io/images/gohugoio-card-1.png" }}
<!-- build will fail if the image does not download -->
<img src="{{ .RelPermalink }}" width="{{ .Width }}" height="{{ .Height }}" alt="">
{{ end }}
```

## Further Reading

<https://github.com/gohugoio/hugo/releases/tag/v0.90.1>

<https://github.com/gohugoio/hugo/releases/tag/v0.90.0>

<https://gohugo.io/hugo-pipes/introduction/>