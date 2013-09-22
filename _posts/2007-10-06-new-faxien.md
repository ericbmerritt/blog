---
layout: post
title: New faxien
date: 2007-10-06
comments: false
---

{{ page.title }}
================

So its been a little bit since I posted. We have been working hard and
heavy on several changes to the erlware repo formats. One of the big
time sinks in this project has been writing a bootstrapper/otp release
launcher for faxien. Well let me break out the two before I go into
details.

### The Repository

A few months ago we made some changes to the repository to reduce the
complexity and reduce the total number of release versions we had to
keep around. Originally we organized the OTP apps in our repository
around the erts version number, major, minor and patch. This worked
but it meant we had a lot of copies of the same apps laying around. We
had hoped that we would be able to organize it around the erts major,
minor version instead. This would have saved a huge amount of
space. so we made some changes in support of that. Well after we made
those changes we found out that the patch version number wasn't as
unimportant as we thought. In fact, among other things it seems that
the OTP guys feel free to modify the wire protocol in patch
versions. They also change the magic version numbers in the c lib so
that they wont communicate with with an erts with a patch version
different then what they where compiled for. What this means for us is
that we had to go back to supporting major, minor and patch
versions. We also changed the way release packages are stored in the
repo. Not much, but enough to require some code changes. All this took
more time then we would have liked.

### The Faxien Bootstrapper

The other problem we had is around matching erts versions with
releases that faxien pulls down. In the past we used whatever version
or erts/erlang was available on the local box. This caused problems if
faxien was built for a version of erts that was greater then the one
present on the box. Of course, this would be a problem for any and
every bit of erlang code that faxien pulls down as well. So we had to
come up with a way to pull down an erts to run on as well as the code
to run. This meant that we had to come up with some way to bootstrap
the system. After much thought and quite a few experiments we decided
to write a minimal bootstrapper in ocaml. What this means is that
folks can download a small binary that will pull down the required
erts version, faxien and all its dependencies. The bootstrapper will
then launch faxien to complete the install process.

With this approach the user doesn't need to have erlang on their
system at all. They just pull down 'faxien' and it pulls down
everything thats required. Thats pretty cool. It also only pulls whats
needed so instead of going out and getting 20 megs of erlang
distribution they will get what they need and just what they need.

We put in a few other nifty features to make command line OTP releases
easier and to make cross platform OTP launch scripts easer to
write. However, I will talk about those in a different post.
