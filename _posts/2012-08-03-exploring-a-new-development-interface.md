---
layout: post
title: Exploring A New Development Interface
---

{{ page.title }}
================

Very recently and after much experimentation I fundamentally changed
the tools I use to do development. This is the first time I have made
a change like this since I started coding in the mid 90s. Where before
I used a pretty standard keyboard (IBM Model 2), a mouse (Kensington
Expert Mouse), and a laptop (Usually running Linux, sometimes Mac OS
X); Now I use a Chording keyboard
([HandyKey Twiddler2](http://handykey.com) and an Android based tablet
(Zareason Zatab). So far this non-traditional setup is working rather
well and has made me much more mobile. Here is how I happened upon it.


I Begin My a Journey
--------------------

 A year or so ago I was working on a personal project. This project
was a simple native application that was designed to run on multiple
different OSes. Since I am a true believer when it comes to testing I
bought a large (at the time) 6 core AMD box with 16 gigs of memory,
up-gradable to 64, as a testing box. The goal was to have a box on
which I could run multiple VMs for building and testing. It worked
well for that, but shortly after I bought the box I found out that
someone had beat me to the punch on the project. They had built a good
implementation of the very thing I was planning on building! That's
life, though, and its nice when someone else does the work for you
anyway.

 Concurrent to this, I started to use IRC a lot more and I wanted a
persistent IRC client. So I did the usual thing and set up a server in
the cloud running irssi and related bits under tmux. This led to me
start to use that wee little box (the cheap one you can actually
afford to leave running 24/7) for things beyond irc, like small
builds, testing etc. After a few months of this I had a eureka
moment. "I have this huge box sitting at home doing nothing and it is
way bigger then anything I could ever afford to run in the cloud. Why
am I not using that?" I said to myself with well deserved slap to the
forehead. So I proceeded to set up a tomato router and Dynamic DNS
through Namecheap, loaded up arch on the server and started using that
instead of the cloud based server I had been using.

Well now I had this powerful box available that I was always connected
to via ssh and so I started using it. I ran builds, did development,
everything you do on any other development box. I use emacs for nearly
everything so going to a terminal based emacs was no real change at
all. After a bit of this I realized that my laptop had become
redundant. I was only using it as a terminal and nothing more. I
wondered what it would take to ditch the laptop and go to a
ultra-portable setup something that was truly just a thin client. I
thought that what would probably be most interesting would be a decent
sized tablet probably Android but a full linux if I could find it.

Solving The Problems
--------------------

As when any time you are trying something new and out of the ordinary
there where lots of problems that would have to be solved to make this
work. At the very least I figured the following problems would need to
be solved.

### Connectivity

This approach requires that I always be connected. In my world, thats
already a pretty conistant requirment. Fortunatly, in the modern world
connectivity is ubiquitous so this is really no problem. Even in the
cases where there is no wifi connectivity, if you have a decent cell
company (Ting!) tethering solves the last bit of connectivity
requirement we might have.

### SSH

While connectivity is ubiquitous it is not always of high
quality. That is, bandwidth may be lacking or congestion high. In
these situations SSH is unusable. It needs a stable strong connection
to be anything like usable. I actually thought this was a deal breaker
until I discovered Mosh. Mosh is SSH for the modern of persistent but
flaky\unstable\low bandwidth connections. With mosh using a low
bandwidth connection as a pipe to your primary working box works
really well. It solved this problem in that nothing else would have.

### Output

Output is pretty straightforward. Android tablets have a decent if
small screen and they support an external monitor. So, as long as I
have a tablet with a decent sized screen I should be ok. When I am on
the go I can just use the tablet and when I am in the office I can
hook up to a monitor for a bit more real estate.

### Input

Input is much harder then output. The simple solution is to use a
hardware keyboard, but these have the negative feature of being heavy
(comparatively) and bulky. I am already a bit of a geek for input
devices and have been exploring them for a long time, alternative
layouts, alternative formats, Chording keyboards, etc. I pulled the
trigger on Dvorak long ago, this just gave me a good reason to pull
the trigger on one of these other methods of input. For ease of
trucking around as well as longevity I choose the
[Twiddler2 by HandyKey](http://handykey.com).

This has solved the input problem quite well but it is buy no means a
trivial solution. Learning a completely new keyboard, remapping long
standing muscle memory and rebuilding speed is a slow frustrating
process. The win at the end is huge but the journey is hard.

Conclusion
----------

Now that all the problems have been solved and the transition made I
have a powerful, ultra-portable setup that has the upside of being
very, very inexpensive for the power available. I don't, yet, have
enough time with this system to know if in will work out in the long
term, but I will keep you appraised of my progress.
