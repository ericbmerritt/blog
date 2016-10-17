---
bg: 'apollo.png'
layout: post
title: Topological Sort Joy
date: 2007-01-26
comments: false
tags: [algorithms,sinan,build,erlang]
---

I finally modified [Sinan](http://code.google.com/p/sinan/) to make
use of all the dependency analysis code I wrote for ewrepo. One thing
that I needed to do was produce a list, in the dependent order, of the
graph of dependencies in the internal project applications. This list
of dependencies is essential if compile time things like parse
transforms are going to work correctly. I wrestled with how to do this
correctly for quite a bit before I realized that it is a [Topological
Sort](http://en.wikipedia.org/wiki/Topological_sorting). I should have
realized this immediately. Fortunately, this isn't a difficult
algorithm, especially since I didn't even need to implement it. Joe
Armstrong wrote a topo sort for his Ermake project and made it
available in the [contribs section of
erlang.org](http://www.erlang.org/user.html). Once I found the
Topological Sort code in Erlang, it didn't take too long to integrate
it into Sinan.

Once I got to thinking about this dependency problem, I realized that
there was at least one more area that a topological sort was needed. I
also need the Topological Sort in the run order of the task list. Each
task may have any number of Tasks that it depend on it. Right now my
code would just run each dependent task multiple times. That is a
bug. Fortunately, it won't take more than a few minutes to integrate
the topo sort into this area as well.