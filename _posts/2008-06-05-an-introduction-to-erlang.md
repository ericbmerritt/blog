---
bg: 'apollo.png'
layout: post
title: An Introduction to Erlang
tags: [erlang]
---

As some of you may have guessed, I am a fan of Erlang. I think that
it's a very interesting language with a tremendous amount of promise
for the type of server side applications that I usually end up working
on. I have talked a lot about various things here on Erlangish, so I
thought it would finally be appropriate to spend a bit of time talking
about the topic of the blog. For the most part, I will be delving into
the, somewhat obscure, history of Erlang. I will also spend a bit of
time providing some instructions on how to get started with the
language.

So What Is Erlang and OTP?
--------------------------

Erlang is a distributed, concurrent, soft real-time functional
programming language and runtime environment developed by Ericsson,
the Telecoms Infrastructure supplier. It has built-in support for
concurrency, distribution and fault tolerance. Since its open source
release in 1999, Erlang has been adopted by many leading telecom and
IT related companies. It is now successfully being used in other
sectors including banking, finance, e-commerce and computer
telephony.

OTP is a large collection of libraries for Erlang to do everything
from compiling ASN.1 to providing an application embeddable HTTP
server. It also consists of implementations of several patterns that
have proven useful in massively concurrent development over the
years. Most production Erlang applications are Erlang/OTP
applications. OTP is also open source and distributed with
Erlang.

Although Erlang is a general purpose language, it has tried to fill a
certain niche. This niche mostly consists of distributed, reliable,
soft real-time concurrent systems. These types of applications are
telecommunication systems, Servers and Database applications which
require soft real-time behavior. Erlang excels at these types of
systems because these are the types of systems that it was originally
designed around. It contains some features that make it particularly
useful in this arena. For example; it provides a simple and powerful
model for error containment and fault tolerance; concurrency and
message passing are a fundamental to the language, applications
written in Erlang are often composed of hundreds or thousands of
lightweight processes; Context switching between Erlang processes is
typically several orders of magnitude cheaper than switching between
threads in other languages; it's distribution mechanisms are
transparent, programs need not be aware that they are distributed, and
The runtime system allows code in a running system to be updated
without interrupting the program.

Given that there are things that Erlang is good at there are bound to
be a few things that it's not so good at. The most common class of
'less suitable' problems is characterized by iterative performance
being a prime requirement with constant-factors having a large effect
on performance. Typical examples are image processing, signal
processing and sorting large volumes of data.

Origins of Erlang
-----------------

I am firmly convinced that Erlang's history is a key ingredient to its
success. I am not aware of any other language whose early development
was so straightforward and pragmatic. For most of its life, Erlang was
developed inside Ericsson, originally for internal use only. Later on,
it was made available to the external world and eventually open
sourced. The timeline of its development goes something like this.

* 1985_Ericsson identified some issues that existed with telecom
  languages at the time. To address these difficulties they started
  experiments with the programming of telecom applications using more
  then twenty different languages. These early experimenters came up
  with a few features that a useful system needed to supply. They
  realized that the target language must be a very high-level symbolic
  language to achieve productivity gains. This new requirement vastly
  subseted the language space and resulted in a very short list of
  languages. The languages included Lisp, Prolog, Parlog, etc.

* 1986 Further experiments within this subseted list where
  performed. New results were generated from this round of experiments
  as well. They found that the theoretically ideal language also
  needed to contain primitives for concurrency and error recovery, and
  the execution model needed to exclude back-tracking. This ruled out
  two more of the contending languages, Lisp and Prolog. This ideal
  language also needed to have a granularity of concurrency such that
  there would be a one to one relationship between one asynchronous
  telephony process and one process in the language. This ruled out
  Parlog. At the end of this round of experiments they where left with
  out any languages in the list they had started with.  Being the
  pragmatic folks that they were, they decided to develop their
  language with the desirable features of Lisp, Prolog, and Parlog,
  but with superior concurrency and error recovery built into the
  language.

* 1987 The first experiments began with this nascent language which
  became Erlang.

* 1988 The ACS/Dunder project was started at Ericsson. This was a
  prototype implementation of PABX by developers external to the core
  Erlang developers.

* 1989 The ACS/Dunder project became a full-fledged project with the
  reconstruction of about a tenth of the complete, production PABX
  called the MD-110 system. The preliminary results where very
  promising. In this early phase developer productivity was already
  ten times greater than during the development of the original system
  in the PLEX language. This reimplementation also pushed forward
  experiments directed at increasing the speed of Erlang.

* 1991 At this point the experiments directed at speeding up Erlang
  bore fruit. A fast implementation of Erlang was released to growing
  user community. Erlang was also presented at an international
  telecom conference that year.

* 1992 During this year the user base started growing
  significantly. Erlang was ported to VxWorks, PC, Macintosh and other
  platforms. Three new, complete applications were written in Erlang
  where presented at another conference. The first two production
  projects using Erlang are started.

* 1993 Distribution primitives were added to the language, which made
  it possible to run a homogeneous Erlang system on heterogeneous
  hardware. A corporate decision was made within Ericsson to begin
  selling and supporting Erlang externally. A new organizational
  structure was built up to maintain and support Erlang
  implementations and Erlang Tools. This resulted in the creation of
  Erlang Systems AB.

