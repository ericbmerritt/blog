---
layout: post
title: JSON Parser
date: 2007-01-10
comments: false
---

{{ page.title }}
================

In the course of developing [tercio](http://code.google.com/p/tercio)
I built a fast little json parser/encoder. Well I decided to use it in
[sinan](http://code.google.com/p/sinan) as well so I moved it to its
own project. The project, [ktuo](http://code.google.com/p/ktuo), is
hosted in the usual place. It outputs strict json, but it parses a
small superset of json. It allows single words as atoms in the json
string as an alternative to a single word enclosed in quotes. This
allows the lib to be used as a config parser. It makes the config much
more readable. In any case, the library is usable and available now.
