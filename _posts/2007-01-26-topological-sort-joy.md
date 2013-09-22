---
layout: post
title: Topological Sort Joy
date: 2007-01-26
comments: false
---

{{ page.title }}
================

I finally modified [sinan](http://code.google.com/p/sinan/) to make
use of all the dependency analysis code I wrote for ewrepo. One thing
that I needed to do was produce a list, in the dependent order, of the
graph of dependencies in the internal project applications. This is
essential for compile time things like parse transforms are going to
work correctly. I wrestled with how to do this correctly for quite a
bit before I realized that its a simple
[topological sort](http://en.wikipedia.org/wiki/Topological_sorting). I
should really have realized this immediately, but I
didn't. Fortunately, this isn't a difficult algorithm, especially
since I didn't even need to implement it. Joe Armstrong wrote a topo
sort for his ermake project and made it available in the
[contribs section of erlang.org](http://www.erlang.org/user.html). Once
I found that it didn't take to long to integrate it into sinan.

Once I got to thinking about this dependency problem I realized that
there was at least one more area that a topological sort was
needed. Thats in the run order of the task list. Each task may have
any number of tasks that it depends on. Right now my code would just
run each dependent task multiple times. Thats a bug, fortunately it
wont take more then a few minutes to integrate the topo sort into this
area as well.
