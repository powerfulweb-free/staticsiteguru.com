---
author: "Sean Emerson"
title: Jsonify
lead:
draft: false
date: 2022-05-07
description: ""
images: [JSON_Blog_Hero.png]
categories: ["Hugo Functions"]
tags: [jsonify, safeJS]
archives: [2022/05]
anchor: Center
---
The little known Hugo function `jsonify` can be used to output JSON formatted code very easily. Common use cases are an Index for a JavaScript site search, or [Structured Markup (JSON-LD)](https://developers.google.com/search/docs/advanced/structured-data/intro-structured-data).

<!--more-->
Rather than painstakingly templating JSON formatted text using variables, you can add all of your data to a `dict` or scratch map via `scratch.SetInMap`. You then run the `jsonify` function to convert the dict/map to a json object.

You **must** pipe your code through `safeJS` as the JSON content inside the script tags is treated as js. All of the strings will be esacaped to be deepend safeJS. Running `safeJS` will bypass this safety feature.

By default the JSON formatted code is outputted in a somewhat minified form. Luckily the [Hugo Docs](https://gohugo.io/functions/jsonify/) have included a parameter for indentation (set it to your preferred indentation - 2 or 4 spaces), so that the code is easy to read. See the following 2 examples (based on the first few lines of some example structured markup for a local business):

I have also added an example where `hugo --minify` has been run, to minify the html output.

Source: <https://developers.google.com/search/docs/advanced/structured-data/local-business>

The following $data is used for each example:

```go-html-template
{{- $data := (dict 
   "@context" "https://schema.org"
    "@type" "Restaurant"
    "image" (slice 
      "https://example.com/photos/1x1/photo.jpg"
      "https://example.com/photos/4x3/photo.jpg"
      "https://example.com/photos/16x9/photo.jpg"
    )
    "name" "Dave's Steak House"
) -}}
```

Input:

```go-html-template
<script type="application/ld+json">
{{ $data | jsonify | safeJS }}
</script>
```

Output:

```go-html-template
<script type="application/ld+json">
  {"@context":"https://schema.org","@type":"Restaurant","image":["https://example.com/photos/1x1/photo.jpg","https://example.com/photos/4x3/photo.jpg","https://example.com/photos/16x9/photo.jpg"],"name":"Dave's Steak House"}
</script>
```

Input:

```go-html-template
<script type="application/ld+json">
{{ $data | jsonify (dict "indent" "  ") | safeJS }}
</script>
```

Output:

```go-html-template
<script type="application/ld+json">
  {
  "@context": "https://schema.org",
  "@type": "Restaurant",
  "image": [
    "https://example.com/photos/1x1/photo.jpg",
    "https://example.com/photos/4x3/photo.jpg",
    "https://example.com/photos/16x9/photo.jpg"
  ],
  "name": "Dave's Steak House"
}
</script>
```

Run `hugo --minify` Input:

```go-html-template
{{ $data | jsonify (dict "indent" "  ") }}
```

Output:

```go-html-template
<script type=application/ld+json>{"@context":"https://schema.org","@type":"Restaurant","image":["https://example.com/photos/1x1/photo.jpg","https://example.com/photos/4x3/photo.jpg","https://example.com/photos/16x9/photo.jpg"],"name":"Dave's Steak House"}</script>
```
