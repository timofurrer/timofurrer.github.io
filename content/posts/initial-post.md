---
title: "Initial Post"
date: 2021-10-22T09:03:16+02:00
tags: [ "initial", "post" ]
categories: [ "python", "meta" ]
draft: false
---


Hello World[^1], this is my initial post[^bignote].

Welcome

This is some awesome code block:

```python
from re import match

x = match("foo", "foobar")
if x:
    print("Hello World)
```
whuat

{{< highlight go "linenos=table,hl_lines=8 15-17,linenostart=199" >}}
// GetTitleFunc returns a func that can be used to transform a string to
// title case.
//
// The supported styles are
//
// - "Go" (strings.Title)
// - "AP" (see https://www.apstylebook.com/)
// - "Chicago" (see https://www.chicagomanualofstyle.org/home.html)
//
// If an unknown or empty style is provided, AP style is what you get.
func GetTitleFunc(style string) func(s string) string {
  switch strings.ToLower(style) {
  case "go":
    return strings.Title
  case "chicago":
    return transform.NewTitleConverter(transform.ChicagoStyle)
  default:
    return transform.NewTitleConverter(transform.APStyle)
  }
}
{{< / highlight >}}

[^1]: quite standard

[^bignote]: what is this, awesome

    Just great and awesome

    `{ bra }`

    sdfg