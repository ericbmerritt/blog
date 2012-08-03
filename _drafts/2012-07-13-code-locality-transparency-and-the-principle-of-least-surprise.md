---
layout: post
title: Code Locality and Transparency or The Principle of Least Surprise
---

{{ page.title }}
================


I happened across a piece of code recently that caused me to spend a a
lot of time thinking about code locality and transparency. I think
about these things a fairly large amount any way, but this
crystallized in my mind, what I think can be and often is a major and
growing problem in many Erlang code-bases.

But first, let me define what I mean by those two things before I move
forward. They are two very closely related concepts with locality
being a subset of transparency, but important enough to get its own
billing.


*Locality*
: The ability to understand where code that is being called lives, at
the very least in what modules the functions being called lives.


*Transparency*
: The ability to understand what is going on in the code by a simple
review. Basically that is accomplished by the coder, making his
intentions clear to the maintainer/reader (which is often also
you). You can do this by trying to follow a few simple things:


- The 'thing' in the code should look like what it is. If a thing
  looks like a function call, it should be a function call, If a thing
  looks like a atom, it should be an atom.
- If code looks like it lives in the current module, it should be live
  in that module. If it lives in a different module, it should be
  obvious that it lives in a different module.


This is following the principle of least surprise.


The best way to judge Transparency is how quickly you can figure out
what is going on in a piece of code. Of course, this is pretty
subjective but still a good measure.


