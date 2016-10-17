---
layout: post
title: More Progress!
date: 2007-01-31
comments: false
tags: [erlang,sinan,progress,build]
---

I resurrected the `test`, `edoc`, `clean`, and `release` tasks
tonight. Everything works as it should except `edoc`. There seems to
be a conflict between `edoc` and `xmerl` in the newest version of
Erlang (R11B-3). I hope to figure out what the problem is and resolve
it soon.

On a side note. I came across the
[cover](http://www.erlang.org/doc/doc-5.5.3/lib/tools-2.5.3/doc/html/cover.html)
module in the tools application of Erlang. Since I was working on the
unit tests task at the time it seemed like a good idea to add this
functionality to the build system either via a code coverage task or
integration into the existing unit tests task. The only real issue is
that all the dependent modules need to be recompiled with coverage
information to make cover work. That recompile requirement adds quite
a bit of complexity to the feature. In any case, it's on my list of
new tasks to add.

I added one additional task, `analyze`. It seems the dialyzer folks
finally made it controllable from Erlang code. That allowed me to
integrate dialyzer into Sinan. Now running dialyzer will be as simple
as running a build task. Hopefully, if dialyzer is easier to use it
will get used more often.