---
layout: post
title: Erlang and the Web
date: 2007-04-23
comments: false
---

{{ page.title }}
================

I have been spending a lot of time thinking about leveraging Erlang's
concurrency features and OTP in web applications. Right now I don't
believe that any of the available frameworks do that very
well. [Erlyweb](http://erlyweb.org/) tries to be 'rails' for Erlang
without really leveraging the features that make Erlang
great. [Yaws ](http://yaws.hyber.org/) is more a web server then a web
app server. It tries to make some amends here by providing things like
[appmods](http://yaws.hyber.org/appmods.yaws) and
[yapps](http://yaws.hyber.org/yapp_intro.yaws) but they feel bolted on
and they don't really leverage OTP at all.

This has been an open issue in my mind for some time. I created the
[tercio](http://code.google.com/p/tercio/) project as a starting point
to solve this problem back in November or December of last year. It
languished for awhile, partly because I had yet to figure out a
elegant solution. Well, I think that I finally have. Its the logical
conclusion of current web development trends. I am surprised that no
one has thought of it yet. The idea is two fold.

1) Let client side handle the client side
2) Let the server side handle the server side

The general idea is that you will let the client side handle all
client side rendering. The only server side participation in this is
serving up files. In turn, the server will handle all server side
(business) logic. The two should really have very little knowledge of
one another. Hmm, seems a bit too simplistic doesn't it? I thought so,
before I realized that the client side in this continuum already has a
perfectly good language on which to base things. That languages is
javascript. I can hear the groans of dismay already. I emitted those
very same groans back in the late '90s through the mid '00s and I
wouldn't have even considered this two years ago. However, the
landscape has changed alot in two short years. Ajax has gained
prominence, libraries like prototype have been created. Its just a
whole different world. We are already in good shape for the server
side with OTP.

So what are the mechanics of making this happen. First we need to get
away from generating html on the server side. To do that we need to
make it easy to generate it on the client side. Manually creating dom
objects really isn't the right way to go. I think we can do this with
a library called
[JavascriptTemplates](http://trimpath.com/project/wiki/JavaScriptTemplates). This
provides a reasonable templating language on top of javascript. Since
each snippet of template will be small this should provide a
reasonable efficient way to go. The second problem is how do we remove
the client side knowledge from the server? I think tercio already does
a good job here. It provides a javascripterlang bridge. With this
javascript can send and receive messages to processes that have
registered interest in client side message no the backend. Its
webserver agnostic, so by providing a small shim you can make it work
with any webserver you want. I am building a small startup on top of
this framework. I am quite sure there will be a lot of issues to work
out. However, I think the fundamental principles are solid and should
provide for the right web development experience in Erlang.

Tercio isn't yet ready for prime time or even late night yet. It
should be soon though so keep your eyes posted here.
