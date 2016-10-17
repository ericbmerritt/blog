---
bg: 'apollo.png'
layout: post
title: How To Manage Erlang/OTP Releases
tags: [erlang,release]
---

It took me quite a few years to arrive at an optimal mental model for
thinking about the Erlang, ERTS, and releases in the context of an
operating system. It is a different enough method of thought that it's
worth talking about.

I think most folks have not yet come to a fundamental realization when
it comes to Erlang, or more specifically, ERTS. That realization is
that ERTS is a Virtual Machine in the classic sense. That is, it views
each release as a complete self-contained 'machine'. That concept is
central to the way it expects releases to be organized and managed.

Once you get your mind around this idea that each release is a
self-contained machine, things begin to make more sense. For example,
why are there only version numbers and not names in release
directories and tarballs created by sys_tools? This is because an
Erlang Release is a self-contained universe, a complete machine
Virtual Machine, sort of like an install of Linux. So asking why you
can't have multiple releases in the same node is very similar to
asking why Linux only has one rc.d directory. It only has one because
in the context of a machine only one 'operating system/bootstrapping
system' makes sense.

So how do you integrate this very different module into the Unix-y way
of doing things? The simple answer is that you don't.

Approaches to Managing a Release Install
----------------------------------------

You can take two approaches when it comes to installing a release on a
Unix box. You can go through the efforts of splitting out the release
into its constituent applications, installing those applications
separately so that nothing is shared. Then you can come up with a
scheme for your release metadata so that they do not collide. After
that, you can shoehorn all this into whatever package management
system exists on your preferred platform. The question I have with
this approach is simply *why*? To retain some idea of the purity of
the Unix model? So you can save a few tens or hundreds of megabytes of
disk space? With this approach you will constantly be fighting ERTS,
coming up with ways to get around the Virtual Machine and what it is
natively expecting. In the end, You will be coming up with all sorts
of ways to create unanticipated pain for yourself with no real gain.

The alternative is that you treat an Erlang/OTP release as the VM
expects. You could use the tools distributed with Erlang to create and
handle releases. You could treat a release tarball as a single
distributable thing. You could even take this to the logical extreme
and, if you are targeting homogeneous hardware even include the ERTS
binary in that tarball so that everything is completely
self-contained. The whole reason ERTS is structured in this manner is
due to the problem it was intended to solve: providing a way to make a
self-contained system that could be installed trivially on target
machines.

So taking advantage of this model, You end up with a system (in either
/opt or /var) that looks something like this.

    /opt/erts//-*

That is, you have a separate directory structure to handle Erlang
Releases, somewhere on your system and each release is in its own
specific location.

Let's look at an example. Let's say that I have a release `foo`. On your
target systems you expect to have versions `0.1.0`, `0.2.0` and
`0.2.1`. You also have the release `bar` with versions `0.1.1` and
`0.1.2`. Then your tree might look like:

    /opt/erts
        |-- bar
        |   |-- 0.1.1
        |   |   |-- ....
        |   `-- 0.1.2
        |   |   |-- ....
        `-- foo
            |-- 0.1.0
            |   |-- ....
            |-- 0.2.0
            |   |-- ....
            `-- 0.2.1
                |-- ....

Where the ellipses identify all the files and directories relating to
the release.

Your chef/puppet/management scripts end up being very trivial, simply
untaring releases into that version scheme and starting up the self
contained releases in those directories, then running the relevant
startup commands for ERTS.

This becomes an even more important factor when you start looking at
hot code loading and live upgrades with Relups. While I don't
recommend that this live upgrading facility for anything but the most
trivial projects or those projects where extremely high up-time is
worth the monumental costs of getting it right, it still is there and
relies on this well-understood layout to function.

#### What You Gain

1. Each release is self-contained and has no dependencies aside from
   OS dependencies
2. You can trivially roll forward and backward with no worries about
   version conflicts or mismatches.
3. It's what Erlang/OTP expects; it's what the build systems based on
   Erlang/OTP expect, you are saving yourself trouble by following the
   garden path.

#### What You Loose

1. Disk space. You have possibly redundant information in each release dir
2. You cant use the native package management system
3. Peace of mind. It's not the Unix-y approach

Conclusion
----------

Unless you have a very good reason not to, I suggest you embrace the
Erlang approach. Its simple, straight forward, easy to understand and
even easier to manage. It has few downsides and major wins for your
infrastructure management and deployment.