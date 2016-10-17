---
layout: post
title: Released - Ktuo 0.1.1
date: 2007-01-19
comments: false
tags: [erlang,json,ktuo]
---

I just released the first alpha version of my JSON Parser/Encoder
Ktuo. I use it in several projects with no problems at all. However,
this is still an alpha release, so be aware that you may run into
issues. The download area for Ktuo is
[here](http://code.google.com/p/ktuo/downloads/list). A bit of
documentation for the project is
[here](http://code.google.com/p/ktuo/wiki/Usage).

I know the question of why another JSON parser is going to come up, so
I am just going to go ahead and address it now. The [existing JSON
parser](http://www.erlang-projects.org/Public/news/ejson/view) is
still available and still useful. It makes elegant use of
continuations to handle cases where it may not have a full JSON
expression. The use of these Continuations makes it very flexible and
pretty darn attractive. However, these extra features also add
significant complexity and no small amount of additional resource
usage. While working on my [Tercio](http://code.google.com/p/tercio/)
project I realized that I didn't need these features and I didn't want
to pay the cost of the complexity and resource usage overhead. So I
wrote my own. Eventually, I realized it was useful in and of itself
and created a project space for it.
