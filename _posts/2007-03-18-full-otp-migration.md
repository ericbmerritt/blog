---
layout: post
title: Full OTP Migration
date: 2007-03-18
comments: false
---

{{ page.title }}
================

I just finished migrating the sinan to a full OTP application. It
already followed all of the OTP principles it just wasn't setup to run
as an application. I ran into a few issues that encouraged me to do
the conversion. It wasn't actually difficult, as I said , I already
followed OTP for the most part. It was mostly a matter of turning the
tasks into gen\_servers and figuring out a way for them to work
together in a meaningful way. For now there isn't much difference for
the user, but it sets things up so that I can rapidly iterate on the
current outstanding issues.

The hardest part of all of this was getting the error\_logger logging
set up right. A lot more loggers then I suspected are involved from
the get go. Kernel sets up a very primitive error\_logger to start. It
also sets up a slightly better tty logger. Then sasl sets up its own
set of loggers. Figuring out where this was coming from, getting rid
of the loggers and adding the custom logger was more difficult then I
actually expected. In the end I got it done via a combination of
configs for kernel and sasl and actually removing the primitive logger
via the gen\_event api.

In any case, I should be able to start knocking my open issues. Thanks
to the beta testers for providing the feedback.
