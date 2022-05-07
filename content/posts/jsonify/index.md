---
author: "Sean Emerson"
title: Jsonify
lead:
draft: false
date: 2022-05-07
description: ""
images: [JSON_Blog_Hero.png]
categories: ["Hugo Functions"]
tags: [jsonify]
archives: [2022/05]
anchor: Center
---
The little known Hugo function `jsonify` can be used to output JSON formatted code very easily. Common use cases are an Index for a JavaScript site search, or [Structured Markup (JSON-LD)](https://developers.google.com/search/docs/advanced/structured-data/intro-structured-data).

<!--more-->
Rather than painstakingly templating JSON formatted text using variables, you can add all of your data to a `dict` or scratch map via `scratch.SetInMap`. You then run the `jsonify` function to convert the dict/map to a json object.

By default the JSON formatted code is outputted in a somewhat minified form. Luckily the [Hugo Docs](https://gohugo.io/functions/jsonify/) have included a parameter for indentation (set it to your preferred indentation - 2 or 4 spaces), so that the code is easy to read. See the following 2 examples (based on the first few lines of some example structured markup for a local business):

Source: <https://developers.google.com/search/docs/advanced/structured-data/local-business>

The following $data is used for each example:

```HTML
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

```HTML
<script type="application/ld+json">
{{ $data | jsonify | safeJS }}
</script>
```

Output:

```HTML
<script type="application/ld+json">
  {"@context":"https://schema.org","@type":"Restaurant","image":["https://example.com/photos/1x1/photo.jpg","https://example.com/photos/4x3/photo.jpg","https://example.com/photos/16x9/photo.jpg"],"name":"Dave's Steak House"}
</script>
```

Input:

```HTML
<script type="application/ld+json">
{{ $data | jsonify (dict "indent" "  ") | safeJS }}
</script>
```

Output:

```HTML
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

```HTML
{{ $data | jsonify (dict "indent" "  ") }}
```

Output:

```HTML
<script type=application/ld+json>{"@context":"https://schema.org","@type":"Restaurant","image":["https://example.com/photos/1x1/photo.jpg","https://example.com/photos/4x3/photo.jpg","https://example.com/photos/16x9/photo.jpg"],"name":"Dave's Steak House"}</script>
```
