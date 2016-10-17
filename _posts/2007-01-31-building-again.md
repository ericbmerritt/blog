---
layout: post
title: Building Again!
date: 2007-01-31
comments: false
tags: [erlang,sinan,build,progress]
---

I finally finished refactoring the build task tonight. For the first
time, the latest version of the build system is building from
source. It took a large amount of effort to get the dependent tasks to
this point. The Engine, Discover, Depends, the Repo puller, all of it
was just preparation for the builder. In any case, I am very pleased.

For the moment it just builds `*.erl` and `*.yrl` but eventually I want to
support all of the Erlang Compilables. I suspect that refactoring the
rest of the tasks (test, release, tar, clean, etc.) won't take much
longer. Hopefully, I will be able to do an alpha release at the end of
this week. I certainly hope so in any case.
