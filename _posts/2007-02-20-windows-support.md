---
layout: post
title: Windows Support
date: 2007-02-20
comments: false
---

{{ page.title }}
================

So far support for Windows has been, at most, an after thought for
me. I own only one rather old windows box that I use for the
occasional gaming. For that reason, windows just isn't a big
priority. However, after a discussion with Martin Logan we came to the
conclusion that sinan will need to support windows from the initial
release. Fortunately, I have used the
[filename](http://www.erlang.org/doc/doc-5.5.3/lib/stdlib-1.14.3/doc/html/filename.html)
and
[filelib](http://www.erlang.org/doc/doc-5.5.3/lib/stdlib-1.14.3/doc/html/filelib.html)
modules through out the implementation. That should remove any path
name issues between the two systems. Unfortunately, I use symlinks
pretty heavily through out the build. I have also used two very unix
specific os commands as part of development, uname and tar. At first I
thought that replacing these commands would pose a big problem. That
proves not to be the case as the stdlib and kernel applications
provide for my needs, though that functionality is pretty well hidden.

To figure out what platform and architecture I am on I use uname. I
need this to pull down the correct version of binary dependencies from
the repository. I didn't see any real solution to replace this
command. I finally ran across a reference to
erlang:system\_info(system\_architecture). This should solve my needs
pretty well. Unfortunately, it only works in R9 and above. So, as you
would expect, its a trade off. If I use this I can make the system
work in windows but not in pre R9 systems. I think the right choice is
to make it work in windows. Hopefully a pre R9 solution will present
itself.

I also thought tar would present more of a problem as tar isn't a
windows command. There is an
[erl\_tar](http://www.erlang.org/doc/doc-5.5.3/lib/stdlib-1.14.3/doc/html/erl_tar.html)
module in stdlib but it claims to support only Sun's version of
tar. However, on examination of the docs for both erl\_tar and gnutar
this seems not to be the case. The format followed by erl\_tar is IEEE
Std 1003.1. This is an extended version of tar called ustar and its a
POSIX standard. It seems that modern versions of gnutar support IEEE
Std 1003.1 as well, as you can see
[here](http://www.gnu.org/software/tar/manual/html_node/tar_134.html)
(check the references at the bottom of the page). So the erlang docs
seem either in accurate or out of date, fortunately for me.

Symlinks are a bit of a harder problem. The need for symlinks is
greatly reduced now that I don't have to build up tar-able
structures. However, I still use them to build up a deployable version
of the OTP apps with sources in the \_build directory. I think that
the only solution to this problem is to copy the sources and related
files into the binary structures instead of symlinking them in. It
will add a bit of overhead but I think thats worth it.

I do have one final problem, this one just occurred to me. I have
written a nice little shell script to kick off a build. This shell
script uses a bit of magic (borrowed from firefox) to follow any
symlinks back to its deployed area where it gathers the paths for
sinan. It then kicks off erl with all the proper paths to the sinan
code. Not being a windows person I have no idea how to handle this on
windows boxes. Hopefully, some erlang oriented windows guy will step
forward and help me out with this.
