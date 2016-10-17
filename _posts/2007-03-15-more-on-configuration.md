---
layout: post
title: More on Configuration
date: 2007-03-15
comments: false
tags: [erlang,configuration]
---

The first, lightly tested, version of
[fconf](http://code.google.com/p/fconf/) is out. At the moment it is
undocumented, but that should change pretty quickly. It is an OTP app
that supports multiple simultaneous configurations and merging configs
together. It supports everything that I talked about in my last
[post](http://erlangish.blogspot.com/2007/03/configurations.html). I
already ported Sinan over to Fconf from its built-in configuration
system.