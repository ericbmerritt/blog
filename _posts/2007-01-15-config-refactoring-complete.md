---
bg: 'apollo.png'
layout: post
title: Config Refactoring Complete!
date: 2007-01-15
comments: false
tags: [erlang,config]
---

I just finished refactoring the config support in Sinan using the new
Ktuo package. It works well and makes the config much more readable.

Original config with Erlang terms

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

Well, I think it is more readable and more maintainable with the
nested namespaces.