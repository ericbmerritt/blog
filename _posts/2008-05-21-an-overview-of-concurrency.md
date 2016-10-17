---
bg: 'apollo.png'
layout: post
title: An Overview of Concurrency
date: 2008-05-21
comments: false
tags: [concurrency]
---

### Introduction

Over the last few weeks, I have had several conversations with people
about concurrency, more precisely the ways in which shared information
is handled in concurrent languages. I have gotten the impression that
there isn't a good understanding of what's out there in the world of
concurrency. That being the case it would be a good idea just to give
a quick overview of some of the mechanisms that are gaining mind share
in the world of concurrency.

Aside from some engineers that are currently so deep in their rut that
they can't see sunlight, it's been accepted that the current
mainstream approach to concurrency just won't work. The idea of using
mutexes and locks to try to take advantage of the up and coming
massively multi-core chips is just laughable. We can't ignore this
topic. As software engineers, we don't have a choice about supporting
large numbers of CPUs; that's the direction that hardware is going
it's up to us to figure out how to make it work in
software. Fortunately, a bunch of really smart folks has been thinking
about this problem for a long time. A few of the things they have been
working on are slowly making their way into the gestalt of the
software world.

We are going to talk about three things. They are Software
Transactional Memory (STM), Dataflow Programing specifically Futures
and Promises and Message Passing Concurrency. There are others, but
these currently have the most mind share and the best
implementations. I have limited space and limited time so I am going
to stick to these three topics. You may have noticed that up to this
point I have only talked about concurrency. More specifically the
communication between processes in concurrent languages. That's
intentional; I am not going to talk about parallelism at all. That's a
different subject and only faintly related to concurrency, but often
conflated with it. So if your looking for that you are looking in the
wrong place. On another note, I am going to use processes and threads
pretty much interchangeably. I know that in certain languages the two
terms have very different meanings, however, in many other languages
they mean the same or one of the terms doesn't apply. What I am
getting at is that if you open the scope of the discussion to the
languages at large the meanings become ambiguous. When I use the term
process, or thread I am talking about a single concurrent activity
that may or may not communicate with other concurrent activities in
same way. That's about the best I can do for you.

### Traditional Shared Memory Communication

Shared Memory Communication is the GOTO of our time. Like GOTO of
years past its the current mainstream concurrent communication
technique and has been for a long, long time. Just like GOTO, there
are so many gotchas and ways to shoot yourself in the head that its
scary. It was so bad that this approach to concurrency had tainted an
entire generation of engineers with a deeply abiding fear of
concurrency in general. This great fear has crippled the ability of
our industry to adapt to the new reality of multi-core systems. The
sooner shared memory dies the horrible death it deserves then, the
better for us all. Having said that, I must now admit that, just like
GOTOs, shared memory has a small niche where it probably can't be
replaced. If you work in that niche then you already know you need
shared memory and if you don't you don't. Just a hint, implementing
business logic in services is not that niche. OK, I am all done with
my rant, and I feel much better, now on to the show.

Shared memory typically refers to a large block of random access
memory that can be accessed by several different concurrently running
processes. This block of memory is protected by some guard that makes
sure that the block of memory isn't being accessed by more than one
process at any particular time. These guards usually take the form of
Locks, Mutexes, Semaphores, etc. There are a bunch of problems with
this shared memory approach. There is complexity in managing the
serial access to the block of memory. There is complexity managing
lock contention for a heavily used resources. There is a very real
possibility of creating deadlocks in your code in a way that isn't
easily visible to you as a developer. There is just all kinds of
nastiness here. This type of concurrency is found in all the current
mainstream programming and scripting languages, C, C++, Java, Perl,
Python, etc. For whatever reason, its ubiquitous and we have all been
exposed to it that doesn't mean we have to accept it as the status
quo.

### Software Transactional Memory (STM)

