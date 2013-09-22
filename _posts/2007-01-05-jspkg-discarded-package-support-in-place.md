---
layout: post
title: JsPkg Discarded, Package Support in Place
date: 2007-01-05
comments: false
---

{{ page.title }}
================

I decided to not use jspkg. The code there needs a massive refactoring
and it approaches the problem from a viewpoint that is very different
from my own. In stead, I wrote a simple packager that doesn't do
loading. So for now I have good namespacing support and stubs for
package loading. Once I get a better understanding of the semantics of
the applications built on this platform I will start filling out the
loading stubs.
