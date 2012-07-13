---
layout: post
title: Differences Between Joxa and LFE
---

{{ page.title }}
================

In the last few days I have gotten the question 'What are the
differences between LFE and Joxa?' quiet a few times. So instead of
answering them individually I thought I would write up the differences
here.

Goals
-----

The primary and most important difference is in the Goals of the two
languages. I believe that the primary goal Robert had in mind when
implementing LFE was to provide a mutable and syntax extensible
version of Erlang. This would allow people to change the language
where they needed to. Also I suspect, very strongly, that Robert likes
implementing languages and he had a lot of fun implementing LFE. I
certainly did with Joxa. However, I had some other very specific goals
when I sat down to create Joxa.

1. I needed a platform for the development of Domain Specific
   Languages.
2. I wanted a more iterative and dynamic development
   environment. Something on the order of Slime and Swank.
3. I wanted to leverage all of the rather awesome lisp tools that are
   out there.

Each of these things could have been solved in Erlang. For example, I
could have implemented each language using leex and yecc. However, my
best experience with DSLs has always come from Lisp and building those
DSLs via functions and macros in Lisp itself. However, I have been
using Erlang for a long time and I was very unwilling give up the
features of the Erlang VM to get those advantages from Lisp. The only
solution seemed to be using a Lisp on the Erlang VM.

The obvious first choice was LFE. So I spent several weeks digging
into the language and its internals. At the end I decided it did not
suit my purposes and the only fallback was to create a language of my
own (there was a bit of sanity questioning involved as well).

With that in mind lets enumerate some of the major differences.

Lisp 1 vs Lisp 2
----------------

Simply put, LFE is a Lisp 2 while Joxa is a Lisp 1. According to
Richard P. Gabriel, the lisp 1 vs Lisp 2 is defined as follows:

&gt; Lisp-1 has a single namespace that serves a dual role as the
&gt; function namespace and value namespace; that is, its function
&gt; namespace and value namespace are not distinct. In Lisp-1, the
&gt; functional position of a form and the argument positions of forms
&gt; are evaluated according to the same rules.
&gt;
&gt; Lisp-2 has distinct function and value namespaces. In Lisp-2, the
&gt; rules for evaluation in the functional position of a form are
&gt; distinct from those for evaluation in the argument positions of the
&gt; form. Common Lisp is a Lisp-2 dialect.

To give a practical example of the above description lets say that you
have a function called `hello-world` that returns an atom
`hello-world`. To define and call that function in Joxa you would do:

    (defn hello-world ()
        :hello-world)

    (hello-world)

To do the same thing in LFE you would do the following:

    (defun hello-world ()
       'hello-world)

    (: hello-world)

*note:* In LFE the `:` serves the same purpose as `funcall` in Common Lisp.

The Lisp world has been arguing about which is better since the dawn
of the universe. It very much depends on personal preference. For me,
I find Lisp 2 to be very unnatural and counter intuitive nearly to the
point where I wont code in it.

Erlang Systax vs Lisp Syntax
----------------------------

LFE is, as its name implies, is a Lisp version of the Erlang Language
and whose intention is to provide a Lisp very close to Erlang and it's
Semantics.

Joxa has no such intention. It is a unique language that happens to be
targeted at the Erlang VM. It makes no effort to provide Erlang,
Common Lisp semantics. The entire goal is to provide a tight, small
well understood functional language with a clean approachable syntax
that allows for the use of Lisp style macros.

As you will notice in using Joxa, there is very little in the way of
Erlang syntax and even less of Common Lisp. Some syntax for
declarative data structures has been pulled over but very little
more. Most of the syntax comes from Clojure and Scheme.

Evaluated Macros vs In System Macros
------------------------------------

In LFE macros are evaluated in LFE itself and not with the Erlang
VM. This means that macro evaluation has different semantics then
function evaluation. In LFE macros are not simply functions that run
at compile time like they are lisp. The are special things that have
their own evaluation semantics different from those of normal
functions. So not only must you keep in mind the normal compile time
vs runtime semantics you must also keep in mind the function vs macro
semantics. I believe this is a hindrance to the easy use of macros.

For that reason Joxa takes an incremental approach to module
compilation that allows macros to be evaluated on the VM in the exact
same way as functions and there is no need at all to worry about
differences in the evaluation environment between normal functions and
macros. Since one of the important goals of Joxa is explicitly
supporting DSLs this unified evaluation environment for functions and
macros is quite important.

Language Bootstrapping
----------------------

I am the co-founder of a startup called Afiniate and we are basing
important aspects of Afiniate's business on Joxa. It is very important
to us that Joxa be both well tested and stable. To that end the
language has been bootstrapped on itself. That is, Joxa is actually
implemented in Joxa. This allowed us to test the system and work out
many problems very early on. This also serves as an important litmus
test for new features and changes. This litmus test caches many types
of problems before they ever leave the developers desktop. I believe
that this bootstrapping is a fundamental requirement for any language
that will be used in production systems.

Joxa is also extremely well tested. As I said we are basing an
important part of our business on this platform and we must ensure, as
much as possible, that we can iterate on the platform quickly while
retaining its stability.