The first non-traditional concurrency mechanism we are going to look
at is Software Transactional Memory or STM for short. The most popular
embodiment of STM is currently available in the GHC implementation of
Haskell. As for the description I will let
[wikipedia](http://en.wikipedia.org/wiki/Software_transactional_memory)
handle it.

Software transactional memory (STM) is a concurrency control mechanism
analogous to database transactions for controlling access to shared
memory in concurrent computing. It functions as an alternative to
lock-based synchronization and is typically implemented in a lock-free
way. A transaction in this context is a piece of code that executes a
series of reads and writes to shared memory. These reads and writes
logically occur at a single instant in time; intermediate states are
not visible to other (successful) transactions.

STM has a few benefits that aren't immediately apparent. First and
foremost STM is optimistic. Every thread does what it needs to do
without knowing or caring if another thread is working with the same
data. At the end of a manipulation if everything is good and nothing
has changed then the changes are committed. If problems or conflicts
occur, the change can be rolled back and retried. The cool part of
this is that there isn't any waiting for resources. Threads can write
to different parts of a structure without sharing any lock. The sad
part about this is that you have to retry failed commits. There is
also some, not insignificant, overhead involved with the transaction
subsystem itself that causes a performance hit. Additionally, in
certain situations, there may be a memory hit, i.e., if n processes
are modifying the same amount of memory you would need O(n) of memory
to support the transaction. This is a million times better then the
mainstream shared memory approach, and if it's the only alternative
available to you, then you should use it. I still consider it shared
memory at its core. That's an argument that I have had many, many
times.

### Dataflow - Futures and Promises

Another approach to concurrency is the use of Futures and
Promises. Its most popular implementation is in Mozart-Oz. Once again
I will let
[wikipedia](http://en.wikipedia.org/wiki/Futures_and_promises) the
description for me.

In computer science, futures and promises are closely related
constructs used for synchronization in some concurrent programming
languages. They both refer to an object that acts as a proxy for a
the result that is initially not known, usually because the computation of
its value has not yet completed.

Let's lay down the difference between Futures and Promises before we
get started. A Future is a contract that a specific thread will, at
some point in the future, provide a value to fill that contract. A
promise is, well a Promise that at some point some thread will provide
the promised value. This is very much a dataflow style of programming
and is mostly found in those languages that support that style, like
Mozart-Oz and Alice ML.

Futures and Promises are conceptually pretty simple. They make passing
around data in concurrent systems pretty intuitive. They also serve as
a good foundation on which to build up more complex structures like
channels. Those languages that support Futures and Promises usually
support advanced ideas like unification and in that context Futures
and Promises work well. However, although Futures and Promises remove
the most egregious possibilities for dead-locking it is still possible
in some cases.

In the end, both of these approaches involve shared memory. They both
do a reasonably good job at mitigating the insidious problems of using
shared memory, but they just mitigate those problems, they don't
eliminate them. The next mechanism takes a completely different
approach to the problem. For that reason, it does manage to eliminate
most of the problems involved with shared memory concurrency. Of
course, there is always a trade-off, and in this case, the trade-off
is in additional memory usage and copying costs. I am getting ahead of
myself let me begin at the beginning and then proceed to the end in a
linear fashion.

### Message Passing Concurrency

The third form of concurrency is built around message passing. Once
again I will let
[wikipedia](http://en.wikipedia.org/wiki/Message_Passing) describe the
system as it tends to be better at it than I am.

Concurrent components communicate by exchanging messages (exemplified
by Erlang and Occam). The exchange of messages may be carried out
asynchronously (sometimes referred to as "send and pray", although it
is standard practice to resend messages that are not acknowledged as
received), or may use a rendezvous style in which the sender blocks
until the message is received. Message-passing concurrency tends to be
far easier to reason about than shared-memory concurrency, and is
typically considered a more robust, although slower, form of
concurrent programming. A wide variety of mathematical theories for
understanding and analyzing message-passing systems are available,
including the Actor model, and various process calculi.Message-passing
concurrency is about processes communicating by sending messages to
one another. Semantically these messages are completely separate
entities unrelated to whatever data they were built from. This means
that when you are writing code that uses this form of concurrency you
don't need to worry about shared state at all, you just need to worry
about how the messages will flow through your system. Of course, you
don't get this for free. In many cases, message passing concurrency is
built by doing a deep copy of the message before sending and then
sending the copy instead of the actual message. The problem here is
that that copy can be quite expensive for sufficiently large
structures. This additional memory usage may have negative
implications for you system if you are in any way memory constrained
or are sending a lot of large messages. In practice, this means that
you must be aware of and manage the size and complexity of the
messages that you are sending and receiving. Much like Futures and
Promises the most egregious 'shoot yourself in the head' possibilities
of deadlocking are removed its still possible to do. You must be aware
of that in your design and implementation.

### Conclusion

In the end any one of these approaches is so much better then the
shared memory approach that it almost doesn't matter which one you
choose for your next project. However, they each have very different
philosophical approaches to concurrency that greatly affect how you go
about designing systems. You should explore each one so that you are
able to make a logical decision about which one to use for your next
project. That said, opinions are like umm, I can't really complete
that, but you get my drift. My opinion on the subject is the message
passing concurrency is by far the best of the three, where best is
defined by most conceptually simple and scalable. In the end the
industry will decide which direction is right for that and head in
that direction. We are still to early in the multi-core age to get any
good impression of which will win out.