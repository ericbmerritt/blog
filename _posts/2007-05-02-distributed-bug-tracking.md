---
layout: post
title: Distributed Bug Tracking
date: 2007-05-02
comments: false
---

{{ page.title }}
================

I came across a project called
[DisTract](http://www.distract.wellquite.org/) last week. DisTract is
basically a distributed bug tracking system. Specifically, its a file
based bug tracking system who's directory structure sits inside of a
repository managed by a distributed version control system. It uses
this distributed version control system to manage distribution. This
system was preceded by another, somewhat bit rotted, system called
[Bugs Everywhere](http://www.panoramicfeedback.com/opensource/).

I really like the idea of distributed bug tracking. It fits in really
well with distributed version control and over all distributed
development. Being able to create new branches for your bug system
along with it source. Merge those branches back into mainline all the
while keeping history etc. Thats a very, very powerful thing. However,
this approach has a fundamental, maybe intractable, problem when it
comes to merging. In a version control system merging is relatively
simple. Files identifiers (basically the path in the workspace) are
fixed there is no ambiguity if the same file is changed on two
different boxes. Its just a matter of merging the contents of that
file. Distributed bug tracking still has the problem of merging
changed bugs. However, it has an additional problem. That is the fact
that there is now why to relate on bug created by one person to a
different bug created by another person. This is a problem that every
bug tracking system has to some extent. In a distributed bug tracking
system its even worse because the bugs created by another person wont
be visible until they get a push or do a pull from that other persons
repository. There may be a way to solve the problem using some of the
modern document similarity algorithms. However, considering the small
amount of text usually supplied with a bug report this is probably
unworkable. I don't have a solution to this yet but I may play around
with some ideas and use some information form some of the larger
public repositories to run some tests.
