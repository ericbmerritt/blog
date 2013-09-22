---
layout: post
title: Config Refactoring Complete!
date: 2007-01-15
comments: false
---

{{ page.title }}
================

I just finished refactoring the config support in sinan using the new
ktuo package. It works really well and makes the config much more
readable.

Original config with erlang terms

    [{repositories, ["http://repo.metadrift.net/repo"]},
      {build_dir, "_build"},
      {ignore_dirs, ["_", "."]},
      {ignore_apps, []},
      {default_task, build},
      {tasks, [{discover,
      [{handler, sin_discover}]},
       {verify, [{depends, [discover]},
       {handler, sin_verify}]},
       {build, [{depends, [verify]},
       {handler, sin_erl_builder}]},
       {doc, [{depends, [build]},
       {handler, sin_edoc}]},
       {tar, [{depends, [build]},
       {handler, sin_tar}]}]}].

new, more readable, config


    repositories : ["http://repo.metadrift.net/repo"],
    build_dir : _build,
    ignore_dirs : ["_", "."],
    ignore_apps : [],
    default_task : build,
    tasks : {
       discover : {
           handler : sin_discover
       },
       verify : {
         depends : [discover],
         handler : sin_verify
      },
      build : {
         depends : [verify],
         handler : sin_erl_builder
      },
      doc : {
          depends : [build],
          handler : sin_edoc
      },
      tar : {
          depends : [build],
          handler : sin_tar
      }
    }

Well, I think its more readable and more maintainable with the nested
namespaces.
