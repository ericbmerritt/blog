---
layout: post
title: Dependency Checking Complete!!
date: 2007-01-23
comments: false
---

{{ page.title }}
================

I finally finished up both the shallow and the deep dependency
resolution for sinan. This took a bit of time and effort to
conceptualize, abstract and implement.

Now given a list of apps and the dependencies (along with the url(s)
of the repo); like this

     [{app1, "0.1", [{edoc, 'LATEST'},
      {syntax_tools, "1.5.0"}]}

It correctly returns a list of all the dependencies required for the
project; like this


    [{stdlib, "1.14.2",
       [compiler,edoc,syntax_tools]},
     {syntax_tools,"1.5.0",
       [app1,edoc]},
     {compiler,"4.4.2",
        [edoc]},
     {kernel,"2.11.2",
        [compiler,stdlib,edoc]},
     {edoc,"0.6.9",
        [app1]},
     {app1,"0.1",[]}]

If it runs into a version conflict it reports what the conflict is and
what applications are causing it.

In the output above. Each entry is a dependency, its version and a
list of apps the depend on that app.
