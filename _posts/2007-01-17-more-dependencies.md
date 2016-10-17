---
bg: 'apollo.png'
layout: post
title: More Dependencies
date: 2007-01-17
comments: false
tags: [erlang,sinan,json]
---

Most people think that Erlang has just runtime dependencies. For the
most part this is true. However, Erlang also has two types of compile
time dependencies. The first is the `.hrl` files that are included in
a module. The second type is `.erl` files that implement a parse
transform. Both of these types of files represent compile time
dependencies. For the most part its simple enough to make sure the
include and code path information is set. However, its much more
difficult to make sure that if one of these types of files change all
of its dependencies are changed as well. I think that the easiest way
to handle this would be to look at the AST of a compiled file during
the compilation process and extract the Includes and the parse
transform information from that AST. Unfortunately, it means knowing
much more about a dependent OTP application then the build system
currently knows. It would definitely need to happen after the high
level runtime dependency mapping takes place. I think, for now, I will
stick with just handling the runtime dependencies and making sure that
the build time dependencies have the right path information
available. Once the system gets out in the wild and I start getting
feedback I may modify it to be a little smarter about the compile time
dependencies.
