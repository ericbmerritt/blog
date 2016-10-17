---
layout: post
title: JSON Parser
date: 2007-01-10
comments: false
tags: [erlang,json]
---

In the course of developing
[tercio](https://github.com/erlware-deprecated/tercio) I built a fast
little JSON parser/encoder. I decided to use it in
[sinan](https://github.com/erlware-deprecated/sinan) as well, so I
moved it to a new project. The project,
[ktuo](https://github.com/erlware-deprecated/ktuo), is hosted in the
usual place. It outputs strict JSON, but it parses a small superset of
JSON. It allows single words as atoms in the JSON string as an
alternative to a single word enclosed in quotes.  This feature allows
me to use the library as a config parser. It makes the config much
more readable. In any case, the library is usable and available now.