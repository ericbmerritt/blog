---
layout: post
title: Building Again!
date: 2007-01-31
comments: false
---

{{ page.title }}
================

I finally finished refactoring the build task tonight. For the first
time the latest version of the build system is actually building
source. It took a huge amount of effort to get the dependent tasks to
this point. The engine, discover, depends, the repo puller, all of it
was just preparation for the builder. In any case, I am very pleased.

For the moment it just builds *.erl and *.yrl but eventually I want to
support all of the erlang compilables. I suspect that refactoring the
rest of the tasks (test, release, tar, clean, etc) wont take much
longer. Hopefully, I will be able to do an alpha release at the end of
this week. I certainly hope so in any case.