In general, there are three major things that are used in Erlang that
cause problems with both Locality and Transparency. The first is the
`import` statement, the second is Erlang`s C Style, find and replace,
macros, and the third are parse transforms. All of these cause big
problems for locality, but macros cause big problems for both locality
and transparency.

In general the following rules should be followed.

- You shouldn't be using imports very much at all, and if you do use
  them you should never, ever, put those imports in a header file.

- Using macros where they are absolutely needed, ie in places where
  functions will not fill the need. *if you are writing a macro that
  looks like a function and dose what a function can do think twice*.

- Use parse transforms sparingly and only in places where other
  options are not possible (where you need to add syntax). If you do
  use them, make activating them explicit and obvious. Do not hide a
  `-compile({parse_transform, <transform name>})` in a header file
  somewhere. Its ok, to wrap it in a macro (see above) but make the
  user call that macro explicitly.


MACROS
------

This is a bit of code from a test module. At the top of the
module,three different header files imported. Can you, by reading the
code, tell whats going on here?


    Fixture = ?fixture,
    ArbitrarySize = 1000000,
    ArbitraryPercent = 1.00,
    ?assertMatch(ok, ?ensure_good()),
    erlcron:set_datetime(Fixture(day1_morning), testing),
    Ask = ?ask(?target, ArbitrarySize, ArbitraryPercent, Fixture(day1)),
    ?assertMatch([open], ?state(Ask)),
    ?cancel(Ask),
    ?assertMatch(closed, ?state(Ask)),
    Fixture(dispose).


First off this just smells bad, There are no less then nine calls to
macros in ten lines of code. What do these macros do? what is
fixture?, what does it do? where does it live? ensure_good?  target?
You don't have a clue by looking at this. You have to dig through the
code, looking for which header file the macro lives in, then follow
that macro to find where the code actually lives to figure out what's
actually going on. That obfuscates the meaning of the code and costs
time that could be more valuably spent.


Let’s take one example that I consider a bit egregious. As with all
macros it has the problems that that you have no clue where it lives and
no clue as to its locality at all. After a bit of digging, you will
find that issuer lives in the egregious.hrl file. What does that macro
look like? It looks as follows.


  %% ?target
  %%
  %% Create a target account with a key tied to the line number in which the macro appears.


  -define(target,
         ((fun() ->
                  AccountKey = list_to_binary(format("~s-~w", [?MODULE, ?LINE])),
                  erg_test_data:create_test_target(AccountKey),
                  AccountKey
          end)())).


So not only is this not a static macro as it appears, its a function
that calls several other bits of code located in other places. So in
this case, the use of the macro not only hides the locality of the
target, it does a very, very good job of hiding the complexity of the
code itself. Thereby obscuring the meaning of the code (what is
actually going on). I consider this a major detraction to both
maintainability and the ability to refactor.


What could we do to make this more obvious? Well there are a few
things we could. We could turn issuer into a function call with a
namethat is a bit more intention, or gives a better idea of what is
actually going on. That would be a very first step.




  %% ?create_test_target Create a target account with a key tied to the
  %%line number in which the macro appears.


  -define(create_test_target,
         ((fun() ->
                  AccountKey = list_to_binary(format("-~s-~w", [?MODULE, ?LINE])),
                  erg_test_data:create_test_target(AccountKey),
                  AccountKey
          end)())).


Now at least it sounds like its doing something. Lets reuse the ask
call, that is a call that taxes a 'Fixture' variable from context (yet
a another non-obvious occurrence in the code base) and calls that
fixture with ask and the arguments provided.


    Ask = ?ask(?create_test_target, ArbitrarySize, ArbitraryPercent, Fixture(day1)),


That's better, but the macro still looks static. You don't realize
that anything complex is executing there. We can fix that by getting
rid of the auto-calling.


  %% ?create_test_target Create a target account with a key tied to the
  %%line number in which the macro appears.


  -define(create_test_target,
         (fun() ->
                  AccountKey = list_to_binary(format("~s-~w", [?MODULE, ?LINE])),
                  erg_test_data:create_test_target(AccountKey),
                  AccountKey
          end)).


Now this function has to be called as follows.


    Ask = ?ask(?create_test_target(), ArbitrarySize, ArbitraryPercent, Fixture(day1)),


Ah that's much better, now it looks like its actually calling
executable code. Its obvious that something is happening. That is an
important step. We still don't know where its happening. Thats mostly
do to the macro-ish nature of the call and how Erlang has implemented
macros. We could get rid of that by changing it from a macro to a
function in a module. We have one problem here though and that is the
?MODULE and ?LINE calls. They need to execute in the context of the
module they are calling from. So if we moved this to its own function
we would loose that. Perhaps not. I would argue that locality is
important enough that we could actually create a function that took as
arguments these functions. I actually think that is a good way to go,
though at this point it becomes a matter of personal style.


Lets convert the macro to a function that takes a few arguments. Then
move it into a module, probably the erg_test_data module.


  %% ?create_test_target Create a target account with a key tied to the
  %%line number in which the macro appears.


  create_test_target(Module, Line) when is_atom(Module), is_integer(Line) ->
       AccountKey = list_to_binary(format("I-~s-~w", [Module, Line])),
       create_test_target(AccountKey),
       AccountKey.




This new function, create_test_target/2, lives in the same module now
as the original create_test_target/1, with slightly different
arities. So now our calls are


    Ask = ?ask(erg_test_data:create_test_target(?MODLE, ?LINE)),
               ArbitrarySize, ArbitraryPercent, Fixture(day1)),


As you can see, it is slightly more verbose then before, but it is
absolutely obvious what is going on at a glance. You also know that
you can go to the erg_test_data module look at the create_test_target
function and understand what is going on there, because, hopefully,
that is following the same guidelines as this.


This is important for understand what is going on in the system.


IMPORTS
-------

Fortunatly, imports aren’t used very much in the Erlang world. Where
they are used its almost always to reduce the amount of code an coder
has to write, in an effort, to save a few characters of
typing. Unfortunately, that vastly reduces understand of where the
code that is being called actually lives. Here is a piece of real
code, that we can use as an example, heavily redacted to remove
specifics.


    dashboard_item({Node, State}, Metrics) ->
        [" some stuff ",
         atom_to_list(State),
         " more stuff ",
         " even more stuff ",
         join([metrics_item(Item) || Item <- Metrics], " some type of seperator"),
         " some finishing up stuff "].


What's is the ‘join’ function doing? Where does it live? We have no
clue. At first glance it looks like it lives in the same module as the
dashboard_item function. But we look in that module and don't see
it. Then we perhaps start look in the various header files. Eventfully
we will see this in a header file included from that module.


    -import(string, [join/2,
                    strip/1,
                    tokens/2]).


Which imports the functions into the current module, completely
obscuring where they live and what's actually going on. We could
change the original function to something more readable vary easily.


   dashboard_item({Node, State}, Metrics) ->
        [" some stuff ",
         erlang:atom_to_list(State),
         " more stuff ",
         " even more stuff ",
         strings:join([metrics_item(Item) || Item <- Metrics], " some type of seperator"),
         " some finishing up stuff "].


Now suddenly, with that simple addition, we know that join is in the
strings module and that join is probably operating on strings
themselves. That bit of contextual information greatly aids the
readability and understandability of the code base. As an aside,
notice that I also prefixed the 'atom_to_list' call with its module
(erlang). That is also a good idea, and the direction that erlang is
going in in general. Understand code locality at a glance whether that
code is built in or part of your application is vastly important.


CONCLUSION
----------


We should optimize for reading and understanding, not for writing. We
only write once but this code is going to be read and maintained for
years to come. Optimizing the process of maintenance and understanding
is vastly more important then saving a few characters when we are
writing code. To that end there are a few things to keep in mind.


- Do not use imports
- Do not use macros


If you must use macros, try to make the meanings of things obvious. If
a function is being called, make the macro user call it. If it does
something at the very least make it an 'action' name.


This is all about intentional coding, making your intentions clear to
the person (maybe you) that is coming after you in the code base. The
clearer you can make your intentions (within reason) the better its
going to be for the code-base as a whole.
