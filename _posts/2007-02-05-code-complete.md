---
layout: post
title: Code Complete!
date: 2007-02-05
comments: false
---

{{ page.title }}
================

I finished up the last few tasks over the weekend. This means that all
the coding is pretty complete at this point. I still need to document
the internals a bit more, so third party tasks can be written. I also
need to spend some time writing high level user documentation so
getting up to speed on the system is strait forward. All this will all
probably take me a week or so. Hopefully, I can then do a true
release.

On a side note, one of the things that I ended up doing was adding
code coverage metrics to the unit test running. The
[cover](http://www.erlang.org/doc/doc-5.5.3/lib/tools-2.5.3/doc/html/cover.html)
module made this possible. Unfortunately, it only provides the number
of runs for each line of executable code without any information about
global coverage percentages. When I get some time I am going to poke
around in system and see if I can come up with a way to get the
executable line count from a module. If I can get that information
then providing high level project statistics wont be so bad.
