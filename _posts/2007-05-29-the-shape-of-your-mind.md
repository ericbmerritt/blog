---
bg: 'apollo.png'
layout: post
title: The Shape Of Your Mind
date: 2007-05-29
comments: false
tags: [philosophy,languages]
---

In several cultures throughout history, like the [Karen Paduang of
Southeast
Asia](http://www.chiangmai-chiangrai.com/longneck_karen.html), the
ancient [Han of China](http://www.bbc.co.uk/dna/h2g2/A1155872) and the
[ancient native tribes of the Paracas region of
Peru](http://www.travelblog.org/South-America/Peru/Paracas/blog-97346.html),
various body modification practices were quite common. In fact, among
the members of these cultures unusual body shapes were (and in some
cases still are) considered breathtaking. Parents went to great
lengths to achieve these shapes using mechanical devices to mold an
infant's growth into a particular shape. For example, among the Karen
Paduang this procedure resulted in women with very, very long
necks. In the case of the Han, it was women with very tiny, actually
unusable feet. For the natives of the Paracas region of Peru, it was
large cone-shaped skulls. Although grotesque by Western standards, the
individual subjected to these procedures was considered significantly
more beautiful than a person shaped along more natural lines. However,
if you removed these people from their culture and time to cast them
into many modern societies, they would be considered very, very
strange at the least and grotesque at worst.


Fascinating, you say, but what does this have to do with computer
science and more specifically languages? In many cases, the
communities that form around languages are very similar to the
isolated communities that created these practices. To take this
analogy one step further, in many ways programming languages act quite
a lot like the devices used to shape the skulls of infants in Paracas,
the feet of Han women or the necks of Karen Paduang women. In the case
of these languages, instead of shaping the skull they tend to shape
the way we think about problems, the way we form ideas and the way
those ideas are applied to a particular problem. For example, if you
have only every written code in early versions of Fortran (Fortran 77
and before) then you probably don't even know recursion exists. Also,
if you have only ever coded in Haskell, you probably would know
nothing about imperative style loops.

If I might take a bit my history as an example, very early in my
forays into coding, I came across a problem requiring a search through
a directory structure. I quickly came up with an imperative-loop-based
solution that did the job. Unfortunately, the code was ugly and (if I
may borrow a term from the refactoring community) it didn't quite
smell right. I didn't know what the right solution was, but I knew I
didn't have it. At that time, the public internet was a new fangled
thing, but I had already found it to be useful for gathering
information. So I searched for solutions that others had found to
similar problems. Eventually, I came across a piece of code that used
recursion to walk the tree instead of imperative looping. It took me a
little while to get my mind around this new concept, having never been
exposed to non-imperative code. However, once I did, I realized that
the recursive solution was a much cleaner and more natural solution to
this problem. Recursion isn't always a more natural solution than
imperative iteration, but in this case, it was. That made me think
about this type of problem in an entirely different light. It added
another tool to my toolbox. If I had been exposed to functional
languages before this point, it would have saved me lots of time and
trouble.

This is a minor example, but it does help prove a point: what a
language permits and does not permit affects how you think about
problems. This is an incredibly important realization because it means
that, with certain caveats, the more programming languages you know,
the more insight you may have into the solution to a particular coding
problem. So, if you accept the fact that languages tend to mold your
way of thinking about problems then you can easily see how languages
can be compared to these body modification devices we spoke of
earlier. If Programing Languages can be compared to these devices then
we, the users of programming languages, can be compared to the
subjects that undergo modification.

The comparison is not exact. As engineers, we don't start out with a
nice round head. We have to work to achieve it using the same tools
that provided the first distortion. To elaborate, we all start out
learning a single language and that language affects the shapes our
'mind' so to speak. This first language pushes our mind out in one
direction, perhaps upward. So after we learn a single language, most
of us are walking around with a big cone shaped mind.


Unfortunately, many of us never go on to learn any other languages. We
keep our big cone shaped mind for the entirety of our career. That may
not be a bad thing. If you are solidly embedded in a particular
'language culture' then cone-shaped minds are probably considered
quite beautiful. In fact, you may be regarded as some type of elder
because of the cone-iness of your mind. We tend to call these people
Gurus and they are deserving of some respect.

These cone-shaped minds are probably not considered all that beautiful
outside of their particular 'language culture'. C gurus aren't going
to be very useful in Scheme community and Scheme gurus aren't going to
be very helpful to the C community. That's bad because both languages
have ideas and features that are useful to understand. Those Cone
minds that keep to a single language throughout their entire career
never realize their full potential. That's unfortunate because a
programmer with a well-shaped mind is more efficient and better able
to find the most elegant solution to a problem. In fact, he ceases to
be a programmer and becomes an Engineer. With diligence and hard
study, he may even become a Good Engineer.

Now, about this time you are probably thinking to yourself, 'I don't
Like the idea of walking around with a big cone-shaped mind.'  If
that's the case, great! Fortunately, unlike the natives of Paracas,
you can do something about they way your algorithmic 'mind' is
shaped. How do you go about reshaping your mind? Well, it's not a
simple process, you need to force your mind into a new shape using the
devices that warped your mind in the first place. You must learn more
and distinctly different languages. Each language forces your mind to
grow in a different direction. Learn enough languages and your mind
will have a nice round shape.

So how many languages do you have to learn and what languages are the
best? There is no fixed number. I usually suggest five as a minimum
number and recommend ten or fifteen. That may sound like a lot but
after the first couple languages picking up new ones starts becoming
much easier. In that regard, it's a little like picking up a new
spoken language. For example, if you know Spanish then Portuguese
isn't all that hard. If you know Spanish and Portuguese, then Italian
is pretty simple. If you know Spanish, Portuguese and Italian, then
picking up French is a snap. This goes on and on. In the case of
programming languages, there are a small number of additional rules
you need to apply to get the most out of this process, but the overall
process is the same. The other rules are listed below.

1. Each language must come from a different family of languages. See
   [this history of languages](http://www.levenez.com/lang/)
   information.
2. All three major paradigms (Procedural, Object Oriented, and
   Functional) must be covered.
3. At least two minor paradigms (Concurrent and Logic/Declarative)
   must be covered.
4. Both static typing and dynamic typing need to be represented.

If you have never heard of functional programming and don't have a
clue what procedural means don't worry. I will help you out a bit by
providing a list of languages broken down by family and paradigm at
the end of this little missive. As you learn new languages, you will
soon have a good idea of the different families of languages and the
paradigms they represent. Before you know it, you will know quite a
few different languages, and you'll be able to think about problems
from many different angles and your mind will have a nice round
shape. You will be able to think clearly about a programming problem
in any number of ways instead of the small number of ways your
previous cone-shaped mind allowed.

Dave Thomas of Pragmatic Programmers (not Wendy's) fame came up with
the idea of learning a language each year. They started a group to
accomplish this back in 2002. Unfortunately, it seems to have started
and stopped all in that same year.

### The Languages

A language breakdown by family is available on the ['History of
Programming Languages' information](http://www.levenez.com/lang/).

As for the breakdown by type, I am not going to try to do this for
every language available. So I am just going to give you a list of ten
of fifteen programming languages broken down according to the rules I
provided previously. This should provide you with a large enough group
to pick five that interest you.

Descriptions are arranged as follows ([paradigms], Typing, Family). If
the family doesn't exist, assume the language is a family in its own
right.

* [Erlang](http://www.erlang.org/) ([Functional, Concurrent, Distributed], Dynamic Typing)
* [Forth](http://www.forth.org/) or [Postscript](http://www.cs.indiana.edu/docproject/programming/postscript/postscript.html) (Dynamic Typing)
* [Mercury](http://www.cs.mu.oz.au/research/mercury/) ([Logic, Declarative], Dynamic Typing)
* [Prolog](http://pauillac.inria.fr/~diaz/gnu-prolog/) ([Logic, Declarative], Dynamic Typing)
* [Mozart-Oz](http://www.mozart-oz.org/) ([Functional, Procedural, Object Oriented, Logic, Distributed, Concurrent], Dynamic Typing)
* [Lisp](http://www.lisp.org/) ([Functional, Procedural, Object Oriented, Logic], Dynamic Typing, Lisp)
* [Scheme](http://www.schemers.org/) ([Functional, Object Oriented, Logic], Dynamic Typing, Lisp)
* [Ada](http://www.engin.umd.umich.edu/CIS/course.des/cis400/ada/ada.html) ([Procedural, Object Oriented, Concurrent], Static Typing, Pascal) (Another resource)
* [Python](http://www.python.org/) ([Procedural, Object Oriented, Functional], Dynamic Typing)
* [Haskell](http://www.haskell.org/) ([Functional, Lazy], Static Typing)
* [Lua](http://www.lua.org/) ([Procedural, Object Oriented], Dynamic Typing)
* [Ruby](http://www.ruby-lang.org/) ([Object Oriented], Dynamic Typing, Smalltalk)
* [Smalltalk](http://www.squeak.org/) ([Object Oriented], Dynamic Typing, Smalltalk)
* [SML](http://www.smlnj.org/) ([Functional], Static Typing, SML)
* [Ocaml](http://www.ocaml.org/) ([Functional], Static Typing, SML)
* [Clean](http://www.cs.ru.nl/~clean/) ([Functional], Static Typing)
* [D](http://www.digitalmars.com/d/) ([Procedural, Object Oriented], Static Typing, Algol)

I didn't include the languages that are common (C, C++, Perl, Java)
because there is a good chance you already know them. They also don't
count for this exercise (C, C++, and Java are all part of the Algol
family and would only count once). Feel free to choose other languages
that you may be aware of and find interesting. This list is only a
'Getting Started' list.

I strongly suggest that you learn a Lisp dialect and a Forth. These
two languages are superb at shaping your mind and the two languages
specifically tend to force the shape of your mind in opposite
directions. It's a somewhat painful process but well worth the quick
results. At the very least make sure that one of these languages is
included on your list.

### Footnotes

Thanks to a comment by Vince I have found that I am not the only one
thinking along these lines. In the linguistics community, there seems
to be a hypothesis call the [Sapir-Whorf
hypothesis](http://en.wikipedia.org/wiki/The_Sapir-Whorf_Hypothesis)
that describes something similar. Kenneth Iverson gave his Turing
Award lecture on the same topic. It was called "Notation as a tool of
thought." Unfortunately, I can't seem to find a good link for it right
now.