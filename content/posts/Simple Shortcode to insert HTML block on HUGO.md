---
title: "Simple Shortcode to Insert Raw HTML in Hugo"
date: "2020-07-15"
author: "Mauro"
tags: [ "Hugo", "Html", ]
---

I love the simplicity of creating new content using markdown in Hugo. But sometimes, markdown is not enough – you might want to do a one-off extra-fancy thing with raw html.

Surprisingly, Hugo doesn’t seem to have a good way to let you do this in a content file. Here is how to create your own one-line custom shortcode to make that possible.

Add a shortcode template to your site, in layouts/shortcodes/rawhtml.html, with the content:
```html
<!-- raw html -->
{{.Inner}}
```

This template tells Hugo to simply render the inner content passed to your shortcode directly into the HTML of your site.

You can then use this shortcode in your markdown content, like so:
```html
{{< rawhtml >}}
  <p class="speshal-fancy-custom">
    This is <strong>raw HTML</strong>, inside Markdown.
  </p>
{{< /rawhtml >}}
```

Links: [Ana Blog](https://anaulin.org/blog/hugo-raw-html-shortcode/)