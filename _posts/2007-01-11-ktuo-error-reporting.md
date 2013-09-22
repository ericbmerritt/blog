---
layout: post
title: Ktuo Error Reporting
date: 2007-01-11
comments: false
---

{{ page.title }}
================

I have vastly increased the usefulness of error reporting in
ktuo. Before it just failed with badmatch if an error occurred. Now it
gives a useful error message along with the line and column number of
the problem. Now that I am using the library as a config parsing lib
in a couple of places it made sense that it report error usefully.
