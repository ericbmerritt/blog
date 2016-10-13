---
layout: post
title: The Proper Use of Dialyzer in Rebar Projects
---

{{ page.title }}
================

The Dialyzer is a static analysis tool that identifies software
discrepancies using a Hindly Milner style system called
[Success Typing](http://www.it.uu.se/research/group/hipe/papers/succ_types.pdf).
Basically, it brings some of the benefits of Haskell or Ocaml style
type checking to Erlang. If you are not using it as part of the
automated build for your system you should be.

Dialyzer does it's thing going through all of your direct and
transitive dependencies and pulling out lots of information about
types, function calls etc. It then checks that that 'type universe' is
consistent with the calls you are making in your target. However, All
this pulling out of type information takes quite a long time so
Dialyzer has been given the ability to cache this information in
files called PLT files.

While PLT files are wonderful for saving you time and you should use
them. You must be careful about how you use them. In fact, most people
are using them in ways that will cause them problems in the long
run. Its common to have a 'Global' PLT file, either shipped with the
Erlang distribution or in the users home directory. Using Global PLT
files is a lot like using global state in your code. Its a practice
that leads to subtle and hard to debug errors.

Why is that? Well it has a lot to do with the fact that in the post
Rebar world version numbers don't actually mean what they should they
should. Huh? what does that have to do with Dialyzer? Give me a
second and I will explain. A lot of folks out there have the
dependencies in their `rebar.config` pointing to a branch or other
mutable `ref` instead of a tag. This means that every time they do a
`rebar get-deps` and pull down their deps (maybe after a clean) they
have slightly different code under the same version number. That is,
the version number is specified in the `*.app` or the `*.app.src` and that
doesn't change but with every new commit the *code* associated with
that version number changes. That pretty much eliminates the value of
version numbers as useful identifiers. Unfortunately, Dialyzer and
several other OTP tools rely on those version numbers. So lets take
the case where you have a Global PLT file where you keep the cached
information about OTP Applications. Internally, this file is organized
by application and version. However, every single one of projects you
are currently working on you work on (if you are depending on branches
or head instead tags) are working on subtly different implicit
versions for each dependency, all identified by an invalid explicit
version that Dialyzer is using.

You may go a long time without this causing you problems, but at some
point Dialyzer is going to think your code is wrong based on what it
knows about an App but that App is different from the one you have as
a dependency and so the warning is invalid. You wont know that though
and you will spend a ton of time trying to figure out what is wrong
and eventually give up and ignore the warning in your mind or maybe
even stop using Dialyzer.

Fixing The Problem
------------------

How do we fix this? We could do this by relying on explicit tags
everywhere and making sure that we don't pollute our version space but
thats hard to do. Its much simpler and safer to solve this problem by
treating the PLT file just like any other compile output. It should be
generated on a per project basis and kept in the 'build' area of the
project. Yes, that means it should be regenerated for each project,
and that takes a little while but doing it this way solves all the
problems described above in a clean and simple way. You probably
already have a `Makefile` driving Rebar so lets leverage that `Makefile`
to do the Dialyzer magic in simplest and least intrusive way
possible. Basically, we will set up a couple of tasks to do what we
need. The first task is a simple rule that creates the PLT file, the
second task runs Dialyzer on the Rebar source.

    $(DEPSOLVER_PLT):
            - dialyzer --output_plt $(DEPSOLVER_PLT) --build_plt \
                    --apps erts kernel stdlib crypto public_key -r deps

    dialyzer: $(DEPSOLVER_PLT)
            dialyzer --plt $(DEPSOLVER_PLT) -Wrace_conditions --src src


Another thing to note. Dialyzer will often fail on your
dependencies. There are too many fools out there that are not using
dialyzer on their project. These rules take that fact of life into
account, but you will see error messages from the PLT file
creation. You can ignore those, its not your responsibility to fix
errors in your upstream (though you can and submit them back, its
always good to be a contributor to your dependencies).

Thats it. You want to change the `--apps` to a list of actual
dependencies for your project rather then the arbitrary list here. A
Dialyzer specific example makefile is
[here](https://gist.github.com/3600078). In my next post, I will put
up a simple general Makefile that you can use to run a Rebar project.
