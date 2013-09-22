---
layout: post
title: Build Flavors
date: 2007-04-08
comments: false
---

{{ page.title }}
================

I had quite a few requests to support different types of builds within
the same project. Usually the request centered around being able to do
'development' and 'release' builds. In these two cases, development
would enable debugging information and unit tests while release would
strip them out. Providing static development and release build flavors
wouldn't really solve the underlaying problem, which is the need to
parameterize the build process. I ended up solving this by adding a
'flavors' option to the build config and having the tasks take
arguments. Together these two features should allow a pretty wide
range of build customizations.

Following is the flavors entry in the default build config. Its pretty
self explanatory, 'default\_flavor' indicates which flavor should be
used when the user doesn't specify a flavor. 'flavors' is assigned to
the build flavor definition. Within each flavor you assign an argument
to each build task.

    default_flavor: development,
    flavors : {
        development : {
            build : "+debug_info -W1"
        },
        release : {
            build : "-DNOTEST=1 -W1"
        }
    }

Right now only the build and test tasks take arguments. However, over
time the other tasks that need arguments will take them as well. On a
side note, I have created a module that trys to parse out erlc
arguments.
