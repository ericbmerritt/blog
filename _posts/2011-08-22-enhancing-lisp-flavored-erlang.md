---
layout: post
title: Enhancing Lisp Flavored Erlang
tags: [erlang,lfe]
---

I have a serious personal investment in Erlang as a language and a
platform. I have written a book on the subject and spent a fair amount
of my professional and open source life working in it. I have done all
of that because I believe its a system that just works for much
of the type of development that I do. However, I also have a secret
long term love affair with Lisp. Unfortunately, I have never really
been able to use it in anything but toy projects. Recently I happened
upon [LFE](https://github.com/rvirding/lfe) again. I remember seeing
it when it was announced and thinking it wasn't quite baked, but that
seems to have changed quite a bit. It may very well solve both my
Erlang as a powerful platform and Lisp as on awesome language itches
for me.

I decided that my very first project was going to be a Swank
([Slime](http://common-lisp.net/project/slime/) backend) server
implementation for LFE. Think about it; you could code in Slime using
Lisp on the Erlang platform. That's pretty exciting to
me. Unfortunately, I immediately ran into a problem. That is that
Macros are accessible after compilation. That is they go away when the
system is compiled and are no longer accessible. Well, that doesn't
work in a system like slime when there isn't a lot of distinction
between compilation time and run time (nor is it very acceptable in
Lisp in general as the authors acknowledge).

Macros are not First Class
--------------------------

Macros are not first class citizens of the LFE world. That is they
exist only at run time, and they are not evaluated by the Erlang
virtual machine; the are evaluated by the LFE build system
itself. This means that macro evaluation has different semantics then
the evaluation of code compiled by the virtual machine in the usual
manner. This forces the developer to make a distinction in his mind
between things that will be evaluated at compile time vs. things that
will evaluate at run time. This is probably best illustrated by the
'eval-when-compile' construct.

A problem related to this is the fact is that because macros only
exist at compile time, if you want to use macros in another file you
must import those files with the Macros, those macros are then
evaluated by the expander in the namespace provided (along with its
other functions). The fact that macros are always evaluated in the
namespace of the macro caller provides its set of problems.

All of these problems can be solved by compiling macros and making
them first class entities in LFE.

Making Macros First Class
-------------------------

In the end, Macros are just functions. They are functions that
are evaluated at compile time, who's input are lists that represent
the AST of the program, but they are just functions just the same. So
it makes sense that we could just compile them as functions and mark
them somehow so that the system knows that they are macros.

So that is exactly what I am going to do. First and foremost I am going
to convert a specified macro to a function. Recursively expanding the
macro (of course) until a specific form is completely expanded. Then
convert that form into a 'define-function' expression. I have to mark
this function as a macro, so the easiest way to do that is to keep a
list of macros around for that particular namespace. When a macro
function is created, we add it to the list of names returned by that
special function. So when the code generation occurs, the macro gets
compiled into a function. We mark the function as being a macro by
generating a special function in LFE compiled modules. I think we can
call that function '__macros__' (it's simple enough after all), and
when called that function would return a list of all macro functions in
the system.

As we generate each 'function' be it an actual function or a macro, the
function we will go through a complete generation process. That is we
will go from a sexpr form describing a 'function' to a compiled erlang
function in the correct namespace. In this manner, the full module will
be available at compile time for every function compiled before the
current function being compiled and expanded. That should give us
the flexibility to just about anything we want, at the cost of
additional compilation time. A nice fringe benefit is that we should
be able to git rid of the eval-when-compile special form.


Implications of Compiling Macros
--------------------------------

There are two big implications that compiling macros in this way
entail. The first is that, if you have macros that depend on
functions then those functions must be defined before the macro that
calls them. It will become a very simple top down evaluation process,
or at least, it will appear that way from the point of view of the
developer.

The second is that macros can not be defined before the module
definition, since in both Erlang and LFE the module definition be the
first thing in the file. In current lfe code, it seems fairly common
for macros to come at the head of the file. Because macros will now be
generated and compiled into functions in that modules namespace the
must be defined after the def module call. Macros can be called before
define module (as long as they evaluate in such a way that the define
module call is the first expanded form in the file) but macros ahead
of modules can only be called from other modules.

Future Directions
-----------------

This is the first of several planned changes to LFE. My next project
will be to complete the swank backend for the language. Once that's
done, I am going to try to switch over to LFE as my primary
Erlang language. I suspect this alone will generate some changes as
well. One thing I would love to see is some more Clojure
goodness in the language. Not a complete Clojure implementation, but
Clojure does have a ton of great syntax ideas that we can
borrow. However, those things are for the future. First and foremost,
I need to finish macro compilation.