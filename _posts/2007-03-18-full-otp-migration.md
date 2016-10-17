---
bg: 'apollo.png'
layout: post
title: Full OTP Migration
date: 2007-03-18
comments: false
tags: [erlang,sinan,progress,build]
---

I just finished migrating the Sinan to a full OTP application. It
already followed all of the OTP principles it just wasn't setup to run
as an application. I ran into a few issues that encouraged me to do
the conversion. It wasn't actually difficult, as I said , I already
followed OTP for the most part. It was mostly a matter of turning the
tasks into gen\_servers and figuring out a way for them to work
together in a meaningful way. For now there isn't much difference for
the user, but it sets things up so that I can rapidly iterate on the
current outstanding issues.

The hardest part of all of this was getting the `error_logger` logging
set up right. A lot more loggers then I suspected are involved from
the get-go. Kernel sets up a very simple `error_logger` to start. It
also sets up a slightly better `tty` logger. Then `sasl` sets up its
set of Loggers. Figuring out where this was coming from, getting rid
of the Loggers and adding the custom logger was more difficult than I
expected. In the end, I got it done via a combination of configs for
kernel and `sasl` and removing the first logger via the `gen_event`
API.

In any case, I should be able to start knocking my open issues. Thanks
to the beta testers for providing the feedback.