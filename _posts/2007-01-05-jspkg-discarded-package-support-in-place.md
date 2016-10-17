---
bg: 'apollo.png'
layout: post
title: JsPkg Discarded, Package Support in Place
date: 2007-01-05
comments: false
tags: [web]
---

I decided not to use JsPkg. The code there needs a massive
refactoring, and it approaches the problem from a viewpoint that is
very different from my own. Instead, I wrote a simple Packager that
doesn't do the loading. So, for now, I have good namespacing support
and stubs for package loading. Once I get a better understanding of
the semantics of the applications built on this platform, I will start
filling out the loading stubs.
