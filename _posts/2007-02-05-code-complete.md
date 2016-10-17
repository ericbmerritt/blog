---
bg: 'apollo.png'
layout: post
title: Code Complete!
date: 2007-02-05
comments: false
tags: [erlang,sinan,progress,build]
---

I finished up the last few tasks over the weekend. All the coding is
complete at this point. I still need to document the internals a bit
more, so that third-parties can write tasks. I also need to spend some
time writing high-level user documentation so getting up to speed on
the system is easier. All this will all take me a week or
so. Hopefully, I can then do a release.

On a side note, one of the things that I ended up doing was adding
code coverage metrics to the unit test running. The
[cover](http://www.erlang.org/doc/doc-5.5.3/lib/tools-2.5.3/doc/html/cover.html)
module made this possible. Unfortunately, it only provides the number
of runs for each line of executable code without any information about
global coverage percentages. When I get some time, I am going to poke
around in the system and see if I can come up with a way to get the
executable line count from a module. If I can get that information
then providing high-level project statistics won't be so bad.
