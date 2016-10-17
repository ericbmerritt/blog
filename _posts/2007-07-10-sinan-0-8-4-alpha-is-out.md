---
bg: 'apollo.png'
layout: post
title: Sinan 0.8.4 Alpha is Out
date: 2007-07-10
comments: false
tags: [erlang,sinan,progress,build]
---

The [latest version](http://code.google.com/p/sinan/downloads/list) of
Sinan is out. This version adds support for spawning a new Erlang node
with shell for the current project. This node makes debugging and
exploratory programming much, much easier. It also spawns the node
with a sname `sinan_shell` so that the new shell will interact well
with Distel. This addition is a change that a lot of people have asked
for and I am pleased to be able to integrate it.

I am still having issues with the analyze task that I am working
on. However, I hope to have it fixed and working shortly.