* 1996 OTP was formed into a separate product group within Erlang
  Systems AB. This represented the maturing of the OTP platform within
  Erlang. After nearly ten years of development the (non-Erlang) AXE/N
  project was closed down and pronounced a failure. This left a large
  whole in Eriksson's product line, and development of a new
  replacement product was started to fill it, it was decided that this
  replacement product would be written in Erlang.

* 1998 After two years the AXE/N replacement project, now called the
  AXD301 was delivered.  Around this same time, the CEO of Ericsson
  Radio became the CEO of Ericsson as a whole. This person had banned
  Erlang at Ericsson Radio, and though he never banned Erlang at
  Ericsson proper, it became career suicide to propose Erlang new
  Erlang projects. This problem effectively killed opportunities for
  Erlang Systems AB to sell the language and support. The primary
  question potential customers asked was 'Who wants to use a language
  developed by Ericsson when Ericsson won't use the language
  itself?'. This just goes to show that corporate bureaucracy will be
  corporate bureaucracy autocracy no matter where you are. In any
  case, this turned into a bit of a blessing in disguise for the rest
  of us.

* 1998-99 to drive further development a decision was made within
  Erlang Systems AB to release Erlang as an Open Source project. This
  didn't imply an abandonment of Erlang by Ericsson or Erlang Systems
  AB. Erlang continued and continues to be supported by these two
  organizations. This decision was made wholly with the idea of
  spreading Erlang and removing the, somewhat negative, idea that it
  was a proprietary language. It has remained open source and
  supported to this day.

As you can see the Erlang didn't start out as Erlang at all. It
started out as just a series of requirements backed up by
experiments. A large number of experiments were done to find the
language that matched those requirements. When no existing language
was found Ericsson decided to create their own.  Considering
Ericsson's resources and the costs associated with development of
their products

I think this was a very pragmatic decision. However, that conclusion
is open to interpretation. In any case, after the initial development,
there was a constant back, and forth dialog between the users and
developers of the language as the language moved through its formative
process. I think this fact alone is one of the reasons that Erlang is
as good as it is today. Later on in its development as Ericsson grew
less resourceful Erlang started to have political problems within the
company. Even though Ericsson had several successful and profitable
products in Erlang and other languages, the de-facto ban
occurred. Fortunately by this time Erlang could and did stand on its
own.  The ban turned out to be fortunate for the rest of us because it
led, pretty directly, to Erlang's eventual Open Sourcing.

Joe Armstrong, one of the original Erlang Developers and a productive
member of the community put together a number of tutorials that are
very useful. Its worth going through these and playing with the code.

* [Robust Server](http://www.sics.se/%7Ejoe/tutorials/robust_server/robust_server.html)
* [Web Server](http://www.sics.se/%7Ejoe/tutorials/web_server/web_server.html)
* [Client Server](http://www.sics.se/%7Ejoe/tutorials/client_server/client_server.html)
* [Wiki](http://www.sics.se/%7Ejoe/tutorials/wiki/wiki.html)

There are a couple of good editors to use with Erlang. The gold
standard is the Erlang Emacs mode distributed as part of the Erlang
distro. A very updated version is now available from the Erlware
folks.  You can get it
[here](http://www.erlware.org/tools/erlware-mode/index.html).  If you
go this route, I suggest you also get
[Distel](http://code.google.com/p/distel/) written by Luke
Gorrie. It's available from the the good folks at
[google code](http://code.google.com/p/distel/).  There are
instructions included with both of these to get you up and going. For
those of you more inclined to the IDE world you may want to take a
look at [Erlide](http://erlide.sourceforge.net/). This is a
set of Eclipse plugins that add support for Erlang to Eclipse. It's
still pretty beta, but it's very usable.

So How Do I Get Started?
------------------------

Learning Erlang is a relatively quick process. For an experienced
developer, it shouldn't take more then a few days before they can
write nontrivial programs, about a week or two to feel comfortable and
a month or so before feeling ready to take on something big by
themselves. It helps a lot to have someone who knows how to use Erlang
around for some hand-holding.

Start off by going through the quick start part of the
[FAQ](http://www.erlang.org/faq/quick_start.html) Then go through the
[Erlang Course](http://www.erlang.org/course/course.html). You can
skip the history part if you would like. I have gone over it in more
detail here. Once you have done the course play around with some of
the
[examples](href="http://erlang.se/doc/doc-5.4.8/doc/programming_examples/part_frame.html).
Then go read the long version of the [getting started
docs](http://www.erlang.org/doc/getting_started/part_frame.html). This
should put you on the road to being able to write some Erlang code. If
you are one to worry about coding conventions, then you may want to
take a look at the [programming
rules](http://www.erlang.se/doc/programming_rules.shtml"&gt;http://www.erlang.se/doc/programming_rules.shtml).
This has quite some useful and well thought out programming rules. One
of the things that make Erlang really interesting is the OTP
System. If you want to get to know something about Erlang, then it
makes sense to spend a bit of time learning OTP, and it's [design
principles](http://www.erlang.org/doc/design_principles/part_frame.html)
is an excellent place to start.