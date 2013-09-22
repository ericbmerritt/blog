---
layout: post
title: Configurations
date: 2007-03-12
comments: false
---

{{ page.title }}
================

OTP applications have their own configuration mechanism in the
[app config](http://www.erlang.org/doc/doc-5.5.3/doc/design_principles/applications.html#7.8)
structure. However, this doesn't always suit every applications
needs. Currently, I have two distinct config mechanisms right now, one
for [sinan](http://code.google.com/p/sinan/) and one for
[tercio](http://code.google.com/p/tercio/). They share a lot of
similarities and I suspect other applications share similar needs. To
that end I have split the config system out into its own project,
[fconf](http://code.google.com/p/fconf/) with the intention of using
it in both projects. There isn't much out there yet but I should have
the config subsystem pulled out of sinan and refactored in the next
day or so. There are a few features that I want to support.

- Reloadable config files (maybe auto reloaded)
- Config file syntax agnostic; use the syntax that is right for your user community
- Good, well defined override semantics

Of course, it all needs to be robust and scalable as well. The
existing sinan config subsystem supplies most of this. It needs to be
converted over to gen\_server, create a supervision tree, and convert
it to an OTP application.
