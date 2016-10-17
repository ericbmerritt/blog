---
bg: 'apollo.png'
layout: post
title: Dependencies Again
date: 2007-03-03
comments: false
tags: [erlang,sinan,progress,build]
---

Just when I think I am done with a project something pops up and bites
me. As I was getting ready to do the release to my beta customers, I
noticed that some of my unit tests in the dependency code no longer
pass. I had made some changes the week before to alter conflict
reporting, and I guess I for got to run the tests. I jumped in and
started debugging. An hour or so latter whop-a-mole with bugs I
realized that the core algorithm was wrong. Leaky edge cases are
almost always a symptom of an underlying fault in the logic. In
nearly, all of these cases about the only thing, you can do is throw
out the existing implementation and start fresh.

I wrestled with a creating a new solution with little or no
luck. Eventually, I went to lunch with some friends to talk out the
problem. We, or more specifically Scott Parish, realized that this is
a backtracing problem very similar to that Prolog was designed to
handle. Fortunately, he had just spent the last month living and
breathing a similar backtracing problem and volunteered to code up a
solution. He took the core of his solution from chapters 11 and 12 of
[PAIP](http://norvig.com/paip.html). It seems that Erlang, due to its
immutability, makes for an excellent platform for these types of
issues. In any case, Later on, he sent me the solution that turned out
to be a special purpose prolog interpreter. Even then it was a
complete, fast solution that occupied only a hundred lines of code. I
am working on getting it integrated back into the build system as I
write this. It is amazing how simple and concise an elegant solution
can be.
